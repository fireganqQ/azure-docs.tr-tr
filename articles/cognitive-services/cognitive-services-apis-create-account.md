---
title: Azure portal bilişsel hizmetler kaynağı oluşturma
titleSuffix: Azure Cognitive Services
description: Azure portal bir kaynağı oluşturup abone olarak Azure bilişsel hizmetler 'i kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
keywords: bilişsel hizmetler, bilişsel zeka, bilişsel çözümler, AI Hizmetleri
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 09/14/2020
ms.author: aahi
ms.openlocfilehash: 3fd1d9e708a5f1a500440f20c2947ddfe4bc7460
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2020
ms.locfileid: "94368925"
---
# <a name="quickstart-create-a-cognitive-services-resource-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak bilişsel hizmetler kaynağı oluşturma

Azure bilişsel hizmetler 'i kullanmaya başlamak için bu hızlı başlangıcı kullanın. Azure portal bilişsel hizmet kaynağı oluşturduktan sonra, uygulamalarınızın kimliğini doğrulamak için bir uç nokta ve anahtar alırsınız.

Azure bilişsel hizmetler, REST API 'lerle bulut tabanlı hizmetlerdir ve geliştiricilerin doğrudan yapay zeka (AI) veya veri bilimi becerileri veya bilgi sahibi olmadan uygulamalara bilişsel zeka bilgileri oluşturmalarına yardımcı olacak istemci kitaplığı SDK 'Ları. Azure bilişsel hizmetler, geliştiricilerin kendi uygulamalarına kolayca bilişsel özellikler eklemesine, duymasına, konuşmasına, anlamasına ve hatta başlayabilmesine olanak tanıyan bilişsel çözümler sunar.


[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="prerequisites"></a>Önkoşullar

* Geçerli bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/).

## <a name="create-a-new-azure-cognitive-services-resource"></a>Yeni bir Azure bilişsel hizmetler kaynağı oluşturma

1. Kaynak oluşturma.

    #### <a name="multi-service-resource"></a>[Çoklu hizmet kaynağı](#tab/multiservice)

    Çoklu hizmet kaynağı, portalda bilişsel **Hizmetler** olarak adlandırılır. Bilişsel [Hizmetler kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne).

    Şu anda, çok hizmet kaynağı aşağıdaki bilişsel hizmetlere erişim sağlar:

    - Görüntü İşleme
    - Content Moderator
    - Yüz
    - Language Understanding (LUIS)
    - Metin Analizi
    - Çevirmen
    - Bing Arama v7 <br>(Web, görüntü, Haberler, video, görsel)
    - Bing Özel Arama
    - Bing Varlık Arama
    - Bing Otomatik Öneri
    - Bing Yazım Denetimi

    #### <a name="single-service-resource"></a>[Tek hizmet kaynağı](#tab/singleservice)

    Kullanılabilir bilişsel hizmetler için bir kaynak oluşturmak üzere aşağıdaki bağlantıları kullanın:

    | Görsel                      | Konuşma                  | Dil                          | Karar             | Arama                 |
    |-----------------------------|-------------------------|-----------------------------------|----------------------|------------------------|
    | [Bilgisayar vizyonu](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision)         | [Konuşma Hizmetleri](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices)     | [Modern okuyucu](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesImmersiveReader)              | [Anomali Algılayıcısı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) | [Bing Arama API v7](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7) |
    | [Özel Vision hizmeti](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesCustomVision) | [Konuşmacı Tanıma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeakerRecognition) | [Language Understanding (LUIS)](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) | [Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) | [Bing Özel Arama](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingCustomSearch) |
    | [Yüz Tanıma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace)                    |                         | [Soru-Cevap Oluşturma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker)                     | [Kişiselleştirme](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)     | [Bing Varlık Arama](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingEntitySearch) |
    | [Mürekkep Tanıma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesInkRecognizer)        |                         | [Metin Analizi](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)                |  [Ölçüm Danışmanı](https://go.microsoft.com/fwlink/?linkid=2142156)                    | [Bing Yazım Denetimi](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingSpellCheck-v7)   |
    |           |                         | [Çevirmen](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation)               |                      | [Bing Otomatik Öneri](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesBingAutosuggest-v7)                       |

    **_

3. _ *Oluştur* * sayfasında, aşağıdaki bilgileri sağlayın:

    #### <a name="multi-service-resource"></a>[Çoklu hizmet kaynağı](#tab/multiservice)

    |    |    |
    |--|--|
    | **Ad** | Bilişsel hizmetler kaynağınız için açıklayıcı bir ad. Örneğin, *Mybiliveservicesresource*. |
    | **Abonelik** | Kullanılabilir Azure aboneliklerinizden birini seçin. |
    | **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme gösterebilir, ancak kaynağınızın çalışma zamanı kullanılabilirliğini etkilemez. |
    | **Fiyatlandırma katmanı** | Bilişsel hizmetler hesabınızın maliyeti, seçtiğiniz seçeneklere ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Kaynak grubu** | Bilişsel hizmetler kaynağını içerecek Azure Kaynak grubu. Yeni bir grup oluşturabilir veya önceden var olan bir gruba ekleyebilirsiniz. |

    ![Çoklu hizmet kaynak kaynağı oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen-multi.png)

    **Oluştur** 'a tıklayın.

    #### <a name="single-service-resource"></a>[Tek hizmet kaynağı](#tab/singleservice)

    |    |    |
    |--|--|
    | **Ad** | Bilişsel hizmetler kaynağınız için açıklayıcı bir ad. Örneğin, *TextAnalyticsResource*. |
    | **Abonelik** | Kullanılabilir Azure aboneliklerinizden birini seçin. |
    | **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme gösterebilir, ancak kaynağınızın çalışma zamanı kullanılabilirliğini etkilemez. |
    | **Fiyatlandırma katmanı** | Bilişsel hizmetler hesabınızın maliyeti, seçtiğiniz seçeneklere ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Kaynak grubu** | Bilişsel hizmetler kaynağını içerecek Azure Kaynak grubu. Yeni bir grup oluşturabilir veya önceden var olan bir gruba ekleyebilirsiniz. |

    ![Tek hizmet kaynak oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen.png)

    **Oluştur** 'a tıklayın.

    **_

[!INCLUDE [Register Azure resource for subscription](./includes/register-resource-subscription.md)]

## <a name="get-the-keys-for-your-resource"></a>Kaynağınızın anahtarlarını alın

1. Kaynağınız başarıyla dağıtıldıktan sonra, **sonraki adımlar** altında _ *Kaynağa Git * öğesine* tıklayın.

    ![Bilişsel hizmetler için arama](media/cognitive-services-apis-create-account/resource-next-steps.png)

2. Açılan hızlı başlangıç bölmesinden, anahtarınız ve uç noktanıza erişebilirsiniz.

    ![Anahtar ve uç nokta al](media/cognitive-services-apis-create-account/get-cog-serv-keys.png)

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, grupta yer alan diğer tüm kaynakları da siler.

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Kaynak Grupları** 'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynağı içeren kaynak grubunu bulun
3. Kaynak grubu listesine sağ tıklayın. **Kaynak grubunu sil** 'i seçip onaylayın.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure bilişsel hizmetler 'e yönelik isteklerin kimliğini doğrulama](authentication.md)
* [Azure bilişsel hizmetler nedir?](./what-are-cognitive-services.md)
* [Azure Yönetim istemci kitaplığını kullanarak yeni kaynak oluşturma](.\cognitive-services-apis-create-account-client-library.md)
* [Doğal dil desteği](language-support.md)
* [Docker kapsayıcı desteği](cognitive-services-container-support.md)