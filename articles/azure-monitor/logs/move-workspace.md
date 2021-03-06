---
title: Azure Izleyici 'de bir Log Analytics çalışma alanını taşıma | Microsoft Docs
description: Log Analytics çalışma alanınızı başka bir aboneliğe veya kaynak grubuna taşımayı öğrenin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/12/2020
ms.openlocfilehash: 7095a4c6928152ad9b643ff71c0c6a1d00036e55
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100623756"
---
# <a name="move-a-log-analytics-workspace-to-different-subscription-or-resource-group"></a>Log Analytics çalışma alanını farklı bir aboneliğe veya kaynak grubuna taşıyın

Bu makalede, Log Analytics çalışma alanını aynı bölgedeki başka bir kaynak grubuna veya aboneliğe taşıma adımlarını öğreneceksiniz. Azure kaynaklarını Azure portal, PowerShell, Azure CLı veya REST API aracılığıyla taşıma hakkında daha fazla bilgi edinebilirsiniz. konumundaki [kaynakları yeni bir kaynak grubuna veya aboneliğe taşıyın](../../azure-resource-manager/management/move-resource-group-and-subscription.md). 

> [!IMPORTANT]
> Çalışma alanını farklı bir bölgeye taşıyamazsınız.

## <a name="verify-active-directory-tenant"></a>Active Directory kiracısını doğrula
Çalışma alanı kaynağı ve hedef abonelikler aynı Azure Active Directory kiracısında bulunmalıdır. Her iki aboneliğin de aynı kiracı KIMLIĞINE sahip olduğunu doğrulamak için Azure PowerShell kullanın.

``` PowerShell
(Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
(Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
```

## <a name="workspace-move-considerations"></a>Çalışma alanı taşıma konuları
- Çalışma alanına yüklenen yönetilen çözümler Log Analytics çalışma alanı taşıma işlemiyle birlikte taşınır. 
- Çalışma alanı anahtarları (birincil ve ikincil) çalışma alanı taşıma işlemi ile yeniden oluşturulur. Çalışma alanı anahtarlarınızın bir kopyasını Anahtar Kasası 'nda tutarsanız, çalışma alanı taşıdıktan sonra oluşturulan yeni anahtarlarla güncelleştirin. 
- Bağlı aracılar bağlı kalacak ve taşıma sonrasında çalışma alanına veri göndermeyecektir. 
- Taşıma işlemi çalışma alanından bağlı bir hizmet olmadığından, çalışma alanının taşınmasına izin vermek için o bağlantıyı kullanan çözümlerin kaldırılması gerekir. Otomasyon Hesabınızın bağlantısını kaldırmak için önce kaldırılması gereken çözümler:
  - Güncelleştirme Yönetimi
  - Değişiklik İzleme
  - Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma
  - Azure Güvenlik Merkezi

>[!IMPORTANT]
> **Azure Sentinel müşterileri**
> - Şu anda, Azure Sentinel bir çalışma alanına dağıtıldıktan sonra, çalışma alanını başka bir kaynak grubuna veya aboneliğe taşımak desteklenmez. 
> - Çalışma alanını zaten taşıdıysanız, **analiz** altındaki tüm etkin kuralları devre dışı bırakın ve beş dakika sonra yeniden etkinleştirin. Bu, çoğu durumda etkili bir çözüm olması gerekir, ancak yeniden yinelemek için, bu, sizin sorumluluğunuzdadır ve riski size aittir.
> 
> **Uyarıları yeniden oluşturma**
> - İzinler çalışma alanının Azure Kaynak KIMLIĞINI temel aldığı ve bu sayede çalışma alanı taşıma sırasında yapılan değişiklikler olduğundan, tüm uyarıların bir taşıma işleminden sonra yeniden oluşturulması gerekir.
>
> **Kaynak yollarını Güncelleştir**
> - Çalışma alanı taşıdıktan sonra, çalışma alanına işaret eden tüm Azure veya dış kaynaklar, yeni kaynak hedef yolunu işaret etmek üzere incelenmeli ve güncelleştirilir.
> 
>   *Örnekler:*
>   - [Azure Izleyici uyarı kuralları](../alerts/alerts-resource-move.md)
>   - Üçüncü taraf uygulamalar
>   - Özel betik oluşturma
>

### <a name="delete-solutions-in-azure-portal"></a>Azure portal çözümleri silme
Azure portal kullanarak çözümleri kaldırmak için aşağıdaki yordamı kullanın:

1. Üzerinde çözümlerin yüklendiği kaynak grubunun menüsünü açın.
2. Kaldırılacak çözümleri seçin.
3. **Kaynakları Sil** ' e tıklayın ve ardından **Sil**' e tıklayarak kaldırılacak kaynakları onaylayın.

![Çözümleri silme](media/move-workspace/delete-solutions.png)

### <a name="delete-using-powershell"></a>PowerShell kullanarak silme

PowerShell 'i kullanarak çözümleri kaldırmak için, aşağıdaki örnekte gösterildiği gibi [Remove-AzResource](/powershell/module/az.resources/remove-azresource) cmdlet 'ini kullanın:

``` PowerShell
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking(<workspace-name>)" -ResourceGroupName <resource-group-name>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates(<workspace-name>)" -ResourceGroupName <resource-group-name>
Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM(<workspace-name>)" -ResourceGroupName <resource-group-name>
```

### <a name="remove-alert-rules-for-startstop-vms-solution"></a>VM 'Leri Başlat/Durdur çözümünün uyarı kurallarını kaldır
**VM 'Leri Başlat/Durdur** çözümünü kaldırmak için, çözüm tarafından oluşturulan uyarı kurallarını da kaldırmanız gerekir. Bu kuralları kaldırmak için Azure portal aşağıdaki yordamı kullanın.

1. **İzleyici** menüsünü açın ve ardından **Uyarılar**' ı seçin.
2. **Uyarı kurallarını yönet**' e tıklayın.
3. Aşağıdaki üç uyarı kuralını seçin ve ardından **Sil**' e tıklayın.

   - AutoStop_VM_Child
   - ScheduledStartStop_Parent
   - SequencedStartStop_Parent

    ![Kuralları silme](media/move-workspace/delete-rules.png)

## <a name="unlink-automation-account"></a>Otomasyon hesabının bağlantısını kaldır
Azure portal kullanarak Otomasyon hesabının çalışma alanından bağlantısını kaldırmak için aşağıdaki yordamı kullanın:

1. **Otomasyon hesapları** menüsünü açın ve kaldırılacak hesabı seçin.
2. Menünün **Ilgili kaynaklar** bölümünde **bağlantılı çalışma alanı**' nı seçin. 
3. Çalışma alanının Otomasyon hesabından bağlantısını kaldırmak için **çalışma alanının bağlantısını kaldır** ' a tıklayın.

    ![Çalışma alanının bağlantısını kaldırma](media/move-workspace/unlink-workspace.png)

## <a name="move-your-workspace"></a>Çalışma alanınızı taşıyın

### <a name="azure-portal"></a>Azure portalı
Azure portal kullanarak çalışma alanınızı taşımak için aşağıdaki yordamı kullanın:

1. **Log Analytics çalışma alanları** menüsünü açın ve ardından çalışma alanınızı seçin.
2. **Genel bakış** sayfasında, **kaynak grubu** veya **abonelik**' ın yanındaki **Değiştir** ' e tıklayın.
3. Çalışma alanıyla ilgili kaynak listesini içeren yeni bir sayfa açılır. Çalışma alanıyla aynı hedef aboneliğe ve kaynak grubuna taşınacak kaynakları seçin. 
4. Hedef **aboneliği** ve **kaynak grubunu** seçin. Çalışma alanını aynı abonelik içindeki başka bir kaynak grubuna taşıyorsanız, **abonelik** seçeneğini görmezsiniz.
5. Çalışma alanını ve seçili kaynakları taşımak için **Tamam** ' ı tıklatın.

    ![Ekran görüntüsü, kaynak grubunu ve abonelik adını değiştirme seçenekleri ile Log Analytics çalışma alanındaki genel bakış bölmesini gösterir.](media/move-workspace/portal.png)

### <a name="powershell"></a>PowerShell
Çalışma alanınızı PowerShell kullanarak taşımak için [Move-AzResource](/powershell/module/AzureRM.Resources/Move-AzureRmResource) öğesini aşağıdaki örnekte olduğu gibi kullanın:

``` PowerShell
Move-AzResource -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyResourceGroup01/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace" -DestinationSubscriptionId "00000000-0000-0000-0000-000000000000" -DestinationResourceGroupName "MyResourceGroup02"
```

> [!IMPORTANT]
> Taşıma işleminden sonra, çalışma alanını önceki durumuna geri getirmek için kaldırılan çözümlerin ve Otomasyon hesabı bağlantısının yeniden yapılandırılması gerekir.


## <a name="next-steps"></a>Sonraki adımlar
- Hangi kaynakların taşınmasını desteklediğini bir liste için bkz. [kaynaklar Için taşıma işlemi desteği](../../azure-resource-manager/management/move-support-resources.md).
