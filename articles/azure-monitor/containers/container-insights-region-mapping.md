---
title: Kapsayıcılar için Azure Izleyici bölge eşlemeleri
description: Kapsayıcılar için Azure Izleyici, Log Analytics çalışma alanı ve özel ölçümler arasında desteklenen bölge eşlemelerini açıklar.
ms.topic: conceptual
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: 9b77242de3e7845a97874b663266103bf00a8e66
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100624907"
---
# <a name="region-mappings-supported-by-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici tarafından desteklenen bölge eşlemeleri

 Kapsayıcılar için Azure Izleyicisini etkinleştirirken, Log Analytics çalışma alanını ve AKS kümesini bağlamak ve Azure Izleyici 'ye gönderilen özel ölçümleri toplamak için yalnızca belirli bölgeler desteklenir.

## <a name="log-analytics-workspace-supported-mappings"></a>Log Analytics çalışma alanı desteklenen eşlemeler

Desteklenen AKS bölgeleri, [bölgeye göre kullanılabilen ürünlerde](https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service)listelenmiştir. Log Analytics çalışma alanı, aşağıdaki tabloda listelenen bölgeler hariç aynı bölgede olmalıdır. Güncelleştirmeler için [aks sürüm notlarını](https://github.com/Azure/AKS/releases) izleyin.


|**AKS küme bölgesi** | **Log Analytics çalışma alanı bölgesi** |
|-----------------------|------------------------------------|
|**Afrika** | |
|SouthAfricaNorth |WestEurope |
|SouthAfricaWest |WestEurope |
|**Avustralya** | |
|AustraliaCentral2 |AustraliaCentral |
|**Brezilya** | |
|BrazilSouth | Güneydoğu ABD |
|**Kanada** ||
|Canadadoğu |Canadaorta |
|**Avrupa** | |
|FranceSouth |Francecna al |
|Ukbatı |UKSouth |
|**Hindistan** | |
|Güneydoğu |Merkezileştirme Hindistan |
|WestIndia |Merkezileştirme Hindistan |
|**Japonya** | |
|JapanWest |JapanEast |
|**Güney Kore** | |
|Koreagüney |KoreaCentral |
|**ABD** | |
|WestCentralUS<sup>1</sup>|EastUS<sup>1</sup>|


<sup>1</sup> kapasite depoları nedeniyle, yeni kaynaklar oluşturulurken bölge kullanılamıyor. Buna bir Log Analytics çalışma alanı dahildir. Ancak, bölgede önceden var olan bağlı kaynakların çalışmaya devam etmesi gerekir.

## <a name="custom-metrics-supported-regions"></a>Özel Ölçüm desteklenen bölgeler

Azure Kubernetes Services (AKS) kümeleri düğümlerinden ve yığınlarından ölçümlerin toplanması yalnızca aşağıdaki [Azure bölgelerinde](../essentials/metrics-custom-overview.md#supported-regions)özel ölçümler olarak yayımlama için desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

AKS kümenizi izlemeye başlamak için, izlemeyi etkinleştirmek üzere gereksinimleri ve kullanılabilir yöntemleri anlamak üzere [kapsayıcılar Için Azure Izleyicisini nasıl etkinleştireceğinizi](container-insights-onboard.md) gözden geçirin.  
