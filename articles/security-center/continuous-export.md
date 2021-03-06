---
title: Sürekli dışarı aktarma, Azure Güvenlik Merkezi 'nin uyarılarını ve önerilerini Log Analytics çalışma alanlarına veya Azure Event Hubs gönderebilir
description: Log Analytics çalışma alanlarına veya Azure Event Hubs sürekli güvenlik uyarılarını ve önerilerini yapılandırmayı öğrenin
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 12/24/2020
ms.author: memildin
ms.openlocfilehash: 9b8dc635781c96dcbd7aa423c77f60ff0556bd71
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100634077"
---
# <a name="continuously-export-security-center-data"></a>Güvenlik Merkezi verilerini sürekli dışa aktarma

Azure Güvenlik Merkezi, ayrıntılı güvenlik uyarıları ve önerileri oluşturur. Bunları portalda veya programlı araçlar aracılığıyla görüntüleyebilirsiniz. Ayrıca, bu bilgilerin bir kısmını veya tümünü ortamınızdaki diğer izleme araçlarıyla izlemek için dışarı aktarmanız gerekebilir. 

**Sürekli dışarı aktarma** , nelerin dışa aktarılacağını ve *nerede* *gidebileceklerini* tamamen özelleştirmenizi sağlar. Örneğin, bunu şu şekilde yapılandırabilirsiniz:

- Tüm yüksek önem derecesi uyarıları bir Azure Olay Hub 'ına gönderilir
- SQL sunucularınızın güvenlik açığı değerlendirme taramalarının tüm Orta veya yüksek önem dereceleri, belirli bir Log Analytics çalışma alanına gönderilir
- Belirli öneriler, her oluşturulduğunda bir olay hub 'ına veya Log Analytics çalışma alanına teslim edilir 
- Bir denetimin puanı 0,01 veya daha fazla değişiklik yaptığı zaman bir aboneliğin güvenli puanı Log Analytics bir çalışma alanına gönderilir 

Özellik *sürekli* olarak adlandırılsa da, güvenli Puanlama veya mevzuat uyumluluk verilerinin haftalık anlık görüntülerini dışarı aktarma seçeneği de vardır.

Bu makalede Log Analytics çalışma alanlarına veya Azure Event Hubs sürekli dışarı aktarmanın nasıl yapılandırılacağı açıklanır.

> [!NOTE]
> Güvenlik merkezini SıEM ile tümleştirmeniz gerekiyorsa, bkz. [BIR SIEM, SOAR veya BT hizmet yönetimi çözümüne akış uyarıları](export-to-siem.md).

> [!TIP]
> Güvenlik Merkezi, CSV 'ye bir kerelik bir el ile dışarı aktarma seçeneği de sunar. [Uyarıların ve önerilerin el ile tek seferlik dışarı aktarılması](#manual-one-time-export-of-alerts-and-recommendations)hakkında daha fazla bilgi edinin.


## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel kullanılabilirlik (GA)|
|Fiyat|Ücretsiz|
|Gerekli roller ve izinler:|<ul><li>Kaynak grubundaki **Güvenlik Yöneticisi** veya **sahibi**</li><li>Hedef kaynak için yazma izinleri</li><li>Aşağıda açıklanan Azure Ilkesi ' DeployIfNotExist ' ilkelerini kullanıyorsanız, ilke atama izinlerine de ihtiyacınız olacaktır</li></ul>|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) US Gov, diğer gov<br>![Yes](./media/icons/yes-icon.png) Çin gov (Olay Hub 'ına)|
|||


## <a name="what-data-types-can-be-exported"></a>Hangi veri türleri verilebilirler?

Sürekli dışarı aktarma, her değiştiğinde aşağıdaki veri türlerini dışarı aktarabilir:

- Güvenlik uyarıları
- Güvenlik önerileri 
- Güvenlik açığı değerlendirme tarayıcılarından veya belirli sistem güncelleştirmelerinden bulguları gibi ' Sub ' önerileri olarak düşünülebilir. Bunları "Ana güncelleştirmeler" makinelerinizde yüklü olmalıdır "gibi ' üst ' önerilerine dahil etmek için seçeneğini belirleyebilirsiniz.
- Güvenli puan (abonelik başına veya denetim başına)
- Mevzuat uyumluluk verileri

> [!NOTE]
> Güvenli puan ve mevzuat uyumluluk verilerinin dışarı aktarılması bir önizleme özelliğidir ve kamu bulutlarında kullanılamaz. 

## <a name="set-up-a-continuous-export"></a>Sürekli dışarı aktarma ayarlama 

Azure portal, güvenlik merkezi REST API aracılığıyla veya sağlanan Azure Ilke şablonlarını kullanarak ölçekteki Güvenlik Merkezi sayfalarından sürekli dışarı aktarmayı yapılandırabilirsiniz. Her birinin ayrıntıları için aşağıdaki uygun sekmeyi seçin.

### <a name="use-the-azure-portal"></a>[**Azure portalını kullanma**](#tab/azure-portal)

### <a name="configure-continuous-export-from-the-security-center-pages-in-azure-portal"></a>Azure portal 'de Güvenlik Merkezi sayfalarından sürekli dışarı aktarmayı yapılandırma

Aşağıdaki adımlar Log Analytics çalışma alanına veya Azure Event Hubs sürekli bir dışarı aktarma işlemi yapılıp yapılmayacağını belirtir.

1. Güvenlik Merkezi 'nin kenar çubuğundan **fiyatlandırma & ayarları**' nı seçin.
1. Veri dışarı aktarmayı yapılandırmak istediğiniz belirli aboneliği seçin.
1. Bu aboneliğin Ayarlar sayfasının kenar çubuğundan **sürekli dışarı aktarma**' yı seçin.

    :::image type="content" source="./media/continuous-export/continuous-export-options-page.png" alt-text="Azure Güvenlik Merkezi 'nde dışarı aktarma seçenekleri":::

    Burada dışa aktarma seçeneklerini görürsünüz. Kullanılabilir her dışa aktarma hedefi için bir sekme vardır. 

1. Dışarı aktarmak istediğiniz veri türünü seçin ve her bir türdeki filtrelerden birini seçin (örneğin, yalnızca yüksek önem derecesine sahip uyarıları dışarı aktarın).
1. Uygun dışarı aktarma sıklığını seçin:
    - **Akış** – bir kaynağın sistem durumu güncelleştirildiğinde değerlendirmeler gerçek zamanlı olarak gönderilir (hiçbir güncelleştirme gerçekleşmezse, hiçbir veri gönderilmez).
    - **Anlık görüntüler** : tüm yasal uyumluluk değerlendirmelerinin geçerli durumunun bir anlık görüntüsü her hafta gönderilir (Bu, güvenli puanlar ve mevzuat uyumluluk verilerinin haftalık anlık görüntüleri için bir önizleme özelliğidir).

1. İsteğe bağlı olarak, seçiminiz Bu önerilerden birini içeriyorsa, güvenlik açığı değerlendirmesi bulgularını bunlarla birlikte dahil edebilirsiniz:
    - SQL veritabanlarındaki güvenlik açığı değerlendirmesi bulguları düzeltildi
    - Makinelerdeki SQL sunucularınızda bulunan güvenlik açığı değerlendirmesi (Önizleme) düzeltilmelidir.
    - Azure Container Registry görüntülerdeki güvenlik açıkları düzeltilmelidir (Qualys tarafından desteklenir)
    - Sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir
    - Makinelerinize sistem güncelleştirmeleri yüklenmelidir

    Bu önerilerin bulgularını dahil etmek için **güvenlik bulgularını dahil et** seçeneğini etkinleştirin.

    :::image type="content" source="./media/continuous-export/include-security-findings-toggle.png" alt-text="Sürekli dışa aktarma yapılandırmasında güvenlik bulgularını dahil et" :::

1. "Dışarı aktarma hedefi" alanından, verilerin kaydedilmesini istediğiniz yeri seçin. Veriler farklı bir abonelikteki hedefe kaydedilebilir (örneğin, merkezi bir olay hub 'ı örneği veya merkezi bir Log Analytics çalışma alanı).
1. **Kaydet**’i seçin.

### <a name="use-the-rest-api"></a>[**REST API’sini kullanma**](#tab/rest-api)

### <a name="configure-continuous-export-using-the-rest-api"></a>REST API kullanarak sürekli dışarı aktarmayı yapılandırma

Sürekli dışarı aktarma, Azure Güvenlik Merkezi tahmin [API 'si](/rest/api/securitycenter/automations)aracılığıyla yapılandırılabilir ve yönetilebilir. Aşağıdaki olası hedeflere herhangi birine dışarı aktarmaya yönelik kurallar oluşturmak veya güncelleştirmek için bu API 'yi kullanın:

- Azure Event Hub
- Log Analytics çalışma alanı
- Azure Logic Apps 

API, Azure portal kullanılamayan ek işlevler sağlar, örneğin:

* **Daha büyük birim** -API, tek bir abonelikte birden çok dışa aktarma yapılandırması oluşturmanıza olanak sağlar. Güvenlik Merkezi 'nin Portal Kullanıcı arabirimindeki **sürekli dışarı aktarma** sayfası, her abonelik için yalnızca bir dışarı aktarma yapılandırmasını destekler.

* **Ek özellikler** -API, Kullanıcı arabiriminde görüntülenmeyen ek parametreler sunar. Örneğin, Otomasyon kaynağına Etiketler ekleyebilir ve ayrıca, güvenlik merkezi 'nin Portal Kullanıcı arabirimindeki **sürekli dışarı aktarma** sayfasında sunulmadan daha geniş bir uyarı ve öneri özellikleri kümesine göre dışarı aktarma işlemini tanımlayabilirsiniz.

* **Daha odaklı kapsam** -API, dışa aktarma yapılandırmalarınızın kapsamı için daha ayrıntılı bir düzey sağlar. API ile bir dışarı aktarma tanımlarken, bunu kaynak grubu düzeyinde yapabilirsiniz. Güvenlik Merkezi 'nin Portal Kullanıcı arabiriminde **sürekli dışarı aktarma** sayfasını kullanıyorsanız, bunu abonelik düzeyinde tanımlamanız gerekir.

    > [!TIP]
    > API kullanarak birden çok dışa aktarma yapılandırması ayarladıysanız veya yalnızca API parametreleri kullandıysanız, bu ek özellikler Güvenlik Merkezi Kullanıcı arabiriminde gösterilmez. Bunun yerine, diğer yapılandırmaların var olduğunu bildiren bir başlık görüntülenir.

[REST API belgelerindeki](/rest/api/securitycenter/automations)akışlarını otomatikleştirin API 'si hakkında daha fazla bilgi edinin.





### <a name="deploy-at-scale-with-azure-policy"></a>[**Azure Ilkesiyle ölçekli olarak dağıtın**](#tab/azure-policy)

### <a name="configure-continuous-export-at-scale-using-the-supplied-policies"></a>Sağlanan ilkeleri kullanarak sürekli dışarı aktarmayı uygun ölçekte yapılandırın

Kuruluşunuzun izleme ve olay yanıtı süreçlerini otomatik hale getirmek, güvenlik olaylarını araştırmak ve azaltmak için gereken süreyi büyük ölçüde iyileştirebilir.

Kuruluşunuzda sürekli dışarı aktarma yapılandırmalarınızı dağıtmak için, aşağıdaki açıklanan Azure Ilkesi ' DeployIfNotExist ' ilkelerini kullanarak sürekli dışarı aktarma yordamları oluşturun ve yapılandırın.

**Bu ilkeleri uygulamak için**

1. Aşağıdaki tablodan, uygulamak istediğiniz ilkeyi seçin:

    |Hedef  |İlke  |İlke KIMLIĞI  |
    |---------|---------|---------|
    |Olay Hub 'ına sürekli dışarı aktarma|[Azure Güvenlik Merkezi uyarıları ve önerileri için Olay Hub'ına dışarı aktarma dağıtımı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fcdfcce10-4578-4ecd-9703-530938e4abcb)|cdfcce10-4578-4ecd-9703-530938e4abcb|
    |Log Analytics çalışma alanına sürekli dışarı aktarma|[Azure Güvenlik Merkezi uyarıları ve önerileri için Log Analytics çalışma alanına dışarı aktarma dağıtımı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fffb6f416-7bd2-4488-8828-56585fef2be9)|ffb6f416-7bd2-4488-8828-56585fef2be9|
    ||||

    > [!TIP]
    > Ayrıca, Azure Ilkesi 'ni arayarak bunları bulabilirsiniz:
    > 1. Azure Ilkesini açın.
    > :::image type="content" source="./media/continuous-export/opening-azure-policy.png" alt-text="Azure Ilkesine erişme":::
    > 2. Azure Ilke menüsünden **tanımlar** ' ı seçin ve adları ada göre arayın. 

1. İlgili Azure Ilkesi sayfasında **ata**' yı seçin.
    :::image type="content" source="./media/continuous-export/export-policy-assign.png" alt-text="Azure Ilkesi atama":::

1. Her sekmeyi açın ve parametreleri istediğiniz şekilde ayarlayın:
    1. **Temel bilgiler** sekmesinde, ilke kapsamını ayarlayın. Merkezi Yönetim 'i kullanmak için, ilkeyi sürekli dışa aktarma yapılandırması kullanacak olan abonelikleri içeren yönetim grubuna atayın. 
    1. **Parametreler** sekmesinde, kaynak grubu ve veri türü ayrıntılarını ayarlayın. 
        > [!TIP]
        > Her parametrenin size sunulan seçenekleri açıklayan bir araç ipucu vardır.
        >
        > Azure Ilkesinin Parametreler sekmesi (1), güvenlik merkezi 'nin sürekli dışa aktarma sayfası (2) olarak benzer yapılandırma seçeneklerine erişim sağlar.
        > :::image type="content" source="./media/continuous-export/azure-policy-next-to-continuous-export.png" alt-text="Azure Ilkesiyle sürekli dışarı aktarmada parametreleri karşılaştırma" lightbox="./media/continuous-export/azure-policy-next-to-continuous-export.png":::
    1. İsteğe bağlı olarak, bu atamayı mevcut aboneliklere uygulamak için **Düzeltme** sekmesini açın ve bir düzeltme görevi oluşturma seçeneğini belirleyin.
1. Özet sayfasını gözden geçirin ve **Oluştur**' u seçin.

--- 

## <a name="information-about-exporting-to-a-log-analytics-workspace"></a>Log Analytics çalışma alanına aktarma hakkında bilgi

Azure Güvenlik Merkezi verilerini bir Log Analytics çalışma alanı içinde çözümlemek veya Azure uyarılarını Güvenlik Merkezi uyarılarla birlikte kullanmak istiyorsanız, Log Analytics çalışma alanınıza sürekli dışarı aktarma ayarlayın.

### <a name="log-analytics-tables-and-schemas"></a>Log Analytics tabloları ve şemaları

Güvenlik uyarıları ve önerileri sırasıyla *Securityalert* ve *securityöneriler* tablolarında depolanır. 

Bu tabloları içeren Log Analytics çözümünün adı, Azure Defender 'ın etkin olup olmamasına bağlıdır: güvenlik (' Güvenlik ve Denetim ') veya SecurityCenterFree. 

> [!TIP]
> Hedef çalışma alanındaki verileri görmek için, Bu çözümlerden birini **güvenlik ve denetim** veya **securitycenterfree**' ı etkinleştirmeniz gerekir.

![Log Analytics içindeki * SecurityAlert * tablosu](./media/continuous-export/log-analytics-securityalert-solution.png)

Aktarılmış veri türlerinin olay şemalarını görüntülemek için [Log Analytics tablo şemalarını](https://aka.ms/ASCAutomationSchemas)ziyaret edin.


##  <a name="view-exported-alerts-and-recommendations-in-azure-monitor"></a>Azure Izleyici 'de, verilmiş uyarıları ve önerileri görüntüleme

Ayrıca, [Azure izleyici](../azure-monitor/alerts/alerts-overview.md)'de, verilmiş güvenlik uyarılarını ve/veya önerilerini görüntülemeyi de tercih edebilirsiniz. 

Azure Izleyici, Log Analytics çalışma alanı sorgularını temel alan tanılama günlüğü, ölçüm uyarıları ve özel uyarılar dahil olmak üzere çeşitli Azure uyarıları için Birleşik bir uyarı deneyimi sağlar.

Azure Izleyici 'de Güvenlik Merkezi 'ndeki uyarıları ve önerileri görüntülemek için Log Analytics sorgularına göre bir uyarı kuralı yapılandırın (günlük uyarısı):

1. Azure Izleyici **uyarıları** sayfasında **Yeni uyarı kuralı**' nı seçin.

    ![Azure Izleyici 'nin uyarılar sayfası](./media/continuous-export/azure-monitor-alerts.png)

1. Kural Oluştur sayfasında, Yeni kuralınızı yapılandırın ( [Azure izleyici 'de bir günlük uyarısı kuralını](../azure-monitor/alerts/alerts-unified-log.md)yapılandırdığınız şekilde):

    * **Kaynak** için güvenlik uyarılarını ve önerilerini verdiğiniz Log Analytics çalışma alanını seçin.

    * **Koşul** için **özel günlük araması**' nı seçin. Görüntülenen sayfada sorguyu, geriye doğru dönemi ve sıklık dönemini yapılandırın. Log Analytics özelliğini sürekli dışarı aktarmayı etkinleştirerek, arama sorgusunda, güvenlik merkezi 'nin sürekli olarak dışa aktardığı veri türlerini sorgulamak için *Securityalert* veya *securityönerisi* yazabilirsiniz. 
    
    * İsteğe bağlı olarak, tetiklemek istediğiniz [Eylem grubunu](../azure-monitor/alerts/action-groups.md) yapılandırın. Eylem grupları e-posta gönderme, ıTSM biletleri, Web kancaları ve daha fazlasını tetikleyebilir.
    ![Azure Izleyici uyarı kuralı](./media/continuous-export/azure-monitor-alert-rule.png)

Artık yeni Azure Güvenlik Merkezi uyarılarını veya önerilerini (yapılandırılmış sürekli dışarı aktarma kurallarınıza ve Azure izleyici uyarı kuralınız ' nda tanımladığınız koşula bağlı olarak) Azure Izleyici uyarıları ' nda, bir eylem grubunun otomatik tetiklenimiyle (sağlanmışsa) görürsünüz.

## <a name="manual-one-time-export-of-alerts-and-recommendations"></a>Uyarıların ve önerilerin el ile tek seferlik dışarı aktarılması

Uyarılar veya öneriler için bir CSV raporu indirmek için, **güvenlik uyarıları** veya **öneriler** sayfasını açın ve **CSV raporu indir** düğmesini seçin.

:::image type="content" source="./media/continuous-export/download-alerts-csv.png" alt-text="Uyarı verilerini CSV dosyası olarak indir" lightbox="./media/continuous-export/download-alerts-csv.png":::

> [!NOTE]
> Bu raporlar Şu anda seçili olan aboneliklerdeki kaynaklara yönelik uyarıları ve önerileri içerir.


## <a name="faq---continuous-export"></a>SSS-sürekli dışarı aktarma

### <a name="what-are-the-costs-involved-in-exporting-data"></a>Verileri dışarı aktarma ile ilgili maliyetler nelerdir?

Sürekli dışarı aktarmayı etkinleştirmenin hiçbir maliyeti yoktur. Yapılandırmaya bağlı olarak, Log Analytics çalışma alanınızda verilerin alımı ve saklanması için maliyetler tahakkuk edebilir. 

[Log Analytics çalışma alanı fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/)hakkında daha fazla bilgi edinin.

[Azure Olay Hub 'ı fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/)hakkında daha fazla bilgi edinin.


### <a name="does-the-export-include-data-about-the-current-state-of-all-resources"></a>Dışarı aktarma, tüm kaynakların geçerli durumuyla ilgili veriler içeriyor mu?

Hayır. Sürekli dışarı aktarma, **olayların** akışı için oluşturulmuştur:

- Dışarı aktarma işlemi etkinleştirilmeden önce alınan **Uyarılar** dışarı aktarılmaz.
- Bir kaynağın uyumluluk durumu her değiştiğinde **öneriler** gönderilir. Örneğin, bir kaynak sağlıklı durumundan sağlıksız duruma döndüğünde. Bu nedenle, uyarılarda olduğu gibi, dışa aktarma işlemini etkinleştirdikten sonra durum değiştirmeyen kaynaklara yönelik öneriler dışarı aktarılmayacaktır.
- Güvenlik denetimi veya daha fazla 0,01 veya daha fazla bir güvenlik denetiminin puanı değiştiğinde **güvenli puan (Önizleme)** güvenlik denetimi veya abonelik için gönderilir. 
- Kaynak uyumluluğun durumu değiştiğinde **mevzuat uyumluluk durumu (Önizleme)** gönderilir.



### <a name="why-are-recommendations-sent-at-different-intervals"></a>Neden öneriler farklı aralıklarda gönderiliyor?

Farklı önerilerin farklı uyumluluk değerlendirme aralıkları vardır ve bu, birkaç dakikadan birkaç güne kadar farklılık gösterebilir. Sonuç olarak, öneriler dışarı aktarımlarınızın görünmesi için gereken süre miktarının farklılık gösterir.

### <a name="does-continuous-export-support-any-business-continuity-or-disaster-recovery-bcdr-scenarios"></a>Sürekli dışarı aktarma, iş sürekliliği veya olağanüstü durum kurtarma (BCDR) senaryolarını destekler mi?

Ortamınızı, hedef kaynağın bir kesinti veya başka bir olağanüstü durum yaşayan BCDR senaryolarında hazırlarken, Azure Event Hubs, Log Analytics çalışma alanı ve mantıksal uygulama yönergelerine göre yedeklemeler kurarak, kuruluşun veri kaybını önleme sorumluluğu vardır.

[Azure Event Hubs coğrafi olağanüstü durum kurtarma](../event-hubs/event-hubs-geo-dr.md)hakkında daha fazla bilgi edinin.


### <a name="is-continuous-export-available-with-azure-security-center-free"></a>Azure Güvenlik Merkezi Ücretsiz olarak sürekli dışarı aktarma kullanılabilir mi?

Evet! Birçok güvenlik merkezi uyarısı yalnızca Azure Defender 'ı etkinleştirdiğiniz zaman sağlandığını unutmayın. Verilen verilerinize alacağınız uyarıları önizlemenin iyi bir yolu, Azure portal Güvenlik Merkezi sayfalarında gösterilen uyarıları görebileceksiniz.



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, önerilerinizi ve uyarılarınızı sürekli dışarı aktarmaların nasıl yapılandırılacağını öğrendiniz. Ayrıca, uyarı verilerinizi bir CSV dosyası olarak nasıl indirileceğini öğrendiniz. 

İlgili malzemeler için aşağıdaki belgelere bakın: 

- [İş akışı Otomasyonu şablonları](https://github.com/Azure/Azure-Security-Center/tree/master/Workflow%20automation)hakkında daha fazla bilgi edinin.
- [Azure Event Hubs belgeleri](../event-hubs/index.yml)
- [Azure Sentinel belgeleri](../sentinel/index.yml)
- [Azure İzleyici belgeleri](../azure-monitor/index.yml)
- [Veri türlerini dışarı aktarma şemaları](https://aka.ms/ASCAutomationSchemas)