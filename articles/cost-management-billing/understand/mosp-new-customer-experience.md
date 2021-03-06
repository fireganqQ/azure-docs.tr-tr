---
title: Güncelleştirilmiş Azure faturalama hesabınızı kullanmaya başlama
description: Yeni faturalama ve maliyet yönetimi deneyimindeki değişiklikleri anlamak için güncelleştirilmiş Azure faturalama hesabınızı kullanmaya başlayın
author: bandersmsft
ms.reviewer: amberbhargava
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 01/11/2021
ms.author: banders
ms.openlocfilehash: 887b7013eb3060020a39d2df0082768b8185bdde
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99575475"
---
# <a name="get-started-with-your-updated-azure-billing-account"></a>Güncelleştirilmiş Azure faturalama hesabınızı kullanmaya başlama

Maliyetleri ve faturaları yönetmek, bulut deneyiminizin temel bileşenlerinden biridir. Buluttaki harcamalarınızı denetlemenize ve anlamanıza yardımcı olur. Microsoft, maliyetlerinizi ve faturalarınızı yönetmenizi kolaylaştırmak için Azure faturalama hesabınızı gelişmiş maliyet yönetimi ve faturalama özellikleriyle güncelleştiriyor. Bu makalede faturalama hesabınıza gelen güncelleştirmeler açıklanır ve yeni özellikler adım adım gösterilir.

> [!IMPORTANT]
> Microsoft’tan hesabınıza yönelik güncelleştirmeler konusunda sizi bilgilendiren bir e-posta aldığınızda hesabınız güncelleştirilecektir. Bildirim, hesabınız güncelleştirilmeden 60 gün önce gönderilir.

## <a name="more-flexibility-with-your-new-billing-account"></a>Yeni faturalama hesabınızda daha fazla esneklik

Aşağıdaki diyagramda eski ve yeni faturalama hesabınız karşılaştırılır:

![Eski ve yeni hesap arasındaki faturalama hiyerarşisinin karşılaştırmasını gösteren diyagram](./media/mosp-new-customer-experience/comparison-old-new-account.png)

Yeni faturalama hesabınız, faturalarınızı ve ödeme yöntemlerinizi yönetmenize olanak sağlayan bir veya daha fazla faturalama profili içerir. Her faturalama profili, faturalama profilinin faturasındaki maliyetleri düzenlemenize olanak sağlayan bir veya daha fazla fatura bölümü içerir.

![Yeni faturalama hiyerarşisini gösteren diyagram](./media/mosp-new-customer-experience/new-billing-account-hierarchy.png)

Ödeme hesabındaki roller en yüksek izin düzeyine sahiptir. Bu roller, kuruluşta faturaları görüntülemesi ve hesabınızın tamamına ilişkin maliyetleri izlemesi gereken finans ve BT yöneticileri gibi kullanıcılara veya hesaba kaydolan bireylere atanmalıdır. Daha fazla bilgi için bkz. [ödeme hesabı rolleri ve görevleri](../manage/understand-mca-roles.md#billing-account-roles-and-tasks). Hesabınız güncelleştirildiğinde, eski faturalama hesabı üzerinde hesap yöneticisi rolüne sahip olan kullanıcıya yeni hesapta sahip rolü verilir.

## <a name="billing-profiles"></a>Faturalama profilleri

Faturalama profili, faturanızı ve ödeme yöntemlerinizi yönetmek için kullanılır. Hesabınızdaki her bir faturalama profili için ay başında aylık bir fatura oluşturulur. Fatura, faturalama profiliyle ilişkilendirilmiş tüm abonelikler için önceki aya ait ilgili ücretleri içerir.

Hesabınız güncelleştirildiğinde her abonelik için otomatik olarak bir faturalama profili oluşturulur. Aboneliğin ücretleri ilgili faturalama profiline faturalandırılır ve faturasında görüntülenir.

Faturalama profillerindeki roller, faturaları ve ödeme yöntemlerini görüntüleme ve yönetme izinlerine sahiptir. Bu roller, kuruluştaki muhasebe ekibinin üyeleri gibi faturaları ödeyen kullanıcılara atanmalıdır. Daha fazla bilgi için bkz. [faturalama profili rolleri ve görevleri](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks).

Hesabınız güncelleştirildiğinde, başkalarına [faturaları görüntüleme](download-azure-invoice.md#allow-others-to-download-your-subscription-invoice) izni verdiğiniz her abonelik için sahip, katkıda bulunan, okuyucu veya fatura okuyucusu Azure rolüne sahip kullanıcılara ilgili faturalama profilinde okuyucu rolü verilir.

## <a name="invoice-sections"></a>Fatura bölümleri

Fatura bölümü, faturanızdaki maliyetleri düzenlemek için kullanılır. Örneğin, tek bir faturaya ihtiyacınız olabilir ama maliyetleri departmana, ekibe veya projeye göre düzenlemek isteyebilirsiniz. Bu senaryoda, her bir bölüm, ekip veya proje için fatura bölümü oluşturduğunuz tek bir faturalama profiliniz vardır.

Hesabınız güncelleştirildiğinde her faturalama profili için bir fatura bölümü oluşturulur ve fatura bölümüne ilgili abonelik atanır. Daha fazla abonelik ekledikçe daha fazla bölümler oluşturabilir ve abonelikleri fatura bölümlerine atayabilirsiniz. Faturalama profilinizin faturasında, atadığınız her aboneliğin kullanımını yansıtan bölümler görürsünüz.

Fatura bölümündeki roller, kimlerin Azure abonelikleri oluşturduğunu denetleme iznine sahiptir. Roller, kuruluştaki ekipler için Azure ortamını ayarlayan mühendislik liderleri ve teknik mimarlar gibi kullanıcılara atanmalıdır. Daha fazla bilgi için bkz. [fatura bölümü rolleri ve görevleri](../manage/understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="enhanced-features-in-your-new-experience"></a>Yeni deneyiminizdeki iyileştirilmiş özellikler

Yeni deneyiminiz, maliyetlerinizi ve faturalarınızı yönetmenizi kolaylaştıran aşağıdaki maliyet yönetimi ve faturalama özelliklerini içerir:

#### <a name="invoice-management"></a>Fatura yönetimi

**Daha tahmin edilebilir aylık faturalama dönemi**: Yeni hesabınızda faturalama dönemi, Azure'ı kullanmak için ne zaman kaydolduğunuza bakılmaksızın ayın ilk günü başlar ve son günü biter. Her ayın başında bir fatura oluşturulur ve bu fatura önceki aydan gelen tüm ücretleri içerir.

**Birden çok abonelik için tek bir aylık fatura alın**: Aboneliklerinizden her biri için ayrı bir aylık fatura veya birden çok abonelik için tek bir fatura arasında seçim yapma esnekliğiniz vardır.

**Azure abonelikleri, destek planları ve Azure Market ürünleri için tek bir aylık fatura alın**: Azure aboneliklerinin kullanım ücretleri, destek planları ve Azure Market alışverişleri dahil tüm ücretler için tek bir aylık fatura alacaksınız.

**Maliyetleri faturanızda ihtiyaçlarınız temelinde gruplandırın**: Maliyetleri faturanızda ihtiyaçlarınız temelinde departmanlara, projelere veya ekiplere göre gruplandırabilirsiniz.

**Faturaya isteğe bağlı bir satın alma siparişi numarası ayarlayın**: Faturanızı iç finans sisteminizle ilişkilendirmek için bir satın alma siparişi numarası ayarlayın. Azure portalda istediğiniz zaman bu numarayı yönetin ve güncelleştirin.

#### <a name="payment-management"></a>Ödeme yönetimi

**Kredi kartı kullanarak faturaları hemen ödeyin**: Ücretin kredi kartınızdan otomatik ödemeyle alınmasını beklemek zorunda değilsiniz. Azure portalda, vadesi gelmiş veya vadesi geçmiş bir faturayı ödemek için herhangi bir kredi kartı kullanabilirsiniz.

**Hesaba uygulanan tüm ödemeleri izleyin**: Azure portalda hesabınıza uygulanan tüm ödemeleri, kullanılan ödeme yöntemi, ödeme tutarı ve ödeme tarihiyle birlikte görüntüleyin.

#### <a name="cost-management"></a>Maliyet yönetimi

**Kullanım verilerinin depolama hesabına aylık aktarımını zamanlayın**: Maliyet ve kullanım verilerinizi günlük, haftalık veya aylık olarak bir depolama hesabında otomatik şekilde yayımlayın.

#### <a name="account-and-subscription-management"></a>Hesap ve abonelik yönetimi

**Faturalama işlemlerini yapmaları için birden çok yönetici atayın**: Hesabınızın faturalamasını yönetmeleri için birden çok kullanıcıya faturalama izinleri atayın. Başkalarına okuma izni, yazma izni veya her ikisini de vererek esneklik elde edin.

**Doğrudan Azure portalda daha fazla abonelik oluşturun**: Azure portalda tüm aboneliklerinizi tek seçimle oluşturun.

#### <a name="api-support"></a>API desteği

**Faturalama ve maliyet yönetimi işlemlerini API'ler, SDK ve PowerShell aracılığıyla yapın**: Faturalama ve maliyet verilerini tercih ettiğiniz veri analizi araçlarına çekmek için maliyet yönetimi, faturalama ve tüketim API'lerini kullanın.

**Tüm abonelik işlemlerini API'ler, SDK ve PowerShell üzerinden yapın**: Azure aboneliklerinizin yönetimini otomatikleştirmek, örneğin aboneliği oluşturma, yeniden adlandırma ve iptal etme işlemlerini yapmak için Azure aboneliği API'lerini kullanın.

## <a name="get-prepared-for-your-new-experience"></a>Yeni deneyiminize hazır olun

Yeni deneyiminize hazır olmanız için şunları öneririz:

**Aylık faturalama dönemi ve farklı fatura tarihi**

Yeni deneyimde faturanız her ayın yaklaşık dokuzuncu günü oluşturulur ve önceki ayın tüm ücretlerini içerir. Bu tarih, eski hesabınızda faturanızın oluşturulduğu tarihten farklı olabilir. Faturalarınızı başkalarıyla paylaşıyorsanız tarih değişikliğini bu kişilere bildirin.

**Yeni faturalama ve maliyet yönetimi API'leri**

Faturalama ve maliyet verilerinizi sorgulayıp güncelleştirmek için Maliyet Yönetimi ve Faturalama API'lerini kullanıyorsanız artık yeni API'leri kullanmalısınız. Aşağıdaki tabloda yeni faturalama hesabında kullanılamayacak API'ler ve yeni faturalama hesabınızda yapmanız gereken değişiklikler listelenir.

|API | Değişiklikler  |
|---------|---------|
|[Faturalama Hesapları - Listeleme](/rest/api/billing/2019-10-01-preview/billingaccounts/list) | Faturalama Hesapları - Listeleme API'sinde eski faturalama hesabınızın agreementType ayarı **MicrosoftOnlineServiceProgram**'dır, yeni faturalama hesabınızın agreementType ayarı **MicrosoftCustomerAgreement** olur. agreementType üzerinde bir bağımlılığınız varsa bunu güncelleştirin. |
|[Faturalar - Faturalama Aboneliğine Göre Listeleme](/rest/api/billing/2019-10-01-preview/invoices/listbybillingsubscription)     | Bu API yalnızca hesabınız güncelleştirilmeden önce oluşturulmuş olan faturaları döndürür. Yeni faturalama hesabınızda oluşturulan faturaları almak için [Faturalar - Faturalama Hesabına Göre Listeleme](/rest/api/billing/2019-10-01-preview/invoices/listbybillingaccount) API'sini kullanmanız gerekebilir. |

## <a name="cost-management-updates-after-account-update"></a>Hesap güncelleştirmesi sonrasında Maliyet Yönetimi güncelleştirmeleri

Microsoft Müşteri Sözleşmesi için güncelleştirilen Azure faturalama hesabınız, Azure portalda kullandıkça öde hesabınızla erişemediğiniz yeni ve genişletilmiş Maliyet Yönetimi deneyimlerine erişmenizi sağlar.

### <a name="new-capabilities"></a>Yeni özellikler

Azure faturalama hesabınızla aşağıdaki yeni özellikler sağlanır.

#### <a name="new-billing-scopes"></a>Yeni faturalama kapsamları

Güncelleştirilmiş hesabınızın bir parçası olarak Maliyet Yönetimi + Faturalama'da yeni kapsamlarınız vardır. Bunlar hiyerarşik düzenleme ve faturalamaya yardımcı olmanın yanı sıra, birden çok temel aboneliğin birleşik ücretlerini görüntülemek için de bir yol sağlar. Faturalama kapsamları hakkında daha fazla bilgi için bkz. [Microsoft Müşteri Sözleşmesi kapsamları](../costs/understand-work-scopes.md#microsoft-customer-agreement-scopes).

Daha üst kapsamlarda birleşik maliyet görünümleri elde etmek için Maliyet Yönetimi API'lerini de kullanabilirsiniz. Abonelik kapsamını kullanan tüm Maliyet Yönetimi API'leri, şemadaki birkaç küçük değişiklikle hala kullanılabilir. API'ler hakkında daha fazla bilgi için bkz. [Azure Maliyet Yönetimi API'leri](/rest/api/cost-management/) ve [Azure Tüketim API'leri](/rest/api/consumption/).

#### <a name="cost-allocation"></a>Maliyet ayırma

Güncelleştirilmiş hesabınızla kuruluşunuzdaki paylaşılan hizmetler arasında maliyet dağılımı yapmak için maliyet ayırma özelliklerini kullanabilirsiniz. Maliyetleri ayırma hakkında daha fazla bilgi için bkz. [Azure maliyet ayırma kurallarını oluşturma ve yönetme](../costs/allocate-costs.md).

#### <a name="power-bi"></a>Power BI

Power BI Desktop için Azure Maliyet Yönetimi bağlayıcısı Azure kullanımınızı ve harcamalarınızı gösteren özel görselleştirmeler ve raporlar oluşturmanıza yardımcı olur. Güncelleştirilmiş hesabınıza bağlandıktan sonra maliyet ve kullanım verilerinize erişirsiniz. Power BI Desktop'ta Azure Maliyet Yönetimi bağlayıcısı hakkında daha fazla bilgi için bkz. [Power BI Desktop'ta Azure Maliyet Yönetimi bağlayıcısıyla görseller ve raporlar oluşturma](/power-bi/connect-data/desktop-connect-azure-cost-management).

### <a name="updated-capabilities"></a>Güncelleştirilmiş özellikler

Azure faturalama hesabınızla aşağıdaki güncelleştirilmiş özellikler sağlanır.

#### <a name="cost-analysis"></a>Maliyet analizi

Aylara göre tüketim maliyetlerinizi görüntülemeye ve izlemeye devam edebileceğiniz gibi, artık Maliyet analizinde rezervasyon ve Market satın alma maliyetlerinizi de görüntüleyebilirsiniz.

Güncelleştirilmiş hesabınızla tüm Azure ücretleri için tek bir fatura alırsınız. Ayrıca önceki faturalama dönemleri görünümü yerine şimdi basitleştirilmiş tek bir aylık takvim görünümünden yararlanırsınız.

Örneğin eski hesabınızda faturalama döneminiz 24 Kasım ile 23 Aralık arasındaysa, yükseltme sonrasında bu tarihler 1 Kasım ile 30 Kasım, 1 Aralık ile 31 Aralık vb. arasında olacaktır.

:::image type="content" source="./media/mosp-new-customer-experience/billing-periods.png" alt-text="Eski ve yeni faturalama dönemleri arasındaki karşılaştırmayı gösteren resim" lightbox="./media/mosp-new-customer-experience/billing-periods.png" :::

#### <a name="budgets"></a>Bütçeler

Artık faturalama hesabı için bütçe oluşturabilir, bu sayede abonelikler arasında maliyetleri izleyebilirsiniz. Ayrıca bütçeleri kullanarak satın alma ücretlerinizi yakın takibe alabilirsiniz. Bütçeler hakkında daha fazla bilgi için bkz. [Azure bütçeleri oluşturma ve yönetme](../costs/tutorial-acm-create-budgets.md).

#### <a name="exports"></a>Dışarı Aktarmalar

Yeni faturalama döneminiz geliştirilmiş dışarı aktarma işlevi sağlar. Örneğin, satın almaları veya amorti edilmiş maliyetleri (satın alma dönemine yayılan rezervasyon satın alma maliyetleri) içeren gerçek maliyetler için dışarı aktarma işlemleri oluşturabilirsiniz. Ayrıca faturalama hesabının tüm aboneliklerindeki kullanımı ve ücretleri elde etmek üzere faturalama hesabı için de bir dışarı aktarma oluşturabilirsiniz. Dışa aktarma işlemleri hakkında daha fazla bilgi için bkz. [Dışarı aktarılan verileri oluşturma ve yönetme](../costs/tutorial-export-acm-data.md).

> [!NOTE]
> Hesabınız güncelleştirilmeden önce **Son ayın maliyetlerinin aylık dışarı aktarması** türünde oluşturulan dışarı aktarmalar son faturalama döneminin değil son takvim ayının verilerini dışarı aktarır.

Örneğin 23 Aralık ile 22 Ocak arasındaki faturalama dönemi için dışarı aktarılan CSV dosyasında söz konusu dönemin maliyet ve kullanım verileri yer alır. Güncelleştirme sonrasında, dışarı aktarma işlemi takvim ayının verilerini içerecektir. Örneğin, 1 Ocak ile 31 Aralık arası vb.

:::image type="content" source="./media/mosp-new-customer-experience/export-amortized-costs.png" alt-text="Eski ve yeni dışarı aktarma ayrıntıları arasındaki karşılaştırmayı gösteren resim" lightbox="./media/mosp-new-customer-experience/export-amortized-costs.png" :::

## <a name="additional-information"></a>Ek bilgiler

Aşağıdaki bölümlerde yeni deneyiminiz hakkında ek bilgiler sağlanır.

**Hizmet kapalı kalma süresi yoktur** Aboneliğinizdeki Azure hizmetleri kesintisiz olarak çalışmaya devam eder. Tek güncelleştirme, faturalama deneyiminizde yapılan güncelleştirmedir. Mevcut kaynaklar, kaynak grupları veya yönetim grupları üzerinde herhangi bir etki söz konusu değildir.

**Azure kaynaklarında değişiklik yoktur** Azure rol tabanlı erişim denetimi (Azure RBAC) kullanılarak ayarlanan Azure kaynaklarının erişimi güncelleştirmeden etkilenmez.

**Yeni deneyimde geçmiş faturalar kullanılabilir** Hesabınız güncelleştirilmeden önce oluşturulan faturalar Azure portalda kullanılabilir olmaya devam eder.

**Ayın ortasında güncelleştirilen hesabın faturaları** Hesabınız ayın ortasında güncelleştirildiyse hesabınızın güncelleştirildiği güne kadar biriken ücretler için bir fatura alırsınız. Ayın kalan bölümü için de başka bir fatura alırsınız. Örneğin hesabınızın tek bir aboneliği olduğunu ve 15 Eylül'de güncelleştirildiğini düşünün. 15 Eylül'e kadar biriken ücretler için bir fatura alırsınız. 15 Eylül ile 30 Eylül arasındaki dönem için de başka bir fatura alırsınız. Eylül'den sonra her ay için bir fatura alırsınız.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.

Yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Sonraki adımlar

Faturalama hesabınız hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın.

- [Yeni faturalama hesabınız için yönetim rollerini anlama](../manage/understand-mca-roles.md)
- [Yeni faturalama hesabınız için ek bir Azure aboneliği oluşturma](../manage/create-subscription.md)
- [Maliyetlerinizi düzenlemek için faturanızda bölümler oluşturma](../manage/mca-section-invoice.md)