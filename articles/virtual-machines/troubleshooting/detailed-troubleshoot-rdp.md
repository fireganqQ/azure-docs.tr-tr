---
title: Azure 'da ayrıntılı Uzak Masaüstü sorunlarını giderme | Microsoft Docs
description: Azure 'da Windows sanal makineleri için kullanabileceğiniz Uzak Masaüstü hataları için ayrıntılı sorun giderme adımlarını gözden geçirin
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: Uzak masaüstüne bağlanılamıyor, Uzak Masaüstü sorunlarını giderme, Uzak Masaüstü bağlantısı, uzak masaüstü hataları, Uzak Masaüstü sorunlarını giderme, uzak masaüstü sorunları
ms.assetid: 9da36f3d-30dd-44af-824b-8ce5ef07e5e0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: a17f3992663fe6de0751f7bc537297b9ba70cfc5
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98738144"
---
# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Azure'daki Windows VM’lerine uzak masaüstü bağlantısı sorunlarında ayrıntılı sorun giderme adımları
Bu makalede, Windows tabanlı Azure sanal makineleri için karmaşık uzak masaüstü hatalarını tanılamak ve gidermek için ayrıntılı sorun giderme adımları sağlanmaktadır.

> [!IMPORTANT]
> Daha yaygın uzak masaüstü hatalarını ortadan kaldırmak için devam etmeden önce [Uzak Masaüstü için temel sorun giderme makalesini](troubleshoot-rdp-connection.md) okuduğunuzdan emin olun.

[Temel uzak masaüstü sorun giderme kılavuzunda](troubleshoot-rdp-connection.md)ele alınan belirli hata iletilerine benzemeyen bir uzak masaüstü hata iletisiyle karşılaşabilirsiniz. Uzak Masaüstü (RDP) istemcisinin neden Azure VM 'de RDP hizmetine bağlanamadığına yönelik bu adımları izleyin.


Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**'ı tıklatın. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.

## <a name="components-of-a-remote-desktop-connection"></a>Uzak Masaüstü bağlantısı bileşenleri
Aşağıdaki bileşenler bir RDP bağlantısına dahil değildir:

![Uzak Masaüstü (RDP) bağlantısıyla ilgili bileşenleri gösteren diyagram.](./media/detailed-troubleshoot-rdp/tshootrdp_0.png)

Devam etmeden önce, sanal makineye yönelik son başarılı uzak masaüstü bağlantısından bu yana nelerin değiştiğini gözden geçirmek için yardımcı olabilir. Örneğin:

* VM 'nin genel IP adresi veya VM 'yi (sanal IP adresi [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)de denir) içeren bulut hizmeti değişmiştir. DNS istemci önbelleğiniz hala DNS adı için kaydedilmiş *ESKI IP adresine* sahıp olduğundan RDP hatası olabilir. DNS istemci önbelleğinizi boşaltıp VM 'yi yeniden bağlamayı deneyin. Ya da yeni VIP ile doğrudan bağlanmayı deneyin.
* Azure portal tarafından oluşturulan bağlantıyı kullanmak yerine uzak masaüstü bağlantılarınızı yönetmek için üçüncü taraf bir uygulama kullanıyorsunuz. Uygulama yapılandırmasının, uzak masaüstü trafiği için doğru TCP bağlantı noktasını içerdiğini doğrulayın. Bu bağlantı noktasını [Azure Portal](https://portal.azure.com)klasik bir sanal makine IÇIN, VM 'nin ayarlarını > uç noktalarına tıklayarak kontrol edebilirsiniz.

## <a name="preliminary-steps"></a>Ön adımlar
Ayrıntılı sorun giderme işlemine geçmeden önce

* Tüm açık sorunlar için Azure portal sanal makinenin durumunu denetleyin.
* [Temel sorun giderme kılavuzunda YAYGıN RDP hataları için hızlı onarım adımlarını](troubleshoot-rdp-connection.md#quick-troubleshooting-steps)izleyin.
* Özel görüntüler için, VHD 'nizin karşıya yüklemeden önce düzgün şekilde hazırlandığından emin olun. Daha fazla bilgi için bkz. [Azure 'a yüklemek için bir WINDOWS VHD veya vhdx hazırlama](../windows/prepare-for-upload-vhd-image.md).


Bu adımlardan sonra Uzak Masaüstü aracılığıyla VM 'ye yeniden bağlanmayı deneyin.

## <a name="detailed-troubleshooting-steps"></a>Ayrıntılı sorun giderme adımları
Aşağıdaki kaynaklardaki sorunlar nedeniyle uzak masaüstü istemcisi, Azure VM 'deki uzak masaüstü hizmetine ulaşamayacak olabilir:

* [Uzak Masaüstü istemci bilgisayarı](#source-1-remote-desktop-client-computer)
* [Kuruluş intranet Edge cihazı](#source-2-organization-intranet-edge-device)
* [Bulut hizmeti uç noktası ve erişim denetimi listesi (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Ağ güvenlik grupları](#source-4-network-security-groups)
* [Windows tabanlı Azure VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Kaynak 1: Uzak Masaüstü istemci bilgisayarı
Bilgisayarınızın diğer şirket içi, Windows tabanlı bir bilgisayara Uzak Masaüstü bağlantıları yapıp yapabildiğini doğrulayın.

![RDP istemcisi ile bir uzak masaüstü (RDP) bağlantısı içindeki bileşenlerin diyagramı ve bir bağlantıyı gösteren başka bir şirket içi bilgisayara işaret eden bir ok.](./media/detailed-troubleshoot-rdp/tshootrdp_1.png)

Bu şekilde, bilgisayarınızda aşağıdaki ayarları kontrol edin:

* Uzak Masaüstü trafiğini engelleyen yerel bir güvenlik duvarı ayarı.
* Uzak Masaüstü bağlantılarını engelleyen yerel olarak yüklenen istemci proxy yazılımı.
* Uzak Masaüstü bağlantılarını engelleyen yerel olarak yüklenen ağ izleme yazılımı.
* Trafiği izleyen veya Uzak Masaüstü bağlantılarını engelleyen belirli trafik türlerine izin veren/izin veren diğer güvenlik yazılımı türleri.

Tüm bu durumlarda, yazılımı geçici olarak devre dışı bırakın ve Uzak Masaüstü aracılığıyla şirket içi bir bilgisayara bağlanmayı deneyin. Gerçek neden bu şekilde bulabiliyorsanız, Uzak Masaüstü bağlantılarına izin vermek için yazılım ayarlarını düzeltmek üzere ağ yöneticinizle birlikte çalışın.

## <a name="source-2-organization-intranet-edge-device"></a>Kaynak 2: kuruluş intranet Edge cihazı
Internet 'e doğrudan bağlı bir bilgisayarın, Azure sanal makinenize Uzak Masaüstü bağlantıları yapıp yapabildiğini doğrulayın.

![İnternet 'e bağlı bir RDP istemcisi ile bir uzak masaüstü (RDP) bağlantısı içindeki bileşenlerin diyagramı, bir bağlantıyı gösteren bir Azure sanal makinesine işaret eden bir ok.](./media/detailed-troubleshoot-rdp/tshootrdp_2.png)

Doğrudan Internet 'e bağlı bir bilgisayarınız yoksa, kaynak grubunda veya bulut hizmetinde yeni bir Azure sanal makinesi oluşturun ve bunlarla test edin. Daha fazla bilgi için bkz. [Azure 'Da Windows çalıştıran bir sanal makine oluşturma](../windows/quick-create-portal.md). Testten sonra sanal makineyi ve kaynak grubunu veya bulut hizmetini silebilirsiniz.

Doğrudan Internet 'e bağlı bir bilgisayar ile bir Uzak Masaüstü bağlantısı oluşturbiliyorsanız, kuruluşunuzun intranet Edge cihazını şu şekilde denetleyin:

* Internet 'e HTTPS bağlantılarını engelleyen dahili bir güvenlik duvarı.
* Uzak Masaüstü bağlantılarını engellediği bir ara sunucu.
* Uç ağınızdaki cihazlarda çalışan, Uzak Masaüstü bağlantılarını önleyen, yetkisiz giriş algılama veya ağ izleme yazılımı.

Internet 'e yönelik HTTPS tabanlı Uzak Masaüstü bağlantılarına izin vermek üzere kuruluşunuzun intranet Edge cihazının ayarlarını düzeltmek için ağ yöneticinizle birlikte çalışın.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Kaynak 3: bulut hizmeti uç noktası ve ACL

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Klasik dağıtım modeli kullanılarak oluşturulan VM 'Ler için, aynı bulut hizmetinde veya sanal ağda bulunan başka bir Azure VM 'nin Azure sanal makinelerinize Uzak Masaüstü bağlantıları yapıp yapabildiğini doğrulayın.

![Bir Azure VM vurgulanmış bir uzak masaüstü (RDP) bağlantısı içindeki bileşenlerin diyagramı ve bir bağlantıyı gösteren aynı bulut hizmeti içindeki başka bir Azure VM 'ye işaret eden bir ok.](./media/detailed-troubleshoot-rdp/tshootrdp_3.png)

> [!NOTE]
> Kaynak Yöneticisi oluşturulan sanal makineler için, kaynak 4 ' e atlayın [: ağ güvenlik grupları](#source-4-network-security-groups).

Aynı bulut hizmetinde veya sanal ağda başka bir sanal makineniz yoksa, bir tane oluşturun. [Azure 'Da Windows çalıştıran bir sanal makine oluşturma](../windows/quick-create-portal.md)bölümündeki adımları izleyin. Test tamamlandıktan sonra test sanal makinesini silin.

Aynı bulut hizmeti veya sanal ağ içindeki bir sanal makineye Uzak Masaüstü aracılığıyla bağlanıyorsanız, bu ayarları kontrol edin:

* Hedef VM 'deki uzak masaüstü trafiğine yönelik uç nokta yapılandırması: uç noktanın özel TCP bağlantı noktası, sanal makinenin uzak masaüstü hizmetinin dinlediği TCP bağlantı noktasıyla eşleşmelidir (varsayılan 3389).
* Hedef VM 'deki uzak masaüstü trafiği uç noktası ACL 'SI: ACL 'Ler, kaynak IP adresine göre Internet 'ten izin verilen veya reddedilen gelen trafiği belirtmenize olanak tanır. Yanlış yapılandırılmış ACL 'Ler, gelen uzak masaüstü trafiğini uç noktaya engelleyebilir. Proxy 'nizin veya diğer uç sunucunuzun genel IP adreslerinden gelen trafiğe izin verildiğinden emin olmak için ACL 'larınızı denetleyin. Daha fazla bilgi için bkz. [ağ Access Control listesi (ACL) nedir?](/previous-versions/azure/virtual-network/virtual-networks-acl)

Uç noktanın sorunun kaynağı olup olmadığını denetlemek için, geçerli uç noktayı kaldırın ve yeni bir tane oluşturun ve dış bağlantı noktası numarası için 49152 – 65535 aralığında rastgele bir bağlantı noktası seçin. Daha fazla bilgi için bkz. [sanal makineye uç noktaları ayarlama](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints?toc=/azure/virtual-machines/windows/classic/toc.json).

## <a name="source-4-network-security-groups"></a>Kaynak 4: ağ güvenlik grupları
Ağ güvenlik grupları izin verilen gelen ve giden trafik üzerinde daha ayrıntılı denetim sağlar. Bir Azure sanal ağında alt ağları ve bulut hizmetlerini kapsayan kurallar oluşturabilirsiniz.

Bir ağ güvenlik grubundaki bir kuralın bir sanal makineden gelen veya giden trafiği engelleyip engellemediğini onaylamak için [IP akışı doğrulama](../../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) kullanın. Ayrıca, gelen "Izin ver" NSG kuralının mevcut olduğundan ve RDP bağlantı noktası (varsayılan 3389) için önceliklendirildiğinden emin olmak için etkin güvenlik grubu kurallarını gözden geçirebilirsiniz. Daha fazla bilgi için bkz. [sanal makine trafiği akışı sorunlarını gidermek Için etkin güvenlik kurallarını kullanma](../../virtual-network/diagnose-network-traffic-filter-problem.md).

## <a name="source-5-windows-based-azure-vm"></a>Kaynak 5: Windows tabanlı Azure VM
![Bir bulut hizmeti içinde vurgulanan bir Azure VM ile uzak masaüstü (RDP) bağlantısı içindeki bileşenlerin diyagramı ve "olası sorun kaynağı" olabilir. Mavi bir çizgi, ağ güvenlik grubu kurallarının Azure VM 'ye giden veya giden trafiği engellediğini gösterir.](./media/detailed-troubleshoot-rdp/tshootrdp_5.png)

[Bu makaledeki](./reset-rdp.md)yönergeleri izleyin. Bu makalede, sanal makinedeki uzak masaüstü hizmeti sıfırlanır:

* "Uzak Masaüstü" Windows Güvenlik Duvarı varsayılan kuralını (TCP bağlantı noktası 3389) etkinleştirin.
* Hklm\system\currentcontrolset\control\terminalserver\fdenytsconnections kayıt defteri değerini 0 olarak ayarlayarak uzak masaüstü bağlantılarını etkinleştirin.

Bilgisayarınızdan bağlantıyı yeniden deneyin. Hala uzak masaüstü aracılığıyla bağlanamadıysanız, aşağıdaki olası sorunları kontrol edin:

* Uzak Masaüstü hizmeti hedef VM üzerinde çalışmıyor.
* Uzak Masaüstü hizmeti 3389 numaralı TCP bağlantı noktasında dinlemiyor.
* Windows güvenlik duvarı veya başka bir yerel güvenlik duvarının uzak masaüstü trafiğini engelleyen bir giden kuralı vardır.
* Azure sanal makinesinde çalışan yetkisiz giriş algılama veya ağ izleme yazılımı uzak masaüstü bağlantılarını engellemektedir.

Klasik dağıtım modeli kullanılarak oluşturulan VM 'Ler için Azure sanal makinesinde bir uzak Azure PowerShell oturumu kullanabilirsiniz. İlk olarak, sanal makinenin barındırma bulut hizmeti için bir sertifika yüklemeniz gerekir. [Azure sanal makinelerine güvenli uzak PowerShell erişimi yapılandırma](https://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) ' ya gidin ve **InstallWinRMCertAzureVM.ps1** betik dosyasını yerel bilgisayarınıza indirin.

Daha önce yapmadıysanız Azure PowerShell ' yi yükleyebilirsiniz. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/).

Sonra, bir Azure PowerShell komut istemi açın ve geçerli klasörü **InstallWinRMCertAzureVM.ps1** betik dosyasının konumuyla değiştirin. Bir Azure PowerShell Betiği çalıştırmak için doğru Yürütme ilkesini ayarlamanız gerekir. Geçerli ilke düzeyinizi öğrenmek için **Get-ExecutionPolicy** komutunu çalıştırın. Uygun düzeyi ayarlama hakkında daha fazla bilgi için bkz. [set-ExecutionPolicy](/powershell/module/microsoft.powershell.security/set-executionpolicy).

Ardından, Azure abonelik adınızı, bulut hizmeti adını ve sanal makine adınızı (< ve > karakterlerini kaldırarak) girin ve ardından bu komutları çalıştırın.

```powershell
$subscr="<Name of your Azure subscription>"
$serviceName="<Name of the cloud service that contains the target virtual machine>"
$vmName="<Name of the target virtual machine>"
.\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName
```

**Get-azuyeniden gönderme** komut dosyası ' nın *subscriptionName* özelliğinden doğru abonelik adını alabilirsiniz. **Get-AzureVM** komutunu görüntüleyen *ServiceName* sütunundan sanal makinenin bulut hizmeti adını alabilirsiniz.

Yeni sertifikaya sahip olup olmadığınızı denetleyin. Geçerli Kullanıcı için bir Sertifikalar ek bileşeni açın ve **Güvenilen kök sertifika yetkilisi sertifikası** klasörüne bakın. Verilen sütununda bulut hizmetinizin DNS adına sahip bir sertifika görmeniz gerekir (örnek: cloudservice4testing.cloudapp.net).

Sonra, bu komutları kullanarak bir uzak Azure PowerShell oturumu başlatın.

```powershell
$uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
$creds = Get-Credential
Enter-PSSession -ConnectionUri $uri -Credential $creds
```

Geçerli yönetici kimlik bilgilerini girdikten sonra aşağıdaki Azure PowerShell istemine benzer bir şey görmeniz gerekir:

```powershell
[cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>
```

Bu istemin ilk bölümü, "cloudservice4testing.cloudapp.net" öğesinden farklı olabilen hedef VM 'yi içeren bulut hizmeti adıdır. Artık bu bulut hizmeti için, bahsedilen sorunları araştırmak ve yapılandırmayı düzeltmek üzere Azure PowerShell komutlar verebilirsiniz.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>TCP bağlantı noktasını dinleyen Uzak Masaüstü Hizmetleri el ile düzeltmek için
Uzaktan Azure PowerShell oturum isteminde bu komutu çalıştırın.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

PortNumber özelliği geçerli bağlantı noktası numarasını gösterir. Gerekirse, bu komutu kullanarak Uzak Masaüstü bağlantı noktası numarasını varsayılan değerine (3389) geri değiştirin.

```powershell
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389
```

Bu komutu kullanarak bağlantı noktasının 3389 olarak değiştirildiğini doğrulayın.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

Bu komutu kullanarak uzak Azure PowerShell oturumundan çıkın.

```powershell
Exit-PSSession
```

Azure VM için Uzak Masaüstü uç noktasının, iç bağlantı noktası olarak TCP bağlantı noktası 3398 ' i de kullandığını doğrulayın. Azure VM 'yi yeniden başlatın ve uzak masaüstü bağlantısını yeniden deneyin.

## <a name="additional-resources"></a>Ek kaynaklar
[Windows sanal makineleri için bir parolayı veya Uzak Masaüstü hizmetini sıfırlama](./reset-rdp.md)

[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/)

[Linux tabanlı bir Azure sanal makinesine yönelik Secure Shell (SSH) bağlantılarında sorun giderme](./troubleshoot-ssh-connection.md?toc=/azure/virtual-machines/linux/toc.json)

[Azure sanal makinesinde çalışan bir uygulamaya erişim sorunlarını giderme](./troubleshoot-app-connection.md?toc=/azure/virtual-machines/linux/toc.json)
