---
title: Azure Active Directory 'de esnekliği için uygulama oturum açma durumunu izleme
description: Uygulamalarınızın oturum açma durumunu izlemek için sorgular ve bildirimler oluşturun.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 01/10/2021
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40bfa27dba905cb2e9a363c7739f0a43e7c2afdf
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100101374"
---
# <a name="monitoring-application-sign-in-health-for-resilience"></a>Esnekliği için uygulama oturum açma durumunu izleme

Altyapı esnekliği artırmak için, önemli uygulamalarınız için uygulama oturum açma durumunu izlemeyi ayarlayarak, etkileyen bir olay oluşursa bir uyarı alırsınız. Bu çabayla ilgili size yardımcı olmak için, oturum açma sistem durumu çalışma kitabına göre uyarıları yapılandırabilirsiniz. 

Bu çalışma kitabı, yöneticilerin kiracınızdaki uygulamalar için kimlik doğrulama isteklerini izlemesini sağlar. Aşağıdaki temel özellikleri sağlar:

* Çalışma kitabını, neredeyse gerçek zamanlı verilere sahip tüm veya bağımsız uygulamaları izlemek üzere yapılandırın.

* İnceleme ve işlem yapmak için kimlik doğrulama desenleri değiştiğinde sizi bilgilendirmek üzere uyarılar yapılandırın.

* Çalışma kitabının varsayılan ayarı olan hafta boyunca haftaya yönelik eğilimleri karşılaştırın.

> [!NOTE]
> Tüm kullanılabilir çalışma kitaplarını ve bunları kullanmaya yönelik önkoşulları görmek için lütfen bkz. [Azure izleyici çalışma kitaplarını raporlar için kullanma](../reports-monitoring/howto-use-azure-monitor-workbooks.md).

Etkileyen bir olay sırasında iki şey meydana gelebilir:

* Kullanıcılar oturum açabilmeniz için, bir uygulama için oturum açma işlemlerinin sayısı precipitously bırakabilir.

* Oturum açma hatalarının sayısı artabilir. 

Bu makalede, kullanıcılarınızın oturum açma işlemlerinde kesintiler izlemek için oturum açma sistem durumu çalışma kitabını ayarlama işlemi adım adım gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar 

* Azure AD kiracısı.

* Azure AD kiracısı için genel yönetici veya güvenlik yöneticisi rolüne sahip bir kullanıcı.

* Azure Izleyici günlüklerine Günlükler göndermek için Azure aboneliğinizdeki bir Log Analytics çalışma alanı. 

   * [Log Analytics çalışma alanı oluşturmayı](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) öğrenin

* Azure Izleyici günlükleri ile tümleştirilmiş Azure AD günlükleri

   * Azure [ad oturum açma günlüklerini Azure Izleyici akışı Ile tümleştirmeyi öğrenin.](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

 

## <a name="configure-the-app-sign-in-health-workbook"></a>Uygulama oturum açma durumu çalışma kitabını yapılandırma 

Çalışma kitaplarına erişmek için **Azure Portal** açın, **Azure Active Directory**' i seçin ve ardından **çalışma kitapları**' nı seçin.

Kullanım, koşullu erişim ve sorun giderme altındaki çalışma kitaplarını görürsünüz. Uygulama oturum açma durumu çalışma kitabı kullanım bölümünde görüntülenir.

Çalışma kitabını kullandığınızda, son değiştirilen çalışma kitapları bölümünde görünebilir.

![Azure portal çalışma kitabı galerinin gösterildiği ekran görüntüsü.](./media/monitor-sign-in-health-for-resilience/sign-in-health-workbook.png)


Uygulama oturum açma sistem durumu çalışma kitabında, oturum açma işlemleri ile neler olduğunu görselleştirmenize olanak sağlar. 

Çalışma kitabı varsayılan olarak iki grafik gösterir. Bu grafikler, artık uygulamalarınız için neler olduğunu ve bir hafta önce aynı döneme karşı karşılaştırın. Mavi çizgiler geçerli ve turuncu çizgiler önceki haftadır.

![Oturum açma sistem durumu grafiklerini gösteren ekran görüntüsü.](./media/monitor-sign-in-health-for-resilience/sign-in-health-graphs.png)

**İlk grafik saatlik kullanımdır (başarılı Kullanıcı sayısı)**. Geçerli başarılı kullanıcı sayısını tipik bir kullanım süresine göre karşılaştırmak, bir kullanıma araştırma gerektirebilecek kullanımda olan bir bırakma oluşturmanıza yardımcı olur. Kullanım oranının başarıyla düşürülmesi, hata oranının performans ve kullanım sorunlarını tespit etmenize yardımcı olabilir. Örneğin, kullanıcılar oturum açmayı denemek için uygulamanıza ulaşamadıysanız, yalnızca kullanımda olan bir başarısızlık olmaz. Bu veriler için örnek sorgu aşağıdaki bölümde bulunabilir.

İkinci grafik saat hatası oranına sahiptir. Hata oranı 'nda ani artış, kimlik doğrulama mekanizmalarıyla ilgili bir sorun olduğunu gösterebilir. Hata oranı yalnızca kullanıcıların kimlik doğrulamaya çalışabilmesi durumunda ölçülebilir. Kullanıcılar, denemesi yapmak için erişim Sağlayamayabiliyorsa, başarısızlıklar gösterilmez.

Kullanım veya başarısızlık oranı belirtilen eşiği aştığında belirli bir gruba bildirimde bulunan bir uyarı yapılandırabilirsiniz. Bu veriler için örnek sorgu aşağıdaki bölümde bulunabilir.

 ## <a name="configure-the-query-and-alerts"></a>Sorgu ve Uyarıları yapılandırma

Azure Izleyici 'de uyarı kuralları oluşturur ve düzenli aralıklarla otomatik olarak kaydedilmiş sorguları veya özel günlük aramalarını çalıştırabilir.

Grafiklerde yansıtılan sorgulara göre e-posta uyarıları oluşturmak için aşağıdaki yönergeleri kullanın. Aşağıdaki örnek betikler, şu durumlarda bir e-posta bildirimi gönderir

* başarılı kullanım, önceki bölümde saatlik kullanım grafiğinde olduğu gibi, aynı saat iki gün önce %90 oranında düştir. 

* hata oranı, önceki bölümde yer aldığı saatlik başarısızlık oranı grafiğinde olduğu gibi, iki günden daha önce %90 oranında artar. 

 Temel alınan sorguyu yapılandırmak ve uyarıları ayarlamak için aşağıdaki adımları izleyin. Yapılandırma için temel olarak örnek sorgu kullanacaksınız. Bu bölümün sonunda sorgu yapısının bir açıklaması görüntülenir.

Azure Izleyici kullanarak günlük uyarılarını oluşturma, görüntüleme ve yönetme hakkında daha fazla bilgi için bkz. [günlük uyarılarını yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log).

 
1. Çalışma kitabında **Düzenle**' yi seçin, sonra grafiğin sağ tarafındaki **Sorgu simgesini** seçin.   

   [![Düzenleme çalışma kitabını gösteren ekran görüntüsü.](./media/monitor-sign-in-health-for-resilience/edit-workbook.png)](./media/monitor-sign-in-health-for-resilience/edit-workbook.png)

   Sorgu günlüğü açılır.

  [![Sorgu günlüğünü gösteren ekran görüntüsü.](./media/monitor-sign-in-health-for-resilience/query-log.png)](/media/monitor-sign-in-health-for-resilience/query-log.png)
‎

2. Yeni bir kusto sorgusu için aşağıdaki örnek betiklerin birini kopyalayın.

**Kullanımda bırakma için kusto sorgusu**

```Kusto

let thisWeek = SigninLogs

| where TimeGenerated > ago(1h)

| project TimeGenerated, AppDisplayName, UserPrincipalName

//| where AppDisplayName contains "Office 365 Exchange Online"

| summarize users = dcount(UserPrincipalName) by bin(TimeGenerated, 1hr)

| sort by TimeGenerated desc

| serialize rn = row_number();

let lastWeek = SigninLogs

| where TimeGenerated between((ago(1h) - totimespan(2d))..(now() - totimespan(2d)))

| project TimeGenerated, AppDisplayName, UserPrincipalName

//| where AppDisplayName contains "Office 365 Exchange Online"

| summarize usersPriorWeek = dcount(UserPrincipalName) by bin(TimeGenerated, 1hr)

| sort by TimeGenerated desc

| serialize rn = row_number();

thisWeek

| join

(

 lastWeek

)

on rn

| project TimeGenerated, users, usersPriorWeek, difference = abs(users - usersPriorWeek), max = max_of(users, usersPriorWeek)

| where (difference * 2.0) / max > 0.9

```

 

**Hata oranı artışı için kusto sorgusu**


```kusto

let thisWeek = SigninLogs

| where TimeGenerated > ago(1 h)

| project TimeGenerated, UserPrincipalName, AppDisplayName, status = case(Status.errorCode == "0", "success", "failure")

| where AppDisplayName == **APP NAME**

| summarize success = countif(status == "success"), failure = countif(status == "failure") by bin(TimeGenerated, 1h)

| project TimeGenerated, failureRate = (failure * 1.0) / ((failure + success) * 1.0)

| sort by TimeGenerated desc

| serialize rn = row_number();

let lastWeek = SigninLogs

| where TimeGenerated between((ago(1 h) - totimespan(2d))..(ago(1h) - totimespan(2d)))

| project TimeGenerated, UserPrincipalName, AppDisplayName, status = case(Status.errorCode == "0", "success", "failure")

| where AppDisplayName == **APP NAME**

| summarize success = countif(status == "success"), failure = countif(status == "failure") by bin(TimeGenerated, 1h)

| project TimeGenerated, failureRatePriorWeek = (failure * 1.0) / ((failure + success) * 1.0)

| sort by TimeGenerated desc

| serialize rn = row_number();

thisWeek

| join (lastWeek) on rn

| project TimeGenerated, failureRate, failureRatePriorWeek

| where abs(failureRate – failureRatePriorWeek) > **THRESHOLD VALUE**

```

3. Sorguyu pencereye yapıştırın ve **Çalıştır**' ı seçin. Aşağıdaki görüntüde gösterilen tamamlanmış iletiyi görtığınızdan ve bu iletinin altına sonuçtan emin olun.

   [![Sorgu sonuçlarını çalıştırmayı gösteren ekran görüntüsü.](./media/monitor-sign-in-health-for-resilience/run-query.png)](./media/monitor-sign-in-health-for-resilience/run-query.png)

4. Sorguyu vurgulayın ve + **Yeni uyarı kuralı**' nı seçin. 
 
   [![Yeni uyarı kuralı ekranını gösteren ekran görüntüsü.](./media/monitor-sign-in-health-for-resilience/new-alert-rule.png)](./media/monitor-sign-in-health-for-resilience/new-alert-rule.png)


5. Uyarı koşullarını yapılandırın. Koşul bölümünde, **Ortalama özel günlük araması, Logic Defined Count değerinden fazla** olduğunda bağlantıyı seçin. Sinyal mantığını Yapılandır bölmesinde, uyarı mantığı ' na kaydırın.

   [![Uyarıları Yapılandır ekranını gösteren ekran görüntüsü.](./media/monitor-sign-in-health-for-resilience/configure-alerts.png)](./media/monitor-sign-in-health-for-resilience/configure-alerts.png)
 
   * **Eşik değeri**: 0. Bu değer, herhangi bir sonucu uyarır.

   * **Değerlendirme süresi (dakika)**: 60. Bu değer bir saat arar

   * **Sıklık (dakika)**: 60. Bu değer, değerlendirme süresini önceki saat için saat başına olacak şekilde ayarlar.

   * **Bitti** seçeneğini belirleyin.

6. **Eylemler** bölümünde, şu ayarları yapılandırın:  

    [![Uyarı kuralı oluştur sayfasını gösteren ekran görüntüsü.](./media/monitor-sign-in-health-for-resilience/create-alert-rule.png)](./media/monitor-sign-in-health-for-resilience/create-alert-rule.png)

   * **Eylemler** altında **eylem grubu seç**' i seçin ve uyarı bildirilmesini istediğiniz grubu ekleyin.

   * **Özelleştirme eylemleri** altında **e-posta uyarıları**' nı seçin.

   * **Konu satırı** ekleyin.

7. **Uyarı kuralı ayrıntıları**' nın altında, şu ayarları yapılandırın:

   * Açıklayıcı bir ad ve açıklama ekleyin.

   * Uyarının ekleneceği **kaynak grubunu** seçin.

   * Uyarının varsayılan **önem derecesini** seçin.

   * **Oluşturma sonrasında uyarı kuralını etkinleştir** ' i seçin, hemen canlı olmasını Istiyorsanız **Uyarıları Gizle**' yi seçin.

8. **Uyarı kuralı oluşturma**’yı seçin.

9. **Kaydet**' i seçin, sorgu için bir ad girin, **uyarı kategorisi olan bir sorgu olarak kaydedin**. Sonra yeniden **Kaydet** ' i seçin.  

   [![Sorguyu Kaydet düğmesinin gösterildiği ekran görüntüsü.](./media/monitor-sign-in-health-for-resilience/save-query.png)](./media/monitor-sign-in-health-for-resilience/save-query.png)



### <a name="refine-your-queries-and-alerts"></a>Sorgularınızı ve uyarılarınızı daraltın
Sorgularınızı ve uyarılarınızı en yüksek verimlilik için değiştirin.

* Uyarılarınızı test ettiğinizden emin olun.

* Önemli bildirimler almanız için uyarı duyarlılığını ve sıklığını değiştirin. Yöneticiler çok fazla ediniyorlarsa ve önemli bir şeyi kaçırırsa Desensitized uyarılar verebilir. 

* Yöneticinin e-posta istemcilerinde gelen uyarıların izin verilen Gönderenler listesine eklendiğinden emin olun. Aksi takdirde, e-posta istemcinizdeki bir istenmeyen posta Filtresi nedeniyle bildirimleri kaçırırdınız. 

* Azure Izleyici 'de uyarılar sorgusu, yalnızca geçmiş 48 saatten sonuçları içerebilir. [Bu, tasarıma göre geçerli bir kısıtlamadır](https://github.com/MicrosoftDocs/azure-docs/issues/22637).

## <a name="create-processes-to-manage-alerts"></a>Uyarıları yönetmek için işlem oluşturma

Sorgu ve uyarıları ayarladıktan sonra, uyarıları yönetmek için iş süreçlerini oluşturun.

* Çalışma kitabını kim ne zaman izlecektir?
* Bir uyarı oluşturulduğunda, kimler araştıracaktır?

* İletişim ihtiyaçları nelerdir? İletişimleri kimler oluşturacak ve kimler alacak?

* Bir kesinti oluşursa, hangi iş süreçlerinin tetiklenmesi gerekir?

## <a name="next-steps"></a>Sonraki adımlar

[Çalışma kitapları hakkında daha fazla bilgi](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks)

 

 

 
