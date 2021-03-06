---
title: Azaltmayı izleme ve azaltma-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights gecikme süresi ve azaltmasına neden olan performans sorunlarını izleme, tanılama ve azaltma hakkında bilgi edinin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 09/29/2020
ms.custom: seodec18
ms.openlocfilehash: e89189b22b144d9e92ee8315bc6fd9aabe699eec
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91531658"
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights-gen1"></a>Azure Time Series Insights Gen1 gecikme süresini azaltmak için azaltmayı izleyin ve azaltır

> [!CAUTION]
> Bu bir Gen1 makaledir.

Gelen veri miktarı ortamınızın yapılandırmasını aştığında, Azure Time Series Insights gecikme süresi veya azaltma yaşayabilirsiniz.

Ortamınızı çözümlemek istediğiniz veri miktarı için düzgün şekilde yapılandırarak gecikme süresini ve azaltmaktan kaçınabilirsiniz.

Şu durumlarda gecikme süresi ve azaltma deneyiminden karşılaşabilirsiniz:

- Ayrılan giriş hızınızı aşmayacak eski verileri içeren bir olay kaynağı ekleyin (Azure Time Series Insights catch gerekecektir).
- Bir ortama daha fazla olay kaynağı ekleyin ve ek olaylardan bir ani artış elde edin (ortamınızın kapasitesini aşabilir).
- Büyük miktarlarda geçmiş olayları bir olay kaynağına göndererek bir gecikme (Azure Time Series Insights yakalamalı olması gerekir) sonucunu vermez.
- Başvuru verilerini telemetri ile birleştirin ve daha büyük bir olay boyutuna neden olur. İzin verilen en büyük paket boyutu 32 KB; 32 KB 'den büyük veri paketleri kesilir.

## <a name="video"></a>Video

### <a name="learn-about-azure-time-series-insights-data-ingress-behavior-and-how-to-plan-for-itbr"></a>Azure Time Series Insights veri giriş davranışı ve bunun nasıl planlanacağı hakkında bilgi edinin.</br>

> [!VIDEO https://www.youtube.com/embed/npeZLAd9lxo]

## <a name="monitor-latency-and-throttling-with-alerts"></a>Uyarıları izleyen uyarılarla gecikme ve azaltma

Uyarılar, ortamınızda oluşan gecikme sorunlarını tanılamanıza ve azaltmanıza yardımcı olabilir.

1. Azure portal, Azure Time Series Insights ortamınızı seçin. Ardından **Uyarılar**' ı seçin.

   [![Azure Time Series Insights ortamınıza bir uyarı ekleme](media/environment-mitigate-latency/mitigate-latency-add-alert.png)](media/environment-mitigate-latency/mitigate-latency-add-alert.png#lightbox)

1. **+ Yeni uyarı kuralı**'nı seçin. **Kural oluştur** paneli görüntülenir. **Koşul**altında **Ekle** ' yi seçin.

   [![Uyarı bölmesi Ekle](media/environment-mitigate-latency/mitigate-latency-add-pane.png)](media/environment-mitigate-latency/mitigate-latency-add-pane.png#lightbox)

1. Sonra, sinyal mantığı için tam koşulları yapılandırın.

   [![Sinyal mantığını yapılandırma](media/environment-mitigate-latency/configure-alert-rule.png)](media/environment-mitigate-latency/configure-alert-rule.png#lightbox)

   Buradan, aşağıdaki koşullardan bazılarını kullanarak uyarılar yapılandırabilirsiniz:

   |Ölçüm  |Açıklama  |
   |---------|---------|
   |**Alınan bayt sayısı**     | Olay kaynaklarından okunan ham bayt sayısı. Ham sayı genellikle özellik adını ve değerini içerir.  |  
   |**Giriş geçersiz Iletiler aldı**     | Tüm Azure Event Hubs veya Azure IoT Hub olay kaynaklarından okunan geçersiz iletilerin sayısı.      |
   |**Giriş alınan Iletiler**   | Tüm Event Hubs veya IoT Hub 'Ları olay kaynaklarından okunan ileti sayısı.        |
   |**Giriş depolanan baytlar**     | Sorgu için depolanan ve kullanılabilir olayların toplam boyutu. Boyut yalnızca özellik değeri üzerinde hesaplanır.        |
   |Giriş **saklı olayları**    |   Depolanan ve sorgu için kullanılabilir düzleştirilmiş olay sayısı.      |
   |**Alınan Ileti zaman gecikmesi alındı**   |  İleti olay kaynağında sıraya alındığı zaman ve giriş sırasında işlendiği zaman arasındaki saniye cinsinden fark.      |
   |Giriş **alınan Ileti sayısı gecikmesi**   |  Olay kaynak bölümünde en son sıraya alınan iletinin sıra numarası ve giriş olarak işlenen iletinin sıra numarası arasındaki fark.      |

   **Bitti** seçeneğini belirleyin.

1. İstenen sinyal mantığını yapılandırdıktan sonra, seçilen uyarı kuralını görsel olarak gözden geçirin.

   [![Gecikme süresi görünümü ve grafik](media/environment-mitigate-latency/mitigate-latency-view-and-charting.png)](media/environment-mitigate-latency/mitigate-latency-view-and-charting.png#lightbox)

## <a name="throttling-and-ingress-management"></a>Daraltma ve giriş yönetimi

- Kısıtladıysanız, *gelen Ileti zaman* gecikmesinin bir değeri, ileti olay kaynağını (Appx dizin oluşturma süresi dışında), iletinin ne kadar saniye Azure Time Series Insights içinde olduğunu belirten Ileti saati gecikmesi için bir değer kaydedilir. 30-60 saniye).  

  *Alınan Ileti sayısı gecikmesi* de bir değer içermelidir, bu da arkasında kaç ileti olduğunu belirlemenizi sağlar.  En kolay şekilde, ortamınızın kapasitesini, farkı aşmanızı sağlayacak bir boyuta artırmanız gerekir.  

  Örneğin, S1 ortamınız 5.000.000 ileti gecikmesi içeriyorsa, bir gün boyunca daha fazla işlem yapmak için ortamınızın boyutunu altı birim olarak artırabilirsiniz.  Daha hızlı bir şekilde daha fazla yakalamak için daha fazla artırabilirsiniz. Yakalama dönemi, özellikle bir ortamı ilk kez sağlarken, özellikle de olayları zaten olan bir olay kaynağına bağladığınızda veya çok sayıda geçmiş verileri toplu olarak karşıya yüklerken yaygın bir oluşumdır.

- Başka bir yöntem de bir giriş **saklı olayları** ayarlaması >= bir eşik değeri 2 saatlik bir dönem boyunca toplam ortam kapasitenizin altına göre biraz daha düşük.  Bu uyarı, gecikme süresinin yüksek olma olasılığını gösteren kapasiteyi sürekli olarak anlamanıza yardımcı olabilir.

  Örneğin, üç adet S1 birimi sağlanmışsa (veya bir dakika başına 2100 olay), 2 saat boyunca >= 1900 olay için bir giriş **saklı olayları** uyarısı ayarlayabilirsiniz. Bu eşiği sürekli aşdıysanız ve bu nedenle uyarınızı tetikleyerek, büyük olasılıkla sağlanmış olursunuz.  

- Kısıtladığınızı kuşkulanıyorsanız, giriş **alınan iletilerinizi** olay kaynağınızın yumurtılan iletileriyle karşılaştırabilirsiniz.  Olay Hub 'ınız giriş **alınızdan**daha büyükse, Azure Time Series Insights muhtemelen kısıtlanıyor demektir.

## <a name="improving-performance"></a>Performansı artırma

Azaltmayı veya gecikmeyi azaltmak için, bunu düzeltmenin en iyi yolu ortamınızın kapasitesini artırmaktır.

Ortamınızı çözümlemek istediğiniz veri miktarı için düzgün şekilde yapılandırarak gecikme süresini ve azaltmaktan kaçınabilirsiniz. Ortamınıza kapasite ekleme hakkında daha fazla bilgi için [ortamınızın ölçeğini](time-series-insights-how-to-scale-your-environment.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Time Series Insights ortamınızda sorunları tanılama ve çözme](time-series-insights-diagnose-and-solve-problems.md)hakkında bilgi edinin.

- [Azure Time Series Insights ortamınızı ölçeklendirmeyi](time-series-insights-how-to-scale-your-environment.md)öğrenin.
