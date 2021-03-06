---
title: Web uygulaması sorunlarını bulmak için Azure Izleyici 'de uygulama değişikliği analizini kullanma | Microsoft Docs
description: Azure App Service üzerindeki canlı sitelerde uygulama sorunlarını gidermek için Azure Izleyici 'de uygulama değişikliği analizini kullanın.
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 05/04/2020
ms.openlocfilehash: 0f541df091733c081c77e41ebff4d0d0d93dca96
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100573922"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Azure Izleyici 'de uygulama değişikliği analizini (Önizleme) kullanma

Canlı bir site sorunu veya kesintisi oluştuğunda, kök nedenin hızla belirlenmesi kritik öneme sahiptir. Standart izleme çözümleri sizi bir sorunla ilgili olarak uyarabilir. Bunlar, hangi bileşenin başarısız olduğunu bile gösterebilir. Ancak bu uyarı hatanın nedenini her zaman açıklamayacaktır. Sitenizde beş dakika önce çalıştık ve artık bozulmuş. Son beş dakika içinde ne değişti? Bu, uygulama değişikliği analizinin Azure Izleyici 'de yanıtlamak üzere tasarlandığına yönelik sorudır.

[Azure Kaynak Grafiği](../../governance/resource-graph/overview.md)'nin gücüyle çalışırken, değişiklik Analizi Observability artırmak ve MTTR 'i azaltmak için Azure uygulamanızın değişiklikleriyle ilgili öngörüler sağlar (ortalama onarım süresi).

> [!IMPORTANT]
> Değişiklik Analizi Şu anda önizleme aşamasındadır. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sunulmaktadır. Bu sürüm, üretim iş yükleri için önerilmez. Bazı özellikler desteklenmeyebilir veya kısıtlı özelliklere sahip olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Genel Bakış

Değişiklik analizi, altyapı katmanından uygulama dağıtımına kadar olan çeşitli değişiklik türlerini algılar. Bu, abonelikteki kaynak değişikliklerini denetleyen abonelik düzeyinde bir Azure Kaynak sağlayıcısıdır. Değişiklik analizi, kullanıcıların sorunlara neden olabilecek değişiklikler olduğunu anlamalarına yardımcı olmak için çeşitli tanılama araçları için veri sağlar.

Aşağıdaki diyagramda değişiklik analizinin mimarisi gösterilmektedir:

![Değişiklik analizinin değişiklik verilerini nasıl aldığı ve istemci araçlarına sağladığı mimari diyagramı](./media/change-analysis/overview.png)

## <a name="supported-resource-types"></a>Desteklenen kaynak türleri

Uygulama değişiklik Analizi hizmeti, aşağıdakiler gibi ortak kaynaklar dahil olmak üzere tüm Azure Kaynak türlerinde kaynak özelliği düzeyi değişikliklerini destekler:
- Sanal Makine
- Sanal makine ölçek kümesi
- App Service
- Azure Kubernetes hizmeti
- Azure İşlevi
- Ağ kaynakları: ağ güvenlik grubu, sanal ağ, Application Gateway, vb.
- Veri Hizmetleri: depolama, SQL, Redis Cache, Cosmos DB, vb.

## <a name="data-sources"></a>Veri kaynakları

Uygulama değişikliği analiz sorguları, Azure Resource Manager izlenen özellikler, proxy kullanan yapılandırma ve Web uygulaması Konuk içi değişiklikler için. Ayrıca, hizmet, bir uygulamanın uçtan uca tanılama ve izleme için kaynak bağımlılığı değişikliklerini izler.

### <a name="azure-resource-manager-tracked-properties-changes"></a>İzlenen Özellikler değişikliklerini Azure Resource Manager

[Azure Kaynak Grafiği](../../governance/resource-graph/overview.md)'ni kullanarak, değişiklik analizi, uygulamanızı barındıran Azure kaynaklarının zaman içinde nasıl değiştiğini gösteren bir geçmiş kaydı sağlar. Yönetilen kimlikler, Platform işletim sistemi yükseltmesi ve ana bilgisayar adları gibi izlenen ayarlar algılanabilir.

### <a name="azure-resource-manager-proxied-setting-changes"></a>Azure Resource Manager proxy ayar değişiklikleri

IP yapılandırma kuralı, TLS ayarları ve uzantı sürümleri gibi ayarlar henüz Azure Kaynak grafiğinde kullanılamıyor, bu nedenle analiz sorgularını değiştirin ve uygulamada nelerin değiştiğini daha fazla ayrıntı sağlamak için bu değişiklikleri güvenli bir şekilde hesaplar.

### <a name="changes-in-web-app-deployment-and-configuration-in-guest-changes"></a>Web uygulaması dağıtımı ve yapılandırmasındaki değişiklikler (konuk içi değişiklikler)

Değişiklik analizi, bir uygulamanın dağıtım ve yapılandırma durumunu her 4 saatte bir yakalar. Uygulama ortamı değişkenlerindeki değişiklikleri algılayabilir. Araç farkları hesaplar ve nelerin değiştiğini gösterir. Kaynak Yöneticisi değişikliklerden farklı olarak, kod dağıtımı değişiklik bilgileri araç içinde hemen kullanılamayabilir. Değişiklik analizinde en son değişiklikleri görüntülemek için **Yenile**' yi seçin.

!["Değişiklikleri şimdi Tara" düğmesinin ekran görüntüsü](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Bağımlılık değişiklikleri

Kaynak bağımlılıklarındaki değişiklikler, bir kaynaktaki sorunlara da neden olabilir. Örneğin, bir Web uygulaması Redsıs önbelleğine çağırırsa, Redsıs Cache SKU 'SU Web uygulaması performansını etkileyebilir. Diğer bir örnek, bir sanal makinenin ağ güvenlik grubunda bağlantı noktası 22 ' nin kapatılmadığı durumlarda bağlantı hatalarına neden olur.

#### <a name="web-app-diagnose-and-solve-problems-navigator-preview"></a>Web uygulaması sorun Gezginini tanılama ve çözme (Önizleme)

Bağımlılıklarda yapılan değişiklikleri algılamak için, değişiklik Analizi Web uygulamasının DNS kaydını denetler. Bu şekilde, tüm uygulama bileşenlerinde sorunlara neden olabilecek değişiklikler tanımlanmaktadır.
Şu anda **Web uygulaması tanılama ve çözme sorunlarını aşağıdaki bağımlılıklar destekler | Gezgin (Önizleme)**:

- Web Apps
- Azure Storage
- Azure SQL

#### <a name="related-resources"></a>İlgili kaynaklar

Uygulama değişiklik Analizi ilgili kaynakları algılar. Ortak örnekler, ağ güvenlik grubu, sanal ağ, Application Gateway ve bir sanal makineyle ilgili Load Balancer.
Ağ kaynakları genellikle onu kullanan kaynaklarla aynı kaynak grubunda otomatik olarak sağlanır, bu nedenle değişiklikleri kaynak grubuna göre filtrelemek, sanal makine ve ilgili ağ kaynakları için tüm değişiklikleri gösterir.

![Ağ değişikliklerinin ekran görüntüsü](./media/change-analysis/network-changes.png)

## <a name="application-change-analysis-service-enablement"></a>Uygulama değişiklik Analizi hizmeti etkinleştirme

Uygulama değişikliği çözümleme hizmeti, yukarıda belirtilen veri kaynaklarından verileri hesaplar ve toplar. Kullanıcıların tüm kaynak değişikliklerinde kolayca gezinecek ve sorun giderme veya izleme bağlamında hangi değişikliğin ilgili olduğunu belirleyebilecekleri bir analiz kümesi sağlar.
"Microsoft. ChangeAnalysis" kaynak sağlayıcısının, Azure Resource Manager izlenen özellikler için bir aboneliğe kayıtlı olması ve proxy ayarları değişiklik verilerinin kullanılabilir olması gerekir. Web uygulaması tanılama ve çözme sorunları aracını girerken veya değişiklik Analizi tek başına sekmesini getirdiğinizde, bu kaynak sağlayıcı otomatik olarak kaydedilir.
Web uygulaması Konuk içi değişiklikler için, bir Web uygulaması içindeki kod dosyalarını taramak üzere ayrı etkinleştirme gerekir. Daha fazla bilgi için bu makalenin ilerleyen kısımlarında bulunan [sorunları Tanıla ve çöz araç bölümündeki değişiklik Analizi](change-analysis-visualizations.md#application-change-analysis-in-the-diagnose-and-solve-problems-tool) bölümüne bakın.

## <a name="cost"></a>Maliyet
Uygulama değişikliği Analizi ücretsiz bir hizmettir; BT etkin olan Aboneliklerle ilgili faturalandırma maliyeti yoktur. Ayrıca hizmetin Azure Kaynak özellikleri değişikliklerini taramak için herhangi bir performans etkisi yoktur. Web uygulamaları için değişiklik analizini Konuk dosya değişikliklerine etkinleştirdiğinizde (veya sorunları Tanıla ve çöz aracını etkinleştirirseniz), Web uygulaması üzerinde daha fazla performans etkisi olur ve fatura maliyeti yoktur.

## <a name="visualizations-for-application-change-analysis"></a>Uygulama değişikliği analizinin görselleştirmeleri

### <a name="standalone-ui"></a>Tek başına kullanıcı arabirimi

Azure Izleyici 'de, uygulama bağımlılıkları ve kaynakları hakkındaki öngörülere sahip tüm değişiklikleri görüntülemek için değişiklik analizinin tek başına bir bölmesi vardır.

Deneyimi başlatmak için Azure portal arama çubuğunda bulunan değişiklik analizini arayın.

![Azure portal değişiklik analizini aramanın ekran görüntüsü](./media/change-analysis/search-change-analysis.png)

Seçili bir abonelik kapsamındaki tüm kaynaklar son 24 saat içindeki değişikliklerle birlikte görüntülenir. Sayfa yükleme performansını iyileştirmek için hizmet bir seferde 10 kaynak görüntülüyor. Daha fazla kaynak görüntülemek için sonraki sayfalara tıklayın. Bu sınırlamayı kaldırmak için çalışıyoruz.

![Azure portal değişiklik Analizi dikey penceresinin ekran görüntüsü](./media/change-analysis/change-analysis-standalone-blade.png)

Tüm değişikliklerini görüntülemek için bir kaynağa tıklanın. Gerekirse, JSON biçimli değişiklik ayrıntılarını ve öngörülerini görüntülemek için bir değişikliği detaya gidin.

![Değişiklik ayrıntılarının ekran görüntüsü](./media/change-analysis/change-details.png)

Herhangi bir geri bildirim için dikey penceredeki veya e-postadaki geri bildirim gönder düğmesini kullanın changeanalysisteam@microsoft.com .

![Değişiklik Analizi dikey penceresinde geri bildirim düğmesinin ekran görüntüsü](./media/change-analysis/change-analysis-feedback.png)

#### <a name="multiple-subscription-support"></a>Çoklu abonelik desteği
UI, kaynak değişikliklerini görüntülemek için birden çok abonelik seçmeyi destekler. Abonelik filtresini kullanın:

![Birden çok abonelik seçmeyi destekleyen abonelik filtresi ekran görüntüsü](./media/change-analysis/multiple-subscriptions-support.png)

### <a name="web-app-diagnose-and-solve-problems"></a>Web uygulaması sorunları tanılama ve çözme

Azure Izleyici 'de, değişiklik Analizi Ayrıca self servis **Tanılama ve sorun sorunları** deneyiminde yerleşik olarak bulunur. App Service uygulamanızın **genel bakış** sayfasından bu deneyimle erişin.

!["Genel bakış" düğmesinin ve "sorunları tanılama ve çözme" düğmesinin ekran görüntüsü](./media/change-analysis/change-analysis.png)

### <a name="application-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Sorunları tanılama ve çözme aracında uygulama değişikliği Analizi

Uygulama değişikliği analizi, Web uygulamasındaki tek başına bir algılayıcı olan sorunları tanılamanıza ve çözmenize yardımcı olur. Ayrıca **uygulama Kilitlenmelerinde** ve **Web uygulaması, algılayıcıları**'nda da toplanır. Sorunları Tanıla ve çöz aracını girerken, **Microsoft. ChangeAnalysis** kaynak sağlayıcısı otomatik olarak kaydedilir. Web uygulamasını Konuk değişiklik izlemeyi etkinleştirmek için bu yönergeleri izleyin.

1. **Kullanılabilirlik ve performans ' ı** seçin.

    !["Kullanılabilirlik ve performans" sorun giderme seçeneklerinin ekran görüntüsü](./media/change-analysis/availability-and-performance.png)

2. **Uygulama değişikliklerini** seçin. Özellik **uygulama Kilitlenmelerinde** da kullanılabilir.

   !["Uygulama kilitlenmeler" düğmesinin ekran görüntüsü](./media/change-analysis/application-changes.png)

3. Bağlantı, Web uygulaması kapsamındaki Aalysis Kullanıcı arabirimini uygulama değişikliğine yönlendirir. Web uygulaması Konuk değişiklik izleme etkin değilse, dosya ve uygulama ayarları değişikliklerini almak için başlığı izleyin.

   !["Uygulama kilitlenmeler" seçeneklerinin ekran görüntüsü](./media/change-analysis/enable-changeanalysis.png)

4. **Değişiklik analizini** açın ve **Kaydet**' i seçin. Araç, tüm Web uygulamalarını bir App Service planı altında görüntüler. Plan düzeyi anahtarını, bir plandaki tüm Web uygulamalarının değişiklik analizini açmak için kullanabilirsiniz.

    !["Değişiklik analizini etkinleştir" Kullanıcı arabiriminin ekran görüntüsü](./media/change-analysis/change-analysis-on.png)

5. Değişiklik verileri, Select **Web App** ı ve **uygulama kilitlenmesi** algılayıcıları içinde de kullanılabilir. Zaman içindeki değişikliklerin türünü ve bu değişikliklerle ilgili ayrıntıları özetleyen bir grafik görürsünüz. Varsayılan olarak, son 24 saat içindeki değişiklikler anında sorunla ilgili yardım almak için görüntülenir.

     ![Değişiklik fark görünümünün ekran görüntüsü](./media/change-analysis/change-view.png)



### <a name="virtual-machine-diagnose-and-solve-problems"></a>Sanal makine tanılama ve çözme sorunları

Bir sanal makine için sorunları tanılama ve çözme aracını ziyaret edin.  **Sorun giderme araçları**' na gidin, sayfayı Inceleyin ve sanal makinedeki değişiklikleri görüntülemek için **son değişiklikleri çözümle** ' yi seçin.

![VM tanılama ve çözme sorunlarının ekran görüntüsü](./media/change-analysis/vm-dnsp-troubleshootingtools.png)

![Sorun giderme araçlarında çözümleyici 'yi değiştirme](./media/change-analysis/analyze-recent-changes.png)

### <a name="activity-log-change-history"></a>Etkinlik günlüğü değişiklik geçmişi
Etkinlik günlüğündeki değişiklik [geçmişini görüntüle](../essentials/activity-log.md#view-change-history) özelliği, bir işlemle ilişkili değişiklikleri almak Için uygulama değişiklik Analizi hizmeti arka ucunu çağırır. [Azure Kaynak grafiğini](../../governance/resource-graph/overview.md) doğrudan çağırmak Için kullanılan **değişiklik geçmişi** , ancak döndürülen değişiklikler, [Azure Kaynak grafiğinden](../../governance/resource-graph/overview.md)kaynak düzeyindeki değişiklikleri, [Azure Resource Manager](../../azure-resource-manager/management/overview.md)kaynak özelliklerini ve uygulama Hizmetleri Web uygulaması gibi PaaS hizmetlerinden gelen Konuk değişiklikleri içerir. Uygulama değişikliği çözümleme hizmeti 'nin Kullanıcı aboneliklerinde değişiklik taraması yapabilmesi için, bir kaynak sağlayıcısının kaydedilmesi gerekir. **Değişiklik geçmişi** sekmesine ilk kez girerken araç otomatik olarak **Microsoft. changeanalysis** kaynak sağlayıcısını kaydetmeye başlayacaktır. Kaydolduktan sonra **Azure Kaynak grafındaki** değişiklikler hemen kullanılabilir ve son 14 güne ait olur. Abonelik eklendikten sonra diğer kaynaklardaki değişiklikler ~ 4 saat sonra kullanılabilir olacaktır.

![Etkinlik günlüğü değişiklik geçmişi tümleştirmesi](./media/change-analysis/activity-log-change-history.png)

### <a name="vm-insights-integration"></a>VM öngörüleri tümleştirmesi
[VM](../vm/vminsights-overview.md) içgörüleri etkin olan kullanıcılar, sanal MAKINELERINIZDE, CPU veya bellek gibi bir ölçüm grafiğinde hangi artışlara neden olabilecek değiştiğini görüntüleyebilir ve bunun ne olduğunu merak edebilir. Değişiklik verileri, VM öngörüleri tarafında gezinme çubuğunda tümleştirilir. Kullanıcı, VM 'de herhangi bir değişiklik olup olmadığını görüntüleyebilir ve uygulama değişiklik Analizi tek başına Kullanıcı arabirimindeki değişiklik ayrıntılarını görüntülemek için **değişiklikleri araştır** ' a tıklayın.

[![VM öngörüleri tümleştirmesi](./media/change-analysis/vm-insights.png)](./media/change-analysis/vm-insights.png#lightbox)


## <a name="enable-change-analysis-at-scale"></a>Ölçek üzerinde değişiklik analizini etkinleştir

Aboneliğiniz çok sayıda Web uygulaması içeriyorsa, hizmeti Web uygulaması düzeyinde etkinleştirmek verimsiz olur. Aboneliğinizdeki tüm Web uygulamalarını etkinleştirmek için aşağıdaki betiği çalıştırın.

Önkoşullar:

- PowerShell az Module. [Azure PowerShell modülünü yüklerken](/powershell/azure/install-az-ps) yönergeleri izleyin

Şu betiği çalıştırın:

```PowerShell
# Log in to your Azure subscription
Connect-AzAccount

# Get subscription Id
$SubscriptionId = Read-Host -Prompt 'Input your subscription Id'

# Make Feature Flag visible to the subscription
Set-AzContext -SubscriptionId $SubscriptionId

# Register resource provider
Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"

# Enable each web app
$webapp_list = Get-AzWebApp | Where-Object {$_.kind -eq 'app'}
foreach ($webapp in $webapp_list)
{
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId $webapp.Id -Tag $tags -Force
}

```

## <a name="next-steps"></a>Sonraki adımlar

- [Değişiklik çözümlemede görselleştirmeler](change-analysis-visualizations.md) hakkında bilgi edinin
- [Değişiklik çözümlemede sorunları nasıl giderebileceğinizi](change-analysis-troubleshoot.md) öğrenin
- [Azure Uygulama Hizmetleri uygulamaları](azure-web-apps.md)için Application Insights etkinleştirin.
- [Azure VM ve Azure sanal makine ölçek kümesi için Application Insights ETKINLEŞTIRME IIS tarafından barındırılan uygulamalar](azure-vm-vmss-apps.md).
