---
ms.openlocfilehash: 2e92d150851c74a84f785d1f5f0ebe2e5870a54e
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97936773"
---


| Özellik |[Tüketim planı](../articles/azure-functions/consumption-plan.md)|[Premium planı](../articles/azure-functions/functions-premium-plan.md)|[Adanmış plan](../articles/azure-functions/dedicated-plan.md)|[ASE](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[Gelen IP kısıtlamaları ve özel site erişimi](../articles/azure-functions/functions-networking-options.md#inbound-access-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|✅Yes|
|[Sanal Ağ tümleştirmesi](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌No|✅Evet (bölgesel)|✅Evet (bölgesel ve ağ geçidi)|✅Yes| ✅Yes|
|[Sanal ağ Tetikleyicileri (HTTP olmayan)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌No| ✅Yes |✅Yes|✅Yes|✅Yes|
|[Karma bağlantılar](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (yalnızca Windows)|❌No|✅Yes|✅Yes|✅Yes|✅Yes|
|[Giden IP kısıtlamaları](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌No| ✅Yes|✅Yes|✅Yes|✅Yes|