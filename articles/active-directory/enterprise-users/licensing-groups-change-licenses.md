---
title: Kullanıcılar ve gruplar için lisans planlarını değiştirme-Azure AD | Microsoft Docs
description: Azure Active Directory 'de grup lisanslama kullanarak bir grup içindeki kullanıcıları farklı hizmet planlarına geçirme
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: enterprise-users
ms.topic: how-to
ms.workload: identity
ms.date: 12/02/2020
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 050ae95c79e7ecb98f8508c2fdb41b90fc1b1da0
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96546548"
---
# <a name="change-license-assignments-for-a-user-or-group-in-azure-active-directory"></a>Azure Active Directory bir kullanıcı veya grup için lisans atamalarını değiştirme

Bu makalede, Azure Active Directory (Azure AD) içinde hizmet lisans planları arasında Kullanıcı ve grupların nasıl taşınacağı açıklanır. Azure AD 'nin yaklaşım hedefi, Lisans değişikliği sırasında hizmet veya veri kaybı olmamasını sağlamaktır. Kullanıcılar, hizmetler arasında sorunsuz bir şekilde geçiş yapılmalıdır. Bu makaledeki lisans planı atama adımları, Office 365 E1 'deki bir kullanıcının veya grubun Office 365 E3 'e değiştirilmesini anlatmaktadır, ancak adımlar tüm lisans planları için geçerlidir. Bir kullanıcı veya grup için lisans atamalarını güncelleştirdiğinizde, lisans atama kaldırma işlemleri ve yeni atamalar, kullanıcıların lisans değişiklikleri sırasında hizmetlerine erişimi kaybetmemesi veya planlar arasındaki lisans çakışmalarını görmek için eşzamanlı olarak yapılır.

## <a name="before-you-begin"></a>Başlamadan önce

Lisans atamalarını güncelleştirmeden önce, tüm Kullanıcı veya grupların güncelleştirilmesi için bazı varsayımlar doğru olduğundan emin olmak önemlidir. Varsayımlar bir gruptaki tüm kullanıcılar için doğru değilse, bazı durumlarda geçiş başarısız olabilir. Sonuç olarak, bazı kullanıcılar hizmetlere veya verilere erişimi kaybedebilir. Aşağıdakileri doğrulayın:

- Kullanıcılar, bir gruba atanan ve Kullanıcı tarafından devralınan ve doğrudan atanmayan geçerli lisans planına (Bu durumda, Office 365 E1) sahiptir.

- Atadığınız lisans planı için yeterli kullanılabilir lisansınız var. Yeterli lisansınız yoksa, bazı kullanıcılara yeni lisans planı atanmayabilir. Kullanılabilir lisansların sayısını kontrol edebilirsiniz.

- Kullanıcılar, istenen lisans ile çakışabilecek veya geçerli lisansın kaldırılmasını önleyen başka atanmış hizmet lisanslarına sahip değildir. Örneğin, çalışma alanı analizi veya diğer hizmetlere bağımlılığı olan Project Online gibi bir hizmetten lisanslayın.

- Grupları şirket içinde yönetebilir ve Azure AD Connect aracılığıyla Azure AD 'ye eşitletiğinizde, şirket içi sisteminizi kullanarak kullanıcıları ekleyin veya kaldırın. Değişikliklerin Azure AD ile eşitlenmesi, Grup lisanslama tarafından çekilmek için biraz zaman alabilir.

- Azure AD dinamik grup üyeliklerini kullanıyorsanız, özniteliklerini değiştirerek kullanıcıları ekler veya kaldırırsınız, ancak lisans atamaları için güncelleştirme işlemi aynı kalır.

## <a name="change-user-license-assignments"></a>Kullanıcı Lisansı atamalarını değiştirme

**Lisans atamalarını Güncelleştir** sayfasında, bazı onay kutularının kullanılamadığını görürseniz, bir grup lisansından devralındıklarından değiştirilemeyen Hizmetleri gösterir.

1. Azure AD kuruluşunuzda bir lisans yöneticisi hesabı kullanarak [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Azure Active Directory**  >  **Kullanıcılar**' ı seçin ve ardından bir kullanıcının **profil** sayfasını açın.
1. **Lisansları** seçin.
1. Kullanıcı veya grup için lisans atamasını düzenlemek üzere **atamalar** ' ı seçin. **Atamalar** sayfası, lisans atama çakışmalarını çözebileceğiniz yerdir.
1. Office 365 E3 onay kutusunu işaretleyin ve kullanıcıya atanmış tüm E1 hizmetlerinin en azından seçili olduğundan emin olun.
1. Office 365 E1 için onay kutusunu temizleyin.

    ![Office 365 E1 temizlenmiş ve Office 365 E3 seçili olduğunu gösteren bir kullanıcının lisans atamaları sayfası](./media/licensing-groups-change-licenses/update-user-license-assignments.png)

1. **Kaydet**’i seçin.

Azure AD, yeni lisansları uygular ve hizmet devamlılığını sağlamak için eski lisansları aynı anda kaldırır.

## <a name="change-group-license-assignments"></a>Grup lisansı atamalarını değiştirme

1. Azure AD kuruluşunuzda bir lisans yöneticisi hesabı kullanarak [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Azure Active Directory**  >  **grupları**' nı seçin ve ardından bir grup için **genel bakış** sayfasını açın.
1. **Lisansları** seçin.
1. Kullanıcı veya grup için lisans atamasını düzenlemek üzere **atamalar** komutunu seçin.
1. Office 365 E3 onay kutusunu seçin. Hizmetin sürekliliği devam etmek için, kullanıcıya zaten atanmış olan E1 hizmetlerini seçtiğinizden emin olun.
1. Office 365 E1 için onay kutusunu temizleyin.

    ![Kullanıcı veya grup lisansları sayfasında atamalar komutunu seçin](./media/licensing-groups-change-licenses/update-group-license-assignments.png)

1. **Kaydet**’i seçin.

Azure AD, hizmet devamlılığını sağlamak için yeni lisansları uygular ve gruptaki tüm kullanıcılar için aynı anda eski lisansları kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde gruplar aracılığıyla lisans yönetimine yönelik diğer senaryolar hakkında bilgi edinin:

- [Azure Active Directory'de gruba lisans atama](licensing-groups-assign.md)
- [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](licensing-groups-resolve-problems.md)
- [Bireysel lisanslı kullanıcıları Azure Active Directory ' de grup lisanslamaya geçirme](licensing-groups-migrate-users.md)
- [Azure Active Directory Grup lisanslama ek senaryoları](licensing-group-advanced.md)
- [Azure Active Directory 'de grup lisanslama için PowerShell örnekleri](licensing-ps-examples.md)
