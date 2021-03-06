---
title: Azure Active Directory ile karma kimlik nedir?
description: Karma kimliğin hem şirket içinde hem de bulutta kimlik doğrulama ve yetkilendirme için ortak bir kullanıcı kimliği vardır.
keywords: Azure AD Connect’e giriş, Azure AD Connect’e genel bakış, Azure AD Connect nedir, active directory yükleme
services: active-directory
author: billmath
manager: daveba
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 05/17/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 895355b5acebffe6ad24b15b0c709d21e8f20be4
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "90016657"
---
# <a name="what-is-hybrid-identity-with-azure-active-directory"></a>Azure Active Directory ile karma kimlik nedir?

Bugün, işletmeler ve şirketler, şirket içi ve bulut uygulamalarının daha fazla ve daha fazlasını haline geliyor.  Kullanıcıların hem şirket içinde hem de bulutta bu uygulamalara erişmesi gerekir. Kullanıcıları hem şirket içinde hem de bulutta yönetmek, zorlu senaryolar doğurur. 

Microsoft 'un kimlik çözümleri, şirket içi ve bulut tabanlı olanakları kapsar.  Bu çözümler, konumdan bağımsız olarak tüm kaynaklarda kimlik doğrulaması ve yetkilendirme için ortak bir kullanıcı kimliği oluşturur. Bu **karma kimliği**çağırıyoruz.

Azure AD ve karma kimlik yönetimine yönelik karma kimlik ile bu senaryolar mümkün olur.

Azure AD ile karma kimlik elde etmek için, senaryolarınıza bağlı olarak üç kimlik doğrulama yönteminden biri kullanılabilir.   Üç yöntem şunlardır: 

- **[Parola karması eşitleme (PHS)](whatis-phs.md)**  
- **[Geçişli kimlik doğrulaması (PTA)](how-to-connect-pta.md)**  
- **[Federasyon (AD FS)](whatis-fed.md)** 

Bu kimlik doğrulama yöntemleri [Çoklu oturum açma](how-to-connect-sso.md) yeteneklerini de sağlar.  Çoklu oturum açma, şirket ağınıza bağlı olan şirket cihazlarındaki kullanıcılarınızı otomatik olarak imzalar.

Daha fazla bilgi için bkz. [Azure Active Directory karma kimlik çözümünüz için doğru kimlik doğrulama yöntemini seçme](./choose-ad-authn.md). 

## <a name="common-scenarios-and-recommendations"></a>Ortak senaryolar ve öneriler 

Aşağıda, bazı yaygın karma kimlik ve erişim yönetimi senaryoları, hangi karma kimlik seçeneğinin (veya seçeneklerinin) hangi senaryoya uygun olabileceği hakkında önerilerle birlikte verilmiştir. 

|Şunu istiyorum:|PHS ve SSO<sup>1</sup>| PTA ve SSO<sup>2</sup> | AD FS<sup>3</sup>| 
|-----|-----|-----|-----| 
|Şirket içi Active Directory ortamında oluşturulan yeni kullanıcı, kişi ve grup hesaplarının otomatik olarak bulutla eşitlenmesi.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Microsoft 365 karma senaryolar için kiracıyı ayarlayın.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kullanıcılarım 'ın şirket içi parolalarını kullanarak bulut hizmetlerine oturum açmasını ve bunlara erişmelerini etkinleştirin.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Şirket kimlik bilgilerini kullanarak çoklu oturum açma uygulayın.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| ![Önerilen](./media/whatis-hybrid-identity/ic195031.png) |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|  
|Bulutta hiçbir parola karması depolanmamış olduğundan emin olun.| |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Bulut tabanlı Multi-Factor Authentication çözümlerini etkinleştirin.|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)|![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Şirket içi Multi-Factor Authentication çözümlerini etkinleştirin.| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Kullanıcılarım için akıllı kart kimlik doğrulamasını destekler. <sup>4</sup>| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 
|Parola süre sonu bildirimlerini Office portalında ve Windows 10 Desktop 'ta görüntüleyin.| | |![Önerilen](./media/whatis-hybrid-identity/ic195031.png)| 

> <sup>1</sup> Çoklu oturum açma ile parola karması eşitleme. 
> 
> <sup>2</sup> Doğrudan kimlik doğrulama ve çoklu oturum açma.  
> 
> <sup>3</sup> AD FS ile federasyon çoklu oturum açma.  
>  
> <sup>4</sup> AD FS, kurumsal PKI çözümünüzle tümleştirilerek sertifika ile oturum açma imkanı sunulabilir. Bu sertifikalar MDM veya GPO gibi güvenilen sağlama kanalları aracılığıyla dağıtılan yazılımsal sertifikalar, akıllı kart sertifikaları (PIV/CAC kartları dahil) veya İç için Hello (sertifika güveni) olabilir. Akıllı kart kimlik doğrulaması desteği hakkında daha fazla bilgi için [bu bloga](/archive/blogs/samueld/adfs-certauth-aad-o365) bakın. 
> 

## <a name="license-requirements-for-using-azure-ad-connect"></a>Azure AD Connect kullanımı için lisans gereksinimleri

[!INCLUDE [active-directory-free-license.md](../../../includes/active-directory-free-license.md)]

## <a name="next-steps"></a>Sonraki Adımlar 

- [Azure AD Connect ve Connect Health nedir?](whatis-azure-ad-connect.md) 
- [Parola karması eşitleme (PHS) nedir?](whatis-phs.md) 
- [Geçişli kimlik doğrulaması (PTA) nedir?](how-to-connect-pta.md) 
- [Federasyon nedir?](whatis-fed.md) 
- [Çoklu oturum açma nedir?](how-to-connect-sso.md)