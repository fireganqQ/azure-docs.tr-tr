---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/09/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 3cd3b12a22742b696b0f7b511a47d16b911a3b68
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100096520"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Yönetilen uygulama için uygulama tanımı, müşteri tarafından sağlanmış depolama hesabını kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9db7917b-1607-4e7d-a689-bca978dd0633) |Bu bir düzenleme veya uyum gereksinimidir, uygulama tanımı verilerini denetlemek için kendi depolama hesabınızı kullanın. Yönetilen uygulama tanımınızı oluşturma sırasında sizin tarafınızdan belirtilen bir depolama hesabı içinde depolamayı seçebilirsiniz; böylece konum ve erişim, mevzuat uyumluluk gereksinimlerini karşılamak üzere sizin tarafınızdan tam olarak yönetilebilir. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Managed%20Application/ApplicationDefinition_Missing_StorageAccount_Deny.json) |
|[Yönetilen bir uygulama için ilişkilendirmeleri dağıtma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F17763ad9-70c0-4794-9397-53d765932634) |Seçilen kaynak türlerini belirtilen yönetilen uygulamayla ilişkilendiren bir ilişki kaynağı dağıtır.  Bu ilke dağıtımı, iç içe geçmiş kaynak türlerini desteklemez. |deployIfNotExists |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Managed%20Application/AssociationForManagedApplication_Deploy.json) |
