---
title: SYNAPSE SQL 'de T-SQL özelliği farklılıkları
description: SYNAPSE SQL 'de kullanılabilen Transact-SQL özelliklerinin listesi.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: f6ba22813139f5a4ad6c01073fe7f7b4b7d5346e
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98878164"
---
# <a name="transact-sql-features-supported-in-azure-synapse-sql"></a>Azure SYNAPSE SQL 'de desteklenen Transact-SQL özellikleri

Azure SYNAPSE SQL, T-SQL dilini kullanarak verilerinizi sorgulayabilir ve analiz etmenizi sağlayan büyük bir veri analizi hizmetidir. Veri analizi için SQL Server ve Azure SQL veritabanı 'nda kullanılan standart ANSI uyumlu SQL dili lehçini kullanabilirsiniz. 

Transact-SQL dili sunucusuz SQL havuzunda kullanılır ve ayrılmış model farklı nesnelere başvurabilir ve desteklenen özellikler kümesinde bazı farklılıklar vardır. Bu sayfada, SYNAPSE SQL 'in tüketim modelleri arasında üst düzey Transact-SQL dil farklarını bulabilirsiniz.

## <a name="database-objects"></a>Veritabanı nesneleri

SYNAPSE SQL 'deki tüketim modelleri farklı veritabanı nesneleri kullanmanıza olanak sağlar. Desteklenen nesne türlerinin karşılaştırılması aşağıdaki tabloda gösterilmiştir:

|   | Ayrılmış | Sunucusuz |
| --- | --- | --- |
| **Tablolar** | [Evet](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Hayır, sunucusuz model yalnızca [Azure Storage](#storage-options) 'a yerleştirilmiş dış verileri sorgulayabilir |
| **Görünümler** | [Evet](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Görünümler, adanmış modelde kullanılabilir olan [sorgu dili öğelerini](#query-language) kullanabilir. | [Evet](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Görünümler, sunucusuz modelde kullanılabilen [sorgu dili öğelerini](#query-language) kullanabilir. |
| **Şemalar** | [Evet](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | [Evet](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) |
| **Geçici tablolar** | [Evet](../sql-data-warehouse/sql-data-warehouse-tables-temporary.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | Hayır |
| **Yordamlar** | [Evet](/sql/t-sql/statements/create-procedure-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Yes |
| **İşlevler** | [Evet](/sql/t-sql/statements/create-function-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Evet, yalnızca satır içi tablo değerli işlevler. |
| **Tetikleyiciler** | Hayır | Hayır |
| **Dış tablolar** | [Evet](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Bkz. desteklenen [veri biçimleri](#data-formats). | [Evet](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Bkz. desteklenen [veri biçimleri](#data-formats). |
| **Sorguları önbelleğe alma** | Evet, birden çok form (SSD tabanlı önbelleğe alma, bellek içi, sonuç kümesi önbelleği). Ayrıca, gerçekleştirilmiş görünüm desteklenir | Hayır |
| **Tablo değişkenleri** | [Hayır](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), geçici tabloları kullan | Hayır |
| **[Tablo dağıtımı](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)**               | Yes | Hayır |
| **[Tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)**                           | Yes | Hayır |
| **[Tablo bölümleri](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)**                     | Yes | Hayır |
| **[İstatistikler](develop-tables-statistics.md)**            | Yes | Yes |
| **[İş yükü yönetimi, kaynak sınıfları ve eşzamanlılık denetimi](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)** | Yes    | Hayır |
| **Maliyet denetimi** | Evet, ölçek yukarı ve ölçek azaltma eylemlerini kullanma. | Evet, [Azure Portal veya T-SQL yordamını](./data-processed.md#cost-control)kullanarak. |

## <a name="query-language"></a>Sorgu dili

SYNAPSE SQL 'de kullanılan sorgu dilleri, tüketim modeline bağlı olarak desteklenen farklı özelliklere sahip olabilir. Aşağıdaki tabloda Transact-SQL dialarcts 'deki en önemli sorgu dili farklılıkları özetlenmektedir:

|   | Ayrılmış | Sunucusuz |
| --- | --- | --- |
| **SELECT deyimleri** | Evet. [XML/JSON için](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)Transact-SQL sorgu yan tümceleri ve [eşleştirme](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) desteklenmez. | Evet. XML, [Match](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [tahmin](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)ve sorgu Ipuçları [için](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)Transact-SQL sorgu yan tümceleri desteklenmez. |
| **INSERT deyimleri** | Yes | Hayır |
| **UPDATE ekstresi** | Yes | Hayır |
| **DELETE deyimleri** | Yes | Hayır |
| **MERGE ekstresi** | Evet ([Önizleme](/sql/t-sql/statements/merge-transact-sql?view=azure-sqldw-latest&preserve-view=true)) | Hayır |
| **[İşlemler](develop-transactions.md)** | Yes | Evet, meta veri nesnelerinde uygulanabilir. |
| **[Etiketler](develop-label.md)** | Yes | Hayır |
| **Veri yükleme** | Evet. Tercih edilen yardımcı program [Copy](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) deyimidir, ancak sistem, veri yüklemesi IÇIN hem toplu yükleme (bcp) hem de [Cetas](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) 'yi destekler. | Hayır |
| **Veri dışarı aktarma** | Evet. [Cetas](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)kullanma. | Evet. [Cetas](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)kullanma. |
| **Türler** | Evet, [imleç](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [HierarchyId](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [ntext, Text ve Image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [uzamsal türler](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [SQL \_ Variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)ve [XML](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) dışındaki tüm Transact-SQL türleri | Evet, [imleç](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [HierarchyId](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [ntext, Text ve Image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [uzamsal türler](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [SQL \_ Variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [XML](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)ve tablo türü dışındaki tüm Transact-SQL türleri |
| **Veritabanları arası sorgular** | Hayır | Yes [, using ifadesiyle](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) birlikte. |
| **Yerleşik işlevler (Analiz)** | Evet, tüm Transact-SQL [analitik](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), dönüştürme, [Tarih ve saat](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), mantıksal, [matematik](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) Işlevleri, [seçme](/sql/t-sql/functions/logical-functions-choose-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [IIf](/sql/t-sql/functions/logical-functions-iif-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)ve [ayrıştırma](/sql/t-sql/functions/parse-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) dışında | Evet, tüm Transact-SQL [analitik](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), dönüştürme, [Tarih ve saat](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), mantıksal, [matematik](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) işlevleri. |
| **Yerleşik işlevler (metin)** | Evet. Tüm Transact-SQL [String](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)ve harmanlama Işlevleri, [STRING_ESCAPE](/sql/t-sql/functions/string-escape-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) ve [çeviri](/sql/t-sql/functions/translate-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) haricinde | Evet. Tüm Transact-SQL [String](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)ve harmanlama işlevleri. |
| **Yerleşik tablo-değer işlevleri** | Evet, [Transact-SQL satır kümesi işlevleri](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json?view=azure-sqldw-latest&preserve-view=true#rowset-functions), [openxml](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [opendatasource](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)ve [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) dışında | Evet, [Transact-SQL Rowset işlevleri](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json?view=azure-sqldw-latest&preserve-view=true#rowset-functions), [openxml](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [opendatasource](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)ve [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) dışında  |
| **Toplamlar** |  [Checksum_agg](/sql/t-sql/functions/checksum-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) ve [GROUPING_ID](/sql/t-sql/functions/grouping-id-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) hariç olmak üzere Transact-SQL yerleşik toplamaları | Transact-SQL yerleşik toplamaları. |
| **İşleçler** | Evet, [! >](/sql/t-sql/language-elements/not-greater-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) ve [! <](/sql/t-sql/language-elements/not-less-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) harıç tüm [Transact-SQL işleçleri](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Evet, tüm [Transact-SQL işleçleri](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)  |
| **Flow denetimi** | Evet. [CONTINUE](/sql/t-sql/language-elements/continue-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [goto](/sql/t-sql/language-elements/goto-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [Return](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [Use](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)ve [waıtm](/sql/t-sql/language-elements/waitfor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) hariç tüm [Transact-SQL-Flow ekstresi](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Evet. Tüm [Transact-SQL akış denetimi](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) -koşul içinde sorgu Seç `WHILE (...)` |
| **DDL deyimleri (oluştur, DEĞIŞTIR, bırak)** | Evet. Desteklenen nesne türleri için geçerli olan tüm Transact-SQL DDL deyimleri | Evet. Desteklenen nesne türleri için geçerli olan tüm Transact-SQL DDL deyimleri |

## <a name="security"></a>Güvenlik

SYNAPSE SQL, verilerinizi güvenli hale getirmek ve erişimi denetlemek için yerleşik güvenlik özelliklerini kullanmanıza olanak sağlar. Aşağıdaki tabloda SYNAPSE SQL tüketim modelleri arasındaki üst düzey farklılıklar karşılaştırılır.

|   | Ayrılmış | Sunucusuz |
| --- | --- | --- |
| **Oturum açma bilgileri** | Yok (veritabanlarında yalnızca kapsanan kullanıcılar desteklenir) | Yes |
| **Kullanıcılar** |  Yok (veritabanlarında yalnızca kapsanan kullanıcılar desteklenir) | Yes |
| **[Bağımsız kullanıcılar](/sql/relational-databases/security/contained-database-users-making-your-database-portable?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)** | Evet. **Note:** yalnızca BIR Azure AD kullanıcısı Kısıtlanmamış yönetici olabilir | Hayır |
| **SQL Kullanıcı adı/parola kimlik doğrulaması**| Yes | Yes |
| **Azure Active Directory (Azure AD) kimlik doğrulaması**| Evet, Azure AD kullanıcıları | Evet, Azure AD oturum açmaları ve kullanıcılar |
| **Depolama Azure Active Directory (Azure AD) geçiş kimlik doğrulaması** | Yes | Yes |
| **Storage SAS belirteci kimlik doğrulaması** | Hayır | Evet, [dış VERI kaynağı](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) veya örnek düzeyi [kimlik BILGILERI](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)IÇINDE [veritabanı kapsamlı kimlik bilgileri](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) kullanılıyor. |
| **Depolama erişim anahtarı kimlik doğrulaması** | Evet, [dış VERI kaynağında](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) [VERITABANı kapsamlı kimlik bilgisi](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) kullanma | Hayır |
| **Depolama [yönetilen kimlik](../security/synapse-workspace-managed-identity.md) doğrulaması** | Evet, [yönetilen hizmet kimliği kimlik bilgisi](../../azure-sql/database/vnet-service-endpoint-rule-overview.md?bc=%2fazure%2fsynapse-analytics%2fbreadcrumb%2ftoc.json&preserve-view=true&toc=%2fazure%2fsynapse-analytics%2ftoc.json&view=azure-sqldw-latest&preserve-view=true) kullanma | Evet, `Managed Identity` kimlik bilgisi kullanılıyor. |
| **Depolama uygulama kimliği kimlik doğrulaması** | [Evet](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Hayır |
| **İzinler-nesne düzeyi** | Evet, kullanıcılar için izin verme, reddetme ve Iptal etme özelliği dahil | Evet, desteklenen Sistem nesnelerinde Kullanıcı/oturum açma izinleri verme, reddetme ve Iptal etme özelliği de dahil olmak. |
| **İzinler-şema düzeyi** | Evet, şemadaki kullanıcılara/oturum açma izinlerini verme, reddetme ve Iptal etme özelliği dahil | Evet, şemadaki kullanıcılara/oturum açma izinlerini verme, reddetme ve Iptal etme özelliği dahil |
| **İzinler- [veritabanı düzeyi](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)** | Yes | Yes |
| **İzinler- [sunucu düzeyi](/sql/relational-databases/security/authentication-access/server-level-roles)** | Hayır | Evet, sysadmin ve diğer sunucu rolleri destekleniyor |
| **İzinler- [sütun düzeyinde güvenlik](../sql-data-warehouse/column-level-security.md?bc=%2fazure%2fsynapse-analytics%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fsynapse-analytics%2ftoc.json)** | Yes | Yes |
| **Roller/gruplar** | Evet (veritabanı kapsamlı) | Evet (sunucu ve veritabanı kapsamlı) |
| **Güvenlik &amp; kimliği işlevleri** | Bazı Transact-SQL güvenlik işlevleri ve işleçleri:  `CURRENT_USER` , `HAS_DBACCESS` , `IS_MEMBER` , `IS_ROLEMEMBER` , `SESSION_USER` , `SUSER_NAME` , `SUSER_SNAME` , `SYSTEM_USER` , `USER` , `USER_NAME` , `EXECUTE AS` , `OPEN/CLOSE MASTER KEY` | Bazı Transact-SQL güvenlik işlevleri ve işleçleri:  `CURRENT_USER` , `HAS_DBACCESS` , `HAS_PERMS_BY_NAME` , `IS_MEMBER', 'IS_ROLEMEMBER` , `IS_SRVROLEMEMBER` , `SESSION_USER` , `SESSION_CONTEXT` , `SUSER_NAME` , `SUSER_SNAME` , `SYSTEM_USER` , `USER` , `USER_NAME` , `EXECUTE AS` ve `REVERT` . Güvenlik işlevleri, dış verileri sorgulamak için kullanılamaz (sonucu sorguda kullanılabilen değişkende depolayın).  |
| **VERITABANı KAPSAMLı KIMLIK BILGILERI** | Yes | Yes |
| **SUNUCU KAPSAMLı KIMLIK BILGILERI** | Hayır | Yes |
| **Satır düzeyi güvenlik** | [Evet](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Hayır |
| **Saydam Veri Şifrelemesi (TDE)** | [Evet](../../azure-sql/database/transparent-data-encryption-tde-overview.md) | Hayır | 
| **Veri Bulma ve Sınıflandırma** | [Evet](../../azure-sql/database/data-discovery-and-classification-overview.md) | Hayır |
| **Güvenlik Açığı Değerlendirmesi** | [Evet](../../azure-sql/database/sql-vulnerability-assessment.md) | Hayır |
| **Gelişmiş Tehdit Koruması** | [Evet](../../azure-sql/database/threat-detection-overview.md)
| **Denetim** | [Evet](../../azure-sql/database/auditing-overview.md) | Hayır |
| **[Güvenlik duvarı kuralları](../security/synapse-workspace-ip-firewall.md)**| Yes | Yes |
| **[Özel uç nokta](../security/synapse-workspace-managed-private-endpoints.md)**| Yes | Yes |

Adanmış SQL havuzu ve sunucusuz SQL havuzu, verileri sorgulamak için standart Transact-SQL dilini kullanır. Ayrıntılı farklılıklar için [Transact-SQL dil başvurusuna](/sql/t-sql/language-reference)bakın.

## <a name="tools"></a>Araçlar

Verileri sorgulamak için SYNAPSE SQL 'e bağlanmak üzere çeşitli araçlar kullanabilirsiniz.

|   | Ayrılmış | Sunucusuz |
| --- | --- | --- |
| **Synapse Studio** | Evet, SQL betikleri | Evet, SQL betikleri |
| **Power BI** | Yes | [Evet](tutorial-connect-power-bi-desktop.md) |
| **Azure Analysis Service** | Yes | Yes |
| **Azure Data Studio** | Yes | Evet, sürüm 1.18.0 veya üzeri. SQL betikleri ve SQL Not defterleri desteklenir. |
| **SQL Server Management Studio** | Yes | Evet, sürüm 18,5 veya üzeri |

> [!NOTE]
> SSMS 'yi, sunucusuz SQL havuzuna ve sorgusuna bağlanmak için kullanabilirsiniz. Sürüm 18,5 ' den itibaren kısmen desteklenir, bunu yalnızca bağlanmak ve sorgulamak için kullanabilirsiniz.

Çoğu uygulama standart Transact-SQL dilini kullanır, SYNAPSE SQL 'in hem adanmış hem de sunucusuz tüketim modellerini sorgulayabilir.

## <a name="storage-options"></a>Depolama seçenekleri

Analiz edilen veriler, çeşitli depolama türlerinde depolanabilir. Aşağıdaki tabloda tüm kullanılabilir depolama seçenekleri listelenmektedir:

|   | Ayrılmış | Sunucusuz |
| --- | --- | --- |
| **İç depolama** | Yes | Hayır |
| **Azure Data Lake v2** | Yes | Yes |
| **Azure Blob Depolama** | Yes | Yes |
| **Azure SQL (uzak)** | Hayır | Hayır |
| **Azure CosmosDB işlem depolaması** | Hayır | Hayır |
| **Azure CosmosDB analitik depolama** | Hayır | Evet, [SYNAPSE link (Önizleme)](../../cosmos-db/synapse-link.md?bc=%2fazure%2fsynapse-analytics%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fsynapse-analytics%2ftoc.json) kullanma ([genel önizleme](../../cosmos-db/synapse-link.md?bc=%2fazure%2fsynapse-analytics%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fsynapse-analytics%2ftoc.json#limitations)) |
| **Apache Spark tabloları (çalışma alanında)** | Hayır | Yalnızca [meta veri eşitlemesini](develop-storage-files-spark-tables.md) kullanarak PARQUET tabloları |
| **Apache Spark tabloları (uzak)** | Hayır | Hayır |
| **Databricks tabloları (uzak)** | Hayır | Hayır |

## <a name="data-formats"></a>Veri biçimleri

Analiz edilen veriler, çeşitli depolama biçimlerinde depolanabilir. Aşağıdaki tabloda, çözümlenebilecek tüm kullanılabilir veri biçimleri listelenmektedir:

|   | Ayrılmış | Sunucusuz |
| --- | --- | --- |
| **Ted** | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | [Evet](query-single-csv-file.md) |
| **CSV** | Evet (çoklu karakter sınırlayıcıları desteklenmez) | [Evet](query-single-csv-file.md) |
| **Parquet** | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | [Evet](query-parquet-files.md), [iç içe geçmiş türler](query-parquet-nested-types.md) içeren dosyalar |
| **Hive ORC** | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Hayır |
| **Hive RC** | [Evet](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) | Hayır |
| **JSON** | Yes | [Evet](query-json-files.md) |
| **Avro** | Hayır | Hayır |
| **[Delta-Gölü](https://delta.io/)** | Hayır | Hayır |
| **[CDM](/common-data-model/)** | Hayır | Hayır |

## <a name="next-steps"></a>Sonraki adımlar
Adanmış SQL havuzu ve sunucusuz SQL havuzu için en iyi uygulamalarla ilgili ek bilgiler aşağıdaki makalelerde bulunabilir:

- [Adanmış SQL havuzu için en iyi uygulamalar](best-practices-sql-pool.md)
- [Sunucusuz SQL havuzu için en iyi yöntemler](best-practices-sql-on-demand.md)