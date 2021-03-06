---
title: PıM-Azure AD 'de Azure AD rolleri için erişim gözden geçirmesi oluşturma | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rolleri için erişim gözden geçirmesi oluşturmayı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 10/22/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d9fdc44681c8773d7b6f724174a74e76e57939e
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92369769"
---
# <a name="create-an-access-review-of-azure-ad-roles-in-privileged-identity-management"></a>Privileged Identity Management Azure AD rolleri için erişim gözden geçirmesi oluşturma

Eski rol atamalarıyla ilişkili riski azaltmak için, erişimi düzenli olarak gözden geçirmeniz gerekir. Ayrıcalıklı Azure AD rolleri için erişim gözden geçirmeleri oluşturmak üzere Azure AD Privileged Identity Management (PıM) kullanabilirsiniz. Ayrıca, otomatik olarak gerçekleşen yinelenen erişim incelemelerini yapılandırabilirsiniz.

Bu makalede, ayrıcalıklı Azure AD rolleri için bir veya daha fazla erişim incelemesi oluşturma açıklanır.

## <a name="prerequisites"></a>Ön koşullar

[Ayrıcalıklı rol yöneticisi](../roles/permissions-reference.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Açık erişim İncelemeleri

1. Ayrıcalıklı rol yöneticisi rolünün üyesi olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ni seçin.

1. Yönet altında, **erişim gözden geçirmeleri**' nı seçin ve ardından **Yeni**' yi seçin.

    ![Azure AD rolleri-tüm incelemeler durumunu gösteren erişim gözden geçirmeleri listesi](./media/pim-how-to-start-security-review/access-reviews.png)

[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]

## <a name="start-the-access-review"></a>Erişim gözden geçirmesini Başlat

Erişim gözden geçirmesi ayarlarını belirttikten sonra **Başlat**' ı seçin. Erişim incelemesi, listenizde durumunun bir göstergesi olacak şekilde görünür.

![Başlangıç incelemelerinin durumunu gösteren erişim gözden geçirmeleri listesi](./media/pim-how-to-start-security-review/access-reviews-list.png)

Varsayılan olarak, Azure AD gözden geçirenlere İnceleme başladıktan kısa bir bir e-posta gönderir. Azure AD 'ye e-posta gönderme seçeneğini belirlerseniz, gözden geçirenlere bir erişim incelemesinin tamamlanmasını beklediğini bildirin. [Azure AD rollerine erişimin nasıl incelemesinin](pim-how-to-perform-security-review.md)yapılacağı yönergeleri gösterebilirsiniz.

## <a name="manage-the-access-review"></a>Erişim gözden geçirmesini yönetme

Gözden geçirenler, erişim incelemesinin **genel bakış** sayfasında İncelemeleri tamamlamalarını izleyerek ilerlemeyi izleyebilirsiniz. [İnceleme tamamlanana](pim-how-to-complete-review.md)kadar dizinde erişim hakkı değiştirilmez.

![İnceleme ayrıntılarını gösteren erişim gözden geçirmeleri genel bakış sayfası](./media/pim-how-to-start-security-review/access-review-overview.png)

Bu bir kerelik bir gözden geçirime sahip olursa, erişim gözden geçirme süresi dolduktan sonra veya yönetici erişim gözden geçirmesini durdurduktan sonra, sonuçları görmek ve uygulamak için [Azure AD rollerinin erişim gözden geçirmesini tamamlayın](pim-how-to-complete-review.md) ' daki adımları izleyin.  

Bir dizi erişim incelemesini yönetmek için, erişim gözden geçirmelerine gidin ve zamanlanan incelemelerde yaklaşan örnekleri bulacaksınız ve son tarihi düzenleyin ya da gözden geçirenleri buna uygun olarak ekleyin/kaldırın.

**Tamamlanma ayarları sırasında**yaptığınız seçimlere bağlı olarak, otomatik uygulama İnceleme bitiş tarihinden sonra veya gözden geçirmeyi el ile durdurduğunuzda yürütülür. İncelemesinin durumu, **uygulama** ve son durum **uygulandı**gibi ara durumlar aracılığıyla **tamamlandı** olarak değişir. Birkaç dakika içinde rollerden kaldırılan reddedilen kullanıcıları görmeyi beklemelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD rollerine erişimi gözden geçirme](pim-how-to-perform-security-review.md)
- [Azure AD rollerinin erişim incelemesini tamamlar](pim-how-to-complete-review.md)
- [Azure Kaynak rolleri için erişim gözden geçirmesi oluşturma](pim-resource-roles-start-access-review.md)
