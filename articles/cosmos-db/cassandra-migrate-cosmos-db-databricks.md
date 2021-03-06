---
title: Databricks (Spark) kullanarak Apache Cassandra 'dan Azure Cosmos DB Cassandra API geçirme
description: Azure Databricks ve Spark kullanarak Apache Cassandra veritabanından Azure Cosmos DB Cassandra API nasıl veri geçirebileceğinizi öğrenin.
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 11/16/2020
ms.author: thvankra
ms.reviewer: thvankra
ms.openlocfilehash: 3cbcb7eb3695e6f57daef741d4cd4b15577d8f58
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99493286"
---
# <a name="migrate-data-from-cassandra-to-azure-cosmos-db-cassandra-api-account-using-azure-databricks"></a>Cassandra 'dan verileri Azure Cosmos DB Cassandra API hesabına Azure Databricks kullanarak geçirin
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Azure Cosmos DB Cassandra API, Apache Cassandra üzerinde çalışan kurumsal iş yükleri için, şunlar gibi çeşitli nedenlerle harika bir seçenek haline geldi: 

* **Yönetme ve izleme için ek yük yoktur:** Bu, işletim sistemi, JVM ve YAML dosyaları ve etkileşimleri arasında ayarların bir listesini yönetme ve izleme yükünü ortadan kaldırır.

* **Önemli maliyet tasarrufları:** VM 'nin, bant genişliğinin ve ilgili lisansların maliyetini içeren Azure Cosmos DB maliyeti tasarrufu sağlayabilirsiniz. Ayrıca, veri merkezlerini, sunucuları, SSD depolamayı, ağ iletişimini ve elektrik maliyetlerini yönetmeniz gerekmez. 

* **Mevcut kod ve araçları kullanma özelliği:** Azure Cosmos DB, mevcut Cassandra SDK 'Ları ve araçları ile kablo protokol düzeyi uyumluluğu sağlar. Bu uyumluluk, küçük değişikliklerle Azure Cosmos DB Cassandra API’si ile mevcut kod tabanınızı kullanabilmenizi sağlar.

Veritabanı iş yüklerini bir platformdan diğerine geçirmek için çeşitli yollar vardır. [Azure Databricks](https://azure.microsoft.com/services/databricks/) , büyük ölçekte çevrimdışı geçişler gerçekleştirmeye yönelik bir yol sunan [Apache Spark](https://spark.apache.org/) hizmet olarak kullanılan bir platformdur. Bu makalede, yerel Apache Cassandra keyspaces/tablolarından Azure Databricks kullanarak Cassandra API Azure Cosmos DB için gereken adımlar açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

* [Azure Cosmos DB Cassandra API hesabı sağlama](create-cassandra-dotnet.md#create-a-database-account)

* [Azure Cosmos DB bağlanma temellerini inceleyin Cassandra API](cassandra-spark-generic.md)

* Uyumluluk sağlamak için [Azure Cosmos DB Cassandra API desteklenen özellikleri](cassandra-support.md) inceleyin.

* Hedef Azure Cosmos DB Cassandra API hesabınızda boş anahtar alanı ve tablo oluşturduğunuzdan emin olun

* [Doğrulama için csqlsh veya hosted Shell kullanma](cassandra-support.md#hosted-cql-shell-preview)

## <a name="provision-an-azure-databricks-cluster"></a>Azure Databricks kümesi sağlama

[Azure Databricks kümesi sağlamak](/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal)için talimatları izleyebilirsiniz. Ancak, lütfen Apache Spark 3. x Apache Cassandra Bağlayıcısı için şu anda desteklenmiyor. Apache Spark desteklenen v2. x sürümü ile bir Databricks çalışma zamanı sağlamanız gerekir. Databricks çalışma zamanının sürümünü, Scala sürüm 2,11 ' den sonra olmayan Spark 2. x öğesinin en son sürümünü destekleyen bir sürümünü seçmenizi öneririz:

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-runtime.png" alt-text="Databricks çalışma zamanı":::


## <a name="add-dependencies"></a>Bağımlılık Ekle

Hem yerel hem de Cosmos DB Cassandra uç noktalarına bağlanmak için Apache Spark Cassandra bağlayıcı kitaplığını kümenize eklemeniz gerekir. Kümenizde kitaplıkları seçin-> yeni > Maven-> arama paketlerini yükler:

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-search-packages.png" alt-text="Databricks arama paketleri":::

`Cassandra`Arama kutusuna yazın ve kullanılabilir en son `spark-cassandra-connector` Maven havuzunu seçin ve ardından Install ' ı seçin:

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-search-packages-2.png" alt-text="Databricks paketleri seçin":::

> [!NOTE]
> Cassandra bağlayıcı kitaplığı yüklendikten sonra Databricks kümesini yeniden başlattığınızdan emin olun.

## <a name="create-scala-notebook-for-migration"></a>Geçiş için Scala Not defteri oluşturma

Databricks 'te aşağıdaki kodla bir Scala Not defteri oluşturun. Kaynak ve hedef Cassandra yapılandırmalarınızı ilgili kimlik bilgileriyle ve kaynak/hedef anahtar alanları ve tabloları ile değiştirin ve ardından şunu çalıştırın:

```scala
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql._
import org.apache.spark.SparkContext

// source cassandra configs
val nativeCassandra = Map( 
    "spark.cassandra.connection.host" -> "<Source Cassandra Host>",
    "spark.cassandra.connection.port" -> "9042",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "false",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>"
)

//target cassandra configs
val cosmosCassandra = Map( 
    "spark.cassandra.connection.host" -> "<USERNAME>.cassandra.cosmos.azure.com",
    "spark.cassandra.connection.port" -> "10350",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "true",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>",
    //throughput related settings below - tweak these depending on data volumes. 
    "spark.cassandra.output.batch.size.rows"-> "1",
    "spark.cassandra.connection.connections_per_executor_max" -> "10",
    "spark.cassandra.output.concurrent.writes" -> "1000",
    "spark.cassandra.concurrent.reads" -> "512",
    "spark.cassandra.output.batch.grouping.buffer.size" -> "1000",
    "spark.cassandra.connection.keep_alive_ms" -> "600000000"
)

//Read from native Cassandra
val DFfromNativeCassandra = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(nativeCassandra)
  .load
  
//Write to CosmosCassandra
DFfromNativeCassandra
  .write
  .format("org.apache.spark.sql.cassandra")
  .options(cosmosCassandra)
  .save
```

> [!NOTE]
> `spark.cassandra.output.batch.size.rows`, `spark.cassandra.output.concurrent.writes` Ve konfigürasyonları, `connections_per_executor_max` Azure Cosmos DB istekleri sağlanan aktarım hızını/([İstek birimleri](./request-units.md)) aştığında gerçekleşen [hız sınırlamasını](/samples/azure-samples/azure-cosmos-cassandra-java-retry-sample/azure-cosmos-db-cassandra-java-retry-sample/)önlemek için önemlidir. Bu ayarları Spark kümesindeki yürüticilere sayısına ve hedef tablolara yazılmakta olan her kaydın boyutunu (ve dolayısıyla RU maliyeti) bağlı olarak ayarlamanız gerekebilir.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="rate-limiting-429-error"></a>Hız sınırlaması (429 hata)
`request rate is large`Yukarıdaki ayarları minimum değerlerine küçültirken 429 veya hata metninin hata kodunu görebilirsiniz. Bazı senaryolar aşağıda verilmiştir:

- **Tabloya ayrılan üretilen iş, 6000 [istek biriminden](./request-units.md)daha azdır**. En düşük ayarlarda bile Spark, 6000 istek birimi veya daha fazla bir hızda yazma işlemleri yürütebilecektir. Sağlanan paylaşılan aktarım hızı ile bir anahtar alanı içinde bir tablo sağladıysanız, bu tabloda çalışma zamanında 6000 ru 'dan az kullanılabilir. Geçirmekte olduğunuz tablonun geçiş çalıştırılırken en az 6000 ru tarafından kullanılabilir olduğundan ve gerekirse adanmış istek birimlerini bu tabloya ayırdığından emin olun. 
- **Büyük veri hacimiyle aşırı veri eğriliği**. Belirli bir tabloya geçiş yapmak için büyük miktarda veriniz (tablo satırları) varsa, ancak veride önemli bir çarpıklık varsa (yani aynı bölüm anahtarı değeri için yazılmakta olan çok sayıda kayıt), tablonuzda sağlanan büyük miktarda [istek birimseniz](./request-units.md) bile ücret sınırlandırmaya devam edebilirsiniz. Bunun nedeni, istek birimlerinin fiziksel bölümler arasında eşit olarak bölünmesinin yanı sıra ağır veri eğriliği, tek bir bölüme yönelik isteklerin performans sorunlarına neden olabilir ve bu da hız sınırlaması sağlar. Bu senaryoda, hız sınırlamasını önlemek ve geçişin yavaş çalışmasına zorlamak için Spark 'ta minimum işleme ayarlarına azaltmanız önerilir. Bu senaryo, erişimin daha sık olduğu ancak eğriliği yüksek olabilecek başvuru veya denetim tabloları geçirilirken daha yaygın olabilir. Ancak, başka bir tablo türünde önemli bir eğme varsa, düzenli durum işlemleri sırasında iş yükünüzün sık erişimli bölüm sorunlarından kaçınmak için veri modelinizi gözden geçirmeniz önerilir. 
- **Büyük tablo üzerinde sayı alınamıyor**. Çalışma `select count(*) from table` Şu anda büyük tablolar için desteklenmiyor. Azure portal ( [sorun giderme makalemize](cassandra-troubleshoot.md)bakın) ölçümlerinden gelen sayıyı alabilirsiniz, ancak bir Spark işi bağlamı içinden büyük bir tablonun sayısını belirlemeniz gerekiyorsa, verileri geçici bir tabloya kopyalayabilir ve sonra sayıyı almak IÇIN Spark SQL 'i kullanabilirsiniz (örneğin, `<primary key>` sonuçta elde edilen geçici tablodaki bazı alanlar ile değiştirin).

  ```scala
  val ReadFromCosmosCassandra = sqlContext
    .read
    .format("org.apache.spark.sql.cassandra")
    .options(cosmosCassandra)
    .load

  ReadFromCosmosCassandra.createOrReplaceTempView("CosmosCassandraResult")
  %sql
  select count(<primary key>) from CosmosCassandraResult
  ```

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md) 
* [Bölüm anahtarı en iyi uygulamaları](partitioning-overview.md#choose-partitionkey)
* [Azure Cosmos DB kapasite planlayıcısı makalelerini kullanarak ru/s 'Yi tahmin](estimate-ru-with-capacity-planner.md) etme
* [Azure Cosmos DB Cassandra API elastik ölçek](manage-scale-cassandra.md)
