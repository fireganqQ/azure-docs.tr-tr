---
title: B2B için bir AD FS doğrudan Federasyonu ayarlama-Azure AD
description: Konukların Azure AD uygulamalarınızda oturum açmasını sağlamak için doğrudan Federasyon için kimlik sağlayıcısı olarak AD FS ayarlamayı öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 07/01/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b3d7c47ff0a2c533bf12a67958a913b22915f75
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87910197"
---
# <a name="example-direct-federation-with-active-directory-federation-services-ad-fs-preview"></a>Örnek: Active Directory Federasyon Hizmetleri (AD FS) ile doğrudan Federasyon (AD FS) (Önizleme)

> [!NOTE]
> Doğrudan Federasyon, Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede SAML 2,0 veya WS-Fed kimlik sağlayıcısı olarak Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanılarak [doğrudan Federasyonun](direct-federation.md) nasıl ayarlanacağı açıklanır. Doğrudan Federasyonu desteklemek için, belirli öznitelikler ve talepler kimlik sağlayıcıda yapılandırılmalıdır. Doğrudan Federasyon için bir kimlik sağlayıcısının nasıl yapılandırılacağını göstermek için, örnek olarak Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanacağız. SAML kimlik sağlayıcısı olarak ve WS-Fed kimlik sağlayıcısı olarak AD FS ayarlamayı göstereceğiz.

> [!NOTE]
> Bu makalede, çizim amacıyla hem SAML hem de WS-Fed için AD FS ayarlama açıklanır. Kimlik sağlayıcısının AD FS doğrudan Federasyon tümleştirmeleri için, protokol olarak WS-Fed kullanmanızı öneririz. 

## <a name="configure-ad-fs-for-saml-20-direct-federation"></a>SAML 2,0 Direct Federasyonu için AD FS yapılandırma
Azure AD B2B, SAML protokolünü kullanan kimlik sağlayıcılarıyla federasyona eklemek için aşağıda listelenen belirli gereksinimlere sahip olacak şekilde yapılandırılabilir. SAML yapılandırma adımlarını göstermek için, bu bölümde SAML 2,0 için AD FS ayarlama gösterilmektedir. 

Doğrudan Federasyonu ayarlamak için, kimlik sağlayıcısından SAML 2,0 yanıtında aşağıdaki öznitelikler alınmalıdır. Bu öznitelikler, çevrimiçi güvenlik belirteci hizmeti XML dosyasına bağlanarak veya el ile girilerek yapılandırılabilir. Adım 12 ' de [bir test AD FS örneği oluşturma](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) AD FS bitiş noktalarının nasıl bulunacağını veya meta veri URL 'nizin nasıl oluşturulacağını açıklar `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml` . 

|Öznitelik  |Değer  |
|---------|---------|
|AssertionConsumerService     |`https://login.microsoftonline.com/login.srf`         |
|Hedef kitle     |`urn:federation:MicrosoftOnline`         |
|Veren     |İş ortağı IDP veren URI 'SI, örneğin `http://www.example.com/exk10l6w90DHM0yi...`         |

Aşağıdaki taleplerin kimlik sağlayıcısı tarafından verilen SAML 2,0 belirtecinde yapılandırılması gerekir:


|Öznitelik  |Değer  |
|---------|---------|
|NameID biçimi     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|EmailAddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |


Sonraki bölümde, SAML 2,0 kimlik sağlayıcısı örneği olarak AD FS kullanarak gerekli özniteliklerin ve taleplerin nasıl yapılandırılacağı gösterilmektedir.

### <a name="before-you-begin"></a>Başlamadan önce

Bu yordama başlamadan önce bir AD FS sunucusu ayarlanmalıdır ve çalışır durumda olmalıdır. AD FS sunucu ayarlama konusunda yardım için bkz. [Azure sanal makinesinde test AD FS 3,0 örneği oluşturma](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed).

### <a name="add-the-claim-description"></a>Talep açıklamasını ekleyin

1. AD FS sunucunuzda **Araçlar**  >  **AD FS Yönetim**' i seçin.
2. Gezinti bölmesinde, **hizmet**  >  **talebi açıklamaları**' nı seçin.
3. **Eylemler**' ın altında **talep açıklaması ekle**' yi seçin.
4. **Talep açıklaması ekle** penceresinde, aşağıdaki değerleri belirtin:

   - **Görünen ad**: kalıcı tanımlayıcı
   - **Talep tanımlayıcısı**: `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` 
   - Bu **talep açıklamasını bu Federasyon hizmetinin kabul edebileceği bir talep türü olarak Federasyon meta verilerinde Yayımla**onay kutusunu seçin.
   - Bu **talep açıklamasını bu Federasyon Hizmeti 'nin gönderebilecek bir talep türü olarak Federasyon meta verilerinde Yayımla**onay kutusunu seçin.

5. **Tamam**’a tıklayın.

### <a name="add-the-relying-party-trust-and-claim-rules"></a>Bağlı olan taraf güvenini ve talep kurallarını ekleyin

1. AD FS sunucusunda **Araçlar**  >  **AD FS Yönetim**' e gidin.
2. Gezinti bölmesinde **güven ilişkileri**  >  **bağlı olan taraf güvenleri**' ni seçin.
3. **Eylemler**altında **bağlı olan taraf güveni Ekle**' yi seçin. 
4. **Veri kaynağı seç**için bağlı olan taraf güveni Ekleme Sihirbazı ' nda, **çevrimiçi veya yerel bir ağ üzerinde yayınlanan bağlı olan taraf hakkında verileri içeri aktarma**seçeneğini kullanın. Bu Federasyon meta veri URL 'sini belirtin- https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml . Diğer varsayılan seçimleri bırakın. **Kapat**’ı seçin.
5. **Talep kurallarını Düzenle** Sihirbazı açılır.
6. **Talep kurallarını Düzenle** sihirbazında **Kural Ekle**' yi seçin. **Kural türünü seçin**bölümünde **LDAP özniteliklerini talep olarak gönder**' i seçin. **İleri**’yi seçin.
7. **Talep Yapılandır kuralını**, aşağıdaki değerleri belirtin: 

   - **Talep kuralı adı**: e-posta talebi kuralı 
   - **Öznitelik deposu**: Active Directory 
   - **LDAP özniteliği**: E-posta adresleri 
   - **Giden talep türü**: e-posta adresi

8. **Son**’u seçin.
9. **Talep kurallarını Düzenle** penceresinde yeni kural görüntülenir. **Uygula**’ya tıklayın. 
10. **Tamam**’a tıklayın.  

### <a name="create-an-email-transform-rule"></a>E-posta dönüştürme kuralı oluşturma
1. **Talep kurallarını Düzenle** ' ye gidin ve **Kural Ekle**' ye tıklayın. **Kural türünü seçin**' de, **gelen talebi Dönüştür** ' ü seçin ve **İleri**' ye tıklayın. 
2. **Talep Yapılandır kuralını**, aşağıdaki değerleri belirtin: 

   - **Talep kuralı adı**: e-posta dönüştürme kuralı 
   - **Gelen talep türü**: e-posta adresi 
   - **Giden talep türü**: ad kimliği 
   - **Giden ad kimlik biçimi**: kalıcı tanımlayıcı 
   - **Tüm talep değerlerini geçir**’i seçin.

3. **Finish (Son)** düğmesine tıklayın. 
4. **Talep kurallarını Düzenle** penceresinde yeni kurallar gösterilir. **Uygula**’ya tıklayın. 
5. **Tamam**'a tıklayın. AD FS sunucusu artık SAML 2,0 protokolü kullanılarak doğrudan Federasyon için yapılandırılmıştır.

## <a name="configure-ad-fs-for-ws-fed-direct-federation"></a>WS-Fed Direct Federation için AD FS yapılandırma 
Azure AD B2B, aşağıda listelenen belirli gereksinimlere sahip WS-Fed protokolünü kullanan kimlik sağlayıcılarıyla federasyona eklemek üzere yapılandırılabilir. Şu anda, iki WS-Fed sağlayıcı Azure AD içerme AD FS ve Shibbomath ile uyumluluk için sınanmıştır. Burada, WS-Fed Identity sağlayıcısına örnek olarak Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanacağız. Azure AD ile WS-Fed uyumlu bir sağlayıcı arasında bağlı olan taraf güveni oluşturma hakkında daha fazla bilgi için Azure AD kimlik sağlayıcısı uyumluluk belgeleri ' ni indirin.

Doğrudan Federasyonu ayarlamak için, kimlik sağlayıcısından gelen WS-Fed iletisinde aşağıdaki öznitelikler alınmalıdır. Bu öznitelikler, çevrimiçi güvenlik belirteci hizmeti XML dosyasına bağlanarak veya el ile girilerek yapılandırılabilir. Adım 12 ' de [bir test AD FS örneği oluşturma](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) AD FS bitiş noktalarının nasıl bulunacağını veya meta veri URL 'nizin nasıl oluşturulacağını açıklar `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml` .
 
|Öznitelik  |Değer  |
|---------|---------|
|Passıverequestorendpoint     |`https://login.microsoftonline.com/login.srf`         |
|Hedef kitle     |`urn:federation:MicrosoftOnline`         |
|Veren     |İş ortağı IDP veren URI 'SI, örneğin `http://www.example.com/exk10l6w90DHM0yi...`         |

IDP tarafından verilen WS-Fed belirteci için gerekli talepler:

|Öznitelik  |Değer  |
|---------|---------|
|ImmutableID     |`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`         |
|EmailAddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

Sonraki bölümde, WS-Fed kimlik sağlayıcısı örneği olarak AD FS kullanarak gerekli özniteliklerin ve taleplerin nasıl yapılandırılacağı gösterilmektedir.

### <a name="before-you-begin"></a>Başlamadan önce
Bu yordama başlamadan önce bir AD FS sunucusu ayarlanmalıdır ve çalışır durumda olmalıdır. AD FS sunucu ayarlama konusunda yardım için bkz. [Azure sanal makinesinde test AD FS 3,0 örneği oluşturma](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed).


### <a name="add-the-relying-party-trust-and-claim-rules"></a>Bağlı olan taraf güvenini ve talep kurallarını ekleyin 
1. AD FS sunucusunda **Araçlar**  >  **AD FS Yönetim**' e gidin. 
1. Gezinti bölmesinde **güven ilişkileri**  >  **bağlı olan taraf güvenleri**' ni seçin. 
1. **Eylemler**altında **bağlı olan taraf güveni Ekle**' yi seçin.  
1. Bağlı olan taraf güveni Ekleme Sihirbazı ' nda, **veri kaynağı seç**için, **çevrimiçi veya yerel bir ağ üzerinde yayınlanan bağlı olan taraf hakkında verileri içeri aktarma**seçeneğini kullanın. Bu Federasyon meta veri URL 'sini belirtin: `https://nexus.microsoftonline-p.com/federationmetadata/2007-06/federationmetadata.xml` .  Diğer varsayılan seçimleri bırakın. **Kapat**’ı seçin.
1. **Talep kurallarını Düzenle** Sihirbazı açılır. 
1. **Talep kurallarını Düzenle** sihirbazında **Kural Ekle**' yi seçin. **Kural türü seç**' te, **talepleri özel bir kural kullanarak gönder**' i seçin. *İleri*’yi seçin. 
1. **Talep Yapılandır kuralını**, aşağıdaki değerleri belirtin:

   - **Talep kuralı adı**: sorun sabit kimliği  
   - **Özel kural**: `c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), query = "samAccountName={0};objectGUID;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);`

1. **Son**’u seçin. 
1. **Talep kurallarını Düzenle** penceresinde yeni kural görüntülenir. **Uygula**’ya tıklayın.  
1. Aynı **talep kurallarını düzenleme** sihirbazında **Kural Ekle**' yi seçin. **Cohortum kural türü**' nde **LDAP özniteliklerini talep olarak gönder**' i seçin. **İleri**’yi seçin.
1. **Talep Yapılandır kuralını**, aşağıdaki değerleri belirtin: 

   - **Talep kuralı adı**: e-posta talebi kuralı  
   - **Öznitelik deposu**: Active Directory  
   - **LDAP özniteliği**: E-posta adresleri  
   - **Giden talep türü**: e-posta adresi 

1.  **Son**’u seçin. 
1.  **Talep kurallarını Düzenle** penceresinde yeni kural görüntülenir. **Uygula**’ya tıklayın.  
1.  **Tamam**'a tıklayın. AD FS sunucusu artık WS-Beskullanan doğrudan Federasyon için yapılandırılmıştır.

## <a name="next-steps"></a>Sonraki adımlar
Bundan sonra Azure [ad 'de doğrudan Federasyonu](direct-federation.md#step-2-configure-direct-federation-in-azure-ad) Azure AD portalında veya PowerShell kullanarak yapılandırırsınız. 
