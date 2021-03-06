---
title: Güvenlik
description: App Service uygulamanızın güvenliğini sağlamaya nasıl yardımcı olduğunu ve uygulamanızı tehditlere karşı daha da nasıl kilitleyeceğinizi öğrenin.
keywords: Azure App Service, Web uygulaması, mobil uygulama, API uygulaması, işlev uygulaması, güvenlik, güvenli, güvenli, uyumluluk, uyumlu, sertifika, sertifikalar, https, FTPS, TLS, güven, şifreleme, şifreleme, şifreli, IP kısıtlaması, kimlik doğrulama, yetkilendirme, AuthN, autho, MSI, yönetilen hizmet kimliği, yönetilen kimlik, gizlilikler, gizli dizi, düzeltme eki, düzeltme eki, yam
ms.topic: article
ms.date: 08/24/2018
ms.custom: seodec18
ms.openlocfilehash: 61bffcfa8b98ed666e450c344023258b752e4880
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98736114"
---
# <a name="security-in-azure-app-service"></a>Azure App Service'te güvenlik

Bu makalede, [Azure App Service](overview.md) Web uygulamanızın, mobil uygulamanızın arka UCUNUN, API uygulamanızın ve [işlev uygulamanızın](../azure-functions/index.yml)güvenliğinin sağlanmasına nasıl yardımcı olduğu gösterilmektedir. Ayrıca, yerleşik App Service özellikleriyle uygulamanızı nasıl daha güvenli hale kullanabileceğinizi gösterir.

[!INCLUDE [app-service-security-intro](../../includes/app-service-security-intro.md)]

Aşağıdaki bölümlerde App Service uygulamanızı tehditlere nasıl daha fazla koruyabileceğiniz gösterilmektedir.

## <a name="https-and-certificates"></a>HTTPS ve sertifikalar

App Service, uygulamalarınızı [https](https://wikipedia.org/wiki/HTTPS)ile korumanıza olanak sağlar. Uygulamanız oluşturulduğunda, varsayılan etki alanı adı ( \<app_name> . azurewebsites.net) HTTPS kullanılarak zaten erişilebilir. [Uygulamanız için özel bir etki alanı yapılandırırsanız](app-service-web-tutorial-custom-domain.md), istemci tarayıcılarının özel etki alanınız IÇIN güvenli HTTPS bağlantıları yapabilmesi için [bunu BIR TLS/SSL sertifikasıyla de güvenli](configure-ssl-bindings.md) hale getirin. App Service tarafından desteklenen çeşitli sertifika türleri vardır:

- Ücretsiz App Service yönetilen sertifika
- App Service sertifikası
- Üçüncü taraf sertifika
- Azure Key Vault sertifika içeri aktarıldı

Daha fazla bilgi için [Azure App Service BIR TLS/SSL sertifikası ekleme](configure-ssl-certificate.md)bölümüne bakın.

## <a name="insecure-protocols-http-tls-10-ftp"></a>Güvenli olmayan protokoller (HTTP, TLS 1,0, FTP)

Uygulamanızın tüm şifrelenmemiş (HTTP) bağlantılara karşı güvenliğini sağlamak için App Service HTTPS 'yi zorlamak için tek tıklamayla yapılandırma sağlar. Güvenli olmayan istekler uygulama kodunuza ulaşmadan önce kapalıdır. Daha fazla bilgi için bkz. [https 'Yi zorlama](configure-ssl-bindings.md#enforce-https).

[TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1,0 artık [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)gibi sektör standartları tarafından güvenli olarak kabul edilmez. App Service, [TLS 1.1/1.2 'yi zorlayarak](configure-ssl-bindings.md#enforce-tls-versions)güncel olmayan protokolleri devre dışı bırakmanızı sağlar.

App Service, dosyalarınızı dağıtmak için hem FTP hem de FTPS 'yi destekler. Ancak, tüm mümkünse, FTPS 'leri FTP yerine kullanılmalıdır. Bu protokollerin biri veya her ikisi kullanımda olmadığında, [bunları devre dışı bırakmanız](deploy-ftp.md#enforce-ftps)gerekir.

## <a name="static-ip-restrictions"></a>Statik IP kısıtlamaları

Varsayılan olarak, App Service uygulamanız internet 'ten gelen tüm IP adreslerinden gelen istekleri kabul eder, ancak bu erişimi, IP adreslerinin küçük bir alt kümesiyle sınırlayabilirsiniz. Windows üzerinde App Service, uygulamanıza erişmelerine izin verilen IP adreslerinin bir listesini tanımlamanızı sağlar. İzin verilen liste, tek tek IP adreslerini veya bir alt ağ maskesi tarafından tanımlanan bir IP adresi aralığını içerebilir. Daha fazla bilgi için bkz. [Azure App Service STATIK IP kısıtlamaları](app-service-ip-restrictions.md).

Windows üzerinde App Service için _web.config_ yapılandırarak IP adreslerini dinamik olarak da kısıtlayabilirsiniz. Daha fazla bilgi için bkz. [dınamık IP \<dynamicIpSecurity> güvenliği](/iis/configuration/system.webServer/security/dynamicIpSecurity/).

## <a name="client-authentication-and-authorization"></a>İstemci kimlik doğrulaması ve yetkilendirme

Azure App Service, kullanıcılara veya istemci uygulamalarına yönelik anahtar kimlik doğrulaması ve yetkilendirme sağlar. Etkinleştirildiğinde, kullanıcılar ve istemci uygulamalarında çok az uygulama kodu olmadan oturum açabilir. Kendi kimlik doğrulama ve yetkilendirme çözümünüzü uygulayabilir veya App Service bunun yerine sizin için işlemesini sağlayabilirsiniz. Kimlik doğrulama ve yetkilendirme modülü, Web isteklerini uygulama kodunuza teslim etmeden önce işler ve kodunuza ulaşmadan önce yetkisiz istekleri reddeder.

App Service kimlik doğrulaması ve yetkilendirme, Azure Active Directory, Microsoft hesapları, Facebook, Google ve Twitter gibi birden çok kimlik doğrulama sağlayıcısını destekler. Daha fazla bilgi için bkz. [Azure App Service’de kimlik doğrulama ve yetkilendirme](overview-authentication-authorization.md).

## <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması

Bir arka uç hizmetinde kimlik doğrulanırken App Service, gereksinimlerinize bağlı olarak iki farklı mekanizma sağlar:

- **Hizmet kimliği** -uygulamanın kimliğini kullanarak uzak kaynakta oturum açın. App Service, [Azure SQL veritabanı](/azure/sql-database/) veya [Azure Key Vault](../key-vault/index.yml)gibi diğer hizmetlerle kimlik doğrulamak için kullanabileceğiniz bir [yönetilen kimliği](overview-managed-identity.md)kolayca oluşturmanıza olanak tanır. Bu yaklaşımın uçtan uca bir öğreticisi için bkz. [yönetilen kimlik kullanarak App Service Azure SQL veritabanı bağlantısını güvenli hale getirme](app-service-web-tutorial-connect-msi.md).
- **Adına (OBO)** -Kullanıcı adına uzak kaynaklara erişim yetkisi alın. Kimlik doğrulama sağlayıcısı olarak Azure Active Directory, App Service uygulamanız [Microsoft Graph API 'si](../active-directory/develop/microsoft-graph-intro.md) veya App Service BIR uzak API uygulaması gibi uzak bir hizmette temsilci olarak oturum açma işlemi gerçekleştirebilir. Bu yaklaşımın uçtan uca bir öğreticisi için bkz. [kimlik doğrulaması ve kullanıcıların Azure App Service içinde uçtan uca yetki verme](tutorial-auth-aad.md).

## <a name="connectivity-to-remote-resources"></a>Uzak kaynaklara bağlantı

Uygulamanızın erişmesi gerekebilecek üç tür uzak kaynak vardır: 

- [Azure kaynakları](#azure-resources)
- [Bir Azure sanal ağı içindeki kaynaklar](#resources-inside-an-azure-virtual-network)
- [Şirket içi kaynaklar](#on-premises-resources)

Bu durumların her birinde, App Service güvenli bağlantılar oluşturmanız için bir yol sağlar, ancak yine de en iyi güvenlik yöntemlerini gözlemleyebilirsiniz. Örneğin, arka uç kaynağı şifrelenmemiş bağlantılara izin veriyorsa bile her zaman şifreli bağlantıları kullanın. Ayrıca, arka uç Azure hizmetinizin en düşük IP adresi kümesine izin verdiğinden emin olun. Uygulamanızın giden IP adreslerini [gelen ve gıden IP adreslerinde Azure App Service](overview-inbound-outbound-ips.md)bulabilirsiniz.

### <a name="azure-resources"></a>Azure kaynakları

Uygulamanız [SQL veritabanı](https://azure.microsoft.com/services/sql-database/) ve [Azure depolama](../storage/index.yml)gibi Azure kaynaklarına bağlanıyorsa, bağlantı Azure 'da kalır ve ağ sınırlarının dışına geçmez. Bununla birlikte, bağlantı Azure 'daki paylaşılan ağ üzerinden geçer, her zaman bağlantınızın şifrelendiğinden emin olun. 

Uygulamanız bir [App Service ortamda](environment/intro.md)barındırılıyorsa, [sanal ağ hizmet uç noktalarını kullanarak desteklenen Azure hizmetlerine bağlanmanız](../virtual-network/virtual-network-service-endpoints-overview.md)gerekir.

### <a name="resources-inside-an-azure-virtual-network"></a>Bir Azure sanal ağı içindeki kaynaklar

Uygulamanız, [sanal ağ tümleştirmesiyle](web-sites-integrate-with-vnet.md)bir [Azure sanal ağındaki](../virtual-network/index.yml) kaynaklara erişebilir. Tümleştirme, Noktadan siteye VPN kullanan bir sanal ağ ile oluşturulur. Uygulama daha sonra özel IP adreslerini kullanarak sanal ağdaki kaynaklara erişebilir. Bununla birlikte, Noktadan siteye bağlantı, hala Azure 'daki paylaşılan ağlara geçer. 

Kaynak bağlantınızı Azure 'daki paylaşılan ağlardan tamamen yalıtmak için uygulamanızı bir [App Service ortamda](environment/intro.md)oluşturun. App Service ortamı her zaman ayrılmış bir sanal ağa dağıtıldığı için, sanal ağ içindeki uygulamanız ve kaynaklarınız arasındaki bağlantı tamamen yalıtılmıştır. App Service ortamındaki ağ güvenliğinin diğer yönleri için bkz. [ağ yalıtımı](#network-isolation).

### <a name="on-premises-resources"></a>Şirket içi kaynaklar

Veritabanları gibi şirket içi kaynaklara üç yolla güvenle erişebilirsiniz: 

- [Karma bağlantılar](app-service-hybrid-connections.md) -TCP tüneli aracılığıyla uzak kaynağınız için noktadan noktaya bağlantı kurar. TCP tüneli, paylaşılan erişim imzası (SAS) anahtarlarıyla TLS 1,2 kullanılarak oluşturulur.
- Siteden siteye VPN ile [sanal ağ tümleştirmesi](web-sites-integrate-with-vnet.md) - [bir Azure sanal ağı Içindeki kaynaklarda](#resources-inside-an-azure-virtual-network)açıklandığı gibi, sanal ağ, [siteden siteye VPN](../vpn-gateway/tutorial-site-to-site-portal.md)aracılığıyla şirket içi ağınıza bağlanabilir. Bu ağ topolojisinde, uygulamanız sanal ağdaki diğer kaynaklar gibi şirket içi kaynaklara bağlanabilir.
- [Bir Azure sanal ağı Içindeki kaynaklarda](#resources-inside-an-azure-virtual-network)açıklandığı gibi, sıteden siteye vpn ile [App Service ortamı](environment/intro.md) , ancak sanal ağ, [siteden siteye VPN](../vpn-gateway/tutorial-site-to-site-portal.md)aracılığıyla şirket içi ağınıza bağlanabilir. Bu ağ topolojisinde, uygulamanız sanal ağdaki diğer kaynaklar gibi şirket içi kaynaklara bağlanabilir.

## <a name="application-secrets"></a>Uygulama gizli dizileri

Kodunuzda veya yapılandırma dosyalarınızda veritabanı kimlik bilgileri, API belirteçleri ve özel anahtarlar gibi uygulama gizli dizileri depolamayın. Yaygın olarak kabul edilen yaklaşım, bunlara seçtiğiniz dilde standart bir model kullanılarak [ortam değişkenleri](https://wikipedia.org/wiki/Environment_variable) olarak erişiyor. App Service, ortam değişkenlerini tanımlamanın yolu [uygulama ayarlarından](configure-common.md#configure-app-settings) (ve özellikle .NET uygulamaları, [bağlantı dizeleri](configure-common.md#configure-connection-strings)için) yapılır. Uygulama ayarları ve bağlantı dizeleri Azure 'da şifrelenir ve uygulama başlatıldığında yalnızca uygulamanızın işlem belleğine eklenmeden önce şifresi çözülür. Şifreleme anahtarları düzenli olarak döndürülür.

Alternatif olarak, gelişmiş gizlilikler yönetimi için App Service uygulamanızı [Azure Key Vault](../key-vault/index.yml) tümleştirebilirsiniz. [Key Vault yönetilen bir kimlikle](../key-vault/general/tutorial-net-create-vault-azure-web-app.md)App Service uygulamanız, ihtiyacınız olan gizli bilgilere güvenli bir şekilde erişebilir.

## <a name="network-isolation"></a>Ağ yalıtımı

**Yalıtılmış** fiyatlandırma katmanı hariç tüm katmanlar, uygulamalarınızı App Service paylaşılan ağ altyapısında çalıştırır. Örneğin, genel IP adresleri ve ön uç yük dengeleyiciler diğer kiracılar ile paylaşılır. **Yalıtılmış** katman, uygulamalarınızı adanmış bir [App Service ortamı](environment/intro.md)içinde çalıştırarak tamamen ağ yalıtımı sağlar. Bir App Service ortamı, kendi [Azure sanal ağı](../virtual-network/index.yml)Örneğinizde çalışır. Şunları yapmanızı sağlar: 

- Özel ön uçlarla, özel bir ortak uç nokta aracılığıyla uygulamalarınıza yönelik olarak sunma.
- Yalnızca Azure sanal ağınızın içinden erişime izin veren iç yük dengeleyici (ıLB) kullanarak iç uygulama sunar. ILB, özel alt ağınızdan, internet 'ten uygulamalarınızın toplam yalıtımını sağlayan bir IP adresine sahiptir.
- [Web uygulaması güvenlik duvarı (WAF) arkasında BIR ıLB kullanın](environment/integrate-with-application-gateway.md). WAF, DDoS koruması, URI filtrelemesi ve SQL ekleme engellemesi gibi herkese yönelik uygulamalarınız için kurumsal düzeyde koruma sunar.

Daha fazla bilgi için bkz. [Azure App Service ortamlara giriş](environment/intro.md).