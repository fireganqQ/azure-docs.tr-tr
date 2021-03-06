---
title: Privileged Identity Management | Azure AD rol özellikleri | Microsoft Docs
description: Privileged Identity Management atama için Azure AD rollerini yönetme (PıM)
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 07/10/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4478c9c286c06d5d6c5593195a0e93abd286b8c
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92371520"
---
# <a name="management-capabilities-for-azure-ad-roles-in-privileged-identity-management"></a>Privileged Identity Management 'de Azure AD rolleri için yönetim özellikleri

Privileged Identity Management 'deki Azure AD rolleri için yönetim deneyimi, Azure AD rollerinin ve Azure Kaynak rollerinin nasıl yönetildiğini birleştirmek üzere güncelleştirilmiştir. Daha önce Azure Kaynak rollerinin Privileged Identity Management, Azure AD rolleri için kullanılamayan birkaç temel özellik içeriyordu.

Güncelleştirme şu anda kullanıma sunulmakta olduğundan, ikisini tek bir yönetim deneyimine birleştiriyoruz ve Azure AD rolleri için Azure Kaynak rolleri için aynı işlevselliğe sahip olursunuz. Bu makalede, güncelleştirilmiş özellikler ve tüm gereksinimler hakkında bilgilendirir.

## <a name="time-bound-assignments"></a>Zamana sınırlı atamalar

Daha önce, rol atamaları için olası iki durum vardır: *uygun* ve *kalıcı*. Artık, her atama türü için bir başlangıç ve bitiş saati de ayarlayabilirsiniz. Bu ek, size bir atama yerleştirebileceğiniz dört olası durum sağlar:

- Kalıcı olarak uygun
- Kalıcı olarak etkin
- Atama için belirtilen başlangıç ve bitiş tarihleri ile uygun
- Atama için belirtilen başlangıç ve bitiş tarihleriyle etkin

Birçok durumda, kullanıcıların her seferinde uygun atamaya ve etkinleşmesini istemediğiniz durumlarda bile, atamalar için bir süre sonu süresi ayarlayarak Azure AD kuruluşunuzu koruyabilirsiniz. Örneğin, uygun olan bazı geçici kullanıcılarınız varsa, bunların işleri tamamlandığında rol atamasından otomatik olarak kaldırmak için bir süre sonu ayarlamayı göz önünde bulundurun.

## <a name="new-role-settings"></a>Yeni rol ayarları

Azure AD rolleri için de yeni ayarlar ekledik.

- **Daha önce**, etkinleştirme ayarlarını rol başına esasına göre yalnızca yapılandırabilirsiniz. Diğer bir deyişle, çok faktörlü kimlik doğrulama gereksinimleri ve olay/istek bileti gereksinimleri gibi etkinleştirme ayarları, belirtilen bir rol için uygun olan tüm kullanıcılara uygulandı.
- **Şimdi**, bir rolün etkinleştirilebilmesi için tek bir kullanıcının çok faktörlü kimlik doğrulaması gerçekleştirmesi gerekip gerekmediğini yapılandırabilirsiniz. Ayrıca, belirli rollerle ilgili Privileged Identity Management e-postalarınız üzerinde gelişmiş denetime sahip olabilirsiniz.

## <a name="extend-and-renew-assignments"></a>Atamaları Genişlet ve Yenile

Zaman bağlantılı atama yaptığınızda, bir rolün süresi dolduğunda sorabileceğiniz ilk soru ne olur? Bu yeni sürümde, bu senaryo için iki seçenek sunuyoruz:

- **Uzat**: bir rol ataması süre sonu yaklaştığında, Kullanıcı bu rol ataması için bir uzantı istemek üzere Privileged Identity Management kullanabilir
- **Yenile**: bir rol atamasının süresi dolduğunda Kullanıcı, bu rol ataması için yenileme isteğinde bulunan Privileged Identity Management kullanabilir

Kullanıcı tarafından başlatılan eylemlerin her ikisi de genel yönetici veya ayrıcalıklı rol yöneticisinden onay gerektirir. Yöneticilerin artık bu süre sonlarını yönetme işinde ihtiyacı olmayacaktır. Yalnızca uzantı veya yenileme isteklerini beklemek ve istek geçerliyse bunları onaylaması gerekir.

## <a name="api-changes"></a>API değişiklikleri

Müşteriler, Azure AD kuruluşunun güncelleştirilmiş sürümüne sahip olduğunda, mevcut Graph API 'SI çalışmayı durdurur. [Azure Kaynak rolleri için Graph API](/graph/api/resources/privilegedidentitymanagement-resources?view=graph-rest-beta)kullanmak için geçiş yapmanız gerekir. Azure AD rollerini bu API kullanarak yönetmek için imzayla değiştirin `/azureResources` `/aadroles` ve IÇIN dizin kimliğini kullanın `resourceId` .

Önceki API 'yi kullanan tüm müşterilerine ulaşmak için en iyi zamanı öğrendiğimiz ve bu değişiklik hakkında daha fazla bilgi sahibi olmak için en iyi şekilde çalıştık. Azure AD kuruluşunuz yeni sürüme taşınmışsa ve hala eski API 'ye bağımlıysa, tarihinde ekibe ulaşın pim_preview@microsoft.com .

## <a name="powershell-change"></a>PowerShell değişikliği

Azure AD rolleri için Privileged Identity Management PowerShell modülünü kullanan müşteriler için, PowerShell güncelleştirme ile çalışmayı durdurur. Önceki cmdlet 'lerin yerine, Azure AD önizleme PowerShell modülünün içindeki Privileged Identity Management cmdlet 'lerini kullanmanız gerekir. [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.17)Azure AD PowerShell modülünü yükler. Artık [Bu PowerShell MODÜLÜNDEKI PIM işlemlerine yönelik belgeleri ve örnekleri okuyabilirsiniz](powershell-for-azure-ad-roles.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD özel rolü atama](azure-ad-custom-roles-assign.md)
- [Azure AD özel rol atamasını kaldırma veya güncelleştirme](azure-ad-custom-roles-update-remove.md)
- [Azure AD özel rol ataması yapılandırma](azure-ad-custom-roles-configure.md)
- [Azure AD 'de rol tanımları](../roles/permissions-reference.md)