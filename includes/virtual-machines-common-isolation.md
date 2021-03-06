---
title: include dosyası
description: include dosyası
services: virtual-machines
author: styli365
ms.service: virtual-machines
ms.topic: include
ms.date: 11/05/2020
ms.author: sttsinar
ms.custom: include file
ms.openlocfilehash: e22c2b7cb561e30e84ea5ede5481fbdc35be8cdf
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100515089"
---
Azure Işlem, belirli bir donanım türüne yalıtılmış ve tek bir müşteriye adanmış sanal makine boyutları sunar. Yalıtılmış boyutlar canlı ve belirli donanım nesli üzerinde çalışır ve donanım oluşturma kullanımdan kaldırıldığında kullanım dışı kalır.

Yalıtılmış sanal makine boyutları, diğer müşterilerin iş yüklerinden, Toplantı uyumluluk ve mevzuat gereksinimlerini kapsayan nedenlerle yüksek derecede yalıtım gerektiren iş yükleri için idealdir.  Yalıtılmış bir boyut kullanılması, sanal makinenizin o belirli sunucu örneğinde çalışan tek bir tane olmasını garanti eder. 


Ayrıca, yalıtılmış boyut sanal makineleri büyük olduğu için müşteriler, [iç içe geçmiş sanal makineler Için Azure desteği](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)'ni kullanarak bu VM 'lerin kaynaklarını alt bölümlere ayırarak seçebiliriz.

Geçerli yalıtılmış sanal makine teklifleri şunları içerir:
* Standard_E80ids_v4
* Standard_E80is_v4
* Standard_F72s_v2
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5


> [!NOTE]
> Yalıtılmış VM boyutları bir donanım sınırlı ömrü vardır. Ayrıntılar için lütfen aşağıya bakın

## <a name="deprecation-of-isolated-vm-sizes"></a>Yalıtılmış VM boyutlarının kullanımdan kaldırılması

Yalıtılmış VM boyutları bir donanım sınırlı ömrü vardır. Azure, boyutların resmi kullanım dışı bırakılmasıyla ilgili 12 ay önce anımsatıcılar verir ve sizin de sizin de sizin için güncel bir yalıtılmış teklif sağlar.

| Boyut | Yalıtım kullanımdan kaldırma tarihi | 
| --- | --- |
| Standard_DS15_v2<sup>1</sup> | 15 Mayıs 2020 |
| Standard_D15_v2<sup>1</sup>  | 15 Mayıs 2020 |

<sup>1</sup>  Standard_DS15_v2 ve Standard_D15_v2 yalıtımı devre dışı bırakma programı hakkında ayrıntılar için bkz. SSS


## <a name="faq"></a>SSS
### <a name="q-is-the-size-going-to-get-retired-or-only-its-isolation-feature"></a>S: boyut, kullanımdan kalkması veya yalnızca "yalıtım" özelliğine gidiyor mu?
Y **: sanal** makine boyutunun "i" alt indisi yoksa yalnızca "yalıtım" özelliği devre dışı bırakılır. Yalıtım gerekmiyorsa, gerçekleştirilecek bir eylem yoktur ve VM beklendiği gibi çalışmaya devam eder. Örnekler şunlardır Standard_DS15_v2, Standard_D15_v2, Standard_M128ms vb. Sanal makine boyutu "i" alt indisi içeriyorsa, boyut kullanımdan kaldırılacak.

### <a name="q-is-there-a-downtime-when-my-vm-lands-on-a-non-isolated-hardware"></a>S: sanal makine, yalıtılmış olmayan bir donanımda yer aldığı zaman kapalı kalma süresi var mı?
Y: yalıtıma gerek duyulmadığında hiçbir işlem yapılması gerekmez ve kapalı kalma süresi olmayacaktır.

### <a name="q-is-there-any-cost-delta-for-moving-to-a-non-isolated-virtual-machine"></a>S: yalıtılmış olmayan bir sanal makineye geçmek için herhangi bir maliyet Delta mı var?
Y **: Hayır**

### <a name="q-when-are-the-other-isolated-sizes-going-to-retire"></a>S: diğer yalıtılmış boyutlar ne zaman devre dışı bırakılacak?
Y **: yalıtılmış** boyutun resmi kullanım dışı bırakılmasının sonunda 12 ay boyunca anımsatıcılar sağlıyoruz.

### <a name="q-im-an-azure-service-fabric-customer-relying-on-the-silver-or-gold-durability-tiers-does-this-change-impact-me"></a>S: gümüş veya altın dayanıklılık katmanlarına bağlı bir Azure Service Fabric müşteriyim. Bu değişiklik beni etkiler mi?
Y **: Hayır**. Service Fabric [dayanıklılık katmanları](../articles/service-fabric/service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster) tarafından belirtilen garantiler, bu değişiklikten sonra bile çalışmaya devam edecektir. Diğer nedenlerle fiziksel donanım yalıtımına ihtiyacınız varsa yukarıda açıklanan eylemlerden birini yapmanız gerekebilir. 
 
### <a name="q-what-are-the-milestones-for-d15_v2-or-ds15_v2-isolation-retirement"></a>S: D15_v2 veya DS15_v2 yalıtımı kullanımdan kaldırma için kilometre taşları nelerdir? 
Y: 
 
| Tarih | Eylem |
|---|---| 
| 18 Kasım 2019 | D/DS15i_v2 (PAYG, 1 yıllık RI) kullanılabilirliği | 
| 14 Mayıs 2020 | DS15i_v2 1-yıl RI satın almak için geçen gün | 
| 15 Mayıs 2020 | D/DS15_v2 yalıtım garantisi kaldırıldı | 
| 15 Mayıs 2021 | Devre dışı bırakma D/DS15i_v2 (18 Kasım 2019 tarihinden önce 3 yıllık D/DS15_v2 dışında tüm müşteriler)| 
| 17 Kasım 2022 | 3 yıllık RIS tamamlandığında D/DS15i_v2 devre dışı bırak (18 Kasım 2019 tarihinden önce 3 yıllık bir D/DS15_v2 satın alan müşteriler için) |

## <a name="next-steps"></a>Sonraki adımlar

Müşteriler Ayrıca, [iç içe geçmiş sanal makineler Için Azure desteği](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)'ni kullanarak bu yalıtılmış sanal makinelerin kaynaklarını daha fazla alt bölümlere ayırmak da tercih edebilir.
