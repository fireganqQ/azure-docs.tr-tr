---
title: V3-Translator ' a geçiş
titleSuffix: Azure Cognitive Services
description: Bu makalede, v2 'den Azure bilişsel hizmetler çeviricisinin v3 'e geçiş yapmanıza yardımcı olacak adımlar sağlanmaktadır.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: lajanuar
ms.openlocfilehash: 13c4d39284fad293c945f8b7e31076dccee84fda
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98896842"
---
# <a name="translator-v2-to-v3-migration"></a>Translator v2-v3 geçişine

> [!NOTE]
> V2, 30 Nisan 2018 tarihinde kullanımdan kaldırılmıştır. Yalnızca v3 'de bulunan yeni işlevsellikten yararlanmak için uygulamalarınızı v3 'e geçirin. V2 24 Mayıs 2021 tarihinde kullanımdan kaldırılacaktır. 

Microsoft Translator ekibi, çeviricisinin sürüm 3 ' ü (v3) yayımladı. Bu sürüm, yeni özellikler, kullanım dışı Yöntemler ve Microsoft Translator hizmetinden veri almaya yönelik yeni bir biçim içerir. Bu belge, uygulamaları v3 kullanacak şekilde değiştirmeye yönelik bilgiler sağlar. 

Bu belgenin sonu, daha fazla bilgi edinmek için yararlı bağlantılar içerir.

## <a name="summary-of-features"></a>Özelliklerin özeti

* Hiçbir Izleme v3 No-Trace, Azure portal tüm fiyatlandırma katmanlarına uygulanır. Bu özellik, v3 API 'sine gönderilen metinlerin Microsoft tarafından kaydedileceği anlamına gelir.
* JSON-XML dosyası JSON ile değiştirilmiştir. Hizmete gönderilen ve hizmetten alınan tüm veriler JSON biçimindedir.
* Tek bir istekte birden çok hedef dil-çevirme yöntemi, tek bir istekteki çeviri için birden fazla ' to ' dillerini kabul eder. Örneğin, tek bir istek ' Ingilizce ve '-' Almanca, Ispanyolca ve Japonca ya da başka bir dil grubu olabilir.
* İki dilli sözlük-API 'ye iki dilli sözlük yöntemi eklenmiştir. Bu yöntem ' Lookup ' ve ' örnekler ' içerir.
* Alfabe içinde, API 'ye bir alfabede bir yöntem eklenmiştir. Bu yöntem, sözcükleri ve tümceleri tek bir betikte dönüştürür (ör. Arapça) başka bir betiğe (ör. Latin).
* Diller-yeni bir ' Languages ' yöntemi, ' Translate ', ' Dictionary ' ve ' alfabede ' yöntemleriyle kullanılmak üzere dil bilgilerini JSON biçiminde sunar.
* Çeviri için yeni-' Çevir ' yöntemine yeni özellikler eklenmiştir. v2 API 'sinde ayrı yöntemler olarak bulunan özelliklerden bazıları desteklenir. TranslateArray örneği bir örnektir.
* Konuş yöntemi-metin okuma işlevselliği artık Microsoft Translator 'da desteklenmemektedir. Metin okuma işlevselliği, [Microsoft konuşma hizmeti](../speech-service/text-to-speech.md)'nde bulunabilir.

Aşağıdaki v2 ve v3 yöntemleri listesi, v2 ile gelen işlevleri sağlayacak olan v3 yöntemlerini ve API 'Leri tanımlar.

| V2 API yöntemi   | V3 API uyumluluğu |
|:----------- |:-------------|
| `Translate`     | [Çevir](reference/v3-0-translate.md)          |
| `TranslateArray`      | [Çevir](reference/v3-0-translate.md)        |
| `GetLanguageNames`      | [Diller](reference/v3-0-languages.md)         |
| `GetLanguagesForTranslate`     | [Diller](reference/v3-0-languages.md)       |
| `GetLanguagesForSpeak`      | [Microsoft konuşma hizmeti](../speech-service/language-support.md#text-to-speech)         |
| `Speak`     | [Microsoft konuşma hizmeti](../speech-service/text-to-speech.md)          |
| `Detect`     | [Acağı](reference/v3-0-detect.md)         |
| `DetectArray`     | [Acağı](reference/v3-0-detect.md)         |
| `AddTranslation`     | Özellik artık desteklenmiyor       |
| `AddTranslationArray`    | Özellik artık desteklenmiyor          |
| `BreakSentences`      | [BreakSentence](reference/v3-0-break-sentence.md)       |
| `GetTranslations`      | Özellik artık desteklenmiyor         |
| `GetTranslationsArray`      | Özellik artık desteklenmiyor         |

## <a name="move-to-json-format"></a>JSON biçimine taşı

Microsoft Translator çeviri v2, XML biçimindeki verileri kabul etti ve döndürdü. V3 'de, API kullanılarak gönderilen ve alınan tüm veriler JSON biçimindedir. XML artık v3 'de kabul edilmez veya döndürülmeyecektir.

Bu değişiklik, v2 metin çevirisi API 'SI için yazılmış bir uygulamanın çeşitli yönlerini etkileyecektir. Örnek olarak: diller API 'SI metin çevirisi, alfabede ve iki sözlük yöntemi için dil bilgilerini döndürür. Tüm yöntemler için tüm dil bilgilerini tek bir çağrıda isteyebilir veya tek tek talep edebilirsiniz.

Diller yöntemi kimlik doğrulaması gerektirmez; Aşağıdaki bağlantıya tıklayarak, JSON 'da v3 için tüm dil bilgilerini görebilirsiniz:

[https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation, sözlük, alfabede](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration)

## <a name="authentication-key"></a>Kimlik doğrulama anahtarı

V2 için kullandığınız kimlik doğrulama anahtarı v3 için kabul edilecektir. Yeni bir abonelik almanız gerekmez. Yıluzun geçiş süresi boyunca uygulamalarınızda v2 ve v3 'i karıştıracaksınız, ancak hala v2-XML ' den v3-JSON ' a geçiş yaparken yeni sürümler yayınlanmasını kolaylaştırabilirsiniz.

## <a name="pricing-model"></a>Fiyatlandırma modeli

Microsoft Translator v3, v2 fiyatlandırılmıştı. boşluk da dahil olmak üzere karakter başına. V3 sürümündeki yeni özellikler, faturalandırma için hangi karakterlerin sayıldığını bazı değişiklikler yapar.

| V3 yöntemi   | Faturalandırma için sayılan karakterler |
|:----------- |:-------------|
| `Languages`     | Hiçbir karakter gönderilmedi, atanmadı, hiçbir ücret alınmaz.          |
| `Translate`     | Sayı, çeviri için kaç karakter gönderildiğini ve karakterlerin kaç dilde çevrileceğini temel alır. 50 karakter gönderildi ve 5 dil istendi.           |
| `Transliterate`     | Alfabede gönderilen karakterlerin sayısı sayılır.         |
| `Dictionary lookup & example`     | Sözlük araması ve örnekleri için gönderilen karakter sayısı sayılır.         |
| `BreakSentence`     | Ücret alınmaz.       |
| `Detect`     | Ücret alınmaz.      |

## <a name="v3-end-points"></a>V3 uç noktaları

Genel

* api.cognitive.microsofttranslator.com

## <a name="v3-api-text-translations-methods"></a>V3 API metin çevirisi yöntemleri

[`Languages`](reference/v3-0-languages.md)

[`Translate`](reference/v3-0-translate.md)

[`Transliterate`](reference/v3-0-transliterate.md)

[`BreakSentence`](reference/v3-0-break-sentence.md)

[`Detect`](reference/v3-0-detect.md)

[`Dictionary/lookup`](reference/v3-0-dictionary-lookup.md)

[`Dictionary/example`](reference/v3-0-dictionary-examples.md)

## <a name="compatibility-and-customization"></a>Uyumluluk ve özelleştirme

> [!NOTE]
> 
> Microsoft Translator hub 'ı 17 Mayıs 2019 tarihinde kullanımdan kaldırılacaktır. [Önemli geçiş bilgilerini ve tarihlerini görüntüleyin](https://www.microsoft.com/translator/business/hub/).   

Microsoft Translator v3 varsayılan olarak sinir makine çevirisini kullanır. Bu nedenle, Microsoft Translator hub 'ı ile kullanılamaz. Translator hub 'ı yalnızca eski istatistiksel makine çevirisini destekler. Sinir çevirisi özelleştirmesi artık özel çevirici kullanılarak kullanılabilir. [Sinir makine çevirisini özelleştirme hakkında daha fazla bilgi edinin](custom-translator/overview.md)

V3 metin API 'SI ile sinir çevirisi, standart kategorilerin (SMT, konuşma, teknik, generalnn) kullanımını desteklemez.

| Sürüm | Uç Nokta | GDPR Işlemci uyumluluğu | Çevirmen hub 'ı kullan | Özel çevirici kullan (Önizleme) |
| :------ | :------- | :------------------------ | :----------------- | :------------------------------ |
|Translator sürüm 2|    api.microsofttranslator.com|    Hayır    |Yes    |Hayır|
|Translator sürümü 3|    api.cognitive.microsofttranslator.com|    Yes|    Hayır|    Yes|

**Translator sürümü 3**
* Genel olarak kullanılabilir ve tamamen desteklenir.
* , GDPR uyumlu bir işlemci olarak uyumludur ve tüm ISO 20001 ve 20018 yanı sıra SOC 3 sertifika gereksinimlerini karşılar. 
* Yeni Translator NMT özelleştirme özelliği olan özel çevirmenle (Önizleme) özelleştirdiğiniz sinir ağ çevirisi sistemlerini çağırabilmeniz için izin verir. 
* , Microsoft Translator hub 'ı kullanılarak oluşturulan özel çeviri sistemlerine erişim sağlamaz.

Api.cognitive.microsofttranslator.com uç noktasını kullanıyorsanız, çeviricisinin sürüm 3 ' ü kullanıyorsunuz.

**Translator sürüm 2**
* Tüm ISO 20.001, 20018 ve SOC 3 sertifika gereksinimlerini karşılamaz. 
* , Çevirmen özelleştirme özelliğiyle özelleştirdiğiniz sinir ağ çevirisi sistemlerini çağırmada izin vermez.
* Microsoft Translator hub 'ı kullanılarak oluşturulan özel çeviri sistemlerine erişim sağlar.
* Api.microsofttranslator.com uç noktasını kullanıyorsanız, çeviricisinin 2. sürümünü kullanıyorsunuz.

Çevirmen 'in hiçbir sürümü çevirilerinizi bir kayıt oluşturur. Çevirlarınız hiçbir şekilde hiç kimseyle paylaşılmaz. [Çevirmen No-Trace](https://www.aka.ms/NoTrace) Web sayfası hakkında daha fazla bilgi.

## <a name="links"></a>Bağlantılar

* [Microsoft Gizlilik İlkesi](https://privacy.microsoft.com/privacystatement)
* [Yasal bilgileri Microsoft Azure](https://azure.microsoft.com/support/legal)
* [Çevrimiçi Hizmet Şartları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [V 3.0 belgelerini görüntüle](reference/v3-0-reference.md)
