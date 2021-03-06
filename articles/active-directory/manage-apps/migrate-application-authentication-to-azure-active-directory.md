---
title: Uygulama kimlik doğrulamasını Azure Active Directory geçir
description: Bu Teknik İnceleme, uygulama kimlik doğrulamasını Azure AD 'ye geçirmeye yönelik planlama ve avantajların ayrıntılarını sağlar.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 02/05/2021
ms.author: kenwith
ms.reviewer: baselden
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd33f9e0b249db6b7c6bd0a0a556d0bb4cf79312
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100101620"
---
# <a name="migrate-application-authentication-to-azure-active-directory"></a>Uygulama kimlik doğrulamasını Azure Active Directory geçir

## <a name="about-this-paper"></a>Bu kağıt hakkında

Bu Teknik İnceleme, uygulama kimlik doğrulamasını Azure AD 'ye geçirmeye yönelik planlama ve avantajların ayrıntılarını sağlar. Azure yöneticileri ve kimlik uzmanları için tasarlanmıştır.

Her biri ayrıntılı planlama ve çıkış ölçütlerine sahip dört aşamaya kadar işlemi bölmek, geçiş stratejinizi planlamanıza ve Azure AD kimlik doğrulamasının kurumsal hedeflerinizi nasıl desteklediğini anlamanıza yardımcı olmak için tasarlanmıştır.

## <a name="introduction"></a>Giriş

Günümüzde kuruluşunuz, kullanıcıların işi yerine getirmek için uygulama (uygulamalar) için bir işlem gerektirir. Her gün uygulama eklemeye, geliştirmeye veya devre dışı bırakmaya devam edersiniz. Kullanıcılar bu uygulamalara çok sayıda şirket ve kişisel cihazdan ve konumlarından erişir. Uygulamalar aşağıdakiler de dahil olmak üzere birçok şekilde açılır:

- Şirket giriş sayfası veya portalı aracılığıyla

- tarayıcılarında bookişaretleme ile

- hizmet olarak yazılım (SaaS) uygulamaları için bir satıcının URL 'SI aracılığıyla

- bir mobil cihaz/uygulama yönetimi (MDM/MAM) çözümü aracılığıyla doğrudan kullanıcının masaüstüne veya mobil cihazlara gönderilen bağlantılar

Uygulamalarınız büyük olasılıkla aşağıdaki kimlik doğrulama türlerini kullanıyor:

- Şirket içi Federasyon çözümleri (Active Directory Federasyon Hizmetleri (AD FS) (ADFS) ve ping gibi)

- Active Directory (Kerberos kimlik doğrulaması ve Windows tümleşik kimlik doğrulaması gibi)

- Diğer bulut tabanlı kimlik ve erişim yönetimi (ıAM) çözümleri (okta veya Oracle gibi)

- Şirket içi web altyapısı (IIS ve Apache gibi)

- Bulutta barındırılan altyapı (Azure ve AWS gibi)

**Kullanıcıların uygulamalara kolayca ve güvenli bir şekilde erişebilmesini sağlamak için, hedefiniz şirket içi ve bulut ortamlarınızda tek bir erişim denetimi ve ilke kümesine sahip olmaktır.**

[Azure Active Directory (Azure AD)](/azure/active-directory/fundamentals/active-directory-whatis) , kişilerle, iş ortaklarınızın ve müşterilerinizin istedikleri uygulamalara erişmesi ve herhangi bir platformundan ve cihazdan işbirliği yapmasına olanak tanıyan bir evrensel kimlik platformu sağlar.

![Azure Active Directory bağlantısının diyagramı](media/migrating-application-authentication-to-azure-active-directory-1.jpg)

Azure AD ['nin eksiksiz bir kimlik yönetimi özellikleri paketi](/azure/active-directory/fundamentals/active-directory-whatis#which-features-work-in-azure-ad)vardır. Uygulama kimlik doğrulaması ve Azure AD yetkilendirmesi için standartlaştırın, bu yeteneklerin sağladığı avantajları elde etmenizi sağlar.

Diğer geçiş kaynaklarına bakın: [https://aka.ms/migrateapps](https://aka.ms/migrateapps)

## <a name="benefits-of-migrating-app-authentication-to-azure-ad"></a>Uygulama kimlik doğrulamasını Azure AD 'ye geçirme avantajları

Uygulama kimlik doğrulamasını Azure AD 'ye taşımak, risk ve maliyeti yönetmenize, üretkenliği artırmanıza ve uyumluluk ve idare gereksinimlerine karşı bir işlem yapmanıza yardımcı olur.

### <a name="manage-risk"></a>Riski yönetin

Uygulamalarınızın güvenliğini sağlamak, tüm risk faktörlerinin tam görünümüne sahip olmanızı gerektirir. Uygulamalarınızı Azure AD 'ye geçirmek, güvenlik çözümlerinizi birleştirir. Bununla birlikte şunları yapabilirsiniz:

- [Koşullu erişim ilkeleri](/azure/active-directory/active-directory-conditional-access-azure-portal), [Multi-Factor Authentication](/azure/active-directory/authentication/concept-mfa-howitworks)ve gerçek zamanlı risk tabanlı [kimlik koruma](/azure/active-directory/active-directory-identityprotection) teknolojilerini kullanarak uygulamalara ve ilişkili kurumsal verilere yönelik güvenli Kullanıcı erişimini geliştirir.

- [Tam zamanında](/azure/managed-applications/request-just-in-time-access) yönetici erişimi sayesinde ayrıcalıklı kullanıcının ortamınıza erişimini koruyun.

- En kritik iş gereksinimleriniz için [Azure AD 'nin çok kiracılı, coğrafi olarak dağıtılmış, yüksek kullanılabilirliğe sahip tasarımını](https://cloudblogs.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/)kullanın.

- Eski uygulamalarınızı, daha önce dağıttığınız [güvenli hibrit erişim iş ortağı tümleştirmelerimizden](https://aka.ms/secure-hybrid-access) biriyle koruyun.

### <a name="manage-cost"></a>Maliyeti yönetme

Kuruluşunuzun yerinde birden fazla kimlik erişim yönetimi (ıAM) çözümü olabilir. Tek bir Azure AD altyapısına geçiş yapmak, ıAM lisanslarındaki bağımlılıkları (Şirket içi veya bulutta) ve altyapı maliyetlerini azaltmaya yönelik bir fırsattır. Azure AD için M365 lisansları aracılığıyla zaten ücretli olabilecek durumlarda, başka bir ıAM çözümünün ek maliyetini ödemenizi gerektirmez.

**Azure AD ile altyapı maliyetlerini şu şekilde azaltabilirsiniz:**

- [Azure AD uygulama ara sunucusu](/azure/active-directory/manage-apps/application-proxy)kullanarak şirket içi uygulamalara güvenli uzaktan erişim sağlama.

- [Güvenilen evrensel kimlik sağlayıcısı olarak Azure AD 'yi ayarlayarak](/azure/active-directory/hybrid/plan-connect-user-signin#choosing-the-user-sign-in-method-for-your-organization)kiracınızdaki şirket içi kimlik bilgisi yaklaşımına yönelik uygulamalar ayırın.

### <a name="increase-productivity"></a>Üretkenliği artırın

Ekonomikler ve güvenlik avantajları, kuruluşların Azure AD 'yi benimsemesini sağlar, ancak tam benimseme ve uyumluluk, kullanıcılar da çok yararlıyor olabilir. Azure AD ile şunları yapabilirsiniz:

- Herhangi bir cihazdan ve herhangi bir yerden herhangi bir uygulamaya sorunsuz ve güvenli erişim yoluyla Son Kullanıcı [tek Sign-On (SSO)](/azure/active-directory/manage-apps/what-is-single-sign-on) deneyimini geliştirebilirsiniz.

- Self Servis [parola sıfırlama](/azure/active-directory/authentication/concept-sspr-howitworks) ve [selfservice Grup Yönetimi](/azure/active-directory/users-groups-roles/groups-self-service-management)gibi self servis IAM özelliklerinden yararlanın.

- Bulut ve şirket içi ortamlar genelinde her bir kullanıcı için yalnızca tek bir kimliği yöneterek yönetim yükünü azaltın:

  - Azure AD kimliklerine göre Kullanıcı hesaplarının ( [Azure AD Galerisi](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps)'nde) sağlanmasını [otomatik hale](/azure/active-directory/active-directory-saas-app-provisioning) getirme
  - [Azure Portal](https://portal.azure.com/) tüm uygulamalarınıza uygulamaps panelinden erişin

- Geliştiricilere Microsoft kimlik doğrulama kitaplığı (MSAL) ile [Microsoft Identity platformunu](/azure/active-directory/develop/about-microsoft-identity-platform) kullanarak uygulamalarına erişimi güvenli hale getirme ve son kullanıcı deneyimini geliştirme olanağı tanıyın.

- [Azure AD B2B işbirliğini](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)kullanarak iş ortaklarınızı bulut kaynaklarına erişimle güçlendirin. Bu, iş ortaklarınız ile noktadan noktaya Federasyonu yapılandırma yükünü ortadan kaldırır.

### <a name="address-compliance-and-governance"></a>Adres uyumluluğu ve idare

Kurumsal erişim ilkelerini zorlayarak ve tümleşik denetim araçları ve API 'Leri kullanarak kullanıcılara ve ilişkili verilere yönelik Kullanıcı erişimini izleyerek, mevzuat gereksinimleriyle uyumluluğu sağlayın. Azure AD ile uygulama oturum açma işlemlerini [güvenlik olayı ve olay izleme (SıEM) araçlarından](/azure/active-directory/reports-monitoring/plan-monitoring-and-reporting)yararlanan raporlar aracılığıyla izleyebilirsiniz. Portala veya API 'lerden raporlara erişebilir ve uygulamalarınıza erişimi olan kişileri programlı bir şekilde denetleyebilir ve erişim gözden geçirmeleri aracılığıyla etkin olmayan kullanıcılara erişimi kaldırabilirler.

## <a name="plan-your-migration-phases-and-project-strategy"></a>Geçiş aşamalarınızı ve proje stratejinizi planlayın

Teknoloji projeleri başarısız olduğunda, genellikle eşleşmeyen beklentiler, doğru proje katılımcılarının dahil olmaması veya iletişimin olmaması nedeniyle oluşur. Projenin kendisini planlayarak başarısından emin olun.

### <a name="the-phases-of-migration"></a>Geçişin aşamaları

Araçlara girmeden önce, geçiş işlemini nasıl düşündüğünü anlamalısınız. Birden çok doğrudan müşteri atölyeler aracılığıyla aşağıdaki dört aşamayı öneririz:

![Geçiş aşamalarının diyagramı](media/migrating-application-authentication-to-azure-active-directory-2.jpg)

### <a name="assemble-the-project-team"></a>Proje ekibini birleştirme

Uygulama geçişi bir ekip çabadır ve tüm önemli konumların doldurulduğundan emin olmanız gerekir. Kıdemli iş liderlerinden destek önemlidir. Doğru yönetim sponsorları, iş kararı mekanizmaları ve ilgili uzmanların (SMEs) bir kümesini belirttiğinizden emin olun.

Geçiş projesi sırasında, bir kişi birden çok rolü karşılamayabilir veya kuruluşunuzun boyutuna ve yapısına bağlı olarak her bir rolü yerine getiren birden fazla kişi vardır. Ayrıca güvenlik yataya bir anahtar rol oynatacak diğer ekiplere de bir bağımlılığı olabilir.

Aşağıdaki tabloda, temel roller ve bunların katkıları yer almaktadır:

| Rol          | Katkılar                                              |
| ------------- | ---------------------------------------------------------- |
| **Project Manager** | Projeyi şu şekilde gösteren proje Coach sorumlu:<br /> -yönetici desteği kazanın<br /> -paydaşlara getir<br /> -zamanlamaları, belgeleri ve iletişimleri yönetme |
| **Kimlik mimarı/Azure AD Uygulaması Yöneticisi** | Bunlar, aşağıdakilerden sorumludur:<br /> -çözümü paydaşlarla birlikte çalıştırma<br /> -işlemler ekibine iletim için çözüm tasarımını ve işletimsel yordamları belgeleyin<br /> -üretim öncesi ve üretim ortamlarını yönetme |
| **Şirket içi AD işlemler ekibi** | AD ormanları, LDAP dizinleri, HR sistemleri vb. gibi farklı şirket içi kimlik kaynaklarını yöneten kuruluş.<br /> -eşitlemeden önce gerekli tüm düzeltme görevlerini gerçekleştirin<br /> -Eşitleme için gereken hizmet hesaplarını belirtin<br /> -Federasyonu Azure AD 'ye yapılandırmak için erişim sağlayın |
| **BT Destek Yöneticisi** | BT destek kuruluşundan, bu değişikliğin bir yardım masası perspektifinden desteklenebilirliği hakkında giriş sağlayabilen bir temsilcisidir. |
| **Güvenlik sahibi**  | Bir güvenlik ekibinin temsilcisinden, planın kuruluşunuzun güvenlik gereksinimlerini karşıladığından emin olabilir. |
| **Uygulama teknik sahipleri** | Azure AD ile tümleştirilecek uygulama ve hizmetlerin teknik sahiplerini içerir. Bunlar, eşitleme işlemine içermesi gereken uygulamaların kimlik özniteliklerini sağlarlar. Genellikle CSV temsilcileriyle bir ilişkisi vardır. |
| **Uygulama iş sahipleri** | Kullanıcı deneyimine giriş ve bu değişikliğin bir kullanıcının perspektifinden sağladığı ve uygulamanın genel iş yönüne sahip olabileceği, erişimin yönetilmesine dahil olabilen temsilci iş arkadaşları. |
| **Pilot Kullanıcı grubu** | Günlük çalışmalarından bir parçası olarak test edecek kullanıcılar, pilot deneyim ve dağıtımların geri kalanını göstermeye yönelik geri bildirim sağlar. |

### <a name="plan-communications"></a>İletişimi planlama

Etkin iş katılımı ve iletişim, başarılı olma anahtarıdır. Paydaşlara ve son kullanıcılara bilgi almak ve zamanlama güncelleştirmeleri hakkında bilgi sahibi olmak için bir avento sağlamak önemlidir. Geçiş değeri, beklenen zaman çizelgelerini ve geçici iş kesintisini nasıl planlayabileceğinize ilişkin herkesi eğitin. Brim oturumları, e-postalar, bire bir toplantılar, başlık ve kasanskls gibi birden çok avenkullanın.

Uygulama için seçtiğiniz iletişim stratejisine bağlı olarak, bekleyen kapalı kalma süresini kullanıcılara hatırlatmak isteyebilirsiniz. Ayrıca, dağıtımı erteleyebilmeniz için gereken son değişiklik veya iş etkileri olmadığını da doğrulamanız gerekir.

Aşağıdaki tabloda, paydaşlarınızı bilgilendirmek için önerilen en düşük iletişimi bulacaksınız:

**Plan aşamaları ve proje stratejisi**:

| İletişim      | Hedef kitle                                          |
| ------------------ | ------------------------------------------------- |
| Projenin tanıma ve iş/teknik değeri | Son kullanıcılar hariç hepsi |
| Pilot uygulamalar için isteme | -Uygulama iş sahipleri<br />-Uygulama teknik sahipleri<br />-Mimarlar ve kimlik ekibi |

**1. aşama-bulma ve kapsam**:

| İletişim      | Hedef kitle                                          |
| ------------------ | ------------------------------------------------- |
| -Uygulama bilgileri için isteme<br />-Kapsamı kapsayan alıştırma sonucu | -Uygulama teknik sahipleri<br />-Uygulama iş sahipleri |

**2. aşama-uygulamaları sınıflandırma ve pilot planı planla**:

| İletişim      | Hedef kitle                                          |
| ------------------ | ------------------------------------------------- |
| -Sınıflandırmalar ve geçiş zamanlaması için ne anlama geldiğini gösteren sonuç<br />-Ön geçiş zamanlaması | -Uygulama teknik sahipleri<br /> -Uygulama iş sahipleri |

**3. aşama – geçişi ve testi planlayın**:

| İletişim      | Hedef kitle                                          |
| ------------------ | ------------------------------------------------- |
| -Uygulama geçişi testi sonucu | -Uygulama teknik sahipleri<br />-Uygulama iş sahipleri |
| -Geçişin geldiği ve sonuçta elde edilen son kullanıcı deneyimleri hakkında bildirim.<br />-Kesinti, geri bildirim ve nasıl yardım almak gerektiği dahil olmak üzere kapalı kalma süresi ve tam iletişimler | -Son kullanıcılar (ve diğer tüm) |

**4. aşama – öngörüleri yönetme ve elde** edin:

| İletişim      | Hedef kitle                                          |
| ------------------ | ------------------------------------------------- |
| Kullanılabilir analiz ve erişim | -Uygulama teknik sahipleri<br />-Uygulama iş sahipleri |

### <a name="migration-states-communication-dashboard"></a>Geçiş durumları iletişim panosu

Geçiş projesinin genel durumunu iletişim kurmak, ilerleme durumunu gösterdiği için çok önemlidir ve taşıma için hazırlanmak üzere uygulamalar, uygulamaları geçiş için kullanıma sunulacak olan uygulama sahiplerine yardımcı olur. Geçiş sırasında uygulamaların durumuna ilişkin görünürlük sağlamak için Power BI veya diğer raporlama araçlarını kullanarak basit bir panoyu birlikte yerleştirebilirsiniz.

Kullanmayı düşünebileceğiniz geçiş durumları şunlardır:

| Geçiş durumları       | Eylem planı                                   |
| ---------------------- | --------------------------------------------- |
| **İlk Istek** | Daha fazla bilgi için uygulamayı bulun ve sahibine başvurun |
| **Değerlendirme Tamam** | Uygulama sahibi uygulama gereksinimlerini değerlendirir ve uygulama anketini döndürür</td>
| **Yapılandırma devam ediyor** | Azure AD 'de kimlik doğrulamasını yönetmek için gerekli değişiklikleri geliştirin |
| **Test yapılandırması başarılı** | Değişiklikleri değerlendirin ve uygulamanın test ortamında Azure AD kiracısına karşı kimliğini doğrulayın |
| **Üretim yapılandırması başarılı** | Yapılandırma işlerini üretim AD kiracısıyla çalışacak şekilde değiştirin ve test ortamında uygulama kimlik doğrulamasını değerlendirin |
| **Tamamlanmış/oturumu Kapat** | Uygulama için değişiklikleri üretim ortamına dağıtın ve üretim Azure AD kiracısına karşı yürütün |

Bu, uygulama sahiplerinin uygulama geçişi ve test zamanlamasının, uygulamaları geçiş için ne zaman olduğunu ve sonuçların zaten geçirilmiş olan diğer uygulamalardan ne olduğunu bilmesini sağlayacaktır. Ayrıca, sahipler 'in geçirilmekte olan uygulamalarla ilgili sorunları dosya ve görüntüleyebilmeleri için hata izleyici veritabanınıza bağlantılar sağlamayı düşünebilirsiniz.

### <a name="best-practices"></a>En iyi uygulamalar

Müşteri ve iş ortaklarımızın başarı hikayeleri ve önerilen en iyi yöntemler aşağıda verilmiştir:

- Müşterilerin Microsoft bulut çözümlerini güvenli bir şekilde dağıtmasını sağlayan iş ortağı ağı 'nın bir üyesi olan Patriot danışmanlığı tarafından [Azure Active Directory için geçiş işlemini geliştirmenin beş ipucu](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Five-tips-to-improve-the-migration-process-to-Azure-Active/ba-p/445364) .

- IAM ve risk yönetimi çözümlerine odaklanan bir iş ortağı olan Edgile [Azure AD uygulama geçişiniz için bir risk yönetimi stratejisi geliştirin](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Develop-a-risk-management-strategy-for-your-Azure-AD-application/ba-p/566488) .

## <a name="phase-1-discover-and-scope-apps"></a>1. Aşama: uygulamaları bulma ve kapsama

**Uygulama bulma ve çözümleme, size iyi bir başlangıç sağlamak için temel bir uygulamadır.** Bilinmeyen uygulamalara uyum sağlamak için hazırlanan her şeyi bilmiyor olabilirsiniz.

### <a name="find-your-apps"></a>Uygulamalarınızı bulun

Bir uygulama geçişinin ilk karar noktası geçirilecek uygulamalar, hangilerinin kalması durumunda ve hangi uygulamaların kullanımdan kaldırılması gerekir. Kuruluşunuzda kullanmayacak uygulamaları kullanımdan kaldırmayı her zaman bir fırsat vardır. Kuruluşunuzda uygulama bulmanın birkaç yolu vardır. **Uygulamaları keşfederken, geliştirme ve planlı uygulamalar dahil olduğunuzdan emin olun. Azure AD 'yi, gelecekteki tüm uygulamalarda kimlik doğrulaması için kullanın.**

**Doğru uygulama envanterini toplamak Için Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanma:**

- **Azure AD Connect Health’i kullanma.** Azure AD Premium lisansınız varsa, şirket içi ortamınızda uygulama kullanımını çözümlemek için [Azure AD Connect Health](/azure/active-directory/hybrid/how-to-connect-health-adfs) dağıtmanız önerilir. Geçirilebilecek ADFS uygulamalarını ve geçirilecek uygulamanın hazır olduğunu değerlendirmek için [ADFS uygulama raporunu](/azure/active-directory/manage-apps/migrate-adfs-application-activity) (Önizleme) kullanabilirsiniz. Geçişinizi tamamladıktan sonra, bulutunuzda olduğunuzda kuruluşunuzda gölgeyi sürekli olarak izlemenize olanak tanıyan [Cloud Discovery](/cloud-app-security/set-up-cloud-discovery) dağıtın.

- **Günlük ayrıştırmayı AD FS**. Azure AD Premium lisansınız yoksa, PowerShell 'i temel alan Azure AD uygulama geçiş araçlarına ADFS 'yi kullanmanızı öneririz [.](https://github.com/AzureAD/Deployment-Plans/tree/master/ADFS%20to%20AzureAD%20App%20Migration) [Çözüm kılavuzuna](https://aka.ms/migrateapps/adfssolutionguide)başvurun:

[Uygulamaları Active Directory Federasyon Hizmetleri (AD FS) (AD FS) konumundan Azure AD 'ye geçirme.](https://aka.ms/migrateapps/adfssolutionguide)

### <a name="using-other-identity-providers-idps"></a>Diğer kimlik sağlayıcılarını (IDPs) kullanma

Diğer kimlik sağlayıcıları (okta veya PING gibi) için, kendi araçlarını kullanarak uygulama envanterini dışarı aktarabilirsiniz. Kuruluşunuzdaki Web uygulamalarına karşılık gelen Active Directory kayıtlı hizmet ilkelerine göz önünde bulundurmanız gerekebilir.

### <a name="using-cloud-discovery-tools"></a>Bulut bulma araçları 'nı kullanma

Bulut ortamında, tüm bulut hizmetlerinize yönelik olarak zengin görünürlük, veri yolculuğu üzerinde denetim ve gelişmiş analizler bulmanız ve bu tehditleri bulmaları gerekir. Aşağıdaki araçları kullanarak, bulut uygulaması envanterinizi toplayabilirsiniz:

- **Bulut erişim güvenlik Aracısı (casb**) – bir [casb](/cloud-app-security/) genellikle, çalışanlarınızın bulut uygulaması kullanımına yönelik görünürlük sağlamak ve kurumsal verilerinizi siber güvenlik tehditlerinden korumanıza yardımcı olması için güvenlik duvarınız ile birlikte çalışmaktadır. CASB raporu, kuruluşunuzda en sık kullanılan uygulamaları ve Azure AD 'ye geçirilecek erken hedefleri belirlemenize yardımcı olabilir.

- **Cloud Discovery** , [Cloud Discovery](/cloud-app-security/set-up-cloud-discovery)yapılandırarak, bulut uygulaması kullanımına yönelik görünürlük elde edersiniz ve Tasdiksiz veya gölge BT uygulamalarını bulabilir.

- **API 'ler** -bulut altyapısına bağlı uygulamalar için, barındırılan uygulamaların envanterini almak üzere bu sistemlerdeki API 'leri ve araçları kullanabilirsiniz. Azure ortamında:

  - Azure Web siteleri hakkında bilgi almak için [Get-AzureWebsite](/powershell/module/servicemanagement/azure/get-azurewebsite?view=azuresmps-4.0.0&redirectedfrom=MSDN&preserve-view=true)cmdlet 'ini kullanın.

  - Azure Web Apps hakkında bilgi almak için [Get-AzureRMWebApp](/powershell/module/azurerm.websites/get-azurermwebapp?view=azurermps-6.13.0&viewFallbackFrom=azurermps-6.2.0&preserve-view=true)cmdlet 'ini kullanın.

  - Microsoft IIS 'de çalışan tüm uygulamaları, [AppCmd.exe](/iis/get-started/getting-started-with-iis/getting-started-with-appcmdexe#working-with-sites-applications-virtual-directories-and-application-pools)kullanarak Windows komut satırından bulabilirsiniz.

  - Azure AD 'de bir dizinde uygulama ve uygulama örneği hakkında bilgi edinmek için [uygulamalar](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#application-entity) ve [hizmet sorumlularını](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity) kullanın.

### <a name="using-manual-processes"></a>El ile işlem kullanma

Yukarıda açıklanan otomatikleştirilmiş yaklaşımları tamamladıktan sonra, uygulamalarınızda iyi bir tanıtıcı olacaktır. Bununla birlikte, tüm Kullanıcı erişim alanlarında iyi kapsama sahip olduğunuzdan emin olmak için aşağıdakileri yapmayı düşünebilirsiniz:

- Kuruluşunuzda kullanımda olan uygulamaları bulmak için kuruluşunuzdaki çeşitli iş sahipleri ile iletişim kurun.

- Trafiğin yaygın olarak yönlendirildiği yerleri görmek için proxy sunucunuzda bir HTTP inceleme aracı çalıştırın veya proxy günlüklerini çözümleyin.

- Kullanıcıların en çok hangi bağlantılara erişebileceğini görmek için popüler şirket portalı sitelerinden Web günlüklerini gözden geçirin.

- İş açısından kritik uygulamaları kapsamış olduğunuzdan emin olmak için yöneticilere veya diğer önemli iş üyelerine ulaşın.

### <a name="type-of-apps-to-migrate"></a>Geçirilecek uygulamaların türü

Uygulamalarınızı bulduktan sonra, bu tür uygulamaları kuruluşunuzda tanımlayacaksınız:

- Modern kimlik doğrulama protokolleri kullanan uygulamalar zaten

- Modernleştirin için seçtiğiniz eski kimlik doğrulama protokollerini kullanan uygulamalar

- Modernleştirin ' yi seçtiğiniz eski kimlik doğrulama protokollerini kullanan uygulamalar

- Yeni Iş kolu (LoB) uygulamaları

### <a name="apps-that-use-modern-authentication-already"></a>Modern kimlik doğrulaması kullanan uygulamalar zaten

Zaten modernlanmış uygulamalar, Azure AD 'ye taşınabilecek en olası uygulamalardır. Bu uygulamalar zaten modern kimlik doğrulama protokolleri kullanıyor (SAML veya OpenID Connect gibi) ve Azure AD ile kimlik doğrulaması için yeniden yapılandırılabilir.

Bu [, Azure AD uygulama galerisindeki](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) seçeneklere ek olarak, kuruluşunuzda zaten mevcut olan veya Azure AD galerisinin ([Galeri olmayan uygulamalar)](/azure/active-directory/manage-apps/add-non-gallery-app)bir parçası olmayan bir satıcıdan herhangi bir üçüncü taraf uygulamanın bulunduğu uygulamalar olabilir.

Modernleştirin için seçtiğiniz eski uygulamalar

Modernleştirin etmek istediğiniz eski uygulamalar için, çekirdek kimlik doğrulaması ve yetkilendirme için Azure AD 'ye taşımak [Microsoft Graph](https://developer.microsoft.com/graph/gallery/?filterBy=Samples,SDKs) ve [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence?rtc=1) 'nin sunabileceği tüm güç ve veri zenginliğini kaldırır.

Bu uygulamalar için **kimlik doğrulama yığını kodunu** eski protokolden (Windows tümleşik kimlik doğrulaması, Kerberos kısıtlı temsılcısı, http üstbilgileri tabanlı kimlik doğrulaması gibi) modern bir PROTOKOLE (SAML veya OpenID Connect gibi) güncelleştirmenizi öneririz.

### <a name="legacy-apps-that-you-choose-not-to-modernize"></a>Modernleştirin için tercih ettiğiniz eski uygulamalar

Eski kimlik doğrulama protokollerini kullanan bazı uygulamalar için, bazen kimlik doğrulamasını modernleştirerek iş nedenleriyle ilgili doğru şey olmaz. Bunlar aşağıdaki uygulama türlerini içerir:

- Uygulamalar, uyumluluk veya denetim nedenleriyle şirket içinde tutulur.

- Değiştirmek istemediğiniz bir şirket içi kimliğe veya Federasyon sağlayıcısına bağlı uygulamalar.

- Taşıma planınız olmayan şirket içi kimlik doğrulama standartları kullanılarak geliştirilen uygulamalar

[Multi-Factor Authentication](/azure/active-directory/authentication/concept-mfa-howitworks), [koşullu erişim](/azure/active-directory/conditional-access/overview), [kimlik koruması](/azure/active-directory/identity-protection/), [uygulama erişimi temsilcisi](/azure/active-directory/manage-apps/access-panel-manage-self-service-access)ve bu uygulamalara yönelik [erişim gözden geçirmeleri](https://docs.microsoft.com/azure/active-directory/governance/manage-user-access-with-access-reviews#create-and-perform-an-access-review) gibi modern Azure AD GÜVENLIK ve BT özelliklerini uygulamaya dokunmadan uygulamaya dokunarak Azure AD, bu eski uygulamalara harika avantajlar verebilir!

Kullanıcılarınızın hızla geçirilmesini sağlamak için basit kimlik doğrulaması ( [parola kullanımı gibi](/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-password-vaulting) ) kullanarak veya zaten dağıtılan uygulama teslim denetleyicileriyle [iş ortağı tümleştirmelerinde](https://azure.microsoft.com/services/active-directory/sso/secure-hybrid-access/) , **Bu uygulamaları buluta genişleterek** başlatın.

### <a name="new-line-of-business-lob-apps"></a>Yeni Iş kolu (LoB) uygulamaları

Genellikle kuruluşunuzun şirket içi kullanımı için LoB uygulamaları geliştirirsiniz. İşlem hattında yeni uygulamalarınız varsa, OpenID Connect 'i uygulamak için [Microsoft Identity platformunu](/azure/active-directory/develop/about-microsoft-identity-platform) kullanmanızı öneririz.

### <a name="apps-to-deprecate"></a>Kullanımdan kalkmak için uygulamalar

Açık sahipleri olmayan uygulamalar ve bakım ve izleme, kuruluşunuz için bir güvenlik riski sunar. Uygulamaları kullanımdan kaldırmayı göz önünde bulundurun:

- işlevleri diğer sistemlerle **çok fazla yedekli** • **iş sahibi yok**

- açıkça **kullanım** yoktur.

Kuşkusuz, **iş açısından kritik uygulamaları yüksek ölçüde etkilemez**. Bu durumlarda, doğru stratejiyi öğrenmek için iş sahipleriyle birlikte çalışın.

### <a name="exit-criteria"></a>Çıkış kriterleri

Şu ile bu aşamada başarılı olursunuz:

- Geçişiniz için kapsamdaki sistemleri (Azure AD 'ye taşındıktan sonra devre dışı bırakabileceğinizi) iyi bir şekilde anlama

- Şunları içeren uygulamaların listesi:

  - Bu uygulamaların hangi sistemler tarafından, hangi cihazlarda ve kullanıcıların bunlara erişebileceğini

  - [Azure AD Connect](/azure/active-directory/hybrid/whatis-azure-ad-connect)geçirilip geçirilmeyeceğini, kullanımdan kalkmı veya bağlanmayacağını belirtir.

> [!NOTE]
> Azure AD kimlik doğrulamasına geçirmek istediğiniz uygulamaları ve [Azure AD Connect](/azure/active-directory/hybrid/whatis-azure-ad-connect)kullanarak bırakmak, ancak yönetmek istediklerinizi kaydetmek Için [uygulama bulma çalışma sayfasını](https://download.microsoft.com/download/2/8/3/283F995C-5169-43A0-B81D-B0ED539FB3DD/Application%20Discovery%20worksheet.xlsx) indirebilirsiniz.

## <a name="phase-2-classify-apps-and-plan-pilot"></a>2. Aşama: uygulamaları sınıflandırma ve pilot planı planla

Uygulamalarınızın geçişini sınıflandırmaya yönelik önemli bir uygulamadır. Her uygulamanın aynı anda geçirilmesi ve geçirilmesi gerekmez. Uygulamaların her biri hakkında bilgi topladıktan sonra, hangi uygulamaların ilk olarak geçirilmesi gerektiğini ve ne zaman eklenebileceğini bir araya getirebilirsiniz.

### <a name="classify-in-scope-apps"></a>Kapsam içi uygulamaları sınıflandırma

Bunu düşünmenin bir yolu, her biri birden çok etkene bağlı olan iş gözlüğü, kullanım ve ömrü için olan eksenlerdir.

### <a name="business-criticality"></a>İş açısından önem derecesi

İş önemi her işletme için farklı boyutlarda olacaktır, ancak dikkate almanız gereken iki ölçü **Özellikler, işlevler** ve **Kullanıcı profilleridir**. Benzersiz işlevlere sahip uygulamaları, gereksiz veya artık kullanılmayan işlevlerle daha yüksek bir nokta değeri ile atayın.

![Işlevlerin ve Kullanıcı profillerinin &, özelliklerin bir diyagramı](media/migrating-application-authentication-to-azure-active-directory-3.jpg)

### <a name="usage"></a>Kullanım

**Yüksek kullanım numaralarına** sahip uygulamalar, düşük kullanımlı uygulamalardan daha yüksek bir değer almalıdır. Dış, Executive veya güvenlik ekibi kullanıcıları olan uygulamalara daha yüksek bir değer atayın. Geçiş portföyünüzdeki her bir uygulama için bu değerlendirmeleri doldurun.

![Kullanıcı birimi ve Kullanıcı sınırları 'nın yansımalı bir diyagramı](media/migrating-application-authentication-to-azure-active-directory-4.jpg)

İş önemi ve kullanımı için değerleri belirledikten sonra, **uygulama ömrü**' ni belirleyebilir ve bir öncelik matrisi oluşturabilirsiniz. Aşağıdaki gibi bir matrisi inceleyin:

![Kullanım, beklenen Lifespan ve Iş önemi arasındaki ilişkileri gösteren üçgen diyagramı](media/migrating-application-authentication-to-azure-active-directory5.jpg)

### <a name="prioritize-apps-for-migration"></a>Geçiş için uygulamaları önceliklendir

Kuruluşunuzun ihtiyaçlarına bağlı olarak en düşük öncelikli uygulamalarla veya en yüksek öncelikli uygulamalarla uygulama geçişine başlama işlemini seçebilirsiniz.

Azure AD ve kimlik hizmetlerini kullanma deneyiminizin olmadığı bir senaryoda, **En düşük öncelikli uygulamalarınızı** önce Azure AD 'ye taşımayı göz önünde bulundurun. Bu, işletmenizin etkisini en aza indirir ve itici güç derleyebilirsiniz. Bu uygulamaları başarıyla taşıdıktan ve Paydaş 'in güvenini kazandıktan sonra, diğer uygulamaları geçirmeye devam edebilirsiniz.

Net bir öncelik yoksa, önce [Azure AD galerisinde](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) olan uygulamaları taşımak ve daha kolay tümleştirilebilmeleri için birden çok kimlik SAĞLAYıCıSıNı (ADFS veya okta) desteklemeyi göz önünde bulundurmanız gerekir. Bu uygulamaların kuruluşunuzdaki **en yüksek öncelikli uygulamalar** olması olasıdır. SaaS uygulamalarınızı Azure AD ile tümleştirmenize yardımcı olmak için, yapılandırma boyunca size kılavuzluk eden bir [öğretici](/azure/active-directory/saas-apps/tutorial-list) koleksiyonu geliştirdik.

Uygulamaları geçirmek için bir son tarih olduğunda, bu en yüksek öncelikli uygulamalar demeti ana iş yükünü alır. Son tarihi taşımış olsanız bile maliyeti değiştirdiklerinden, daha düşük öncelikli uygulamaları seçebilirsiniz. Lisansı yenilemeniz gerekir olsa da, küçük bir miktar olacaktır.

Bu sınıflandırmanın yanı sıra, geçişinizin aciliyet 'e bağlı olarak, uygulama sahiplerinin uygulamalarının geçirilmesini sağlamak için katılım gereken bir **geçiş zamanlaması** yerleştirmeyi de düşünebilirsiniz. Bu işlemin sonunda, geçiş için öncelik alanındaki tüm uygulamaların bir listesine sahip olmanız gerekir.

### <a name="document-your-apps"></a>Uygulamalarınızı belgeleyin

İlk olarak, uygulamalarınız hakkında önemli ayrıntılar toplanarak başlayın. [Uygulama bulma çalışma sayfası](https://download.microsoft.com/download/2/8/3/283F995C-5169-43A0-B81D-B0ED539FB3DD/Application%20Discovery%20worksheet.xlsx), geçiş kararlarınızı hızlı bir şekilde yapmanıza yardımcı olur ve hiç zaman olmayacak şekilde iş grubunuza yönelik bir öneri almanızı sağlar.

Geçiş kararlarınızı oluşturmak için önemli olan bilgiler şunları içerir:

- **Uygulama adı** : Bu uygulama iş olarak bilinen nedir?

- **Uygulama türü** : Bu bir üçüncü taraf SaaS uygulaması mı? Özel bir iş kolu Web uygulaması API mi?

- **İş önemi** : yüksek önem derecesine sahip mi? Zayıf? Ya da aralarında bir yerde mi?

- **Kullanıcı erişimi birimi** – herkes bu uygulamaya mı, yoksa yalnızca birkaç kişiye mı erişebilsin mi?

- **Planlanmış** kullanım ömrü: Bu uygulama ne kadar süreyle olacaktır? 6 aydan daha az mı? 2 yıldan daha fazla mı?

- **Geçerli kimlik sağlayıcısı** : Bu uygulama Için birincil IDP nedir? Ya da yerel depolama alanını kullanıyor mu?

- **Kimlik doğrulama yöntemi** : uygulama açık standartları kullanarak kimlik doğrulaması yapar mi?

- **Uygulama kodunu güncelleştirmek Isteyip istemediğinizi planlıyor** musunuz – planlanmış veya etkin geliştirme kapsamında uygulama mi?

- **Uygulamayı şirket içinde tutmayı planladığınızdan** bağımsız olarak, uygulamayı veri merkezinizde uzun süreli tutmak mı istiyorsunuz?

- **Uygulamanın diğer uygulamalara veya API 'lere bağlı olup olmadığı** – uygulama şu anda diğer uygulamalara veya API 'lere çağrı yapmaz mı?

- **Uygulamanın Azure AD galerisinde olup olmadığı** ; Şu anda [Azure AD Galerisi](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps)ile tümleştirilmiş uygulama mı?

Daha sonra size yardımcı olacak, ancak anında geçiş kararı vermeniz gerekmeyen diğer veriler şunlardır:

- **Uygulama URL 'si** : kullanıcılar uygulamaya erişmek için nereye gidebilirim?

- **Uygulama açıklaması** : uygulamanın ne yaptığını açıklayan kısa bir açıklama nedir?

- **Uygulama sahibi** : iş kim, uygulama için ana POC mi?

- **Genel açıklamalar veya notlar** – uygulama veya işletme sahipliğiyle ilgili diğer genel bilgiler

Uygulamanızı sınıflandırdıktan ve ayrıntıları belgeledikten sonra, planlı geçiş stratejinize iş sahibi satın alma avantajlarından emin olun.

### <a name="plan-a-pilot"></a>Pilot planı planlayın

Pilot için seçtiğiniz uygulamalar, kuruluşunuzun anahtar kimliğini ve güvenlik gereksinimlerini temsil etmelidir ve uygulama sahiplerinden açık satın alma yapmanız gerekir. Pilots genellikle ayrı bir test ortamında çalışır. Dağıtım planları sayfasında, bkz. [pilot uygulamalar için en iyi yöntemler](/azure/active-directory/fundamentals/active-directory-deployment-plans#best-practices-for-a-pilot) .

**Dış iş ortaklarınız hakkında unutmayın.** Geçiş zamanlamalarına ve teste katıldıklarından emin olun. Son olarak, sorun olması durumunda yardım masasına erişmek için bir yol olduğundan emin olun.

### <a name="plan-for-limitations"></a>Sınırlamalar için plan

Bazı uygulamaların geçirilmesi kolay olsa da, diğerleri birden çok sunucu veya örnek nedeniyle daha uzun sürebilir. Örneğin, SharePoint geçişi özel oturum açma sayfaları nedeniyle daha uzun sürebilir.

Birçok SaaS uygulaması satıcısı, SSO bağlantısını değiştirmeye yönelik ücret ödeiyor. Bunlarla görüşün ve planı planlayın.

Ayrıca, Azure AD 'nin farkında olmanız gereken [hizmet sınırları ve kısıtlamaları](/azure/active-directory/users-groups-roles/directory-service-limits-restrictions) vardır.

### <a name="app-owner-sign-off"></a>Uygulama sahibi oturumunu kapatma

İş açısından kritik ve evrensel olarak kullanılan uygulamalar, pilot aşamada uygulamayı test etmek için bir grup pilot kullanıcının olması gerekebilir. Bir uygulamayı üretim öncesi veya pilot ortamda sınadıktan sonra, uygulama ve tüm kullanıcıların kimlik doğrulaması için Azure AD 'nin üretim kullanımına yönelik olarak, uygulama iş sahiplerinin performans üzerinde oturum açmasını sağlayın.

### <a name="plan-the-security-posture"></a>Güvenlik duruşunu planlayın

Geçiş işlemini çalıştırmadan önce, kurumsal kimlik sisteminiz için geliştirmek istediğiniz güvenlik duruşunu tam olarak göz önüne alın. Bu, bu değerli bilgi kümelerini toplamaya dayalıdır: verilerinize ve verilerinize, **verilerinize ve konumlarına erişen kimlik**.

### <a name="identities-and-data"></a>Kimlikler ve veriler

Çoğu kuruluş, sektör segmentine ve kuruluşlardaki iş işlevlerine göre farklılık gösteren kimlikler ve veri koruması hakkında belirli gereksinimlere sahiptir. Önceden tanımlanmış [koşullu erişim ilkeleri](/azure/active-directory/active-directory-conditional-access-azure-portal) ve ilgili yetenekler kümesi de dahil olmak üzere önerilerin [kimlik ve cihaz erişim yapılandırmalarına](/microsoft-365/enterprise/microsoft-365-policies-configurations) bakın.

Bu bilgileri, Azure AD ile tümleştirilen tüm hizmetlere erişimi korumak için kullanabilirsiniz. Bu öneriler, Microsoft güvenli puanı ile ve [Azure AD 'de kimlik puanı](/azure/active-directory/fundamentals/identity-secure-score)ile hizalanır. Puanın yardımıyla:

- Kimlik güvenliği duruşunuzu nesnel olarak ölçebilirsiniz

- Kimlik güvenliği geliştirmelerini planlayabilirsiniz

- Geliştirmelerinizin başarısını gözden geçirebilirsiniz

Bu ayrıca [kimlik altyapınızı güvenli hale getirmek için beş adımı](/azure/security/azure-ad-secure-steps)uygulamanıza yardımcı olur. Kuruluşunuz için bir başlangıç noktası olarak kılavuzunuzu kullanın ve kuruluşunuzun belirli gereksinimlerini karşılayacak şekilde ilkeleri ayarlayın.

### <a name="who-is-accessing-your-data"></a>Verilerinize kimler erişiyor?

Azure AD 'nin desteklediği uygulamalarınızın ve kaynaklarınızın iki ana kategorisi vardır:

- **İç:** Kimlik sağlayıcınız içinde hesapları olan çalışanlar, yükleniciler ve satıcılar. Bu, diğer çalışanlara karşı Yöneticiler veya liderlik için farklı kurallara sahip daha fazla özette bulunabilir.

- **Dış:** [Azure AD B2B işbirliği](/azure/active-directory/b2b/what-is-b2b) ile düzenli iş aşamasında kuruluşunuzla etkileşime geçen satıcılar, tedarikçiler, dağıtımcılar veya diğer iş ortakları.

Bu kullanıcılar için gruplar tanımlayabilir ve bu grupları farklı şekillerde doldurabilirsiniz. Bir yöneticinin bir gruba el ile üye eklemesi gerektiğini seçebilir veya selfserviceportal grup üyeliğini etkinleştirebilirsiniz. [Dinamik grupları](/azure/active-directory/users-groups-roles/groups-dynamic-membership)kullanarak belirtilen ölçütlere göre otomatik olarak gruplara üye ekleyen kurallar oluşturulabilir.

Dış kullanıcılar ayrıca, özel bir değerlendirme gerektiren müşterilere de başvurabilir. [Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-overview), ayrı bir ürün müşteri kimlik doğrulamasını destekler. Ancak, bu kağıdın kapsamı dışındadır.

### <a name="devicelocation-used-to-access-data"></a>Verilere erişmek için kullanılan cihaz/konum

Bir kullanıcının bir uygulamaya erişmek için kullandığı cihaz ve konum da önemlidir. Kurumsal ağınıza fiziksel olarak bağlı olan cihazlar daha güvenlidir. VPN üzerinden Ağ dışından yapılan bağlantılarda scrutlı gerekebilir.

![Kullanıcı konumu ve veri erişimi arasındaki ilişkiyi gösteren diyagram](media/migrating-application-authentication-to-azure-active-directory-6.jpg)

Kaynak, Kullanıcı ve cihazın bu yönlerini göz önünde bulundurarak [Azure AD koşullu erişim](/azure/active-directory/active-directory-conditional-access-azure-portal) özelliklerini kullanmayı seçebilirsiniz. Koşullu Erişim Kullanıcı izinlerinin ötesine geçer: bir kullanıcı veya grubun kimliği, kullanıcının bağlandığı ağ, kullandıkları cihaz ve uygulama ve erişmeye çalıştıkları veri türleri gibi faktörlerin birleşimini temel alır. Kullanıcıya verilen erişim, bu daha geniş koşullar kümesine uyum sağlar.

### <a name="exit-criteria"></a>Çıkış kriterleri

Şu durumlarda bu aşamada başarılı olursunuz:

- Uygulamalarınızı öğrenin
  - Geçirmek istediğiniz uygulamaları tam olarak belgelenmiş yapın
  - İş önemi, kullanım hacmi ve ömrü temelinde öncelikli uygulamalara sahip olma

- Pilot için gereksinimlerinizi temsil eden uygulamaları seçili yapın

- Önceliklendirme ve stratejinize iş sahibi satın alma

- Güvenlik sonrası ihtiyaçlarınızı ve bunların nasıl uygulanacağını anlayın

## <a name="phase-3-plan-migration-and-testing"></a>3. Aşama: geçişi ve testi planlayın

İş satın alma işlemini kazandıktan sonra, bir sonraki adım bu uygulamaları Azure AD kimlik doğrulamasına geçirmeye başlamadır.

### <a name="migration-tools-and-guidance"></a>Geçiş araçları ve kılavuz

Uygulamalarınızı Azure AD 'ye geçirmek için gereken kesin adımları izlemek üzere aşağıdaki araçları ve yönergeleri kullanın:

- **Genel geçiş kılavuzu** – uygulamalarınızı keşfetme, sınıflandırmak ve geçirmek IÇIN [Azure AD Apps geçiş araç setinde](https://aka.ms/migrateapps) Teknik İnceleme, Araçlar, e-posta şablonları ve uygulamalar anketini kullanın.

- **SaaS uygulamaları** – uçtan uca işleme yol göstermek Için [yüzlerce SaaS uygulama öğreticileri](/azure/active-directory/active-directory-saas-tutorial-list) listemize ve tam [Azure AD SSO dağıtım planına](https://aka.ms/ssodeploymentplan) göz atın.

- **Şirket içinde çalışan uygulamalar** – [Azure AD uygulama ara sunucusu hakkında](/azure/active-directory/manage-apps/application-proxy) bilgi edinin ve tam [Azure AD uygulama ara sunucusu dağıtım planını](https://aka.ms/AppProxyDPDownload) kullanarak hızlıca çalışmaya başlayın.

- **Geliştirdiğiniz uygulamalar** – adım adım [tümleştirme](/azure/active-directory/develop/active-directory-integrating-applications) ve [kayıt](/azure/active-directory/develop/active-directory-v2-app-registration) kılavuzumuzu okuyun.

Geçişten sonra, başarılı dağıtım kullanıcılarına Kullanıcı bildirerek iletişim gönderilmesini ve bunları yapması gereken yeni adımlara hatırlatmak isteyebilirsiniz.

### <a name="plan-testing"></a>Test planı

Geçiş işlemi sırasında, uygulamanız düzenli dağıtımlar sırasında kullanılan bir test ortamına zaten sahip olabilir. Bu ortamı geçiş testi için kullanmaya devam edebilirsiniz. Şu anda bir test ortamı yoksa, uygulama mimarisine bağlı olarak Azure App Service veya Azure sanal makinelerini kullanarak bir tane ayarlayabilirsiniz. Uygulama yapılandırmalarından geliştirme sırasında kullanmak üzere ayrı bir test Azure AD kiracısı ayarlamayı tercih edebilirsiniz. Bu kiracı temiz bir durumda başlayacak ve herhangi bir sistemle eşitlenecek şekilde yapılandırılmayacak.

Her uygulamayı test kullanıcısı ile oturum açarak test edebilir ve tüm işlevlerin geçişten önceki ile aynı olduğundan emin olun. Kullanıcıların [MFA](/active-directory/authentication/howto-mfa-userstates) veya [SSPR](/azure/active-directory/authentication/quickstart-sspr)ayarlarını güncelleştirmesi gereken testi sırasında veya bu işlevselliği geçiş sırasında ekliyorsanız, bunu Son Kullanıcı iletişim planınıza eklediğinizden emin olun. Bkz. [MFA](https://aka.ms/mfatemplates) ve [SSPR](https://aka.ms/ssprtemplates) Son Kullanıcı iletişim şablonları.

Uygulamaları geçirdikten sonra, geçişin başarılı olup olmadığını test etmek için [Azure portalına](https://aad.portal.azure.com/) gidin. Aşağıdaki yönergeleri izleyin:

- **Kurumsal uygulamalar &gt; tüm uygulamalar** ' ı seçin ve listeden uygulamanızı bulun.

- Uygulamaya en az bir kullanıcı veya grup atamak için **&gt; kullanıcıları ve grupları yönet '** i seçin.

- **&gt; Koşullu erişimi yönet**' i seçin. İlke listenizi gözden geçirin ve [koşullu erişim ilkesiyle](/azure/active-directory/active-directory-conditional-access-azure-portal)uygulamaya erişimi engellemediğinizden emin olun.

Uygulamanızı nasıl yapılandırdığınıza bağlı olarak, SSO 'nun düzgün çalıştığını doğrulayın.

| Kimlik doğrulaması türü      | Sınama                                             |
| ------------------------ | --------------------------------------------------- |
| **OAuth/OpenID Connect** | **Kurumsal uygulama &gt; izinleri** ' ni seçin ve uygulamanızda, uygulamanızın kullanıcı ayarlarında kullanılacak uygulamayı onaylarınızdan emin olun. |
| **SAML tabanlı SSO** | **Çoklu oturum açma** altında bulunan [Test SAML ayarları](/azure/active-directory/develop/howto-v1-debug-saml-sso-issues) düğmesini kullanın. |
| **Parola tabanlı SSO** | [Uygulamaps güvenli oturum açma uzantısını](/azure/active-directory/user-help/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension)indirip yükleyin. Bu uzantı, bir SSO işlemi kullanmanızı gerektiren kuruluşunuzun bulut uygulamalarından herhangi birini başlatmanıza yardımcı olur. |
| **[Uygulama Ara Sunucusu](/azure/active-directory/manage-apps/application-proxy)** | Bağlayıcının çalıştığından ve uygulamanıza atandığından emin olun. Daha fazla yardım için [uygulama proxy sorun giderme kılavuzunu](/azure/active-directory/manage-apps/application-proxy-troubleshoot) ziyaret edin. |

### <a name="troubleshoot"></a>Sorun giderme

Sorunlarla karşılaşırsanız, yardım almak için [uygulamalar sorun giderme kılavuzumuzu](https://aka.ms/troubleshoot-apps) inceleyin. Ayrıca bkz. [özel olarak geliştirilmiş bir uygulamada oturum açma sorunları](/azure/active-directory/manage-apps/application-sign-in-problem-custom-dev).

### <a name="plan-rollback"></a>Planı geri alma

Geçişiniz başarısız olursa en iyi strateji geri alma ve test etme. Geçiş sorunlarını hafifletmek için uygulayabileceğiniz adımlar şunlardır:

- Uygulamanızın mevcut yapılandırmasının **ekran görüntülerini alın** . Uygulamayı bir kez daha yeniden yapılandırmanız gerekiyorsa geri dönebilirsiniz.

- Ayrıca, bulut kimlik doğrulamasıyla ilgili sorunlar söz konusu olduğunda **eski kimlik doğrulamasına bağlantılar sağlamayı** da düşünebilirsiniz.

- Geçişinizi tamamlamadan önce, **mevcut yapılandırmanızı** önceki kimlik sağlayıcısıyla değiştirmeyin.

- **Birden çok IDP 'yi destekleyen uygulamaları** geçirerek başlayın. Bir sorun varsa, her zaman tercih edilen IDP yapılandırmasına geçiş yapabilirsiniz.

- Uygulama deneyiminizin sorun olması durumunda **yardım masasına** bir **geri bildirim düğmesine** veya işaretçilerine sahip olduğundan emin olun.

### <a name="exit-criteria"></a>Çıkış kriterleri

Bu aşamada şunları yaptığınızda başarılı olursunuz:

- Her uygulamanın nasıl geçirileceğini tespit edilir

- Geçiş araçları incelendi

- Test ortamları ve grupları dahil testiniz planlandı

- Planlı geri alma

## <a name="phase-4-plan-management-and-insights"></a>4. Aşama: yönetim ve Öngörüler planlayın

Uygulamalar geçirildikten sonra şunları yapmanız gerekir:

- Kullanıcılar güvenli bir şekilde erişebilir ve yönetebilir

- Kullanım ve uygulama sistem durumu ile ilgili Öngörüler elde edebilirsiniz

Aşağıdaki eylemleri kuruluşunuza uygun şekilde yapmanız önerilir.

### <a name="manage-your-users-app-access"></a>Kullanıcılarınızın uygulama erişimini yönetin

Uygulamaları geçirdikten sonra, Kullanıcı deneyiminizi birçok şekilde zenginleştirebilirsiniz

**Uygulamaları bulunabilir yapın**

Kullanıcıyı [Uygulamaps](/azure/active-directory/user-help/my-apps-portal-end-user-access#my-apps-secure-sign-in-extension)portalı deneyimine **işaret edin** . Burada, tüm bulut tabanlı uygulamalara, [Azure AD Connect](/azure/active-directory/hybrid/whatis-azure-ad-connect)kullanarak kullanılabilir hale getirme uygulamalarına ve [uygulama proxy 'si](/azure/active-directory/manage-apps/application-proxy) kullanarak uygulamalar bu uygulamalara erişim izinleri sağlanmış uygulamalara erişebilirler.

Kullanıcılarınıza uygulamalarını nasıl keşfedeceksiniz hakkında rehberlik edebilirsiniz:

- [Mevcut çoklu oturum açma](/azure/active-directory/active-directory-saas-custom-apps#existing-single-sign-on) özelliğini kullanarak **kullanıcılarınızı herhangi bir uygulamaya bağlayın**

- Bir uygulamaya [self servis uygulama erişimini](/azure/active-directory/application-access-self-service-how-to)etkinleştirin ve **Kullanıcıların seçtiğiniz uygulamaları eklemesini sağlayın**

- [Uygulamaları son kullanıcılardan gizleyin](/azure/active-directory/manage-apps/hide-application-from-user-portal) (varsayılan Microsoft uygulamaları veya diğer uygulamalar) **ve uygulamaları daha fazla bulunabilir hale getirmek** için

### <a name="make-apps-accessible"></a>Uygulamaları erişilebilir yapma

**Kullanıcıların mobil cihazlarından uygulamalara erişmesine Izin verin**. Kullanıcılar, [iOS](/azure/active-directory/manage-apps/hide-application-from-user-portal) 7,0 veya üzeri ya da [Android](/azure/active-directory/manage-apps/hide-application-from-user-portal) cihazlarda Intune tarafından yönetilen tarayıcıyla uygulamaps portalına erişebilir.

Kullanıcılar, **Intune tarafından yönetilen bir tarayıcı** indirebilir:

- **Android cihazlar için**, [Google Play mağazasından](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser)

- Apple [App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) 'dan **Apple cihazları Için** veya [uygulamalarım mobil uygulaması 'nı iOS için](https://apps.apple.com/us/app/my-apps-azure-active-directory/id824048653) karşıdan yükleyebilir

**Kullanıcıların uygulamalarını tarayıcı uzantısından açmasına izin ver.**

Kullanıcılar, [Chrome,](https://chrome.google.com/webstore/detail/my-apps-secure-sign-in-ex/ggjhpefgjjfobnfoldnjipclpcfbgbhl) [Firefox](https://addons.mozilla.org/firefox/addon/access-panel-extension/) veya [Microsoft Edge](https://www.microsoft.com/p/my-apps-secure-sign-in-extension/9pc9sckkzk84?rtc=1&activetab=pivot%3Aoverviewtab) 'de [uygulamaps güvenli oturum açma uzantısını indirebilir](https://www.microsoft.com/p/my-apps-secure-sign-in-extension/9pc9sckkzk84?rtc=1&activetab=pivot%3Aoverviewtab) ve uygulamaları, tarayıcı çubuğundan doğrudan şu şekilde başlatabilir:

- **Uygulamalarını arayın ve en son kullanılan uygulamalarının görüntülenmesini sağlayabilirsiniz**

- [Uygulama proxy](/azure/active-directory/manage-apps/application-proxy) 'sinde yapılandırdığınız Iç URL 'leri uygun dış URL 'Lere **otomatik olarak dönüştürün** . Kullanıcılarınız, nerede olurlarsa olsun, öğrendikleri bağlantılarla birlikte çalışabilir.

**Kullanıcıların uygulamalarını Office.com 'tan açmasına izin verin.**

Kullanıcılar, uygulamalarını aramak için [Office.com](https://www.office.com/) adresine gidebilir **ve en son kullanılan uygulamalarının** çalıştıkları yerden hemen böyle görünmesine neden olabilir.

### <a name="secure-app-access"></a>Güvenli uygulama erişimi

Azure AD, geçirilen uygulamalarınızı yönetmek için merkezi bir erişim konumu sağlar. [Azure Portal](https://portal.azure.com/) gidin ve aşağıdaki özellikleri etkinleştirin:

- **Uygulamalara güvenli Kullanıcı erişimi.** Cihaz durumu, konumu ve daha fazlasına bağlı olarak uygulamalara Kullanıcı erişimini güvenli hale getirmek için [koşullu erişim ilkelerini](/azure/active-directory/active-directory-conditional-access-azure-portal)veya [kimlik korumasını](/azure/active-directory/active-directory-identityprotection)etkinleştirin.

- **Otomatik sağlama.** Kullanıcıların erişmesi gereken çeşitli üçüncü taraf SaaS uygulamalarına sahip [kullanıcılar için otomatik sağlamayı](/azure/active-directory/manage-apps/user-provisioning) ayarlayın. Kullanıcı kimlikleri oluşturmaya ek olarak, durum veya rol değişikliği olarak Kullanıcı kimliklerinin bakımını ve kaldırılmasını içerir.

- **Kullanıcı erişim** **yönetimi** temsilcisi. Uygun şekilde, uygulamalarınıza Self Servis uygulama erişimini etkinleştirin ve *Bu uygulamalara erişimi onaylamak için bir iş onaylayan atayın*. Uygulama koleksiyonlarına atanan gruplar için [self servis grup yönetimi](/azure/active-directory/users-groups-roles/groups-self-service-management)'ni kullanın.

- **Yönetici erişimi temsilcisi.** kullanıcıya bir yönetici rolü (örneğin, uygulama Yöneticisi, bulut uygulaması Yöneticisi veya uygulama geliştiricisi) atamak için **Dizin rolü** kullanma.

### <a name="audit-and-gain-insights-of-your-apps"></a>Uygulamalarınızın öngörülerini denetleyin ve elde edin

Ayrıca, tüm uygulamalarınızı merkezi bir konumdan denetlemek için [Azure Portal](https://portal.azure.com/) de kullanabilirsiniz.

- **Kurumsal uygulamaları** kullanarak **uygulamanızı denetleyin** , en sevdiğiniz araçlarla tümleştirilecek [Azure AD Raporlama API](/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) 'sindeki bilgileri denetleyin veya aynı bilgilere erişin.

- **Kurumsal uygulamalar,** OAuth/OpenID Connect kullanan uygulamalar için izinler ' i kullanarak **bir uygulama için izinleri görüntüleyin** .

- **Kurumsal uygulamalar, oturum açma** bilgileri ile **oturum açma öngörülerini alın** . [Azure AD Raporlama API](/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) 'sinden aynı bilgilere erişin.

- [Azure AD PowerBI içerik paketi](/azure/active-directory/active-directory-reporting-power-bi-content-pack-how-to) 'nden **uygulamanızın kullanımını görselleştirin**

### <a name="exit-criteria"></a>Çıkış kriterleri

Şu durumlarda bu aşamada başarılı olursunuz:

- Kullanıcılarınıza güvenli uygulama erişimi sağlama

- Geçirilen uygulamaları denetlemek ve öngörüleri sağlamak için yönetin

### <a name="do-even-more-with-deployment-plans"></a>Dağıtım planlarından daha da fazlasını yapın

Dağıtım planları, uygulama geçişi senaryoları dahil olmak üzere Azure AD çözümlerinin iş değeri, planlama, uygulama adımları ve yönetimi konusunda size yol gösterir. Azure AD özelliklerini dağıtmaya ve almaya başlamak için ihtiyacınız olan her şeyi bir araya getirir. Dağıtım kılavuzlarında, Microsoft tarafından önerilen en iyi uygulamalar, Son Kullanıcı iletişimleri, planlama kılavuzlarınız, uygulama adımları, test çalışmaları ve daha fazlası gibi içerikler bulunur.

Birçok [dağıtım planı](https://aka.ms/deploymentplans) kullanım için kullanılabilir ve her zaman daha fazla yapıyoruz!

### <a name="contact-support"></a>Desteğe başvurun

Destek bileti oluşturmak veya izlemek ve sistem durumunu izlemek için aşağıdaki destek bağlantılarını ziyaret edin.

- **Azure desteği:** Herhangi bir Azure için [Microsoft desteği](https://azure.microsoft.com/support) çağırıp bir bileti açabilirsiniz

Microsoft ile Kurumsal Anlaşma göre kimlik dağıtımı sorunu.

- **FastTrack**: kurumsal taşınabilirlik ve GÜVENLIK (EMS) veya Azure AD Premium lisanslarını satın aldıysanız, [FastTrack programından](/enterprise-mobility-security/solutions/enterprise-mobility-fasttrack-program) dağıtım yardımını almaya uygun olursunuz.

- **Ürün mühendisliği ekibine katılın:** Milyonlarca kullanıcıyla büyük bir müşteri dağıtımında çalışıyorsanız, Microsoft hesabı takımdan veya bulut çözümleri Mimarinizden destek hakkına sahip olursunuz. Projenin dağıtım karmaşıklığına bağlı olarak, doğrudan [Azure Identity ürün Mühendisliği ekibi](https://aad.portal.azure.com/#blade/Microsoft_Azure_Marketplace/MarketplaceOffersBlade/selectedMenuItemId/solutionProviders) ile çalışabilirsiniz.

- **Azure AD kimlik blogu:** Doğrudan kimlik Mühendisliği ekibi tarafından sunulan en son ürün duyuruları, derin ve yol haritası bilgilerini kullanarak güncel kalmak için [Azure AD kimlik bloguna](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/bg-p/Identity) abone olun.
