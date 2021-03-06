---
title: Azure DevTest Labs bir VM için otomatik başlatma ayarlarını yapılandırma | Microsoft Docs
description: Laboratuvardaki VM 'Ler için otomatik başlatma ayarlarını yapılandırmayı öğrenin. Bu ayar, laboratuvardaki VM 'Lerin zamanlamaya göre otomatik olarak başlatılmasını sağlar.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 83e7b0836273a59eaaf66471bd0cb42d63ccf1c3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91328496"
---
# <a name="auto-startup-lab-virtual-machines"></a>Otomatik başlatma Laboratuvarı sanal makineleri  
Azure DevTest Labs, laboratuvarınızda sanal makineleri bir zamanlamaya göre otomatik olarak başlatılıp kapatılacak şekilde yapılandırmanıza olanak tanır. Oto kapatma ayarlarını yapılandırma hakkında daha fazla bilgi için bkz. [Azure DevTest Labs laboratuvar için oto kapatma Ilkelerini yönetme](devtest-lab-auto-shutdown.md). 

Otomatik kapatmadan farklı olarak, ilke açıldığında tüm VM 'Ler dahil edildiğinde, otomatik başlatma ilkesi laboratuvar kullanıcısının bir VM 'yi açık olarak seçmesini ve bu zamanlamayı kabul edilmesini gerektirir. Bu şekilde, istenmeyen sanal makinelerin yanlışlıkla otomatik olarak başlatıldığı ve beklenmeyen harcama gönderilmesine neden olan durumlarda kolayca çalıştırmayacağız.

Bu makalede, bir laboratuvar için otomatik başlatma ilkesini yapılandırma gösterilmektedir.

## <a name="configure-autostart-settings-for-a-lab"></a>Laboratuvar için otomatik başlatma ayarlarını yapılandırma 
1. Laboratuvarınızın giriş sayfasına gidin. 
2. Sol taraftaki menüden **yapılandırma ve ilkeler** ' i seçin. 

    ![DevTest laboratuvarında "yapılandırma ve ilkeler" menüsünü gösteren ekran görüntüsü.](./media/devtest-lab-auto-startup-vm/configuration-policies-menu.png)
3. **Yapılandırma ve ilkeler** sayfasında, aşağıdaki adımları uygulayın:
    
    1. Bu laboratuvar için autostart özelliğini etkinleştirmek üzere **sanal makinelerin otomatik başlatma için zamanlanmasını Izin ver** Için **Açık '** ı seçin. 
    2. **Zamanlama başlangıç** alanı için bir başlangıç saati seçin (örneğin: 8:00:00). 
    3. Kullanılacak bir **saat dilimi** seçin. 
    4. VM 'Lerin otomatik olarak başlatılması gereken **haftanın günlerini** seçin. 
    5. Ardından, ayarları kaydetmek için araç çubuğunda **Kaydet** ' i seçin. 

        ![Otomatik başlatma ayarları](./media/devtest-lab-auto-startup-vm/auto-start-configuration.png)

        > [!IMPORTANT]
        > Bu ilke, laboratuvardaki herhangi bir sanal makineye otomatik başlatma otomatik olarak uygulanmaz. Tek tek sanal makineleri **kabul** etmek için sanal makine sayfasına gidin ve bu VM için **Otomatik başlatmayı** etkinleştirin.

## <a name="enable-autostart-for-a-vm-in-the-lab"></a>Laboratuvardaki bir VM için autostart 'ı etkinleştirme
Aşağıdaki yordam, bir VM 'de laboratuvar için otomatik başlatma ilkesine yönelik adımları sağlar. 

1. Laboratuvarınızın giriş sayfasında **sanal makinelerim** listesinden **VM** 'yi seçin. 

    ![Yapılandırma ve ilkeler menüsü](./media/devtest-lab-auto-startup-vm/select-vm.png)
2. **Sanal makine** sayfasında, sol menüden veya **zamanlamalar** listesinden **autostart** ' ı seçin. 

    ![Autostart menüsünü seçin](./media/devtest-lab-auto-startup-vm/select-auto-start.png)
3. **Autostart** sayfasında, **Bu sanal makinenin otomatik başlatma Için zamanlanmasını izin ver** seçeneği için **Açık** ' ı seçin.

    ![VM için autostart 'ı etkinleştirme](./media/devtest-lab-auto-startup-vm/auto-start-vm.png)
4. Ardından, ayarı kaydetmek için araç çubuğunda **Kaydet** ' i seçin. 


## <a name="next-steps"></a>Sonraki adımlar
Laboratuvar için yapılandırma oto kapatma ilkesi hakkında bilgi edinmek için bkz. [Azure DevTest Labs laboratuvar için oto kapatma Ilkelerini yönetme](devtest-lab-auto-shutdown.md)
