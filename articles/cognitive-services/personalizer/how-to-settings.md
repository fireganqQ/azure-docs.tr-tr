---
title: Kişiselleştirme’yi yapılandırma
description: Hizmet yapılandırması, hizmetin yeniden nasıl davrandığını, hizmetin ne sıklıkla incelendiğine, modelin ne sıklıkta geri alınacağını ve ne kadar veri depolandığını içerir.
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: how-to
ms.date: 04/29/2020
ms.openlocfilehash: 9b7347cb98bcbf2e1d92f115d404197083acef4d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91292623"
---
# <a name="configure-personalizer-learning-loop"></a>Kişiselleştirici öğrenme döngüsünü yapılandırma

Hizmet yapılandırması, hizmetin yeniden nasıl davrandığını, hizmetin ne sıklıkla incelendiğine, modelin ne sıklıkta geri alınacağını ve ne kadar veri depolandığını içerir.

**Yapılandırma** sayfasında, söz konusu kişiselleştirici kaynağı için Azure Portal için öğrenme döngüsünü yapılandırın.

<a name="configure-service-settings-in-the-azure-portal"></a>
<a name="configure-reward-settings-for-the-feedback-loop-based-on-use-case"></a>

## <a name="planning-configuration-changes"></a>Yapılandırma değişikliklerini planlama

Bazı yapılandırma değişiklikleri [modelinizi sıfırlacağından](#settings-that-include-resetting-the-model), yapılandırma değişikliklerinizi planlamanız gerekir.

[Apprenlet modunu](concept-apprentice-mode.md)kullanmayı planlıyorsanız, apprenlet moduna geçmeden önce kişiselleştirici yapılandırmanızı gözden geçirdiğinizden emin olun.

<a name="clear-data-for-your-learning-loop"></a>

## <a name="settings-that-include-resetting-the-model"></a>Modeli sıfırlamayı içeren ayarlar

Aşağıdaki eylemler, son 2 güne kadar kullanılabilir olan verileri kullanarak modelin yeniden eğitimini tetikler.

* Ödül
* Araştırma

Tüm verilerinizi [temizlemek](how-to-manage-model.md) için **model ve öğrenme ayarları** sayfasını kullanın.

## <a name="configure-rewards-for-the-feedback-loop"></a>Geri bildirim döngüsü için yeniden ödüller yapılandırma

Öğrenme döngünüz için hizmeti yapılandırın. Aşağıdaki değerlerde yapılan değişiklikler geçerli kişiselleştirici modeli sıfırlar ve son 2 güne kadar verileri yeniden eğitecektir.

> [!div class="mx-imgBorder"]
> ![Geri bildirim döngüsü için geri değer değerlerini yapılandırma](media/settings/configure-model-reward-settings.png)

|Değer|Amaç|
|--|--|
|Bekleme süresi|Bir derecelendirme çağrısı için Kişiselleştiriciye ait değer toplama işleminin ne kadar süre olacağını belirleyen süreyi ayarlar. Bu değer şu sorarak ayarlanır: "kişiselleştirici, yeniden ödüller için ne kadar bekleneceği?" Bu pencere günlüğe yazılır, ancak öğrenimi için kullanılmaz.|
|Varsayılan değer|Bir derecelendirme çağrısıyla ilişkili bir yeniden bekleme süresi penceresi sırasında kişiselleştirici tarafından hiçbir yeniden çağrı alınmıyorsa, kişiselleştirici varsayılan bir ödül atar. Varsayılan olarak ve çoğu senaryoda varsayılan değer sıfırdır (0).|
|Yeniden toplama|Aynı derecelendirme API çağrısı için birden fazla yeniden yer alınmışsa, bu toplama yöntemi kullanılır: **Sum** veya **en erken**. En erken alınan puanı seçer ve kalanı atar. Muhtemelen yinelenen çağrılar arasında benzersiz bir değer istiyorsanız bu yararlı olur. |

Bu değerleri değiştirdikten sonra **Kaydet**' i seçtiğinizden emin olun.

## <a name="configure-exploration-to-allow-the-learning-loop-to-adapt"></a>Öğrenme döngüsünün uyarlanmasına izin vermek için araştırmayı yapılandırın

Kişiselleştirme, eğitilen modelin tahminini kullanmak yerine alternatifleri inceleyerek yeni desenleri bulabilir ve zaman içinde Kullanıcı davranışı değişikliklerine uyum sağlayabilir. **Araştırma** değeri, araştırma Için hangi derece çağrı yüzdesinin yanıtlandığını belirler.

Bu değerde yapılan değişiklikler geçerli kişiselleştirici modeli sıfırlar ve son 2 güne kadar yeniden eğitecektir.

![Araştırma değeri, araştırma ile hangi derece çağrı yüzdesinin yanıtlandığını belirler](media/settings/configure-exploration-setting.png)

Bu değeri değiştirdikten sonra **Kaydet**' i seçtiğinizden emin olun.

<a name="model-update-frequency"></a>

## <a name="configure-model-update-frequency-for-model-training"></a>Model eğitimi için model güncelleştirme sıklığını yapılandırma

**Model güncelleştirme sıklığı** , modelin eğitilme sıklığını ayarlar.

|Sıklık ayarı|Amaç|
|--|--|
|1 dakika|Tek dakikalık güncelleştirme frekansları, bir uygulama kodunun kişiselleştirici kullanılarak **hata ayıklaması** , tanıtımlar yapılması veya makine öğrenimi yönlerini etkileşimli olarak test edilmesi için yararlıdır.|
|15 dakika|Yüksek model güncelleştirme frekansları, Kullanıcı davranışlardaki **değişiklikleri yakından izlemek** istediğiniz durumlar için yararlıdır. Örnek olarak canlı haberler, viral içerik veya canlı Ürün Deklarasyon üzerinde çalışan siteler bulunur. Bu senaryolarda 15 dakikalık bir sıklık kullanabilirsiniz. |
|1 saat|Çoğu kullanım durumu için, daha düşük bir güncelleştirme sıklığı etkilidir.|

![Model güncelleştirme sıklığı, yeni bir kişiselleştirici modelin ne sıklıkta geri alınacağını ayarlar.](media/settings/configure-model-update-frequency-settings-15-minutes.png)

Bu değeri değiştirdikten sonra **Kaydet**' i seçtiğinizden emin olun.

## <a name="data-retention"></a>Veri saklama

**Veri saklama süresi** , kişiselleştirmede kaç gün veri günlüğü tutacağını ayarlar. Son veri günlükleri, kişiselleştirici 'nin verimliliğini ölçmek ve öğrenme Ilkesini iyileştirmek için kullanılan [çevrimdışı değerlendirmeler](concepts-offline-evaluation.md)gerçekleştirmek için gereklidir.

Bu değeri değiştirdikten sonra **Kaydet**' i seçtiğinizden emin olun.



## <a name="next-steps"></a>Sonraki adımlar

[Modelinizi yönetme hakkında bilgi edinin](how-to-manage-model.md)
