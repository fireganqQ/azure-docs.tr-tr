---
title: NSG 'de RDP bağlantı noktası etkinleştirilmediği için Azure VM 'lerine bağlanılamıyor | Microsoft Docs
description: Azure portal NSG yapılandırması nedeniyle RDP 'nin başarısız olduğu bir sorunu nasıl giderebileceğinizi öğrenin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: v-jesits
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 878e2c233f2171c3c9a6fbd2a8d629d3f3987c3a
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91976734"
---
#  <a name="cannot-connect-remotely-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>NSG 'de RDP bağlantı noktası etkinleştirilmediği için bir VM 'ye uzaktan bağlanılamıyor

Bu makalede, ağ güvenlik grubunda (NSG) Uzak Masaüstü Protokolü (RDP) bağlantı noktası etkinleştirilmediği için bir Azure Windows sanal makinesine (VM) bağlanamadaki bir sorunu nasıl giderebileceğiniz açıklanır.


## <a name="symptom"></a>Belirti

RDP bağlantı noktası ağ güvenlik grubunda açılmadığından, Azure 'daki bir VM 'ye RDP bağlantısı yapamazsınız.

## <a name="solution"></a>Çözüm 

Yeni bir VM oluşturduğunuzda, Internet 'ten gelen tüm trafik varsayılan olarak engellenir. 

Bir NSG 'de RDP bağlantı noktasını etkinleştirmek için şu adımları izleyin:
1. [Azure Portal](https://portal.azure.com)oturum açın.
2. **Sanal makinelerde**, sorunu olan VM 'yi seçin. 
3. **Ayarlar**' da **ağ**' ı seçin. 
4. **Gelen bağlantı noktası kurallarında**, RDP bağlantı noktasının doğru şekilde ayarlandığından emin olun. Yapılandırmaya bir örnek aşağıda verilmiştir: 

    **Öncelik**: 300 </br>
    **Ad**: Port_3389 </br>
    **Bağlantı noktası (hedef)**: 3389 </br>
    **Protokol**: TCP </br>
    **Kaynak**: any </br>
    **Hedefler**: any </br>
    **Eylem**: izin ver </br>

Kaynak IP adresini belirtirseniz, bu ayar VM 'ye bağlanmak için yalnızca belirli bir IP adresinden veya IP adresi aralığından gelen trafiğe izin verir. RDP oturumunu başlatmak için kullandığınız bilgisayarın aralığın içinde olduğundan emin olun.

NSG 'ler hakkında daha fazla bilgi için bkz. [ağ güvenlik grubu](../../virtual-network/network-security-groups-overview.md).

> [!NOTE]
> RDP bağlantı noktası 3389, Internet 'e açıktır. Bu nedenle, bu bağlantı noktasını yalnızca test için önerilen için kullanmanızı öneririz. Üretim ortamları için bir VPN veya özel bağlantı kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

NSG 'de RDP bağlantı noktası zaten etkinse bkz. [Azure VM 'de RDP genel hatası giderme](./troubleshoot-rdp-general-error.md).