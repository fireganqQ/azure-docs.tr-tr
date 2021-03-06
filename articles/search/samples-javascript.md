---
title: JavaScript örnekleri
titleSuffix: Azure Cognitive Search
description: JavaScript için Azure .NET SDK 'sını kullanan Azure Bilişsel Arama demo JavaScript kod örneklerini bulun.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/27/2021
ms.openlocfilehash: f83767813ea3923d85db2ca3f0164776c610525e
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98955029"
---
# <a name="javascript-code-samples-for-azure-cognitive-search"></a>Azure Bilişsel Arama için JavaScript kod örnekleri

Azure Bilişsel Arama çözümünün işlevselliğini ve iş akışını gösteren JavaScript kod örnekleri hakkında bilgi edinin. Bu örnekler, aşağıdaki bağlantılardan keşfedebileceğiniz [**JavaScript Için Azure SDK 'sı**](/azure/developer/javascript/)için [**Azure bilişsel arama istemci kitaplığı**](/javascript/api/overview/azure/search-documents-readme) ' nı kullanır.

| Hedef | Bağlantı |
|--------|------|
| Paket indirme | [www.npmjs.com/package/@azure/search-documents](https://www.npmjs.com/package/@azure/search-documents) |
| API başvurusu | [@azure/search-documents](/javascript/api/@azure/search-documents/)  |
| API test çalışmaları | [github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/test](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/test) |
| Kaynak kod | [github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents)  |

## <a name="sdk-samples"></a>SDK örnekleri

Azure SDK geliştirme ekibinin kod örnekleri API kullanımını gösterir. Bu örnekleri, [**Azure-SDK-for-js/Tree/Master/SDK/arama/arama-belgeler/örnekler**](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples) ' de GitHub 'da bulabilirsiniz.

### <a name="javascript-sdk-samples"></a>JavaScript SDK örnekleri

| Örnekler | Description |
|---------|-------------|
| [dizinlerde](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/indexes) | [Arama dizinlerini](search-what-is-an-index.md)oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. Bu örnek kategori Ayrıca bir hizmet istatistiği örneği içerir. |
| [dataSourceConnections (Dizin oluşturucular için)](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/dataSourceConnections) | [Desteklenen Azure veri kaynaklarının](search-indexer-overview.md#supported-data-sources)Dizin Oluşturucu tabanlı dizin oluşturma için gerekli olan Dizin Oluşturucu veri kaynaklarını oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Dizinleyiciler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/indexers) |  [Dizin oluşturucular](search-indexer-overview.md)oluşturma, güncelleştirme, alma, listeleme, sıfırlama ve silme işlemlerinin nasıl yapılacağını gösterir.|
| [Beceri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/skillSets) |   Bağlı Dizin oluşturucular olan ve dizin oluşturma sırasında AI tabanlı zenginleştirme gerçekleştiren [becerileri](cognitive-search-working-with-skillsets.md) oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Eş anlamlı eşlemeler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/synonymMaps) | [Eş anlamlı haritalar](search-synonyms.md)oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir.  |
| [Sorgular](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/search/search-documents/samples/javascript/src/readonlyQuery.js) | Microsoft tarafından barındırılan salt okunurdur bir ortak dizine karşı sorgu yürütmeyi gösterir.  |

### <a name="typescript-samples"></a>TypeScript örnekleri

| Örnekler | Description |
|---------|-------------|
| [dizinlerde](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/indexes) | [Arama dizinlerini](search-what-is-an-index.md)oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. Bu örnek kategori Ayrıca bir hizmet istatistiği örneği içerir. |
| [dataSourceConnections (Dizin oluşturucular için)](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/dataSourceConnections) | [Desteklenen Azure veri kaynaklarının](search-indexer-overview.md#supported-data-sources)Dizin Oluşturucu tabanlı dizin oluşturma için gerekli olan Dizin Oluşturucu veri kaynaklarını oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Dizinleyiciler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/indexers) |  [Dizin oluşturucular](search-indexer-overview.md)oluşturma, güncelleştirme, alma, listeleme, sıfırlama ve silme işlemlerinin nasıl yapılacağını gösterir.|
| [Beceri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/skillSets) |   Bağlı Dizin oluşturucular olan ve dizin oluşturma sırasında AI tabanlı zenginleştirme gerçekleştiren [becerileri](cognitive-search-working-with-skillsets.md) oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Eş anlamlı eşlemeler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/synonymMaps) | [Eş anlamlı haritalar](search-synonyms.md)oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir.  |
| [Sorgular](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/search/search-documents/samples/typescript/src/readonlyQuery.ts) | Microsoft tarafından barındırılan salt okunurdur bir ortak dizine karşı sorgu yürütmeyi gösterir.  |

## <a name="doc-samples"></a>Belge örnekleri

Bilişsel Arama ekibinin kod örnekleri, özellikleri ve iş akışlarını gösterir. Bu örneklerin çoğuna öğreticiler, hızlı başlangıçler ve nasıl yapılır makalelerinde başvurulur. Bu örnekleri GitHub 'daki [**Azure-Samples/Azure-Search-JavaScript-Samples**](https://github.com/Azure-Samples/azure-search-javascript-samples) ' de bulabilirsiniz.

| Örnekler | Makale |
|---------|---------|
| [hızlı başlangıç](https://github.com/Azure-Samples/azure-search-javascript-samples/tree/master/quickstart/v11) | Hızlı başlangıç için kaynak kodu [: JavaScript 'te bir arama dizini oluşturun](search-get-started-javascript.md). Bu makale, örnek verileri kullanarak bir arama dizini oluşturma, yükleme ve sorgulama için temel iş akışını ele almaktadır. |

> [!Tip]
> GitHub 'da ürün, hizmet ve dile göre filtrelenmiş Microsoft kod örneklerini aramak için [örnekler tarayıcısını](/samples/browse/?languages=javascript&products=azure-cognitive-search) deneyin.

## <a name="other-samples"></a>Diğer örnekler

Aşağıdaki örnekler de Bilişsel Arama ekibi tarafından yayımlanır, Ancak belgelerde başvurulmaz. İlişkili Benioku dosyaları Kullanım yönergeleri sağlar.

| Örnekler | Description |
|---------|-------------|
| [Azure-Arama-tepki-şablon](https://github.com/dereklegenzoff/azure-search-react-template) | Azure Bilişsel Arama için tepki şablonu (github.com) |