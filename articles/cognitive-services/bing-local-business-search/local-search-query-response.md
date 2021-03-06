---
title: API sorguları ve yanıtları gönderme ve kullanma-Bing yerel Iş arama
titleSuffix: Azure Cognitive Services
description: Bing yerel Iş Arama API 'siyle arama sorguları gönderme ve kullanma hakkında bilgi edinmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: rosh
ms.openlocfilehash: 70a33774ac82312660d887fb86f7e2a482c30a0c
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96487176"
---
# <a name="sending-and-using-bing-local-business-search-api-queries-and-responses"></a>Bing yerel Iş Arama API 'SI sorguları ve yanıtları gönderme ve kullanma

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Yerel sonuçları, uç noktasına bir arama sorgusu göndererek ve gerekli olan üstbilgiyi de ekleyerek, Bing yerel Iş Arama API 'sinden elde edebilirsiniz `Ocp-Apim-Subscription-Key` . Kullanılabilir [üstbilgiler](local-search-reference.md#headers) ve [parametrelerle](local-search-reference.md#query-parameters)birlikte, aramalar, aranacak alanın [coğrafi sınırları](specify-geographic-search.md) belirtilerek ve döndürülen konum [kategorileri](local-search-query-response.md) belirtilerek özelleştirilebilir.

## <a name="creating-a-request"></a>İstek oluşturma

Bing yerel Iş Arama API 'sine bir istek göndermek için, `q=` API uç noktasına eklemeden önce parametreye bir arama terimi ekleyin ve `Ocp-Apim-Subscription-Key` üst bilgi dahil edin. Örnek:

`https://api.cognitive.microsoft.com/bing/localbusinesses/v7.0/search?q=restaurant+in+Bellevue`

Tam istek URL 'SI sözdizimi aşağıda gösterilmiştir. İstek gönderme hakkında daha fazla bilgi için bkz. Bing yerel Iş Arama API 'SI [hızlı başlangıç](quickstarts/local-quickstart.md)bilgileri ve [üstbilgiler](local-search-reference.md#headers) ve [Parametreler](local-search-reference.md#query-parameters) için başvuru içeriği. 

Yerel Arama kategorileri hakkında daha fazla bilgi için bkz. [Bing yerel Iş Arama API 'si Için Arama kategorileri](local-categories.md).

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search[?q][&localCategories][&cc][&mkt][&safesearch][&setlang][&count][&first][&localCircularView][&localMapView]
```

## <a name="using-responses"></a>Yanıtları kullanma

Bing yerel Iş Arama API 'sindeki JSON yanıtları bir nesne içerir `SearchResponse` . API, alana ilgili arama sonuçlarını döndürür `places` . hiçbir sonuç bulunamazsa, `places` Bu alan yanıta eklenmez.

[!INCLUDE [cognitive-services-bing-url-note](../../../includes/cognitive-services-bing-url-note.md)]

```
{
   "_type": "SearchResponse",
   "queryContext": {
      "originalQuery": "restaurant in Bellevue"
   },
   "places": {
      "totalEstimatedMatches": 10,
. . . 
```

### <a name="search-result-attributes"></a>Arama sonucu öznitelikleri

API tarafından döndürülen JSON sonuçları aşağıdaki öznitelikleri içerir:

* _type
* adres
* Entitypresentationınfo
* Co
* kimlik
* name
* routeablePoint
* Telefon
* url

Üstbilgiler, parametreler, Pazar kodları, yanıt nesneleri, hatalar vb. hakkında genel bilgi için bkz. [Bing yerel arama API 'si v7](local-search-reference.md) başvurusu.

> [!NOTE]
> Siz veya sizin adınıza üçüncü bir taraf, Microsoft dışı bir hizmet veya özelliği test etmek, geliştirmek, eğitmek, dağıtmak veya kullanıma açmak amacıyla yerel arama API 'sindeki herhangi bir veriyi kullanamaz, koruyabilir, saklayabilir, önbelleğe alabilir veya dağıtamazsınız. 


## <a name="example-json-response"></a>Örnek JSON yanıtı

Aşağıdaki JSON yanıtı sorgu tarafından belirtilen arama sonuçlarını içerir `?q=restaurant+in+Bellevue` .

```json
Vary: Accept-Encoding
BingAPIs-TraceId: 5376FFEB65294E24BB9F91AD70545826
BingAPIs-SessionId: 06ED7CEC80F746AA892EDAAC97CB0CB4
X-MSEdge-ClientID: 112C391E72C0624204153594738C63DE
X-MSAPI-UserState: aeab
BingAPIs-Market: en-US
X-Search-ResponseInfo: InternalResponseTime=659,MSDatacenter=CO4
X-MSEdge-Ref: Ref A: 5376FFEB65294E24BB9F91AD70545826 Ref B: BY3EDGE0306 Ref C: 2018-10-16T16:26:15Z
apim-request-id: fe54f585-7c54-4bf5-8b92-b9bede2b710a
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Cache-Control: max-age=0, private
Date: Tue, 16 Oct 2018 16:26:15 GMT
P3P: CP="NON UNI COM NAV STA LOC CURa DEVa PSAa PSDa OUR IND"
Content-Length: 978
Content-Type: application/json; charset=utf-8
Expires: Tue, 16 Oct 2018 16:25:15 GMT

{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "restaurant Bellevue"
  },
  "places": {
    "totalEstimatedMatches": 50,
    "value": [{
      "_type": "LocalBusiness",
      "id": "https:\/\/cognitivegblppe.azure-api.net\/api\/v7\/#Places.0",
      "name": "Facing East Taiwanese Restaurant",
      "url": "http:\/\/litadesign.wix.com\/facingeastrestaurant",
      "entityPresentationInfo": {
        "entityScenario": "ListItem",
        "entityTypeHints": ["Place", "LocalBusiness", "Restaurant"]
      },
      "geo": {
        "latitude": 47.6199188232422,
        "longitude": -122.202796936035
      },
      "routablePoint": {
        "latitude": 47.6199188232422,
        "longitude": -122.201713562012
      },
      "address": {
        "streetAddress": "1075 Bellevue Way NE Ste B2",
        "addressLocality": "Bellevue",
        "addressRegion": "WA",
        "postalCode": "98004",
        "addressCountry": "US",
        "neighborhood": "Bellevue",
        "text": "1075 Bellevue Way NE Ste B2, Bellevue, WA 98004"
      },
      "telephone": "(425) 688-2986"
    }],
    "searchAction": {
      "location": [{
        "name": "Bellevue, Washington"
      }],
      "query": "restaurant"
    }
  }
}
 
```

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]


## <a name="next-steps"></a>Sonraki adımlar
- [Yerel Iş araması hızlı başlangıç](quickstarts/local-quickstart.md)
- [Yerel Iş arama Java hızlı başlangıç](quickstarts/local-search-java-quickstart.md)
- [Yerel Iş arama düğümü hızlı başlangıç](quickstarts/local-search-node-quickstart.md)
- [Yerel Iş arama Python hızlı başlangıç](quickstarts/local-search-python-quickstart.md)