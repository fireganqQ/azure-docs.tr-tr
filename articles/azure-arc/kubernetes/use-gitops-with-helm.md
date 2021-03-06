---
title: Yay etkin Kubernetes kümesinde Gilar kullanarak Held grafikleri dağıtma (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Azure Arc etkin küme yapılandırması (Önizleme) için Held ile Gilar kullanma
keywords: Gilar, Kubernetes, K8s, Azure, Held, Arc, AKS, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: 2dfb516487d1064f29b4018cc8b322e8db44e53a
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100558519"
---
# <a name="deploy-helm-charts-using-gitops-on-arc-enabled-kubernetes-cluster-preview"></a>Yay etkin Kubernetes kümesinde Gilar kullanarak Held grafikleri dağıtma (Önizleme)

Helm, Kubernetes uygulamalarını yüklemenize ve yaşam döngüsünü yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır. APT ve yum gibi Linux paket yöneticilerine benzer şekilde, Helm, önceden yapılandırılmış Kubernetes kaynakları paketleri olan Kubernetes grafiklerini yönetmek için kullanılır.

Bu makalede, Azure Arc etkinleştirilmiş Kubernetes ile Held 'yi yapılandırma ve kullanma işlemlerinin nasıl yapılacağı gösterilir.

## <a name="before-you-begin"></a>Başlamadan önce

Mevcut bir Azure Arc etkin Kubernetes bağlı kümesine sahip olduğunuzu doğrulayın. Bağlı bir kümeye ihtiyacınız varsa bkz. [Azure yay etkin Kubernetes kümesi hızlı başlangıç](./connect-cluster.md).

## <a name="overview-of-using-gitops-and-helm-with-azure-arc-enabled-kubernetes"></a>Azure Arc etkin Kubernetes ile Gira ve Held kullanımına genel bakış

 Hele işleci, Held grafik sürümlerini otomatikleştiren bir Flox uzantısı sağlar. Helk grafik sürümü, HelmRelease adlı bir Kubernetes özel kaynağı aracılığıyla açıklanır. Flox, bu kaynakları git 'ten kümeye eşitler, ancak Held operatörü kaynaklarda belirtilen şekilde Heln grafiklerinin serbest bırakılacağını sağlar.

 Bu makalede kullanılan [örnek depo](https://github.com/Azure/arc-helm-demo) aşağıdaki şekilde yapılandırılmıştır:

```console
├── charts
│   └── azure-arc-sample
│       ├── Chart.yaml
│       ├── templates
│       │   ├── NOTES.txt
│       │   ├── deployment.yaml
│       │   └── service.yaml
│       └── values.yaml
└── releases
    └── app.yaml
```

Git deposunda iki dizin vardır: biri bir helk grafiği ve biri de yayınlar yapılandırmasını içerir. Dizininde, `releases` `app.yaml` aşağıda gösterildiği HelmRelease yapılandırmasını içerir:

```yaml
apiVersion: helm.fluxcd.io/v1
kind: HelmRelease
metadata:
  name: azure-arc-sample
  namespace: arc-k8s-demo
spec:
  releaseName: arc-k8s-demo
  chart:
    git: https://github.com/Azure/arc-helm-demo
    ref: master
    path: charts/azure-arc-sample
  values:
    serviceName: arc-k8s-demo
```

HELI yayın yapılandırması aşağıdaki alanları içerir:

| Alan | Açıklama |
| ------------- | ------------- | 
| `metadata.name` | Zorunlu alan. Kubernetes adlandırma kurallarını izlemesi gerekir. |
| `metadata.namespace` | İsteğe bağlı alan. Sürümün oluşturulduğu yeri belirler. |
| `spec.releaseName` | İsteğe bağlı alan. Sağlanmazsa, yayın adı olacaktır `$namespace-$name` . |
| `spec.chart.path` | Depo köküne göre verilen, grafiği içeren dizin. |
| `spec.values` | Varsayılan parametre değerlerinin grafiğin kendisindeki Kullanıcı özelleştirmeleri. |

HelmRelease içinde belirtilen seçenekler, `spec.values` grafik kaynağından belirtilen seçenekleri geçersiz kılar `values.yaml` .

HelmRelease hakkında daha fazla bilgi için resmi [hele işleci belgelerine bakın](https://docs.fluxcd.io/projects/helm-operator/en/stable/).

## <a name="create-a-configuration"></a>Yapılandırma oluşturma

İçin Azure CLı uzantısını kullanarak `k8sconfiguration` , bağlı kümenizi örnek git deposuna bağlayın. Bu yapılandırmaya ad verin `azure-arc-sample` ve Flox işlecini `arc-k8s-demo` ad alanına dağıtın.

```console
az k8sconfiguration create --name azure-arc-sample --cluster-name AzureArcTest1 --resource-group AzureArcTest --operator-instance-name flux --operator-namespace arc-k8s-demo --operator-params='--git-readonly --git-path=releases' --enable-helm-operator --helm-operator-version='1.2.0' --helm-operator-params='--set helm.versions=v3' --repository-url https://github.com/Azure/arc-helm-demo.git --scope namespace --cluster-type connectedClusters
```

### <a name="configuration-parameters"></a>Yapılandırma parametreleri

Yapılandırmanın oluşturulmasını özelleştirmek için [kullanabileceğiniz ek parametreler hakkında bilgi edinin](./use-gitops-connected-cluster.md#additional-parameters).

## <a name="validate-the-configuration"></a>Yapılandırmayı doğrulama

Azure CLı 'yı kullanarak `sourceControlConfiguration` başarıyla oluşturulduğunu doğrulayın.

```console
az k8sconfiguration show --name azure-arc-sample --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

`sourceControlConfiguration`Kaynak, uyumluluk durumu, iletiler ve hata ayıklama bilgileriyle güncelleştirilir.

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Installed",
    "lastConfigApplied": "2019-12-05T05:34:41.481000",
    "message": "{\"OperatorMessage\":null,\"ClusterState\":null}",
    "messageLevel": "3"
  },
  "enableHelmOperator": "True",
  "helmOperatorProperties": {
    "chartValues": "--set helm.versions=v3",
    "chartVersion": "1.2.0"
  },
  "id": "/subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/azure-arc-sample",
  "name": "azure-arc-sample",
  "operatorInstanceName": "flux",
  "operatorNamespace": "arc-k8s-demo",
  "operatorParams": "--git-readonly --git-path=releases",
  "operatorScope": "namespace",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "",
  "repositoryUrl": "https://github.com/Azure/arc-helm-demo.git",
  "resourceGroup": "AzureArcTest",
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

## <a name="validate-application"></a>Uygulamayı doğrula

Aşağıdaki komutu çalıştırın ve `localhost:8080` uygulamanın çalıştığını doğrulamak için tarayıcınızda öğesine gidin.

```console
kubectl port-forward -n arc-k8s-demo svc/arc-k8s-demo 8080:8080
```

## <a name="next-steps"></a>Sonraki adımlar

- [Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)
