---
title: PowerShell Galerisi 'de Azure Otomasyonu runbook 'larını ve modüllerini kullanma
description: Bu makalede, Microsoft 'tan ve topluluk PowerShell Galerisi içindeki runbook 'ları ve modülleri nasıl kullanacağınız açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 01/08/2021
ms.topic: conceptual
ms.openlocfilehash: 590220782a7f43e785cc7885e68eefa99afb7d1d
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98049125"
---
# <a name="use-runbooks-and-modules-in-powershell-gallery"></a>PowerShell Galerisi’nde runbook’ları ve modülleri kullanma

Azure Otomasyonu 'nda kendi runbook 'larınızı ve modüllerinizi oluşturmak yerine, Microsoft ve Community tarafından önceden oluşturulmuş senaryolara erişebilirsiniz. Azure Otomasyonu GitHub kuruluşundaki PowerShell Galerisi ve [Python runbook](#use-python-runbooks) 'lardan PowerShell runbook 'ları ve [modülleri](#modules-in-powershell-gallery) alabilirsiniz. Ayrıca, [geliştirdiğiniz senaryoları](#add-a-powershell-runbook-to-the-gallery)paylaşarak topluluğa katkıda bulunabilirsiniz.

> [!NOTE]
> TechNet Betik Merkezi devre dışı bırakılıyor. Runbook galerisindeki betik merkezi 'ndeki tüm runbook 'lar, daha fazla [bilgi için bkz](https://github.com/azureautomation) . [buraya](https://techcommunity.microsoft.com/t5/azure-governance-and-management/azure-automation-runbooks-moving-to-github/ba-p/2039337)bakın.

## <a name="runbooks-in-powershell-gallery"></a>PowerShell Galerisi runbook 'lar

[PowerShell Galerisi](https://www.powershellgallery.com/packages) , Microsoft 'un yanı sıra Azure Otomasyonu 'na aktarabileceğiniz topluluk için çeşitli runbook 'lar sağlar. Bir tane kullanmak için, Galeriden bir runbook indirin veya Runbook 'tan veya Azure portal Otomasyon hesabınızdan doğrudan runbook 'ları içeri aktarabilirsiniz.

> [!NOTE]
> Grafik runbook 'ları PowerShell Galerisi desteklenmez.

Azure portal kullanarak yalnızca PowerShell Galerisi doğrudan içeri aktarabilirsiniz. PowerShell kullanarak bu işlevi gerçekleştiremezsiniz.

> [!NOTE]
> PowerShell Galerisi aldığınız tüm runbook 'ların içeriğini doğrulamanız ve bunları bir üretim ortamına yüklemek ve çalıştırmak için çok dikkatli olmanız gerekir.

## <a name="modules-in-powershell-gallery"></a>PowerShell Galerisi modüller

PowerShell modülleri, runbook 'larınızda kullanabileceğiniz cmdlet 'leri içerir ve Azure Otomasyonu 'nda yükleyebileceğiniz mevcut modüller [PowerShell Galerisi](https://www.powershellgallery.com)kullanılabilir. Bu galeriyi Azure portal başlatabilir ve doğrudan Azure Otomasyonu 'na yükleyebilirsiniz. Ayrıca, bunları indirip el ile yükleyebilirsiniz.

## <a name="common-scenarios-available-in-powershell-gallery"></a>PowerShell Galerisi 'de bulunan yaygın senaryolar

Aşağıdaki liste, yaygın senaryoları destekleyen birkaç runbook içerir. Azure Otomasyonu ekibi tarafından oluşturulan runbook 'ların tam listesi için bkz. [Azureautomationteam profile](https://www.powershellgallery.com/profiles/AzureAutomationTeam).

   * [Update-ModulesInAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/) -PowerShell Galerisi bir Otomasyon hesabındaki tüm modüllerin en son sürümünü içeri aktarır.
   * [Enable-AzureDiagnostics](https://www.powershellgallery.com/packages/Enable-AzureDiagnostics/) -iş durumu ve iş akışları Içeren Azure Otomasyonu günlüklerini almak için Azure Tanılama ve Log Analytics yapılandırır.
   * [Copy-ıtemfrolaurevm](https://www.powershellgallery.com/packages/Copy-ItemFromAzureVM/) -bir Windows Azure sanal makinesinden uzak bir dosyayı kopyalar.
   * [Copy-ItemToAzureVM](https://www.powershellgallery.com/packages/Copy-ItemToAzureVM/) -bir Azure sanal makinesine yerel bir dosya kopyalar.

## <a name="import-a-powershell-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Azure portal ile runbook galerisinden bir PowerShell runbook 'unu içeri aktarın

1. Azure portalında, Otomasyon hesabınızı açın.
2. **Işlem Otomasyonu** altında **runbook 'lar Galerisi** ' ni seçin.
3. **Kaynak seçin: PowerShell Galerisi**.
4. İstediğiniz Galeri öğesini bulun ve ayrıntılarını görüntülemek için seçin. Sol tarafta yayımcı için ek arama parametreleri girebilir ve yazın.

   ![Galeriye göz atma](media/automation-runbook-gallery/browse-gallery.png)

5. [Azure Otomasyonu GitHub kuruluşunda](https://github.com/azureautomation)öğeyi görüntülemek için **kaynak projeyi görüntüle** ' ye tıklayın.
6. Bir öğeyi içeri aktarmak için üzerine tıklayarak ayrıntılarını görüntüleyin ve ardından **Içeri aktar**' a tıklayın.

   ![İçeri Aktar düğmesi](media/automation-runbook-gallery/gallery-item-detail.png)

7. İsteğe bağlı olarak, runbook 'un adını değiştirin ve ardından runbook 'u içeri aktarmak için **Tamam** ' ı tıklatın.
8. Runbook, Otomasyon hesabı için **runbook 'lar** sekmesinde görünür.

## <a name="add-a-powershell-runbook-to-the-gallery"></a>Galeriye PowerShell runbook 'u ekleme

Microsoft, diğer müşteriler için yararlı olacağını düşündüğünüz PowerShell Galerisi runbook eklemenizi önerir. PowerShell Galerisi PowerShell modüllerini ve PowerShell betiklerini kabul eder. Bir runbook 'u [PowerShell Galerisi yükleyerek](/powershell/scripting/gallery/how-to/publishing-packages/publishing-a-package)ekleyebilirsiniz.

## <a name="import-a-module-from-the-module-gallery-with-the-azure-portal"></a>Modül galerisinden bir modülü Azure portal içeri aktarma

1. Azure portalında, Otomasyon hesabınızı açın.
2. Modüllerin listesini açmak için **paylaşılan kaynaklar** altındaki **modüller** ' i seçin.
3. Sayfanın üst kısmından **tarayıcı Galerisi** ' ne tıklayın.

   ![Modül Galerisi](media/automation-runbook-gallery/modules-blade.png)

4. Galeriye gözatamıyorum sayfasında, aşağıdaki alanlara göre arama yapabilirsiniz:

   * Modül Adı
   * Etiketler
   * Yazma
   * Cmdlet/DSC kaynak adı

5. İlgilendiğiniz bir modül bulun ve ayrıntılarını görüntülemek için seçin.

   Belirli bir modülün detayına göre daha fazla bilgi görüntüleyebilirsiniz. Bu bilgiler PowerShell Galerisi, tüm gerekli bağımlılıkları ve modülün içerdiği tüm cmdlet 'leri ya da DSC kaynaklarını geri bir bağlantı içerir.

   ![PowerShell modülü ayrıntıları](media/automation-runbook-gallery/gallery-item-details-blade.png)

6. Modülü doğrudan Azure Otomasyonu 'na yüklemek için **Içeri aktar**' a tıklayın.
7. Içeri aktarma bölmesinde, içeri aktarılacak modülün adını görebilirsiniz. Tüm bağımlılıklar yüklüyse, **Tamam** düğmesi etkinleştirilir. Bağımlılıklarınız eksikse, bu modülü içeri aktarmadan önce bu bağımlılıkları içeri aktarmanız gerekir.
8. Içeri aktarma bölmesinde, modülü içeri aktarmak için **Tamam** ' ı tıklatın. Azure Otomasyonu hesabınıza bir modül içeri aktarırken, modül ve cmdlet 'ler hakkında meta verileri ayıklar. Bu eylem, her etkinliğin ayıklanması gerektiğinden birkaç dakika sürebilir.
9. Modülün dağıtılmakta olduğu bir başlangıç bildirimi ve tamamlandığında başka bir bildirim alırsınız.
10. Modül içeri aktarıldıktan sonra, kullanılabilir etkinlikleri görebilirsiniz. Runbook 'larınızda ve DSC kaynaklarınızın modül kaynaklarını kullanabilirsiniz.

> [!NOTE]
> Yalnızca PowerShell Core 'u destekleyen modüller Azure Otomasyonu 'nda desteklenmez ve Azure portal içeri aktarılamaz ya da doğrudan PowerShell Galerisi dağıtılır.

## <a name="use-python-runbooks"></a>Python runbook 'larını kullanma

Python runbook 'Ları, [Azure Otomasyonu GitHub kuruluşunda](https://github.com/azureautomation)bulunabilir. GitHub deponuzda katkıda bulundığınızda, katkılarınızı karşıya yüklerken Tag **(GitHub konu): Python3** ekleyin.

## <a name="request-a-runbook-or-module"></a>Runbook veya modül isteme

İstekleri [Kullanıcı seslerine](https://feedback.azure.com/forums/246290-azure-automation/)gönderebilirsiniz.  Bir Runbook yazma veya PowerShell hakkında sorularınız ile ilgili yardıma ihtiyacınız varsa [Microsoft Q&soru sayfasına](/answers/topics/azure-automation.html)bir soru gönderin.

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell runbook 'unu kullanmaya başlamak için bkz. [öğretici: PowerShell runbook 'U oluşturma](learn/automation-tutorial-runbook-textual-powershell.md).
* Runbook 'larla çalışmak için bkz. [Azure Otomasyonu 'nda runbook 'Ları yönetme](manage-runbooks.md).
* PowerShell ayrıntıları için bkz. [PowerShell belgeleri](/powershell/scripting/overview).
* PowerShell cmdlet başvurusu için bkz. [az. Automation](/powershell/module/az.automation).
