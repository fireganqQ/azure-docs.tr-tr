---
title: Azure Arc etkin Kubernetes sık sorulan sorular
services: azure-arc
ms.service: azure-arc
ms.date: 02/17/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Bu makale, Azure Arc etkin Kubernetes ile ilgili sık sorulan soruların bir listesini içerir
keywords: Kubernetes, yay, Azure, kapsayıcılar, yapılandırma, Gilar, SSS
ms.openlocfilehash: e0d7501dc1a82940571d0168222c396f61a70bce
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100652505"
---
# <a name="frequently-asked-questions---azure-arc-enabled-kubernetes"></a>Sık sorulan sorular-Azure Arc etkin Kubernetes

Bu makalede, Azure Arc etkin Kubernetes hakkında sık sorulan sorular ele alınmaktadır.

## <a name="what-is-the-difference-between-azure-arc-enabled-kubernetes-and-azure-kubernetes-service-aks"></a>Azure Arc etkin Kubernetes ve Azure Kubernetes hizmeti (AKS) arasındaki fark nedir?

AKS, Azure tarafından yönetilen Kubernetes teklifidir. AKS, Azure 'da karmaşıklık ve operasyonel ek yükün çoğunu boşaltarak Azure 'da yönetilen bir Kubernetes kümesinin dağıtılmasını basitleştirir. Kubernetes yöneticileri Azure tarafından yönetildiğinden yalnızca aracı düğümlerini yönetebilir ve bakımını yapabilirsiniz.

Azure Arc etkin Kubernetes, Kubernetes kümelerini Azure 'a bağlayarak Azure 'un yönetim özelliklerini (Azure Izleyici ve Azure Ilkesi gibi) genişletmenizi sağlar. Temel Kubernetes kümesinin kendisini korursunuz.

## <a name="do-i-need-to-connect-my-aks-clusters-running-on-azure-to-azure-arc"></a>Azure 'da çalışan AKS kümelerimi Azure yaya bağlamanız gerekiyor mu?

Hayır. Azure Izleyici ve Azure Ilkesi (Gatekeeper) dahil olmak üzere tüm Azure Arc etkin Kubernetes Özellikleri AKS (Azure Resource Manager içindeki yerel bir kaynak) ile kullanılabilir.
    
## <a name="should-i-connect-my-aks-hci-cluster-and-kubernetes-clusters-on-azure-stack-hub-and-azure-stack-edge-to-azure-arc"></a>AKS-HI kümesi ve Kubernetes kümelerimi Azure Stack hub 'a ve Azure Stack kenarından Azure yaya bağlamanız gerekir mi?

Evet, AKS-HI kümenizi veya Kubernetes kümelerinizi Azure Stack Edge veya Azure Stack hub 'a bağlamak Azure Resource Manager ' de kaynak gösterimine sahip kümeler sağlar. Bu kaynak temsili, küme yapılandırması, Azure Izleyici ve Azure Ilkesi (Gatekeeper) gibi özellikleri, bağlı Kubernetes kümelerine genişletir.

## <a name="how-to-address-expired-azure-arc-enabled-kubernetes-resources"></a>Kullanım dışı Azure Arc etkin Kubernetes kaynakları nasıl ele alınacağını?

Azure Arc etkin Kubernetes ile ilişkili Yönetilen Hizmet Kimliği (MSI) sertifikası, 90 günlük bir süre sonu penceresine sahiptir. Bu sertifikanın süresi dolduktan sonra kaynak kabul edilir `Expired` ve tüm Özellikler (yapılandırma, izleme ve ilke gibi) bu kümede çalışmayı durdurur. Kubernetes kümenizi Azure Arc ile yeniden çalışmaya almak için:

1. Kümedeki Azure Arc etkin Kubernetes kaynağını ve aracılarını silin. 

    ```console
    az connectedk8s delete -n <name> -g <resource-group>
    ```

1. Kümedeki aracıları dağıtarak Azure Arc etkin Kubernetes kaynağını yeniden oluşturun.
    
    ```console
    az connectedk8s connect -n <name> -g <resource-group>
    ```

> [!NOTE]
> `az connectedk8s delete` Ayrıca, kümenin üstündeki konfigürasyonları da siler. Çalıştırdıktan sonra, `az connectedk8s connect` el ile veya Azure ilkesi kullanarak kümedeki konfigürasyonları yeniden oluşturun.

## <a name="if-i-am-already-using-cicd-pipelines-can-i-still-use-azure-arc-enabled-kubernetes-and-configurations"></a>Zaten CI/CD işlem hatları kullanıyorum, Azure Arc etkinleştirilmiş Kubernetes ve konfigürasyonları kullanmaya devam edebilir miyim?

Evet, bir CI/CD işlem hattı aracılığıyla bir küme alma dağıtımında yapılandırma kullanmaya devam edebilirsiniz. Geleneksel CI/CD işlem hatları ile karşılaştırıldığında, yapılandırmaların iki ek avantajı vardır:

**Drift mutabakatı**

CI/CD işlem hattı, işlem hattı çalıştırması sırasında değişiklikleri yalnızca bir kez uygular. Ancak kümedeki Gila işleci, kümedeki Kubernetes kaynaklarının istenen durumunu getirmek için Git deposunu sürekli olarak yoklar. Giüstler işleci, istenen kaynak durumunu kümedeki kaynakların gerçek durumundan farklı olacak şekilde bulursa, bu DRIP mutabakatı yapılır.

**Gilar 'ı ölçeğe göre Uygula**

CI/CD işlem hatları, Kubernetes kümenize yönelik olay odaklı dağıtımlar (örneğin, bir git deposuna gönderim) için faydalıdır. Ancak, aynı yapılandırmayı Kubernetes kümelerinize dağıtmak istiyorsanız, her bir Kubernetes kümesinin kimlik bilgilerini CI/CD işlem hattına el ile yapılandırmanız gerekir. 

Azure Resource Manager yapılandırmalarını yönettiğinden, Azure Arc etkin Kubernetes için, bir aboneliğin veya bir kaynak grubunun kapsamındaki Azure Ilkesi 'ni kullanarak tüm Azure Arc etkin Kubernetes kaynakları genelinde aynı yapılandırmanın oluşturulmasını otomatik hale getirebilirsiniz. Bu özellik, ilke atamasından sonra oluşturulan Azure Arc etkin Kubernetes kaynakları için de geçerlidir.

Bu özellik, uyumluluk ve idare gereksinimlerini karşılamak üzere tüm Kubernetes kümesi envanterinde temel yapılandırma (ağ ilkeleri, rol bağlamaları ve pod güvenlik ilkeleri gibi) uygular.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir kümeyi Azure yaya bağlama](./connect-cluster.md)
* [Arc etkin Kubernetes kümesinde yapılandırma oluşturma](./use-gitops-connected-cluster.md)
* [Uygun ölçekte yapılandırma uygulamak için Azure Ilkesini kullanma](./use-azure-policy.md)
