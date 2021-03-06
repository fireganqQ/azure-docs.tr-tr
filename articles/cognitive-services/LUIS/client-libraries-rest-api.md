---
title: 'Hızlı başlangıç: Language Understanding (LUSıS) SDK istemci kitaplıkları ve REST API'
description: LUSıS SDK istemci kitaplıkları ve REST API ile bir LUO uygulaması oluşturun ve sorgulayın.
ms.topic: quickstart
ms.date: 12/09/2020
ms.service: cognitive-services
ms.author: aahi
manager: nitinme
ms.subservice: language-understanding
author: aahill
keywords: Azure, yapay zeka, AI, doğal dil işleme, NLP, LUO, Azure lusıs, doğal dil anlama, AI sohbet botu, sohbet botu Maker, doğal dili anlama
ms.custom: devx-track-python, devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: programming-languages-set-luis
ms.openlocfilehash: 7ff5844cf9f1bce45df438fc00e24da28c438b05
ms.sourcegitcommit: dea56e0dd919ad4250dde03c11d5406530c21c28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96939225"
---
# <a name="quickstart-language-understanding-luis-client-libraries-and-rest-api"></a>Hızlı başlangıç: Language Understanding (LUSıS) istemci kitaplıkları ve REST API

Bu hızlı başlangıçta C#, Python veya JavaScript kullanarak bir Azure LUSıS yapay zeka (AI) uygulaması oluşturup bu hızlı başlangıç ile sorgulayın. Ayrıca, REST API kullanarak istek göndermek için kıvrımlı da kullanabilirsiniz.

Language Understanding (LUU), bir kullanıcının konuşma için doğal dil işleme (NLP), genel anlamı tahmin etmek için doğal dil metnine uygulamanıza ve ilgili, ayrıntılı bilgiler almanıza olanak sağlar.

* **Yazma** istemci kitaplığı ve REST API, lusıs uygulamanızı oluşturmanıza, düzenlemenize, eğmenize ve yayımlamanıza olanak sağlar.
* **Tahmin çalışma zamanı** istemci kitaplığı ve REST API yayımlanan uygulamayı sorgulamanızı sağlar.

::: zone pivot="programming-language-csharp"
[!INCLUDE [LUIS development with C# SDK](./includes/sdk-csharp.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [LUIS development with Node.js SDK](./includes/sdk-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [LUIS development with Python SDK](./includes/sdk-python.md)]
::: zone-end

::: zone pivot="rest-api"
[!INCLUDE [LUIS development with REST API](./includes/rest-api.md)]
::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulamayı [Luo portalından](https://www.luis.ai) silebilir ve Azure kaynaklarını [Azure Portal](https://portal.azure.com/)silebilirsiniz.

REST API kullanıyorsanız, `ExampleUtterances.JSON` hızlı başlangıç ile işiniz bittiğinde dosyayı dosya sisteminden silin.

## <a name="troubleshooting"></a>Sorun giderme

* İstemci kitaplığı kimlik doğrulaması hataları, genellikle yanlış anahtar & uç noktasının kullanıldığını gösterir. Bu hızlı başlangıç, tahmin çalışma zamanı için yazma anahtarını ve uç noktayı bir kolaylık olarak kullanır, ancak yalnızca aylık kotayı henüz kullanmadıysanız çalışır. Yazma anahtarını ve uç noktayı kullanamıyoruz, tahmin çalışma zamanı SDK 'Sı istemci kitaplığına erişirken tahmin çalışma zamanı anahtarını ve uç noktasını kullanmanız gerekir.
* Varlık oluşturma-Bu öğreticide kullanılan iç içe makine öğrenme varlığını oluştururken bir hata alırsanız, kodu kopyaladığınızdan ve farklı bir varlık oluşturmak için kodu değiştirmediğinizden emin olun.
* Örnek oluşturma-Bu öğreticide kullanılan etiketli örneği oluştururken bir hata alırsanız, kodu kopyaladığınızdan ve farklı etiketlenmiş bir örnek oluşturmak için kodu değiştirmediğinizden emin olun.
* Eğitim-bir eğitim hatası alırsanız, bu genellikle boş bir uygulamayı (örnek bir amaç olmadan) veya yanlış biçimlendirilmiş amaçlar veya varlıklar içeren bir uygulamayla gösterir.
* Çeşitli hatalar-kod, istemci kitaplıklarına metin ve JSON nesneleriyle çağrı yaptığından kodu değiştirmemiş olduğunuzdan emin olun.

Diğer hatalar: Yukarıdaki listede kapsanmayan bir hata alırsanız, bu sayfanın en altında geri bildirim vererek bize bilgi verin. Yüklediğiniz istemci kitaplıklarının programlama dilini ve sürümünü ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Language Understanding (LUSıS) API nedir?](what-is-luis.md)
* [Yenilikler nelerdir?](whats-new.md)
* [Amaçlar](luis-concept-intent.md), [varlıklar](luis-concept-entity-types.md)ve [örnek](luis-concept-utterance.md)bildirimler ve [önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code)' da bulunabilir.
* Doğal dili anlama: [doğal dil anlama (NLU) ve doğal dil işleme (NLP)](artificial-intelligence.md)
* Botlar: [AI chatbots](luis-csharp-tutorial-bf-v4.md "sohbet botu Oluşturucusu öğreticisi")
