---
title: Privileged Identity Management ile ilgili sorun giderme-Azure Active Directory | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içindeki rollerle sistem hatalarını nasıl giderebileceğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 10/18/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: dcbebcb32e912abdf0112007c743c6890fae36e4
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92372404"
---
# <a name="troubleshoot-a-problem-with-privileged-identity-management"></a>Privileged Identity Management sorun giderme

Azure Active Directory (Azure AD) içinde Privileged Identity Management (PıM) ile ilgili bir sorun var mı? Aşağıdaki bilgiler, yeniden çalışan işlemleri almanıza yardımcı olabilir.

## <a name="access-to-azure-resources-denied"></a>Azure kaynaklarına erişim reddedildi

### <a name="problem"></a>Sorun

Bir Azure kaynağı için etkin bir sahip veya Kullanıcı erişimi Yöneticisi olarak, kaynağınızı Privileged Identity Management içinde görebilir, ancak uygun bir atama yapma veya kaynağa genel bakış sayfasından rol atamalarının bir listesini görüntüleme gibi işlemleri gerçekleştiremeyebilirsiniz. Bu eylemlerin herhangi biri bir yetkilendirme hatasına neden olur.

### <a name="cause"></a>Nedeni

Bu sorun, PıM hizmeti sorumlusu için Kullanıcı erişimi yönetici rolü yanlışlıkla aboneliğinden kaldırıldığında meydana gelebilir. Privileged Identity Management hizmetinin Azure kaynaklarına erişebilmesi için, MS-PıM hizmet sorumlusuna her zaman Azure aboneliği üzerinden [Kullanıcı erişimi yönetici rolü](../../role-based-access-control/built-in-roles.md#user-access-administrator) atanmalıdır.

### <a name="resolution"></a>Çözüm

Kullanıcı erişimi Yöneticisi rolünü, abonelik düzeyinde ayrıcalıklı kimlik yönetimi hizmet asıl adına (MS – PıM) atayın. Bu atama, ayrıcalıklı kimlik yönetimi hizmetinin Azure kaynaklarına erişmesine izin verilmelidir. Rol, gereksinimlerinize bağlı olarak bir yönetim grubu düzeyinde veya abonelik düzeyinde atanabilir. Daha fazla bilgi hizmet sorumlusu için bkz. [bir role uygulama atama](../develop/howto-create-service-principal-portal.md#assign-a-role-to-the-application).

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management kullanılacak lisans gereksinimleri](subscription-requirements.md)
- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](../roles/security-planning.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
- [Privileged Identity Management'ı dağıtma](pim-deployment-plan.md)