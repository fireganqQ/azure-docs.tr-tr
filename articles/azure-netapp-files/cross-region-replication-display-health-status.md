---
title: Azure NetApp Files çoğaltma ilişkisinin sistem durumunu görüntüle | Microsoft Docs
description: Kaynak birimde veya Azure NetApp Files hedef biriminde çoğaltma durumunun nasıl görüntüleneceğini açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: a14656bc1abc42fdc3e948fdae1ca07469681d21
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2020
ms.locfileid: "95249861"
---
# <a name="display-health-status-of-replication-relationship"></a>Çoğaltma ilişkisinin uygunluk durumunu görüntüleme 

Çoğaltma durumunu kaynak birimde veya hedef birimde görüntüleyebilirsiniz.  

## <a name="display-replication-status"></a>Çoğaltma durumunu görüntüle

1. Kaynak birimden veya hedef birimden, her iki birim için de depolama hizmeti altında **çoğaltma** ' ya tıklayın.

    Aşağıdaki çoğaltma durumu ve sistem durumu bilgileri görüntülenir:  
    * **Uç nokta türü** – birimin çoğaltmanın kaynağı mı yoksa hedefi mi olduğunu tanımlar.
    * **Sistem durumu** – çoğaltma ilişkisinin sistem durumunu görüntüler.
    * **Yansıtma durumu** – aşağıdaki değerlerden birini gösterir:
        * *Başlatılmamış*:  
            Bu, bir eşleme ilişkisi oluşturulduğunda ilk ve varsayılan durumdur. Başlatma başarıyla tamamlanana kadar durum başlatılmamış olarak kalır.
        * *Yansıtılmış*:   
            Hedef birim başlatıldı ve yansıtma güncelleştirmelerini almaya hazırlanıyor.
        * *Bozuk*:   
            Bu durum, eşleme ilişkisini bozduktan sonraki durumdur. Hedef birim `‘RW’` ve anlık görüntüler vardır.
    * **İlişki durumu** – aşağıdaki değerlerden birini gösterir: 
        * *Boşta*:  
            Devam eden bir aktarım işlemi yok ve gelecekteki aktarımlar devre dışı değil.
        * *Aktarılıyor*:  
            Bir aktarım işlemi devam ediyor ve gelecekteki aktarımlar devre dışı değil.
    * **Çoğaltma zamanlaması** : başlatma (ana hat kopyası) tamamlandığında artımlı yansıtma güncelleştirmelerinin ne sıklıkta gerçekleştirileceğini gösterir.

    * **Toplam ilerleme** --geçerli aktarım işlemi için aktarılan bayt cinsinden toplam veri miktarını gösterir. Bu miktar, aktarılan gerçek bittir ve kaynak ve hedef birimlerin rapor aldığı mantıksal alandan farklı olabilir.  

    ![Çoğaltma sistem durumu](../media/azure-netapp-files/cross-region-replication-health-status.png)

> [!NOTE] 
> Çoğaltma ilişkisi, önceki çoğaltma işleri tamamlanmadıysa sistem durumunu *sağlıksız* olarak gösterir. Bu durum, daha düşük bir aktarım penceresiyle (örneğin, büyük bir birim için on dakikalık bir aktarım süresi) aktarılmakta olan büyük birimlerin bir sonucudur. Bu durumda, ilişki durumu *aktarma* ve sistem durumunun *sağlıksız* olduğunu gösterir.

## <a name="next-steps"></a>Sonraki adımlar  

* [Bölgeler arası çoğaltma](cross-region-replication-introduction.md)
* [Olağanüstü durum kurtarmayı yönetme](cross-region-replication-manage-disaster-recovery.md)
* [Birim çoğaltma ölçümleri](azure-netapp-files-metrics.md#replication)
* [Birim çoğaltmaları veya birimleri silme](cross-region-replication-delete.md)
* [Bölgeler arası çoğaltma sorunlarını giderme](troubleshoot-cross-region-replication.md)

