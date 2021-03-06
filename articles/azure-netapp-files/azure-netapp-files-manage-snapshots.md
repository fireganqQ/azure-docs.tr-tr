---
title: Azure NetApp Files kullanarak anlık görüntüleri yönetme | Microsoft Docs
description: Azure NetApp Files kullanarak anlık görüntüleri oluşturmayı, yönetmeyi ve kullanmayı açıklar.
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
ms.date: 02/10/2021
ms.author: b-juche
ms.openlocfilehash: 4d992bcc202dc8bdacdda6426371df1adb1ec3e6
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100379123"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Azure NetApp Files kullanarak anlık görüntüleri yönetme

Azure NetApp Files, otomatik anlık görüntü oluşturmayı zamanlamak için isteğe bağlı anlık görüntüler oluşturmayı ve anlık görüntü ilkelerini kullanmayı destekler. Ayrıca, bir anlık görüntüyü yeni bir birime geri yükleyebilir, istemci kullanarak tek bir dosyayı geri yükleyebilir veya bir anlık görüntü kullanarak var olan bir birimi geri alabilirsiniz.

> [!NOTE] 
> Çapraz bölge çoğaltmasında anlık görüntü yönetimiyle ilgili konular için bkz. [çapraz bölge çoğaltmasını kullanma gereksinimleri ve konuları](cross-region-replication-requirements-considerations.md).

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Bir birim için isteğe bağlı anlık görüntü oluşturma

İsteğe bağlı birim anlık görüntüleri oluşturabilirsiniz. 

1.  Anlık görüntüsü oluşturmak istediğiniz birime gidin. **Anlık görüntüler**' e tıklayın.

    ![Anlık görüntülere git](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Bir birim için isteğe bağlı bir anlık görüntü oluşturmak için **+ anlık görüntü ekle** ' ye tıklayın.

    ![Anlık görüntü ekle](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  Yeni anlık görüntü penceresinde oluşturmakta olduğunuz yeni anlık görüntü için bir ad belirtin.   

    ![Yeni anlık görüntü](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. **Tamam**'a tıklayın. 

## <a name="manage-snapshot-policies"></a>Anlık görüntü ilkelerini yönetme

Anlık görüntü ilkeleri kullanarak, birim anlık görüntülerinin otomatik olarak alınmasını zamanlayabilirsiniz. Ayrıca, gerektiğinde bir anlık görüntü ilkesini değiştirebilir veya artık ihtiyacınız olmayan bir anlık görüntü ilkesini silebilirsiniz.  

### <a name="register-the-feature"></a>Özelliği kaydetme

**Anlık görüntü ilkesi** özelliği şu anda önizlemededir. Bu özelliği ilk kez kullanıyorsanız önce özelliği kaydetmeniz gerekir. 

1. Özelliği kaydedin: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSnapshotPolicy
    ```

2. Özellik kaydının durumunu denetleyin: 

    > [!NOTE]
    > **Registrationstate** , ' a `Registering` değiştirilmeden önce 60 dakikaya kadar bir durumda olabilir `Registered` . Devam etmeden önce durum **kaydoluncaya** kadar bekleyin.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSnapshotPolicy
    ```
Ayrıca, [Azure CLI komutlarını](/cli/azure/feature?preserve-view=true&view=azure-cli-latest) kullanarak `az feature register` `az feature show` özelliği kaydedebilir ve kayıt durumunu görüntüleyebilirsiniz. 

### <a name="create-a-snapshot-policy"></a>Anlık görüntü ilkesi oluşturma 

Anlık görüntü ilkesi, anlık görüntü oluşturma sıklığını saatlik, günlük, haftalık veya aylık Döngülerde belirtmenizi sağlar. Birim için tutulacak en fazla anlık görüntü sayısını da belirtmeniz gerekir.  

1.  NetApp hesabı görünümünde, **anlık görüntü ilkesi**' ne tıklayın.

    ![Anlık görüntü ilkesi gezintisi](../media/azure-netapp-files/snapshot-policy-navigation.png)

2.  Anlık görüntü Ilkesi penceresinde, Ilke durumunu **etkin** olarak ayarlayın. 

3.  Saatlik, günlük, haftalık veya aylık anlık görüntü ilkeleri oluşturmak için **saatlik**, **günlük**, **haftalık** veya **aylık** sekmesine tıklayın. **Saklanacak anlık görüntü sayısını** belirtin.  

    Bir birim için izin verilen en fazla anlık görüntü sayısı hakkında [Azure NetApp Files Için kaynak sınırlarına](azure-netapp-files-resource-limits.md) bakın. 

    Aşağıdaki örnek, saatlik anlık görüntü ilkesi yapılandırmasını gösterir. 

    ![Anlık görüntü ilkesi saatlik](../media/azure-netapp-files/snapshot-policy-hourly.png)

    Aşağıdaki örnek günlük anlık görüntü ilkesi yapılandırmasını gösterir.

    ![Anlık görüntü ilkesi günlük](../media/azure-netapp-files/snapshot-policy-daily.png)

    Aşağıdaki örnekte haftalık anlık görüntü ilkesi yapılandırması gösterilmektedir.

    ![Anlık görüntü ilkesi haftalık](../media/azure-netapp-files/snapshot-policy-weekly.png)

    Aşağıdaki örnek, aylık anlık görüntü ilkesi yapılandırmasını gösterir.

    ![Anlık görüntü ilkesi aylık](../media/azure-netapp-files/snapshot-policy-monthly.png) 

4.  **Kaydet**’e tıklayın.  

Ek anlık görüntü ilkeleri oluşturmanız gerekiyorsa 3. adımı yineleyin.
Oluşturduğunuz ilkeler anlık görüntü ilkesi sayfasında görünür.

Bir birimin anlık görüntü ilkesini kullanmasını istiyorsanız [ilkeyi birime uygulamanız](azure-netapp-files-manage-snapshots.md#apply-a-snapshot-policy-to-a-volume)gerekir. 

### <a name="apply-a-snapshot-policy-to-a-volume"></a>Bir birime anlık görüntü ilkesi uygulama

Bir birimin oluşturduğunuz bir anlık görüntü ilkesini kullanmasını istiyorsanız ilkeyi birime uygulamanız gerekir. 

Çapraz bölge çoğaltmasında bir hedef birime anlık görüntü ilkesi uygulayamazsınız.  

1.  **Birimler** sayfasına gidin, anlık görüntü ilkesi uygulamak istediğiniz birime sağ tıklayın ve **Düzenle**' yi seçin.

    ![Birimler sağ tıklama menüsü](../media/azure-netapp-files/volume-right-cick-menu.png) 

2.  Düzenle penceresinde, **anlık görüntü ilkesi** altında, birim için kullanılacak bir ilke seçin.  İlkeyi uygulamak için **Tamam** ' ı tıklatın.  

    ![Anlık görüntü ilkesi düzenleme](../media/azure-netapp-files/snapshot-policy-edit.png) 

### <a name="modify-a-snapshot-policy"></a>Anlık görüntü ilkesini değiştirme 

Mevcut bir anlık görüntü ilkesini, ilke durumunu, anlık görüntü sıklığını (saatlik, günlük, haftalık veya aylık) veya saklanacak anlık görüntü sayısını değiştirmek üzere değiştirebilirsiniz.  
 
1.  NetApp hesabı görünümünde, **anlık görüntü ilkesi**' ne tıklayın.

2.  Değiştirmek istediğiniz anlık görüntü ilkesine sağ tıkladıktan sonra **Düzenle**' yi seçin.

    ![Anlık görüntü ilkesi sağ tıklama menüsü](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Görüntülenen anlık görüntü Ilkesi penceresinde değişiklikleri yapın ve ardından **Kaydet**' e tıklayın. 

### <a name="delete-a-snapshot-policy"></a>Anlık görüntü ilkesini silme 

Artık saklamak istemediğiniz bir anlık görüntü ilkesini silebilirsiniz.   

1.  NetApp hesabı görünümünde, **anlık görüntü ilkesi**' ne tıklayın.

2.  Değiştirmek istediğiniz anlık görüntü ilkesine sağ tıklayıp **Sil**' i seçin.

    ![Anlık görüntü ilkesi sağ tıklama menüsü](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Anlık görüntü ilkesini silmek istediğinizi onaylamak için **Evet** ' e tıklayın.   

    ![Anlık görüntü ilkesi silme onayı](../media/azure-netapp-files/snapshot-policy-delete-confirm.png) 

## <a name="edit-the-hide-snapshot-path-option"></a>Anlık görüntü yolunu gizle seçeneğini düzenleme
Anlık görüntü yolunu Gizle seçeneği, bir birimin anlık görüntü yolunun görünür olup olmadığını denetler. Bir [NFS](azure-netapp-files-create-volumes.md#create-an-nfs-volume) veya [SMB](azure-netapp-files-create-volumes-smb.md#add-an-smb-volume) birimi oluşturulurken, anlık görüntü yolunun gizlenip gizlenmeyeceğini belirtme seçeneğiniz vardır. Daha sonra gerektiğinde anlık görüntü yolunu gizle seçeneğini düzenleyebilirsiniz.  

> [!NOTE]
> Çapraz bölge çoğaltmasında bir [hedef birim](cross-region-replication-create-peering.md#create-the-data-replication-volume-the-destination-volume) Için, anlık görüntü yolunu Gizle seçeneği varsayılan olarak etkindir ve ayar değiştirilemez. 

1. Bir birimin anlık görüntü yolunu Gizle seçenek ayarını görüntülemek için birimi seçin. **Anlık görüntü yolunu Gizle** alanı, seçeneğinin etkin olup olmadığını gösterir.   
    ![Anlık görüntü yolunu Gizle alanını açıklayan ekran görüntüsü.](../media/azure-netapp-files/hide-snapshot-path-field.png) 
2. Anlık görüntü yolunu gizle seçeneğini düzenlemek için, birim sayfasında **Düzenle** ' ye tıklayın ve ardından **anlık görüntü yolunu Gizle** seçeneğini gerektiği gibi değiştirin.   
    ![Birim anlık görüntüsünü Düzenle seçeneğini açıklayan ekran görüntüsü.](../media/azure-netapp-files/volume-edit-snapshot-options.png) 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Bir anlık görüntüyü yeni bir birime geri yükleme

Şu anda, bir anlık görüntüyü yalnızca yeni bir birime geri yükleyebilirsiniz. 
1. Anlık görüntü listesini göstermek için birim dikey penceresinden **anlık görüntüler** ' i seçin. 
2. Geri yüklemek için anlık görüntüye sağ tıklayın ve menü seçeneğinde **Yeni birime geri yükle** ' yi seçin.  

    ![Anlık görüntüyü yeni birime geri yükle](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

3. Birim Oluştur penceresinde, yeni birim için bilgi sağlayın:  
    * **Ada**   
        Oluşturmakta olduğunuz birim için ad belirtin.  
        
        Ad, kaynak grubu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır.  Tüm alfasayısal karakterler kullanılabilir.

    * **Kota**  
        Birime ayırmak istediğiniz mantıksal depolama miktarını belirtin.  

    ![Yeni birime geri yükle](../media/azure-netapp-files/snapshot-restore-new-volume.png) 

4. **Gözden geçir + oluştur**' a tıklayın.  **Oluştur**’a tıklayın.   
    Yeni birim, anlık görüntünün kullandığı protokolü kullanır.   
    Anlık görüntünün geri yüklendiği yeni birim birimler dikey penceresinde görünür.

## <a name="restore-a-file-from-a-snapshot-using-a-client"></a>Bir istemciyi kullanarak bir anlık görüntüden dosya geri yükleme

[Anlık görüntünün tamamını bir birime geri yüklemek](#restore-a-snapshot-to-a-new-volume)istemiyorsanız, bir dosyayı bir anlık görüntüden bağlı olan bir istemciyi kullanarak geri yükleme seçeneğiniz vardır.  

Takılan birim,  `.snapshot` istemci tarafından erişilebilen (NFS istemcileri 'nde) veya `~snapshot` (SMB istemcilerinde) adlı bir anlık görüntü dizini içerir. Snapshot dizini birimin anlık görüntülerine karşılık gelen alt dizinleri içerir. Her alt dizin, anlık görüntünün dosyalarını içerir. Yanlışlıkla bir dosyayı silerseniz veya üzerine yazarsanız, dosyayı bir anlık görüntü alt dizininden oku-yaz dizinine kopyalayarak dosyayı üst okuma-yazma dizinine geri yükleyebilirsiniz. 

Anlık görüntü [yolunu gizle seçeneğini](#edit-the-hide-snapshot-path-option)kullanarak anlık görüntü dizinlerine erişimi denetleyebilirsiniz. Bu seçenek, dizinin istemcilerinden gizlenmesi gerekip gerekmediğini denetler. Bu nedenle, anlık görüntülerle dosya ve klasörlere erişimi de denetler.  

NFSv 4.1, `.snapshot` dizini ( `ls -la` ) göstermiyor. Ancak, anlık görüntü yolunu Gizle seçeneği ayarlanmamışsa, `.snapshot` `cd <snapshot-path>` istemci komut satırındaki komutunu kullanarak NFSv 4.1 aracılığıyla dizine erişmeye devam edebilirsiniz. 

### <a name="restore-a-file-by-using-a-linux-nfs-client"></a>Linux NFS istemcisi kullanarak bir dosyayı geri yükleme 

1. `ls`Dizinden geri yüklemek istediğiniz dosyayı listelemek için Linux komutunu kullanın `.snapshot` . 

    Örneğin:

    `$ ls my.txt`   
    `ls: my.txt: No such file or directory`   

    `$ ls .snapshot`   
    `daily.2020-05-14_0013/              hourly.2020-05-15_1106/`   
    `daily.2020-05-15_0012/              hourly.2020-05-15_1206/`   
    `hourly.2020-05-15_1006/             hourly.2020-05-15_1306/`   

    `$ ls .snapshot/hourly.2020-05-15_1306/my.txt`   
    `my.txt`

2. `cp`Dosyayı üst dizine kopyalamak için komutunu kullanın.  

    Örneğin: 

    `$ cp .snapshot/hourly.2020-05-15_1306/my.txt .`   

    `$ ls my.txt`   
    `my.txt`   

### <a name="restore-a-file-by-using-a-windows-client"></a>Bir Windows istemcisi kullanarak bir dosyayı geri yükleme 

1. `~snapshot`Birimin dizini gizliyse, görüntülenecek üst dizindeki [gizli öğeleri gösterin](https://support.microsoft.com/help/4028316/windows-view-hidden-files-and-folders-in-windows-10) `~snapshot` .

    ![Gizli öğeleri gösterme](../media/azure-netapp-files/snapshot-show-hidden.png) 

2. `~snapshot`Geri yüklemek istediğiniz dosyayı bulmak için içindeki alt dizine gidin.  Dosyaya sağ tıklayın. **Kopyala**’yı seçin.  

    ![Dosyayı geri yüklenecek şekilde Kopyala](../media/azure-netapp-files/snapshot-copy-file-restore.png) 

3. Üst dizine geri dönün. Üst dizine sağ tıklayıp `Paste` dosyayı dizine yapıştırmayı seçin.

    ![Geri yüklenecek dosyayı Yapıştır](../media/azure-netapp-files/snapshot-paste-file-restore.png) 

4. Ayrıca, üst dizine sağ tıklayıp **Özellikler**' i seçip, **önceki sürümler** sekmesine tıklayarak anlık görüntü listesini görebilir ve bir dosyayı geri yüklemek için **geri yükle** ' yi seçebilirsiniz.  

    ![Önceki sürümlere özellikler](../media/azure-netapp-files/snapshot-properties-previous-version.png) 

## <a name="revert-a-volume-using-snapshot-revert"></a>Anlık görüntü döndürmeyi kullanarak bir birimi döndürür

Anlık görüntü alma işlevselliği, bir birimi belirli bir anlık görüntü çekilirken bulunduğu duruma hızlıca döndürmenizi sağlar. Çoğu durumda, bir birimin geri döndürülmesi, tek tek dosyaları bir anlık görüntüden etkin dosya sistemine geri yüklemeden çok daha hızlıdır. Ayrıca, bir anlık görüntünün yeni bir birime geri yüklenmesi ile karşılaştırıldığında daha fazla alan da daha etkilidir. 

Birimin anlık görüntüler menüsünde birimi çevir seçeneğini bulabilirsiniz. Yeniden sürüm için bir anlık görüntü seçtikten sonra, Azure NetApp Files, birimi seçilen anlık görüntü alındığı sırada içerdiği verilere ve zaman damgalarına geri döndürür. 

> [!IMPORTANT]
> Seçili anlık görüntü alındıktan sonra gerçekleştirilen etkin dosya sistemi verileri ve anlık görüntüler kaybedilir. Anlık görüntü değiştirme işlemi, Hedeflenen birimdeki *Tüm* verilerin seçilen anlık görüntüdeki verilerle değiştirilir. Bir anlık görüntü seçtiğinizde anlık görüntü içeriği ve oluşturma tarihi ile ilgilenmelisiniz. Anlık görüntü geri alma işlemini geri alamazsınız.

1. Bir birimin **anlık görüntüler** menüsüne gidin.  Alma işlemi için kullanmak istediğiniz anlık görüntüye sağ tıklayın. **Birimi tekrar çevir**' i seçin. 

    ![Bir anlık görüntünün sağ tıklama menüsünü açıklayan ekran görüntüsü](../media/azure-netapp-files/snapshot-right-click-menu.png) 

2. Sesi anlık görüntüye çevir penceresinde birimin adını yazın ve **çevir**' e tıklayın.   

    Birim artık seçili anlık görüntünün zaman noktasına geri yüklendi.

    ![Birimin anlık görüntüye döndürüleceği pencere görüntüsü](../media/azure-netapp-files/snapshot-revert-volume.png) 

## <a name="delete-snapshots"></a>Anlık görüntüleri Sil  

Artık saklamak zorunda kalmayacak anlık görüntüleri silebilirsiniz. 

1. Bir birimin **anlık görüntüler** menüsüne gidin. Silmek istediğiniz anlık görüntüye sağ tıklayın. **Sil**’i seçin.

    ![Bir anlık görüntünün sağ tıklama menüsünü açıklayan ekran görüntüsü](../media/azure-netapp-files/snapshot-right-click-menu.png) 

2. Anlık görüntüyü Sil penceresinde **Evet**' e tıklayarak anlık görüntüyü silmek istediğinizi onaylayın. 

    ![Anlık görüntü silmeyi doğrulayan ekran görüntüsü](../media/azure-netapp-files/snapshot-confirm-delete.png)  

## <a name="next-steps"></a>Sonraki adımlar

* [Anlık görüntü ilkelerinde sorun giderme](troubleshoot-snapshot-policies.md)
* [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
* [Azure NetApp Files Snapshot 101 videosu](https://www.youtube.com/watch?v=uxbTXhtXCkw&feature=youtu.be)
* [Azure Uygulamayla Tutarlı Anlık Görüntü Aracı nedir?](azacsnap-introduction.md)
