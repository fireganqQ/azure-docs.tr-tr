---
title: SYNAPSE çalışma alanı için sürekli tümleştirme ve teslim
description: Çalışma alanındaki değişiklikleri bir ortamdan (geliştirme, test, üretim) diğerine dağıtmak için sürekli tümleştirme ve teslimi nasıl kullanacağınızı öğrenin.
services: synapse-analytics
author: liud
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: liud
ms.reviewer: pimorano
ms.openlocfilehash: 5f82e8b7359b90d5127e2c20a2b89cc5ad739a56
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2021
ms.locfileid: "99624784"
---
# <a name="continuous-integration-and-delivery-for-azure-synapse-workspace"></a>Azure SYNAPSE çalışma alanı için sürekli tümleştirme ve teslim

## <a name="overview"></a>Genel Bakış

Sürekli tümleştirme (CI), bir takım üyesinin sürüm denetimine değişiklik yaptığı her seferinde kodun derlemesini ve test edilmesini otomatikleştirme işlemidir. Sürekli dağıtım (CD), üretim ortamına birden çok test veya hazırlık ortamından derleme, test etme, yapılandırma ve dağıtma işlemidir.

Azure SYNAPSE çalışma alanı için, sürekli tümleştirme ve teslim (CI/CD) tüm varlıkları bir ortamdan (geliştirme, test, üretim) diğerine taşır. Çalışma alanınızı başka bir çalışma alanına yükseltmek için iki bölüm vardır: çalışma alanı kaynakları oluşturmak veya güncelleştirmek için [Azure Resource Manager şablonları](../../azure-resource-manager/templates/overview.md) kullanın (havuzlar ve çalışma alanı); yapıtları (SQL betikleri, Not defteri, Spark iş tanımı, işlem hatları, veri kümeleri, veri akışları vb.) Azure DevOps 'daki SYNAPSE CI/CD araçlarıyla geçirin. 

Bu makale, bir Synapse çalışma alanının birden çok ortama dağıtımını otomatik hale getirmek için Azure sürüm ardışık düzeni kullanılarak ana hatlarıyla sunulacaktır.

## <a name="prerequisites"></a>Önkoşullar

-   Geliştirme için kullanılan çalışma alanı Studio 'daki bir git deposu ile yapılandırılmış, bkz. [SYNAPSE Studio 'Da kaynak denetimi](source-control.md).
-   Bir Azure DevOps projesi, yayın işlem hattını çalıştırmak için hazırlandı.

## <a name="set-up-a-release-pipelines"></a>Yayın işlem hatlarını ayarlama

1.  [Azure DevOps](https://dev.azure.com/)'da, yayın için oluşturulan projeyi açın.

1.  Sayfanın sol tarafında, işlem **hatları**' nı seçin ve ardından **yayınlar**' ı seçin.

    ![İşlem hatları, yayınlar seçin](media/create-release-1.png)

1.  **Yeni işlem hattı**' nı seçin veya mevcut işlem hatlarınız varsa **Yeni** ' yi ve ardından **Yeni yayın** işlem hattını seçin.

1.  **Boş iş** şablonunu seçin.

    ![Boş işi seçin](media/create-release-select-empty.png)

1.  **Aşama adı** kutusuna ortamınızın adını girin.

1.  **Yapıt Ekle**' yi seçin ve ardından geliştirme SYNAPSE Studio ile yapılandırılmış Git deposunu seçin. Havuzların ve çalışma alanının ARM şablonunu yönetmek için kullandığınız Git deposunu seçin. Kaynak olarak GitHub kullanıyorsanız, GitHub hesabınız ve çekme depolarınız için bir hizmet bağlantısı oluşturmanız gerekir. [Hizmet bağlantısı](/azure/devops/pipelines/library/service-endpoints) hakkında daha fazla bilgi için 

    ![Yayın dalı Ekle](media/release-creation-github.png)

1.  Kaynak güncelleştirmesi için ARM şablonunun dalını seçin. **Varsayılan sürüm** için **varsayılan daldan en son**' u seçin.

    ![ARM Şablonu Ekle](media/release-creation-arm-branch.png)

1.  **Varsayılan dal** için deponun [Yayımla dalını](source-control.md#configure-publishing-settings) seçin. Bu yayın dalı varsayılan olarak `workspace_publish` . **Varsayılan sürüm** için **varsayılan daldan en son**' u seçin.

    ![Yapıt ekleme](media/release-creation-publish-branch.png)

## <a name="set-up-a-stage-task-for-arm-resource-create-and-update"></a>ARM kaynağı oluşturma ve güncelleştirme için bir aşama görevi ayarlama 

Çalışma alanı ve havuzlar dahil olmak üzere kaynakları oluşturmak veya güncelleştirmek için bir Azure Resource Manager Dağıtım görevi ekleyin:

1. Aşama görünümünde, **aşama görevlerini görüntüle**' yi seçin.

    ![Aşama görünümü](media/release-creation-stage-view.png)

1. Yeni bir görev oluşturun. **ARM şablon dağıtımı** araması yapın ve ardından **Ekle**' yi seçin.

1. Dağıtım görevinde, hedef çalışma alanı için abonelik, kaynak grubu ve konum ' u seçin. Gerekirse kimlik bilgilerini sağlayın.

1. **Eylem** listesinde, **kaynak grubunu oluştur veya Güncelleştir**' i seçin.

1. **Şablon** kutusunun yanındaki üç nokta düğmesini (**...**) seçin. Hedef çalışma alanınızın Azure Resource Manager şablonuna gözatamazsınız

1. Seç **...** **şablon parametreleri** kutusunun yanındaki parametreler dosyasını seçin.

1. Seç **...** **şablon parametrelerini geçersiz kıl** kutusunun yanında, hedef çalışma alanı için istenen parametre değerlerini girin. 

1. **Dağıtım modu** için **artımlı** ' ı seçin.
    
    ![çalışma alanı ve havuzlar dağıtım](media/pools-resource-deploy.png)

1. Seçim Çalışma alanı rol atamasını verme ve güncelleştirme için **Azure PowerShell** ekleyin. Bir Synapse çalışma alanı oluşturmak için yayın işlem hattını kullanıyorsanız, işlem hattının hizmet sorumlusu varsayılan çalışma alanı yöneticisi olarak eklenir. Diğer hesaplara çalışma alanına erişim vermek için PowerShell 'i çalıştırabilirsiniz. 
    
    ![izin ver](media/release-creation-grant-permission.png)

 > [!WARNING]
> Tüm dağıtım modunda, kaynak grubunda bulunan ancak yeni Kaynak Yöneticisi şablonunda belirtilmeyen kaynaklar **silinir**. Daha fazla bilgi için lütfen [Azure Resource Manager dağıtım modlarına](../../azure-resource-manager/templates/deployment-modes.md) başvurun

## <a name="set-up-a-stage-task-for-artifacts-deployment"></a>Yapıt dağıtımı için bir aşama görevi ayarlama 

SYNAPSE çalışma alanı, SQL betiği, Not defteri, Spark iş tanımı, dataflow, işlem hattı, bağlantılı hizmet, kimlik bilgileri ve IR (Integration Runtime) gibi diğer öğeleri dağıtmak için [SYNAPSE çalışma alanı dağıtım](https://marketplace.visualstudio.com/items?itemName=AzureSynapseWorkspace.synapsecicd-deploy) uzantısı ' nı kullanın.  

1. **Azure DevOps marketi**'nden uzantıyı arayın ve alın (https://marketplace.visualstudio.com/azuredevops) 

     ![Uzantıyı al](media/get-extension-from-market.png)

1. Uzantıyı yüklemek için bir kuruluş seçin. 

     ![Uzantıyı yükleme](media/install-extension.png)

1. Azure DevOps işlem hattının hizmet sorumlusuna abonelik izni verildiğinden ve aynı zamanda hedef çalışma alanı için çalışma alanı yöneticisi olarak atanmış olduğundan emin olun. 

1. Yeni bir görev oluşturun. **SYNAPSE çalışma alanı dağıtımı** araması yapın ve ardından **Ekle**' yi seçin.

     ![Uzantı Ekle](media/add-extension-task.png)

1.  Görevde **..** . seçeneğini belirleyin. şablon dosyasını seçmek için **şablon** kutusunun yanındaki öğesini seçin.

1. Seç **...** **şablon parametreleri** kutusunun yanındaki parametreler dosyasını seçin.

1. Hedef çalışma alanının bağlantısını, kaynak grubunu ve adını seçin. 

1. Seç **...** **şablon parametrelerini geçersiz kıl** kutusunun yanında, hedef çalışma alanı için istenen parametre değerlerini girin. 

    ![SYNAPSE çalışma alanı dağıtma](media/create-release-artifacts-deployment.png)

> [!IMPORTANT]
> CI/CD senaryolarında, farklı ortamlardaki Integration Runtime (IR) türü aynı olmalıdır. Örneğin, geliştirme ortamında şirket içinde barındırılan bir IR varsa, aynı IR, test ve üretim gibi diğer ortamlarda da kendi kendine barındırılan türde olmalıdır. Benzer şekilde, tümleştirme çalışma zamanlarını birden çok aşamada paylaşıyorsanız, tümleştirme çalışma zamanlarını, geliştirme, test ve üretim gibi tüm ortamlarda bağlanmış bir şekilde yapılandırmanız gerekir.

## <a name="create-release-for-deployment"></a>Dağıtım için yayın oluştur 

Tüm değişiklikleri kaydettikten sonra bir yayını el ile oluşturmak için **yayın oluştur** ' u seçebilirsiniz. Yayınların oluşturulmasını otomatikleştirmek için bkz. [Azure DevOps yayın Tetikleyicileri](/azure/devops/pipelines/release/triggers)

   ![Yayın oluştur ' u seçin](media/release-creation-manually.png)

## <a name="best-practices-for-cicd"></a>CI/CD için en iyi yöntemler

SYNAPSE çalışma alanınız ile git tümleştirmesi kullanıyorsanız ve değişikliklerinizi geliştirmeden test ve daha sonra üretime taşıyan bir CI/CD işlem hattına sahipseniz, bu en iyi yöntemleri öneririz:

-   **Git tümleştirmesi**. Git tümleştirmesiyle yalnızca geliştirme SYNAPSE çalışma alanınızı yapılandırın. Test ve üretim çalışma alanlarındaki değişiklikler CI/CD aracılığıyla dağıtılır ve git tümleştirmesi gerekmez.
-   **Yapıları yapıtları geçişten önce hazırlayın**. Geliştirme çalışma alanındaki havuzlara eklenmiş SQL betiği veya Not defteriniz varsa, farklı ortamlardaki havuzların adı da beklenmektedir. 
-   **Kod olarak altyapı (IAC)**. Bir açıklayıcı modelde altyapının (ağlar, sanal makineler, yük dengeleyiciler ve bağlantı topolojisi) yönetimi, DevOps ekibinin kaynak kodu için kullandığı sürüm oluşturma 'yı kullanır. 
-   **Diğerleri**. Bkz. [ADF yapıtları için en iyi uygulamalar](../../data-factory/continuous-integration-deployment.md#best-practices-for-cicd)

## <a name="troubleshooting-artifacts-deployment"></a>Yapıt dağıtımı sorunlarını giderme 

### <a name="use-the-synapse-workspace-deployment-task"></a>SYNAPSE çalışma alanı dağıtım görevini kullanın

SYNAPSE ' de ARM kaynakları olmayan bir dizi yapıtlar vardır. Bu Azure Data Factory farklıdır. ARM şablonu Dağıtım görevi SYNAPSE yapıtları dağıtmak için düzgün çalışmayacak
 
### <a name="unexpected-token-error-in-release"></a>Yayında beklenmeyen belirteç hatası

Parametre dosyanızda, kaçış olmayan parametre değerleri varsa, yayın işlem hattı dosyayı ayrıştırmaz ve "beklenmeyen belirteç" hatasını üretir. Parametre değerlerini almak için, parametreleri geçersiz kılmayı veya Azure Anahtar Kasası 'nı kullanmanızı öneririz. Geçici çözüm olarak çift kaçış karakterleri de kullanabilirsiniz.
