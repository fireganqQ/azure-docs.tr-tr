---
title: Çok kiracılı arka uçlar
titleSuffix: Azure Application Gateway
description: Bu sayfada, Application Gateway’in çok kiracılı arka uç desteği için genel bir bakış sunulmuştur.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 06/09/2020
ms.author: victorh
ms.openlocfilehash: fe6ea6f348d796962141bd39ff858d891a29a2f6
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397697"
---
# <a name="application-gateway-support-for-multi-tenant-back-ends-such-as-app-service"></a>App Service gibi çok kiracılı arka uçlar için Application Gateway desteği

Web sunucularındaki çok kiracılı mimari tasarımlarda, aynı Web sunucusu örneğinde birden çok Web sitesi çalışmaktadır. Konak adları, barındırılan farklı uygulamalar arasında ayrım yapmak için kullanılır. Varsayılan olarak, application gateway istemciden gelen HTTP ana bilgisayar üst bilgisini değiştirmez ve üst bilgiyi değiştirilmemiş halde arka uca gönderir. Bu, NIC 'ler, sanal makine ölçek kümeleri, genel IP adresleri, iç IP adresleri ve FQDN 'ler gibi, doğru uç noktaya çözümlemek için belirli bir ana bilgisayar üst bilgisini veya SNı uzantısını kullanmadığı için iyi sonuç verir. Ancak, Azure App Service Web Apps ve Azure API yönetimi gibi birçok hizmet, doğası gereği çok kiracılı ve doğru uç noktaya çözümlemek için belirli bir ana bilgisayar üst bilgisini veya SNı uzantısını kullanır. Genellikle uygulamanın DNS adı, uygulama ağ geçidi ile ilişkili DNS adıdır, arka uç hizmetinin etki alanı adından farklıdır. Bu nedenle, uygulama ağ geçidi tarafından alınan özgün istekteki ana bilgisayar üst bilgisi, arka uç hizmetinin ana bilgisayar adıyla aynı değildir. Bu nedenle, uygulama ağ geçidindeki istekteki ana bilgisayar üst bilgisi arka uç hizmetinin ana bilgisayar adına değiştirilmediği için, çok kiracılı arka uçlar isteği doğru uç noktaya çözemeyebilir. 

Application Gateway kullanıcılara, arka ucun ana bilgisayar adına göre istekteki HTTP barındırma üst bilgisini geçersiz kılma olanağı tanıyan bir özellik sunar. Bu özellik Azure App Service web uygulamaları ve API Management gibi çok kiracılı arka uçlar için destek sağlar. Bu özellik hem v1 ve v2 standardı hem de WAF SKU’larında kullanılabilir. 

![Konak geçersiz kılma](./media/application-gateway-web-app-overview/host-override.png)

> [!NOTE]
> Bu, bir çok kiracılı kaynak olan Azure App Service 'ten farklı olarak, Ao 'un ayrılmış bir kaynak olduğundan, Azure App Service ortamı (Ao) için geçerli değildir.

## <a name="override-host-header-in-the-request"></a>İstekteki ana bilgisayar üst bilgisini geçersiz kıl

Bir konak geçersiz kılma belirtme özelliği, [http ayarlarında](./configuration-overview.md#http-settings) tanımlanmıştır ve kural oluşturma sırasında herhangi bir arka uç havuzuna uygulanabilir. Çok kiracılı arka uçlar için konak üstbilgisini ve SNı uzantısını geçersiz kılmanın aşağıdaki iki yolu desteklenir:

- Ana bilgisayar adını HTTP ayarlarında açıkça girilen sabit bir değere ayarlama yeteneği. Bu özellik, belirli HTTP ayarlarının uygulandığı arka uç havuzuna giden tüm trafik için ana bilgisayar üstbilgisinin bu değere geçersiz kılınmasını sağlar. Uçtan uca TLS kullanırken, bu geçersiz kılınan konak adı SNı uzantısında kullanılır. Bu özellik, bir arka uç havuzu grubunun gelen müşteri ana bilgisayar başlığından farklı bir ana bilgisayar üst bilgisi beklediği senaryolara olanak tanıyor.

- Ana bilgisayar adını, arka uç havuzu üyelerinin IP veya FQDN 'sinden türetebilme özelliği. HTTP ayarları Ayrıca, tek bir arka uç havuzu üyesinden ana bilgisayar adı türeme seçeneğiyle yapılandırılmışsa, arka uç havuzu üyesinin FQDN 'sinden dinamik olarak ana bilgisayar adını seçme seçeneği sağlar. Uçtan uca TLS kullanırken, bu ana bilgisayar adı FQDN 'den türetilir ve SNı uzantısında kullanılır. Bu özellik, bir arka uç havuzunun Azure Web Apps gibi iki ya da daha fazla kiracı PaaS hizmetine sahip olduğu ve isteğin her üyeye ait ana bilgisayar üst bilgisinin FQDN 'den türetilen ana bilgisayar adını içerdiği senaryolara olanak sağlar. Bu senaryoyu uygulamak için, arka uç havuzunda belirtilen bir konak üstbilgisini, arka uç havuzunda bahsedilen bir ana bilgisayar üst bilgisini dinamik olarak geçersiz kılacak bir oturum ana [bilgisayar adresini seçin](./configuration-http-settings.md#pick-host-name-from-back-end-address) adlı bir anahtar kullanıyoruz.  Örneğin, arka uç havuzu FQDN 'niz "contoso11.azurewebsites.net" ve "contoso22.azurewebsites.net" içeriyorsa, istek uygun arka uç sunucusuna gönderildiğinde, contoso.com olan özgün isteğin ana bilgisayar üst bilgisi contoso11.azurewebsites.net veya contoso22.azurewebsites.net üzerine yazılacak. 

  ![web uygulaması senaryosu](./media/application-gateway-web-app-overview/scenario.png)

Bu özellik sayesinde müşteriler, HTTP ayarları ve özel araştırmalardaki seçenekleri uygun yapılandırmaya göre belirtebilir. Bu ayar daha sonra bir kural kullanılarak bir dinleyiciye ve arka uç havuzuna bağlanır.

## <a name="special-considerations"></a>Özel Konular

### <a name="tls-termination-and-end-to-end-tls-with-multi-tenant-services"></a>Çok kiracılı hizmetlerle TLS sonlandırma ve uçtan uca TLS

Hem TLS sonlandırması hem de uçtan uca TLS şifrelemesi, çok kiracılı hizmetler ile desteklenir. Uygulama ağ geçidinde TLS sonlandırması için, uygulama ağ geçidi dinleyicisine eklenmek üzere TLS sertifikası gerekir. Bununla birlikte, uçtan uca TLS, Azure App Service Web Apps gibi güvenilen Azure Hizmetleri, uygulama ağ geçidinde arka uçlara izin vermeyi gerektirmez. Bu nedenle, herhangi bir kimlik doğrulama sertifikası eklemeniz gerekmez. 

![uçtan uca TLS](./media/application-gateway-web-app-overview/end-to-end-ssl.png)

Yukarıdaki görüntüde, App Service arka uç olarak seçildiğinde kimlik doğrulama sertifikalarının eklenmesi için bir gereksinim olmadığına dikkat edin.

### <a name="health-probe"></a>Durum yoklaması

**Http ayarlarındaki** ana bilgisayar üst bilgisini geçersiz kılmak yalnızca isteği ve onun yönlendirmesini etkiler. durum araştırma davranışını etkilemez. Uçtan uca işlevselliğin çalışması için hem araştırma hem de HTTP ayarları doğru yapılandırmayı yansıtacak şekilde değiştirilmelidir. Özel yoklamalar, araştırma yapılandırmasında bir ana bilgisayar üst bilgisi belirtme olanağı sağlamanın yanı sıra, ana bilgisayar üst bilgisini Şu anda yapılandırılmış HTTP ayarlarından türeme özelliğini de destekler. Bu yapılandırma, araştırma yapılandırmasındaki `PickHostNameFromBackendHttpSettings` parametresi kullanılarak belirtilebilir.

### <a name="redirection-to-app-services-url-scenario"></a>App Service URL senaryosuna yeniden yönlendirme

App Service 'in yanıtındaki ana bilgisayar adının, Application Gateway ilişkili etki alanı yerine Son Kullanıcı tarayıcısını *. azurewebsites.net ana bilgisayar adına yönlendirebilir. Bu sorun aşağıdaki durumlarda gerçekleşebilir:

- App Service yeniden yönlendirme yapılandırdınız. Yeniden yönlendirme, isteğe bir sondaki eğik çizgi eklemek kadar basit olabilir.
- Yeniden yönlendirmeye neden olan Azure AD kimlik doğrulaması var.

Bu tür durumları çözmek için bkz. [App Service 'ın URL sorunu için yeniden yönlendirme sorunlarını giderme](./troubleshoot-app-service-redirection-app-service-url.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure App Service Web uygulaması gibi çok kiracılı bir uygulamayla, arka uç havuzu üyesi olarak bir uygulama ağ geçidi ayarlamayı öğrenin [Application Gateway ile App Service Web Apps yapılandırma](./configure-web-app-portal.md) ziyaret edin
