---
title: 'Azure Toolkit for IntelliJ: Spark uygulaması-HDInsight'
description: Scala 'da yazılmış Spark uygulamaları geliştirmek ve bunları bir HDInsight Spark kümesine göndermek için Azure Toolkit for IntelliJ kullanın.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 04/13/2020
ms.openlocfilehash: 0b2c2a91ba9d029bcf24b0980d0a2339194e6d8a
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98931034"
---
# <a name="use-azure-toolkit-for-intellij-to-create-apache-spark-applications-for-hdinsight-cluster"></a>HDInsight kümesi için Apache Spark uygulamalar oluşturmak için Azure Toolkit for IntelliJ kullanma

Bu makalede, IntelliJ IDE için **Azure araç seti** eklentisini kullanarak Azure hdınsight 'ta Apache Spark uygulamalarının nasıl geliştirilmesi gösterilmektedir. [Azure HDInsight](../hdinsight-overview.md) , bulutta yönetilen ve açık kaynaklı bir analiz hizmetidir. Hizmet, Hadoop, Apache Spark, Apache Hive ve Apache Kafka gibi açık kaynaklı çerçeveleri kullanmanıza olanak sağlar.

**Azure araç seti** eklentisini birkaç şekilde kullanabilirsiniz:

* Bir HDInsight Spark kümesine Scala Spark uygulaması geliştirin ve gönderebilirsiniz.
* Azure HDInsight Spark kümesi kaynaklarınıza erişin.
* Yerel olarak bir Scala Spark uygulaması geliştirin ve çalıştırın.

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Azure Toolkit for IntelliJ eklentisini kullanma
> * Apache Spark uygulamaları geliştirme
> * Azure HDInsight kümesine uygulama gönderme

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

* [Oracle Java geliştirme seti](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).  Bu makalede Java sürüm 8.0.202 kullanılmaktadır.

* IntelliJ fıkrı. Bu makalede [IntelliJ fikir topluluk ver kullanılmaktadır.  2018.3.4](https://www.jetbrains.com/idea/download/).

* Azure Toolkit for IntelliJ.  Bkz. [Azure Toolkit for IntelliJ yükleme](/azure/developer/java/toolkit-for-intellij/).

## <a name="install-scala-plugin-for-intellij-idea"></a>IntelliJ IDEA için Scala eklentisini yükleme

Scala eklentisini yüklemeye yönelik adımlar:

1. IntelliJ IDEA’yı açın.

2. Giriş ekranında , eklentiler  >  penceresini açmak için **eklentileri** Yapılandır ' a gidin  .

    ![IntelliJ fıkır Scala eklentisine izin verebilir](./media/apache-spark-intellij-tool-plugin/enable-scala-plugin1.png)

3. Yeni pencerede tanıtılan Scala eklentisine yönelik **Install** öğesini seçin.  

    ![IntelliJ fıkır Scala eklentisini yüklüyor](./media/apache-spark-intellij-tool-plugin/install-scala-plugin.png)

4. Eklenti başarıyla yüklendikten sonra IDE’yi yeniden başlatmanız gerekir.

## <a name="create-a-spark-scala-application-for-an-hdinsight-spark-cluster"></a>HDInsight Spark kümesi için Spark Scala uygulaması oluşturma

1. IntelliJ FIKRINI başlatın ve yeni proje **Oluştur** ' u seçerek **Yeni proje** penceresini açın.

2. Sol bölmeden **Azure Spark/HDInsight** ' ı seçin.

3. Ana pencereden **Spark projesi (Scala)** öğesini seçin.

4. **Yapı aracı** açılan listesinde, aşağıdaki seçeneklerden birini seçin:
   * Scala projesi oluşturma Sihirbazı desteği için **Maven** .
   * Scala projesi için bağımlılıkları ve oluşturmayı yönetmek üzere **SBT** .

     ![IntelliJ fıkır yeni proje iletişim kutusu](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

5. **İleri**’yi seçin.

6. **Yeni proje** penceresinde, aşağıdaki bilgileri sağlayın:  

    |  Özellik   | Açıklama   |  
    | ----- | ----- |  
    |Proje adı| Bir ad girin.  Bu makalede, kullanılır `myApp` .|  
    |Proje &nbsp; konumu| Projenizin kaydedileceği konumu girin.|
    |Proje SDK 'Sı| Bu alan, ilk fıkrın kullanımı üzerinde boş olabilir.  **Yeni...** öğesini seçin ve JDK 'nize gidin.|
    |Spark sürümü|Oluşturma Sihirbazı Spark SDK ve Scala SDK için doğru sürümü tümleştirir. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark2.x** seçeneğini belirleyin. Bu örnek **Spark 2.3.0 (Scala 2.11.8)** kullanır.|

    ![Apache Spark SDK 'sını seçme](./media/apache-spark-intellij-tool-plugin/intellij-new-project.png)

7. **Son**'u seçin.  Projenin kullanılabilir hale gelmesi birkaç dakika sürebilir.

8. Spark projesi sizin için otomatik olarak bir yapı oluşturur. Yapıtı görüntülemek için aşağıdaki adımları uygulayın:

   a. Menü çubuğundan **Dosya**  >  **projesi yapısına git...**.

   b. **Proje yapısı** penceresinde **yapıtlar**' ı seçin.  

   c. Yapıtı görüntüledikten sonra **iptal**  ' i seçin.

      ![İletişim kutusunda yapıt bilgisi](./media/apache-spark-intellij-tool-plugin/default-artifact-dialog.png)

9. Aşağıdaki adımları uygulayarak uygulama kaynak kodunuzu ekleyin:

    a. Projeden **MyApp**  >  **src**  >  **Main**  >  **Scala**' ya gidin.  

    b. **Scala** öğesine sağ tıklayın ve ardından **Yeni**  >  **Scala sınıfına** gidin.

   ![Projeden bir Scala sınıfı oluşturma komutları](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   c. **Yeni Scala sınıfı oluştur** iletişim kutusunda bir ad belirtin, **tür** açılır listesinden **nesne** ' yi seçin ve ardından **Tamam**' ı seçin.

     ![Yeni Scala sınıfı oluştur iletişim kutusu](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   d. **MyApp. Scala** dosyası daha sonra ana görünümde açılır. Varsayılan kodu aşağıda bulunan kodla değiştirin:  

    ```scala
    import org.apache.spark.SparkConf
    import org.apache.spark.SparkContext

    object myApp{
        def main (arg: Array[String]): Unit = {
        val conf = new SparkConf().setAppName("myApp")
        val sc = new SparkContext(conf)

        val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        //find the rows that have only one digit in the seventh column in the CSV file
        val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

        rdd1.saveAsTextFile("wasbs:///HVACOut")
        }

    }
    ```

    Kod, HVAC.csv verileri okur (tüm HDInsight Spark kümelerinde kullanılabilir), CSV dosyasında yedinci sütununda yalnızca bir basamak olan satırları alır ve çıktıyı `/HVACOut` kümenin varsayılan depolama kapsayıcısının altına yazar.

## <a name="connect-to-your-hdinsight-cluster"></a>HDInsight kümenize bağlanma

Kullanıcı [Azure aboneliğinde oturum](#sign-in-to-your-azure-subscription)açabilir ya da [bir HDInsight kümesini bağlayabilir](#link-a-cluster). HDInsight kümenize bağlanmak için, ambarı Kullanıcı adı/parolası veya etki alanına katılmış kimlik bilgilerini kullanın.

### <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

1. Menü çubuğunda, **Görünüm**  >  **Aracı Windows**  >  **Azure Gezgini**' ne gidin.

   ![IntelliJ fıkır Azure Gezginini gösterir](./media/apache-spark-intellij-tool-plugin/show-azure-explorer1.png)

2. Azure Gezgini 'nden **Azure** düğümüne sağ tıklayın ve ardından **oturum aç**' ı seçin.

   ![IntelliJ fıkır Gezgini, Azure 'a sağ tıklayın](./media/apache-spark-intellij-tool-plugin/explorer-rightclick-azure.png)

3. **Azure oturum açma** Iletişim kutusunda **cihaz oturumu açma**' yı ve ardından **oturum aç**' ı seçin.

    ![' IntelliJ fıkır Azure oturum açma aygıtı login'](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer2.png)

4. **Azure cihaz oturum açma** Iletişim kutusunda **Kopyala&aç**' a tıklayın.

   ![' IntelliJ fıkır Azure cihaz login'](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer5.png)

5. Tarayıcı arabiriminde, kodu yapıştırın ve ardından **İleri**' ye tıklayın.

   ![' HDI için Microsoft Kod gir iletişim kutusu](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer6.png)

6. Azure kimlik bilgilerinizi girin ve ardından tarayıcıyı kapatın.

   ![' Microsoft HDI için e-posta gir iletişim kutusu](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer7.png)

7. Oturum açtıktan sonra, **abonelikler Seç** iletişim kutusunda kimlik bilgileriyle Ilişkili tüm Azure abonelikleri listelenir. Aboneliğinizi seçin ve ardından **Seç** düğmesini seçin.

    ![Abonelik Seç iletişim kutusu](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

8. **Azure Explorer**'da, aboneliklerinizde bulunan HDInsight Spark kümelerini görüntülemek için **HDInsight** ' ı genişletin.

    ![IntelliJ fıkır Azure Explorer ana görünümü](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer3.png)

9. Kümeyle ilişkili kaynakları (örneğin, depolama hesapları) görüntülemek için bir küme adı düğümünü daha da genişletebilirsiniz.

    ![Azure Gezgini depolama hesapları](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer4.png)

### <a name="link-a-cluster"></a>Bir kümeyi bağlama

Apache ambarı yönetilen Kullanıcı adını kullanarak bir HDInsight kümesini bağlayabilirsiniz. Benzer şekilde, etki alanına katılmış bir HDInsight kümesi için, etki alanı ve Kullanıcı adı (gibi) kullanarak bağlantı oluşturabilirsiniz `user1@contoso.com` . Ayrıca, Livy hizmet kümesini bağlayabilirsiniz.

1. Menü çubuğunda, **Görünüm**  >  **Aracı Windows**  >  **Azure Gezgini**' ne gidin.

1. Azure Gezgini 'nden **HDInsight** düğümüne sağ tıklayın ve ardından **bir kümeyi bağla**' yı seçin.

   ![Azure Explorer bağlantı kümesi bağlam menüsü](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

1. **Küme bağlantısı** penceresindeki kullanılabilir seçenekler, **kaynak türü** açılan listesinde hangi değere seçtiğinize bağlı olarak değişir.  Değerlerinizi girip **Tamam**' ı seçin.

    * **HDInsight kümesi**  
  
        |Özellik |Değer |
        |----|----|
        |Kaynak türünü bağla|Aşağı açılan listeden **HDInsight kümesi** ' ni seçin.|
        |Küme adı/URL 'SI| Küme adı girin.|
        |Kimlik Doğrulama Türü| **Temel kimlik doğrulaması** olarak bırak|
        |User Name| Küme Kullanıcı adı ' nı girin, varsayılan olarak admin ' dir.|
        |Parola| Kullanıcı adı için parola girin.|

        ![IntelliJ fıkır bir küme iletişim kutusu bağlama](./media/apache-spark-intellij-tool-plugin/link-hdinsight-cluster-dialog.png)

    * **Livy hizmeti**  
  
        |Özellik |Değer |
        |----|----|
        |Kaynak türünü bağla|Açılan listeden **Livy hizmeti** ' ni seçin.|
        |Livy uç noktası| Livy uç noktası girin|
        |Küme Adı| Küme adı girin.|
        |Yarn uç noktası|İsteğe bağlı.|
        |Kimlik Doğrulama Türü| **Temel kimlik doğrulaması** olarak bırak|
        |User Name| Küme Kullanıcı adı ' nı girin, varsayılan olarak admin ' dir.|
        |Parola| Kullanıcı adı için parola girin.|

        ![IntelliJ fıkır bağlama Livy kümesi iletişim kutusu](./media/apache-spark-intellij-tool-plugin/link-livy-cluster-dialog.png)

1. Bağlı kümenizi **HDInsight** düğümünden görebilirsiniz.

   ![Azure Explorer bağlantılı Cluster1](./media/apache-spark-intellij-tool-plugin/hdinsight-linked-cluster.png)

1. Ayrıca bir kümenin bağlantısını **Azure Explorer**'da kaldırabilirsiniz.

   ![Azure Explorer bağlantısı olmayan küme](./media/apache-spark-intellij-tool-plugin/hdi-unlinked-cluster.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Bir HDInsight Spark kümesinde Spark Scala uygulaması çalıştırma

Bir Scala uygulaması oluşturduktan sonra kümeye gönderebilirsiniz.

1. Projeden **MyApp**  >  **src**  >  **Main**  >  **Scala**  >  **MyApp** sayfasına gidin.  **MyApp** öğesine sağ tıklayın ve **Spark uygulaması gönder** ' i seçin (büyük olasılıkla listenin altında bulunur).

      ![Spark uygulamasını HDInsight 'a Gönder komutu](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

2. **Spark uygulaması gönder** iletişim penceresinde 1 ' i seçin **. HDInsight 'ta Spark**.

3. **Yapılandırma düzenleme** penceresinde, aşağıdaki değerleri girip **Tamam**' ı seçin:

    |Özellik |Değer |
    |----|----|
    |Spark kümeleri (yalnızca Linux)|Uygulamanızı çalıştırmak istediğiniz HDInsight Spark kümesini seçin.|
    |Göndermek için bir yapıt seçin|Varsayılan ayarı bırakın.|
    |Ana sınıf adı|Varsayılan değer, seçili dosyadaki ana sınıftır. Üç noktayı (**...**) seçerek sınıfı değiştirebilirsiniz  ve başka bir sınıf seçebilirsiniz.|
    |İş yapılandırması|Varsayılan anahtarları ve ya da değerleri değiştirebilirsiniz. Daha fazla bilgi için bkz. [Apache Livy REST API](https://livy.incubator.apache.org/docs/latest/rest-api.html).|
    |Komut satırı bağımsız değişkenleri|Gerekirse ana sınıf için boşlukla ayrılmış bağımsız değişkenler girebilirsiniz.|
    |Başvurulan Jliler ve başvurulan dosyalar|Varsa, başvurulan jars ve dosyalar için yollar girebilirsiniz. Azure sanal dosya sisteminde, şu anda yalnızca ADLS Gen 2 kümesini destekleyen dosyalara da gidebilirsiniz. Daha fazla bilgi için: [Apache Spark yapılandırma](https://spark.apache.org/docs/latest/configuration.html#runtime-environment).  Ayrıca bkz. [kaynakları kümeye yükleme](../../storage/blobs/storage-quickstart-blobs-storage-explorer.md).|
    |İş yükleme depolaması|Ek seçenekleri açığa çıkarmak için genişletin.|
    |Depolama Türü|Açılan listeden **karşıya yüklemek Için Azure blobu kullan** ' ı seçin.|
    |Depolama Hesabı|Depolama hesabınızı girin.|
    |Depolama anahtarı|Depolama anahtarınızı girin.|
    |Depolama kapsayıcısı|**Depolama hesabı** ve **depolama anahtarı** girildikten sonra, açılan listeden depolama kapsayıcınızı seçin.|

    ![Spark gönderimi iletişim kutusu](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-02.png)

4. Projenizi seçili kümeye göndermek için **mini Iş çalıştırma** ' yı seçin. **Küme Içindeki uzak Spark işi** , en altta iş yürütme ilerleme durumunu görüntüler. Kırmızı düğmeye tıklayarak uygulamayı durdurabilirsiniz.

    ![Apache Spark gönderme penceresi](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)

## <a name="debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>HDInsight kümesinde Apache Spark uygulamalarında yerel olarak veya uzaktan hata ayıklama

Spark uygulamasının kümeye gönderilmesi için de başka bir yol önerilir. Bunu, **Çalıştır/hata ayıkla yapılandırması** IDE içindeki parametreleri ayarlayarak yapabilirsiniz. Bkz. [Apache Spark uygulamaları, Azure TOOLKIT for INTELLIJ SSH aracılığıyla bir HDInsight kümesinde yerel olarak veya uzaktan hata ayıklayın](apache-spark-intellij-tool-debug-remotely-through-ssh.md).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ kullanarak HDInsight Spark kümelerine erişin ve yönetin

Azure Toolkit for IntelliJ kullanarak çeşitli işlemler gerçekleştirebilirsiniz.  İşlemlerin çoğu **Azure Explorer**'dan başlatılır.  Menü çubuğunda, **Görünüm**  >  **Aracı Windows**  >  **Azure Gezgini**' ne gidin.

### <a name="access-the-job-view"></a>İş görünümüne erişin

1. Azure Gezgini 'nden **HDInsight**  >  \<Your Cluster>  >  **işleri**' ne gidin.

    ![IntelliJ Azure Gezgini Iş görünümü düğümü](./media/apache-spark-intellij-tool-plugin/intellij-job-view-node.png)

2. Sağ bölmede **Spark Iş görünümü** sekmesi kümede çalıştırılan tüm uygulamaları görüntüler. Daha fazla ayrıntı görmek istediğiniz uygulamanın adını seçin.

    ![Spark Iş görünümü uygulama ayrıntıları](./media/apache-spark-intellij-tool-plugin/intellij-view-job-logs.png)

3. Temel çalışan iş bilgilerini göstermek için iş grafiğinin üzerine gelin. Her işin ürettiği aşamalar grafiğini ve bilgilerini görüntülemek için, iş grafiğinde bir düğüm seçin.

    ![Spark Iş görünümü Iş aşaması ayrıntıları](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. *Sürücü stderr*, *sürücü stdout* ve *Dizin bilgileri* gibi sık kullanılan günlükleri görüntülemek için **günlük** sekmesini seçin.

    ![Spark Iş görünümü günlüğü ayrıntıları](./media/apache-spark-intellij-tool-plugin/intellij-job-log-info.png)

5. Spark geçmişi Kullanıcı arabirimini ve YARN Kullanıcı arabirimini (uygulama düzeyinde) görüntüleyebilirsiniz. Pencerenin üst kısmında bir bağlantı seçin.

### <a name="access-the-spark-history-server"></a>Spark geçmiş sunucusuna erişin

1. Azure Gezgini 'nden **HDInsight**' ı genişletin, Spark kümeniz adına sağ tıklayın ve ardından **Spark geçmişi Kullanıcı arabirimini açın**' ı seçin.  
2. İstendiğinde, kümeyi ayarlarken belirttiğiniz kümenin yönetici kimlik bilgilerini girin.

3. Spark geçmiş sunucusu panosunda, az önce çalıştırmayı tamamlamış uygulamayı aramak için uygulama adını kullanabilirsiniz. Yukarıdaki kodda, kullanarak uygulama adını ayarlarsınız `val conf = new SparkConf().setAppName("myApp")` . Spark uygulamanızın adı **MyApp**.

### <a name="start-the-ambari-portal"></a>Ambarı portalını başlatma

1. Azure Gezgini 'nden **HDInsight**' ı genişletin, Spark kümeniz adına sağ tıklayın ve ardından **küme yönetim portalı aç (ambarı)** öğesini seçin.  

2. İstendiğinde, kümenin yönetici kimlik bilgilerini girin. Bu kimlik bilgilerini küme kurulum işlemi sırasında belirttiniz.

### <a name="manage-azure-subscriptions"></a>Azure aboneliklerini yönetme

Varsayılan olarak, Azure Toolkit for IntelliJ tüm Azure aboneliklerinizden Spark kümelerini listeler. Gerekirse, erişmek istediğiniz abonelikleri belirtebilirsiniz.  

1. Azure Gezgini 'nden **Azure** kök düğümüne sağ tıklayın ve ardından **abonelikleri Seç**' i seçin.  

2. **Abonelikleri Seç** penceresinde, erişmek istemediğiniz aboneliklerin yanındaki onay kutularını temizleyin ve ardından **Kapat**' ı seçin.

## <a name="spark-console"></a>Spark konsolu

Spark yerel konsolu 'Nu (Scala) çalıştırabilir veya Spark Livy etkileşimli oturum konsolunu (Scala) çalıştırabilirsiniz.

### <a name="spark-local-consolescala"></a>Spark yerel Konsolu (Scala)

WINUTILS.EXE önkoşulu karşılatığınızdan emin olun.

1. Menü çubuğundan **Çalıştır**  >  **yapılandırma yapılandırma...**' ya gidin.

2. **Çalıştır/hata ayıkla yapılandırma** penceresinde, sol bölmedeki **HDInsight 'ta Apache Spark**' a gidin (  >  **HDInsight üzerinde Spark) MyApp**.

3. Ana pencereden **`Locally Run`** sekmeyi seçin.

4. Aşağıdaki değerleri girip **Tamam**' ı seçin:

    |Özellik |Değer |
    |----|----|
    |İş ana sınıfı|Varsayılan değer, seçili dosyadaki ana sınıftır. Üç noktayı (**...**) seçerek sınıfı değiştirebilirsiniz  ve başka bir sınıf seçebilirsiniz.|
    |Ortam değişkenleri|HADOOP_HOME değerinin doğru olduğundan emin olun.|
    |WINUTILS.exe konumu|Yolun doğru olduğundan emin olun.|

    ![Yerel konsol kümesi yapılandırması](./media/apache-spark-intellij-tool-plugin/console-set-configuration.png)

5. Projeden **MyApp**  >  **src**  >  **Main**  >  **Scala**  >  **MyApp** sayfasına gidin.  

6. Menü çubuğundan **Araçlar**  >  **Spark konsolu**  >  **Spark yerel konsoluna (Scala)** gidin.

7. Ardından, bağımlılıkları otomatik olarak onarmak isteyip istemediğinizi sormak için iki iletişim kutusu görüntülenebilir. Öyleyse **otomatik çözüm**' i seçin.

    ![IntelliJ fıkır Spark otomatik çözüm Dialog1](./media/apache-spark-intellij-tool-plugin/intellij-console-autofix1.png)

    ![IntelliJ fıkır Spark otomatik çözüm dialog2](./media/apache-spark-intellij-tool-plugin/intellij-console-autofix2.png)

8. Konsol aşağıdaki resme benzer görünmelidir. Konsol penceresi yazın `sc.appName` ve ardından CTRL + ENTER tuşlarına basın.  Sonuç görüntülenecektir. Yerel konsolu kırmızı düğme ' ye tıklayarak sona bırakabilirsiniz.

    ![IntelliJ fıkır yerel konsol sonucu](./media/apache-spark-intellij-tool-plugin/local-console-result.png)

### <a name="spark-livy-interactive-session-consolescala"></a>Spark Livy etkileşimli oturum Konsolu (Scala)

1. Menü çubuğundan **Çalıştır**  >  **yapılandırma yapılandırma...**' ya gidin.

2. **Çalıştır/hata ayıkla yapılandırma** penceresinde, sol bölmedeki **HDInsight 'ta Apache Spark**' a gidin (  >  **HDInsight üzerinde Spark) MyApp**.

3. Ana pencereden **`Remotely Run in Cluster`** sekmeyi seçin.

4. Aşağıdaki değerleri girip **Tamam**' ı seçin:

    |Özellik |Değer |
    |----|----|
    |Spark kümeleri (yalnızca Linux)|Uygulamanızı çalıştırmak istediğiniz HDInsight Spark kümesini seçin.|
    |Ana sınıf adı|Varsayılan değer, seçili dosyadaki ana sınıftır. Üç noktayı (**...**) seçerek sınıfı değiştirebilirsiniz  ve başka bir sınıf seçebilirsiniz.|

    ![Etkileşimli konsol ayarlama yapılandırması](./media/apache-spark-intellij-tool-plugin/interactive-console-configuration.png)

5. Projeden **MyApp**  >  **src**  >  **Main**  >  **Scala**  >  **MyApp** sayfasına gidin.  

6. Menü çubuğundan **Araçlar**  >  **Spark konsolu**,  >  **Spark Livy etkileşimli oturum konsolu 'nu (Scala) çalıştırın**.

7. Konsol aşağıdaki resme benzer görünmelidir. Konsol penceresi yazın `sc.appName` ve ardından CTRL + ENTER tuşlarına basın.  Sonuç görüntülenecektir. Yerel konsolu kırmızı düğme ' ye tıklayarak sona bırakabilirsiniz.

    ![IntelliJ fıkır etkileşimli konsol sonucu](./media/apache-spark-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Seçimi Spark konsoluna gönder

Yerel konsola veya Livy etkileşimli oturum konsoluna (Scala) bazı kodlar göndererek betik sonucunu öngörülebilir bir şekilde görmeniz yararlı olur. Scala dosyasındaki bazı kodları vurgulayabilir, ardından **seçme konsolu 'na sağ tıklayarak seçim gönderebilirsiniz**. Seçili kod konsola gönderilir. Sonuç, konsolundaki koddan sonra görüntülenecektir. Varsa, konsol hataları kontrol eder.  

   ![Seçimi Spark konsoluna gönder](./media/apache-spark-intellij-tool-plugin/send-selection-to-console.png)

## <a name="integrate-with-hdinsight-identity-broker-hib"></a>HDInsight Identity broker (HIB) ile tümleştirme

### <a name="connect-to-your-hdinsight-esp-cluster-with-id-broker-hib"></a>HDInsight ESP kümenize KIMLIK broker (HIB) ile bağlanma

KIMLIK broker (HIB) ile HDInsight ESP kümenize bağlanmak için, Azure aboneliğinde oturum açmak üzere normal adımları izleyebilirsiniz. Oturum açtıktan sonra, Azure Explorer 'da küme listesini görürsünüz. Daha fazla yönerge için bkz. [HDInsight kümenize bağlanma](#connect-to-your-hdinsight-cluster).

### <a name="run-a-spark-scala-application-on-an-hdinsight-esp-cluster-with-id-broker-hib"></a>KIMLIK broker (HIB) ile bir HDInsight ESP kümesinde Spark Scala uygulaması çalıştırma

KIMLIK broker (HIB) ile HDInsight ESP kümesine iş göndermek için normal adımları izleyebilirsiniz. Daha fazla yönerge için bir [HDInsight Spark kümesinde Spark Scala uygulaması çalıştırma](#run-a-spark-scala-application-on-an-hdinsight-spark-cluster) bölümüne bakın.

Gerekli dosyaları, oturum açma hesabınızla adlı bir klasöre karşıya yüklememiz ve yükleme yolunu yapılandırma dosyasında görebiliriz.

   ![yapılandırmada yolu karşıya yükle](./media/apache-spark-intellij-tool-plugin/upload-path-in-the-configuration.png)

### <a name="spark-console-on-an-hdinsight-esp-cluster-with-id-broker-hib"></a>KIMLIK broker (HIB) ile HDInsight ESP kümesinde Spark konsolu

Spark yerel konsolu 'Nu (Scala) çalıştırabilir ya da KIMLIK broker (HIB) ile HDInsight ESP kümesinde Spark Livy etkileşimli oturum konsolunu (Scala) çalıştırabilirsiniz. Daha fazla yönerge için [Spark konsoluna](#spark-console) bakın.

   > [!NOTE]  
   > Kimlik broker (HIB) ile HDInsight ESP kümesi için, [bir küme bağlayın](#link-a-cluster) ve [uygulamaları uzaktan Apache Spark ayıklayın](#debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster) .

## <a name="reader-only-role"></a>Yalnızca okuyucu rolü

Kullanıcılar işi yalnızca okuyucu rolü izni olan bir kümeye gönderdiğinde, ambarı kimlik bilgileri gereklidir.

### <a name="link-cluster-from-context-menu"></a>Bağlam menüsünden küme bağlama

1. Yalnızca okuyucu rol hesabıyla oturum açın.

2. **Azure Explorer**'da, aboneliğinizdeki HDInsight kümelerini görüntülemek için **HDInsight** ' ı genişletin. **"Role: Reader"** olarak işaretlenen kümelerin yalnızca yalnızca okuyucu rolü izni vardır.

    ![' IntelliJ Azure gezgin rolü: okuyucu '](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer15.png)

3. Yalnızca okuyucu rolü izniyle kümeye sağ tıklayın. **Bu kümeyi** bağlama menüsünden Bağla ' yı seçin. Ambarı Kullanıcı adı ve parolasını girin.

    ![IntelliJ Azure Gezgini bu kümeyi bağla](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer11.png)

4. Küme başarıyla bağlanmışsa, HDInsight yenilenir.
   Kümenin aşaması bağlantılı hale gelir.
  
    ![IntelliJ Azure Explorer bağlantılı iletişim kutusu](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer8.png)

### <a name="link-cluster-by-expanding-jobs-node"></a>Işler düğümünü genişleterek küme bağlama

1. **İşler** düğümü ' ne tıklayın, **küme işi erişimi reddedildi** penceresi açılır.

2. **Bu kümeyi** bağlantı kümesine bağla ' ya tıklayın.

    ![küme işi erişimi reddedildi iletişim kutusu](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer9.png)

### <a name="link-cluster-from-rundebug-configurations-window"></a>Kümeyi çalıştırma/hata ayıklama yapılandırması penceresinden bağla

1. HDInsight yapılandırması oluşturun. Sonra **kümede uzaktan Çalıştır '** ı seçin.

2. Spark kümeleri için yalnızca okuyucu rolü iznine sahip olan bir küme seçin **(yalnızca Linux)**. Uyarı iletisi görüntülenir. **Bu kümeyi** bağlantı kümesine bağla ' ya tıklayabilirsiniz.

   ![IntelliJ fıkır çalıştırma/hata ayıklama yapılandırması oluşturma](./media/apache-spark-intellij-tool-plugin/create-configuration.png)

### <a name="view-storage-accounts"></a>Depolama hesaplarını görüntüleme

* Yalnızca okuyucu rolü iznine sahip kümeler için **depolama hesapları** düğümü ' ne tıklayın, **depolama erişimi reddedildi** penceresi açılır. Depolama Gezgini açmak için **Azure Depolama Gezgini aç** ' a tıklayabilirsiniz.

   ![' IntelliJ fıkır depolama erişimi reddedildi '](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer14.png)

   ![IntelliJ fıkır depolama erişimi reddedildi düğmesi](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer10.png)

* Bağlantılı kümeler için **depolama hesapları** düğümü ' ne tıklayın, **depolama erişimi reddedildi** penceresi açılır. Depolama Gezgini açmak için **Azure Storage 'ı aç** ' a tıklayabilirsiniz.

   ![' IntelliJ fıkır depolama erişimi Denied2 '](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer13.png)

   ![IntelliJ fıkır depolama erişimi Denied2 düğmesi](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer12.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Mevcut IntelliJ fıkır uygulamalarını kullanacak şekilde Dönüştür Azure Toolkit for IntelliJ

IntelliJ fıkır içinde oluşturduğunuz mevcut Spark Scala uygulamalarını Azure Toolkit for IntelliJ uyumlu olacak şekilde dönüştürebilirsiniz. Daha sonra, uygulamayı bir HDInsight Spark kümesine göndermek için eklentiyi kullanabilirsiniz.

1. IntelliJ fıkrı aracılığıyla oluşturulmuş mevcut bir Spark Scala uygulaması için ilişkili `.iml` dosyayı açın.

2. Kök düzeyinde, aşağıdaki metin gibi bir **Modül** öğesidir:

    ```xml
    <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">
    ```

   `UniqueKey="HDInsightTool"` **Modül** öğesinin aşağıdaki metin gibi görünmesi için eklenecek öğeyi düzenleyin:

    ```xml
    <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">
    ```

3. Değişiklikleri kaydedin. Uygulamanız artık Azure Toolkit for IntelliJ uyumlu olmalıdır. Projedeki proje adına sağ tıklayarak test edebilirsiniz. Açılır menüde artık **Spark uygulamasını HDInsight 'A gönder** seçeneği vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekecekseniz, oluşturduğunuz kümeyi aşağıdaki adımlarla silin:

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Üstteki **arama** kutusuna **HDInsight** yazın.

1. **Hizmetler** altında **HDInsight kümeleri** ' ni seçin.

1. Görüntülenen HDInsight kümeleri listesinde, bu makale için oluşturduğunuz kümenin yanındaki **...** seçeneğini belirleyin.

1. **Sil**’i seçin. **Evet**’i seçin.

![Azure portal HDInsight kümesini siler](./media/apache-spark-intellij-tool-plugin/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini Sil")

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, [Scala](https://www.scala-lang.org/)'da yazılan Apache Spark uygulamaları geliştirmek için Azure Toolkit for IntelliJ eklentisinin nasıl kullanılacağını öğrendiniz. Ardından bunları doğrudan IntelliJ tümleşik geliştirme ortamından (IDE) bir HDInsight Spark kümesine gönderdiniz. Apache Spark ' de kaydettiğiniz verilerin Power BI gibi bir bı Analytics aracına nasıl çekkullanılabileceğini görmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Power BI kullanarak Apache Spark verileri çözümleme](apache-spark-use-bi-tools.md)