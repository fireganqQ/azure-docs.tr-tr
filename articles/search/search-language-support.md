---
title: Ingilizce olmayan arama sorguları için çok dilli dizin oluşturma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama, Microsoft 'un Lucene ve doğal dil Işleme teknolojisinden dil çözümleyicilerinden yararlanarak 56 dili destekler.
manager: nitinme
author: yahnoosh
ms.author: jlembicz
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/12/2020
ms.openlocfilehash: 588de9c9cae114b5f5396db17f7ecb19bcde25c6
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423088"
---
# <a name="how-to-create-an-index-for-multiple-languages-in-azure-cognitive-search"></a>Azure Bilişsel Arama birden çok dil için dizin oluşturma

Dizinler, birden fazla dilin içeriğini içeren alanları içerebilir, örneğin dile özgü dizeler için ayrı alanlar oluşturabilir. Dizin oluşturma ve sorgulama sırasında en iyi sonuçlar için, uygun dil kurallarını sağlayan bir dil Çözümleyicisi atayın. 

Azure Bilişsel Arama, her iki Lucene ve Microsoft 'tan gelen ve çözümleyici özelliği kullanılarak tek tek alanlara atanabilecek dil çözümleyicilerinin büyük bir seçimini sunmaktadır. Portalda, bu makalede açıklandığı gibi bir dil Çözümleyicisi de belirtebilirsiniz.

## <a name="add-analyzers-to-fields"></a>Alanlara çözümleyiciler ekleme

Bir alan oluşturulduğunda dil Çözümleyicisi belirtilir. Var olan bir alan tanımına çözümleyici eklemek için, dizini üzerine yazma (ve yeniden yükleme) ya da orijinalle özdeş yeni bir alan oluşturma (çözümleyici atamasıyla) gerekir. Daha sonra, kullanılmayan alanı dilediğiniz zaman silebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın ve arama hizmetinizi bulun.
1. Yeni bir dizin başlatmak için hizmet panosunun en üstündeki komut çubuğunda **Dizin Ekle** ' ye tıklayın veya var olan bir dizine eklemekte olduğunuz yeni alanlara bir çözümleyici ayarlamak için mevcut bir dizini açın.
1. Bir ad sağlayarak alan tanımı başlatın.
1. EDM. String veri türünü seçin. Yalnızca dize alanları tam metin aranabilir.
1. Çözümleyici özelliğini etkinleştirmek için **aranabilir** özniteliği ayarlayın. Bir dil Çözümleyicisi 'nin kullanılabilmesi için alanın metin tabanlı olması gerekir.
1. Kullanılabilir çözümleyiciler arasından birini seçin. 

![Alan tanımı sırasında dil Çözümleyicileri atama](media/search-language-support/select-analyzer.png "Alan tanımı sırasında dil Çözümleyicileri atama")

Varsayılan olarak, tüm aranabilir alanlar, dilden bağımsız olan [Standart Lucene çözümleyici](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html) 'yi kullanır. Desteklenen çözümleyiciler listesinin tam listesini görüntülemek için bkz. [Azure bilişsel arama dizinine dil Çözümleyicileri ekleme](index-add-language-analyzers.md).

Portalda, çözümleyiciler olduğu gibi kullanılmak üzere tasarlanmıştır. Özelleştirmeler veya filtre ve belirteçlerin belirli bir yapılandırmasına ihtiyacınız varsa, kodda [özel bir çözümleyici oluşturmanız](index-add-custom-analyzers.md) gerekir. Portal özel Çözümleyicileri seçmeyi veya yapılandırmayı desteklemez.

## <a name="query-language-specific-fields"></a>Dile özgü alanları sorgulama

Bir alan için dil Çözümleyicisi seçildikten sonra, bu alan için her bir dizin oluşturma ve arama isteğiyle birlikte kullanılacaktır. Farklı çözümleyiciler kullanılarak birden çok alana karşı bir sorgu verildiğinde, sorgu her bir alan için atanan çözümleyiciler tarafından bağımsız olarak işlenir.

Bir sorgu veren aracının dili biliniyorsa, bir arama isteği, **Searchfields** sorgu parametresi kullanılarak belirli bir alana kapsam yapılabilir. Aşağıdaki sorgu yalnızca Lehçe 'daki açıklamaya göre verilecek:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=PolishContent&api-version=2020-06-30`

Yukarıdaki gibi bir sorguya benzer bir sorgu yapıştırmak için [**Arama Gezgini**](search-explorer.md) 'ni kullanarak portalınızın dizinini sorgulayabilirsiniz.

## <a name="boost-language-specific-fields"></a>Dile özgü alanları arttırma

Bazen bir sorguyu veren aracının dili, bu durumda sorgunun tüm alanlara aynı anda verilebileceği anlamına gelir. Gerekirse, belirli bir dilin sonuçları için tercih, [Puanlama profilleri](index-add-scoring-profiles.md)kullanılarak tanımlanabilir. Aşağıdaki örnekte, Ingilizce 'deki açıklamada bulunan eşleşmeler, Lehçe ve Fransızca 'daki eşleştirmelere göre daha yüksek puanlanacaktır:

```http
    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]
```

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2020-06-30`

## <a name="next-steps"></a>Sonraki adımlar

.NET geliştiricisiyseniz, [Azure bilişsel arama .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Search) ve [LexicalAnalyzer](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzer) özelliğini kullanarak dil Çözümleyicileri yapılandırabileceğinizi unutmayın.