---
title: Hizmet sağlayıcıları için Azure Izleyici günlükleri | Microsoft Docs
description: Azure Izleyici günlükleri, yönetilen hizmet sağlayıcılarının (MSPs), büyük kuruluşların, bağımsız yazılım satıcılarının (ISV 'Ler) ve barındırma hizmeti sağlayıcılarının, müşterinin Şirket içi veya bulut altyapısındaki sunucuları yönetmesine ve izlemesine yardımcı olabilir.
ms.subservice: logs
ms.topic: conceptual
author: MeirMen
ms.author: meirm
ms.date: 02/03/2020
ms.openlocfilehash: 5f69de583dea14be3c7ce3ab6779af549e95de75
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100624938"
---
# <a name="azure-monitor-logs-for-service-providers"></a>Hizmet sağlayıcıları için Azure Izleyici günlükleri

Azure Izleyici 'deki Log Analytics çalışma alanları, yönetilen hizmet sağlayıcılarının (MSPs), büyük kuruluşların, bağımsız yazılım satıcılarının (ISV) ve barındırma hizmeti sağlayıcılarının, müşterinin Şirket içi veya bulut altyapısındaki sunucuları yönetmesine ve izlemesine yardımcı olabilir.

Büyük kuruluşlar, özellikle birçok farklı iş birimi için yönetmekten sorumlu bir merkezi BT ekibi olduğunda, servis sağlayıcılarıyla birçok benzerlik paylaşır. Kolaylık olması için, bu belge *hizmet sağlayıcısını* kullanır, ancak kuruluşlar ve diğer müşteriler için aynı işlevsellik de mevcuttur.

[Bulut çözümü sağlayıcısı (CSP)](https://partner.microsoft.com/membership/cloud-solution-provider) programının parçası olan iş ortakları ve hizmet sağlayıcıları Için Azure izleyici 'de log ANALYTICS Azure CSP aboneliklerinde bulunan Azure hizmetlerinden biridir.

Azure Izleyici 'de Log Analytics Ayrıca, Azure açık [Thouse](../../lighthouse/overview.md)'daki Azure Temsilcili kaynak yönetimi özelliği aracılığıyla müşteri kaynaklarını yöneten bir hizmet sağlayıcısı tarafından da kullanılabilir.

## <a name="architectures-for-service-providers"></a>Hizmet sağlayıcıları için mimariler

Log Analytics çalışma alanları, yöneticinin [günlük](../logs/data-platform-logs.md) verilerinin akışını ve yalıtımının denetlenmesi ve belirli iş ihtiyaçlarını karşılayan bir mimari oluşturması için bir yöntem sağlar. [Bu makalede](../logs/design-logs-deployment.md) bir çalışma alanı için tasarım, dağıtım ve geçiş konuları açıklanmakta ve [erişim yönetimi](../logs/manage-access.md) makalesi, günlük verilerine yönelik izinlerin nasıl uygulanacağını ve yönetileceğini açıklar. Hizmet sağlayıcılarının ek konuları vardır.

Hizmet sağlayıcılarının Log Analytics çalışma alanlarıyla ilgili üç olası mimarisi vardır:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. dağıtılmış Günlükler müşterinin kiracısında bulunan çalışma alanlarında depolanır

Bu mimaride, müşterinin kiracısında müşterinin tüm günlükleri için kullanılan bir çalışma alanı dağıtılır.

Hizmet sağlayıcı yöneticilerinin bir müşteri kiracısında bir Log Analytics çalışma alanına erişim sağlayabilmenin iki yolu vardır:

- Müşteri, hizmet sağlayıcısından [Azure Active Directory Konuk Kullanıcı (B2B)](../../active-directory/external-identities/what-is-b2b.md)olarak bireysel kullanıcılar ekleyebilir. Hizmet sağlayıcı yöneticilerinin, bu çalışma alanlarına erişebilmek için Azure portal her bir müşterinin dizininde oturum açması gerekir. Bu, müşterilerin her bir hizmet sağlayıcı Yöneticisi için bireysel erişimi yönetmesini de gerektirir.
- Hizmet sağlayıcılar, daha fazla ölçeklenebilirlik ve esneklik sağlamak için [Azure](../../lighthouse/overview.md) açık [kaynak yönetimi](../../lighthouse/concepts/azure-delegated-resource-management.md) özelliğini kullanarak müşterinin kiracısına erişim sağlayabilir. Bu yöntemde, hizmet sağlayıcı yöneticileri hizmet sağlayıcısının kiracısındaki bir Azure AD kullanıcı grubuna dahil edilir ve bu gruba her müşteri için ekleme işlemi sırasında erişim verilir. Bu Yöneticiler daha sonra her bir müşterinin kiracısında tek tek oturum açmasını sağlamak yerine, her bir müşterinin çalışma alanına kendi hizmet sağlayıcısı kiracının içinden erişmesini sağlayabilir. Müşterilerinizin Log Analytics çalışma alanları kaynaklarına bu şekilde erişmek, müşteri tarafında gereken işi azaltır ve [Azure Izleyici çalışma kitapları](../visualize/workbooks-overview.md)gibi araçlarla aynı hizmet sağlayıcısı tarafından yönetilen birden fazla müşteriye veri toplayıp analiz etmeyi kolaylaştırır. Daha fazla bilgi için bkz. [Müşteri kaynaklarını ölçeklendirerek izleme](../../lighthouse/how-to/monitor-at-scale.md).

Dağıtılmış mimarinin avantajları şunlardır:

* Müşteri, [Azure tarafından atanan kaynak yönetimi](../../lighthouse/concepts/azure-delegated-resource-management.md)aracılığıyla belirli düzeylerde izinleri belirtebilir veya kendi [Azure rol tabanlı erişim DENETIMINI (Azure RBAC)](../../role-based-access-control/overview.md)kullanarak günlüklere erişimi yönetebilir.
* Günlükler yalnızca aracı tabanlı VM verilerini değil, tüm kaynak türleri için toplanabilir. Örneğin, Azure denetim günlükleri.
* Her müşteri, çalışma alanı için bekletme ve veri dönüşü gibi farklı ayarlara sahip olabilir.
* Yönetmelik ve zorluk açısından müşteriler arasında yalıtım.
* Her çalışma alanı için ücret müşterinin aboneliğine alınacaktır.

Dağıtılmış mimarinin dezavantajları şunlardır:

* Azure Izleyici çalışma kitapları gibi araçlarla verilerin müşteri kiracılarında merkezi olarak görselleştirilmesi ve çözümlenmesi, özellikle 50 ' den fazla çalışma alanındaki verileri analiz edilirken daha yavaş deneyimler oluşmasına neden olabilir.
* Müşteriler, Azure tarafından yetkilendirilen kaynak yönetimi için eklendi değilse, servis sağlayıcı yöneticilerinin müşteri dizininde sağlanması gerekir ve hizmet sağlayıcının aynı anda çok sayıda müşteri kiracıyı yönetmesi daha zordur.

### <a name="2-central---logs-are-stored-in-a-workspace-located-in-the-service-provider-tenant"></a>2. orta Günlükler, hizmet sağlayıcı kiracısında bulunan bir çalışma alanında depolanır

Bu mimaride, Günlükler müşterinin kiracılarında depolanmaz, ancak yalnızca hizmet sağlayıcının aboneliklerinde bulunan merkezi bir konumda depolanır. Müşterinin VM 'lerine yüklenen aracılar, çalışma alanı KIMLIĞI ve gizli anahtarı kullanılarak günlüklerini bu çalışma alanına gönderecek şekilde yapılandırılmıştır.

Merkezi mimarinin avantajları şunlardır:

* Çok sayıda müşteriyi yönetmek ve bunları çeşitli arka uç sistemleriyle bütünleştirmek kolaydır.
* Hizmet sağlayıcısı Günlükler üzerinde tam sahiplik ve işlevler ve kaydedilmiş sorgular gibi çeşitli yapıtlar içerir.
* Hizmet sağlayıcı, tüm müşterileri genelinde analiz gerçekleştirebilir.

Merkezi mimarinin dezavantajları şunlardır:

* Bu mimari yalnızca aracı tabanlı VM verileri için geçerlidir; PaaS, SaaS ve Azure Fabric veri kaynaklarını kapsamaz.
* Tek bir çalışma alanında birleştirildiklerinde müşteriler arasında verileri ayırmak zor olabilir. Bunu yapmak için tek iyi yöntem bilgisayarın tam etki alanı adını (FQDN) veya Azure abonelik KIMLIĞINI kullanmaktır.
* Tüm müşterilerin tüm verileri, tek bir fatura ve aynı bekletme ve yapılandırma ayarları ile aynı bölgede depolanır.
* Azure Tanılama ve Azure denetim günlükleri gibi Azure Fabric ve PaaS hizmetleri, çalışma alanının kaynakla aynı kiracıda olmasını gerektirir, bu nedenle günlükleri merkezi çalışma alanına gönderemez.
* Tüm müşterilerin tüm VM aracılarının, aynı çalışma alanı KIMLIĞI ve anahtarı kullanılarak merkezi çalışma alanında kimlik doğrulaması yapılır. Belirli bir müşteriden diğer müşterileri kesintiye uğramadan günlüklerin engellenme yöntemi yoktur.

### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. karma Günlükler müşterinin kiracısında bulunan çalışma alanında depolanır ve bazıları merkezi bir konuma çekilir.

İki seçenek arasındaki üçüncü mimari karışımı. Günlüklerin her müşteri için yerelden olduğu, ancak merkezi bir günlük deposu oluşturmak için bazı mekanizmalarının kullanıldığı ilk dağıtılmış mimariye dayanır. Günlüklerin bir kısmı, raporlama ve analiz için merkezi bir konuma çekilir. Bu bölüm, az sayıda veri türü veya günlük istatistikler gibi etkinliğin bir özeti olabilir.

Günlükleri merkezi bir konumda uygulamak için iki seçenek vardır:

1. Merkezi çalışma alanı: hizmet sağlayıcı kiracısında bir çalışma alanı oluşturabilir ve verileri çeşitli çalışma alanlarından bu merkezi konuma getirmek için [veri toplama API](../logs/data-collector-api.md) 'Siyle bırlıkte [sorgu API](https://dev.loganalytics.io/) 'sini kullanan bir komut dosyası kullanabilir. Bir komut dosyası dışında başka bir seçenek [Azure Logic Apps](../../logic-apps/logic-apps-overview.md)kullanmaktır.

2. Merkezi bir konum olarak Power BI: Power BI, Log Analytics çalışma alanı ve [Power BI](../visualize/powerbi.md)arasındaki tümleştirmeyi kullanarak verileri dışa aktardığınızda merkezi konum olarak görev yapabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak Yöneticisi şablonlarını](../logs/resource-manager-workspace.md) kullanarak çalışma alanlarının oluşturulmasını ve yapılandırılmasını otomatikleştirin

* [PowerShell](../logs/powershell-workspace-configuration.md) kullanarak çalışma alanlarının oluşturulmasını otomatikleştirme

* Mevcut sistemlerle tümleştirme için [uyarıları](../platform/alerts-overview.md) kullanma

* [Power BI](../visualize/powerbi.md) kullanarak özet raporlar oluşturma

* [Azure tarafından atanan kaynak yönetimine](../../lighthouse/concepts/azure-delegated-resource-management.md)müşteri ekleme.