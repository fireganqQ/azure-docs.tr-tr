---
title: VM'ler için Azure İzleyici için Log Analytics çalışma alanı yapılandırma
description: VM'ler için Azure İzleyici tarafından kullanılan Log Analytics çalışma alanının nasıl oluşturulduğunu ve yapılandırılacağını açıklar.
ms.subservice: ''
ms.topic: conceptual
ms.custom: references_regions
author: bwren
ms.author: bwren
ms.date: 12/22/2020
ms.openlocfilehash: b84f9cae848d53cf04e1b77810b347786e122c5b
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100625189"
---
# <a name="configure-log-analytics-workspace-for-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici için Log Analytics çalışma alanı yapılandırma
VM'ler için Azure İzleyici, Azure Izleyici 'deki bir veya daha fazla Log Analytics çalışma alanından verilerini toplar. Aracıları ekleme öncesinde, bir çalışma alanı oluşturmanız ve yapılandırmanız gerekir. Bu makalede, çalışma alanının gereksinimleri açıklanmakta ve VM'ler için Azure İzleyici için yapılandırılacak.

## <a name="overview"></a>Genel Bakış
Tek bir abonelik, gereksinimlerinize bağlı olarak istediğiniz sayıda çalışma alanını kullanabilir. Çalışma alanının tek gereksinimi, desteklenen bir konumda konumlandırılabilir ve *Vminsıghts* çözümü ile yapılandırılmalıdır.

Çalışma alanı yapılandırıldıktan sonra, gerekli aracıları sanal makineye ve sanal makine ölçek kümesine yüklemek için kullanılabilir seçeneklerden herhangi birini kullanabilir ve bunların verilerini gönderebilmesi için bir çalışma alanı belirtebilirsiniz. VM'ler için Azure İzleyici, aboneliğindeki herhangi bir yapılandırılmış çalışma alanından veri toplar.

> [!NOTE]
> Tek bir sanal makinede veya Azure portal kullanarak sanal makine ölçek kümesinde VM'ler için Azure İzleyici etkinleştirdiğinizde, var olan bir çalışma alanını seçme veya yeni bir tane oluşturma seçeneği sunulur. *Vminsıghts* çözümü henüz yoksa bu çalışma alanına yüklenecek. Daha sonra bu çalışma alanını diğer aracılar için kullanabilirsiniz.


## <a name="create-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

>[!NOTE]
>Bu bölümde açıklanan bilgiler [hizmet eşlemesi çözümü](service-map.md)için de geçerlidir. 

**Log Analytics çalışma alanları** menüsünden Azure Portal Log Analytics çalışma alanlarına erişin.

[![Anlytiği çalışma alanlarını günlüğe kaydet](media/vminsights-configure-workspace/log-analytics-workspaces.png)](media/vminsights-configure-workspace/log-analytics-workspaces.png#lightbox)

Aşağıdaki yöntemlerden herhangi birini kullanarak yeni bir Log Analytics çalışma alanı oluşturabilirsiniz. Ortamınızda kullanmanız gereken çalışma alanı sayısını ve erişim stratejisini nasıl tasarlayacağınızı belirlemek için bkz. [Azure Izleyici günlükleri dağıtımınızı tasarlama](../platform/design-logs-deployment.md) .


* [Azure portalı](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../platform/powershell-workspace-configuration.md)
* [Azure Resource Manager](../samples/resource-manager-workspace.md)

## <a name="supported-regions"></a>Desteklenen bölgeler
VM'ler için Azure İzleyici, aşağıdakiler dışında [Log Analytics tarafından desteklenen bölgelerde](https://azure.microsoft.com/global-infrastructure/services/?products=monitor&regions=all) Log Analytics bir çalışma alanını destekler:

- Almanya Orta Batı
- Güney Kore - Orta

>[!NOTE]
>Azure VM 'Leri herhangi bir bölgede izleyebilirsiniz. VM 'Ler, Log Analytics çalışma alanı tarafından desteklenen bölgelerle sınırlı değildir.

## <a name="azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi
VM'ler için Azure İzleyici özellikleri etkinleştirmek ve bu özelliklere erişmek için, çalışma alanında [Log Analytics katkıda bulunan rolüne](../platform/manage-access.md#manage-access-using-azure-permissions) sahip olmanız gerekir. Performansı, sistem durumunu ve eşleme verilerini görüntülemek için, Azure VM için [izleme okuyucu rolüne](../platform/roles-permissions-security.md#built-in-monitoring-roles) sahip olmanız gerekir. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../platform/manage-access.md).

## <a name="add-vminsights-solution-to-workspace"></a>Çalışma alanına Vminsıghts çözümü ekleme
Bir Log Analytics çalışma alanının VM'ler için Azure İzleyici ile kullanılabilmesi için önce *Vminsıghts* çözümünün yüklü olması gerekir. Çalışma alanını yapılandırma yöntemleri aşağıdaki bölümlerde açıklanmıştır.

> [!NOTE]
> *Vminsıghts* çözümünü çalışma alanına eklediğinizde, çalışma alanına bağlı olan tüm mevcut sanal makineler, verileri ınsightsölçümlerini gönderecek şekilde başlayacaktır. Diğer veri türleri için veriler, çalışma alanına bağlı mevcut sanal makinelere Dependency Agent ekleyene kadar toplanmaz.

### <a name="azure-portal"></a>Azure portalı
Azure portal kullanarak var olan bir çalışma alanını yapılandırmak için üç seçenek vardır. Her biri aşağıda açıklanmıştır.

Tek bir çalışma alanını yapılandırmak için **Azure izleyici** menüsünde **sanal makineler** seçeneğine gidin, **diğer ekleme seçeneklerini** belirleyin ve ardından **bir çalışma alanı yapılandırın**. Bir abonelik ve çalışma alanı seçin ve ardından **Yapılandır**' a tıklayın.

[![Çalışma alanını yapılandırma](../vm/media/vminsights-enable-policy/configure-workspace.png)](../vm/media/vminsights-enable-policy/configure-workspace.png#lightbox)

Birden çok çalışma alanını yapılandırmak için Azure portal **Monitor** menüsündeki **sanal makineler** menüsünde bulunan **çalışma alanı yapılandırması** sekmesini seçin. Varolan çalışma alanlarının listesini göstermek için filtre değerlerini ayarlayın. Etkinleştirilecek her çalışma alanının yanındaki kutuyu işaretleyin ve ardından **Seçileni Yapılandır**' a tıklayın.

[![Çalışma alanı yapılandırması](../vm/media/vminsights-enable-policy/workspace-configuration.png)](../vm/media/vminsights-enable-policy/workspace-configuration.png#lightbox)


Tek bir sanal makinede veya Azure portal kullanarak sanal makine ölçek kümesinde VM'ler için Azure İzleyici etkinleştirdiğinizde, var olan bir çalışma alanını seçme veya yeni bir tane oluşturma seçeneği sunulur. *Vminsıghts* çözümü henüz yoksa bu çalışma alanına yüklenecek. Daha sonra bu çalışma alanını diğer aracılar için kullanabilirsiniz.

[![Portalda tek VM 'yi etkinleştirme](../vm/media/vminsights-enable-portal/enable-vminsights-vm-portal.png)](../vm/media/vminsights-enable-portal/enable-vminsights-vm-portal.png#lightbox)


### <a name="resource-manager-template"></a>Resource Manager şablonu
VM'ler için Azure İzleyici için Azure Resource Manager şablonları, [GitHub deponuzdan indirebileceğiniz](https://aka.ms/VmInsightsARMTemplates)bir arşiv dosyasında (. zip) verilmiştir. Bu, VM'ler için Azure İzleyici için Log Analytics çalışma alanı yapılandıran **configureworkspace** adlı bir şablon içerir. Aşağıdaki örnek PowerShell ve CLı komutları dahil olmak üzere standart yöntemlerden birini kullanarak bu şablonu dağıtabilirsiniz: 

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli
az deployment group create --name ConfigureWorkspace --resource-group my-resource-group --template-file CreateWorkspace.json  --parameters workspaceResourceId='/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace' workspaceLocation='eastus'

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```powershell
New-AzResourceGroupDeployment -Name ConfigureWorkspace -ResourceGroupName my-resource-group -TemplateFile ConfigureWorkspace.json -workspaceResourceId /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace -location eastus
```

---



## <a name="next-steps"></a>Sonraki adımlar
- Aracılardan VM'ler için Azure İzleyici bağlamak için [VM'ler için Azure izleyici aracıları](vminsights-enable-overview.md) ekleme bölümüne bakın.
- Bir çözümünden çalışma alanına gönderilen veri miktarını sınırlamak için bkz. [Azure izleyici 'de izleme çözümlerini hedefleme (Önizleme)](../insights/solution-targeting.md) .
