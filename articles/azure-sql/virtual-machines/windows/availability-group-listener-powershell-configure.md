---
title: Kullanılabilirlik grubu dinleyicilerini ve yük dengeleyiciyi yapılandırma (PowerShell)
description: Bir veya daha fazla IP adresi olan bir iç yük dengeleyici kullanarak Azure Resource Manager modelinde kullanılabilirlik grubu dinleyicilerini yapılandırın.
services: virtual-machines
documentationcenter: na
author: MashaMSFT
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.subservice: hadr
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/06/2019
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 9337d1c2767923e6dc7c6b267e0c180b460a116e
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97359430"
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Bir veya daha fazla Always on kullanılabilirlik grubu dinleyicisi yapılandırma-Kaynak Yöneticisi

[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu belgede aşağıdaki görevlerden birini yapmak için PowerShell 'in nasıl kullanılacağı gösterilmektedir:
- yük dengeleyici oluşturma
- SQL Server kullanılabilirlik grupları için mevcut yük dengeleyiciye IP adresleri ekleyin.

Kullanılabilirlik grubu dinleyicisi, istemcilerin veritabanı erişimi için bağlanacağı bir sanal ağ adıdır. Azure sanal makinelerinde, yük dengeleyici dinleyicinin IP adresini tutar. Yük dengeleyici, trafiği araştırma bağlantı noktasında dinleme yapan SQL Server örneğine yönlendirir. Genellikle bir kullanılabilirlik grubu, iç yük dengeleyici kullanır. Bir Azure iç yük dengeleyici bir veya daha fazla IP adresini barındırabilirler. Her IP adresi belirli bir yoklama bağlantı noktası kullanır. 

Bir iç yük dengeleyiciye birden çok IP adresi atama özelliği Azure 'da yenidir ve yalnızca Kaynak Yöneticisi modelinde kullanılabilir. Bu görevi gerçekleştirmek için, Kaynak Yöneticisi modelindeki Azure sanal makinelerinde dağıtılmış bir SQL Server kullanılabilirlik grubunuz olması gerekir. SQL Server sanal makinelerin her ikisi de aynı Kullanılabilirlik kümesine ait olmalıdır. Azure Resource Manager ' de kullanılabilirlik grubunu otomatik olarak oluşturmak için [Microsoft şablonunu](./availability-group-quickstart-template-configure.md) kullanabilirsiniz. Bu şablon, sizin için iç yük dengeleyici dahil olmak üzere kullanılabilirlik grubunu otomatik olarak oluşturur. İsterseniz, [her zaman açık kullanılabilirlik grubunu el ile yapılandırabilirsiniz](availability-group-manually-configure-tutorial.md).

Bu makaledeki adımları tamamlayabilmeniz için kullanılabilirlik gruplarının zaten yapılandırılmış olması gerekir.  

İlgili konular şunları içerir:

* [Azure VM 'de AlwaysOn Kullanılabilirlik Grupları Yapılandırma (GUI)](availability-group-manually-configure-tutorial.md)   
* [Azure Resource Manager ve PowerShell kullanarak bir Sanal Ağdan Sanal Ağa bağlantısı yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="verify-powershell-version"></a>PowerShell sürümünü doğrula

Bu makaledeki örneklerde Azure PowerShell Module sürümü 5.4.1 kullanılarak test edilmiştir.

PowerShell modülünüzün 5.4.1 veya üzeri olduğunu doğrulayın.

Bkz. [Azure PowerShell modülünü Install](/powershell/azure/install-az-ps).

## <a name="configure-the-windows-firewall"></a>Windows güvenlik duvarını yapılandırma

Windows güvenlik duvarını SQL Server erişimine izin verecek şekilde yapılandırın. Güvenlik duvarı kuralları SQL Server örneği ve dinleyici araştırması tarafından kullanılan bağlantı noktalarına TCP bağlantılarına izin verir. Ayrıntılı yönergeler için bkz. [veritabanı altyapısı erişimi için bir Windows Güvenlik Duvarı yapılandırma](/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access#Anchor_1). SQL Server bağlantı noktası ve yoklama bağlantı noktası için bir gelen kuralı oluşturun.

Azure ağ güvenlik grubuyla erişimi kısıtladığınız takdirde, izin ver kurallarının arka uç SQL Server VM IP adreslerini ve ağ dinleyicisi için yük dengeleyici kayan IP adreslerini ve varsa küme çekirdek IP adresini içerdiğinden emin olun.

## <a name="determine-the-load-balancer-sku-required"></a>Gerekli yük dengeleyici SKU 'sunu belirleme

[Azure yük dengeleyici](../../../load-balancer/load-balancer-overview.md) iki SKU 'da kullanılabilir: temel & standart. Standart yük dengeleyici önerilir. Sanal makineler bir kullanılabilirlik kümesinde ise, temel yük dengeleyiciye izin verilir. Sanal makineler bir kullanılabilirlik bölgeindeyse standart yük dengeleyici gereklidir. Standart yük dengeleyici, tüm VM IP adreslerinin standart IP adreslerini kullanmasını gerektirir.

Bir kullanılabilirlik grubu için geçerli [Microsoft şablonu](./availability-group-quickstart-template-configure.md) , temel IP adresleriyle temel bir yük dengeleyici kullanır.

   > [!NOTE]
   > Bulut tanığı için standart yük dengeleyici ve Azure Storage kullanıyorsanız, bir [hizmet uç noktası](../../../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network) yapılandırmanız gerekir. 
   > 

Bu makaledeki örneklerde standart yük dengeleyici verilmiştir. Örneklerde, komut dosyası içerir `-sku Standard` .

```powershell
$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe -sku Standard
```

Temel yük dengeleyici oluşturmak için `-sku Standard` Yük dengeleyiciyi oluşturan satırdan kaldırın. Örneğin:

```powershell
$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe
```

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Örnek betik: PowerShell ile iç yük dengeleyici oluşturma

> [!NOTE]
> Kullanılabilirlik grubunuzu [Microsoft şablonuyla](./availability-group-quickstart-template-configure.md)oluşturduysanız, iç yük dengeleyici zaten oluşturulmuştur.

Aşağıdaki PowerShell betiği, bir iç yük dengeleyici oluşturur, Yük Dengeleme kurallarını yapılandırır ve yük dengeleyici için bir IP adresi ayarlar. Betiği çalıştırmak için Windows PowerShell ISE açın ve betiği Betik bölmesine yapıştırın. `Connect-AzAccount`PowerShell 'de oturum açmak için kullanın. Birden çok Azure aboneliğiniz varsa, `Select-AzSubscription` aboneliği ayarlamak için kullanın. 

```powershell
# Connect-AzAccount
# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the back-end configuration

$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzNetworkInterface -NetworkInterface $NIC
        start-AzVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a><a name="Add-IP"></a> Örnek betik: PowerShell ile mevcut yük dengeleyicisine bir IP adresi ekleme

Birden fazla kullanılabilirlik grubu kullanmak için yük dengeleyicisine ek bir IP adresi ekleyin. Her IP adresi kendi yük dengeleme kuralını, araştırma bağlantı noktasını ve ön bağlantı noktasını gerektirir.

Ön uç bağlantı noktası, uygulamaların SQL Server örneğine bağlanmak için kullandığı bağlantı noktasıdır. Farklı kullanılabilirlik gruplarının IP adresleri aynı ön uç bağlantı noktasını kullanabilir.

> [!NOTE]
> SQL Server kullanılabilirlik grupları için, her IP adresi için belirli bir yoklama bağlantı noktası gerekir. Örneğin, bir yük dengeleyicideki bir IP adresi araştırma bağlantı noktası 59999 ' i kullanıyorsa, bu yük dengeleyicide başka IP adresleri araştırma bağlantı noktası 59999 ' i kullanabilir.

* Yük dengeleyici sınırları hakkında daha fazla bilgi için bkz. [ağ sınırları](../../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits)altında **yük dengeleyici başına özel ön uç IP 'si** Azure Resource Manager.
* Kullanılabilirlik grubu sınırları hakkında daha fazla bilgi için bkz. [kısıtlamalar (kullanılabilirlik grupları)](/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability#RestrictionsAG).

Aşağıdaki betik var olan bir yük dengeleyiciye yeni bir IP adresi ekler. ILB, Yük Dengeleme ön uç bağlantı noktası için dinleyici bağlantı noktasını kullanır. Bu bağlantı noktası SQL Server dinlediği bağlantı noktası olabilir. SQL Server varsayılan örnekleri için bağlantı noktası 1433 ' dir. Bir kullanılabilirlik grubunun Yük Dengeleme kuralı, bir kayan IP (doğrudan sunucu dönüşü) gerektirir, bu nedenle arka uç bağlantı noktası ön uç bağlantı noktasıyla aynı olur. Ortamınızın değişkenlerini güncelleştirin. 

```powershell
# Connect-AzAccount
# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzLoadBalancer 

$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzLoadBalancer   
```

## <a name="configure-the-listener"></a>Dinleyiciyi yapılandırma

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-the-listener-port-in-sql-server-management-studio"></a>SQL Server Management Studio dinleyici bağlantı noktasını ayarlama

1. SQL Server Management Studio başlatın ve birincil çoğaltmaya bağlanın.

1. **AlwaysOn yüksek kullanılabilirlik**  >  **kullanılabilirlik grupları**  >  **kullanılabilirlik grubu dinleyicileri**' ne gidin. 

1. Artık Yük Devretme Kümesi Yöneticisi oluşturduğunuz dinleyici adını görmeniz gerekir. Dinleyici adına sağ tıklayın ve **Özellikler**' i seçin.

1. **Bağlantı noktası** kutusunda, daha önce kullandığınız $EndpointPort kullanarak kullanılabilirlik grubu dinleyicisinin bağlantı noktası numarasını belirtin (1433 varsayılandır) ve ardından **Tamam**' ı seçin.

## <a name="test-the-connection-to-the-listener"></a>Dinleyiciyle bağlantıyı test etme

Bağlantıyı test etmek için:

1. Aynı sanal ağdaki bir SQL Server bağlanmak için Uzak Masaüstü Protokolü (RDP) kullanın, ancak çoğaltmaya sahip değildir. Bu, kümedeki diğer SQL Server olabilir.

1. Bağlantıyı sınamak için **sqlcmd** yardımcı programını kullanın. Örneğin, aşağıdaki komut dosyası, Windows kimlik doğrulaması ile dinleyici aracılığıyla birincil çoğaltmaya bir **sqlcmd** bağlantısı kurar:
   
    ```
    sqlcmd -S <listenerName> -E
    ```
   
    Dinleyici varsayılan bağlantı noktası (1433) dışında bir bağlantı noktası kullanıyorsa bağlantı dizesinde bağlantı noktasını belirtin. Örneğin, aşağıdaki sqlcmd komutu 1435 numaralı bağlantı noktasında bir dinleyiciye bağlanır: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

SQLCMD bağlantısı, birincil çoğaltmayı barındıran SQL Server her örneğine otomatik olarak bağlanır. 

> [!NOTE]
> Belirttiğiniz bağlantı noktasının her iki SQL Server güvenlik duvarında açık olduğundan emin olun. Her iki sunucu da kullandığınız TCP bağlantı noktası için bir gelen kuralı gerektirir. Daha fazla bilgi için bkz. [güvenlik duvarı kuralı ekleme veya düzenleme](/previous-versions/orphan-topics/ws.11/cc753558(v=ws.11)). 
> 

## <a name="guidelines-and-limitations"></a>Yönergeler ve sınırlamalar

İç yük dengeleyici kullanılarak Azure 'da kullanılabilirlik grubu dinleyicisi hakkında aşağıdaki yönergelere göz önünde edin:

* İç yük dengeleyici ile yalnızca aynı sanal ağ içinden gelen dinleyiciye erişirsiniz.

* Azure ağ güvenlik grubuyla erişimi kısıtyorsanız, izin ver kurallarının şunları içerdiğinden emin olun:
  - Arka uç SQL Server VM IP adresleri
  - AG dinleyicisi için yük dengeleyici kayan IP adresleri
  - Varsa, küme çekirdek IP adresi.

* Bulut tanığı için Azure depolama ile standart yük dengeleyici kullanırken bir hizmet uç noktası oluşturun. Daha fazla bilgi için bkz. [sanal ağdan erişim verme](../../../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network).

## <a name="for-more-information"></a>Daha fazla bilgi edinmek için

Daha fazla bilgi için bkz. [Azure VM 'de Always on kullanılabilirlik grubunu el Ile yapılandırma](availability-group-manually-configure-tutorial.md).

## <a name="powershell-cmdlets"></a>PowerShell cmdlet'leri

Azure sanal makineleri için bir iç yük dengeleyici oluşturmak için aşağıdaki PowerShell cmdlet 'lerini kullanın.

* [New-AzLoadBalancer](/powershell/module/Azurerm.Network/New-AzureRmLoadBalancer) bir yük dengeleyici oluşturur. 
* [New-Azloadbalancerfrontendıpconfig](/powershell/module/Azurerm.Network/New-AzureRmLoadBalancerFrontendIpConfig) , yük dengeleyici için bir ön uç IP yapılandırması oluşturur. 
* [New-AzLoadBalancerRuleConfig](/powershell/module/Azurerm.Network/New-AzureRmLoadBalancerRuleConfig) , bir yük dengeleyici için bir kural yapılandırması oluşturur. 
* [New-Azloadbalancerbackendadddresspoolconfig](/powershell/module/Azurerm.Network/New-AzureRmLoadBalancerBackendAddressPoolConfig) , bir yük dengeleyici için arka uç adres havuzu yapılandırması oluşturur. 
* [New-AzLoadBalancerProbeConfig](/powershell/module/Azurerm.Network/New-AzureRmLoadBalancerProbeConfig) , yük dengeleyici için bir araştırma yapılandırması oluşturur.
* [Remove-AzLoadBalancer](/powershell/module/Azurerm.Network/Remove-AzureRmLoadBalancer) bir Azure Kaynak grubundaki yük dengeleyiciyi kaldırır.