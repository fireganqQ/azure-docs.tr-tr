---
title: Azure 'da Windows VM 'sine RDP ile bağlanamaz | Microsoft Docs
description: Azure 'da uzak masaüstü 'Nü kullanarak Windows sanal makinenize bağlanamadığınızda oluşan sorunları giderin
keywords: Uzak Masaüstü hatası, Uzak Masaüstü bağlantı hatası, VM 'ye bağlanılamıyor, Uzak Masaüstü sorunlarını giderme
services: virtual-machines-windows
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 0d740f8e-98b8-4e55-bb02-520f604f5b18
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/23/2018
ms.author: akjosh
ms.openlocfilehash: 33bd7c82568f836e498a05411d2b3cf1c6a950b9
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98878638"
---
# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Azure sanal makinesine yönelik uzak masaüstü bağlantılarında sorun giderme
Windows tabanlı Azure sanal makinenize (VM) olan Uzak Masaüstü Protokolü (RDP) bağlantısı çeşitli sebeplerle başarısız olabilir ve VM'nize erişememenize yol açabilir. Bu sorun VM'deki Uzak Masaüstü hizmetinden, ağ bağlantısından veya ana bilgisayarınızdaki Uzak Masaüstü istemcisinden kaynaklanabilir. Bu makale RDP bağlantı sorunlarını çözmeye yönelik en yaygın yöntemlerin bazılarında size yol gösterir. 

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.

 

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Hızlı sorun giderme adımları
Her bir sorun giderme adımından sonra, sanal makineye yeniden bağlanmayı deneyin:

1. Uzak Masaüstü yapılandırmasını sıfırlayın.
2. Ağ güvenlik grubu kurallarını/Cloud Services uç noktalarını denetleyin.
3. VM konsol günlüklerini gözden geçirin.
4. VM için NIC 'ı sıfırlayın.
5. VM Kaynak Durumu denetleyin.
6. VM parolanızı sıfırlayın.
7. Sanal makineyi yeniden başlatın.
8. Sanal makineyi yeniden dağıtın.

Daha ayrıntılı adımlar ve açıklamalar gerekiyorsa okumaya devam edin. Yönlendiriciler ve güvenlik duvarları gibi yerel ağ donanımının, [AYRıNTıLı RDP sorun giderme senaryolarında](detailed-troubleshoot-rdp.md)belirtildiği gıbı giden TCP bağlantı noktası 3389 ' i engellemediğinden emin olun.

> [!TIP]
> VM 'niz için **Bağlan** düğmesi portalda gri Ise ve Azure 'A bir [Express Route](../../expressroute/expressroute-introduction.md) veya [siteden siteye VPN](../../vpn-gateway/tutorial-site-to-site-portal.md) bağlantısı aracılığıyla bağlı DEĞILSENIZ, RDP 'YI kullanabilmeniz için sanal makinenizin genel IP adresini oluşturmanız ve atamanız gerekir. [Azure’da genel IP adresleri](../../virtual-network/public-ip-addresses.md) hakkında daha fazlasını okuyabilirsiniz.


## <a name="ways-to-troubleshoot-rdp-issues"></a>RDP sorunlarını gidermeye yönelik yollar
Aşağıdaki yöntemlerden birini kullanarak Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan sanal makinelerin sorunlarını giderebilirsiniz:

* Azure portal-RDP yapılandırmasını veya Kullanıcı kimlik bilgilerini hızlıca sıfırlamanız gerekiyorsa ve Azure Araçları yüklü değilse harika.
* Azure PowerShell-bir PowerShell istemiyle rahat bir şekilde karşılaşırsanız, Azure PowerShell cmdlet 'lerini kullanarak RDP yapılandırmasını veya Kullanıcı kimlik bilgilerini hızlıca sıfırlayın.

[Klasik dağıtım modeli](#troubleshoot-vms-created-using-the-classic-deployment-model)kullanılarak oluşturulan VM 'lerde sorun giderme adımlarını da bulabilirsiniz.

<a id="fix-common-remote-desktop-errors"></a>

## <a name="troubleshoot-using-the-azure-portal"></a>Azure portal kullanarak sorun giderme
Her bir sorun giderme adımından sonra, sanal makinenize yeniden bağlanmayı deneyin. Hala bağlanamıyorsanız, bir sonraki adımı deneyin.

1. **RDP bağlantınızı sıfırlayın**. Bu sorun giderme adımı, uzaktan bağlantılar devre dışı bırakıldığında veya Windows güvenlik duvarı kuralları RDP 'yi engellediği zaman RDP yapılandırmasını sıfırlar.
   
    Azure portal VM 'nizi seçin. Ayarlar bölmesini listenin en altında bulunan **destek + sorun giderme** bölümüne kaydırın. **Parolayı Sıfırla** düğmesine tıklayın. **Modu** **yalnızca yapılandırmayı Sıfırla** olarak ayarlayın ve ardından **Güncelleştir** düğmesine tıklayın:
   
    ![Azure portal R D P yapılandırmasını sıfırlayın.](./media/troubleshoot-rdp-connection/reset-rdp.png)
2. **Ağ güvenlik grubu kurallarını doğrulayın**. Bir ağ güvenlik grubundaki bir kuralın bir sanal makineden gelen veya giden trafiği engelleyip engellemediğini onaylamak için [IP akışı doğrulama](../../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) kullanın. Ayrıca, gelen "Izin ver" NSG kuralının mevcut olduğundan ve RDP bağlantı noktası (varsayılan 3389) için önceliklendirildiğinden emin olmak için etkin güvenlik grubu kurallarını gözden geçirebilirsiniz. Daha fazla bilgi için bkz. [sanal makine trafiği akışı sorunlarını gidermek Için etkin güvenlik kurallarını kullanma](../../virtual-network/diagnose-network-traffic-filter-problem.md).

3. **VM önyüklemesi tanılamayı gözden geçirin**. Bu sorun giderme adımı, VM 'nin bir sorunu bildirmekte olup olmadığını anlamak için VM konsol günlüklerini inceler. Tüm VM 'Lerde önyükleme tanılaması etkin değildir, bu nedenle bu sorun giderme adımı isteğe bağlı olabilir.
   
    Belirli sorun giderme adımları Bu makalenin kapsamı dışındadır, ancak RDP bağlantısını etkileyen daha geniş bir sorun olduğunu gösteriyor olabilir. Konsol günlüklerini ve VM ekran görüntüsünü İnceleme hakkında daha fazla bilgi için bkz. [VM 'ler Için önyükleme tanılaması](boot-diagnostics.md).

4. **VM için NIC 'ı sıfırlayın**. Daha fazla bilgi için bkz. [Azure WINDOWS VM IÇIN NIC sıfırlama](./reset-network-interface.md).
5. **VM kaynak durumu denetleyin**. Bu sorun giderme adımı, Azure platformunda VM bağlantısını etkileyebilecek bilinen bir sorun olmadığını doğrular.
   
    Azure portal VM 'nizi seçin. Ayarlar bölmesini listenin en altında bulunan **destek + sorun giderme** bölümüne kaydırın. **Kaynak durumu** düğmesine tıklayın. Sağlıklı bir VM 'nin **kullanılabilir** olduğu şekilde raporları:
   
    ![Azure portal V d kaynak durumunu denetleyin.](./media/troubleshoot-rdp-connection/check-resource-health.png)
6. **Kullanıcı kimlik bilgilerini sıfırlayın**. Bu sorun giderme adımı, bir yerel yönetici hesabındaki parolayı, kimlik bilgilerini bilmiyorsanız veya unuttuyordu.  VM 'de oturum açtıktan sonra, bu kullanıcının parolasını sıfırlamalısınız.
   
    Azure portal VM 'nizi seçin. Ayarlar bölmesini listenin en altında bulunan **destek + sorun giderme** bölümüne kaydırın. **Parolayı Sıfırla** düğmesine tıklayın. **Modunun** **Parolayı Sıfırla** olarak ayarlandığından emin olun ve ardından Kullanıcı adınızı ve yeni bir parolayı girin. Son olarak, **Güncelleştir** düğmesine tıklayın:
   
    ![Azure portal Kullanıcı kimlik bilgilerini sıfırlayın.](./media/troubleshoot-rdp-connection/reset-password.png)
7. **Sanal makineyi yeniden başlatın**. Bu sorun giderme adımı, VM 'nin sahip olduğu temeldeki sorunları düzeltebilir.
   
    Azure portal VM 'nizi seçin ve **genel bakış** sekmesine tıklayın. **Yeniden Başlat** düğmesine tıklayın:
   
    ![Azure portal V d 'yi yeniden başlatın.](./media/troubleshoot-rdp-connection/restart-vm.png)
8. **Sanal makineyi yeniden dağıtın**. Bu sorun giderme adımı, temel alınan platformu veya ağ sorunlarını düzeltmek için VM 'nizi Azure 'daki başka bir konağa yeniden dağıtır.
   
    Azure portal VM 'nizi seçin. Ayarlar bölmesini listenin en altında bulunan **destek + sorun giderme** bölümüne kaydırın. Yeniden **Dağıt** düğmesine tıklayın ve ardından yeniden **Dağıt**' a tıklayın:
   
    ![Azure portal sanal makineyi yeniden dağıtın](./media/troubleshoot-rdp-connection/redeploy-vm.png)
   
    Bu işlem tamamlandıktan sonra, kısa ömürlü disk verileri kaybedilir ve VM ile ilişkili dinamik IP adresleri güncellenir.

9. **Yönlendirmeyi doğrulayın**. Bir yolun bir sanal makineye veya bir sanal makineye yönlendirilmesini engellemediğini doğrulamak için ağ Izleyicisi 'nin [sonraki atlama](../../network-watcher/diagnose-vm-network-routing-problem.md) özelliğini kullanın. Ayrıca, bir ağ arabirimi için tüm etkin yolları görmek üzere geçerli yolları gözden geçirebilirsiniz. Daha fazla bilgi için bkz. [VM trafik akışı sorunlarını gidermek için geçerli yolları kullanma](../../virtual-network/diagnose-network-routing-problem.md).

10. Bilgisayarınızda herhangi bir şirket içi güvenlik duvarının veya güvenlik duvarının Azure 'a giden TCP 3389 trafiğine izin verdiğinden emin olun.

Hala RDP sorunlarıyla karşılaşdıysanız [bir destek isteği açabilir](https://azure.microsoft.com/support/options/) veya [daha ayrıntılı RDP sorun giderme kavramlarını ve adımlarını](detailed-troubleshoot-rdp.md)okuyabilirsiniz.

## <a name="troubleshoot-using-azure-powershell"></a>Azure PowerShell'i kullanarak sorun giderme
Henüz yapmadıysanız, [en son Azure PowerShell yükleyip yapılandırın](/powershell/azure/).

Aşağıdaki örnekler,, ve gibi değişkenleri `myResourceGroup` kullanır `myVM` `myVMAccessExtension` . Bu değişken adlarını ve konumlarını kendi değerlerinizle değiştirin.

> [!NOTE]
> [Set-Azvmaccessextenma](/powershell/module/az.compute/set-azvmaccessextension) PowerShell cmdlet 'ini kullanarak Kullanıcı kimlik BILGILERINI ve RDP yapılandırmasını sıfırladınız. Aşağıdaki örneklerde, `myVMAccessExtension` işlemin bir parçası olarak belirttiğiniz bir addır. Daha önce VMAccessAgent ile çalıştıysanız, `Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"` sanal makinenin özelliklerini denetlemek için kullanarak var olan uzantının adını alabilirsiniz. Adı görüntülemek için çıktının ' Uzantılar ' bölümüne bakın.

Her bir sorun giderme adımından sonra, sanal makinenize yeniden bağlanmayı deneyin. Hala bağlanamıyorsanız, bir sonraki adımı deneyin.

1. **RDP bağlantınızı sıfırlayın**. Bu sorun giderme adımı, uzaktan bağlantılar devre dışı bırakıldığında veya Windows güvenlik duvarı kuralları RDP 'yi engellediği zaman RDP yapılandırmasını sıfırlar.
   
    Aşağıdaki örnek, `myVM` `WestUS` konumunda ve adlı kaynak grubunda ADLı bir VM 'deki RDP bağlantısını sıfırlar `myResourceGroup` :
   
    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```
2. **Ağ güvenlik grubu kurallarını doğrulayın**. Bu sorun giderme adımı, RDP trafiğine izin vermek için ağ güvenlik grubunuzda bir kuralınız olduğunu doğrular. RDP için varsayılan bağlantı noktası, TCP bağlantı noktası 3389 ' dir. VM 'nizi oluşturduğunuzda RDP trafiğine izin veren bir kural otomatik olarak oluşturulamaz.
   
    İlk olarak, ağ güvenlik grubunuzun tüm yapılandırma verilerini `$rules` değişkenine atayın. Aşağıdaki örnek, adlı kaynak grubunda adlı ağ güvenlik grubu hakkında bilgi edinir `myNetworkSecurityGroup` `myResourceGroup` :
   
    ```powershell
    $rules = Get-AzNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```
   
    Şimdi bu ağ güvenlik grubu için yapılandırılmış kuralları görüntüleyin. Gelen bağlantılar için TCP bağlantı noktası 3389 ' e izin vermek için bir kuralın mevcut olduğunu aşağıdaki gibi doğrulayın:
   
    ```powershell
    $rules.SecurityRules
    ```
   
    Aşağıdaki örnek, RDP trafiğine izin veren geçerli bir güvenlik kuralını gösterir. ,,, `Protocol` `DestinationPortRange` `Access` Ve `Direction` doğru şekilde yapılandırıldığını görebilirsiniz:
   
    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```
   
    RDP trafiğine izin veren bir kuralınız yoksa [bir ağ güvenlik grubu kuralı oluşturun](../windows/nsg-quickstart-powershell.md). TCP bağlantı noktası 3389 ' ye izin verin.
3. **Kullanıcı kimlik bilgilerini sıfırlayın**. Bu sorun giderme adımı, kimlik bilgilerini bilmiyorsanız veya unutdığınızda belirttiğiniz yerel yönetici hesabındaki parolayı sıfırlar.
   
    İlk olarak, aşağıdaki gibi, değişkenine kimlik bilgileri atayarak Kullanıcı adını ve yeni bir parolayı belirtin `$cred` :
   
    ```powershell
    $cred=Get-Credential
    ```
   
    Şimdi, sanal makinenizin kimlik bilgilerini güncelleştirin. Aşağıdaki örnek, `myVM` `WestUS` konumunda ve adlı kaynak grubunda ADLı bir sanal makinede bulunan kimlik bilgilerini güncelleştirir `myResourceGroup` :
   
    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```
4. **Sanal makineyi yeniden başlatın**. Bu sorun giderme adımı, VM 'nin sahip olduğu temeldeki sorunları düzeltebilir.
   
    Aşağıdaki örnek, adlı kaynak grubunda adlı VM 'yi yeniden başlatır `myVM` `myResourceGroup` :
   
    ```powershell
    Restart-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```
5. **Sanal makineyi yeniden dağıtın**. Bu sorun giderme adımı, temel alınan platformu veya ağ sorunlarını düzeltmek için VM 'nizi Azure 'daki başka bir konağa yeniden dağıtır.
   
    Aşağıdaki örnek, `myVM` `WestUS` konumunda ve adlı kaynak grubunda adlı sanal makineyi yeniden dağıtır `myResourceGroup` :
   
    ```powershell
    Set-AzVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

6. **Yönlendirmeyi doğrulayın**. Bir yolun bir sanal makineye veya bir sanal makineye yönlendirilmesini engellemediğini doğrulamak için ağ Izleyicisi 'nin [sonraki atlama](../../network-watcher/diagnose-vm-network-routing-problem.md) özelliğini kullanın. Ayrıca, bir ağ arabirimi için tüm etkin yolları görmek üzere geçerli yolları gözden geçirebilirsiniz. Daha fazla bilgi için bkz. [VM trafik akışı sorunlarını gidermek için geçerli yolları kullanma](../../virtual-network/diagnose-network-routing-problem.md).

7. Bilgisayarınızda herhangi bir şirket içi güvenlik duvarının veya güvenlik duvarının Azure 'a giden TCP 3389 trafiğine izin verdiğinden emin olun.

Hala RDP sorunlarıyla karşılaşdıysanız [bir destek isteği açabilir](https://azure.microsoft.com/support/options/) veya [daha ayrıntılı RDP sorun giderme kavramlarını ve adımlarını](detailed-troubleshoot-rdp.md)okuyabilirsiniz.

## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Klasik dağıtım modeli kullanılarak oluşturulan sanal makinelerin sorunlarını giderme

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]


Her bir sorun giderme adımından sonra sanal makineye yeniden bağlanmayı deneyin.

1. **RDP bağlantınızı sıfırlayın**. Bu sorun giderme adımı, uzaktan bağlantılar devre dışı bırakıldığında veya Windows güvenlik duvarı kuralları RDP 'yi engellediği zaman RDP yapılandırmasını sıfırlar.
   
    Azure portal VM 'nizi seçin. ... Öğesine tıklayın **. Daha fazla** düğme ve sonra **Uzaktan erişimi Sıfırla**' ya tıklayın:
   
    ![Klasik dağıtım modelini kullanarak V M için Azure portal R D P yapılandırmasını sıfırlayın.](./media/troubleshoot-rdp-connection/classic-reset-rdp.png)
2. **Cloud Services uç noktalarını doğrulayın**. Bu sorun giderme adımı, Cloud Services RDP trafiğine izin vermek için uç noktalarınızın olduğunu doğrular. RDP için varsayılan bağlantı noktası, TCP bağlantı noktası 3389 ' dir. VM 'nizi oluşturduğunuzda RDP trafiğine izin veren bir kural otomatik olarak oluşturulamaz.
   
   Azure portal VM 'nizi seçin. VM 'niz için yapılandırılmış olan uç noktaları görüntülemek için **uç noktalar** düğmesine tıklayın. TCP bağlantı noktası 3389 ' de RDP trafiğine izin veren uç noktaların mevcut olduğunu doğrulayın.
   
   Aşağıdaki örnekte, RDP trafiğine izin veren geçerli uç noktalar gösterilmektedir:
   
   ![Klasik dağıtım modelini kullanarak V d için Azure portal Cloud Services uç noktalarını doğrulayın.](./media/troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)
   
   RDP trafiğine izin veren bir uç noktanız yoksa [Cloud Services uç noktası oluşturun](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints). TCP 'nin özel bağlantı noktası 3389 ' e izin verin.
3. **VM önyüklemesi tanılamayı gözden geçirin**. Bu sorun giderme adımı, VM 'nin bir sorunu bildirmekte olup olmadığını anlamak için VM konsol günlüklerini inceler. Tüm VM 'Lerde önyükleme tanılaması etkin değildir, bu nedenle bu sorun giderme adımı isteğe bağlı olabilir.
   
    Belirli sorun giderme adımları Bu makalenin kapsamı dışındadır, ancak RDP bağlantısını etkileyen daha geniş bir sorun olduğunu gösteriyor olabilir. Konsol günlüklerini ve VM ekran görüntüsünü İnceleme hakkında daha fazla bilgi için bkz. [VM 'ler Için önyükleme tanılaması](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).
4. **VM kaynak durumu denetleyin**. Bu sorun giderme adımı, Azure platformunda VM bağlantısını etkileyebilecek bilinen bir sorun olmadığını doğrular.
   
    Azure portal VM 'nizi seçin. Ayarlar bölmesini listenin en altında bulunan **destek + sorun giderme** bölümüne kaydırın. **Kaynak durumu** düğmesine tıklayın. Sağlıklı bir VM 'nin **kullanılabilir** olduğu şekilde raporları:
   
    ![Klasik dağıtım modelini kullanarak V için Azure portal V d kaynak durumunu denetleyin.](./media/troubleshoot-rdp-connection/classic-check-resource-health.png)
5. **Kullanıcı kimlik bilgilerini sıfırlayın**. Bu sorun giderme adımı, kimlik bilgilerini bilmiyorsanız veya unuttuysanız, belirttiğiniz yerel yönetici hesabındaki parolayı sıfırlar.  VM 'de oturum açtıktan sonra, bu kullanıcının parolasını sıfırlamalısınız.
   
    Azure portal VM 'nizi seçin. Ayarlar bölmesini listenin en altında bulunan **destek + sorun giderme** bölümüne kaydırın. **Parolayı Sıfırla** düğmesine tıklayın. Kullanıcı adınızı ve yeni bir parolayı girin. Son olarak **Kaydet** düğmesine tıklayın:
   
    ![Klasik dağıtım modelini kullanarak V d için Azure portal Kullanıcı kimlik bilgilerini sıfırlayın.](./media/troubleshoot-rdp-connection/classic-reset-password.png)
6. **Sanal makineyi yeniden başlatın**. Bu sorun giderme adımı, VM 'nin sahip olduğu temeldeki sorunları düzeltebilir.
   
    Azure portal VM 'nizi seçin ve **genel bakış** sekmesine tıklayın. **Yeniden Başlat** düğmesine tıklayın:
   
    ![Klasik dağıtım modelini kullanarak v için Azure portal V d 'yi yeniden başlatın.](./media/troubleshoot-rdp-connection/classic-restart-vm.png)

7. Bilgisayarınızda herhangi bir şirket içi güvenlik duvarının veya güvenlik duvarının Azure 'a giden TCP 3389 trafiğine izin verdiğinden emin olun.

Hala RDP sorunlarıyla karşılaşdıysanız [bir destek isteği açabilir](https://azure.microsoft.com/support/options/) veya [daha ayrıntılı RDP sorun giderme kavramlarını ve adımlarını](detailed-troubleshoot-rdp.md)okuyabilirsiniz.

## <a name="troubleshoot-specific-rdp-errors"></a>Belirli RDP hatalarında sorun giderme
VM 'nize RDP aracılığıyla bağlanmaya çalışırken belirli bir hata iletisiyle karşılaşabilirsiniz. En yaygın hata iletileri aşağıda verilmiştir:

* [Bir lisans sağlamak için kullanılabilir Uzak Masaüstü lisans sunucusu olmadığından uzak oturumun bağlantısı kesildi](troubleshoot-specific-rdp-errors.md#rdplicense).
* [Uzak Masaüstü "adı" bilgisayarını bulamıyor](troubleshoot-specific-rdp-errors.md#rdpname).
* [Bir kimlik doğrulama hatası oluştu. Yerel güvenlik yetkilisine](troubleshoot-specific-rdp-errors.md#rdpauth)ulaşılamıyor.
* [Windows güvenlik hatası: kimlik bilgileriniz çalışmadı](troubleshoot-specific-rdp-errors.md#wincred).
* [Bu bilgisayar uzak bilgisayara bağlanamıyor](troubleshoot-specific-rdp-errors.md#rdpconnect).

## <a name="additional-resources"></a>Ek kaynaklar
Bu hatalardan hiçbiri gerçekleşemiyorsa ve Uzak Masaüstü aracılığıyla sanal makineye bağlanamıyorsanız, [Uzak Masaüstü için ayrıntılı sorun giderme kılavuzunu](detailed-troubleshoot-rdp.md)okuyun.
* Bir VM üzerinde çalışan uygulamalara erişme sorunlarını giderme adımları için bkz. [Azure VM 'de çalışan bir uygulamaya erişim sorunlarını giderme](./troubleshoot-app-connection.md?toc=/azure/virtual-machines/linux/toc.json).
* Azure 'da bir Linux VM 'sine bağlanmak üzere Secure Shell (SSH) kullanarak sorun yaşıyorsanız, bkz. [Azure 'Da LINUX VM Ile SSH bağlantılarında sorun giderme](./troubleshoot-ssh-connection.md?toc=/azure/virtual-machines/linux/toc.json).