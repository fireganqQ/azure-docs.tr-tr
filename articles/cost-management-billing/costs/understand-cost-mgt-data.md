---
title: Azure Maliyet Yönetimi verilerini anlama
description: Bu makale Azure Maliyet Yönetimi verilerini daha iyi anlamanıza ve işlenme, toplanma, gösterilme ve kapatılma sıklığı hakkında bilgi edinmenize yardımcı olur.
author: bandersmsft
ms.author: banders
ms.date: 01/17/2021
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: micflan
ms.custom: contperf-fy21q2
ms.openlocfilehash: ad099fc7dfcee168186ef5229785933f4b1c5a90
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100650890"
---
# <a name="understand-cost-management-data"></a>Maliyet Yönetimi verilerini anlama

Bu makale, Azure Maliyet Yönetimi'ndeki Azure maliyet ve kullanım verilerini daha iyi anlamanıza yardımcı olur. Verilerin hangi sıklıkta işlendiği, toplandığı, gösterildiği ve kapatıldığını açıklar. Azure kullanımı için aylık olarak faturalandırılırsınız. Faturalama dönemleri aylık dönemler olsa da döngü başlangıç ve bitiş tarihleri abonelik türüne göre farklılık gösterir. Maliyet Yönetimi'nin kullanım verilerini alma sıklığı birçok faktöre bağlıdır. Bu faktörlerden bazıları, verileri işleme süresi ve Azure hizmetlerinin kullanım bilgilerini faturalama sistemine gönderme sıklığıdır.

Maliyet Yönetimi, rezervasyonlar ve Kurumsal Anlaşma (EA) hesapları için üçüncü taraf teklifler dahil olmak üzere tüm kullanımları ve satın alma işlemlerini kapsar. Microsoft Müşteri Sözleşmesi hesapları ve kullandıkça öde ücretlerini kullanan bağımsız abonelikler yalnızca Azure ve Market hizmetlerinin kullanımını kapsar. Destek ve diğer maliyetler dahil değildir. Maliyetler, fatura oluşturulana kadar tahmini değerlerdir ve krediler dikkate alınmaz.

Yeni bir aboneliğiniz varsa Maliyet Yönetimi özelliklerini hemen kullanamazsınız. Maliyet Yönetimi özelliklerini kullanabilmek için 48 saat kadar beklemeniz gerekebilir.

## <a name="supported-microsoft-azure-offers"></a>Desteklenen Microsoft Azure teklifleri

Aşağıdaki bilgiler, Azure Maliyet Yönetimi'nde desteklenen güncel [Microsoft Azure tekliflerini](https://azure.microsoft.com/support/legal/offer-details/) göstermektedir. Azure teklifi, sahip olduğunuz Azure aboneliğinin türüdür. Veriler, **Veri kullanım başlangıcı** tarihinden itibaren Maliyet Yönetimi'nde kullanılabilir durumdadır. Bir aboneliğin farklı bir teklife geçmesi durumunda teklif değişiklik tarihinden önceki maliyetler kullanılamaz.

| **Kategori**  | **Teklif adı** | **Kota kimliği** | **Teklif numarası** | **Veri kullanım başlangıcı** |
| --- | --- | --- | --- | --- |
| **Azure Devlet Kurumları** | Azure Kamu Kurumsal                                                         | EnterpriseAgreement_2014-09-01 | MS-AZR-USGOV-0017P | Mayıs 2014<sup>1</sup> |
| **Azure Devlet Kurumları** | Azure Kamu Kullandıkça Öde | PayAsYouGo_2014-09-01 | MS-AZR-USGOV-0003P | 2 Ekim 2018<sup>2</sup> |
| **Kurumsal Anlaşma (EA)** | Kurumsal Geliştirme ve Test                                                        | MSDNDevTest_2014-09-01 | MS-AZR-0148P | Mayıs 2014<sup>1</sup> |
| **Kurumsal Anlaşma (EA)** | Microsoft Azure Kurumsal | EnterpriseAgreement_2014-09-01 | MS-AZR-0017P | Mayıs 2014<sup>1</sup> |
| **Microsoft Müşteri Sözleşmesi** | Microsoft Azure Planı | EnterpriseAgreement_2014-09-01 | Yok | Mart 2019<sup>3</sup> |
| **Microsoft Müşteri Sözleşmesi** | Geliştirme ve Test için Microsoft Azure Planı | MSDNDevTest_2014-09-01 | Yok | Mart 2019<sup>3</sup> |
| **İş ortakları tarafından desteklenen Microsoft Müşteri Sözleşmesi** | Microsoft Azure Planı | CSP_2015-05-01, CSP_MG_2017-12-01 ve CSPDEVTEST_2018-05-01<br><br>Kota kimliği, Microsoft Müşteri Sözleşmesi ve eski CSP abonelikleri için yeniden kullanılır. Şu anda yalnızca Microsoft Müşteri Sözleşmesi abonelikleri desteklenir. | Yok | Ekim 2019 |
| **Microsoft Developer Network (MSDN)** | MSDN Platformları<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0062P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | Kullandıkça Öde                  | PayAsYouGo_2014-09-01 | MS-AZR-0003P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | Kullandıkça Öde Geliştirme ve Test         | MSDNDevTest_2014-09-01 | MS-AZR-0023P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | Microsoft İş Ortağı Ağı      | MPN_2014-09-01 | MS-AZR-0025P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | Ücretsiz Deneme<sup>4</sup>         | FreeTrial_2014-09-01 | MS-AZR-0044P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | Open ile Azure<sup>4</sup>      | AzureInOpen_2014-09-01 | MS-AZR-0111P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | Azure Pass<sup>4</sup>                                                            | AzurePass_2014-09-01 | MS-AZR-0120P, MS-AZR-0122P - MS-AZR-0125P, MS-AZR-0128P - MS-AZR-0130P | 2 Ekim 2018<sup>2</sup> |
| **Visual Studio** | Visual Studio Enterprise – MPN<sup>4</sup>     | MPN_2014-09-01 | MS-AZR-0029P | 2 Ekim 2018<sup>2</sup> |
| **Visual Studio** | Visual Studio Professional<sup>4</sup>         | MSDN_2014-09-01 | MS-AZR-0059P | 2 Ekim 2018<sup>2</sup> |
| **Visual Studio** | Visual Studio Test Professional<sup>4</sup>    | MSDNDevTest_2014-09-01 | MS-AZR-0060P | 2 Ekim 2018<sup>2</sup> |
| **Visual Studio** | Visual Studio Enterprise<sup>4</sup>           | MSDN_2014-09-01 | MS-AZR-0063P | 2 Ekim 2018<sup>2</sup> |
| **Visual Studio** | Visual Studio Enterprise: BizSpark<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0064P | 2 Ekim 2018<sup>2</sup> |

_<sup>**1**</sup> Mayıs 2014 öncesi veriler için [Azure Enterprise Portal](https://ea.azure.com)'ı ziyaret edin._

_<sup>**2 ekim**</sup> 2018 tarihinden önceki veriler için, genel hesapların [Azure Hesap Merkezi](https://account.azure.com/subscriptions) ve Azure Kamu hesapları için [Azure Hesap Merkezi gov](https://account.windowsazure.us/subscriptions) 'yi ziyaret edin._

_<sup>**3**</sup> Microsoft Müşteri Sözleşmeleri Mart 2019'da başlamıştır ve önceki dönemlere ait veri mevcut değildir._

_<sup>**4**</sup> Kredi tabanlı ve önceden öde abonelikler için geçmişe dönük veriler faturanızla eşleşmeyebilir. Aşağıdaki [Geçmişe dönük veriler faturayla eşleşmeyebilir](#historical-data-might-not-match-invoice) bölümünü inceleyin._

Aşağıdaki teklifler henüz desteklenmiyor:

| Kategori  | **Teklif adı** | **Kota kimliği** | **Teklif numarası** |
| --- | --- | --- | --- |
| **Azure Almanya** | Azure Almanya Kullandıkça Öde | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P |
| **Bulut Çözümü Sağlayıcısı (CSP)** | Microsoft Azure                                    | CSP_2015-05-01 | MS-AZR-0145P |
| **Bulut Çözümü Sağlayıcısı (CSP)** | Azure Kamu CSP                               | CSP_2015-05-01 | MS-AZR-USGOV-0145P |
| **Bulut Çözümü Sağlayıcısı (CSP)** | Microsoft Bulut Almanya için CSP'de Azure Almanya   | CSP_2015-05-01 | MS-AZR-DE-0145P |
| **Kullandıkça Öde**                 | Öğrenciler için Azure Başlangıç | DreamSpark_2015-02-01 | MS-AZR-0144P |
| **Kullandıkça Öde** | Öğrenciler için Azure<sup>4</sup> | AzureForStudents_2018-01-01 | MS-AZR-0170P |
| **Kullandıkça Öde**                 | Microsoft Azure Sponsorluğu | Sponsored_2016-01-01 | MS-AZR-0036P |
| **Destek Planları** | Standart destek                    | Default_2014-09-01 | MS-AZR-0041P |
| **Destek Planları** | Profesyonel Doğrudan destek         | Default_2014-09-01 | MS-AZR-0042P |
| **Destek Planları** | Geliştirici desteği                   | Default_2014-09-01 | MS-AZR-0043P |
| **Destek Planları** | Almanya destek planı                | Default_2014-09-01 | MS-AZR-DE-0043P |
| **Destek Planları** | Azure Kamu Standart Destek   | Default_2014-09-01 | MS-AZR-USGOV-0041P |
| **Destek Planları** | Azure Kamu Pro-Direct Destek | Default_2014-09-01 | MS-AZR-USGOV-0042P |
| **Destek Planları** | Azure Kamu Geliştirici Desteği  | Default_2014-09-01 | MS-AZR-USGOV-0043P |

### <a name="free-trial-to-pay-as-you-go-upgrade"></a>Ücretsiz denemeden kullandıkça öde planına yükseltme

Ücretsiz denemeden kullandıkça öde fiyatlandırmasına yükseltme yaptıktan sonra sunulan ücretsiz katman hizmetleri hakkında bilgi edinmek için bkz. [Azure ücretsiz hesabı hakkında SSS](https://azure.microsoft.com/free/free-account-faq/).

### <a name="determine-your-offer-type"></a>Teklif türünü belirleme

Abonelikle ilgili veri görmüyorsanız ve aboneliğinizin desteklenen tekliflerin arasında olup olmadığı öğrenmek istiyorsanız aboneliğinizin desteklenip desteklenmediğini doğrulayabilirsiniz. Bir Azure aboneliğinin desteklenip desteklenmediğini doğrulamak için Azure portalında oturum açın. Ardından sol taraftaki menü bölmesinden **Tüm Hizmetler**'i seçin. Hizmetler listesinde **Abonelikler**'i seçin. Abonelik listesi menüsünden doğrulamak istediğiniz aboneliği seçin. Aboneliğiniz Genel Bakış sekmesinde gösterilir ve **Teklif** ile **Teklif Kimliği** bilgilerini görebilirsiniz. Aşağıdaki resimde bir örnek gösterilir.

![Teklif ve Teklif Kimliği bilgilerinin gösterildiği Aboneliğe Genel Bakış sekmesi örneği](./media/understand-cost-mgt-data/offer-and-offer-id.png)

## <a name="costs-included-in-cost-management"></a>Maliyet Yönetimi'ne dahil olan maliyetler

Aşağıdaki tablolarda Maliyet Yönetimi'ne dahil olan ve olmayan veriler gösterilmiştir. Tüm maliyetler, fatura oluşturulana kadar tahmini değerlerdir. Gösterilen maliyetler ücretsiz ve ön ödemeli kredileri içermez.

| **Dahil** | **Dahil değil** |
| --- | --- |
| Azure hizmet kullanımı<sup>5</sup>        | Destek ücretleri - Daha fazla bilgi için bkz. [Fatura terimlerinin açıklaması](../understand/understand-invoice.md). |
| Market teklifi kullanımı<sup>6</sup> | Vergiler - Daha fazla bilgi için bkz. [Fatura terimlerinin açıklaması](../understand/understand-invoice.md). |
| Market satın alma işlemleri<sup>6</sup>      | Krediler - Daha fazla bilgi için bkz. [Fatura terimlerinin açıklaması](../understand/understand-invoice.md). |
| Rezervasyon satın alma işlemleri<sup>7</sup>      |  |
| Amorti edilmiş rezervasyon satın alma işlemleri<sup>7</sup>      |  |

_<sup>**5**</sup> Azure hizmeti kullanımı, rezervasyona ve anlaşma fiyatlarına göre belirlenir._

_<sup>**6**</sup> Şu anda MSDN ve Visual Studio için Market satın almaları sağlanmamaktadır._

_<sup>**7**</sup> Rezervasyon satın alma işlemleri şu an için yalnızca Kurumsal Anlaşma (EA) ve Microsoft Müşteri Sözleşmesi hesapları için kullanılabilir._

## <a name="how-tags-are-used-in-cost-and-usage-data"></a>Maliyet ve kullanım verilerinde etiket kullanımı

Azure Maliyet Yönetimi, ayrı hizmetler tarafından gönderilen kullanım kayıtlarıyla birlikte etiketleri de alır. Etiketler için aşağıdaki kısıtlamalar geçerlidir:

- Etiketlerin doğrudan kaynaklara uygulanması gerekir ve üst kaynak grubundaki etiketler devralınmaz.
- Kaynak etiketleri yalnızca kaynak gruplarına dağıtılmış olan kaynaklar için desteklenir.
- Dağıtılan kaynakların bazıları etiket desteği sunmayabilir veya kullanım verilerine etiket eklemeyebilir.
- Kaynak etiketleri yalnızca etiketin uygulanmış olduğu kullanım verilerine dahil edilir. Etiketler, verilere geçmişe dönük olarak uygulanmaz.
- Kaynak etiketleri Maliyet Yönetimi'nde yalnızca veriler yenilendikten sonra kullanılabilir.
- Kaynak etiketleri Maliyet Yönetimi'nde yalnızca kaynak etkin/çalışır durumda olduğunda ve kullanım kaydı oluşturduğunda kullanılabilir. Örneğin VM'nin serbest bırakılmış olduğu durumlarda kullanılır.
- Etiketlerin yönetilmesi için her bir kaynakta katkıda bulunan erişimi gerekir.
- Etiket ilkelerinin yönetilmesi için yönetim grubu, abonelik veya kaynak grubu düzeyinde sahip veya ilke katkıda bulunanı erişimi gerekir.
    
Maliyet Yönetimi'nde belirli bir etiketi göremiyorsanız şu soruları göz önünde bulundurun:

- Etiket doğrudan kaynağa mı uygulandı?
- Etiketin uygulanmasının üzerinden 24 saat geçti mi?
- Kaynak türü etiketleri destekliyor mu? Aşağıdaki kaynak türleri 1 Aralık 2019’dan itibaren kullanım verilerinde etiketleri desteklememektedir. Desteklenen türlerin tam listesi için bkz. [Azure kaynakları için etiket desteği](../../azure-resource-manager/management/tag-support.md).
    - Azure Active Directory B2C Dizinleri
    - Azure Bastion
    - Azure Güvenlik Duvarları
    - Azure NetApp Files
    - Data Factory
    - Databricks
    - Yük dengeleyiciler
    - Ağ İzleyicisi
    - Notification Hubs
    - Service Bus
    - Time Series Insights
    
Aşağıda, etiketlerle çalışma konusunda birkaç ipucu verilmiştir:

- Uzun vadeli planlar yapın ve maliyetleri kuruluş, uygulama ve ortam gibi kaynaklara göre ayırmanızı mümkün kılacak bir etiketleme stratejisi tanımlayın.
- Azure İlkesi'ni kullanarak kaynak grubu etiketlerini kaynaklara kopyalayın ve etiketleme stratejinizi uygulayın.
- Geçerli etiketlere göre maliyetlere ulaşmak için Query veya UsageDetails ile Tags API'sini kullanın.

## <a name="cost-and-usage-data-updates-and-retention"></a>Maliyet ile kullanım verilerinin güncelleştirilmesi ve saklama

Maliyet ve kullanım verileri genellikle 8 ila 24 saat arasında, Azure portalının Maliyet Yönetimi + Faturalandırma bölümünden ve destekleyen API’lerden kullanılabilir. Maliyetleri incelerken şu noktaları dikkate alın:

- Her Azure hizmeti (Depolama, İşlem ve SQL gibi) kullanımı farklı aralıklara yayar. Bazı hizmetlere ait verileri diğerlerinden daha önce görebilirsiniz.
- Geçerli faturalama dönemine ait tahmini ücretler günde altı kez güncelleştirilir.
- Geçerli faturalama dönemine ait tahmini ücretler, kullanım arttıkça değişebilir.
- Güncelleştirmeler birikmelidir ve her biri, bir önceki güncelleştirmede bulunan satır öğelerinin ve bilgilerinin tamamını içerir.
- Azure, faturalama dönemi sona erdikten 72 saat (üç takvim günü) sonra geçerli faturalama dönemini sonlandırır veya _kapatır_.
- Açık ay (faturalanmamış) döneminde maliyet yönetimi verileri yalnızca tahmin olarak düşünülmelidir. Bazı durumlarda kullanım gerçekleştikten sonra ücretlerin sisteme ulaşmasında gecikme olabilir.

Aşağıdaki örnekte faturalama döneminin nasıl bitebileceği gösterilmiştir:

* Kurumsal Anlaşma (EA) abonelikleri: Faturalama ayının 31 Mart tarihinde sona ermesi halinde tahmini ücretler 72 saat sonrasına kadar güncelleştirilir. Bu örnekte 4 Nisan gece yarısı (UTC).
* Kullandıkça öde abonelikleri: Faturalama ayının 15 Mart tarihinde sona ermesi halinde tahmini ücretler 72 saat sonrasına kadar güncelleştirilebilir. Bu örnekte 19 Mayıs gece yarısı (UTC).

Maliyet ve kullanım verileri Maliyet Yönetimi ve Faturalama bölümünde yer alır ve en az yedi yıl boyunca saklanır.

### <a name="rerated-data"></a>Yeniden ücretlendirilen veriler

Veri almak için maliyet yönetimi API 'Lerini, Power BI veya Azure portal kullanıp kullanmayacağınızı, geçerli fatura döneminin ücretlendirmelerinin yeniden yönlendirilmesine beklemeniz gerekir. Ücretler, fatura kapatılana kadar değişebilir.

## <a name="cost-rounding"></a>Maliyet yuvarlama

Maliyet Yönetimi’nde gösterilen ücretler yuvarlanır. Sorgu API’si tarafından döndürülen ücretler yuvarlanmaz. Örneğin:

- Azure portalında maliyet analizi - Ücretler, standart yuvarlama kuralları kullanılarak yuvarlanır. 0,5’ten daha yüksek değerler yukarı, diğer değerler ise aşağı yuvarlanır. Yuvarlama, yalnızca değerler gösterildiğinde gerçekleşir. Yuvarlama veri işleme ve toplama işlemi esnasında gerçekleşmez. Örneğin, maliyet analizi maliyetleri şu şekilde toplar:
  -    Ücret 1: 0,004 ABD doları
  - Ücret 2: 0,004 ABD doları
  -    İşlenen toplam ücret: 0,004 + 0,004 = 0,008. Gösterilen ücret 0,01 ABD dolarıdır.
- Sorgu API’si - Ücretler sekiz ondalık basamak ile gösterilir ve yuvarlama yapılmaz.

## <a name="historical-data-might-not-match-invoice"></a>Geçmişe dönük veriler faturayla eşleşmeyebilir

Kredi tabanlı ve önceden öde teklifleri için geçmişe dönük veriler faturanızla eşleşmeyebilir. Azure kullandıkça öde, MSDN ve Visual Studio teklifleri faturalarına Azure kredisi ve avans ödeme yansıtılabilir. Maliyet yönetimi 'nde gösterilen geçmiş verileri, yalnızca tahmini tüketim ücretlerine göre belirlenir. Maliyet Yönetimi'ndeki geçmişe dönük verilerde ödemeler ve krediler bulunmaz. Aşağıdaki teklifler için gösterilen geçmiş verileri, faturanızdan tam olarak eşleşmeyebilir.

- Öğrenciler için Azure (MS-AZR-0170P)
- Open ile Azure (MS-AZR-0111P)
- Azure Pass (MS-AZR-0120P, MS-AZR-0123P, MS-AZR-0125P, MS-AZR-0128P, MS-AZR-0129P)
- Ücretsiz Deneme (MS-AZR-0044P)
- MSDN (MS-AZR-0062P)
- Visual Studio (MS-AZR-0029P, MS-AZR-0059P, MS-AZR-0060P, MS-AZR-0063P, MS-AZR-0064P)

## <a name="next-steps"></a>Sonraki adımlar

- Maliyet Yönetimi için ilk hızlı başlangıcı önceden tamamlamadıysanız, [Maliyetleri analiz etmeye başlama](./quick-acm-cost-analysis.md) bölümünden bilgi edinin.