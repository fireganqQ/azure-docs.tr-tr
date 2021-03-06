---
title: Azure Güvenlik Merkezi 'ndeki izinler | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi 'nin kullanıcılara izinler atamak ve her bir rol için izin verilen eylemleri tanımlamak için rol tabanlı erişim denetimi nasıl kullanıldığı açıklanmaktadır.
services: security-center
cloud: na
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 12/01/2020
ms.author: memildin
ms.openlocfilehash: 668fe1436feeb0888f7d871144da6cb6b8e3864e
ms.sourcegitcommit: fec60094b829270387c104cc6c21257826fccc54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96922603"
---
# <a name="permissions-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde İzinler

Azure Güvenlik Merkezi, Azure 'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../role-based-access-control/built-in-roles.md) sağlayan [Azure rol tabanlı erişim denetimi 'NI (Azure RBAC)](../role-based-access-control/role-assignments-portal.md)kullanır.

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını belirlemek için kaynaklarınızın yapılandırmasını değerlendirir. Güvenlik Merkezi 'nde, yalnızca bir kaynağın ait olduğu abonelik veya kaynak grubu için sahip, katkıda bulunan veya okuyucu rolüne atadığınız zaman kaynakla ilgili bilgileri görürsünüz.

Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

* **Güvenlik okuyucusu**: Bu role ait olan bir Kullanıcı, güvenlik merkezi 'ne yönelik hakları görüntülüyor. Kullanıcı önerileri, uyarıları, güvenlik ilkesini ve güvenlik durumlarını görüntüleyebilir, ancak değişiklik yapamaz.
* **Güvenlik Yöneticisi**: Bu role ait olan bir Kullanıcı güvenlik okuyucuyla aynı haklara sahiptir ve ayrıca güvenlik ilkesini güncelleştirebilir ve uyarıları ve önerileri kapatabilir.

> [!NOTE]
> Güvenlik, güvenlik okuyucusu ve Güvenlik Yöneticisi güvenlik rollerinin yalnızca güvenlik merkezi 'ne erişimi vardır. Güvenlik rollerinin, Azure 'un depolama, Web & Mobile veya Nesnelerin İnterneti gibi diğer hizmet bölümlerine erişimi yoktur.
>

## <a name="roles-and-allowed-actions"></a>Roller ve izin verilen eylemler

Aşağıdaki tabloda Güvenlik Merkezi 'nde roller ve izin verilen eylemler görüntülenmektedir.

|Eylem|Güvenlik okuyucusu/ <br> Okuyucu |Güvenlik Yöneticisi  |Kaynak grubu Katılımcısı/ <br> Kaynak grubu sahibi  |Abonelik Katılımcısı  |Abonelik sahibi  |
|:--- |:---:|:---:|:---:|:---:|:---:|
|Güvenlik ilkesini Düzenle|-|✔|-|-|✔|
|Ekleme/atama (yasal uyumluluk standartları dahil)|-|-|-|-|✔|
|Azure Defender 'ı etkinleştirme/devre dışı bırakma|-|✔|-|-|✔|
|Otomatik sağlamayı etkinleştir/devre dışı bırak|-|✔|-|✔|✔|
|Bir kaynak için güvenlik önerilerini uygulama</br> (ve [hızlı düzelme kullanın!](security-center-remediate-recommendations.md#quick-fix-remediation))|-|-|✔|✔|✔|
|Uyarıları kapat|-|✔|-|✔|✔|
|Uyarıları ve önerileri görüntüleme|✔|✔|✔|✔|✔|

> [!NOTE]
> Kullanıcılara, görevlerini tamamlamak için gereken rolleri en alt seviyede esneklik sunacak şekilde atamanızı öneririz. Örneğin, okuyucu rolünü yalnızca bir kaynağın güvenlik durumu ile ilgili bilgileri görüntülemesi gereken ancak öneri veya ilkeleri Düzenle gibi işlemleri olmayan kullanıcılara atayın.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, güvenlik merkezi 'nin kullanıcılara izin atamak ve her bir rol için izin verilen eylemleri tanımlanması için Azure RBAC nasıl kullandığı açıklanmaktadır. Artık aboneliğinizin güvenlik durumunu izlemek, güvenlik ilkelerini düzenlemek ve öneriler uygulamak için gereken rol atamalarıyla ilgili bilgi sahibi olduğunuza göre şunları yapın:

- [Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md)
- [Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md)
- [Azure kaynaklarınızın güvenlik durumunu izleme](security-center-monitoring.md)
- [Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
- [İş ortağı güvenlik çözümlerini izleme](./security-center-partner-integration.md)