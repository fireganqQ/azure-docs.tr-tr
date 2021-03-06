---
title: Azure AD rol açıklamaları ve izinleri-Azure Active Directory | Microsoft Docs
description: Yönetici rolü kullanıcı ekleyebilir, yönetici roller atayabilir, Kullanıcı parolalarını sıfırlayabilir, kullanıcı lisanslarını yönetebilir veya etki alanlarını yönetebilir.
services: active-directory
author: rolyon
manager: daveba
search.appverid: MET150
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: reference
ms.date: 02/01/2020
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro, fasttrack-edit
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0211388f3b638080c79a58330ba1fff1989def7d
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101095870"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Azure Active Directory'de yönetici rolü izinleri

Azure Active Directory (Azure AD) kullanarak, daha az ayrıcalıklı rollerdeki kimlik görevlerini yönetmek için sınırlı yöneticiler atayabilirsiniz. Yöneticiler, Kullanıcı ekleme veya değiştirme, yönetici rolleri atama, Kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme ve etki alanı adlarını yönetme gibi amaçlar için atanabilir. [Varsayılan Kullanıcı izinleri](../fundamentals/users-default-permissions.md) yalnızca Azure AD 'deki Kullanıcı ayarları ' nda değiştirilebilir.

## <a name="limit-use-of-global-administrator"></a>Genel yönetici kullanımını sınırla

Genel yönetici rolüne atanan kullanıcılar, Azure AD kuruluşunuzda her yönetim ayarını okuyabilir ve değiştirebilir. Varsayılan olarak, bir Kullanıcı bir Microsoft bulut hizmetine kaydolduğunda, bir Azure AD kiracısı oluşturulur ve Kullanıcı genel Yöneticiler rolünün bir üyesi yapılır. Mevcut bir kiracıya bir abonelik eklediğinizde, genel yönetici rolüne atanmamıştır. Yalnızca genel Yöneticiler ve ayrıcalıklı rol yöneticileri yönetici rollerini temsil edebilir. İşletmenize yönelik riski azaltmak için, bu rolü kuruluşunuzdaki en az olası kişilere atamanızı öneririz.

En iyi uygulama olarak, bu rolü kuruluşunuzda beşten az kişiden fazlasına atamanız önerilir. Kuruluşunuzda genel yönetici rolüne atanmış beş taneden fazla yönetici varsa, bunun kullanımını azaltmanın bazı yolları aşağıda verilmiştir.

### <a name="find-the-role-you-need"></a>İhtiyaç duyduğunuz rolü bulun

Birçok rolün bir listesini almanız gereken rolü bulmak için, Azure AD rol kategorilerine göre rollerin alt kümelerini gösterebilir. [Azure AD rolleri ve yöneticileri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) Için yeni **tür** filtreimize göz atın ve yalnızca seçili türdeki rolleri gösterin.

### <a name="a-role-exists-now-that-didnt-exist-when-you-assigned-the-global-administrator-role"></a>Genel yönetici rolünü atadığınızda artık mevcut olmayan bir rol var

Azure AD 'ye, bazı kullanıcıları küresel yöneticiye yükseltmiş bir seçenek olmayan daha ayrıntılı izinler sağlayan bir rol veya rol eklenmiş olabilir. Zaman içinde, yalnızca genel yönetici rolünün gerçekleştirebileceği görevleri gerçekleştiren ek roller sunuyoruz. Aşağıdaki [kullanılabilir rollere](#available-roles)yansıtıldığını görebilirsiniz.

## <a name="assign-or-remove-administrator-roles"></a>Yönetici rolleri atama veya kaldırma

Azure Active Directory ' de bir kullanıcıya yönetim rolleri atamayı öğrenmek için, bkz. [Azure Active Directory yönetici rollerini görüntüleme ve atama](manage-roles-portal.md).

> [!Note]
> Bir Azure AD Premium P2 lisansınız varsa ve zaten bir Privileged Identity Management (PıM) kullanıcısı varsa, tüm rol yönetimi görevleri Azure AD 'de değil, ayrıcalık kimlik yönetimi 'nde gerçekleştirilir.
>
> ![Zaten PıM kullanan ve Premium P2 lisansına sahip olan kullanıcılar için PıM 'de yönetilen Azure AD rolleri](./media/permissions-reference/pim-manages-roles-for-p2.png)

## <a name="available-roles"></a>Kullanılabilir roller

Aşağıdaki Yönetici rolleri kullanılabilir:

### <a name="application-administrator"></a>[Uygulama Yöneticisi](#application-administrator-permissions)

Bu roldeki kullanıcılar kurumsal uygulamaların, uygulama kayıtlarının ve uygulama proxy ayarlarının tüm yönlerini oluşturabilir ve yönetebilir. Bu role atanan kullanıcıların yeni uygulama kayıtları veya kurumsal uygulamalar oluştururken sahip olarak eklenmediğini unutmayın.

Bu rol Ayrıca, Microsoft Graph API üzerinde uygulama izinleri hariç olmak üzere izinleri ve uygulama izinlerini _kabul_ etme olanağı verir.

> [!IMPORTANT]
> Bu özel durum, _diğer_ uygulamalar için (örneğin, Microsoft dışı uygulamalar veya Kaydolmakta olduğunuz uygulamalar) izinleri hala kabul edebilirsiniz, ancak Azure AD 'de izinler için izin vermez. Bu izinleri uygulama kaydının bir parçası olarak _isteyebilirsiniz_ , ancak bu izinleri _verme_ (yani, ' ye yarışmaya YÖNELIK) bir Azure AD yöneticisi gerektirir. Bu, kötü niyetli bir kullanıcının izinlerini kolayca yükseltebileceği anlamına gelir. Örneğin, tüm dizine yazabilme ve uygulamanın izinlerinin genel yönetici olmaya dönüşmesine yol açabilir.
>
>Bu rol, uygulama kimlik bilgilerini yönetme olanağı verir. Bu role atanan kullanıcılar, bir uygulamaya kimlik bilgileri ekleyebilir ve uygulamanın kimliğini taklit etmek için bu kimlik bilgilerini kullanabilir. Uygulamanın kimliğine bir kaynağa erişim verildiyse (Kullanıcı veya diğer nesneleri oluşturma veya güncelleştirme yeteneği gibi), bu role atanan bir Kullanıcı, uygulamayı taklit ederken bu işlemleri gerçekleştirebilir. Uygulamanın kimliğini taklit etme özelliği, kullanıcının rol atamaları aracılığıyla ne yapabilecekleri konusunda ayrıcalık yükselmesine neden olabilir. Bir kullanıcıyı uygulama yöneticisi rolüne atamanın, uygulamanın kimliğini taklit etme olanağı sağladığını anlamak önemlidir.

### <a name="application-developer"></a>[Uygulama Geliştirici](#application-developer-permissions)

Bu roldeki kullanıcılar, "kullanıcılar uygulamaları kaydedebilirler" ayarı Hayır olarak ayarlanırsa uygulama kayıtları oluşturabilir. Bu rol Ayrıca, "kullanıcılar kendi adına şirket verilerine erişim izni verebilir" ayarında Hayır olarak ayarlanırsa, tek başına kendi adına onay izni verir. Bu role atanan kullanıcılar, yeni uygulama kayıtları veya kurumsal uygulamalar oluştururken sahip olarak eklenir.

### <a name="attack-payload-author"></a>[Saldırı yükü yazarı](#attack-payload-author-permissions)

Bu roldeki kullanıcılar, saldırı yükleri oluşturabilir ancak bunları gerçekten başlatamaz veya zamanlayamaz. Böylece saldırı yükleri, kiracının benzetim oluşturmak için kullanabilecekleri tüm yöneticiler tarafından kullanılabilir.

### <a name="attack-simulation-administrator"></a>[Saldırı simülasyonu Yöneticisi](#attack-simulation-administrator-permissions)

Bu roldeki kullanıcılar, saldırı simülasyonu oluşturma, simülasyon başlatma/zamanlama ve simülasyon sonuçlarının incelenmesi gibi tüm özellikleri oluşturabilir ve yönetebilir. Bu rolün üyeleri Kiracıdaki tüm benzetimler için bu erişime sahiptir.

### <a name="authentication-administrator"></a>[Kimlik doğrulama Yöneticisi](#authentication-administrator-permissions)

Bu role sahip kullanıcılar, yönetici olmayanlar ve bazı roller için herhangi bir kimlik doğrulama yöntemi (parolalar dahil) ayarlayabilir veya sıfırlayabilir. Kimlik doğrulama yöneticileri, var olan parola olmayan kimlik bilgilerine (örneğin, MFA veya FIDO) göre yeniden kaydolmak için yönetici olmayan veya bazı rollere atanmayan kullanıcıların, bir sonraki oturum açma üzerinde MFA 'yı isteyen **CIHAZDA MFA 'yı hatırlayabilmesini** gerektirebilir. Kimlik doğrulama yöneticisinin authentcation yöntemlerini okuyaacağı veya güncelleştirebir rolün listesi için bkz. [parola sıfırlama izinleri](#password-reset-permissions).

[Ayrıcalıklı kimlik doğrulama Yöneticisi](#privileged-authentication-administrator) rolünün tüm kullanıcılar için yeniden kaydolmayı ve Multi-Factor Authentication 'ı zorlama izni vardır.

[Kimlik doğrulama İlkesi yönetici](#authentication-policy-administrator) rolü, her kullanıcının kaydedebileceği ve kullanabileceği yöntemleri belirleyen kiracının kimlik doğrulama yöntemi ilkesini ayarlama iznine sahiptir.

| Rol | Kullanıcının kimlik doğrulama yöntemlerini yönetme | Kullanıcı başına MFA yönetme | MFA ayarlarını yönetme | Kimlik doğrulama yöntemi ilkesini Yönet | Parola koruma ilkesini Yönet |
| ---- | ---- | ---- | ---- | ---- | ---- | 
| Kimlik doğrulama Yöneticisi | Bazı kullanıcılar için Evet (yukarıya bakın) | Bazı kullanıcılar için Evet (yukarıya bakın) | Hayır | Hayır | Hayır | 
| Ayrıcalıklı kimlik doğrulama Yöneticisi| Tüm kullanıcılar için Evet | Tüm kullanıcılar için Evet | Hayır | Hayır | Hayır | 
| Kimlik doğrulama İlkesi Yöneticisi | Hayır |Hayır | Yes | Yes | Yes | 

> [!IMPORTANT]
> Bu role sahip kullanıcılar, Azure Active Directory ' nin içindeki ve dışındaki gizli veya özel bilgilere veya kritik yapılandırmaya erişimi olabilecek kişiler için kimlik bilgilerini değiştirebilir. Bir kullanıcının kimlik bilgilerini değiştirmek, kullanıcının kimliğini ve izinlerini kabul etme imkanını ifade edebilir. Örneğin:
>
>* Sahip oldukları uygulamaların kimlik bilgilerini yönetebilen uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaların Azure AD 'de ayrıcalıklı izinleri olabilir ve kimlik doğrulama yöneticilerine başka bir yerde izin verilmez. Bu yol aracılığıyla bir kimlik doğrulama Yöneticisi bir uygulama sahibinin kimliğini varsayabilir ve sonra uygulamanın kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini daha fazla varsayabilir.
>* Azure abonelik sahipleri, hassas veya özel bilgilere veya Azure 'da kritik yapılandırmaya erişebilir.
>* Grup üyeliğini yönetebilen güvenlik grubu ve Grup sahipleri Microsoft 365. Bu gruplar, Azure AD 'de ve başka bir yerde hassas veya özel bilgilere veya kritik yapılandırmaya erişim verebilir.
>* Exchange Online, Office Security ve Uyumluluk Merkezi ve insan kaynakları sistemleri gibi Azure AD dışında diğer hizmetlerde bulunan yöneticiler.
>* Gizli veya özel bilgilere erişebilen Yöneticiler, yasal Counsel ve insan kaynakları çalışanları gibi yönetici olmayanlar.

> [!IMPORTANT]
> Bu rol, şu anda eski MFA yönetim portalındaki Kullanıcı başına MFA 'yı yönetme yeteneğine sahip değil. Aynı işlevler [set-MsolUser](https://docs.microsoft.com/powershell/module/msonline/set-msoluser) komutunu Azure AD PowerShell modülü kullanılarak gerçekleştirilebilir.

### <a name="authentication-policy-administrator"></a>[Kimlik doğrulama Ilkesi Yöneticisi](#authentication-policy-administrator-permissions)

Bu role sahip kullanıcılar, kimlik doğrulama yöntemleri ilkesini, kiracı genelinde MFA ayarlarını ve parola koruma ilkesini yapılandırabilir. Bu rol, parola koruma ayarlarını yönetme izni verir: akıllı kilitleme yapılandırması ve özel yasaklanmış parolalar listesi güncelleştiriliyor. 

[Kimlik doğrulama Yöneticisi](#authentication-administrator) ve [ayrıcalıklı kimlik doğrulama Yöneticisi](#privileged-authentication-administrator) rollerinin kullanıcılara kayıtlı kimlik doğrulama yöntemlerini yönetme izni vardır ve tüm kullanıcılar için yeniden kayıt ve Multi-Factor Authentication 'ı zorunlu hale getirebilirsiniz. 

| Rol | Kullanıcının kimlik doğrulama yöntemlerini yönetme | Kullanıcı başına MFA yönetme | MFA ayarlarını yönetme | Kimlik doğrulama yöntemi ilkesini Yönet | Parola koruma ilkesini Yönet |
| ---- | ---- | ---- | ---- | ---- | ---- | 
| Kimlik doğrulama Yöneticisi | Bazı kullanıcılar için Evet (yukarıya bakın) | Bazı kullanıcılar için Evet (yukarıya bakın) | Hayır | Hayır | Hayır | 
| Ayrıcalıklı kimlik doğrulama Yöneticisi| Tüm kullanıcılar için Evet | Tüm kullanıcılar için Evet | Hayır | Hayır | Hayır | 
| Kimlik doğrulama İlkesi Yöneticisi | Hayır | Hayır | Yes | Yes | Yes | 

> [!IMPORTANT]
> Bu rol, şu anda eski MFA yönetim portalındaki MFA ayarlarını yönetme yeteneğine sahip değil.

### <a name="azure-ad-joined-device-local-administrator"></a>[Azure AD 'ye katılmış cihaz yerel Yöneticisi](#azure-ad-joined-device-local-administrator-permissions)

Bu rol, yalnızca [cihaz ayarları](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/)'nda ek bir yerel yönetici olarak atanmak üzere kullanılabilir. Bu role sahip kullanıcılar, Azure Active Directory katılmış tüm Windows 10 cihazlarında yerel makine yöneticileri haline gelir. Azure Active Directory cihaz nesnelerini yönetme yeteneğine sahip değildir.

### <a name="azure-devops-administrator"></a>[Azure DevOps Yöneticisi](#azure-devops-administrator-permissions)

Bu role sahip olan kullanıcılar, yeni Azure DevOps kuruluş oluşturmayı yapılandırılabilir bir kullanıcı veya grup kümesine kısıtlamak için Azure DevOps ilkesini yönetebilir. Bu roldeki kullanıcılar, bu ilkeyi şirketin Azure AD kuruluşu tarafından desteklenen herhangi bir Azure DevOps organizasyonu aracılığıyla yönetebilir. Bu rol, şirketin Azure AD organizasyonu tarafından desteklenen herhangi bir Azure DevOps kuruluşunun içindeki başka bir Azure DevOps 'a özgü izin (örneğin, proje koleksiyonu yöneticileri) vermez.

Tüm kurumsal Azure DevOps ilkeleri, bu roldeki kullanıcılar tarafından yönetilebilir.

### <a name="azure-information-protection-administrator"></a>[Azure Information Protection Yöneticisi](#azure-information-protection-administrator-permissions)

Bu role sahip olan kullanıcılar Azure Information Protection hizmetinde tüm izinlere sahiptir. Bu rol, Azure Information Protection ilkesi için etiketlerin yapılandırılmasını, koruma şablonlarının yönetilmesini ve korumanın etkinleşmesini sağlar. Bu rol, kimlik koruma Merkezi, Privileged Identity Management, Izleme Microsoft 365 hizmet durumu veya Office 365 güvenlik & Uyumluluk Merkezi 'nde herhangi bir izin vermez.

### <a name="b2c-ief-keyset-administrator"></a>[B2C ıEF anahtar kümesi Yöneticisi](#b2c-ief-keyset-administrator-permissions)

Kullanıcı belirteç şifreleme, belirteç imzaları ve talep şifreleme/şifre çözme için ilke anahtarlarını ve gizli dizileri oluşturabilir ve yönetebilir. Mevcut anahtar kapsayıcılarına yeni anahtarlar ekleyerek, bu sınırlı yönetici, var olan uygulamaları etkilemeden gizli dizileri gereken şekilde alabilir. Bu Kullanıcı, oluşturulduktan sonra bile bu parolaların tam içeriğini ve bunların sona erme tarihlerini görebilir.

> [!IMPORTANT]
> Bu hassas bir roldür. Anahtar kümesi yönetici rolü, üretim öncesi ve üretim sırasında dikkatli bir şekilde denetlenmeli ve bunlarla atanmalıdır.

### <a name="b2c-ief-policy-administrator"></a>[B2C ıEF Ilke Yöneticisi](#b2c-ief-policy-administrator-permissions)

Bu roldeki kullanıcılar, Azure AD B2C tüm özel ilkeleri oluşturma, okuma, güncelleştirme ve silme yeteneğine sahiptir ve bu nedenle ilgili Azure AD B2C kuruluştaki kimlik deneyimi çerçevesinde tam denetime sahiptir. İlkeleri düzenleyerek, bu kullanıcı dış kimlik sağlayıcılarıyla doğrudan Federasyon oluşturabilir, Dizin şemasını değiştirebilir, kullanıcıya yönelik tüm içeriği (HTML, CSS, JavaScript) değiştirebilir, bir kimlik doğrulamasını tamamlamaya yönelik gereksinimleri değiştirebilir, yeni kullanıcılar oluşturabilir, tam geçişler dahil olmak üzere dış sistemlere Kullanıcı verileri gönderebilir ve parolalar ve telefon numaraları gibi hassas alanlar da dahil olmak üzere tüm Kullanıcı bilgilerini düzenleyebilir. Buna karşılık, bu rol, şifreleme anahtarlarını değiştiremez veya kuruluşta Federasyon için kullanılan gizli dizileri düzenleyemez.

> [!IMPORTANT]
> B2 ıEF Ilke Yöneticisi, üretimde kuruluşlar için çok sınırlı bir temelde atanması gereken son derece duyarlı bir roldür. Bu kullanıcıların etkinlikleri, özellikle üretimde kuruluşlar için yakından denetlenmelidir.

### <a name="billing-administrator"></a>[Faturalama yöneticisi](#billing-administrator-permissions)

Satın alma işlemleri yapar, abonelikleri yönetir, destek biletlerini yönetir ve hizmetin sistem durumunu izler.

### <a name="cloud-application-administrator"></a>[Bulut Uygulaması Yöneticisi](#cloud-application-administrator-permissions)

Bu roldeki kullanıcılar uygulama proxy 'si rolüyle aynı izinlere sahiptir ve uygulama ara sunucusunu yönetme imkanını dışlar. Bu rol, kurumsal uygulamaların ve uygulama kayıtlarının tüm yönlerini oluşturma ve yönetme olanağı verir. Bu rol Ayrıca, Microsoft Graph ve Azure AD grafiğini dışlayarak uygulama izinleri için izin verme izni verir. Bu role atanan kullanıcılar, yeni uygulama kayıtları veya kurumsal uygulamalar oluştururken sahip olarak eklenmez.

> [!IMPORTANT]
> Bu rol, uygulama kimlik bilgilerini yönetme olanağı verir. Bu role atanan kullanıcılar, bir uygulamaya kimlik bilgileri ekleyebilir ve uygulamanın kimliğini taklit etmek için bu kimlik bilgilerini kullanabilir. Uygulamanın kimliğine bir kaynağa erişim verildiyse (Kullanıcı veya diğer nesneleri oluşturma veya güncelleştirme yeteneği gibi), bu role atanan bir Kullanıcı, uygulamayı taklit ederken bu işlemleri gerçekleştirebilir. Uygulamanın kimliğini taklit etme özelliği, kullanıcının rol atamaları aracılığıyla ne yapabilecekleri konusunda ayrıcalık yükselmesine neden olabilir. Bir kullanıcıyı bulut uygulaması Yöneticisi rolüne atamanın, bir uygulamanın kimliğini taklit etme olanağı sağladığını anlamak önemlidir.


### <a name="cloud-device-administrator"></a>[Bulut Cihaz Yöneticisi](#cloud-device-administrator-permissions)

Bu roldeki kullanıcılar, Azure AD 'de cihazları etkinleştirebilir, devre dışı bırakabilir ve silebilir ve Azure portal Windows 10 BitLocker anahtarlarını (varsa) okuyabilir. Rol, cihazdaki diğer özellikleri yönetmek için izinler vermez.

### <a name="compliance-administrator"></a>[Uyumluluk Yöneticisi](#compliance-administrator-permissions)

Bu role sahip olan kullanıcılar Microsoft 365 Uyumluluk Merkezi 'nde uyumlulukla ilgili özellikleri yönetme izinlerine sahiptir Microsoft 365 Yönetim Merkezi, Azure ve Office 365 güvenlik & Uyumluluk Merkezi. Atanan, Exchange Yönetim Merkezi ve takımlar & Skype Kurumsal Yönetici merkezlerinin içindeki tüm özellikleri yönetebilir ve Azure ve Microsoft 365 için destek biletleri oluşturabilir. [Microsoft 365 Yönetici rolleri hakkında](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)daha fazla bilgi bulabilirsiniz.

İçinde | Yapılabilir
----- | ----------
[Uyumluluk Merkezi Microsoft 365](https://protection.office.com) | Kuruluşunuzun verilerini Microsoft 365 hizmetleri arasında koruyun ve yönetin<br>Uyumluluk uyarılarını yönetme
[Uyumluluk Yöneticisi](/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Kuruluşunuzun yasal uyumluluk etkinliklerini izleyin, atayın ve doğrulayın
[Office 365 güvenlik & Uyumluluk Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Veri idare yönetimi<br>Yasal ve veri araştırmasını gerçekleştirme<br>Veri konu Isteğini yönetme<br><br>Bu rol, Office 365 güvenlik & Uyumluluk Merkezi rol tabanlı erişim denetimindeki [Uyumluluk Yöneticisi RoleGroup](/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center#permissions-needed-to-use-features-in-the-security--compliance-center) ile aynı izinlere sahiptir.
[Intune](/intune/role-based-access-control) | Tüm Intune denetim verilerini görüntüleyin
[Cloud App Security](/cloud-app-security/manage-admins) | Salt okuma izinlerine sahiptir ve uyarıları yönetebilir<br>Dosya ilkeleri oluşturabilir ve değiştirebilir ve dosya idare eylemlerine izin verebilir<br>Tüm yerleşik raporları Veri Yönetimi altına görüntüleyebilir

### <a name="compliance-data-administrator"></a>[Uyumluluk verileri Yöneticisi](#compliance-data-administrator-permissions)

Bu role sahip olan kullanıcılar, Microsoft 365 Uyumluluk Merkezi, Microsoft 365 Yönetim Merkezi ve Azure 'da verileri izleme iznine sahiptir. Kullanıcılar ayrıca Exchange Yönetim Merkezi, uyumluluk Yöneticisi ve takımlar & Skype Kurumsal Yönetim Merkezi 'nde uyumluluk verilerini izleyebilir ve Azure ve Microsoft 365 için destek biletleri oluşturabilir. [Bu belgede](/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center#permissions-needed-to-use-features-in-the-security--compliance-center) , uyumluluk Yöneticisi ve uyumluluk veri Yöneticisi arasındaki farklara ilişkin ayrıntılar bulunur.

İçinde | Yapılabilir
----- | ----------
[Uyumluluk Merkezi Microsoft 365](https://protection.office.com) | Microsoft 365 hizmetleri genelinde uyumlulukla ilgili ilkeleri izleme<br>Uyumluluk uyarılarını yönetme
[Uyumluluk Yöneticisi](/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Kuruluşunuzun yasal uyumluluk etkinliklerini izleyin, atayın ve doğrulayın
[Office 365 güvenlik & Uyumluluk Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Veri idare yönetimi<br>Yasal ve veri araştırmasını gerçekleştirme<br>Veri konu Isteğini yönetme<br><br>Bu rol, Office 365 güvenlik & Uyumluluk Merkezi rol tabanlı erişim denetimindeki [Uyumluluk verileri Yöneticisi RoleGroup](/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center#permissions-needed-to-use-features-in-the-security--compliance-center) ile aynı izinlere sahiptir.
[Intune](/intune/role-based-access-control) | Tüm Intune denetim verilerini görüntüleyin
[Cloud App Security](/cloud-app-security/manage-admins) | Salt okuma izinlerine sahiptir ve uyarıları yönetebilir<br>Dosya ilkeleri oluşturabilir ve değiştirebilir ve dosya idare eylemlerine izin verebilir<br>Tüm yerleşik raporları Veri Yönetimi altına görüntüleyebilir

### <a name="conditional-access-administrator"></a>[Koşullu Erişim Yöneticisi](#conditional-access-administrator-permissions)

Bu role sahip olan kullanıcılar Azure Active Directory Koşullu erişim ayarlarını yönetebilir.

### <a name="customer-lockbox-access-approver"></a>[Müşteri Kasası erişimi onaylayan](#customer-lockbox-access-approver-permissions)

Kuruluşunuzdaki [müşteri kasası isteklerini](/office365/admin/manage/customer-lockbox-requests) yönetir. Müşteri Kasası istekler için e-posta bildirimleri alırlar ve Microsoft 365 Yönetim merkezinden gelen istekleri onaylayabilir ve reddedebilirler. Ayrıca Müşteri Kasası özelliğini açabilir veya kapatabilir. Yalnızca genel Yöneticiler, bu role atanan kişilerin parolalarını sıfırlayabilir.

### <a name="desktop-analytics-administrator"></a>[Masaüstü Analizi Yöneticisi](#desktop-analytics-administrator-permissions)

Bu roldeki kullanıcılar masaüstü Analizi ve Office özelleştirmesi & Ilkesi hizmetlerini yönetebilir. Bu, masaüstü analizi için varlık envanterini görüntüleme, dağıtım planları oluşturma, dağıtım ve sistem durumunu görüntüleme imkanını içerir. Office özelleştirmesi & Ilke hizmetinde, bu rol kullanıcıların Office ilkelerini yönetmesine olanak sağlar.

### <a name="directory-readers"></a>[Dizin okuyucuları](#directory-readers-permissions)

Bu roldeki kullanıcılar, temel dizin bilgilerini okuyabilir. Bu rol için kullanılmalıdır:

* Belirli bir konuk kullanıcıları kümesine, tüm konuk kullanıcılara vermek yerine okuma erişimi verme.
* "Azure AD portalına erişimi yalnızca yöneticilerle kısıtla" olarak ayarlarsanız, yönetici olmayan kullanıcıların belirli bir kümesini Azure portal erişimine izin vermek "Evet" olarak ayarlanır.
* Dizin. Read. All 'un bir seçenek olmadığı dizine hizmet sorumlusu erişimi veriliyor.

### <a name="directory-synchronization-accounts"></a>[Dizin eşitleme hesapları](#directory-synchronization-accounts-permissions)

Kullanmayın. Bu rol, Azure AD Connect hizmetine otomatik olarak atanır ve başka bir kullanım için tasarlanmamıştır veya desteklenmez.

### <a name="directory-writers"></a>[Dizin yazarları](#directory-writers-permissions)
Bu roldeki kullanıcılar, kullanıcıların, grupların ve hizmet sorumlularının temel bilgilerini okuyabilir ve güncelleştirebilir. Bu rolü yalnızca [onay çerçevesini](../develop/quickstart-register-app.md)desteklemeyen uygulamalara atayın. Herhangi bir kullanıcıya atanmamalıdır.

### <a name="domain-name-administrator"></a>[Etki alanı adı yöneticisi](#domain-name-administrator-permissions)

Bu role sahip kullanıcılar, etki alanı adlarını yönetebilir (okuyabilir, ekleyebilir, doğrulayamaz, güncelleştirebilir ve silebilir). Kullanıcılar, gruplar ve uygulamalarla ilgili dizin bilgilerini de okuyabilir, bu nesneler etki alanı bağımlılıklarına sahip olabilir. Şirket içi ortamlarda, bu role sahip olan kullanıcılar, ilgili kullanıcıların her zaman şirket içinde kimlik doğrulamasından geçirilmeleri için, Federasyon etki alanı adlarını yapılandırabilir. Bu kullanıcılar daha sonra Azure AD tabanlı hizmetlerde çoklu oturum açma yoluyla şirket içi parolalarla oturum açabilirler. Federasyon ayarlarının Azure AD Connect aracılığıyla eşitlenmesi gerekir, böylece kullanıcıların Azure AD Connect yönetme izinleri de vardır.

### <a name="dynamics-365-administrator"></a>[Dynamics 365 Yöneticisi](#dynamics-365-administrator-permissions)

Bu role sahip olan kullanıcılar, hizmet mevcut olduğunda Microsoft Dynamics 365 Online içinde genel izinlere sahiptir ve destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. Daha fazla bilgi [için, Azure AD kuruluşunuzu yönetmek üzere hizmet yöneticisi rolünü kullanın](/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell 'de, bu rol "Dynamics 365 hizmet yöneticisi" olarak tanımlanır. [Azure Portal](https://portal.azure.com), "Dynamics 365 Yöneticisi" dir.

### <a name="exchange-administrator"></a>[Exchange Yöneticisi](#exchange-administrator-permissions)

Bu role sahip olan kullanıcılar, hizmet mevcut olduğunda Microsoft Exchange Online içinde genel izinlere sahiptir. Ayrıca tüm Microsoft 365 gruplarını oluşturup yönetebilir, destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. [Yönetici rolleri hakkında](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)daha fazla bilgi Microsoft 365.

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell 'de, bu rol "Exchange hizmeti Yöneticisi" olarak tanımlanır. [Azure Portal](https://portal.azure.com), "Exchange Yöneticisi" dir. [Exchange Yönetim Merkezi](https://go.microsoft.com/fwlink/p/?LinkID=529144)'Nde "Exchange Online Yöneticisi" dir.


### <a name="external-id-user-flow-administrator"></a>[Dış KIMLIK Kullanıcı akış Yöneticisi](#external-id-user-flow-administrator-permissions)

Bu role sahip kullanıcılar, Azure portal Kullanıcı akışları oluşturabilir ve yönetebilir ("yerleşik" ilkeleri olarak da bilinir). Bu kullanıcılar HTML/CSS/JavaScript içeriğini özelleştirebilir, MFA gereksinimlerini değiştirebilir, belirteçteki talepleri seçebilir, API bağlayıcılarını yönetebilir ve Azure AD kuruluşunda tüm Kullanıcı akışları için oturum ayarlarını yapılandırabilir. Diğer taraftan, bu rol, Kullanıcı verilerini gözden geçirme veya kuruluş şemasına dahil olan özniteliklerde değişiklik yapma özelliğini içermez. Kimlik deneyimi çerçevesi ilkelerine (özel ilkeler olarak da bilinir) yapılan değişiklikler Ayrıca bu rolün kapsamı dışındadır.

### <a name="external-id-user-flow-attribute-administrator"></a>[Dış KIMLIK Kullanıcı akışı öznitelik Yöneticisi](#external-id-user-flow-attribute-administrator-permissions)

Bu role sahip kullanıcılar, Azure AD kuruluşundaki tüm Kullanıcı akışları için kullanılabilir özel öznitelikler ekler veya siler. Bu nedenle, bu role sahip olan kullanıcılar Son Kullanıcı şemasına değiştirebilir veya yeni öğe ekleyebilir, tüm Kullanıcı akışlarının davranışını etkileyebilir ve son kullanıcılar ve son olarak uygulamalara talepler olarak gönderilebilir. Bu rol Kullanıcı akışlarını düzenleyemez.

### <a name="external-identity-provider-administrator"></a>[Dış kimlik sağlayıcısı Yöneticisi](#external-identity-provider-administrator-permissions)

Bu yönetici Azure AD kuruluşları ve dış kimlik sağlayıcıları arasında Federasyonu yönetir. Bu rolle, kullanıcılar yeni kimlik sağlayıcıları ekleyebilir ve tüm kullanılabilir ayarları (örneğin, kimlik doğrulama yolu, hizmet KIMLIĞI, atanan anahtar kapsayıcıları) yapılandırabilir. Bu Kullanıcı Azure AD kuruluşunun, dış kimlik sağlayıcılarından kimlik doğrulamaları güvenmesini sağlayabilir. Son Kullanıcı deneyimlerinde ortaya çıkan etki, kuruluşun türüne bağlıdır:

* Çalışanlar ve iş ortakları için Azure AD kuruluşları: bir Federasyonun eklenmesi (örn. Gmail ile), henüz kullanılmadı tüm konuk davetlerini hemen etkiler. Bkz. [B2B Konuk kullanıcıları için kimlik sağlayıcısı olarak Google ekleme](../external-identities/google-federation.md).
* Azure Active Directory B2C kuruluşlar: bir Federasyonun eklenmesi (örneğin, Facebook ile veya başka bir Azure AD kuruluşu ile), kimlik sağlayıcısı Kullanıcı akışına bir seçenek olarak ekleninceye kadar Son Kullanıcı akışlarını hemen etkilemez (yerleşik ilke olarak da bilinir). Bir örnek için [kimlik sağlayıcısı olarak Microsoft hesabı yapılandırma](../../active-directory-b2c/identity-provider-microsoft-account.md) konusuna bakın. Kullanıcı akışlarını değiştirmek için, "B2C Kullanıcı akış Yöneticisi" nin sınırlı rolü gereklidir.

### <a name="global-administrator"></a>[Genel Yönetici](#global-administrator-permissions)

Bu role sahip olan kullanıcılar, Azure Active Directory ' deki tüm yönetim özelliklerine ve Microsoft 365 Güvenlik Merkezi, Microsoft 365 Uyumluluk Merkezi, Exchange Online, SharePoint Online ve Skype Kurumsal Çevrimiçi kullanım gibi Azure Active Directory kimliklerini kullanan hizmetlere erişebilir. Ayrıca, genel Yöneticiler tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek üzere [erişimini yükseltebilir](../../role-based-access-control/elevate-access-global-admin.md) . Bu, genel yöneticilerin ilgili Azure AD kiracısı kullanarak tüm Azure kaynaklarına tam erişim sağlamasına izin verir. Azure AD organizasyonu için kaydolan kişi genel yönetici haline gelir. Şirketinizde birden fazla genel yönetici olabilir. Genel Yöneticiler, tüm kullanıcılar ve diğer tüm yöneticiler için parolayı sıfırlayabilir.

### <a name="global-reader"></a>[Genel okuyucu](#global-reader-permissions)

Bu roldeki kullanıcılar Microsoft 365 hizmetleri genelinde ayarları ve yönetim bilgilerini okuyabilir, ancak yönetim eylemleri alamaz. Genel okuyucu genel yöneticiye salt okunurdur. Planlama, denetim veya araştırmalar için genel yönetici yerine genel okuyucu atayın. Küresel bir yönetici rolü atanmadan çalışmayı kolaylaştırmak için, Exchange Yöneticisi gibi diğer sınırlı yönetici rolleriyle birlikte genel okuyucu kullanın. Küresel okuyucu Microsoft 365 Yönetim Merkezi, Exchange Yönetim Merkezi, SharePoint Yönetim Merkezi, takımlar yönetici Merkezi, Güvenlik Merkezi, Uyumluluk Merkezi, Azure AD Yönetim Merkezi ve cihaz yönetimi Yönetim Merkezi ile birlikte kullanılabilir.

> [!NOTE]
> Genel okuyucu rolünde Şu anda birkaç kısıtlama vardır-
>
>- [OneDrive Yönetim Merkezi](https://admin.onedrive.com/) -OneDrive Yönetim Merkezi, genel okuyucu rolünü desteklemez
>- [M365 Yönetim Merkezi](https://admin.microsoft.com/Adminportal/Home#/homepage) -genel okuyucu, müşteri kasası isteklerini okuyamıyor. M365 Yönetim Merkezi 'nin sol bölmesinde, **destek** altında **müşteri kasası istekleri** sekmesini bulmayacağız.
>- [Office güvenlik & Uyumluluk Merkezi](https://sip.protection.office.com/homepage) -küresel okuyucu SCC denetim günlüklerini okuyamıyor, içerik araması yapamıyor veya güvenli puanı göremez.
>- [Takımlar Yönetim Merkezi](https://admin.teams.microsoft.com) -küresel okuyucu **takımlar yaşam döngüsünü**, **analiz & raporlarını**, **IP telefon cihaz yönetimini** ve **uygulama kataloğunu** okuyamıyor.
>- [Privileged Access Management (Pam)](/office365/securitycompliance/privileged-access-management-overview) , genel okuyucu rolünü desteklemez.
>- [Azure Information Protection](/azure/information-protection/what-is-information-protection) -genel okuyucu yalnızca [merkezi raporlama Için](/azure/information-protection/reports-aip) desteklenir ve Azure AD Kuruluşunuz [Birleşik etiketleme platformunda](/azure/information-protection/faqs#how-can-i-determine-if-my-tenant-is-on-the-unified-labeling-platform)ne zaman değildir.
>
> Bu özellikler Şu anda geliştirme aşamasındadır.
>

### <a name="groups-administrator"></a>[Grup Yöneticisi](#groups-administrator-permissions)

Bu roldeki kullanıcılar, adlandırma ve süre sonu ilkeleri gibi grupları ve ayarlarını oluşturabilir/yönetebilir. Bu role bir Kullanıcı atamanın, bu kullanıcılara kuruluştaki tüm grupları takımlar, SharePoint, Yammer gibi çeşitli iş yükleri arasında Outlook 'a ek olarak yönetme olanağı sağladığını anlamak önemlidir. Ayrıca, Kullanıcı, çeşitli grup ayarlarını Microsoft Yönetim Merkezi, Azure portal gibi çeşitli yönetim portallarında ve takımlar ve SharePoint yönetim merkezleri gibi iş yüklerinin yanı sıra iş yüküne göre yönetebilecektir.

### <a name="guest-inviter"></a>[Konuk davetci](#guest-inviter-permissions)

**Üyeler** Kullanıcı ayarı Hayır olarak ayarlandığında, bu roldeki KULLANıCıLAR Azure Active Directory B2B Konuk kullanıcı davetlerini yönetebilir. [Azure AD B2B Işbirliği hakkında](../external-identities/what-is-b2b.md)B2B işbirliği hakkında daha fazla bilgi. Başka herhangi bir izin içermez.

### <a name="helpdesk-administrator"></a>[Yardım Masası Yöneticisi](#helpdesk-administrator-permissions)

Bu role sahip kullanıcılar parolaları değiştirebilir, yenileme belirteçlerini geçersiz kılabilir, hizmet isteklerini yönetebilir ve hizmet durumunu izleyebilir. Yenileme belirtecinin geçersiz kılınması, kullanıcının yeniden oturum açmasını zorlar. Bir yardım masası yöneticisinin bir kullanıcının parolasını sıfırlayıp sıfırlayamayacağı ve yenileme belirteçlerini geçersiz kılabileceği, kullanıcının atandığı role göre değişir. Bir yardım masası yöneticisinin parolalarını sıfırlayıp yenileme belirteçlerini geçersiz kılabileceği rollerin listesi için bkz. [parola sıfırlama izinleri](#password-reset-permissions).

> [!IMPORTANT]
> Bu role sahip kullanıcılar, Azure Active Directory ' nin içindeki ve dışındaki gizli veya özel bilgilere veya kritik yapılandırmaya erişimi olabilecek kişilerin parolalarını değiştirebilir. Bir kullanıcının parolasını değiştirmek, kullanıcının kimliğini ve izinlerini kabul etme imkanını ifade edebilir. Örneğin:
>
>- Sahip oldukları uygulamaların kimlik bilgilerini yönetebilen uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaların Azure AD 'de ayrıcalıklı izinleri olabilir ve yardım masası yöneticilerine başka bir yerde izin verilmez. Bu yol aracılığıyla bir yardım masası Yöneticisi, bir uygulama sahibinin kimliğini varsayabilir ve daha sonra uygulamanın kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini daha fazla varsayabilir.
>- Azure abonelik sahipleri, hassas veya özel bilgilere veya Azure 'da kritik yapılandırmaya erişebilir.
>- Grup üyeliğini yönetebilen güvenlik grubu ve Grup sahipleri Microsoft 365. Bu gruplar, Azure AD 'de ve başka bir yerde hassas veya özel bilgilere veya kritik yapılandırmaya erişim verebilir.
>- Exchange Online, Office Security ve Uyumluluk Merkezi ve insan kaynakları sistemleri gibi Azure AD dışında diğer hizmetlerde bulunan yöneticiler.
>- Gizli veya özel bilgilere erişebilen Yöneticiler, yasal Counsel ve insan kaynakları çalışanları gibi yönetici olmayanlar.

Kullanıcıların alt kümeleri üzerinde yönetim izinlerinin yetkisini verme ve ilkeleri bir kullanıcı alt kümesine uygulama, [yönetim birimleriyle](administrative-units.md)mümkündür.

Bu rol daha önce [Azure Portal](https://portal.azure.com/)"parola Yöneticisi" olarak adlandırılmıştı. Azure AD 'deki "Yardım Masası Yöneticisi" adı artık Azure AD PowerShell 'deki adıyla ve Microsoft Graph API 'siyle eşleşiyor.

### <a name="hybrid-identity-administrator"></a>[Karma kimlik yöneticisi](#hybrid-identity-administrator-permissions)

Bu roldeki kullanıcılar, bulut sağlama ve Federasyon ayarlarını yönetme işlemlerini kullanarak AD 'den Azure AD 'ye sağlama yapılandırma kurulumu oluşturabilir, yönetebilir ve dağıtabilir. Kullanıcılar ayrıca bu rolü kullanarak günlükleri giderebilir ve izleyebilir.

### <a name="insights-administrator"></a>[Öngörüler Yöneticisi](#insights-administrator-permissions)
Bu roldeki kullanıcılar, [M365 Insights uygulamasındaki](https://go.microsoft.com/fwlink/?linkid=2129521)tüm yönetim özelliklerine erişebilir. Bu rol, dizin bilgilerini okuyabilir, hizmet durumunu izleyebilir, dosya destek biletlerini izleyebilir ve Öngörüler yönetici ayarları yönlerini erişebilir.

### <a name="insights-business-leader"></a>[Öngörüler Iş lideri](#insights-business-leader-permissions)
Bu roldeki kullanıcılar, [M365 Insights uygulaması](https://go.microsoft.com/fwlink/?linkid=2129521)aracılığıyla bir panolar ve Öngörüler kümesine erişebilir. Bu, tüm panolara ve sunulan Öngörüler ve veri araştırma işlevlerine tam erişim içerir. Bu roldeki kullanıcılar, Öngörüler yönetici rolünün sorumluluğu olan ürün yapılandırma ayarlarına erişemez.

### <a name="intune-administrator"></a>[Intune Yöneticisi](#intune-administrator-permissions)

Bu role sahip olan kullanıcılar, hizmet mevcut olduğunda Microsoft Intune çevrimiçi olarak genel izinlere sahiptir. Ayrıca, bu rol, ilkeyi ilişkilendirmek ve grupları oluşturmak ve yönetmek için kullanıcıları ve cihazları yönetme özelliğini içerir. [Microsoft Intune Ile rol tabanlı yönetim denetimi (RBAC)](/intune/role-based-access-control)hakkında daha fazla bilgi.

Bu rol, tüm güvenlik gruplarını oluşturabilir ve yönetebilir. Ancak, Intune yöneticisinin Office grupları üzerinde yönetici hakları yoktur. Bu, yöneticinin kuruluştaki tüm Office gruplarının sahiplerini veya üyeliklerini güncelleştiremediği anlamına gelir. Ancak, oluşturduğu Office grubunu, son kullanıcı ayrıcalıklarının bir parçası olarak gelen oluşturduğu Office grubunu yönetebilir. Bu nedenle, oluşturduğu herhangi bir Office grubu (güvenlik grubu değil), 250 kotasına göre sayılır.

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell 'de, bu rol "Intune Hizmet Yöneticisi" olarak tanımlanır. Bu, [Azure Portal](https://portal.azure.com)"Intune Yöneticisi" dir.

### <a name="kaizala-administrator"></a>[Kaizala Yöneticisi](#kaizala-administrator-permissions)

Bu role sahip olan kullanıcılar, hizmet mevcut olduğunda Microsoft Kaizala içindeki ayarları yönetmek için genel izinlere sahiptir ve destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. Ayrıca, Kullanıcı, kaizala tarafından Kaizala tarafından kullanılan ve Kaizala eylemleri kullanılarak oluşturulan iş raporlarının kullanımına & benimseme ile ilgili raporlara erişebilir.

### <a name="license-administrator"></a>[Lisans Yöneticisi](#license-administrator-permissions)

Bu roldeki kullanıcılar, kullanıcılar, gruplar (grup tabanlı lisanslama kullanarak) için lisans atamaları ekleyebilir, kaldırabilir ve güncelleştirebilir ve kullanıcılara kullanım konumunu yönetebilir. Rol, abonelik satın alma veya yönetme, Grup oluşturma veya yönetme veya kullanım konumunun ötesinde Kullanıcı oluşturma veya yönetme olanağı vermez. Bu rolün destek biletlerini görüntüleme, oluşturma veya yönetme erişimi yok.

### <a name="message-center-privacy-reader"></a>[İleti Merkezi Gizlilik okuyucusu](#message-center-privacy-reader-permissions)

Bu roldeki kullanıcılar, veri gizlilik iletileri dahil olmak üzere Ileti merkezindeki tüm bildirimleri izleyebilir. İleti Merkezi Gizlilik okuyucuları, veri gizliliğiyle ilgili olanlar da dahil olmak üzere e-posta bildirimleri alır ve Ileti merkezi tercihlerini kullanarak aboneliği kaldırabilirsiniz Veri gizliliği iletilerini yalnızca genel yönetici ve Ileti Merkezi Gizlilik okuyucusu okuyabilir. Ayrıca, bu rol grupları, etki alanlarını ve abonelikleri görüntüleme özelliğini içerir. Bu rolün hizmet isteklerini görüntüleme, oluşturma veya yönetme izni yok.

### <a name="message-center-reader"></a>[İleti Merkezi okuyucusu](#message-center-reader-permissions)

Bu roldeki kullanıcılar, Exchange, Intune ve Microsoft ekipleri gibi yapılandırılmış hizmetlerde kuruluş için [ileti merkezindeki](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) bildirimleri ve danışmanlık sistem durumu güncelleştirmelerini izleyebilir. İleti Merkezi okuyucuları, e-posta, güncelleştirme ve Microsoft 365 İleti Merkezi gönderilerini paylaşabilir. Azure AD 'de, bu role atanan kullanıcılar yalnızca kullanıcılar ve gruplar gibi Azure AD hizmetlerinde salt okuma erişimine sahip olur. Bu rolün destek biletlerini görüntüleme, oluşturma veya yönetme erişimi yok.

### <a name="modern-commerce-user"></a>[Modern ticari Kullanıcı](#modern-commerce-user-permissions)

Kullanmayın. Bu rol, otomatik olarak ticari olarak atanır ve başka bir kullanım için tasarlanmamıştır veya desteklenmez. Ayrıntıları aşağıda bulabilirsiniz.

Modern ticaret Kullanıcı rolü, belirli kullanıcılara Microsoft 365 yönetim merkezine erişme izni verir ve **giriş**, **faturalandırma** ve **destek** için sol gezinti girişlerini görebilir. Bu alanlarda bulunan içerik, kullanıcılara kendi veya kuruluşunuz için satın aldıkları ürünleri yönetmek üzere atanan, [ticari özel roller](../../cost-management-billing/manage/understand-mca-roles.md) tarafından denetlenir. Bu, faturalandırma veya faturalama hesaplarına ve faturalandırma profillerine erişim için ödeme gibi görevleri içerebilir. 

Modern ticaret Kullanıcı rolüne sahip kullanıcılar genellikle diğer Microsoft satın alma sistemlerinde yönetici izinlerine sahiptir ancak yönetim merkezine erişmek için genel yönetici veya faturalama yöneticisi rollerine sahip değildir. 

**Modern ticaret Kullanıcı rolü ne zaman atanır?**

* **Microsoft 365 Yönetim Merkezi 'Nde self servis satın alma** – self servis satın alma, kullanıcılara kendi kendilerine satın alarak veya kendi kendilerine kaydolup yeni ürünleri denemenize olanak tanır. Bu ürünler Yönetim merkezinde yönetilir. Self servis satın alma yapan kullanıcılara, Commerce sisteminde bir rol atanır ve bunları Yönetim Merkezi 'nde satın almaları için modern ticaret Kullanıcı rolü atanır. Yöneticiler, [PowerShell](/microsoft-365/commerce/subscriptions/allowselfservicepurchase-powershell)aracılığıyla self servis satın alımları (Power BI, Power Apps, Power otomatikleştirmek) engelleyebilir. Daha fazla bilgi için bkz. [Self servis satın alma hakkında SSS](/microsoft-365/commerce/subscriptions/self-service-purchase-faq).
* **Microsoft Commercial Market 'Ten satın** alma – self servis satın almaya benzer şekilde, bir Kullanıcı Microsoft AppSource veya Azure Marketi 'nden bir ürün veya hizmet satın aldığında, modern ticaret Kullanıcı rolü, genel yönetici veya faturalandırma Yöneticisi rolüne sahip olmadıkları takdirde atanır. Bazı durumlarda, kullanıcıların bu satınalmaları yapmasını engellemiş olabilir. Daha fazla bilgi için bkz. [Microsoft ticari Market](../../marketplace/marketplace-faq-publisher-guide.md#what-could-block-a-customer-from-completing-a-purchase).
* Microsoft **'un teklifleri** : teklif, Microsoft 'un Microsoft ürün ve hizmetlerini satın alması için Microsoft 'un sunduğu resmi bir tekliftir. Teklifi kabul eden kişinin Azure AD 'de Genel yönetici veya faturalama yöneticisi rolü yoksa, teklif ve yönetim merkezine erişmek için modern ticaret Kullanıcı rolünü tamamlamaya yönelik olarak hem ticari özel bir rol atanır. Yönetim merkezine erişirken yalnızca, ticari olarak verilen rolleri karşılayan özellikleri kullanabilir.
* **Ticarete özgü roller** – bazı kullanıcılara ticari özel roller atanır. Bir Kullanıcı genel veya faturalama yöneticisi değilse, yönetim merkezine erişebilmeleri için modern ticaret Kullanıcı rolü alırlar.

Modern ticaret Kullanıcı rolü bir kullanıcıdan atanmadıklarında, Microsoft 365 yönetim merkezine erişimi kaybeder. Kendileri veya kuruluşunuz için herhangi bir ürünü yönetiyorsanız, bunları yönetemezler. Bu, lisansları atama, ödeme yöntemlerinin değiştirilmesi, faturalandırların faturalandırılır veya abonelikleri yönetmek için başka görevler içerebilir.

### <a name="network-administrator"></a>[Ağ Yöneticisi](#network-administrator-permissions)

Bu roldeki kullanıcılar, Kullanıcı konumlarından ağ telemetrisini temel alan Microsoft 'un ağ çevre mimarisi önerilerini gözden geçirebilir. Microsoft 365 için ağ performansı, genellikle kullanıcı konumlarına özgü, dikkatli kurumsal müşteri ağ çevre mimarisine bağımlıdır. Bu rol, geliştirilmiş telemetri ölçümlerini ve tasarım önerilerini kolaylaştırmak için, bulunan Kullanıcı konumlarının düzenlenmesine ve bu konumların ağ parametrelerinin yapılandırılmasını sağlar
### <a name="office-apps-administrator"></a>[Office uygulamaları Yöneticisi](#office-apps-administrator-permissions)

Bu roldeki kullanıcılar, Microsoft 365 uygulamalarının bulut ayarlarını yönetebilir. Bu, bulut ilkelerinin yönetimi, self servis indirme yönetimi ve Office uygulamalarıyla ilgili raporu görüntüleme imkanını içerir. Bu rol Ayrıca, destek biletlerini yönetme ve ana yönetim merkezinde hizmet durumunu izleme özelliğini sağlar. Bu role atanan kullanıcılar, Office uygulamalarındaki yeni özelliklerin iletişimini de yönetebilir. 

### <a name="partner-tier1-support"></a>[Partner Katman1 desteği](#partner-tier1-support-permissions)

Kullanmayın. Bu rol kullanımdan kaldırılmıştır ve gelecekte Azure AD 'den kaldırılacak. Bu rol, az sayıda Microsoft yeniden iş ortağı tarafından kullanılmaya yöneliktir ve genel kullanım için tasarlanmamıştır.

### <a name="partner-tier2-support"></a>[Partner Katman2 desteği](#partner-tier2-support-permissions)

Kullanmayın. Bu rol kullanımdan kaldırılmıştır ve gelecekte Azure AD 'den kaldırılacak. Bu rol, az sayıda Microsoft yeniden iş ortağı tarafından kullanılmaya yöneliktir ve genel kullanım için tasarlanmamıştır.

### <a name="password-administrator"></a>[Parola Yöneticisi](#password-administrator-permissions)

Bu role sahip olan kullanıcılar, parolaları yönetme yeteneğine sahiptir. Bu rol, hizmet isteklerini yönetme veya hizmet durumunu izleme özelliği vermez. Parola yöneticisinin bir kullanıcının parolasını sıfırlayıp sıfırlayamayacağı, kullanıcının atandığı role bağlıdır. Parola yöneticisinin parolalarını sıfırlayaacağı rollerin listesi için bkz. [parola sıfırlama izinleri](#password-reset-permissions).

### <a name="power-bi-administrator"></a>[Power BI Yöneticisi](#power-bi-administrator-permissions)

Bu role sahip olan kullanıcılar Microsoft Power BI içinde genel izinlere sahiptir, hizmet mevcut olduğunda, destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. [Power BI yönetici rolünü anlama](/power-bi/service-admin-role)hakkında daha fazla bilgi.

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell 'de, bu rol "Power BI Hizmet Yöneticisi" olarak tanımlanır. Bu, [Azure Portal](https://portal.azure.com)"Power BI Yöneticisi" dir.

### <a name="power-platform-administrator"></a>[Power Platform Yöneticisi](#power-platform-administrator-permissions)

Bu roldeki kullanıcılar, ortamların, PowerApps, akışların ve veri kaybı önleme ilkelerinin tüm yönlerini oluşturabilir ve yönetebilir. Ayrıca, bu role sahip olan kullanıcılar destek biletlerini yönetebilir ve hizmet durumunu izleyebilir.

### <a name="printer-administrator"></a>[Yazıcı Yöneticisi](#printer-administrator-permissions)

Bu roldeki kullanıcılar, Evrensel Yazdırma Bağlayıcısı ayarları dahil olmak üzere Microsoft Evrensel Yazdırma çözümünde tüm yazıcı yapılandırmalarının yazıcılarını kaydedebilir ve tüm yönlerini yönetebilir. Tüm temsilci atanmış yazdırma izni isteklerini kabul edebilirler. Yazıcı yöneticilerinin Ayrıca raporları yazdırma erişimi de vardır.

### <a name="printer-technician"></a>[Yazıcı teknisyeni](#printer-technician-permissions)

Bu role sahip kullanıcılar Microsoft Evrensel Yazdırma çözümünde yazıcı kaydedebilir ve yazıcı durumunu yönetebilir. Ayrıca, tüm bağlayıcı bilgilerini okuyabilir. Anahtar görevi bir yazıcı teknisyeni, yazıcılar ve paylaşım yazıcılarında Kullanıcı izinlerini ayarladı.

### <a name="privileged-authentication-administrator"></a>[Ayrıcalıklı kimlik doğrulama Yöneticisi](#privileged-authentication-administrator-permissions)

Bu role sahip kullanıcılar, genel yöneticiler dahil olmak üzere herhangi bir kullanıcı için herhangi bir kimlik doğrulama yöntemi (parolalar dahil) ayarlayabilir veya sıfırlayabilir. Ayrıcalıklı kimlik doğrulama yöneticileri, kullanıcıların mevcut parola olmayan kimlik bilgilerine (MFA veya FIDO gibi) yeniden kaydolmasını ve ' cihazda MFA 'yı hatırlamaları için, tüm kullanıcıların bir sonraki oturum açma aşamasında MFA istemini sormasını sağlayabilir. 

[Kimlik doğrulama Yöneticisi](#authentication-administrator) rolünün, yönetici rollerine sahip standart kullanıcılar ve kullanıcılar için yeniden kaydolmayı ve Multi-Factor Authentication 'ı zorlama izni vardır.

[Kimlik doğrulama İlkesi yönetici](#authentication-policy-administrator) rolü, her kullanıcının kaydedebileceği ve kullanabileceği yöntemleri belirleyen kiracının kimlik doğrulama yöntemi ilkesini ayarlama iznine sahiptir.

| Rol | Kullanıcının kimlik doğrulama yöntemlerini yönetme | Kullanıcı başına MFA yönetme | MFA ayarlarını yönetme | Kimlik doğrulama yöntemi ilkesini Yönet | Parola koruma ilkesini Yönet |
| ---- | ---- | ---- | ---- | ---- | ---- | 
| Kimlik doğrulama Yöneticisi | Bazı kullanıcılar için Evet (yukarıya bakın) | Bazı kullanıcılar için Evet (yukarıya bakın) | Hayır | Hayır | Hayır | 
| Ayrıcalıklı kimlik doğrulama Yöneticisi| Tüm kullanıcılar için Evet | Tüm kullanıcılar için Evet | Hayır | Hayır | Hayır | 
| Kimlik doğrulama İlkesi Yöneticisi | Hayır | Hayır | Yes | Yes | Yes | 

> [!IMPORTANT]
> Bu role sahip kullanıcılar, Azure Active Directory ' nin içindeki ve dışındaki gizli veya özel bilgilere veya kritik yapılandırmaya erişimi olabilecek kişiler için kimlik bilgilerini değiştirebilir. Bir kullanıcının kimlik bilgilerini değiştirmek, kullanıcının kimliğini ve izinlerini kabul etme imkanını ifade edebilir. Örneğin:
>
>* Sahip oldukları uygulamaların kimlik bilgilerini yönetebilen uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaların Azure AD 'de ayrıcalıklı izinleri olabilir ve kimlik doğrulama yöneticilerine başka bir yerde izin verilmez. Bu yol aracılığıyla bir kimlik doğrulama Yöneticisi bir uygulama sahibinin kimliğini varsayabilir ve sonra uygulamanın kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini daha fazla varsayabilir.
>* Azure abonelik sahipleri, hassas veya özel bilgilere veya Azure 'da kritik yapılandırmaya erişebilir.
>* Grup üyeliğini yönetebilen güvenlik grubu ve Grup sahipleri Microsoft 365. Bu gruplar, Azure AD 'de ve başka bir yerde hassas veya özel bilgilere veya kritik yapılandırmaya erişim verebilir.
>* Exchange Online, Office Security ve Uyumluluk Merkezi ve insan kaynakları sistemleri gibi Azure AD dışında diğer hizmetlerde bulunan yöneticiler.
>* Gizli veya özel bilgilere erişebilen Yöneticiler, yasal Counsel ve insan kaynakları çalışanları gibi yönetici olmayanlar.


> [!IMPORTANT]
> Bu rol, şu anda eski MFA yönetim portalındaki Kullanıcı başına MFA 'yı yönetme yeteneğine sahip değil. Aynı işlevler [set-MsolUser](https://docs.microsoft.com/powershell/module/msonline/set-msoluser) komutunu Azure AD PowerShell modülü kullanılarak gerçekleştirilebilir.

### <a name="privileged-role-administrator"></a>[Ayrıcalıklı rol yöneticisi](#privileged-role-administrator-permissions)

Bu role sahip kullanıcılar, Azure Active Directory ve Azure AD Privileged Identity Management içindeki rol atamalarını yönetebilir. Azure AD rollerine atanabilecek grupları oluşturabilir ve yönetebilir. Ayrıca, bu rol Privileged Identity Management ve yönetim birimlerinin tüm yönlerinin yönetimine izin verir.

> [!IMPORTANT]
> Bu rol, genel yönetici rolü dahil olmak üzere tüm Azure AD rolleri için atamaları yönetme olanağı verir. Bu rol, Azure AD 'de Kullanıcı oluşturma veya güncelleştirme gibi başka ayrıcalıklı yetenekler içermez. Ancak, bu role atanan kullanıcılar, ek roller atayarak kendilerine veya başkalarına ek ayrıcalık verebilir.

### <a name="reports-reader"></a>[Rapor okuyucu](#reports-reader-permissions)

Bu role sahip kullanıcılar, Microsoft 365 Yönetim Merkezi 'nde ve Power BI ' de benimseme bağlam paketindeki kullanım raporlama verilerini ve raporlar panosunu görüntüleyebilir. Rol Ayrıca, Azure AD 'de oturum açma raporlarına ve etkinliklere ve Microsoft Graph Raporlama API 'SI tarafından döndürülen verilere erişim sağlar. Rapor okuyucu rolüne atanan bir Kullanıcı yalnızca ilgili kullanım ve benimseme ölçümlerine erişebilir. Ayarları yapılandırmak veya Exchange gibi ürüne özel yönetim merkezlerine erişmek için yönetici izinleri yoktur. Bu rolün destek biletlerini görüntüleme, oluşturma veya yönetme erişimi yok.

### <a name="search-administrator"></a>[Yönetici ara](#search-administrator-permissions)

Bu roldeki kullanıcılar, Microsoft 365 Yönetim Merkezi 'ndeki tüm Microsoft arama yönetimi özelliklerine tam erişime sahiptir. Ayrıca, bu kullanıcılar ileti merkezini görüntüleyebilir, hizmet durumunu izleyebilir ve hizmet istekleri oluşturabilir.

### <a name="search-editor"></a>[Arama Düzenleyicisi](#search-editor-permissions)

Bu roldeki kullanıcılar, Microsoft 365 Yönetim Merkezi 'nde yer işaretleri, Q&as ve konumlar dahil Microsoft Search için içerik oluşturabilir, yönetebilir ve silebilir.

### <a name="security-administrator"></a>[Güvenlik Yöneticisi](#security-administrator-permissions)

Bu role sahip olan kullanıcılar, Microsoft 365 Güvenlik Merkezi, Azure Active Directory Kimlik Koruması, Azure Active Directory kimlik doğrulaması, Azure Information Protection ve Office 365 güvenlik & Uyumluluk Merkezi 'ndeki güvenlikle ilgili özellikleri yönetme iznine sahiptir. Office 365 izinleri hakkında daha fazla bilgi [Için güvenlik & Uyumluluk Merkezi ' nde izinler](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)bulabilirsiniz.

İçinde | Yapılabilir
--- | ---
[Microsoft 365 Güvenlik Merkezi](https://protection.office.com) | Microsoft 365 hizmetleri arasında güvenlikle ilgili ilkeleri izleme<br>Güvenlik tehditlerini ve uyarıları yönetme<br>Raporları görüntüle
Kimlik koruma Merkezi | Güvenlik okuyucusu rolünün tüm izinleri<br>Ayrıca, parola sıfırlama hariç tüm kimlik koruması merkezi işlemlerini gerçekleştirme özelliği
[Privileged Identity Management](../privileged-identity-management/pim-configure.md) | Güvenlik okuyucusu rolünün tüm izinleri<br>Azure AD rol atamaları veya **ayarları yönetemez**
[Office 365 güvenlik & Uyumluluk Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Güvenlik ilkelerini yönetin<br>Güvenlik tehditlerini görüntüleyin, araştırın ve yanıtlayın<br>Raporları görüntüle
Azure Gelişmiş Tehdit Koruması | Şüpheli güvenlik etkinliğini izleyin ve yanıtlayın
Windows Defender ATP ve EDR | Rolleri atama<br>Makine gruplarını yönetme<br>Uç nokta tehdit algılamasını ve otomatik düzeltmeyi yapılandırma<br>Uyarıları görüntüleyin, araştırın ve yanıtlayın
[Intune](/intune/role-based-access-control) | Kullanıcı, cihaz, kayıt, yapılandırma ve uygulama bilgilerini görüntüler<br>Intune üzerinde değişiklik yapılamıyor
[Cloud App Security](/cloud-app-security/manage-admins) | Yönetici ekleme, ilke ve ayarlar ekleme, günlükleri karşıya yükleme ve idare eylemleri gerçekleştirme
[Azure Güvenlik Merkezi](../../key-vault/managed-hsm/built-in-roles.md) | Güvenlik ilkelerini görüntüleyebilir, güvenlik durumlarını görüntüleyebilir, güvenlik ilkelerini düzenleyebilir, uyarıları ve önerileri görüntüleyebilir, uyarıları ve önerileri kapatabilir
[Microsoft 365 hizmet durumu](/office365/enterprise/view-service-health) | Microsoft 365 hizmetlerinin sistem durumunu görüntüleme
[Akıllı kilitleme](../authentication/howto-password-smart-lockout.md) | Başarısız oturum açma olayları gerçekleştiğinde, loctattatların eşiğini ve süresini tanımlayın.
[Parola koruması](../authentication/concept-password-ban-bad.md) | Özel yasaklanmış parola listesini veya şirket içi parola korumasını yapılandırın.

### <a name="security-operator"></a>[Güvenlik operatörü](#security-operator-permissions)

Bu role sahip kullanıcılar, uyarıları yönetebilir ve güvenlikle ilgili özellikler üzerinde Microsoft 365 Güvenlik Merkezi, Azure Active Directory, kimlik koruması Privileged Identity Management ve Office 365 güvenlik & Uyumluluk Merkezi 'ndeki tüm bilgiler dahil olmak üzere genel salt okuma erişimine sahip olabilir. Office 365 izinleri hakkında daha fazla bilgi [Için güvenlik & Uyumluluk Merkezi ' nde izinler](/office365/securitycompliance/permissions-in-the-security-and-compliance-center)bulabilirsiniz.

İçinde | Yapılabilir
--- | ---
[Microsoft 365 Güvenlik Merkezi](https://protection.office.com) | Güvenlik okuyucusu rolünün tüm izinleri<br>Güvenlik tehditleri uyarılarını görüntüleyin, araştırın ve yanıtlayın
Azure AD Kimlik Koruması | Güvenlik okuyucusu rolünün tüm izinleri<br>Ayrıca, parolaları sıfırlama ve uyarı e-postalarını yapılandırma dışında tüm kimlik koruması merkezi işlemlerini gerçekleştirme özelliği de vardır.
[Privileged Identity Management](../privileged-identity-management/pim-configure.md) | Güvenlik okuyucusu rolünün tüm izinleri
[Office 365 güvenlik & Uyumluluk Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Güvenlik okuyucusu rolünün tüm izinleri<br>Güvenlik uyarılarını görüntüleyin, araştırın ve yanıtlayın
Windows Defender ATP ve EDR | Güvenlik okuyucusu rolünün tüm izinleri<br>Güvenlik uyarılarını görüntüleyin, araştırın ve yanıtlayın
[Intune](/intune/role-based-access-control) | Güvenlik okuyucusu rolünün tüm izinleri
[Cloud App Security](/cloud-app-security/manage-admins) | Güvenlik okuyucusu rolünün tüm izinleri
[Microsoft 365 hizmet durumu](/office365/enterprise/view-service-health) | Microsoft 365 hizmetlerinin sistem durumunu görüntüleme

### <a name="security-reader"></a>[Güvenlik Okuyucusu](#security-reader-permissions)

Bu role sahip olan kullanıcılar, Microsoft 365 Güvenlik Merkezi, Azure Active Directory, kimlik koruması, Privileged Identity Management ve Azure Active Directory oturum açma raporlarını ve denetim günlüklerini okuma ve Office 365 güvenlik & Uyumluluk Merkezi 'ndeki tüm bilgiler dahil olmak üzere güvenlikle ilgili özellik üzerinde genel salt okuma erişimine sahiptir. Office 365 izinleri hakkında daha fazla bilgi [Için güvenlik & Uyumluluk Merkezi ' nde izinler](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)bulabilirsiniz.

İçinde | Yapılabilir
--- | ---
[Microsoft 365 Güvenlik Merkezi](https://protection.office.com) | Microsoft 365 hizmetleri arasında güvenlikle ilgili ilkeleri görüntüleme<br>Güvenlik tehditlerini ve uyarılarını görüntüleme<br>Raporları görüntüle
Kimlik koruma Merkezi | Güvenlik özellikleri için tüm güvenlik raporlarını ve ayar bilgilerini okuyun<br><ul><li>İstenmeyen postadan koruma<li>Şifreleme<li>Veri kaybını önleme<li>Kötü amaçlı yazılımdan koruma<li>Gelişmiş tehdit koruması<li>Kimlik avlayan koruma<li>Mailflow kuralları
[Privileged Identity Management](../privileged-identity-management/pim-configure.md) | Azure AD Privileged Identity Management ' de ortaya çıkacak tüm bilgilere salt okuma erişimi vardır: Azure AD rol atamaları ve güvenlik incelemeleri için Ilkeler ve raporlar.<br>Azure AD Privileged Identity Management için **kaydolabilir veya** herhangi bir değişiklik yapabilirsiniz. Privileged Identity Management portalında veya PowerShell aracılığıyla, bu roldeki birisi, Kullanıcı için uygun olması durumunda ek rolleri (örneğin, genel yönetici veya ayrıcalıklı rol yöneticisi) etkinleştirebilir.
[Office 365 güvenlik & Uyumluluk Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Güvenlik ilkelerini görüntüleme<br>Güvenlik tehditlerini görüntüle ve araştır<br>Raporları görüntüle
Windows Defender ATP ve EDR | Uyarıları görüntüleyin ve araştırın. Windows Defender ATP 'de rol tabanlı erişim denetimini açtığınızda, Azure AD güvenlik okuyucusu rolü gibi salt okuma izinlerine sahip kullanıcılar bir Windows Defender ATP rolüne atanana kadar erişimi kaybeder.
[Intune](/intune/role-based-access-control) | Kullanıcı, cihaz, kayıt, yapılandırma ve uygulama bilgilerini görüntüler. Intune’da değişiklik yapamaz.
[Cloud App Security](/cloud-app-security/manage-admins) | Salt okuma izinlerine sahiptir ve uyarıları yönetebilir
[Azure Güvenlik Merkezi](../../key-vault/managed-hsm/built-in-roles.md) | Önerileri ve uyarıları görüntüleyebilir, güvenlik ilkelerini görüntüleyebilir, güvenlik durumlarını görüntüleyebilir, ancak değişiklik yapamaz
[Microsoft 365 hizmet durumu](/office365/enterprise/view-service-health) | Microsoft 365 hizmetlerinin sistem durumunu görüntüleme

### <a name="service-support-administrator"></a>[Hizmet desteği Yöneticisi](#service-support-administrator-permissions)

Bu role sahip kullanıcılar, Azure ve Microsoft 365 hizmetleri için Microsoft ile destek istekleri açabilir ve [Azure Portal](https://portal.azure.com) ve [Microsoft 365 Yönetim Merkezi](https://admin.microsoft.com)'nde hizmet panosu ve ileti merkezini görüntüler. [Yönetici rolleri hakkında](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)daha fazla bilgi.

> [!NOTE]
> Daha önce bu rol, [Azure Portal](https://portal.azure.com) ve [Microsoft 365 Yönetim Merkezi](https://admin.microsoft.com)'nde "Hizmet Yöneticisi" olarak adlandırılmıştı. Microsoft Graph API 'SI, Azure AD Graph API ve Azure AD PowerShell 'de bulunan adı ile uyum sağlamak için bunu "hizmet desteği Yöneticisi" olarak yeniden adlandırdık.

### <a name="sharepoint-administrator"></a>[SharePoint Yöneticisi](#sharepoint-administrator-permissions)

Bu role sahip olan kullanıcılar Microsoft SharePoint Online içinde genel izinlere sahiptir, hizmet mevcut olduğunda, tüm Microsoft 365 grupları oluşturup yönetebilir, destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. [Yönetici rolleri hakkında](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)daha fazla bilgi.

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell 'de, bu rol "SharePoint Hizmet Yöneticisi" olarak tanımlanır. Bu, [Azure Portal](https://portal.azure.com)"SharePoint Yöneticisi" dir.

> [!NOTE]
> Bu rol Ayrıca, Microsoft Intune için Microsoft Graph API 'sine kapsamlı izinler vererek SharePoint ve OneDrive kaynaklarıyla ilgili ilkelerin yönetimine ve yapılandırılmasına izin verir.

### <a name="skype-for-business-administrator"></a>[Skype Kurumsal Yöneticisi](#skype-for-business-administrator-permissions)

Bu role sahip olan kullanıcılar, hizmet mevcut olduğunda, Microsoft Skype Kurumsal içinde genel izinlere sahiptir ve ayrıca Azure Active Directory Skype 'e özgü kullanıcı özniteliklerini yönetir. Ayrıca, bu rol, destek biletlerini yönetme ve hizmet durumunu izleme ve takımlar ve Skype Kurumsal yönetim merkezine erişme olanağı verir. Hesap Ayrıca takımlar için lisanslanmış olmalıdır veya takımlar PowerShell cmdlet 'lerini çalıştıramıyor. Skype Kurumsal [yönetici rolü](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) ve [Skype Kurumsal ve Microsoft ekipleri eklenti lisanslaması](/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing) hakkında daha fazla bilgi

> [!NOTE]
> Microsoft Graph API ve Azure AD PowerShell 'de, bu rol "Lync Hizmet Yöneticisi" olarak tanımlanır. Bu, [Azure Portal](https://portal.azure.com/)"Skype Kurumsal Yöneticisi" dir.

### <a name="teams-administrator"></a>[Takımlar Yöneticisi](#teams-administrator-permissions)

Bu roldeki kullanıcılar Microsoft ekipleri & Skype Kurumsal Yönetici Merkezi ve ilgili PowerShell modülleri aracılığıyla Microsoft ekipleri iş yükünün tüm yönlerini yönetebilir. Bu, diğer alanların yanı sıra telefon, mesajlaşma, toplantılar ve takımların kendileri ile ilgili tüm yönetim araçlarını içerir. Bu rol Ayrıca, tüm Microsoft 365 grupları oluşturma ve yönetme, destek biletlerini yönetme ve hizmet durumunu izleme özelliğini verir.

### <a name="teams-communications-administrator"></a>[Takımlar Iletişim Yöneticisi](#teams-communications-administrator-permissions)

Bu roldeki kullanıcılar, Voice & telefonlarıyla ilgili Microsoft ekipleri iş yükünün yönlerini yönetebilir. Buna telefon numarası atama, sesli ve toplantı ilkelerinin yönetim araçları ve çağrı Analizi araç takımını tam erişim dahildir.

### <a name="teams-communications-support-engineer"></a>[Takımlar Iletişimleri Destek Mühendisi](#teams-communications-support-engineer-permissions)

Bu roldeki kullanıcılar, Microsoft ekiplerinde Skype Kurumsal Yönetim Merkezi & Kullanıcı çağrısı sorun giderme araçlarını kullanarak Microsoft & ekiplerinde bulunan iletişim sorunlarını giderebilir. Bu roldeki kullanıcılar, dahil edilen tüm katılımcılar için tam çağrı kaydı bilgilerini görüntüleyebilir. Bu rolün destek biletlerini görüntüleme, oluşturma veya yönetme erişimi yok.

### <a name="teams-communications-support-specialist"></a>[Takımlar Iletişimleri destek uzmanı](#teams-communications-support-specialist-permissions)

Bu roldeki kullanıcılar, Microsoft ekiplerinde Skype Kurumsal Yönetim Merkezi & Kullanıcı çağrısı sorun giderme araçlarını kullanarak Microsoft & ekiplerinde bulunan iletişim sorunlarını giderebilir. Bu roldeki kullanıcılar, yalnızca baktıkları belirli kullanıcı için yapılan çağrıda Kullanıcı ayrıntılarını görüntüleyebilir. Bu rolün destek biletlerini görüntüleme, oluşturma veya yönetme erişimi yok.

### <a name="teams-devices-administrator"></a>[Takımlar cihazları Yöneticisi](#teams-devices-administrator-permissions)

Bu role sahip kullanıcılar takımlar [tarafından sertifikalı cihazları](https://www.microsoft.com/microsoft-365/microsoft-teams/across-devices/devices) takımlar yönetim merkezinden yönetebilir. Bu rol, cihazları arama ve filtreleme özelliği sayesinde tüm cihazların tek bakışta görüntülenmesine izin verir. Kullanıcı, cihazın oturum açma hesabı, marka ve modeli dahil olmak üzere her bir cihazın ayrıntılarını denetleyebilir. Kullanıcı cihazdaki ayarları değiştirebilir ve yazılım sürümlerini güncelleştirebilir. Bu rol, ekip etkinliğini denetlemek ve cihazın kalitesini çağırmak için izinler vermez. 

### <a name="usage-summary-reports-reader"></a>[Kullanım Özeti raporları okuyucusu](#usage-summary-reports-reader-permissions)

Bu role sahip kullanıcılar, kullanım ve üretkenlik puanı için Microsoft 365 Yönetim Merkezi 'nde kiracı düzeyindeki toplu verilere ve ilişkili öngörülere erişebilir, ancak herhangi bir Kullanıcı düzeyi ayrıntılarına veya öngörülere erişemez. İki rapor için Microsoft 365 Yönetim Merkezi 'nde, kiracı düzeyi toplanmış veriler ve Kullanıcı düzeyi ayrıntıları arasında ayrım yaptık. Bu rol, hem müşteriler hem de yasal takımlar tarafından istenen bireysel kullanıcı tarafından tanımlanabilen veriler üzerinde ek bir koruma katmanı sağlar. 

### <a name="user-administrator"></a>[Kullanıcı Yöneticisi](#user-administrator-permissions)

Bu role sahip kullanıcılar Kullanıcı oluşturabilir ve bazı kısıtlamalara sahip kullanıcıların tüm yönlerini yönetebilir (tabloya bakın) ve parola süre sonu ilkelerini güncelleştirebilir. Ayrıca, bu role sahip kullanıcılar tüm grupları oluşturabilir ve yönetebilir. Bu rol Ayrıca Kullanıcı görünümleri oluşturma ve yönetme, destek biletlerini yönetme ve hizmet durumunu izleme özelliğini de içerir. Kullanıcı yöneticilerinin çoğu yönetici rolünde kullanıcılar için bazı kullanıcı özelliklerini yönetme izni yoktur. Bu role sahip olan kullanıcının MFA 'yı yönetme izni yok. Bu kısıtlamanın özel durumları olan roller aşağıdaki tabloda listelenmiştir.

| Kullanıcı Yöneticisi izni | Notlar |
| --- | --- |
| Kullanıcı ve grup oluşturma<br/>Kullanıcı görünümleri oluşturma ve yönetme<br/>Office destek biletlerini yönetme<br/>Parola süre sonu ilkelerini Güncelleştir |  |
| Lisansları yönetme<br/>Kullanıcı asıl adı dışındaki tüm kullanıcı özelliklerini yönet | Tüm yöneticiler dahil tüm kullanıcılar için geçerlidir |
| Sil ve geri yükle<br/>Devre dışı bırak ve Etkinleştir<br/>Kullanıcı asıl adı dahil tüm kullanıcı özelliklerini yönet<br/>Güncelleştirme (FIDO) cihaz anahtarları | Yönetici olmayan veya aşağıdaki rollerden hiçbirinde olan kullanıcılar için geçerlidir:<ul><li>Yardım Masası Yöneticisi</li><li>Rolü olmayan Kullanıcı</li><li>Kullanıcı Yöneticisi</li></ul> |
| Yenileme belirteçlerini geçersiz kıl<br/>Parola sıfırlama | Bir Kullanıcı yöneticisinin parola sıfırlayıp yenileme belirteçlerini geçersiz kılabileceği rollerin listesi için bkz. [parola sıfırlama izinleri](#password-reset-permissions). |

> [!IMPORTANT]
> Bu role sahip kullanıcılar, Azure Active Directory ' nin içindeki ve dışındaki gizli veya özel bilgilere veya kritik yapılandırmaya erişimi olabilecek kişilerin parolalarını değiştirebilir. Bir kullanıcının parolasını değiştirmek, kullanıcının kimliğini ve izinlerini kabul etme imkanını ifade edebilir. Örneğin:
>
>- Sahip oldukları uygulamaların kimlik bilgilerini yönetebilen uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaların Azure AD 'de ayrıcalıklı izinleri olabilir ve Kullanıcı yöneticilerine başka bir yerde izin verilmez. Bu yoldan, bir Kullanıcı Yöneticisi bir uygulama sahibinin kimliğini varsayabilir ve daha sonra uygulamanın kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini daha fazla varsayabilir.
>- Azure abonelik sahipleri, hassas veya özel bilgilere veya Azure 'da kritik yapılandırmaya erişebilir.
>- Grup üyeliğini yönetebilen güvenlik grubu ve Grup sahipleri Microsoft 365. Bu gruplar, Azure AD 'de ve başka bir yerde hassas veya özel bilgilere veya kritik yapılandırmaya erişim verebilir.
>- Exchange Online, Office Security ve Uyumluluk Merkezi ve insan kaynakları sistemleri gibi Azure AD dışında diğer hizmetlerde bulunan yöneticiler.
>- Gizli veya özel bilgilere erişebilen Yöneticiler, yasal Counsel ve insan kaynakları çalışanları gibi yönetici olmayanlar.

## <a name="role-permissions"></a>Rol izinleri

Aşağıdaki tablolarda, her role verilen Azure Active Directory belirli izinler açıklanır. Bazı rollerin, Microsoft hizmetlerinde Azure Active Directory dışında ek izinleri olabilir.

### <a name="application-administrator-permissions"></a>Uygulama Yöneticisi izinleri

, Uygulama kayıtlarının ve kurumsal uygulamaların tüm yönlerini oluşturabilir ve yönetebilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Application/appProxyAuthentication/Update | Azure Active Directory 'de hizmet sorumluları 'nda uygulama proxy 'Si kimlik doğrulama özelliklerini güncelleştirin. |
> | Microsoft. Directory/Application/appProxyUrlSettings/Update | Azure Active Directory içindeki uygulama proxy 'si iç ve dış URL 'LERI güncelleştirin. |
> | Microsoft. Directory/uygulamalar/applicationProxy/okuma | Tüm uygulama proxy özelliklerini okuyun. |
> | Microsoft. Directory/uygulamalar/applicationProxy/güncelleştirme | Tüm uygulama proxy özelliklerini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/hedef kitle/güncelleştirme | Azure Active Directory 'da Applications. Audience özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/kimlik doğrulaması/güncelleştirme | Azure Active Directory güncelleştirme uygulamaları. kimlik doğrulama özelliği. |
> | Microsoft. Directory/uygulamalar/temel/güncelleştirme | Azure Active Directory içindeki uygulamalarda temel özellikleri güncelleştirin. |
> | Microsoft. Directory/uygulamalar/oluşturma | Azure Active Directory uygulamalar oluşturun. |
> | Microsoft. Directory/uygulamalar/kimlik bilgileri/güncelleştirme | Azure Active Directory içindeki Applications. Credentials özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/Sil | Azure Active Directory uygulamaları silin. |
> | Microsoft. Directory/uygulamalar/sahipler/güncelleştirme | Azure Active Directory içindeki Applications. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/izinler/güncelleştirme | Azure Active Directory 'da Applications. Permissions özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/ilkeler/güncelleştirme | Azure Active Directory içindeki Applications. Policies özelliğini güncelleştirin. |
> | Microsoft. Directory/Approtaatamalar/oluşturma | Azure Active Directory Approlet atamaları oluşturun. |
> | Microsoft. Directory/Approtaatamalar/okuma | Azure Active Directory Approtaatamaları okuyun. |
> | Microsoft. Directory/Approtaatamalar/güncelleştirme | Azure Active Directory Approtaatamaları güncelleştirin. |
> | Microsoft. Directory/Approtaatamaları/silme | Azure Active Directory Approtaatamaları silin. |
> | Microsoft. Directory/auditLogs/allProperties/Read | Azure Active Directory içindeki auditLogs 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Directory/connectorGroups/allProperties/Read | Azure Active Directory içindeki uygulama proxy Bağlayıcısı grubu özelliklerini okuyun. |
> | Microsoft. Directory/connectorGroups/allProperties/Update | Azure Active Directory tüm uygulama proxy Bağlayıcısı grubu özelliklerini güncelleştirin. |
> | Microsoft. Directory/connectorGroups/Create | Azure Active Directory 'de uygulama proxy Bağlayıcısı grupları oluşturun. |
> | Microsoft. Directory/connectorGroups/Delete | Azure Active Directory içindeki uygulama proxy Bağlayıcısı gruplarını silin. |
> | Microsoft. Directory/bağlayıcılar/allProperties/Read | Azure Active Directory içindeki tüm uygulama proxy Bağlayıcısı özelliklerini okuyun. |
> | Microsoft. Directory/bağlayıcılar/oluşturma | Azure Active Directory içinde uygulama proxy bağlayıcıları oluşturun. |
> | Microsoft. Directory/policies/applicationConfiguration/Basic/Read | Azure Active Directory içindeki policies. applicationConfiguration özelliğini okuyun. |
> | Microsoft. Directory/policies/applicationConfiguration/Basic/Update | Azure Active Directory ilke. applicationConfiguration özelliğini güncelleştirin. |
> | Microsoft. Directory/policies/applicationConfiguration/Create | Azure Active Directory ilke oluşturun. |
> | Microsoft. Directory/policies/applicationConfiguration/Delete | Azure Active Directory ilkeleri silin. |
> | Microsoft. Directory/policies/applicationConfiguration/Owners/Read | Azure Active Directory içindeki policies. applicationConfiguration özelliğini okuyun. |
> | Microsoft. Directory/policies/applicationConfiguration/Owners/Update | Azure Active Directory ilke. applicationConfiguration özelliğini güncelleştirin. |
> | Microsoft. Directory/policies/applicationConfiguration/policyAppliedTo/Read | Azure Active Directory içindeki policies. applicationConfiguration özelliğini okuyun. |
> | Microsoft. Directory/provisioningLogs/allProperties/Read | Sağlama günlüklerinin tüm özelliklerini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/appRoleAssignedTo/Update | Azure Active Directory Servicesorumlularını. appRoleAssignedTo özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/Approtaatamalar/güncelleştirme | Azure Active Directory Servicesorumlularını. Approtaatamalar özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/hedef kitle/güncelleştirme | Azure Active Directory Servicesorumlularını. Audience özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/kimlik doğrulaması/güncelleştirme | Azure Active Directory Servicesorumlularını. Authentication özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/temel/güncelleştirme | Azure Active Directory içindeki Servicesorumlularını temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/oluşturma | Azure Active Directory 'de Servicesorumlularını oluşturun. |
> | Microsoft. Directory/Servicesorumlularını/kimlik bilgileri/güncelleştirme | Azure Active Directory Servicesorumlularını. Credentials özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/silme | Azure Active Directory Servicesorumlularını silin. |
> | Microsoft. Directory/Servicesorumlularını/Owners/Update | Azure Active Directory Servicesorumlularını. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/izinleri/güncelleştirme | Azure Active Directory Servicesorumlularını. Permissions özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/ilkeleri/güncelleştirme | Azure Active Directory Servicesorumlularını. Policies özelliğini güncelleştirin. |
> | Microsoft. Directory/Signınreports/allProperties/Read | Azure Active Directory içindeki Signınreports 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="application-developer-permissions"></a>Uygulama geliştirici izinleri

' Kullanıcılar uygulama kaydedebilir ' ayarından bağımsız olarak uygulama kayıtları oluşturabilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Applications/createAsOwner | Azure Active Directory uygulamalar oluşturun. Oluşturucu ilk sahip olarak eklenir ve oluşturulan nesne, oluşturucunun 250 tarafından oluşturulan nesneler kotasına göre sayılır. |
> | Microsoft. Directory/Approtaatamalar/createAsOwner | Azure Active Directory Approlet atamaları oluşturun. Oluşturucu ilk sahip olarak eklenir ve oluşturulan nesne, oluşturucunun 250 tarafından oluşturulan nesneler kotasına göre sayılır. |
> | Microsoft. Directory/oAuth2PermissionGrants/createAsOwner | Azure Active Directory 'de oAuth2PermissionGrants oluşturun. Oluşturucu ilk sahip olarak eklenir ve oluşturulan nesne, oluşturucunun 250 tarafından oluşturulan nesneler kotasına göre sayılır. |
> | Microsoft. Directory/Servicesorumlularını/createAsOwner | Azure Active Directory 'de Servicesorumlularını oluşturun. Oluşturucu ilk sahip olarak eklenir ve oluşturulan nesne, oluşturucunun 250 tarafından oluşturulan nesneler kotasına göre sayılır. |

### <a name="attack-payload-author-permissions"></a>Saldırı yükü yazar izinleri

, Bir yönetici tarafından daha sonra dağıtılabilecek saldırı yükleri oluşturabilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. protectionCenter/saldırıda Ksimülatör/yük/allProperties/allTasks | Saldırı simülatörü 'nda saldırı yüklerini oluşturun ve yönetin. |
> | Microsoft. office365. protectionCenter/saldırıda Ksimülatör/raporlar/allProperties/Read | Saldırı simülasyonu, yanıtları ve ilgili eğitimin raporlarını okuyun. |

### <a name="attack-simulation-administrator-permissions"></a>Saldırı simülasyonu yönetici izinleri

, Saldırı Benzetimi kampanyalarının tüm yönlerini oluşturabilir ve yönetebilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. protectionCenter/saldırıda Ksimülatör/yük/allProperties/allTasks | Saldırı simülatörü 'nda saldırı yüklerini oluşturun ve yönetin. |
> | Microsoft. office365. protectionCenter/saldırıda Ksimülatör/raporlar/allProperties/Read | Saldırı simülasyonu, yanıtları ve ilgili eğitimin raporlarını okuyun. |
> | Microsoft. office365. protectionCenter/saldırıda Ksimülatör/simülasyon/allProperties/allTasks | Saldırı simülatörü 'nda saldırı simülasyonu şablonları oluşturun ve yönetin. |

### <a name="authentication-administrator-permissions"></a>Kimlik doğrulama Yöneticisi izinleri

Yönetici olmayan kullanıcılar için kimlik doğrulama yöntemi bilgilerini görüntüleme, ayarlama ve sıfırlamaya izin verildi.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Users/ınvalidateallrefreshtokens | Azure Active Directory tüm Kullanıcı yenileme belirteçlerini geçersiz kılar. |
> | Microsoft. Directory/Users/strongAuthentication/Update | MFA kimlik bilgileri gibi güçlü kimlik doğrulama özelliklerini güncelleştirin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. Directory/Users/Password/Update | Microsoft 365 kuruluştaki tüm kullanıcılar için parolaları güncelleştirin. Daha ayrıntılı bilgi için çevrimiçi belgelere bakın. |

### <a name="authentication-policy-administrator-permissions"></a>Kimlik doğrulama Ilkesi yönetici izinleri

Kimlik doğrulama yöntemleri ilkesini, parola koruma ilkesini ve kiracı genelinde MFA ayarlarını görüntüleme ve ayarlamaya izin verilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/kuruluş/strongAuthentication/Update | Azure Active Directory bir kuruluşun güçlü kimlik doğrulama özelliklerini güncelleştirin. |
> | Microsoft. Directory/userCredentialPolicies/oluşturma | Azure Active Directory kullanıcılar için kimlik bilgisi ilkeleri oluşturun. |
> | Microsoft. Directory/userCredentialPolicies/Delete | Azure Active Directory kullanıcılar için kimlik bilgisi ilkelerini silin. |
> | Microsoft. Directory/userCredentialPolicies/standart/okuma | Azure Active Directory kullanıcılar için kimlik bilgisi ilkelerinin standart özelliklerini okuyun. |
> | Microsoft. Directory/userCredentialPolicies/sahipler/okuma | Azure Active Directory kullanıcılar için kimlik bilgisi ilkelerinin sahiplerini okuyun. |
> | Microsoft. Directory/userCredentialPolicies/policyAppliedTo/Read | Azure Active Directory ilke. appliesTo gezinti bağlantısını okuyun. |
> | Microsoft. Directory/userCredentialPolicies/temel/güncelleştirme | Azure Active Directory kullanıcılar için temel ilkeleri güncelleştirin. |
> | Microsoft. Directory/userCredentialPolicies/Owners/Update | Azure Active Directory kullanıcılar için kimlik bilgisi ilkelerinin sahiplerini güncelleştirin. |
> | Microsoft. Directory/userCredentialPolicies/tenantDefault/Update | Azure Active Directory ilke. ıorganizationdefault özelliğini güncelleştirin. |

### <a name="azure-ad-joined-device-local-administrator-permissions"></a>Azure AD 'ye katılmış cihaz yerel yönetici izinleri

Bu role atanan kullanıcılar, Azure AD 'ye katılmış cihazlarda yerel Yöneticiler grubuna eklenir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/groupSettings/Basic/Read | Azure Active Directory groupSettings üzerindeki temel özellikleri okuyun. |
> | Microsoft. Directory/groupSettingTemplates/Basic/Read | Azure Active Directory groupSettingTemplates üzerindeki temel özellikleri okuyun. |

### <a name="azure-devops-administrator-permissions"></a>Azure DevOps yönetici izinleri

, Azure DevOps kuruluş ilkesini ve ayarlarını yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki [rol açıklaması](#azure-devops-administrator) konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. devOps/allEntities/allTasks | Azure DevOps 'u okuyun ve yapılandırın. |

### <a name="azure-information-protection-administrator-permissions"></a>Yönetici izinlerini Azure Information Protection

Azure Information Protection hizmetinin tüm yönlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki [rol açıklaması](#) konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. ınformationprotection/allEntities/allTasks | Azure Information Protection tüm yönlerini yönetin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="b2c-ief-keyset-administrator-permissions"></a>B2C ıEF anahtar kümesi yönetici izinleri

Kimlik deneyimi çerçevesinde Federasyon ve şifreleme için gizli dizileri yönetin.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. AAD. B2C/trustFramework/keySets/allTasks | Azure Active Directory B2C 'de anahtar kümelerini okuyun ve yapılandırın. |

### <a name="b2c-ief-policy-administrator-permissions"></a>B2C ıEF Ilkesi yönetici izinleri

Kimlik deneyimi çerçevesinde güven çerçevesi ilkeleri oluşturun ve yönetin.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. AAD. B2C/trustFramework/policies/allTasks | Azure Active Directory B2C özel ilkeleri okuyun ve yapılandırın. |

### <a name="billing-administrator-permissions"></a>Faturalandırma Yöneticisi izinleri

, Ödeme bilgilerini güncelleştirme gibi genel faturalandırma ile ilgili görevleri gerçekleştirebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/kuruluş/temel/güncelleştirme | Azure Active Directory 'da kuruluştaki temel özellikleri güncelleştirin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Commerce. faturalandırma/allEntities/allTasks | Faturalandırma 'nin tüm yönlerini yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="cloud-application-administrator-permissions"></a>Bulut uygulaması yönetici izinleri

Uygulama kayıtları ve kurumsal uygulamaların uygulama proxy 'Si hariç tüm yönlerini oluşturabilir ve yönetebilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/uygulamalar/hedef kitle/güncelleştirme | Azure Active Directory 'da Applications. Audience özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/kimlik doğrulaması/güncelleştirme | Azure Active Directory güncelleştirme uygulamaları. kimlik doğrulama özelliği. |
> | Microsoft. Directory/uygulamalar/temel/güncelleştirme | Azure Active Directory içindeki uygulamalarda temel özellikleri güncelleştirin. |
> | Microsoft. Directory/uygulamalar/oluşturma | Azure Active Directory uygulamalar oluşturun. |
> | Microsoft. Directory/uygulamalar/kimlik bilgileri/güncelleştirme | Azure Active Directory içindeki Applications. Credentials özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/Sil | Azure Active Directory uygulamaları silin. |
> | Microsoft. Directory/uygulamalar/sahipler/güncelleştirme | Azure Active Directory içindeki Applications. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/izinler/güncelleştirme | Azure Active Directory 'da Applications. Permissions özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/ilkeler/güncelleştirme | Azure Active Directory içindeki Applications. Policies özelliğini güncelleştirin. |
> | Microsoft. Directory/Approtaatamalar/oluşturma | Azure Active Directory Approlet atamaları oluşturun. |
> | Microsoft. Directory/Approtaatamalar/güncelleştirme | Azure Active Directory Approtaatamaları güncelleştirin. |
> | Microsoft. Directory/Approtaatamaları/silme | Azure Active Directory Approtaatamaları silin. |
> | Microsoft. Directory/auditLogs/allProperties/Read | Azure Active Directory içindeki auditLogs 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Directory/policies/applicationConfiguration/Create | Azure Active Directory ilke oluşturun. |
> | Microsoft. Directory/policies/applicationConfiguration/Basic/Read | Azure Active Directory içindeki policies. applicationConfiguration özelliğini okuyun. |
> | Microsoft. Directory/policies/applicationConfiguration/Basic/Update | Azure Active Directory ilke. applicationConfiguration özelliğini güncelleştirin. |
> | Microsoft. Directory/policies/applicationConfiguration/Delete | Azure Active Directory ilkeleri silin. |
> | Microsoft. Directory/policies/applicationConfiguration/Owners/Read | Azure Active Directory içindeki policies. applicationConfiguration özelliğini okuyun. |
> | Microsoft. Directory/policies/applicationConfiguration/Owners/Update | Azure Active Directory ilke. applicationConfiguration özelliğini güncelleştirin. |
> | Microsoft. Directory/policies/applicationConfiguration/policyAppliedTo/Read | Azure Active Directory içindeki policies. applicationConfiguration özelliğini okuyun. |
> | Microsoft. Directory/provisioningLogs/allProperties/Read | Sağlama günlüklerinin tüm özelliklerini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/appRoleAssignedTo/Update | Azure Active Directory Servicesorumlularını. appRoleAssignedTo özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/Approtaatamalar/güncelleştirme | Azure Active Directory Servicesorumlularını. Approtaatamalar özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/hedef kitle/güncelleştirme | Azure Active Directory Servicesorumlularını. Audience özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/kimlik doğrulaması/güncelleştirme | Azure Active Directory Servicesorumlularını. Authentication özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/temel/güncelleştirme | Azure Active Directory içindeki Servicesorumlularını temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/oluşturma | Azure Active Directory 'de Servicesorumlularını oluşturun. |
> | Microsoft. Directory/Servicesorumlularını/kimlik bilgileri/güncelleştirme | Azure Active Directory Servicesorumlularını. Credentials özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/silme | Azure Active Directory Servicesorumlularını silin. |
> | Microsoft. Directory/Servicesorumlularını/Owners/Update | Azure Active Directory Servicesorumlularını. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/izinleri/güncelleştirme | Azure Active Directory Servicesorumlularını. Permissions özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/ilkeleri/güncelleştirme | Azure Active Directory Servicesorumlularını. Policies özelliğini güncelleştirin. |
> | Microsoft. Directory/Signınreports/allProperties/Read | Azure Active Directory içindeki Signınreports 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="cloud-device-administrator-permissions"></a>Bulut aygıtı yönetici izinleri

Azure AD 'de cihazları yönetmek için tam erişim.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/auditLogs/allProperties/Read | Azure Active Directory içindeki auditLogs 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Directory/bitlockerKeys/Key/Read | Azure Active Directory içindeki BitLocker anahtar nesnelerini ve özelliklerini (kurtarma anahtarı dahil) okuyun. |
> | Microsoft. Directory/Devices/Delete | Azure Active Directory cihazları silin. |
> | Microsoft. Directory/cihazlar/devre dışı bırak | Azure Active Directory cihazları devre dışı bırakın. |
> | Microsoft. Directory/cihazlar/etkinleştir | Azure Active Directory cihazları etkinleştirin. |
> | Microsoft. Directory/Devices/extensionAttributes/Update | Azure Active Directory içindeki Devices. extensionAttributes özelliğinin tüm değerlerini güncelleştirin. |
> | Microsoft. Directory/Signınreports/allProperties/Read | Azure Active Directory içindeki Signınreports 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |

### <a name="compliance-administrator-permissions"></a>Uyumluluk Yöneticisi izinleri

, Azure AD ve Microsoft 365 uyumluluk yapılandırma ve raporlarını okuyabilir ve yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Directory/entitlementManagement/allProperties/Read | Azure AD Yetkilendirme Yönetimi 'ndeki tüm özellikleri okuyun. |
> | Microsoft. office365. Karmaşıssuancemanager/allEntities/allTasks | Office 365 uyumluluk Yöneticisi 'nin tüm yönlerini yönetme |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="compliance-data-administrator-permissions"></a>Uyumluluk verileri Yöneticisi izinleri

Uyumluluk içeriğini oluşturur ve yönetir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory. cloudAppSecurity/allEntities/allTasks | Microsoft Cloud App Security okuyun ve yapılandırın. |
> | Microsoft. Azure. ınformationprotection/allEntities/allTasks | Azure Information Protection tüm yönlerini yönetin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. Karmaşıssuancemanager/allEntities/allTasks | Office 365 uyumluluk Yöneticisi 'nin tüm yönlerini yönetme |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="conditional-access-administrator-permissions"></a>Koşullu Erişim Yöneticisi izinleri

, Koşullu erişim yeteneklerini yönetebilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/policies/conditionalAccess/temel/okuma | Azure Active Directory içindeki policies. conditionalAccess özelliğini okuyun. |
> | Microsoft. Directory/policies/conditionalAccess/temel/güncelleştirme | Azure Active Directory policies. conditionalAccess özelliğini güncelleştirin. |
> | Microsoft. Directory/policies/Conditionalaişim/oluşturma | Azure Active Directory ilke oluşturun. |
> | Microsoft. Directory/policies/conditionalAccess/Delete | Azure Active Directory ilkeleri silin. |
> | Microsoft. Directory/policies/conditionalAccess/Owners/Read | Azure Active Directory içindeki policies. conditionalAccess özelliğini okuyun. |
> | Microsoft. Directory/policies/conditionalAccess/Owners/Update | Azure Active Directory policies. conditionalAccess özelliğini güncelleştirin. |
> | Microsoft. Directory/policies/conditionalAccess/policiesAppliedTo/Read | Azure Active Directory içindeki policies. conditionalAccess özelliğini okuyun. |
> | Microsoft. Directory/policies/conditionalAccess/tenantDefault/Update | Azure Active Directory policies. conditionalAccess özelliğini güncelleştirin. |

### <a name="customer-lockbox-access-approver-permissions"></a>Müşteri Kasası erişimi onaylayan izinleri

, Müşteri kurumsal verilerine erişmek için Microsoft destek isteklerini onaylayabilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. kasa/allEntities/allTasks | Office 365 Müşteri Kasası tüm yönlerini yönetin |

### <a name="desktop-analytics-administrator-permissions"></a>Masaüstü Analizi yönetici izinleri

, Masaüstü Analizi ve Office özelleştirmesi & Ilkesi hizmetlerini yönetebilir. Bu, masaüstü analizi için varlık envanterini görüntüleme, dağıtım planları oluşturma, dağıtım ve sistem durumunu görüntüleme imkanını içerir. Office özelleştirmesi & Ilke hizmetinde, bu rol kullanıcıların Office ilkelerini yönetmesine olanak sağlar.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. desktopAnalytics/allEntities/allTasks | Masaüstü analizinin tüm yönlerini yönetin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="directory-readers-permissions"></a>Dizin okuyucular izinleri
Temel dizin bilgilerini okuyabilir. Uygulamalara erişim vermek için, kullanıcılara yönelik değildir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Yönetimtiveunits/Basic/Read | Azure Active Directory ' de Yönetimtiveunits üzerindeki temel özellikleri okuyun. |
> | Microsoft. Directory/Yönetimtiveunits/Üyeler/Read | Azure Active Directory içindeki Yönetimtiveunits. Members özelliğini okuyun. |
> | Microsoft. Directory/uygulamalar/temel/okuma | Azure Active Directory içindeki uygulamalarda temel özellikleri okuyun. |
> | Microsoft. Directory/uygulamalar/sahipler/okuma | Azure Active Directory içindeki Applications. Owners özelliğini okuyun. |
> | Microsoft. Directory/uygulamalar/ilkeler/okuma | Azure Active Directory içindeki Applications. Policies özelliğini okuyun. |
> | Microsoft. Directory/Contacts/Basic/Read | Azure Active Directory kişilerdeki temel özellikleri okuyun. |
> | Microsoft. Directory/kiþiler/memberOf/Read | Azure Active Directory içinde Contacts. memberOf özelliğini okuyun. |
> | Microsoft. Directory/sözleşmeleri/temel/okuma | Azure Active Directory 'daki sözleşmelerdeki temel özellikleri okuyun. |
> | Microsoft. Directory/Devices/Basic/Read | Azure Active Directory cihazlarda temel özellikleri okuyun. |
> | Microsoft. Directory/Devices/memberOf/Read | Azure Active Directory içindeki Devices. memberOf özelliğini okuyun. |
> | Microsoft. Directory/Devices/kayıt bilgileri/okuma | Azure Active Directory içindeki Devices. Kaydedeteredowners özelliğini okuyun. |
> | Microsoft. Directory/Devices/registeredUsers/Read | Azure Active Directory içindeki Devices. registeredUsers özelliğini okuyun. |
> | Microsoft. Directory/directoryRoles/Basic/Read | Azure Active Directory 'de directoryRoles temel özelliklerini okuyun. |
> | Microsoft. Directory/directoryRoles/eli, Lemembers/Read | Azure Active Directory directoryRoles. Eli, Lemembers özelliğini okuyun. |
> | Microsoft. Directory/directoryRoles/Members/Read | Azure Active Directory içindeki directoryRoles. Members özelliğini okuyun. |
> | Microsoft. Directory/Domains/Basic/Read | Azure Active Directory etki alanlarında temel özellikleri okuyun. |
> | Microsoft. Directory/Groups/Approtaatamalar/Read | Azure Active Directory groups. Approtaatamalar özelliğini okuyun. |
> | Microsoft. Directory/Groups/Basic/Read | Azure Active Directory gruplardaki temel özellikleri okuyun. |
> | Microsoft. Directory/gruplar/memberOf/Read | Azure Active Directory groups. memberOf özelliğini okuyun. |
> | Microsoft. Directory/gruplar/Üyeler/okuma | Azure Active Directory gruplar. Members özelliğini okuyun. |
> | Microsoft. Directory/gruplar/Owners/Read | Azure Active Directory içindeki groups. Owners özelliğini okuyun. |
> | Microsoft. Directory/Groups/Settings/Read | Azure Active Directory içindeki groups. Settings özelliğini okuyun. |
> | Microsoft. Directory/groupSettings/Basic/Read | Azure Active Directory groupSettings üzerindeki temel özellikleri okuyun. |
> | Microsoft. Directory/groupSettingTemplates/Basic/Read | Azure Active Directory groupSettingTemplates üzerindeki temel özellikleri okuyun. |
> | Microsoft. Directory/oAuth2PermissionGrants/Basic/Read | Azure Active Directory içindeki temel özellikleri okuyun oAuth2PermissionGrants. |
> | Microsoft. Directory/kuruluş/temel/okuma | Azure Active Directory içindeki kuruluştaki temel özellikleri okuyun. |
> | Microsoft. Directory/Organization/trustedCAsForPasswordlessAuth/okuma | Azure Active Directory içinde Organization. trustedCAsForPasswordlessAuth özelliğini okuyun. |
> | Microsoft. Directory/Roleatamalar/temel/okuma | Azure Active Directory içindeki Roleatamalarındaki temel özellikleri okuyun. |
> | Microsoft. Directory/roleDefinitions/temel/okuma | Azure Active Directory içindeki roleDefinitions ' daki temel özellikleri okuyun. |
> | Microsoft. Directory/Servicesorumlularını/appRoleAssignedTo/Read | Azure Active Directory Servicesorumlularını. appRoleAssignedTo özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/Approtaatamalar/okuma | Azure Active Directory Servicesorumlularını. Approtaatamalar özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlular/Basic/Read | Azure Active Directory içindeki Servicesorumlularını temel özelliklerini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/memberOf/Read | Azure Active Directory Servicesorumlularını. memberOf özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/oAuth2PermissionGrants/Basic/Read | Azure Active Directory Servicesorumlularını. oAuth2PermissionGrants özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/ownedObjects/Read | Azure Active Directory Servicesorumlularını. ownedObjects özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/Owners/Read | Azure Active Directory içindeki Servicesorumlularını. Owners özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/ilkeleri/okuma | Azure Active Directory içindeki Servicesorumlularını. Policies özelliğini okuyun. |
> | Microsoft. Directory/subscribedSkus/Basic/Read | Azure Active Directory içindeki temel özellikleri okuyun subscribedSkus. |
> | Microsoft. Directory/Users/Approtaatamalar/okuma | Azure Active Directory içindeki Users. Approtaatamalar özelliğini okuyun. |
> | Microsoft. Directory/Users/Basic/Read | Azure Active Directory kullanıcıların temel özelliklerini okuyun. |
> | Microsoft. Directory/Users/directReports/Read | Azure Active Directory Users. directReports özelliğini okuyun. |
> | Microsoft. Directory/Users/Manager/okuma | Azure Active Directory içindeki Users. Manager özelliğini okuyun. |
> | Microsoft. Directory/kullanıcılar/memberOf/Read | Azure Active Directory içindeki Users. memberOf özelliğini okuyun. |
> | Microsoft. Directory/Users/oAuth2PermissionGrants/Basic/Read | Azure Active Directory Users. oAuth2PermissionGrants özelliğini okuyun. |
> | Microsoft. Directory/Users/ownedDevices/Read | Azure Active Directory Users. ownedDevices özelliğini okuyun. |
> | Microsoft. Directory/Users/ownedObjects/Read | Azure Active Directory Users. ownedObjects özelliğini okuyun. |
> | Microsoft. Directory/Users/registeredDevices/Read | Azure Active Directory Users. registeredDevices özelliğini okuyun. |

### <a name="directory-synchronization-accounts-permissions"></a>Dizin eşitleme hesapları izinleri

Yalnızca Azure AD Connect hizmeti tarafından kullanılır.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/kuruluş/dirSync/Update | Azure Active Directory içinde Organization. dirSync özelliğini güncelleştirin. |
> | Microsoft. Directory/policies/Create | Azure Active Directory ilke oluşturun. |
> | Microsoft. Directory/policies/Delete | Azure Active Directory ilkeleri silin. |
> | Microsoft. Directory/policies/Basic/Read | Azure Active Directory ilke üzerindeki temel özellikleri okuyun. |
> | Microsoft. Directory/policies/Basic/Update | Azure Active Directory ilkelerde temel özellikleri güncelleştirin. |
> | Microsoft. Directory/policies/Owners/Read | Azure Active Directory içindeki policies. Owners özelliğini okuyun. |
> | Microsoft. Directory/policies/Owners/Update | Azure Active Directory içindeki policies. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/policies/policiesAppliedTo/Read | Azure Active Directory içindeki policies. policiesAppliedTo özelliğini okuyun. |
> | Microsoft. Directory/policies/tenantDefault/Update | Azure Active Directory ilkeleri. tenantDefault özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/appRoleAssignedTo/Read | Azure Active Directory Servicesorumlularını. appRoleAssignedTo özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/appRoleAssignedTo/Update | Azure Active Directory Servicesorumlularını. appRoleAssignedTo özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/Approtaatamalar/okuma | Azure Active Directory Servicesorumlularını. Approtaatamalar özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/Approtaatamalar/güncelleştirme | Azure Active Directory Servicesorumlularını. Approtaatamalar özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/hedef kitle/güncelleştirme | Azure Active Directory Servicesorumlularını. Audience özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/kimlik doğrulaması/güncelleştirme | Azure Active Directory Servicesorumlularını. Authentication özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlular/Basic/Read | Azure Active Directory içindeki Servicesorumlularını temel özelliklerini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/temel/güncelleştirme | Azure Active Directory içindeki Servicesorumlularını temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/oluşturma | Azure Active Directory 'de Servicesorumlularını oluşturun. |
> | Microsoft. Directory/Servicesorumlularını/kimlik bilgileri/güncelleştirme | Azure Active Directory Servicesorumlularını. Credentials özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/memberOf/Read | Azure Active Directory Servicesorumlularını. memberOf özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/oAuth2PermissionGrants/Basic/Read | Azure Active Directory Servicesorumlularını. oAuth2PermissionGrants özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/Owners/Read | Azure Active Directory içindeki Servicesorumlularını. Owners özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/Owners/Update | Azure Active Directory Servicesorumlularını. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/ownedObjects/Read | Azure Active Directory Servicesorumlularını. ownedObjects özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/izinleri/güncelleştirme | Azure Active Directory Servicesorumlularını. Permissions özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/ilkeleri/okuma | Azure Active Directory içindeki Servicesorumlularını. Policies özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/ilkeleri/güncelleştirme | Azure Active Directory Servicesorumlularını. Policies özelliğini güncelleştirin. |
> | Microsoft. directorySync/Allentitıes/allTasks | Azure AD Connect tüm eylemleri gerçekleştirin. |

### <a name="directory-writers-permissions"></a>Dizin yazıcılar izinleri

Temel dizin bilgilerini okuyabilir & yazabilir. Uygulamalara erişim vermek için, kullanıcılara yönelik değildir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Groups/Approtaatamalar/güncelleştirme | Azure Active Directory groups. Approtaatamalar özelliğini güncelleştirin. |
> | Microsoft. Directory/Groups/Atamalisansı | Azure Active Directory gruplardaki lisansları yönetin. |
> | Microsoft. Directory/Groups/Basic/Update | Azure Active Directory gruplardaki temel özellikleri güncelleştirin. |
> | Microsoft. Directory/gruplar/sınıflandırma/güncelleştirme | Azure Active Directory içindeki grubun sınıflandırma özelliğini güncelleştirin. |
> | Microsoft. Directory/gruplar/oluştur | Azure Active Directory grupları oluşturun. |
> | Microsoft. Directory/gruplar/groupType/Update | Azure Active Directory bir grubun groupType özelliğini güncelleştirin. |
> | Microsoft. Directory/gruplar/Üyeler/güncelleştirme | Azure Active Directory groups. Members özelliğini güncelleştirin. |
> | Microsoft. Directory/gruplar/Owners/Update | Azure Active Directory groups. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/Groups/Reprocesslicenseatama | Azure Active Directory bir grup için lisans atamalarını yeniden işleyin. |
> | Microsoft. Directory/Groups/securityEnabled/Update | Azure Active Directory bir grubun Secuyenabled özelliğini güncelleştirin. |
> | Microsoft. Directory/Groups/Settings/Update | Azure Active Directory groups. Settings özelliğini güncelleştirin. |
> | Microsoft. Directory/gruplar/görünürlük/güncelleştirme | Grubun görünürlük özelliğini güncelleştirme |
> | Microsoft. Directory/groupSettings/Basic/Update | Azure Active Directory içindeki groupSettings üzerindeki temel özellikleri güncelleştirin. |
> | Microsoft. Directory/groupSettings/Create | Azure Active Directory groupSettings oluşturma.. |
> | Microsoft. Directory/groupSettings/Delete | Azure Active Directory groupSettings 'i silin. |
> | Microsoft. Directory/oAuth2PermissionGrants/Basic/Update | Azure Active Directory 'de oAuth2PermissionGrants 'in temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/oAuth2PermissionGrants/Create | Azure Active Directory 'de oAuth2PermissionGrants oluşturun. |
> | Microsoft. Directory/Servicesorumlularını/synchronizationCredentials/Manage | Uygulama sağlama gizli dizilerini ve kimlik bilgilerini yönetin. |
> | Microsoft. Directory/Servicesorumlularını/synchronizationJobs/Manage | Uygulama sağlama eşitleme işlerini başlatın, yeniden başlatın ve duraklatın. |
> | Microsoft. Directory/Servicesorumlularını/synchronizationSchema/Yönet | Uygulama sağlama eşitleme işleri ve şeması oluşturun ve yönetin. |
> | Microsoft. Directory/Users/Approtaatamalar/güncelleştirme | Azure Active Directory Users. Approtaatamalar özelliğini güncelleştirin. |
> | Microsoft. Directory/Users/Atamalisansı | Azure Active Directory kullanıcılar üzerinde lisansları yönetin. |
> | Microsoft. Directory/Users/Basic/Update | Azure Active Directory kullanıcıların temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/kullanıcılar/oluştur | Azure Active Directory’de kullanıcılar oluşturun. |
> | Microsoft. Directory/kullanıcılar/devre dışı bırak | Azure Active Directory bir kullanıcı hesabını devre dışı bırakın. |
> | Microsoft. Directory/kullanıcılar/etkinleştir | Azure Active Directory Kullanıcı hesabını etkinleştirme |
> | Microsoft. Directory/Users/ınvalidateallrefreshtokens | Kullanıcıların bir sonraki oturum açma sırasında yeniden kimlik doğrulaması yapmasını gerektiren Azure Active Directory tüm Kullanıcı yenileme belirteçlerini geçersiz kılar |
> | Microsoft. Directory/Users/Manager/Update | Azure Active Directory içindeki Users. Manager özelliğini güncelleştirin. |
> | Microsoft. Directory/Users/Reprocesslicenseatama | Azure Active Directory bir kullanıcı için lisans atamalarını yeniden işleyin. |
> | Microsoft. Directory/Users/userPrincipalName/Update | Azure Active Directory içindeki Users. userPrincipalName özelliğini güncelleştirin. |

### <a name="domain-name-administrator-permissions"></a>Etki alanı adı yönetici izinleri

, Bulutta ve şirket içinde etki alanı adlarını yönetebilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Domains/allProperties/allTasks | Etki alanlarını oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="dynamics-365-administrator-permissions"></a>Dynamics 365 yönetici izinleri

, Dynamics 365 ürününün tüm yönlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. powerApps. dynamics365/allEntities/allTasks | Dynamics 365 'in tüm yönlerini yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="exchange-administrator-permissions"></a>Exchange yönetici izinleri

, Exchange ürününün tüm yönlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Directory/Groups/hiddenMembers/Read | Bir grubun gizli üyelerini oku |
> | Microsoft. Directory/groups. Unified/Basic/Update | Microsoft 365 gruplarının temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/groups. Unified/Create | Microsoft 365 grupları oluşturun. |
> | Microsoft. Directory/groups. Unified/Delete | Microsoft 365 grupları silin. |
> | Microsoft. Directory/groups. Unified/restore | Microsoft 365 gruplarını geri yükleme |
> | Microsoft. Directory/groups. Unified/Members/Update | Microsoft 365 gruplarının üyeliğini güncelleştirin. |
> | Microsoft. Directory/groups. Unified/Owners/Update | Microsoft 365 gruplarının sahipliğini güncelleştirin. |
> | Microsoft. office365. Exchange/allEntities/allTasks | Exchange Online 'ın tüm yönlerini yönetin. |
> | Microsoft. office365. Network/Performance/allProperties/Read | Microsoft 365 Yönetim Merkezi 'nde ağ performansı sayfalarını okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. usageReports/allEntities/allProperties/Read | Office 365 kullanım raporlarını okuyun. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="external-id-user-flow-administrator-permissions"></a>Dış KIMLIK Kullanıcı akış Yöneticisi izinleri

Kullanıcı akışlarının tüm yönlerini oluşturun ve yönetin.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. AAD. B2C/Userakışlar/allTasks | Azure Active Directory B2C Kullanıcı akışlarını okuyun ve yapılandırın. |

### <a name="external-id-user-flow-attribute-administrator-permissions"></a>Dış KIMLIK Kullanıcı akışı öznitelik Yöneticisi izinleri

Tüm Kullanıcı akışları için kullanılabilen öznitelik şemasını oluşturun ve yönetin.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. AAD. B2C/userAttributes/allTasks | Azure Active Directory B2C Kullanıcı özniteliklerini okuyun ve yapılandırın. |

### <a name="external-identity-provider-administrator-permissions"></a>Dış kimlik sağlayıcısı yönetici izinleri

Kimlik sağlayıcılarını doğrudan federasyonda kullanılmak üzere yapılandırın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. AAD. B2C/IdentityProviders/allTasks | Azure Active Directory B2C kimlik sağlayıcılarını okuyun ve yapılandırın. |

### <a name="global-administrator-permissions"></a>Genel yönetici izinleri

, Azure AD kimliklerini kullanan Azure AD ve Microsoft hizmetlerinin tüm yönlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. AAD. cloudAppSecurity/allEntities/allTasks | Tüm kaynakları oluşturup silin ve Microsoft. AAD. cloudAppSecurity içindeki standart özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Yönetimtiveunits/allProperties/allTasks | Yönettiveunits oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/uygulamalar/allProperties/allTasks | Uygulamalar oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Approtaatamalar/allProperties/allTasks | Approtaatamalar oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/auditLogs/allProperties/Read | Azure Active Directory içindeki auditLogs 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Directory/bitlockerKeys/Key/Read | Azure Active Directory içindeki BitLocker anahtar nesnelerini ve özelliklerini (kurtarma anahtarı dahil) okuyun. |
> | Microsoft. Directory/Contacts/allProperties/allTasks | Kişiler oluşturup silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/sözleşmeleri/allProperties/allTasks | Sözleşmeleri oluşturup silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Devices/allProperties/allTasks | Cihazları oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/directoryRoles/allProperties/allTasks | DirectoryRoles oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/directoryRoleTemplates/allProperties/allTasks | DirectoryRoleTemplates oluşturup silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Domains/allProperties/allTasks | Etki alanlarını oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/entitlementManagement/allProperties/allTasks | Kaynakları oluşturup silin ve Azure AD Yetkilendirme Yönetimi 'nde tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Groups/allProperties/allTasks | Grupları oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Groupsastifbletoroles/allProperties/Update | Azure Active Directory, ısastifbletorole özelliği true olarak ayarlanan grupları güncelleştirin. |
> | Microsoft. Directory/Groupsastifbletoroles/Create | Azure Active Directory ' de ısastifbletorole özelliği true olarak ayarlanan gruplar oluşturun. |
> | Microsoft. Directory/Groupsastifbletoroles/Delete | Iastifbletorole özelliği Azure Active Directory olan grupları silin. |
> | Microsoft. Directory/groupSettings/allProperties/allTasks | GroupSettings oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/groupSettingTemplates/allProperties/allTasks | GroupSettingTemplates oluşturup silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Logınantmarkalaması/allProperties/allTasks | Logınantmarkalaması oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/oAuth2PermissionGrants/allProperties/allTasks | OAuth2PermissionGrants oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/kuruluş/allProperties/allTasks | Kuruluş oluşturup silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/policies/allProperties/allTasks | İlkeleri oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/provisioningLogs/allProperties/Read | Sağlama günlüklerinin tüm özelliklerini okuyun. |
> | Microsoft. Directory/Roleatamaları/allProperties/allTasks | Roleatamalar oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/roleDefinitions/allProperties/allTasks | RoleDefinitions oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Scopedrolemembersevk/allProperties/allTasks | Scopedrolemembersevk eden oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/serviceAction/activateService | Azure Active Directory 'de Activateservice hizmeti eylemini gerçekleştirebilir |
> | Microsoft. Directory/serviceAction/disableDirectoryFeature | Azure Active Directory 'da Disabledirectoryfeature hizmet eylemini gerçekleştirebilir |
> | Microsoft. Directory/serviceAction/enableDirectoryFeature | Azure Active Directory 'da Enabledirectoryfeature hizmeti eylemini gerçekleştirebilir |
> | Microsoft. Directory/serviceAction/getAvailableExtentionProperties | Azure Active Directory içinde Getavailableextentionproperties hizmeti eylemini gerçekleştirebilir |
> | Microsoft. Directory/Servicesorumlularını/allProperties/allTasks | Servicesorumlularını oluşturup silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Signınreports/allProperties/Read | Azure Active Directory içindeki Signınreports 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Directory/subscribedSkus/allProperties/allTasks | SubscribedSkus oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Users/allProperties/allTasks | Kullanıcı oluşturun ve silin ve Azure Active Directory tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. directorySync/Allentitıes/allTasks | Azure AD Connect tüm eylemleri gerçekleştirin. |
> | Microsoft. AAD. ıdentityprotection/allEntities/allTasks | Tüm kaynakları oluşturup silin ve Microsoft. AAD. ıdentityprotection 'da standart özellikleri okuyun ve güncelleştirin. |
> | Microsoft. AAD. Privilegedıdentitymanagement/allEntities/okuma | Microsoft. AAD. Privilegedıdentitymanagement içindeki tüm kaynakları okuyun. |
> | Microsoft. Azure. advancedThreatProtection/Allentitıes/Read | Microsoft. Azure. advancedThreatProtection içindeki tüm kaynakları okuyun. |
> | Microsoft. Azure. ınformationprotection/allEntities/allTasks | Azure Information Protection tüm yönlerini yönetin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Commerce. faturalandırma/allEntities/allTasks | Faturalandırma 'nin tüm yönlerini yönetin. |
> | Microsoft. Intune/allEntities/allTasks | Intune 'un tüm yönlerini yönetin. |
> | Microsoft. office365. Karmaşıssuancemanager/allEntities/allTasks | Office 365 uyumluluk Yöneticisi 'nin tüm yönlerini yönetme |
> | Microsoft. office365. desktopAnalytics/allEntities/allTasks | Masaüstü analizinin tüm yönlerini yönetin. |
> | Microsoft. office365. Exchange/allEntities/allTasks | Exchange Online 'ın tüm yönlerini yönetin. |
> | Microsoft. office365. kasa/allEntities/allTasks | Office 365 Müşteri Kasası tüm yönlerini yönetin |
> | Microsoft. office365. messageCenter/messages/okundu | Microsoft. office365. messageCenter içindeki iletileri okuyun. |
> | Microsoft. office365. messageCenter/securityMessages/Read | Microsoft. office365. messageCenter içindeki securityMessages 'i okuyun. |
> | Microsoft. office365. protectionCenter/allEntities/allTasks | Office 365 Koruma Merkezi 'nin tüm yönlerini yönetin. |
> | Microsoft. office365. Securityzorluk Ancecenter/allEntities/allTasks | Tüm kaynakları oluşturup silin ve Microsoft. office365. Securitykarmaşıkancecenter içindeki standart özellikleri okuyun ve güncelleştirin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. SharePoint/allEntities/allTasks | Tüm kaynakları oluşturup silin ve Microsoft. office365. SharePoint içindeki standart özellikleri okuyun ve güncelleştirin. |
> | Microsoft. office365. Sktypeınfo/allEntities/allTasks | Skype Kurumsal Çevrimiçi 'nin tüm yönlerini yönetin. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. usageReports/allEntities/okuma | Office 365 kullanım raporlarını okuyun. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. powerApps. dynamics365/allEntities/allTasks | Dynamics 365 'in tüm yönlerini yönetin. |
> | Microsoft. powerApps. PowerBI/allEntities/allTasks | Power BI tüm yönlerini yönetin. |
> | Microsoft. Windows. Savunderadvancedthreatprotection/Allentitıes/Read | Microsoft. Windows. savunma Deradvancedthreatprotection içindeki tüm kaynakları okuyun. |

### <a name="global-reader-permissions"></a>Genel okuyucu izinleri

Genel yöneticinin yapabileceği her şeyi okuyabilir, ancak düzenleyemez.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki [rol açıklaması](#global-reader) konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Commerce. faturalandırma/allEntities/okuma | Faturalandırma 'nin tüm yönlerini okuyun. |
> | Microsoft. Directory/Yönetimtiveunits/Basic/Read | Azure Active Directory ' de Yönetimtiveunits üzerindeki temel özellikleri okuyun. |
> | Microsoft. Directory/Yönetimtiveunits/Üyeler/Read | Azure Active Directory içindeki Yönetimtiveunits. Members özelliğini okuyun. |
> | Microsoft. Directory/uygulamalar/temel/okuma | Azure Active Directory içindeki uygulamalarda temel özellikleri okuyun. |
> | Microsoft. Directory/uygulamalar/sahipler/okuma | Azure Active Directory içindeki Applications. Owners özelliğini okuyun. |
> | Microsoft. Directory/uygulamalar/ilkeler/okuma | Azure Active Directory içindeki Applications. Policies özelliğini okuyun. |
> | Microsoft. Directory/bitlockerKeys/Key/Read | Azure Active Directory içindeki BitLocker anahtar nesnelerini ve özelliklerini (kurtarma anahtarı dahil) okuyun. |
> | Microsoft. Directory/Contacts/Basic/Read | Azure Active Directory kişilerdeki temel özellikleri okuyun. |
> | Microsoft. Directory/kiþiler/memberOf/Read | Azure Active Directory içinde Contacts. memberOf özelliğini okuyun. |
> | Microsoft. Directory/sözleşmeleri/temel/okuma | Azure Active Directory 'daki sözleşmelerdeki temel özellikleri okuyun. |
> | Microsoft. Directory/Devices/Basic/Read | Azure Active Directory cihazlarda temel özellikleri okuyun. |
> | Microsoft. Directory/Devices/memberOf/Read | Azure Active Directory içindeki Devices. memberOf özelliğini okuyun. |
> | Microsoft. Directory/Devices/kayıt bilgileri/okuma | Azure Active Directory içindeki Devices. Kaydedeteredowners özelliğini okuyun. |
> | Microsoft. Directory/Devices/registeredUsers/Read | Azure Active Directory içindeki Devices. registeredUsers özelliğini okuyun. |
> | Microsoft. Directory/directoryRoles/Basic/Read | Azure Active Directory 'de directoryRoles temel özelliklerini okuyun. |
> | Microsoft. Directory/directoryRoles/eli, Lemembers/Read | Azure Active Directory directoryRoles. Eli, Lemembers özelliğini okuyun. |
> | Microsoft. Directory/directoryRoles/Members/Read | Azure Active Directory içindeki directoryRoles. Members özelliğini okuyun. |
> | Microsoft. Directory/Domains/Basic/Read | Azure Active Directory etki alanlarında temel özellikleri okuyun. |
> | Microsoft. Directory/entitlementManagement/allProperties/Read | Azure AD Yetkilendirme Yönetimi 'ndeki tüm özellikleri okuyun. |
> | Microsoft. Directory/Groups/Approtaatamalar/Read | Azure Active Directory groups. Approtaatamalar özelliğini okuyun. |
> | Microsoft. Directory/Groups/Basic/Read | Azure Active Directory gruplardaki temel özellikleri okuyun. |
> | Microsoft. Directory/Groups/hiddenMembers/Read | Azure Active Directory gruplar. hiddenMembers özelliğini okuyun. |
> | Microsoft. Directory/gruplar/memberOf/Read | Azure Active Directory groups. memberOf özelliğini okuyun. |
> | Microsoft. Directory/gruplar/Üyeler/okuma | Azure Active Directory gruplar. Members özelliğini okuyun. |
> | Microsoft. Directory/gruplar/Owners/Read | Azure Active Directory içindeki groups. Owners özelliğini okuyun. |
> | Microsoft. Directory/Groups/Settings/Read | Azure Active Directory içindeki groups. Settings özelliğini okuyun. |
> | Microsoft. Directory/groupSettings/Basic/Read | Azure Active Directory groupSettings üzerindeki temel özellikleri okuyun. |
> | Microsoft. Directory/groupSettingTemplates/Basic/Read | Azure Active Directory groupSettingTemplates üzerindeki temel özellikleri okuyun. |
> | Microsoft. Directory/oAuth2PermissionGrants/Basic/Read | Azure Active Directory içindeki temel özellikleri okuyun oAuth2PermissionGrants. |
> | Microsoft. Directory/kuruluş/temel/okuma | Azure Active Directory içindeki kuruluştaki temel özellikleri okuyun. |
> | Microsoft. Directory/Organization/trustedCAsForPasswordlessAuth/okuma | Azure Active Directory içinde Organization. trustedCAsForPasswordlessAuth özelliğini okuyun. |
> | Microsoft. Directory/policies/standart/Read | Azure Active Directory içindeki standart ilkeleri okuyun. |
> | Microsoft. Directory/provisioningLogs/allProperties/Read | Sağlama günlüklerinin tüm özelliklerini okuyun. |
> | Microsoft. Directory/Roleatamalar/temel/okuma | Azure Active Directory içindeki Roleatamalarındaki temel özellikleri okuyun. |
> | Microsoft. Directory/roleDefinitions/temel/okuma | Azure Active Directory içindeki roleDefinitions ' daki temel özellikleri okuyun. |
> | Microsoft. Directory/Servicesorumlularını/appRoleAssignedTo/Read | Azure Active Directory Servicesorumlularını. appRoleAssignedTo özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/Approtaatamalar/okuma | Azure Active Directory Servicesorumlularını. Approtaatamalar özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlular/Basic/Read | Azure Active Directory içindeki Servicesorumlularını temel özelliklerini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/memberOf/Read | Azure Active Directory Servicesorumlularını. memberOf özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/oAuth2PermissionGrants/Basic/Read | Azure Active Directory Servicesorumlularını. oAuth2PermissionGrants özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/ownedObjects/Read | Azure Active Directory Servicesorumlularını. ownedObjects özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/Owners/Read | Azure Active Directory içindeki Servicesorumlularını. Owners özelliğini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/ilkeleri/okuma | Azure Active Directory içindeki Servicesorumlularını. Policies özelliğini okuyun. |
> | Microsoft. Directory/Signınreports/allProperties/Read | Azure Active Directory içindeki Signınreports 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Directory/subscribedSkus/Basic/Read | Azure Active Directory içindeki temel özellikleri okuyun subscribedSkus. |
> | Microsoft. Directory/Users/Approtaatamalar/okuma | Azure Active Directory içindeki Users. Approtaatamalar özelliğini okuyun. |
> | Microsoft. Directory/Users/Basic/Read | Azure Active Directory kullanıcıların temel özelliklerini okuyun. |
> | Microsoft. Directory/Users/directReports/Read | Azure Active Directory Users. directReports özelliğini okuyun. |
> | Microsoft. Directory/Users/Manager/okuma | Azure Active Directory içindeki Users. Manager özelliğini okuyun. |
> | Microsoft. Directory/kullanıcılar/memberOf/Read | Azure Active Directory içindeki Users. memberOf özelliğini okuyun. |
> | Microsoft. Directory/Users/oAuth2PermissionGrants/Basic/Read | Azure Active Directory Users. oAuth2PermissionGrants özelliğini okuyun. |
> | Microsoft. Directory/Users/ownedDevices/Read | Azure Active Directory Users. ownedDevices özelliğini okuyun. |
> | Microsoft. Directory/Users/ownedObjects/Read | Azure Active Directory Users. ownedObjects özelliğini okuyun. |
> | Microsoft. Directory/Users/registeredDevices/Read | Azure Active Directory Users. registeredDevices özelliğini okuyun. |
> | Microsoft. Directory/Users/strongAuthentication/Read | MFA kimlik bilgileri gibi güçlü kimlik doğrulama özelliklerini okuyun. |
> | Microsoft. office365. Exchange/allEntities/okuma | Exchange Online 'ın tüm yönlerini okuyun. |
> | Microsoft. office365. messageCenter/messages/okundu | Microsoft. office365. messageCenter içindeki iletileri okuyun. |
> | Microsoft. office365. messageCenter/securityMessages/Read | Microsoft. office365. messageCenter içindeki securityMessages 'i okuyun. |
> | Microsoft. office365. Network/Performance/allProperties/Read | Microsoft 365 Yönetim Merkezi 'nde ağ performansı sayfalarını okuyun. |
> | Microsoft. office365. protectionCenter/allEntities/okuma | Office 365 Koruma Merkezi 'nin tüm yönlerini okuyun. |
> | Microsoft. office365. Securityzorluk Ancecenter/allEntities/Read | Microsoft. office365. Securitykarmaşıkancecenter içindeki tüm standart özellikleri okuyun. |
> | Microsoft. office365. usageReports/allEntities/okuma | Office 365 kullanım raporlarını okuyun. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda standart özellikleri okuyun. |

### <a name="groups-administrator-permissions"></a>Gruplar yönetici izinleri

, Adlandırma ve süre sonu ilkeleri gibi grupların ve Grup ayarlarının tüm yönlerini yönetebilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Groups/Basic/Read | Azure Active Directory gruplardaki gruplar üzerinde standart özellikleri okuyun.  |
> | Microsoft. Directory/Groups/Basic/Update | Azure Active Directory gruplardaki temel özellikleri güncelleştirin. |
> | Microsoft. Directory/gruplar/oluştur | Azure Active Directory grupları oluşturun. |
> | Microsoft. Directory/Groups/createAsOwner | Azure Active Directory grupları oluşturun. Oluşturucu ilk sahip olarak eklenir ve oluşturulan nesne, oluşturucunun 250 tarafından oluşturulan nesneler kotasına göre sayılır. |
> | Microsoft. Directory/gruplar/Sil | Azure Active Directory grupları silin. |
> | Microsoft. Directory/Groups/hiddenMembers/Read | Azure Active Directory gruplar. hiddenMembers özelliğini okuyun. |
> | Microsoft. Directory/gruplar/Üyeler/güncelleştirme | Azure Active Directory groups. Members özelliğini güncelleştirin. |
> | Microsoft. Directory/gruplar/Owners/Update | Azure Active Directory groups. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/Groups/restore | Azure Active Directory grupları geri yükleyin. |
> | Microsoft. Directory/Groups/Settings/Update | Azure Active Directory groups. Settings özelliğini güncelleştirin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. messageCenter/messages/okundu | Microsoft. office365. messageCenter içindeki iletileri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="guest-inviter-permissions"></a>Konuk Davetleyici izinleri

' Üyeler konukları davet edebilir ' ayarından bağımsız olarak Konuk kullanıcıları davet edebilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Users/Approtaatamalar/okuma | Azure Active Directory içindeki Users. Approtaatamalar özelliğini okuyun. |
> | Microsoft. Directory/Users/Basic/Read | Azure Active Directory kullanıcıların temel özelliklerini okuyun. |
> | Microsoft. Directory/Users/directReports/Read | Azure Active Directory Users. directReports özelliğini okuyun. |
> | Microsoft. Directory/Users/Davetteguest | Azure Active Directory Konuk kullanıcıları davet edin. |
> | Microsoft. Directory/Users/Manager/okuma | Azure Active Directory içindeki Users. Manager özelliğini okuyun. |
> | Microsoft. Directory/kullanıcılar/memberOf/Read | Azure Active Directory içindeki Users. memberOf özelliğini okuyun. |
> | Microsoft. Directory/Users/oAuth2PermissionGrants/Basic/Read | Azure Active Directory Users. oAuth2PermissionGrants özelliğini okuyun. |
> | Microsoft. Directory/Users/ownedDevices/Read | Azure Active Directory Users. ownedDevices özelliğini okuyun. |
> | Microsoft. Directory/Users/ownedObjects/Read | Azure Active Directory Users. ownedObjects özelliğini okuyun. |
> | Microsoft. Directory/Users/registeredDevices/Read | Azure Active Directory Users. registeredDevices özelliğini okuyun. |

### <a name="helpdesk-administrator-permissions"></a>Yardım Masası yönetici izinleri

, Yönetici olmayanlar ve Yardım Masası yöneticileri için parolaları sıfırlayabilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Devices/bitLockerRecoveryKeys/Read | Azure Active Directory içindeki Devices. bitLockerRecoveryKeys özelliğini okuyun. |
> | Microsoft. Directory/Users/ınvalidateallrefreshtokens | Azure Active Directory tüm Kullanıcı yenileme belirteçlerini geçersiz kılar. |
> | Microsoft. Directory/Users/Password/Update | Azure Active Directory tüm kullanıcılar için parolaları güncelleştirin. Daha ayrıntılı bilgi için çevrimiçi belgelere bakın. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="hybrid-identity-administrator-permissions"></a>Karma kimlik Yöneticisi izinleri

AD 'yi Azure AD bulut sağlamasını ve Federasyon ayarlarını yönetebilir. 

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Directory/uygulamalar/hedef kitle/güncelleştirme | Azure Active Directory 'da Applications. Audience özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/kimlik doğrulaması/güncelleştirme | Azure Active Directory güncelleştirme uygulamaları. kimlik doğrulama özelliği. |
> | Microsoft. Directory/uygulamalar/temel/güncelleştirme | Azure Active Directory içindeki uygulamalarda temel özellikleri güncelleştirin. |
> | Microsoft. Directory/uygulamalar/oluşturma | Azure Active Directory uygulamalar oluşturun. |
> | Microsoft. Directory/uygulamalar/kimlik bilgileri/güncelleştirme | Azure Active Directory içindeki Applications. Credentials özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/Sil | Azure Active Directory uygulamaları silin. |
> | Microsoft. Directory/uygulamalar/sahipler/güncelleştirme | Azure Active Directory içindeki Applications. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/izinler/güncelleştirme | Azure Active Directory 'da Applications. Permissions özelliğini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/ilkeler/güncelleştirme | Azure Active Directory içindeki Applications. Policies özelliğini güncelleştirin. |
> | Microsoft. Directory/applicationTemplates/örnek oluştur | Uygulama şablonlarından Galeri uygulamaları oluşturun. |
> | Microsoft. Directory/auditLogs/allProperties/Read | Azure Active Directory içindeki auditLogs 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Directory/Cloudsağlamasını/allProperties/allTasks | Azure AD bulut sağlama hizmeti 'nin tüm özelliklerini okuyun ve yapılandırın. |
> | Microsoft. Directory/Domains/allProperties/Read | Etki alanlarının tüm özelliklerini okuyun. |
> | Microsoft. Directory/Domains/Federation/Update | Etki alanlarının Federasyon özelliğini güncelleştirin. |
> | Microsoft. Directory/kuruluş/dirSync/Update | Azure Active Directory içinde Organization. dirSync özelliğini güncelleştirin. |
> | Microsoft. Directory/provisioningLogs/allProperties/Read | Sağlama günlüklerinin tüm özelliklerini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/hedef kitle/güncelleştirme | Azure Active Directory Servicesorumlularını. Audience özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/kimlik doğrulaması/güncelleştirme | Azure Active Directory Servicesorumlularını. Authentication özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/temel/güncelleştirme | Azure Active Directory içindeki Servicesorumlularını temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/oluşturma | Azure Active Directory 'de Servicesorumlularını oluşturun. |
> | Microsoft. Directory/Servicesorumlularını/kimlik bilgileri/güncelleştirme | Azure Active Directory Servicesorumlularını. Credentials özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/silme | Azure Active Directory Servicesorumlularını silin. |
> | Microsoft. Directory/Servicesorumlularını/Owners/Update | Azure Active Directory Servicesorumlularını. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/izinleri/güncelleştirme | Azure Active Directory Servicesorumlularını. Permissions özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/ilkeleri/güncelleştirme | Azure Active Directory Servicesorumlularını. Policies özelliğini güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/synchronizationJobs/Manage | Azure AD 'de eşitleme işlerinin tüm yönlerini yönetin. |
> | Microsoft. Directory/Servicesorumlularını/synchronizationSchema/Yönet | Azure AD 'de eşitleme şemasının tüm yönlerini yönetin. |
> | Microsoft. Directory/Servicesorumlularını/synchronizationCredentials/Manage | Azure AD 'de eşitleme kimlik bilgilerinin tüm yönlerini yönetin. |
> | Microsoft. Directory/Servicesorumlularını/etiketi/güncelleştirme | Azure Active Directory Servicesorumlularını. Tag özelliğini güncelleştirin. |
> | Microsoft. Directory/Signınreports/allProperties/Read | Azure Active Directory içindeki Signınreports 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. office365. messageCenter/messages/okundu | Microsoft. office365. messageCenter içindeki iletileri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="insights-administrator-permissions"></a>Öngörüler yönetici izinleri

Microsoft 365 Insights uygulamasında yönetim erişimine sahiptir. 

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Insights/allEntities/allTasks | Öngörülerin tüm yönlerini yönetin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="insights-business-leader-permissions"></a>Öngörüler Iş lideri izinleri

, M365 Insights uygulaması aracılığıyla panoları ve öngörüleri görüntüleyebilir ve paylaşabilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Insights/raporlar/okuma | Öngörüler uygulamasındaki raporları ve panoları görüntüleyin. |
> | Microsoft. Insights/programlar/güncelleştirme | Insights uygulamasındaki programları dağıtın ve yönetin. |

### <a name="intune-administrator-permissions"></a>Intune yönetici izinleri

, Intune ürününün tüm yönlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/bitlockerKeys/Key/Read | Azure Active Directory içindeki BitLocker anahtar nesnelerini ve özelliklerini (kurtarma anahtarı dahil) okuyun. |
> | Microsoft. Directory/Contacts/Basic/Update | Azure Active Directory kişilerdeki temel özellikleri güncelleştirin. |
> | Microsoft. Directory/kişiler/oluştur | Azure Active Directory kişiler oluşturun. |
> | Microsoft. Directory/Contacts/Delete | Azure Active Directory kişileri silin. |
> | Microsoft. Directory/Devices/Basic/Update | Azure Active Directory cihazlarda temel özellikleri güncelleştirin. |
> | Microsoft. Directory/cihazlar/oluşturma | Azure Active Directory cihazlar oluşturun. |
> | Microsoft. Directory/Devices/Delete | Azure Active Directory cihazları silin. |
> | Microsoft. Directory/cihazlar/devre dışı bırak | Azure Active Directory cihazları devre dışı bırakın. |
> | Microsoft. Directory/cihazlar/etkinleştir | Azure Active Directory cihazları etkinleştirin. |
> | Microsoft. Directory/Devices/extensionAttributes/Update | Azure Active Directory içindeki Devices. extensionAttributes özelliğinin tüm değerlerini güncelleştirin. |
> | Microsoft. Directory/Devices/kayıt araçları/güncelleştirme | Azure Active Directory Devices. Kaydedeteredowners özelliğini güncelleştirin. |
> | Microsoft. Directory/Devices/registeredUsers/Update | Azure Active Directory Devices. registeredUsers özelliğini güncelleştirin. |
> | Microsoft. Directory/deviceManagementPolicies/standart/okuma | Cihaz Yönetimi uygulama ilkelerindeki standart özellikleri okuyun |
> | Microsoft. Directory/deviceRegistrationPolicy/Standard/Read | Cihaz kayıt ilkelerinde standart özellikleri okuyun |
> | Microsoft. Directory/Groups/hiddenMembers/Read | Azure Active Directory gruplar. hiddenMembers özelliğini okuyun. |
> | Microsoft. Directory/groups. Security/Basic/Update | Azure Active Directory gruplardaki temel özellikleri güncelleştirin. |
> | Microsoft. Directory/gruplar. güvenlik/sınıflandırma/güncelleştirme | Rol atanabilir grupların dışlamasıyla güvenlik gruplarının sınıflandırma özelliğini güncelleştirme |
> | Microsoft. Directory/groups. Security/Create | Azure Active Directory grupları oluşturun. |
> | Microsoft. Directory/groups. Security/Delete | Azure Active Directory grupları silin. |
> | Microsoft. Directory/groups. Security/dynamicMembershipRule/Update | Rol atanabilir grupların dışlamasıyla birlikte güvenlik gruplarının dynamicMembershipRule özelliğini güncelleştirin |
> | Microsoft. Directory/groups. Security/groupType/Update | Rol atanabilir grupların dışlamasıyla birlikte güvenlik gruplarının Grup türü özelliğini güncelleştirme |
> | Microsoft. Directory/gruplar. güvenlik/Üyeler/güncelleştirme | Azure Active Directory groups. Members özelliğini güncelleştirin. |
> | Microsoft. Directory/gruplar. güvenlik/sahipler/güncelleştirme | Azure Active Directory groups. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/groups. Security/Visibility/Update | Rol atanabilir grupların dışlamasıyla güvenlik gruplarının görünürlük özelliğini güncelleştirme |
> | Microsoft. Directory/Users/Basic/Update | Azure Active Directory kullanıcıların temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/Users/Manager/Update | Azure Active Directory içindeki Users. Manager özelliğini güncelleştirin. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Intune/allEntities/allTasks | Intune 'un tüm yönlerini yönetin. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="kaizala-administrator-permissions"></a>Kaizala yönetici izinleri

, Microsoft Kaizala ayarlarını yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft 365 Yönetim merkezini okuyun. |

### <a name="license-administrator-permissions"></a>Lisans Yöneticisi izinleri

, Kullanıcılar ve gruplar üzerinde Ürün lisanslarını yönetebilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Users/Atamalisansı | Azure Active Directory kullanıcılar üzerinde lisansları yönetin. |
> | Microsoft. Directory/Users/usageLocation/Update | Azure Active Directory Users. usageLocation özelliğini güncelleştirin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |

### <a name="message-center-privacy-reader-permissions"></a>İleti Merkezi Gizlilik okuyucusu izinleri

Ileti merkezi gönderilerini, veri gizliliği iletilerini, grupları, etki alanlarını ve abonelikleri okuyabilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. messageCenter/messages/okundu | Microsoft. office365. messageCenter içindeki iletileri okuyun. |
> | Microsoft. office365. messageCenter/securityMessages/Read | Microsoft. office365. messageCenter içindeki securityMessages 'i okuyun. |

### <a name="message-center-reader-permissions"></a>İleti Merkezi okuyucu izinleri

Yalnızca Ileti merkezindeki kuruluşlarındaki iletileri ve güncelleştirmeleri okuyabilir. 

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. messageCenter/messages/okundu | Microsoft. office365. messageCenter içindeki iletileri okuyun. |

### <a name="modern-commerce-user-permissions"></a>Modern ticari Kullanıcı izinleri

, Bir şirket, departman veya takım için ticari satınalmaları yönetebilir. 

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Commerce. faturalandırma/iş ortakları/okuma | Microsoft 365 faturalandırma iş ortağı özelliğini okuyun. |
> | Microsoft. Commerce. volumeLicenseServiceCenter/allEntities/allTasks | Toplu Lisanslama hizmeti Merkezi 'nin tüm yönlerini yönetin. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Kendi Office 365 destek biletlerini oluşturun ve görüntüleyin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="network-administrator-permissions"></a>Ağ Yöneticisi izinleri

, Hizmet uygulamaları olarak Microsoft 365 yazılım için ağ konumlarını yönetebilir ve kurumsal ağ tasarımı öngörülerini gözden geçirebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. Network/Performance/allProperties/Read | M365 Yönetim Merkezi 'nde ağ performansı sayfalarını okuyun. |
> | Microsoft. office365. Network/Locations/allProperties/allTasks | Her konum için ağ konumları özelliklerini okuyun ve yapılandırın. |

### <a name="office-apps-administrator-permissions"></a>Office uygulamaları yönetici izinleri

, İlke ve ayarlar yönetimi dahil olmak üzere Office uygulamalarının bulut hizmetlerini yönetebilir ve son kullanıcının cihazlarına "yenilikler" özellik içeriğini seçme, seçme ve yayımlama yeteneğini yönetme olanağı sağlar.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. messageCenter/messages/okundu | Microsoft. office365. messageCenter içindeki iletileri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. userCommunication/allEntities/allTasks | Yeni iletilerin görünürlüğünü okuyun ve güncelleştirin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="partner-tier1-support-permissions"></a>İş ortağı Katman1 destek izinleri

Kullanmayın-genel kullanıma yönelik değildir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/uygulamalar/appRoles/Update | Uygulama rollerini yönetin ve uygulamalar için temsilci izinleri isteyin. |
> | Microsoft. Directory/uygulamalar/hedef kitle/güncelleştirme | Tüm uygulama türlerinde izleyiciyi güncelleştirin. |
> | Microsoft. Directory/uygulamalar/kimlik doğrulaması/güncelleştirme | Tüm uygulama türlerinde kimlik doğrulamasını güncelleştirin. |
> | Microsoft. Directory/uygulamalar/temel/güncelleştirme | Tüm uygulama türlerinde temel özellikleri güncelleştirin. |
> | Microsoft. Directory/uygulamalar/kimlik bilgileri/güncelleştirme | Tüm uygulama türlerinde kimlik bilgilerini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/sahipler/güncelleştirme | Tüm uygulama türlerindeki sahipleri güncelleştirin. |
> | Microsoft. Directory/uygulamalar/izinler/güncelleştirme | Tüm uygulama türlerinde sunulan izinleri ve gerekli izinleri güncelleştirin. |
> | Microsoft. Directory/uygulamalar/ilkeler/güncelleştirme | Azure Active Directory içindeki Applications. Policies özelliğini güncelleştirin. |
> | Microsoft. Directory/Contacts/Basic/Update | Azure Active Directory kişilerdeki temel özellikleri güncelleştirin. |
> | Microsoft. Directory/kişiler/oluştur | Azure Active Directory kişiler oluşturun. |
> | Microsoft. Directory/Contacts/Delete | Azure Active Directory kişileri silin. |
> | Microsoft. Directory/gruplar/oluştur | Azure Active Directory grupları oluşturun. |
> | Microsoft. Directory/gruplar/Sil | Rol atanabilir grup hariç olmak üzere grupları sil |
> | Microsoft. Directory/gruplar/Üyeler/güncelleştirme | Azure Active Directory groups. Members özelliğini güncelleştirin. |
> | Microsoft. Directory/gruplar/Owners/Update | Azure Active Directory groups. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/Groups/restore | Silinen grupları geri yükleme |
> | Microsoft. Directory/oAuth2PermissionGrants/allProperties/allTasks | OAuth 2,0 izin verir ve silin ve tüm özellikleri okuyun ve güncelleştirin |
> | Microsoft. Directory/Servicesorumlularını/appRoleAssignedTo/Update | Hizmet sorumlusu rol atamalarını güncelleştirme |
> | Microsoft. Directory/Users/Atamalisansı | Azure Active Directory kullanıcılar üzerinde lisansları yönetin. |
> | Microsoft. Directory/Users/Basic/Update | Azure Active Directory kullanıcıların temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/kullanıcılar/oluştur | Kullanıcı ekle |
> | Microsoft. Directory/kullanıcılar/Sil | Azure Active Directory Kullanıcıları silin. |
> | Microsoft. Directory/kullanıcılar/devre dışı bırak | Kullanıcıları devre dışı bırak |
> | Microsoft. Directory/kullanıcılar/etkinleştir | Kullanıcıları etkinleştir |
> | Microsoft. Directory/Users/ınvalidateallrefreshtokens | Azure Active Directory tüm Kullanıcı yenileme belirteçlerini geçersiz kılar. |
> | Microsoft. Directory/Users/Manager/Update | Azure Active Directory içindeki Users. Manager özelliğini güncelleştirin. |
> | Microsoft. Directory/Users/Password/Update | Azure Active Directory tüm kullanıcılar için parolaları güncelleştirin. Daha ayrıntılı bilgi için çevrimiçi belgelere bakın. |
> | Microsoft. Directory/kullanıcılar/geri yükleme | Azure Active Directory silinen kullanıcıları geri yükleyin. |
> | Microsoft. Directory/Users/userPrincipalName/Update | Azure Active Directory içindeki Users. userPrincipalName özelliğini güncelleştirin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="partner-tier2-support-permissions"></a>İş ortağı Katman2 destek izinleri

Kullanmayın-genel kullanıma yönelik değildir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/uygulamalar/appRoles/Update | Uygulama rollerini yönetin ve uygulamalar için temsilci izinleri isteyin. |
> | Microsoft. Directory/uygulamalar/hedef kitle/güncelleştirme | Tüm uygulama türlerinde izleyiciyi güncelleştirin. |
> | Microsoft. Directory/uygulamalar/kimlik doğrulaması/güncelleştirme | Tüm uygulama türlerinde kimlik doğrulamasını güncelleştirin. |
> | Microsoft. Directory/uygulamalar/temel/güncelleştirme | Tüm uygulama türlerinde temel özellikleri güncelleştirin. |
> | Microsoft. Directory/uygulamalar/kimlik bilgileri/güncelleştirme | Tüm uygulama türlerinde kimlik bilgilerini güncelleştirin. |
> | Microsoft. Directory/uygulamalar/sahipler/güncelleştirme | Tüm uygulama türlerindeki sahipleri güncelleştirin. |
> | Microsoft. Directory/uygulamalar/izinler/güncelleştirme | Tüm uygulama türlerinde sunulan izinleri ve gerekli izinleri güncelleştirin. |
> | Microsoft. Directory/uygulamalar/ilkeler/güncelleştirme | Azure Active Directory içindeki Applications. Policies özelliğini güncelleştirin. |
> | Microsoft. Directory/Contacts/Basic/Update | Azure Active Directory kişilerdeki temel özellikleri güncelleştirin. |
> | Microsoft. Directory/kişiler/oluştur | Azure Active Directory kişiler oluşturun. |
> | Microsoft. Directory/Contacts/Delete | Azure Active Directory kişileri silin. |
> | Microsoft. Directory/Domains/Basic/allTasks | Etki alanlarını oluşturun ve silin ve Azure Active Directory standart özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/gruplar/oluştur | Azure Active Directory grupları oluşturun. |
> | Microsoft. Directory/gruplar/Sil | Azure Active Directory grupları silin. |
> | Microsoft. Directory/gruplar/Üyeler/güncelleştirme | Azure Active Directory groups. Members özelliğini güncelleştirin. |
> | Microsoft. Directory/gruplar/Owners/Update | Rol atanabilir gruplar hariç olmak üzere grupların sahiplerini Güncelleştir |
> | Microsoft. Directory/Groups/restore | Azure Active Directory grupları geri yükleyin. |
> | Microsoft. Directory/oAuth2PermissionGrants/allProperties/allTasks | OAuth 2,0 izin verir ve silin ve tüm özellikleri okuyun ve güncelleştirin |
> | Microsoft. Directory/kuruluş/temel/güncelleştirme | Azure Active Directory 'da kuruluştaki temel özellikleri güncelleştirin. |
> | Microsoft. Directory/Roleatamaları/allProperties/allTasks | Rol atamaları oluşturun ve silin ve tüm rol atama özelliklerini okuyun ve güncelleştirin |
> | Microsoft. Directory/roleDefinitions/allProperties/allTasks | Rol tanımları oluşturun ve silin ve tüm özellikleri okuyun ve güncelleştirin |
> | Microsoft. Directory/Scopedrolemembersevk/allProperties/allTasks | Scopedrolemembersevk eden oluşturun ve silin ve tüm özellikleri okuyun ve güncelleştirin |
> | Microsoft. Directory/Servicesorumlularını/appRoleAssignedTo/Update | Hizmet sorumlusu rol atamalarını güncelleştirme |
> | Microsoft. Directory/subscribedSkus/Standard/Read | Aboneliklerdeki temel özellikleri okuyun |
> | Microsoft. Directory/Users/Atamalisansı | Azure Active Directory kullanıcılar üzerinde lisansları yönetin. |
> | Microsoft. Directory/Users/Basic/Update | Azure Active Directory kullanıcıların temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/kullanıcılar/oluştur | Kullanıcı ekle |
> | Microsoft. Directory/kullanıcılar/Sil | Azure Active Directory Kullanıcıları silin. |
> | Microsoft. Directory/kullanıcılar/devre dışı bırak | Kullanıcıları devre dışı bırak |
> | Microsoft. Directory/kullanıcılar/etkinleştir | Kullanıcıları etkinleştir |
> | Microsoft. Directory/Users/ınvalidateallrefreshtokens | Azure Active Directory tüm Kullanıcı yenileme belirteçlerini geçersiz kılar. |
> | Microsoft. Directory/Users/Manager/Update | Azure Active Directory içindeki Users. Manager özelliğini güncelleştirin. |
> | Microsoft. Directory/Users/Password/Update | Azure Active Directory tüm kullanıcılar için parolaları güncelleştirin. Daha ayrıntılı bilgi için çevrimiçi belgelere bakın. |
> | Microsoft. Directory/kullanıcılar/geri yükleme | Azure Active Directory silinen kullanıcıları geri yükleyin. |
> | Microsoft. Directory/Users/userPrincipalName/Update | Azure Active Directory içindeki Users. userPrincipalName özelliğini güncelleştirin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="password-administrator-permissions"></a>Parola yönetici izinleri

, Yönetici olmayan ve parola yöneticileri için parolaları sıfırlayabilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Users/Password/Update | Azure Active Directory tüm kullanıcılar için parolaları güncelleştirin. Daha ayrıntılı bilgi için çevrimiçi belgelere bakın. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="power-bi-administrator-permissions"></a>Yönetici izinlerini Power BI

Power BI ürünün tüm yönlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. powerApps. PowerBI/allEntities/allTasks | Power BI tüm yönlerini yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="power-platform-administrator-permissions"></a>Güç platformu yönetici izinleri

, Microsoft Dynamics 365, PowerApps ve Power otomatikleştirmenin tüm yönlerini oluşturabilir ve yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. dynamics365/allEntities/allTasks | Dynamics 365 'in tüm yönlerini yönetin. |
> | Microsoft. Flow/Allentitıes/allTasks | Güç otomatikleştirmenin tüm yönlerini yönetin. |
> | Microsoft. powerApps/allEntities/allTasks | PowerApps 'in tüm yönlerini yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="printer-administrator-permissions"></a>Yazıcı Yöneticisi izinleri

, Yazıcıların ve yazıcı bağlayıcılarının tüm yönlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. Print/Allentitıes/allProperties/allTasks | Yazıcı ve bağlayıcılar oluşturup silin ve Microsoft Print 'teki tüm özellikleri okuyun ve güncelleştirin. |

### <a name="printer-technician-permissions"></a>Yazıcı teknisyen izinleri

Yazıcı kaydedebilir ve kayıt kaydedebilir ve yazıcı durumunu güncelleştirebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. Print/bağlayıcılar/allProperties/Read | Microsoft Print 'teki tüm bağlayıcıların özelliklerini okuyun. |
> | Microsoft. Azure. Print/Printers/allProperties/Read | Microsoft Print 'teki yazıcıların tüm özelliklerini okuyun. |
> | Microsoft. Azure. Print/Printers/Basic/Update | Microsoft Print 'te yazıcıların temel özelliklerini güncelleştirin. |
> | Microsoft. Azure. Print/Printers/Register | Yazıcıları Microsoft Print 'e kaydedin. |
> | Microsoft. Azure. Print/Printers/Unregister | Microsoft yazdırma 'da yazıcıların kaydını kaldırın. |

### <a name="privileged-authentication-administrator-permissions"></a>Ayrıcalıklı kimlik doğrulama Yöneticisi izinleri

Herhangi bir Kullanıcı (yönetici veya yönetici olmayan) için kimlik doğrulama yöntemi bilgilerini görüntüleme, ayarlama ve sıfırlamaya izin verilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Users/ınvalidateallrefreshtokens | Azure Active Directory tüm Kullanıcı yenileme belirteçlerini geçersiz kılar. |
> | Microsoft. Directory/Users/strongAuthentication/Update | MFA kimlik bilgileri gibi güçlü kimlik doğrulama özelliklerini güncelleştirin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. Directory/Users/Password/Update | Microsoft 365 kuruluştaki tüm kullanıcılar için parolaları güncelleştirin. Daha ayrıntılı bilgi için çevrimiçi belgelere bakın. |

### <a name="privileged-role-administrator-permissions"></a>Ayrıcalıklı rol Yöneticisi izinleri

, Azure AD 'de rol atamalarını ve tüm Privileged Identity Management yönlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Groupsastifbletoroles/allProperties/Update | Azure Active Directory, ısastifbletorole özelliği true olarak ayarlanan grupları güncelleştirin. |
> | Microsoft. Directory/Groupsastifbletoroles/Create | Azure Active Directory ' de ısastifbletorole özelliği true olarak ayarlanan gruplar oluşturun. |
> | Microsoft. Directory/Groupsastifbletoroles/Delete | Iastifbletorole özelliği Azure Active Directory olan grupları silin. |
> | Microsoft. Directory/privilegedIdentityManagement/allEntities/allTasks | Tüm kaynakları oluşturup silin ve Microsoft. AAD. Privilegedıdentitymanagement içindeki standart özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Servicesorumlularını/appRoleAssignedTo/allTasks | Azure Active Directory Servicesorumlularını. appRoleAssignedTo özelliğini okuyun ve yapılandırın. |
> | Microsoft. Directory/Servicesorumlularını/oAuth2PermissionGrants/allTasks | Azure Active Directory Servicesorumlularını. oAuth2PermissionGrants özelliğini okuyun ve yapılandırın. |
> | Microsoft. Directory/Yönetimtiveunits/allProperties/allTasks | Yönetim birimleri oluşturma ve yönetme (Üyeler dahil) |
> | Microsoft. Directory/Roleatamaları/allProperties/allTasks | Rol atamaları oluşturun ve yönetin. |
> | Microsoft. Directory/roleDefinitions/allProperties/allTasks | Rol tanımları oluşturun ve yönetin. |

### <a name="reports-reader-permissions"></a>Rapor okuyucusu izinleri

, Oturum açma ve Denetim raporlarını okuyabilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/auditLogs/allProperties/Read | Azure Active Directory içindeki auditLogs 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Directory/provisioningLogs/allProperties/Read | Sağlama günlüklerinin tüm özelliklerini okuyun. |
> | Microsoft. Directory/Signınreports/allProperties/Read | Azure Active Directory içindeki Signınreports 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. usageReports/allEntities/okuma | Office 365 kullanım raporlarını okuyun. |

### <a name="search-administrator-permissions"></a>Yönetici izinlerinde ara

, Microsoft Search ayarlarının tüm yönlerini oluşturabilir ve yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. messageCenter/messages/okundu | Microsoft. office365. messageCenter içindeki iletileri okuyun. |
> | Microsoft. office365. Search/Allentitıes/allProperties/allTasks | Tüm kaynakları oluşturup silin ve Microsoft. office365. Search içindeki tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="search-editor-permissions"></a>Arama Düzenleyicisi izinleri

Yer işaretleri, Q ve as, konumlar, FloorPlan gibi düzenleme içeriği oluşturabilir ve yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. messageCenter/messages/okundu | Microsoft. office365. messageCenter içindeki iletileri okuyun. |
> | Microsoft. office365. Search/Content/allProperties/allTasks | İçerik oluşturun ve silin ve Microsoft. office365. Search içindeki tüm özellikleri okuyun ve güncelleştirin. |

### <a name="security-administrator-permissions"></a>Güvenlik Yöneticisi izinleri

Güvenlik bilgilerini ve raporlarını okuyabilir ve Azure AD 'de yapılandırmayı yönetebilir ve Microsoft 365.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Directory/uygulamalar/ilkeler/güncelleştirme | Azure Active Directory içindeki Applications. Policies özelliğini güncelleştirin. |
> | Microsoft. Directory/auditLogs/allProperties/Read | Azure Active Directory içindeki auditLogs 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Directory/bitlockerKeys/Key/Read | Azure Active Directory içindeki BitLocker anahtar nesnelerini ve özelliklerini (kurtarma anahtarı dahil) okuyun. |
> | Microsoft. Directory/entitlementManagement/allProperties/Read | Azure AD Yetkilendirme Yönetimi 'ndeki tüm özellikleri okuyun. |
> | Microsoft. Directory/ıdentityprotection/allProperties/Read | Microsoft. AAD. ıdentityprotection içindeki tüm kaynakları okuyun. |
> | Microsoft. Directory/ıdentityprotection/allProperties/Update | Microsoft. AAD. ıdentityprotection içindeki tüm kaynakları güncelleştirin. |
> | Microsoft. Directory/policies/Basic/Update | Azure Active Directory ilkelerde temel özellikleri güncelleştirin. |
> | Microsoft. Directory/policies/Create | Azure Active Directory ilke oluşturun. |
> | Microsoft. Directory/policies/Delete | Azure Active Directory ilkeleri silin. |
> | Microsoft. Directory/policies/Owners/Update | Azure Active Directory içindeki policies. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/policies/tenantDefault/Update | Azure Active Directory ilkeleri. tenantDefault özelliğini güncelleştirin. |
> | Microsoft. Directory/Privilegedıdentitymanagement/allProperties/Read | Microsoft. AAD. Privilegedıdentitymanagement içindeki tüm kaynakları okuyun. |
> | Microsoft. Directory/provisioningLogs/allProperties/Read | Sağlama günlüklerinin tüm özelliklerini okuyun. |
> | Microsoft. Directory/Servicesorumlularını/ilkeleri/güncelleştirme | Azure Active Directory Servicesorumlularını. Policies özelliğini güncelleştirin. |
> | Microsoft. Directory/Signınreports/allProperties/Read | Azure Active Directory içindeki Signınreports 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. office365. protectionCenter/allEntities/okuma | Office 365 Koruma Merkezi 'nin tüm yönlerini okuyun. |
> | Microsoft. office365. protectionCenter/allEntities/Update | Microsoft. office365. protectionCenter 'daki tüm kaynakları güncelleştirin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="security-operator-permissions"></a>Güvenlik Işleci izinleri

Güvenlik olaylarını oluşturur ve yönetir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. advancedThreatProtection/Allentitıes/Read | Azure AD Gelişmiş tehdit korumasını okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Directory/cloudAppSecurity/allProperties/allTasks | Microsoft Cloud App Security okuyun ve yapılandırın. |
> | Microsoft. Directory/ıdentityprotection/allProperties/Read | Microsoft. AAD. ıdentityprotection içindeki tüm kaynakları okuyun. |
> | Microsoft. Directory/Privilegedıdentitymanagement/allProperties/Read | Microsoft. AAD. Privilegedıdentitymanagement içindeki tüm kaynakları okuyun. |
> | Microsoft. Directory/provisioningLogs/allProperties/Read | Sağlama günlüklerinin tüm özelliklerini okuyun. |
> | Microsoft. Intune/allEntities/allTasks | Intune 'un tüm yönlerini yönetin. |
> | Microsoft. office365. Securityzorluk Ancecenter/allEntities/allTasks | Güvenlik & uyumluluk merkezini okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. Windows. Savunderadvancedthreatprotection/Allentitıes/Read | Windows Defender Gelişmiş tehdit koruması 'nı okuyun ve yapılandırın. |

### <a name="security-reader-permissions"></a>Güvenlik okuyucusu izinleri

, Azure AD ve Microsoft 365 güvenlik bilgilerini ve raporlarını okuyabilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/auditLogs/allProperties/Read | Azure Active Directory içindeki auditLogs 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. Directory/bitlockerKeys/Key/Read | Azure Active Directory içindeki BitLocker anahtar nesnelerini ve özelliklerini (kurtarma anahtarı dahil) okuyun. |
> | Microsoft. Directory/entitlementManagement/allProperties/Read | Azure AD Yetkilendirme Yönetimi 'ndeki tüm özellikleri okuyun. |
> | Microsoft. Directory/policies/conditionalAccess/temel/okuma | Azure Active Directory içindeki policies. conditionalAccess özelliğini okuyun. |
> | Microsoft. Directory/Signınreports/allProperties/Read | Azure Active Directory içindeki Signınreports 'ta tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
> | Microsoft. AAD. ıdentityprotection/Allentitıes/Read | Microsoft. AAD. ıdentityprotection içindeki tüm kaynakları okuyun. |
> | Microsoft. AAD. Privilegedıdentitymanagement/allEntities/okuma | Microsoft. AAD. Privilegedıdentitymanagement içindeki tüm kaynakları okuyun. |
> | Microsoft. Directory/provisioningLogs/allProperties/Read | Sağlama günlüklerinin tüm özelliklerini okuyun. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. protectionCenter/allEntities/okuma | Office 365 Koruma Merkezi 'nin tüm yönlerini okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |

### <a name="service-support-administrator-permissions"></a>Hizmet desteği yönetici izinleri

, Hizmet durumu bilgilerini okuyabilir ve destek biletlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="sharepoint-administrator-permissions"></a>SharePoint yönetici izinleri

, SharePoint hizmetinin tüm yönlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Directory/groups. Unified/Basic/Update | Microsoft 365 gruplarının temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/groups. Unified/Create | Microsoft 365 grupları oluşturun. |
> | Microsoft. Directory/groups. Unified/Delete | Microsoft 365 grupları silin. |
> | Microsoft. Directory/groups. Unified/Members/Update | Microsoft 365 gruplarının üyeliğini güncelleştirin. |
> | Microsoft. Directory/groups. Unified/Owners/Update | Microsoft 365 gruplarının sahipliğini güncelleştirin. |
> | Microsoft. Directory/groups. Unified/restore | Microsoft 365 gruplarını geri yükleme |
> | Microsoft. office365. Network/Performance/allProperties/Read | M365 Yönetim Merkezi 'nde ağ performansı sayfalarını okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. SharePoint/allEntities/allTasks | Tüm kaynakları oluşturup silin ve Microsoft. office365. SharePoint içindeki standart özellikleri okuyun ve güncelleştirin. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. usageReports/allEntities/allProperties/Read | Office 365 kullanım raporlarını okuyun. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="skype-for-business-administrator-permissions"></a>Skype Kurumsal Yönetici izinleri

, Skype Kurumsal ürününün tüm yönlerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Sktypeınfo/allEntities/allTasks | Skype Kurumsal Çevrimiçi 'nin tüm yönlerini yönetin. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. usageReports/allEntities/okuma | Office 365 kullanım raporlarını okuyun. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="teams-administrator-permissions"></a>Takımlar yönetici izinleri

, Microsoft ekipleri hizmetini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. Directory/Groups/hiddenMembers/Read | Azure Active Directory gruplar. hiddenMembers özelliğini okuyun. |
> | Microsoft. Directory/Groups/Unified/Approtaatamalar/Update | Azure Active Directory groups. Unified özelliğini güncelleştirin. |
> | Microsoft. Directory/groups. Unified/Basic/Update | Microsoft 365 gruplarının temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/groups. Unified/Create | Microsoft 365 grupları oluşturun. |
> | Microsoft. Directory/groups. Unified/Delete | Microsoft 365 grupları silin. |
> | Microsoft. Directory/groups. Unified/Members/Update | Microsoft 365 gruplarının üyeliğini güncelleştirin. |
> | Microsoft. Directory/groups. Unified/Owners/Update | Microsoft 365 gruplarının sahipliğini güncelleştirin. |
> | Microsoft. Directory/groups. Unified/restore | Microsoft 365 gruplarını geri yükleme |
> | Microsoft. Directory/Servicesorumlularını/managePermissionGrantsForGroup. Microsoft-All-Application-Permissions | Grup adına temsilci izinleri için izin verme |
> | Microsoft. office365. Network/Performance/allProperties/Read | M365 Yönetim Merkezi 'nde ağ performansı sayfalarını okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Sktypeınfo/allEntities/allTasks | Skype Kurumsal Çevrimiçi 'nin tüm yönlerini yönetin |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. usageReports/allEntities/allProperties/Read | Office 365 kullanım raporlarını okuyun. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. ekipler/allEntities/allProperties/allTasks | Ekipteki tüm kaynakları yönetin. |

### <a name="teams-communications-administrator-permissions"></a>Takımlar Iletişimleri yönetici izinleri

, Microsoft ekipleri hizmeti içindeki çağrı ve toplantılar özelliklerini yönetebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. usageReports/allEntities/okuma | Office 365 kullanım raporlarını okuyun. |
> | Microsoft. ekipler/toplantılar/allProperties/allTasks | Toplantı ilkeleri, konfigürasyonlar ve konferans köprüleri dahil olmak üzere toplantıları yönetin. |
> | Microsoft. ekipler/ses/allProperties/allTasks | Arama ilkeleri ve telefon numarası envanteri ve atama dahil olmak üzere sesi yönetin. |
> | Microsoft. ekipler/callQuality/allProperties/Read | Çağrı kalitesi panosundaki (CQD) tüm verileri okuyun. |

### <a name="teams-communications-support-engineer-permissions"></a>Takımlar Iletişimleri destek mühendisi izinleri

, Gelişmiş araçları kullanarak takımlar içindeki iletişim sorunlarını giderebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. ekipler/callQuality/allProperties/Read | Çağrı kalitesi panosundaki (CQD) tüm verileri okuyun. |

### <a name="teams-communications-support-specialist-permissions"></a>Takımlar Iletişimleri destek uzmanı izinleri

Temel araçları kullanarak takımlar içindeki iletişim sorunlarını giderebilirsiniz.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. ekipler/callQuality/Basic/Read | Çağrı kalitesi panosundaki temel verileri okuyun (CQD). |

### <a name="teams-devices-administrator-permissions"></a>Takımlar cihazlar yönetici izinleri

Ekip sertifikalı cihazlarda yönetim ile ilgili görevler gerçekleştirebilir.

> [!NOTE]
> Bu rol Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol açıklaması konusuna bakın.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. takımlar/cihazlar/temel/okuma | Yapılandırma ilkeleri de dahil olmak üzere takımlar tarafından sertifikalı cihazların tüm yönlerini yönetin. |

### <a name="usage-summary-reports-reader-permissions"></a>Kullanım Özeti raporları okuyucu izinleri

, M365 kullanım analizlerinin ve üretkenlik puanındaki yalnızca kiracı düzeyindeki toplamaları görebilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. office365. usageReports/allEntities/standart/okuma | Kiracı düzeyinde toplanmış Office 365 kullanım raporlarını okuyun. |
> | Microsoft. office365. webPortal/allEntities/standart/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun.|

### <a name="user-administrator-permissions"></a>Kullanıcı Yöneticisi izinleri

, Sınırlı yöneticiler için parola sıfırlama dahil olmak üzere kullanıcıların ve grupların tüm yönlerini yönetebilir.

> [!div class="mx-tableFixed"]
> | Eylemler | Açıklama |
> | --- | --- |
> | Microsoft. Directory/Approtaatamalar/oluşturma | Azure Active Directory Approlet atamaları oluşturun. |
> | Microsoft. Directory/Approtaatamaları/silme | Azure Active Directory Approtaatamaları silin. |
> | Microsoft. Directory/Approtaatamalar/güncelleştirme | Azure Active Directory Approtaatamaları güncelleştirin. |
> | Microsoft. Directory/Contacts/Basic/Update | Azure Active Directory kişilerdeki temel özellikleri güncelleştirin. |
> | Microsoft. Directory/kişiler/oluştur | Azure Active Directory kişiler oluşturun. |
> | Microsoft. Directory/Contacts/Delete | Azure Active Directory kişileri silin. |
> | Microsoft. Directory/entitlementManagement/allProperties/allTasks | Kaynakları oluşturup silin ve Azure AD Yetkilendirme Yönetimi 'nde tüm özellikleri okuyun ve güncelleştirin. |
> | Microsoft. Directory/Groups/Approtaatamalar/güncelleştirme | Azure Active Directory groups. Approtaatamalar özelliğini güncelleştirin. |
> | Microsoft. Directory/Groups/Basic/Update | Azure Active Directory gruplardaki temel özellikleri güncelleştirin. |
> | Microsoft. Directory/gruplar/oluştur | Azure Active Directory grupları oluşturun. |
> | Microsoft. Directory/Groups/createAsOwner | Azure Active Directory grupları oluşturun. Oluşturucu ilk sahip olarak eklenir ve oluşturulan nesne, oluşturucunun 250 tarafından oluşturulan nesneler kotasına göre sayılır. |
> | Microsoft. Directory/gruplar/Sil | Azure Active Directory grupları silin. |
> | Microsoft. Directory/Groups/hiddenMembers/Read | Azure Active Directory gruplar. hiddenMembers özelliğini okuyun. |
> | Microsoft. Directory/gruplar/Üyeler/güncelleştirme | Azure Active Directory groups. Members özelliğini güncelleştirin. |
> | Microsoft. Directory/gruplar/Owners/Update | Azure Active Directory groups. Owners özelliğini güncelleştirin. |
> | Microsoft. Directory/Groups/restore | Azure Active Directory grupları geri yükleyin. |
> | Microsoft. Directory/Groups/Settings/Update | Azure Active Directory groups. Settings özelliğini güncelleştirin. |
> | Microsoft. Directory/Users/Approtaatamalar/güncelleştirme | Azure Active Directory Users. Approtaatamalar özelliğini güncelleştirin. |
> | Microsoft. Directory/Users/Atamalisansı | Azure Active Directory kullanıcılar üzerinde lisansları yönetin. |
> | Microsoft. Directory/Users/Basic/Update | Azure Active Directory kullanıcıların temel özelliklerini güncelleştirin. |
> | Microsoft. Directory/kullanıcılar/oluştur | Azure Active Directory’de kullanıcılar oluşturun. |
> | Microsoft. Directory/kullanıcılar/Sil | Azure Active Directory Kullanıcıları silin. |
> | Microsoft. Directory/Users/ınvalidateallrefreshtokens | Azure Active Directory tüm Kullanıcı yenileme belirteçlerini geçersiz kılar. |
> | Microsoft. Directory/Users/Manager/Update | Azure Active Directory içindeki Users. Manager özelliğini güncelleştirin. |
> | Microsoft. Directory/Users/Password/Update | Azure Active Directory tüm kullanıcılar için parolaları güncelleştirin. Daha ayrıntılı bilgi için çevrimiçi belgelere bakın. |
> | Microsoft. Directory/kullanıcılar/geri yükleme | Azure Active Directory silinen kullanıcıları geri yükleyin. |
> | Microsoft. Directory/Users/userPrincipalName/Update | Azure Active Directory içindeki Users. userPrincipalName özelliğini güncelleştirin. |
> | Microsoft. Azure. serviceHealth/Allentitıes/allTasks | Azure hizmet durumunu okuyun ve yapılandırın. |
> | Microsoft. Azure. Supportbilet/allEntities/allTasks | Dizin düzeyinde hizmetler için Azure destek biletleri oluşturun ve yönetin. |
> | Microsoft. office365. webPortal/allEntities/temel/okuma | Microsoft. office365. webPortal 'daki tüm kaynaklarda temel özellikleri okuyun. |
> | Microsoft. office365. serviceHealth/allEntities/allTasks | Microsoft 365 hizmeti durumunu okuyun ve yapılandırın. |
> | Microsoft. office365. Supportbilet/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

## <a name="role-template-ids"></a>Rol şablonu kimlikleri

Rol şablonu kimlikleri, genellikle Microsoft Graph API 'SI veya PowerShell kullanıcıları tarafından kullanılır.

Grafik displayName | Görünen ad Azure portal | Directoryroletemplateıd
----------------- | ------------------------- | -------------------------
Uygulama Yöneticisi | Uygulama Yöneticisi | 9B895D92-2CD3-44C7-9D02-A6AC2D5EA5C3
Uygulama Geliştirici | Uygulama geliştiricisi | CF1C38E5-3621-4004-A7CB-879624DCED7C
Kimlik doğrulama Yöneticisi | Kimlik doğrulama Yöneticisi | c4e39bd9-1100-46d3-8c65-fb160da0071f
Kimlik doğrulama Ilkesi Yöneticisi | Kimlik doğrulama İlkesi Yöneticisi | 0526716b-113d-4c15-b2c8-68e3c22b9f80
Saldırı yükü yazarı | Saldırı yükü yazarı | 9c6df0f2-1e7c-4dc3-B195-66dfbd24aa8f
Saldırı simülasyonu Yöneticisi | Saldırı simülasyonu Yöneticisi | c430b396-e693-46cc-96f3-db01bf8bb62a
Azure AD 'ye katılmış cihaz yerel Yöneticisi | Azure AD 'ye katılmış cihaz yerel Yöneticisi | 9f06204d-73c1-4d4c-880a-6edb90606fd8
Azure DevOps Yöneticisi | Azure DevOps Yöneticisi | e3973bdf-4987-49ae-837a-ba8e231c7286
Azure Information Protection Yöneticisi | Azure Information Protection Yöneticisi | 7495fdc4-34c4-4d15-A289-98788ce399fd
B2C ıEF anahtar kümesi Yöneticisi | B2C ıEF anahtar kümesi Yöneticisi | aaf43236-0c0d-4d5f-883a-6955382ac081
B2C ıEF Ilke Yöneticisi | B2C ıEF Ilke Yöneticisi | 3edaf663-341e-4475-9f94-5c398ef6c070
Faturalama Yöneticisi | Faturalama yöneticisi | b0f54661-2d74-4c50-afa3-1ec803f12efe
Bulut Uygulaması Yöneticisi | Bulut uygulaması Yöneticisi | 158c047a-C907-4556-b7ef-446551a6b5f7
Bulut Cihaz Yöneticisi | Bulut Cihaz Yöneticisi | 7698a772-787B-4ac8-901f-60d6b08afd2
Uyumluluk Yöneticisi | Uyumluluk Yöneticisi | 17315797-102D-40b4-93e0-432062caca18
Uyumluluk verileri Yöneticisi | Uyumluluk verileri Yöneticisi | e6d1a23a-da11-4be4-9570-befc86d067a7
Koşullu Erişim Yöneticisi | Koşullu Erişim Yöneticisi | b1be1c3e-b65d-4f19-8427-f6fa0d97feb9
Müşteri Kasası erişim onaylayıcısı | Müşteri Kasası erişimi onaylayan | 5c4f9dcd-47dc-4CF7-8c9a-9e4207cbfc91
Masaüstü Analizi Yöneticisi | Masaüstü Analizi Yöneticisi | 38a96431-2bdf-4B4C-8B6E-5d3d8abac1a4
Cihaz katılımı | Kullanım Dışı | 9c094953-4995-41C8-84c8-3ebb9b32c93f
Cihaz yöneticileri | Kullanım Dışı | 2b499bcd-da44-4968-8aec-78e1674fa64d
Cihaz kullanıcıları | Kullanım Dışı | d405c6df-0af8-4e3b-95e4-4d06e542189e
Dizin okuyucuları | Dizin okuyucuları | 88d8e3e3-8f55-4a1e-953a-9b9898b8876b
Dizin eşitleme hesapları | Kullanılmaması nedeniyle gösterilmez | d29b2b05-8046-44ba-8758-1e26182fcf32
Dizin yazarları | Dizin yazarları | 9360feb5-f418-4baa-8175-e2a00bac4301
Etki alanı adı yöneticisi | Etki alanı adı yöneticisi | 8329153b-31d0-4727-b945-745eb3bc5f31
Dynamics 365 Yöneticisi | Dynamics 365 Yöneticisi | 44367163-eba1-44c3-98af-f5787879f96a
Exchange Yöneticisi | Exchange yöneticisi | 29232cdf-9323-42fd-ade2-1d097af3e4de
Dış kimlik Kullanıcı akış Yöneticisi | Dış kimlik Kullanıcı akış Yöneticisi | 6e591065-9bad-43ed-90f3-e9424366d2f0
Dış kimlik Kullanıcı akışı öznitelik Yöneticisi | Dış kimlik Kullanıcı akışı öznitelik Yöneticisi | 0f971eea-41eb-4569-a71e-57bb8a3eff1e
Dış kimlik sağlayıcısı Yöneticisi | Dış kimlik sağlayıcısı Yöneticisi | be2f45a1-457d-42af-a067-6ec1fa63bc45
Genel Yönetici | Genel yönetici | 62e90394-69f5-4237-9190-012177145e10
Genel okuyucu | Genel okuyucu | f2ef992c-3afb-46b9-b7cf-a126ee74c451
Grup Yöneticisi | Grup Yöneticisi | fdd7a751-b60b-444a-984c-02652fe8fa1c 
Konuk davetci | Konuk davetci | 95e79109-95c0-4d8e-aee3-d01accf2d47b
Yardım Masası Yöneticisi | Yardım Masası Yöneticisi | 729827e3-9c14-49f7-bb1b-9608f156bbb8
Karma kimlik yöneticisi | Karma kimlik yöneticisi | 8ac3fc64-6eca-42ea-9e69-59f4c7b60eb2
Öngörüler Yöneticisi | Öngörüler Yöneticisi | eb1f4a8d-243a-41f0-9fbd-c7cdf6c5ef7c
Öngörüler Iş lideri | Öngörüler iş lideri | 31e939ad-9672-4796-9c2e-873181342d2d
Intune Yöneticisi | Intune yöneticisi | 3a2c62db-5318-420d-8d74-23afee5d9d5
Kaizala Yöneticisi | Kaizala Yöneticisi | 74ef975b-6605-40af-a5d2-b9539d836353
Lisans Yöneticisi | Lisans yöneticisi | 4d6ac14f-3453-41d0-bef9-a3e0c569773a
İleti Merkezi Gizlilik okuyucusu | İleti Merkezi Gizlilik okuyucusu | ac16e43d-7b2d-40e0-ac05-243ff356ab5b
İleti Merkezi okuyucusu | İleti Merkezi okuyucusu | 790c1fb9-7f7d-4f88-86a1-ef1f95c05c1b
Modern ticari Kullanıcı | Modern ticari Kullanıcı | d24aef57-1500-4070-84db-2666f29cf966
Ağ Yöneticisi | Ağ Yöneticisi | d37c8bed-0711-4417-ba38-b4abe66ce4c2
Office uygulamaları Yöneticisi | Office uygulamaları Yöneticisi | 2b745bdf-0803-4d80-aa65-822c4493dadac
Partner Katman1 desteği | Kullanılmaması nedeniyle gösterilmez | 4ba39ca4-527c-499a-B93D-d9b492c50246
Partner Katman2 desteği | Kullanılmaması nedeniyle gösterilmez | e00e864a-17c5-4a4b-9c06-f5b95a8d5bd8
Parola Yöneticisi | Parola yöneticisi | 966707d0-3269-4727-9be2-8c3a10f19b9d
Power BI Yöneticisi | Power BI Yöneticisi | a9ea8996-122f-4c74-9520-8edcd192826c
Power Platform Yöneticisi | Power platformu Yöneticisi | 11648597-926c-4CF3-9c36-bcebb0ba8dcc
Yazıcı Yöneticisi | Yazıcı Yöneticisi | 644ef478-e28f-4e28-b9dc-3fdde9aa0b1f
Yazıcı teknisyeni | Yazıcı teknisyeni | e8cef6f1-e4bd-4ea8-bc07-4b8d950f4477
Ayrıcalıklı kimlik doğrulama Yöneticisi | Ayrıcalıklı kimlik doğrulama Yöneticisi | 7be44c8a-adaf-4e2a-84d6-ab2649e08a13
Ayrıcalıklı rol yöneticisi | Ayrıcalıklı rol yöneticisi | e8611ab8-c189-46e8-94e1-60213ab1f814
Rapor okuyucu | Rapor okuyucu | 4a5d8f65-41DA-4de4-8968-e035b65339cf
Yönetici ara | Yönetici ara | 0964bb5e-9bdb-4d7b-ac29-58e794862a40
Arama Düzenleyicisi | Arama Düzenleyicisi | 8835291a-918c-4fd7-a9ce-faa49f0cf7d9
Güvenlik Yöneticisi | Güvenlik yöneticisi | 194ae4cb-B126-40b2-bd5b-6091b380977d
Güvenlik operatörü | Güvenlik operatörü | 5f2222b1-57c3-48ba-8ad5-d4759f1fde6f
Güvenlik Okuyucusu | Güvenlik okuyucusu | 5d6b6bb7-de71-4623-B4AF-96380a352509
Hizmet desteği Yöneticisi | Hizmet desteği yöneticisi | f023fd81-a637-4b56-95fd-791ac0226033
SharePoint Yöneticisi | SharePoint yöneticisi | f28a1f50-f6e7-4571-818b-6a12f2af6b6c
Skype Kurumsal Yöneticisi | Skype Kurumsal yöneticisi | 75941009-915a-4869-ABE7-691bff18279e
Takımlar Yöneticisi | Takımlar Yöneticisi | 69091246-20e8-4a56-aa4d-066075b2a7a8
Takımlar Iletişim Yöneticisi | Takımlar Iletişim Yöneticisi | baf37b3a-610e-45da-9e62-d9d1e5e8914b
Takımlar Iletişimleri Destek Mühendisi | Takımlar Iletişimleri Destek Mühendisi | f70938a0-fc10-4177-9e90-2178f8765737
Takımlar Iletişimleri destek uzmanı | Takımlar Iletişimleri destek uzmanı | fcf91098-03e3-41a9-b5ba-6f0ec8188a12
Takımlar cihazları Yöneticisi | Takımlar cihazları Yöneticisi | 3d762c5a-1B6C-493F-843e-55a3b42923d4
Kullanım Özeti raporları okuyucusu | Kullanım Özeti raporları okuyucusu | 75934031-6c7e-415A-99d7-48dbd49e875e
Kullanıcı | Kullanılmadığından gösterilmez | a0b1b346-4d3e-4e8b-98f8-753987be4970
Kullanıcı Yöneticisi | Kullanıcı yöneticisi | fe930be7-5e62-47db-91af-98c3a49a38b1
Çalışma alanına cihaz katılımı | Kullanım Dışı | c34f683f-4d5a-4403-AFD-6615e00e3a7f

## <a name="deprecated-roles"></a>Kullanım dışı roller

Aşağıdaki roller kullanılmamalıdır. Bunlar kullanım dışı bırakılmıştır ve gelecekte Azure AD 'den kaldırılacaktır.

* Geçici Lisans Yöneticisi
* Cihaz katılımı
* Cihaz yöneticileri
* Cihaz kullanıcıları
* E-posta doğrulanan kullanıcı Oluşturucu
* Posta kutusu Yöneticisi
* Çalışma alanına cihaz katılımı

## <a name="roles-not-shown-in-the-portal"></a>Portalda gösterilen roller

PowerShell veya MS Graph API tarafından döndürülen her rol Azure portal görünür. Aşağıdaki tabloda bu farklılıklar düzenlerler.

API adı | Azure portal adı | Notlar
-------- | ------------------- | -------------
Cihaz katılımı | Kullanım Dışı | [Kullanım dışı roller belgeleri](permissions-reference.md#deprecated-roles)
Cihaz yöneticileri | Kullanım Dışı | [Kullanım dışı roller belgeleri](permissions-reference.md#deprecated-roles)
Cihaz kullanıcıları | Kullanım Dışı | [Kullanım dışı roller belgeleri](permissions-reference.md#deprecated-roles)
Dizin eşitleme hesapları | Kullanılmaması nedeniyle gösterilmez | [Dizin eşitleme hesapları belgeleri](permissions-reference.md#directory-synchronization-accounts)
Konuk Kullanıcı | Kullanılmadığından gösterilmez | NA
İş ortağı katman 1 desteği | Kullanılmaması nedeniyle gösterilmez | [İş ortağı Katman1 Destek belgeleri](permissions-reference.md#partner-tier1-support)
İş ortağı katman 2 desteği | Kullanılmaması nedeniyle gösterilmez | [İş ortağı Katman2 Destek belgeleri](permissions-reference.md#partner-tier2-support)
Kısıtlı Konuk Kullanıcı | Kullanılmadığından gösterilmez | NA
Kullanıcı | Kullanılmadığından gösterilmez | NA
Çalışma alanına cihaz katılımı | Kullanım Dışı | [Kullanım dışı roller belgeleri](permissions-reference.md#deprecated-roles)

## <a name="password-reset-permissions"></a>Parola sıfırlama izinleri

Sütun başlıkları, parolaları sıfırlayabilirler rollerini temsil eder. Tablo satırlarında, parolasının sıfırlayabileceği roller bulunur.

Parola sıfırlanabilir | Parola Yöneticisi | Yardım Masası Yöneticisi | Kimlik doğrulama Yöneticisi | Kullanıcı Yöneticisi | Ayrıcalıklı kimlik doğrulama Yöneticisi | Genel Yönetici
------ | ------ | ------ | ------ | ------ | ------ | ------
Kimlik doğrulama Yöneticisi | &nbsp; | &nbsp; | :heavy_check_mark: | &nbsp; | :heavy_check_mark: | :heavy_check_mark:
Dizin okuyucuları | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Genel Yönetici | &nbsp; | &nbsp; | &nbsp; | &nbsp; | :heavy_check_mark: | :heavy_check_mark:\*
Gruplar Yöneticisi | &nbsp; | &nbsp; | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Konuk davetci | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Yardım Masası Yöneticisi | &nbsp; | :heavy_check_mark: | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
İleti Merkezi okuyucusu | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Parola Yöneticisi | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Ayrıcalıklı kimlik doğrulama Yöneticisi | &nbsp; | &nbsp; | &nbsp; | &nbsp; | :heavy_check_mark: | :heavy_check_mark:
Ayrıcalıklı rol yöneticisi | &nbsp; | &nbsp; | &nbsp; | &nbsp; | :heavy_check_mark: | :heavy_check_mark:
Rapor okuyucu | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Kullanıcı (yönetici rolü yok) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Kullanıcı Yöneticisi | &nbsp; | &nbsp; | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Kullanım Özeti raporları okuyucusu | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:

\* Genel yönetici kendi genel yönetici atamasını kaldıramıyor. Bu, bir kuruluşun 0 genel yöneticisi olan bir durumu önlemektir.

## <a name="next-steps"></a>Sonraki adımlar

* Bir kullanıcıyı Azure aboneliğinin Yöneticisi olarak atama hakkında daha fazla bilgi edinmek için bkz. [Azure aboneliğinin Yöneticisi olarak kullanıcı atama](../../role-based-access-control/role-assignments-portal-subscription-admin.md)
* Microsoft Azure içinde kaynak erişiminin nasıl denetlendiği hakkında daha fazla bilgi için bkz [. farklı rolleri anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Abonelikler ve bir Azure AD kiracısı arasındaki ilişki hakkında daha fazla bilgi için veya bir aboneliği ilişkilendirme veya ekleme yönergeleri için bkz. [Azure Active Directory Kiracınıza Azure aboneliği ilişkilendirme veya ekleme](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
