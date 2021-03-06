---
title: Azure VM 'de Konuk işletim sisteminde bir güvenlik duvarı kuralını etkinleştirme veya devre dışı bırakma | Microsoft Docs
description: Uzak bir Azure VM 'de Konuk işletim sistemi güvenlik duvarı kurallarını etkinleştirmek veya devre dışı bırakmak için çevrimiçi veya çevrimdışı uzak araçları veya kayıt defteri ayarlarını nasıl kullanacağınızı öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 11/22/2018
ms.author: delhan
ms.openlocfilehash: 9ee27f429dbfd1e550a45bbc26413a1c259c4fbe
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99092377"
---
# <a name="enable-or-disable-a-firewall-rule-on-an-azure-vm-guest-os"></a>Azure VM konuk işletim sisteminde güvenlik duvarı kuralını etkinleştirin veya devre dışı bırakın

Bu makalede, Konuk işletim sistemi güvenlik duvarının bir sanal makinede (VM) kısmi trafiği filtrelerken şüphelendiğiniz bir durum sorunlarını gidermeye yönelik bir başvuru sağlanmaktadır. Bu, aşağıdaki nedenlerden dolayı yararlı olabilir:

*   Güvenlik duvarında RDP bağlantılarının başarısız olmasına neden olan bir değişiklik daha önce yapılmışsa, Özel Betik uzantısı özelliğinin kullanılması sorunu çözebilir.

*   Tüm güvenlik duvarı profillerinin devre dışı bırakılması, RDP 'ye özgü güvenlik duvarı kuralını ayarlamaktan farklı sorun gidermeye yönelik daha fazla kanıt yoludur.

## <a name="solution"></a>Çözüm

Güvenlik duvarı kurallarını yapılandırma, gereken sanal makineye erişim düzeyine bağlıdır. Aşağıdaki örnekler RDP kurallarını kullanır. Bununla birlikte, doğru kayıt defteri anahtarına işaret ederek aynı yöntemler diğer tür trafiğe de uygulanabilir.

### <a name="online-troubleshooting"></a>Çevrimiçi sorun giderme 

#### <a name="mitigation-1-custom-script-extension"></a>Hafifletme 1: özel Betik uzantısı

1.  Aşağıdaki şablonu kullanarak komut dosyanızı oluşturun.

    *   Bir kuralı etkinleştirmek için:
        ```cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=yes
        ```

    *   Bir kuralı devre dışı bırakmak için:
        ```cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=no
        ```

2.  [Özel Betik uzantısı](../extensions/custom-script-windows.md) özelliğini kullanarak bu betiği Azure Portal yükleyin. 

#### <a name="mitigation-2-remote-powershell"></a>Risk azaltma 2: uzak PowerShell

VM çevrimiçiyse ve aynı sanal ağdaki başka bir VM üzerinden erişilebiliyorsa, diğer VM 'yi kullanarak takip azaltıcı etkenleri yapabilirsiniz.

1.  Sorun giderme VM 'sinde bir PowerShell konsol penceresi açın.

2.  Aşağıdaki komutları uygun şekilde çalıştırın.

    *   Bir kuralı etkinleştirmek için:
        ```powershell
        Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
        Enable-NetFirewallRule -DisplayName  "RemoteDesktop-UserMode-In-TCP"
        exit
        ```

    *   Bir kuralı devre dışı bırakmak için:
        ```powershell
        Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
        Disable-NetFirewallRule -DisplayName  "RemoteDesktop-UserMode-In-TCP"
        exit
        ```

#### <a name="mitigation-3-pstools-commands"></a>Hafifletme 3: Padstools komutları

VM çevrimiçiyse ve aynı sanal ağdaki başka bir VM üzerinden erişilebiliyorsa, diğer VM 'yi kullanarak takip azaltıcı etkenleri yapabilirsiniz.

1.  Sorun giderme sanal makinesinde [PsTools](/sysinternals/downloads/pstools)'u indirin.

2.  Bir CMD örneği açın ve VM 'ye Iç IP (DIP) üzerinden erişin. 

    * Bir kuralı etkinleştirmek için:
        ```cmd
        psexec \\<DIP> -u <username> cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=yes
        ```

    *   Bir kuralı devre dışı bırakmak için:
        ```cmd
        psexec \\<DIP> -u <username> cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=no
        ```

#### <a name="mitigation-4-remote-registry"></a>Hafifletme 4: uzak kayıt defteri

VM çevrimiçiyse ve aynı sanal ağdaki başka bir VM üzerinden erişilebiliyorsa, diğer VM 'de [Uzak kayıt defteri](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/314837) kullanabilirsiniz.

1.  Sorun giderme sanal makinesinde, kayıt defteri Düzenleyicisi 'ni (regedit.exe) başlatın ve ardından **Dosya**  >  **Connect ağ kayıt defteri**' ni seçin.

2.  *Hedef makine*\System dalını açın ve ardından aşağıdaki değerleri belirtin:

    * Bir kuralı etkinleştirmek için aşağıdaki kayıt defteri değerini açın:
    
        *Hedef makine*\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-usermode-ın-TCP
    
        Ardından, dizedeki **etkin = FALSE değerini** **etkin = true** olarak değiştirin:

        `v2.22|Action=Allow|Active=TRUE|Dir=In|Protocol=6|Profile=Domain|Profile=Private|Profile=Public|LPort=3389|App=%SystemRoot%\system32\svchost.exe|Svc=termservice|Name=\@FirewallAPI.dll,-28775|Desc=\@FirewallAPI.dll,-28756|EmbedCtxt=\@FirewallAPI.dll,-28752|`
    
    * Bir kuralı devre dışı bırakmak için aşağıdaki kayıt defteri değerini açın:
    
        *Hedef makine*\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-usermode-ın-TCP

        Ardından, **etkin = TRUE değerini** **etkin = false** olarak değiştirin:
        
        `v2.22|Action=Allow|Active=FALSE|Dir=In|Protocol=6|Profile=Domain|Profile=Private|Profile=Public|LPort=3389|App=%SystemRoot%\system32\svchost.exe|Svc=termservice|Name=\@FirewallAPI.dll,-28775|Desc=\@FirewallAPI.dll,-28756|EmbedCtxt=\@FirewallAPI.dll,-28752|`

3.  Değişiklikleri uygulamak için VM 'yi yeniden başlatın.

### <a name="offline-troubleshooting"></a>Çevrimdışı sorun giderme 

VM 'ye herhangi bir yöntemle erişemiyorsanız, Özel Betik uzantısının kullanılması başarısız olur ve doğrudan sistem diski aracılığıyla çalışarak ÇEVRIMDıŞı modda çalışmanız gerekir.

Bu adımları izlemeden önce, etkilenen VM 'nin sistem diskinin bir anlık görüntüsünü yedek olarak alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

1.  [Sistem diskini bir kurtarma VM 'Sine bağlayın](troubleshoot-recovery-disks-portal-windows.md).

2.  Kurtarma VM 'sine bir Uzak Masaüstü bağlantısı başlatın.

3.  Diskin Disk Yönetimi konsolunda **çevrimiçi** olarak işaretlendiğinden emin olun. Eklenmiş sistem diskine atanan sürücü harfinin olduğunu unutmayın.

4.  Değişiklik yapmadan önce, değişikliklerin geri alınması gerekli olduğunda \Windows\System32\Config klasörünün bir kopyasını oluşturun.

5.  Sorun giderme VM 'sinde, kayıt defteri Düzenleyicisi 'ni (regedit.exe) başlatın.

6.  **HKEY_LOCAL_MACHINE** anahtarını vurgulayın ve menüden **Dosya**  >  **yükleme Hive** ' yi seçin.

    ![B](./media/enable-or-disable-firewall-rule-guest-os/load-registry-hive.png)

7.  \Windows\system32\config\SYSTEM dosyasını bulun ve açın. 

    > [!Note]
    > Sizden bir ad girmeniz istenir. **Brokensystem** girin ve ardından **HKEY_LOCAL_MACHINE**' ı genişletin. Şimdi, **brokensystem** adlı ek bir anahtar görürsünüz. Bu sorun giderme için, bu sorun kovanlarını **brokensystem** olarak oluşturacağız.

8.  BROKENSYSTEM dalında aşağıdaki değişiklikleri yapın:

    1.  VM 'nin başladığı **CONTROLSET** kayıt defteri anahtarını denetleyin. Anahtar numarasını, Hklm\brokensystem\select\currentkonumunda görürsünüz.

    2.  Bir kuralı etkinleştirmek için aşağıdaki kayıt defteri değerini açın:
    
        HKLM\BROKENSYSTEM\ControlSet00X\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP
        
        Ardından, **etkin = FALSE değerini** **etkin = true** olarak değiştirin.
        
        `v2.22|Action=Allow|Active=TRUE|Dir=In|Protocol=6|Profile=Domain|Profile=Private|Profile=Public|LPort=3389|App=%SystemRoot%\system32\svchost.exe|Svc=termservice|Name=\@FirewallAPI.dll,-28775|Desc=\@FirewallAPI.dll,-28756|EmbedCtxt=\@FirewallAPI.dll,-28752|`

    3.  Bir kuralı devre dışı bırakmak için aşağıdaki kayıt defteri anahtarını açın:

        HKLM\BROKENSYSTEM\ControlSet00X\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP

        Ardından, **etkin = true değerini** **etkin = false** olarak değiştirin.
        
        `v2.22|Action=Allow|Active=FALSE|Dir=In|Protocol=6|Profile=Domain|Profile=Private|Profile=Public|LPort=3389|App=%SystemRoot%\system32\svchost.exe|Svc=termservice|Name=\@FirewallAPI.dll,-28775|Desc=\@FirewallAPI.dll,-28756|EmbedCtxt=\@FirewallAPI.dll,-28752|`

9.  **Brokensystem** öğesini vurgulayın ve sonra menüden **Dosya**  >  **Kaldır Hive** öğesini seçin.

10. [Sistem diskini ayırın ve VM 'yi yeniden oluşturun](troubleshoot-recovery-disks-portal-windows.md).

11. Sorunun çözümlenip çözümlenmediğini denetleyin.
