---
title: Apache Kafka için Akka akışlarını kullanma-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Olay Hub 'ına Akka akışlarını bağlama hakkında bilgi sağlanır.
ms.topic: how-to
ms.date: 06/23/2020
ms.openlocfilehash: 92ab927189329493696c70b61ffc7f11cad22a66
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92369582"
---
# <a name="using-akka-streams-with-event-hubs-for-apache-kafka"></a>Apache Kafka için Event Hubs ile Akka Streams’i kullanma

Bu öğreticide, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Apache Kafka için Event Hubs desteği aracılığıyla Akka akışlarını nasıl bağlayabilmeniz gösterilmektedir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Akka Streams üreticisi çalıştırma 
> * Akka akışları tüketicisini Çalıştır

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/akka/java) 'da kullanılabilir

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlayabilmeniz için aşağıdaki önkoşullara sahip olduğunuzdan emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun. 
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.8+](/azure/developer/java/fundamentals/java-jdk-long-term-support)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* Maven ikili Arşivi [indirme](https://maven.apache.org/download.cgi) ve [yükleme](https://maven.apache.org/install.html)
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Herhangi bir Event Hubs hizmetinden göndermek veya almak için bir Event Hubs ad alanı gerekir. Ayrıntılı bilgi için bkz. [Olay Hub 'ı oluşturma](event-hubs-create.md) . Event Hubs bağlantı dizesini daha sonra kullanmak üzere kopyalamadığınızdan emin olun.

## <a name="clone-the-example-project"></a>Örnek projeyi kopyalama

Artık Event Hubs bir bağlantı dizesine sahip olduğunuza göre, Azure Event Hubs for Kafka Repository 'yi klonlayın ve `akka` alt klasöre gidin:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/akka/java
```

## <a name="run-akka-streams-producer"></a>Akka Streams üreticisi çalıştırma

Sunulan Akka Streams üreticisi örneğini kullanarak, Event Hubs hizmetine ileti gönderin.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Event Hubs Kafka uç noktası sağlama

#### <a name="producer-applicationconf"></a>Producer Application. conf

`bootstrap.servers` `sasl.jaas.config` Üretici ve değerlerini, `producer/src/main/resources/application.conf` doğru kimlik doğrulamasıyla, üreticiyi Event Hubs Kafka uç noktasına yönlendirmek için güncelleştirin.

```xml
akka.kafka.producer {
    #Akka Kafka producer properties can be defined here


    # Properties defined by org.apache.kafka.clients.producer.ProducerConfig
    # can be defined in this configuration section.
    kafka-clients {
        bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
        sasl.mechanism=PLAIN
        security.protocol=SASL_SSL
        sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

> [!IMPORTANT]
> `{YOUR.EVENTHUBS.CONNECTION.STRING}`Event Hubs ad alanınız için bağlantı dizesiyle değiştirin. Bağlantı dizesini alma hakkında yönergeler için bkz. [Event Hubs bağlantı dizesi alma](event-hubs-get-connection-string.md). Örnek bir yapılandırma aşağıda verilmiştir: `sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="Endpoint=sb://mynamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXX";`


### <a name="run-producer-from-the-command-line"></a>Üretici 'yi komut satırından Çalıştır

Producer 'ı komut satırından çalıştırmak için, JAR 'yi oluşturun ve Maven içinden çalıştırın (veya Maven kullanarak JAR 'yi oluşturun ve ardından, gerekli Kafka JAR 'leri, Sınıfyoluna ekleyerek Java 'da çalıştırın):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestProducer"
```

Üretici, konuya Olay Hub 'ına olay göndermeye başlar `test` ve olayları stdout 'a yazdırır.

## <a name="run-akka-streams-consumer"></a>Akka akışları tüketicisini Çalıştır

Belirtilen tüketici örneğini kullanarak Olay Hub 'ından ileti alın.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Event Hubs Kafka uç noktası sağlama

#### <a name="consumer-applicationconf"></a>Tüketici uygulaması. conf

`bootstrap.servers`' İ ve `sasl.jaas.config` değerlerini, `consumer/src/main/resources/application.conf` doğru kimlik doğrulamasıyla tüketicisini Event Hubs Kafka uç noktasına yönlendirmek için güncelleştirin.

```xml
akka.kafka.consumer {
    #Akka Kafka consumer properties defined here
    wakeup-timeout=60s

    # Properties defined by org.apache.kafka.clients.consumer.ConsumerConfig
    # defined in this configuration section.
    kafka-clients {
       request.timeout.ms=60000
       group.id=akka-example-consumer

       bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
       sasl.mechanism=PLAIN
       security.protocol=SASL_SSL
       sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

> [!IMPORTANT]
> `{YOUR.EVENTHUBS.CONNECTION.STRING}`Event Hubs ad alanınız için bağlantı dizesiyle değiştirin. Bağlantı dizesini alma hakkında yönergeler için bkz. [Event Hubs bağlantı dizesi alma](event-hubs-get-connection-string.md). Örnek bir yapılandırma aşağıda verilmiştir: `sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="Endpoint=sb://mynamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXX";`


### <a name="run-consumer-from-the-command-line"></a>Komut satırından tüketici çalıştırma

Tüketici 'yi komut satırından çalıştırmak için, JAR 'yi oluşturun ve Maven içinden çalıştırın (veya Maven kullanarak JAR 'yi oluşturun ve ardından, gerekli Kafka JAR 'leri, Sınıfyoluna ekleyerek Java 'da çalıştırın):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestConsumer"
```

Olay Hub 'ının olayları varsa (örneğin, üreticisi de çalışıyorsa), tüketici konusunun olayları almaya başlar `test` . 

Akka akışları hakkında daha ayrıntılı bilgi için [Akka Streams Kafka kılavuzuna](https://doc.akka.io/docs/akka-stream-kafka/current/home.html) göz atın.

## <a name="next-steps"></a>Sonraki adımlar
Kafka için Event Hubs hakkında daha fazla bilgi için aşağıdaki makalelere bakın:  

- [Bir olay hub'ında Kafka aracısı yansıtma](event-hubs-kafka-mirror-maker-tutorial.md)
- [Apache Spark'ı bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Apache Flink'i bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md)
- [Kafka Connect 'i bir olay hub 'ı ile tümleştirme](event-hubs-kafka-connect-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)
- [Azure Event Hubs için Apache Kafka Geliştirici Kılavuzu](apache-kafka-developer-guide.md)