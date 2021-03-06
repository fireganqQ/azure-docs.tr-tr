---
title: Oracle Database Bağlan
description: Oracle Database REST API 'Leri ve Azure Logic Apps kayıtları ekleyin ve yönetin
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 05/20/2020
tags: connectors
ms.openlocfilehash: 91873a2d6a498712773bfe721653e64c3364666f
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92674817"
---
# <a name="get-started-with-the-oracle-database-connector"></a>Oracle Database bağlayıcısını kullanmaya başlama

Oracle Database bağlayıcısını kullanarak, mevcut veritabanınızdaki verileri kullanan kurumsal iş akışları oluşturursunuz. Bu bağlayıcı, Oracle Database yüklü bir şirket içi Oracle Database veya bir Azure sanal makinesine bağlanabilir. Bu bağlayıcı ile şunları yapabilirsiniz:

* Müşteriler veritabanına yeni bir müşteri ekleyerek veya bir Siparişler veritabanındaki bir siparişi güncelleştirerek iş akışınızı oluşturun.
* Bir veri satırı almak, yeni bir satır eklemek ve hatta silmek için eylemleri kullanın. Örneğin, Dynamics CRM Online 'da (bir tetikleyici) bir kayıt oluşturulduğunda bir Oracle Database (bir eylem) bir satır ekleyin. 

Bu bağlayıcı aşağıdaki öğeleri desteklemez:

* Görünümler 
* Bileşik anahtarlara sahip herhangi bir tablo
* Tablolardaki iç içe nesne türleri
* Skalar olmayan değerleri olan veritabanı işlevleri

Bu makalede, Oracle Database bağlayıcısının bir mantıksal uygulamada nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Desteklenen Oracle sürümleri: 
    * Oracle 9 ve üstü
    * Oracle istemci yazılımı 8.1.7 ve üstü

* Şirket içi veri ağ geçidini yükler. [Logic Apps 'ten şirket içi verilere bağlanma](../logic-apps/logic-apps-gateway-connection.md) adımları listelenir. Ağ geçidinin, bir şirket içi Oracle Database veya Oracle DB yüklü bir Azure VM 'ye bağlanması gerekir. 

    > [!NOTE]
    > Şirket içi veri ağ geçidi bir köprü görevi görür ve şirket içi veriler (bulutta olmayan veriler) ve mantıksal uygulamalarınız arasında güvenli bir veri aktarımı sağlar. Aynı ağ geçidi birden fazla hizmet ve birden çok veri kaynağı ile kullanılabilir. Bu nedenle, ağ geçidini yalnızca bir kez yüklemeniz gerekebilir.

* Şirket içi veri ağ geçidini yüklediğiniz makineye Oracle Istemcisini yükleme. Oracle 'dan .NET için 64-bit Oracle Veri Sağlayıcısı 'yi yüklediğinizden emin olun:  

  [Windows x64 için 64 bit ODAC 12c Sürüm 4 (12.1.0.2.4)](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > Oracle istemcisi yüklü değilse, bağlantıyı oluşturmaya veya kullanmaya çalıştığınızda bir hata oluşur. Bu makaledeki yaygın hatalara bakın.


## <a name="add-the-connector"></a>Bağlayıcıyı ekleme

> [!IMPORTANT]
> Bu bağlayıcının tetikleyicisi yok. Yalnızca eylemlere sahiptir. Bu nedenle mantıksal uygulamanızı oluştururken, **zamanlama-yinelenme** veya **Istek/yanıt yanıtı** gibi mantıksal uygulamanızı başlatmak için başka bir tetikleyici ekleyin. 

1. [Azure Portal](https://portal.azure.com), boş bir mantıksal uygulama oluşturun.

2. Mantıksal uygulamanızın başlangıcında, **istek/yanıt-istek** tetikleyicisi ' ni seçin: 

    ![Bir iletişim kutusu, tüm Tetiklerde arama yapmak için bir kutu içerir. Ayrıca, "Istek/yanıt-Istek" adlı ve seçim düğmesi ile gösterilen tek bir tetikleyici de vardır.](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. **Kaydet** ’i seçin. Kaydettiğinizde, otomatik olarak bir istek URL 'SI oluşturulur. 

4. **Yeni adım** ’ı ve **Eylem ekle** ’yi seçin. `oracle`Kullanılabilir eylemleri görmek için yazın: 

    ![Arama kutusu "Oracle" içerir. Arama "Oracle Database" etiketli bir vuruş üretir. Sekmeli bir sayfa, bir sekme "TETIKLEYICILER (0)", başka bir deyişle "eylemler (6)" gösteriliyor. Altı eylem listelenir. Bunlardan ilki "satır önizlemesini al".](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > Bu Ayrıca, tüm bağlayıcılar için kullanılabilir Tetikleyicileri ve eylemleri görmenin en hızlı yoludur. Bağlayıcı adının bir bölümünü (gibi) yazın `oracle` . Tasarımcı tüm Tetikleyicileri ve eylemleri listeler. 

5. **Oracle Database-al satırı** gibi eylemlerden birini seçin. Şirket **içi veri ağ geçidi üzerinden Bağlan '** ı seçin. Oracle sunucusu adı, kimlik doğrulama yöntemi, Kullanıcı adı, parola girin ve ağ geçidini seçin:

    ![İletişim kutusu "Oracle Database-satırı al" olarak adlandırılmış olur. "Şirket içi veri ağ geçidi üzerinden Bağlan" şeklinde işaretlenmiş bir kutu vardır. Aşağıda diğer beş metin kutusu bulunur.](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. Bağlandıktan sonra listeden bir tablo seçin ve tablonuza satır KIMLIĞINI girin. Tabloya tanımlayıcıyı bilmeniz gerekir. Bilmiyorsanız Oracle DB yöneticinize başvurun ve çıktıyı alın `select * from yourTableName` . Bu, size devam etmeniz için gereken bilgileri sağlar.

    Aşağıdaki örnekte, iş verileri bir Insan kaynakları veritabanından döndürülüyor: 

    !["Satırı al (Önizleme)" başlıklı iletişim kutusunda iki metin kutusu vardır: "H R ışlerı" içeren ve bir açılan listeye sahip olan ve "S A _ REP" içeren "Row ı d".](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. Bu sonraki adımda, iş akışınızı derlemek için diğer bağlayıcılardan herhangi birini kullanabilirsiniz. Oracle 'dan veri almayı test etmek istiyorsanız, Office 365 Outlook gibi e-posta bağlayıcılarından birini kullanarak kendi kendinize bir e-posta gönderin. `Subject`E-postanızı oluşturmak Için Oracle tablosundaki dinamik belirteçleri kullanın `Body` :

    ![İki iletişim kutusu vardır. "E-posta kutusu gönder", e-postanın "gövde", "konu" ve "to" adresini belirtmek için kutular içerir. "Dinamik içerik Ekle" iletişim kutusu, akışın uygulama ve hizmetlerinden dinamik içerik araması sağlar.](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. Mantıksal uygulamanızı **kaydedin** ve sonra **Çalıştır** ' ı seçin. Tasarımcıyı kapatın ve durum için çalıştırma geçmişine bakın. Başarısız olursa, başarısız ileti satırını seçin. Tasarımcı açılır ve hangi adımın başarısız olduğunu gösterir ve ayrıca hata bilgilerini gösterir. Başarılı olursa, eklediğiniz bilgileri içeren bir e-posta almalısınız.


### <a name="workflow-ideas"></a>İş akışı fikirleri

* #Oracle hashtag ' i izlemek ve bir veritabanına, sorgulanabilmeleri ve diğer uygulamalar içinde kullanmak için bu alanı yerleştirmek istiyorsunuz. Bir mantıksal uygulamada `Twitter - When a new tweet is posted` tetikleyiciyi ekleyin ve **#oracle** diyez etiketini girin. Sonra, eylemi ekleyin `Oracle Database - Insert row` ve tablonuzu seçin:

    !["Yeni bir tweet gönderildiğinde" iletişim kutusu, arama metni olarak "hashtag Oracle" ifadesini gösterir ve denetleme sıklığını belirtmenizi sağlar. Bu iletişim kutusu, eylemi seçmenize olanak sağlayan "Oracle Database" iletişim kutusuna yönlendirir.](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* İletiler bir Service Bus kuyruğuna gönderilir. Bu iletileri almak ve bir veritabanına koymak istiyorsunuz. Bir mantıksal uygulamada `Service Bus - when a message is received in a queue` tetikleyiciyi ekleyin ve kuyruğu seçin. Sonra, eylemi ekleyin `Oracle Database - Insert row` ve tablonuzu seçin:

    !["İleti alındığında..." iletişim kutusunda "kuyruk adı" olarak "Orders" gösterilir ve denetleme sıklığını belirtmenize olanak tanır. Bu kutu "Tablo adı" seçmenizi sağlayan "satır ekle (Önizleme)" iletişim kutusuna yol açar.](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a>Sık karşılaşılan hatalar

#### <a name="error-cannot-reach-the-gateway"></a>**Hata** : ağ geçidine ulaşılamıyor

**Neden** : şirket içi veri ağ geçidi buluta bağlanamıyor. 

**Risk azaltma** : ağ geçidinizin, yüklediğiniz şirket içi makinede çalıştığından ve internet 'e bağlanabildiğinden emin olun.  Ağ geçidini kapalı veya uyku moduna geçecek bir bilgisayara yüklememenizi öneririz. Şirket içi veri ağ geçidi hizmeti 'ni (Pbıegwservice) da yeniden başlatabilirsiniz.

#### <a name="error-the-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-see-httpsgomicrosoftcomfwlinkplinkid272376-to-install-the-official-provider"></a>**Hata** : kullanılan sağlayıcı kullanım dışı: ' System. Data. OracleClient, Oracle istemci yazılımı sürümü 8.1.7 veya daha üstünü gerektiriyor. '. [https://go.microsoft.com/fwlink/p/?LinkID=272376](/power-bi/connect-data/desktop-connect-oracle-database)Resmi sağlayıcıyı yüklemek için bkz..

**Neden** : Oracle istemci SDK 'sı, şirket içi veri ağ geçidinin çalıştığı makinede yüklü değil.  

**Çözüm** : Oracle istemci SDK 'sını, şirket içi veri ağ geçidiyle aynı bilgisayara indirin ve yükleyin.

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a>**Hata** : ' [TableName] ' tablosu hiçbir anahtar sütun tanımlamıyor

**Neden** : tabloda birincil anahtar yok.  

**Çözüm** : Oracle Database Bağlayıcısı, birincil anahtar sütunu olan bir tablonun kullanılmasını gerektirir.
 
## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Swagger 'da tanımlanan Tetikleyicileri ve eylemleri görüntüleyin ve ayrıca [bağlayıcı ayrıntılarında](/connectors/oracle/)tüm limitleri inceleyin. 

## <a name="get-some-help"></a>Yardım alın

[Azure Logic Apps Için Microsoft Q&soru sayfası](/answers/topics/azure-logic-apps.html) , sorularınızı sormak, soruları yanıtlamak ve diğer Logic Apps kullanıcıların ne yaptığını görmek için harika bir yerdir. 

Fikirlerinizi oylama ve göndererek Logic Apps ve bağlayıcıların artırılmasına yardımcı olabilirsiniz [https://aka.ms/logicapps-wish](https://aka.ms/logicapps-wish) . 


## <a name="next-steps"></a>Sonraki adımlar
[Bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md)ve [apı 'ler listesinde](apis-list.md)Logic Apps ' de kullanılabilir bağlayıcıları keşfedebilirsiniz.