---
title: Özel yetenekler için arabirim tanımı
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de bir AI zenginleştirme ardışık düzeninde Web API özel yeteneği için özel veri ayıklama arabirimi.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/06/2020
ms.openlocfilehash: e78f0d1e8d6d637dfebe1ff475ab8416ba49a263
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88935424"
---
# <a name="how-to-add-a-custom-skill-to-an-azure-cognitive-search-enrichment-pipeline"></a>Azure Bilişsel Arama enzenginleştirme ardışık düzenine özel bir yetenek ekleme

> [!VIDEO https://www.youtube.com/embed/fHLCE-NZeb4?version=3&start=172&end=221]

Azure Bilişsel Arama 'deki bir [zenginleştirme ardışık düzeni](cognitive-search-concept-intro.md) , yerleşik bilişsel yeteneklerin yanı sıra, kişisel [olarak](cognitive-search-predefined-skills.md) oluşturup işlem hattına eklediğiniz [özel becerileri](cognitive-search-custom-skill-web-api.md) de derlenebilir. Bu makalede, bir, bir AI zenginleştirme işlem hattına dahil edilmesini sağlayan bir arabirimi kullanıma sunan özel bir yetenek oluşturmayı öğrenin. 

Özel bir beceri oluşturmak, içeriğinize benzersiz dönüştürmeler eklemek için bir yol sağlar. Özel bir beceri, gerek duyduğunuz zenginleştirme adımını uygulayarak bağımsız olarak yürütülür. Örneğin, alana özgü özel varlıklar tanımlayabilir, iş ve finansal sözleşmeleri ve belgeleri ayırt etmek için özel sınıflandırma modelleri oluşturabilir veya ilgili içerik için ses dosyalarına daha derin ulaşmak üzere bir konuşma tanıma yeteneği ekleyebilirsiniz. Adım adım bir örnek için bkz. [örnek: AI zenginleştirme için özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md).

 İhtiyaç duyduğunuz özel bir yetenek, özel bir beceriye, zenginleştirme işlem hattının geri kalanına bağlamak için basit ve açık bir arabirim vardır. Bir [beceri](cognitive-search-defining-skillset.md) eklemek için tek gereksinim, girişleri kabul etme ve çıkışları bir bütün olarak tüketim kapsamında tüketilen yollarla yayma olanağıdır. Bu makalenin odağı, enzenginleştirme ardışık düzeninin gerektirdiği giriş ve çıkış biçimlerinden oluşur.

## <a name="web-api-custom-skill-interface"></a>Web API özel yetenek arabirimi

30 saniyelik bir pencerede yanıt döndürmezseniz varsayılan zaman aşımı ile özel WebAPI beceri uç noktaları. Dizin oluşturma işlem hattı zaman uyumludur ve bu pencerede bir yanıt alınmadığında dizin oluşturma zaman aşımı hatası oluşturur.  Zaman aşımı parametresini ayarlayarak zaman aşımını 230 saniyeye kadar yapılandırmak mümkündür:

```json
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill has a 230 second timeout",
        "uri": "https://[your custom skill uri goes here]",
        "timeout": "PT230S",
```

URI 'nin güvenli (HTTPS) olduğundan emin olun.

Şu anda, özel bir beceriyle etkileşimde bulunmak için tek mekanizma bir Web API arabirimi aracılığıyla yapılır. Web API 'sinin bu bölümde açıklanan gereksinimleri karşılaması gerekir.

### <a name="1--web-api-input-format"></a>1. Web API giriş biçimi


> [!VIDEO https://www.youtube.com/embed/fHLCE-NZeb4?version=3&start=294&end=340]


Web API 'sinin işlenecek bir kayıt dizisini kabul etmesi gerekir. Her kayıt, Web API 'nize girilen giriş olan bir "özellik paketi" içermelidir. 

Bir sözleşmenin metninde bahsedilen ilk tarihi tanımlayan basit bir zenginlik oluşturmak istediğinizi varsayalım. Bu örnekte, yetenek sözleşme metni olarak tek bir giriş *Contracttext* 'i kabul eder. Bu beceri, sözleşmenin tarihi olan tek bir çıktıya de sahiptir. Daha zengin bir şekilde daha ilginç hale getirmek için, bu *Contractdate* öğesini çok parçalı bir karmaşık türün şekline döndürün.

Web API 'niz bir toplu giriş kaydı almaya hazırlanmalıdır. *Values* dizisinin her üyesi belirli bir kayıt için girişi temsil eder. Her kaydın aşağıdaki öğelere sahip olması gerekir:

+ Bir *recordID* üyesi, belirli bir kayıt için benzersiz tanıtıcıdır. Daha zengin bir sonuçları geri döndürürse, çağıranın kayıt sonuçlarıyla aynı şekilde eşleşmesini sağlamak için bu *recordID* sağlaması gerekir.

+ Her bir kayıt için temel bir giriş alanı paketi olan bir *veri* üyesi.

Yukarıdaki örnek başına daha somut olması için Web API 'nizin şuna benzer istekleri beklemesi gerekir:

```json
{
    "values": [
      {
        "recordId": "a1",
        "data":
           {
             "contractText": 
                "This is a contract that was issues on November 3, 2017 and that involves... "
           }
      },
      {
        "recordId": "b5",
        "data":
           {
             "contractText": 
                "In the City of Seattle, WA on February 5, 2018 there was a decision made..."
           }
      },
      {
        "recordId": "c3",
        "data":
           {
             "contractText": null
           }
      }
    ]
}
```
Gerçekte, hizmetiniz yalnızca burada gösterilen üçü yerine yüzlerce veya binlerce kayıt ile çağrılabilir.

### <a name="2-web-api-output-format"></a>2. Web API 'SI çıkış biçimi

Çıktının biçimi, bir *recordID*ve bir özellik paketi içeren bir kayıt kümesidir 

```json
{
  "values": 
  [
      {
        "recordId": "b5",
        "data" : 
        {
            "contractDate":  { "day" : 5, "month": 2, "year" : 2018 }
        }
      },
      {
        "recordId": "a1",
        "data" : {
            "contractDate": { "day" : 3, "month": 11, "year" : 2017 }                    
        }
      },
      {
        "recordId": "c3",
        "data" : 
        {
        },
        "errors": [ { "message": "contractText field required "}   ],  
        "warnings": [ {"message": "Date not found" }  ]
      }
    ]
}
```

Bu belirli örnek yalnızca bir çıkışa sahiptir, ancak birden fazla özelliğe de çıkış yapabilirsiniz. 

### <a name="errors-and-warning"></a>Hatalar ve uyarı

Önceki örnekte gösterildiği gibi, her bir kayıt için hata ve uyarı iletileri döndürebilirsiniz.

## <a name="consuming-custom-skills-from-skillset"></a>Beceri 'ten özel beceriler kullanma

Bir Web API 'SI oluşturduğunuzda, isteğin bir parçası olarak HTTP üst bilgilerini ve parametrelerini tanımlayabilirsiniz. Aşağıdaki kod parçacığında, istek parametrelerinin ve *isteğe bağlı* http üstbilgilerinin beceri tanımının bir parçası olarak nasıl açıklandığı gösterilmektedir. HTTP üstbilgileri bir gereksinim değildir, ancak becerinize ek yapılandırma özellikleri eklemenize ve bunları beceri tanımından ayarlamanıza imkan tanır.

```json
{
    "skills": [
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill calls an Azure function, which in turn calls TA sentiment",
        "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/DateExtractor?language=en",
        "context": "/document",
        "httpHeaders": {
            "DateExtractor-Api-Key": "foo"
        },
        "inputs": [
          {
            "name": "contractText",
            "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "contractDate",
            "targetName": "date"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel bir yeteneğin bir beceri ile tümleştirilmesi için gereken arabirim gereksinimleri ele alınmıştır. Özel yetenekler ve beceri kompozisyonu hakkında daha fazla bilgi edinmek için aşağıdaki bağlantılara tıklayın.

+ [Özel yetenekler hakkında videolarımızı izleyin](https://youtu.be/fHLCE-NZeb4)
+ [Güç becerileri: özel yeteneklerin bir deposu](https://github.com/Azure-Samples/azure-search-power-skills)
+ [Örnek: AI zenginleştirme için özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri oluşturma (REST)](/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanları eşleme](cognitive-search-output-field-mapping.md)