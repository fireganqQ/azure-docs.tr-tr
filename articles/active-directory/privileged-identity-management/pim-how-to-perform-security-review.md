---
title: PıM-Azure AD 'de Azure AD rollerine erişimi gözden geçirme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içindeki Azure Active Directory rollerinin erişimini incelemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 04/24/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4dd0b63e08f44167b89b6e2d491722f3708b4036
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90055444"
---
# <a name="review-access-to-azure-ad-roles-in-privileged-identity-management"></a>Privileged Identity Management Azure AD rollerine erişimi gözden geçirin

Privileged Identity Management (PıM), kuruluşların Azure Active Directory (AD) ve Microsoft 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetler kaynaklara ayrıcalıklı erişimini nasıl yöneteceğini basitleştirir. Atanan rollerinizi başarılı bir şekilde kendinize gözden geçirmek için bu makaledeki adımları izleyin.

Bir Yönetim rolüne atandıysanız, kuruluşunuzun ayrıcalıklı rol yöneticisi, işiniz için hala bu rolün gerekli olduğunu düzenli olarak doğrulamanızı isteyebilir. Bir bağlantı içeren bir e-posta alabilir veya [Azure Portal](https://portal.azure.com) doğrudan ve başlamadan başlayabilirsiniz.

Erişim gözden geçirmeleriyle ilgilenen ayrıcalıklı bir rol yöneticisiyseniz veya küresel yöneticisiyseniz, [bir erişim incelemesi başlatma hakkında](pim-how-to-start-security-review.md)daha fazla bilgi alın.

## <a name="add-a-pim-dashboard-tile"></a>PıM panosu kutucuğu ekleme

Privileged Identity Management hizmeti Azure portal panonuza sabitlenmemişse, başlamak için bu adımları izleyin.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Azure portal sağ üst köşesindeki Kullanıcı adınızı seçin ve çalıştıracağınız Azure AD kuruluş ' u seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

## <a name="approve-or-deny-access"></a>Erişimi onayla veya Reddet

Erişimi onayladığınızda veya reddetmeniz durumunda, gözden geçireni bu rolü hâlâ kullanıp kullanmayacağınızı söylemiş olursunuz. Rol üzerinde kalmak istiyorsanız **Onayla** ' yı seçin veya artık erişime Ihtiyacınız yoksa **Reddet** ' i seçin. Gözden geçiren sonuçları uygulayaana kadar durumunuz hemen değişmez.
Erişim gözden geçirmesini bulmak ve gerçekleştirmek için şu adımları izleyin:

1. Privileged Identity Management hizmetinde, **ayrıcalıklı erişimi gözden geçir**' i seçin. Bekleyen erişim gözden geçirmeleri varsa, bunlar Azure AD **erişim İncelemeleri** sayfasında görünürler.
2. Gerçekleştirmek istediğiniz gözden geçirmeyi seçin.
3. Gözden geçirmeyi oluşturmadığınız sürece, incelemede yalnızca Kullanıcı olarak görüntülenir. Adınızın yanındaki onay işaretini seçin.
4. **Onayla** veya **Reddet**' i seçin. **Bir neden belirtin** metin kutusu içinde kararınız için bir neden eklemeniz gerekebilir.  
5. **Azure AD rollerini gözden geçir** dikey penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM hizmetinde Azure kaynak rollerimin erişim gözden geçirmesini gerçekleştirme](pim-resource-roles-perform-access-review.md)
