---
title: TAHMIN eden makine öğrenimi modellerini puan edin
description: Adanmış SQL havuzundaki T-SQL tahmın etme işlevini kullanarak makine öğrenimi modellerini nasıl puanlandırıp kullanacağınızı öğrenin.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 07/21/2020
ms.author: anjangsh
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 9e7d45a588e60cd082f1eef43d1d1b6681b9e912
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98117750"
---
# <a name="score-machine-learning-models-with-predict"></a>TAHMIN eden makine öğrenimi modellerini puan edin

Adanmış SQL havuzu, tanıdık T-SQL dilini kullanarak makine öğrenimi modellerini skor yeteneği sağlar. T-SQL ' i [tahmin](/sql/t-sql/queries/predict-transact-sql?preserve-view=true&view=azure-sqldw-latest)etmek için, mevcut makine öğrenimi modellerinizi geçmiş verilerle eğitimli hale getirebilir ve veri ambarınızın güvenli sınırları dahilinde puan verebilirsiniz. TAHMIN işlevi bir [Onnx (Open sinir Network Exchange)](https://onnx.ai/) modeli ve verileri giriş olarak alır. Bu özellik, değerli verileri Puanlama için veri ambarının dışına taşıma adımını ortadan kaldırır. Veri uzmanlarının, kendi görevleri için doğru çatı ile çalışan veri bilimcilerine sorunsuz işbirliği yapmasına olanak tanıyan T-SQL arabirimiyle makine öğrenimi modellerini kolay bir şekilde dağıtmak için amaçlar.

> [!NOTE]
> Bu işlevsellik şu anda sunucusuz SQL havuzunda desteklenmez.

İşlevsellik, modelin SYNAPSE SQL dışında eğitilliğini gerektirir. Modeli oluşturduktan sonra, verileri veri ambarına yükleyin ve verilerden Öngörüler elde etmek için T-SQL tahmin sözdizimiyle puan edin.

![predictoverview](./media/sql-data-warehouse-predict/datawarehouse-overview.png)

## <a name="training-the-model"></a>Modeli eğitme

Adanmış SQL havuzu, önceden eğitilen bir model bekliyor. Adanmış SQL havuzunda tahmin yapmak için kullanılan bir makine öğrenimi modeline eğitim yaparken aşağıdaki faktörleri aklınızda tutun.

- Adanmış SQL havuzu yalnızca ONNX biçim modellerini destekler. ONNX, birlikte çalışabilirliği etkinleştirmek için çeşitli çerçeveler arasında modeller alışverişi yapmanıza olanak sağlayan açık kaynaklı bir model biçimidir. Mevcut modellerinizi, yerel olarak destekleyen veya kullanılabilir paketleri dönüştüren çerçeveleri kullanarak ONNX biçimine dönüştürebilirsiniz. Örneğin, [sköğren-onnx](https://github.com/onnx/sklearn-onnx) paketi scikit-geçiş modellerini onnx 'e dönüştürün. [Onnx GitHub deposu](https://github.com/onnx/tutorials#converting-to-onnx-format) desteklenen çerçeveler ve örneklerin bir listesini sağlar.

   Eğitim için [OTOMATIK ml](../../machine-learning/concept-automated-ml.md) kullanıyorsanız, BIR onnx biçim modeli oluşturmak için *enable_onnx_compatible_models* parametresini true olarak ayarladığınızdan emin olun. [Otomatik Machine Learning Not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) , otomatik ml 'nın onnx biçiminde bir makine öğrenimi modeli oluşturmak için nasıl kullanılacağına ilişkin bir örnek gösterir.

- Giriş verileri için aşağıdaki veri türleri desteklenir:
    - int, bigint, Real, float
    - Char, varchar, nvarchar

- Puanlama verilerinin eğitim verileriyle aynı biçimde olması gerekir. Çok boyutlu diziler gibi karmaşık veri türleri tahmın tarafından desteklenmez. Bu nedenle, eğitim için her model girişinin tüm girişleri içeren tek bir diziyi geçirmek yerine Puanlama tablosunun tek bir sütununa karşılık geldiğinden emin olun.

- Model girişlerinin adlarının ve veri türlerinin, yeni tahmin verilerinin sütun adlarıyla ve veri türleriyle eşleştiğinden emin olun. Çeşitli açık kaynaklı araçları kullanarak bir ONNX modelini görselleştirme, hata ayıklama konusunda daha fazla yardım sağlayabilir.

## <a name="loading-the-model"></a>Model yükleniyor

Model, ayrılmış bir SQL havuzu Kullanıcı tablosunda onaltılık bir dize olarak depolanır. Modeli tanımlamak için model tablosuna KIMLIK ve açıklama gibi ek sütunlar eklenebilir. Model sütununun veri türü olarak varbinary (max) kullanın. Modelleri depolamak için kullanılabilecek bir tablo için kod örneği aşağıda verilmiştir:

```sql
-- Sample table schema for storing a model and related data
CREATE TABLE [dbo].[Models]
(
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [Model] [varbinary](max) NULL,
    [Description] [varchar](200) NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN,
    HEAP
)
GO

```

Model bir onaltılık dizeye ve belirtilen tablo tanımına dönüştürüldükten sonra, modeli adanmış SQL havuzu tablosuna yüklemek için [copy komutunu](/sql/t-sql/statements/copy-into-transact-sql?preserve-view=true&view=azure-sqldw-latest) veya PolyBase 'i kullanın. Aşağıdaki kod örneği, modeli yüklemek için Copy komutunu kullanır.

```sql
-- Copy command to load hexadecimal string of the model from Azure Data Lake storage location
COPY INTO [Models] (Model)
FROM '<enter your storage location>'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<enter your storage key here>')
)
```

## <a name="scoring-the-model"></a>Modeli Puanlama

Model ve veriler veri ambarına yüklendikten sonra, modele puan vermek için **T-SQL tahmin** etme işlevini kullanın. Yeni giriş verilerinin, modeli oluşturmak için kullanılan eğitim verileriyle aynı biçimde olduğundan emin olun. T-SQL tahmın etme iki giriş alır: model ve yeni Puanlama girişi verileri ve çıktı için yeni sütunlar oluşturur. Model bir değişken, değişmez değer veya skaler sub_query olarak belirtilebilir. Veri parametresi için adlandırılmış bir sonuç kümesi belirtmek üzere [common_table_expression ile](/sql/t-sql/queries/with-common-table-expression-transact-sql?preserve-view=true&view=azure-sqldw-latest) kullanın.

Aşağıdaki örnekte, tahmin işlevi kullanan örnek bir sorgu gösterilmektedir. Tahmin sonuçlarını içeren ad *puanı* ve *float* veri türü içeren ek bir sütun oluşturulur. Tüm giriş verisi sütunlarının yanı sıra, select ifadesiyle birlikte görüntülenecek çıkış tahmin sütunları vardır. Daha ayrıntılı bilgi için bkz. [tahmin (Transact-SQL)](/sql/t-sql/queries/predict-transact-sql?preserve-view=true&view=azure-sqldw-latest).

```sql
-- Query for ML predictions
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT Model FROM Models WHERE Id = 1),
DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score float) AS p;
```

## <a name="next-steps"></a>Sonraki adımlar

TAHMIN işlevi hakkında daha fazla bilgi edinmek için bkz. [tahmin (Transact-SQL)](/sql/t-sql/queries/predict-transact-sql?preserve-view=true&view=azure-sqldw-latest).