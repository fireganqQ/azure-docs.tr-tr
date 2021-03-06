---
title: Azure portal ile Mantıksal Uygulamayı bir API olarak içeri aktarma | Microsoft Docs
description: Bu makalede, mantıksal uygulamayı bir API olarak içeri aktarmak için API Management (APıM) kullanımı gösterilmektedir.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/22/2020
ms.author: apimpm
ms.openlocfilehash: 0dd460bce01b86c72d0b8fd70351fdcc85c69bd4
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2020
ms.locfileid: "93146969"
---
# <a name="import-a-logic-app-as-an-api"></a>Mantıksal Uygulamayı API olarak içeri aktarma

Bu makalede, bir mantıksal uygulamanın API olarak nasıl içeri aktarılacağı ve içeri aktarılan API 'nin nasıl test yapılacağı gösterilir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> -   Mantıksal Uygulamayı API olarak içeri aktarma
> -   Azure portalında API’yi test etme
> -   Geliştirici portalında API’yi test etme

## <a name="prerequisites"></a>Önkoşullar

-   Şu hızlı başlangıcı doldurun: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
-   Aboneliğinizde bir HTTP uç noktası sunan bir mantıksal uygulama olduğundan emin olun. Daha fazla bilgi için, [http uç noktaları ile iş akışlarını tetikleme](../logic-apps/logic-apps-http-endpoint.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-back-end-api"></a><a name="create-api"> </a>Arka uç API'sini içeri aktarma ve yayımlama

1. Azure portal API Management hizmetinize gidin ve menüden **API 'leri** seçin.
2. **Yeni API ekleyin** listesinden **Mantıksal Uygulama** ’yı seçin.

    ![Mantıksal uygulama](./media/import-logic-app-as-api/logic-app-api.png)

3. Aboneliğinizde HTTP tetikleyicisiyle Logic Apps listesini görmek için, **Araştır** ' a basın. (HTTP tetikleyicisi olmayan Logic Apps listede görünmeyecektir.)
4. Uygulamayı seçin. API Management seçilen uygulamayla ilişkili Swagger 'yi bulur, getirir ve içeri aktarır.
5. API URL'si soneki ekleyin. Sonek, bu belirli API’yi bu API Management örneğinde tanımlayan bir addır. Bu API Management örneğinde benzersiz olmalıdır.
6. API’yi bir ürünle ilişkilendirerek yayımlayın. Bu durumda, " _sınırsız_ " ürün kullanılır. API’nin yayımlanmasını ve geliştiricilerin kullanımına sunulmasını istiyorsanız API’yi bir ürüne ekleyin. Bunu API oluşturması sırasında yapabilir ya da daha sonra ayarlayabilirsiniz.

    Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir dizi API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Geliştiricilerin bir API’ye erişebilmesi için önce ürüne abone olması gerekir. Abone olduklarında, ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinirler. API Management örneğini oluşturduysanız, zaten bir yöneticiyseniz, varsayılan olarak her ürüne abone olursunuz.

    Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir:

    - **Başlangıç**
    - **Sınırsız**

7. Diğer API ayarlarını girin. Oluşturma sırasında değerleri ayarlayabilir veya daha sonra **Ayarlar** sekmesine giderek yapılandırabilirsiniz. Ayarlar, [Ilk API 'Nizi Içeri aktarma ve yayımlama](import-and-publish.md#import-and-publish-a-backend-api) öğreticisinde açıklanmıştır.
8. **Oluştur** ’u seçin.

## <a name="test-the-api-in-the-azure-portal"></a>Azure portalında API’yi test etme

İşlemler doğrudan bir API’nin işlemlerini görüntülemek ve test etmek için kullanışlı bir yol sağlayan Azure portalından çağrılabilir.

1. Önceki adımda oluşturduğunuz API’yi seçin.
2. **Test** sekmesine basın.
3. Bir işlem seçin.

    Sayfa, sorgu parametrelerinin ve üst bilgilerin alanlarını görüntüler. Bu API ile ilişkilendirilmiş ürünün abonelik anahtarı için, üst bilgilerden biri "Ocp-Apim-Subscription-Key" üst bilgisidir. API Management örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve anahtar otomatik olarak doldurulur.

4. **Gönder** ’e basın.

    Arka uç, **200 OK** ve bazı verilerle yanıt verir.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

>[!NOTE]
>Her Mantıksal Uygulama, **el ile çağırma** işlemine sahiptir. API’nizin birden çok mantıksal uygulamadan oluşmasını istiyorsanız, çakışma olmaması için işlevi yeniden adlandırmanız gerekir.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>
> [Yayınlanmış bir API 'yi dönüştürme ve koruma](transform-api.md)
