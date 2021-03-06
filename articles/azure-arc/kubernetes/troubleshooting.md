---
title: Yaygın Azure Arc etkin Kubernetes sorunlarını giderme (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Arc etkin Kubernetes kümeleriyle ilgili yaygın sorunları giderme.
keywords: Kubernetes, yay, Azure, kapsayıcılar
ms.openlocfilehash: 0827386eb6ec089cf7951e8fa513a77fc78aef22
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98684098"
---
# <a name="azure-arc-enabled-kubernetes-troubleshooting-preview"></a>Azure Arc etkin Kubernetes sorunlarını giderme (Önizleme)

Bu belgede bağlantı, izinler ve aracılar ile ilgili bazı genel sorun giderme senaryoları sağlanmaktadır.

## <a name="general-troubleshooting"></a>Genel sorun giderme

### <a name="azure-cli-set-up"></a>Azure CLı kurulumu
Az connectedk8s veya az k8sconfiguration CLı komutlarını kullanmadan önce, az önce doğru Azure aboneliğine göre çalışacak şekilde ayarlandığından emin olmanız gerekir.

```azurecli
az account set --subscription 'subscriptionId'
az account show
```

### <a name="azure-arc-agents"></a>Azure-yay aracıları
Azure Arc etkin Kubernetes için tüm aracılar ad alanında Pod olarak dağıtılır `azure-arc` . Normal işlemler altında tüm yığınların çalışıyor olması ve sistem durumu denetimlerinin geçirilmesi gerekir.

İlk olarak, Azure Arc helb sürümünü doğrulayın:

```console
$ helm --namespace default status azure-arc
NAME: azure-arc
LAST DEPLOYED: Fri Apr  3 11:13:10 2020
NAMESPACE: default
STATUS: deployed
REVISION: 5
TEST SUITE: None
```

Held sürümü bulunamazsa veya eksikse kümeyi yeniden eklemeyi deneyin.

Helb sürümü varsa ve `STATUS: deployed` aracı durumunu şunu kullanarak tespit ederseniz `kubectl` :

```console
$ kubectl -n azure-arc get deployments,pods
NAME                                       READY  UP-TO-DATE  AVAILABLE  AGE
deployment.apps/clusteridentityoperator     1/1       1          1       16h
deployment.apps/config-agent                1/1       1          1       16h
deployment.apps/cluster-metadata-operator   1/1       1          1       16h
deployment.apps/controller-manager          1/1       1          1       16h
deployment.apps/flux-logs-agent             1/1       1          1       16h
deployment.apps/metrics-agent               1/1       1          1       16h
deployment.apps/resource-sync-agent         1/1       1          1       16h

NAME                                            READY   STATUS  RESTART  AGE
pod/cluster-metadata-operator-7fb54d9986-g785b  2/2     Running  0       16h
pod/clusteridentityoperator-6d6678ffd4-tx8hr    3/3     Running  0       16h
pod/config-agent-544c4669f9-4th92               3/3     Running  0       16h
pod/controller-manager-fddf5c766-ftd96          3/3     Running  0       16h
pod/flux-logs-agent-7c489f57f4-mwqqv            2/2     Running  0       16h
pod/metrics-agent-58b765c8db-n5l7k              2/2     Running  0       16h
pod/resource-sync-agent-5cf85976c7-522p5        3/3     Running  0       16h
```

Tüm pods 'Ler `STATUS` olarak göstermelidir `Running` ve ya `READY` da olmalıdır `3/3` `2/2` . Günlükleri getirin ve döndürülen veya döndüren Pod 'leri `Error` tanıtın `CrashLoopBackOff` . Bu yığınların herhangi biri durumunda kalırsa, `Pending` küme düğümlerinde kaynakların yetersiz olması olabilir. [Kümenizin ölçeğini](https://kubernetes.io/docs/tasks/administer-cluster/) ölçeklendirmek, bu yığınların duruma geçmesi için bu FID 'leri alır `Running` .

## <a name="connecting-kubernetes-clusters-to-azure-arc"></a>Kubernetes kümelerini Azure yaya bağlama

Kümelerin Azure 'a bağlanması için hem Azure aboneliğine hem de `cluster-admin` hedef kümeye erişime sahip olmanız gerekir. Kümeye ulaşılamadığından veya izinleri yetersizse, ekleme başarısız olur.

### <a name="insufficient-cluster-permissions"></a>Küme izinleri yetersiz

Belirtilen kubeconfig dosyası, Azure Arc aracılarını yüklemek için yeterli izinlere sahip değilse, Azure CLı komutu Kubernetes API 'sini çağırmaya çalışırken bir hata döndürür.

```azurecli
$ az connectedk8s connect --resource-group AzureArc --name AzureArcCluster
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Ensure that you have the latest helm version installed before proceeding to avoid unexpected errors.
This operation might take a while...

Error: list: failed to list: secrets is forbidden: User "myuser" cannot list resource "secrets" in API group "" at the cluster scope
```

Küme sahibi, Küme Yöneticisi izinlerine sahip bir Kubernetes kullanıcısı kullanmalıdır.

### <a name="installation-timeouts"></a>Yükleme zaman aşımları

Azure Arc aracı yüklemesi için hedef kümede bir kapsayıcı kümesi çalışıyor olmalıdır. Küme yavaş bir internet bağlantısı üzerinden çalışıyorsa kapsayıcı görüntüsü çekme işlemi Azure CLı zaman aşımlarından daha uzun sürebilir.

```azurecli
$ az connectedk8s connect --resource-group AzureArc --name AzureArcCluster
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Ensure that you have the latest helm version installed before proceeding to avoid unexpected errors.
This operation might take a while...
```

### <a name="helm-issue"></a>Helb sorunu

`v3.3.0-rc.1`Helmversion, Held yüklemesi/yükseltmesi (CONNECTEDK8S CLI uzantısı altında kullanılır), tüm kancaları aşağıdaki hata ile çalıştırmaya neden olan bir [soruna](https://github.com/helm/helm/pull/8527) sahiptir:

```console
$ az connectedk8s connect -n shasbakstest -g shasbakstest
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Ensure that you have the latest helm version installed before proceeding.
This operation might take a while...

Please check if the azure-arc namespace was deployed and run 'kubectl get pods -n azure-arc' to check if all the pods are in running state. A possible cause for pods stuck in pending state could be insufficientresources on the kubernetes cluster to onboard to arc.
ValidationError: Unable to install helm release: Error: customresourcedefinitions.apiextensions.k8s.io "connectedclusters.arc.azure.com" not found
```

Bu sorundan kurtulmak için aşağıdaki adımları izleyin:

1. Azure portal Azure Arc etkin Kubernetes kaynağını silin.
2. Makinenizde aşağıdaki komutları çalıştırın:
    
    ```console
    kubectl delete ns azure-arc
    kubectl delete clusterrolebinding azure-arc-operator
    kubectl delete secret sh.helm.release.v1.azure-arc.v1
    ```

3. Release Candidate sürümü yerine makinenize Held 3 ' ün [kararlı bir sürümünü yükler](https://helm.sh/docs/intro/install/) .
4. `az connectedk8s connect`Kümeyi Azure yaya bağlamak için uygun değerlerle komutu çalıştırın.

## <a name="configuration-management"></a>Yapılandırma yönetimi

### <a name="general"></a>Genel
Kaynak denetimi yapılandırmasıyla ilgili sorunları gidermeye yardımcı olmak için,--hata ayıklama anahtarıyla az Commands komutunu çalıştırın.

```console
az provider show -n Microsoft.KubernetesConfiguration --debug
az k8sconfiguration create <parameters> --debug
```

### <a name="create-source-control-configuration"></a>Kaynak denetimi yapılandırması oluştur
Microsoft. Kubernetes/connectedCluster kaynağında katkıda bulunan rolü, Microsoft. KubernetesConfiguration/sourceControlConfiguration kaynağı oluşturmak için gereklidir ve yeterlidir.

### <a name="configuration-remains-pending"></a>Yapılandırma kalır `Pending`

```console
kubectl -n azure-arc logs -l app.kubernetes.io/component=config-agent -c config-agent
$ k -n pending get gitconfigs.clusterconfig.azure.com  -o yaml
apiVersion: v1
items:
- apiVersion: clusterconfig.azure.com/v1beta1
  kind: GitConfig
  metadata:
    creationTimestamp: "2020-04-13T20:37:25Z"
    generation: 1
    name: pending
    namespace: pending
    resourceVersion: "10088301"
    selfLink: /apis/clusterconfig.azure.com/v1beta1/namespaces/pending/gitconfigs/pending
    uid: d9452407-ff53-4c02-9b5a-51d55e62f704
  spec:
    correlationId: ""
    deleteOperator: false
    enableHelmOperator: false
    giturl: git@github.com:slack/cluster-config.git
    helmOperatorProperties: null
    operatorClientLocation: azurearcfork8s.azurecr.io/arc-preview/fluxctl:0.1.3
    operatorInstanceName: pending
    operatorParams: '"--disable-registry-scanning"'
    operatorScope: cluster
    operatorType: flux
  status:
    configAppliedTime: "2020-04-13T20:38:43.081Z"
    isSyncedWithAzure: true
    lastPolledStatusTime: ""
    message: 'Error: {exit status 1} occurred while doing the operation : {Installing
      the operator} on the config'
    operatorPropertiesHashed: ""
    publicKey: ""
    retryCountPublicKey: 0
    status: Installing the operator
kind: List
metadata:
  resourceVersion: ""
  selfLink: ""
```
## <a name="monitoring"></a>İzleme

Kapsayıcılar için Azure Izleyici, DaemonSet 'in ayrıcalıklı modda çalıştırılmasını gerektirir. İzleme için kurallı bir Kubernetes kümesini başarılı bir şekilde ayarlamak için aşağıdaki komutu çalıştırın:

```console
juju config kubernetes-worker allow-privileged=true
```