---
title: Windows sanal masaüstü Kullanıcı bağlantısı gecikmesi-Azure
description: Windows sanal masaüstü kullanıcıları için bağlantı gecikme süresi.
author: Heidilohr
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 403cf584c79bc0a166054ae36c9d2ea50e4b9d9e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88008738"
---
# <a name="determine-user-connection-latency-in-windows-virtual-desktop"></a>Windows sanal masaüstü 'nde Kullanıcı bağlantı gecikmesini belirleme

Windows sanal masaüstü küresel olarak kullanılabilir. Yöneticiler, istedikleri Azure bölgesinde sanal makineler (VM) oluşturabilir. Bağlantı gecikmesi, kullanıcıların ve sanal makinelerin konumuna göre değişir. Windows Sanal Masaüstü Hizmetleri, gecikme süresini artırmak için sürekli olarak yeni coğrafi bir şekilde kullanıma sunulacaktır.

[Windows sanal masaüstü deneyimi Estimator Aracı](https://azure.microsoft.com/services/virtual-desktop/assessment/) , sanal makinelerinizin gecikmesini iyileştirmek için en iyi konumu belirlemenize yardımcı olabilir. Windows sanal masaüstü yeni alanlara kaydolurken en iyi konumun değişmediğinden emin olmak için aracı iki ile üç ayda bir kullanmanızı öneririz.

## <a name="azure-traffic-manager"></a>Azure Traffic Manager

Windows sanal masaüstü, en yakın Windows sanal masaüstü hizmeti örneğini bulmak için kullanıcının DNS sunucusunun konumunu denetleyen Azure Traffic Manager kullanır. Yöneticiler, VM 'Lerin konumunu seçmeden önce kullanıcının DNS sunucusunun konumunu incelemenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

- En iyi gecikme süresinin en iyi konumunu denetlemek için bkz. [Windows sanal masaüstü deneyimi Estimator Aracı](https://azure.microsoft.com/services/virtual-desktop/assessment/).
- Fiyatlandırma planları için bkz. [Windows sanal masaüstü fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-desktop/).
- Windows sanal masaüstü dağıtımınızı kullanmaya başlamak için [öğreticimize](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md)göz atın.