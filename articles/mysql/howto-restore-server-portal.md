---
title: Yedekleme ve geri yükleme-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı 'nda bir sunucunun nasıl geri yükleneceği açıklanmaktadır.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 6/30/2020
ms.openlocfilehash: 9bc31cf8fee2669634ff366caac77cb090baf075
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96000309"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nda sunucu yedekleme ve geri yükleme

## <a name="backup-happens-automatically"></a>Yedekleme otomatik olarak gerçekleşir
MySQL için Azure veritabanı sunucuları, geri yükleme özelliklerini etkinleştirmek üzere düzenli aralıklarla yedeklenir. Bu özelliği kullanarak, sunucuyu ve tüm veritabanlarını yeni bir sunucuda daha önceki bir zaman noktasına geri yükleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MySQL Için Azure veritabanı sunucusu ve veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-backup-configuration"></a>Yedekleme yapılandırmasını ayarla

Sunucu oluşturma sırasında, **fiyatlandırma katmanı** penceresinde yerel olarak yedekli yedeklemeler veya coğrafi olarak yedekli yedeklemeler için sunucunuzu yapılandırma arasında seçim yaparsınız.

> [!NOTE]
> Bir sunucu oluşturulduktan sonra, coğrafi olarak yedekli ve yerel olarak yedekli olan artıklık türü değiştirilemez.
>

Azure portal aracılığıyla bir sunucu oluştururken, **fiyatlandırma katmanı** penceresi sunucunuz Için **yerel olarak yedekli** veya **coğrafi olarak yedekli** yedeklemeleri seçtiğiniz yerdir. Bu pencere Ayrıca, **yedekleme bekletme dönemini** (gün sayısı cinsinden) sunucu yedeklemelerinin ne kadar süreyle depolandığını seçtiğiniz yerdir.

   :::image type="content" source="./media/howto-restore-server-portal/pricing-tier.png" alt-text="Fiyatlandırma Katmanı-yedek yedeklilik seçin":::

Oluşturma sırasında bu değerleri ayarlama hakkında daha fazla bilgi için bkz. [MySQL Için Azure veritabanı sunucusu hızlı başlangıç](quickstart-create-mysql-server-database-using-azure-portal.md).

Yedekleme saklama süresi bir sunucuda aşağıdaki adımlarla değiştirilebilir:
1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. MySQL için Azure veritabanı sunucunuzu seçin. Bu eylem **genel bakış** sayfasını açar.
3. Menüde, **Ayarlar**' ın altında **fiyatlandırma katmanı** ' nı seçin. Kaydırıcıyı kullanarak **yedekleme saklama süresini** 7 ila 35 gün süreyle tercihlerinize göre değiştirebilirsiniz.
Aşağıdaki ekran görüntüsünde 34 güne yükselmiştir.
:::image type="content" source="./media/howto-restore-server-portal/3-increase-backup-days.png" alt-text="Yedekleme bekletme süresi arttı":::

4. Değişikliği onaylamak için **Tamam** ' ı tıklatın.

Yedekleme bekletme süresi, kullanılabilir yedeklemeler temel aldığı için zaman içinde bir nokta geri yüklemesi alma süresinin ne kadar geri alınacağını yönetir. Bir noktadan sonra geri yükleme aşağıdaki bölümde daha ayrıntılı olarak açıklanmıştır. 

## <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme
MySQL için Azure veritabanı, sunucuyu bir zaman noktasına geri yüklemenize ve sunucunun yeni bir kopyasına geri yüklemenize olanak tanır. Verilerinizi kurtarmak için bu yeni sunucuyu kullanabilir veya istemci uygulamalarınızın bu yeni sunucuyu işaret edebilir.

Örneğin, bir tablo bugün öğleden sonra yanlışlıkla bırakıldıysa, daha önce geçen saate geri yükleyebilir ve eksik tablo ve verileri sunucunun bu yeni kopyasından elde edebilirsiniz. Zaman içinde geri yükleme, veritabanı düzeyinde değil, sunucu düzeyindedir.

Aşağıdaki adımlar örnek sunucuyu bir zaman noktasına geri yükler:
1. Azure portal, MySQL için Azure veritabanı sunucunuzu seçin. 

2. Sunucunun **genel bakış** sayfasının araç çubuğunda **geri yükle**' yi seçin.

   :::image type="content" source="./media/howto-restore-server-portal/2-server.png" alt-text="MySQL için Azure veritabanı-genel bakış-geri yükleme düğmesi":::

3. Geri yükleme formunu gereken bilgilerle doldurun:

   :::image type="content" source="./media/howto-restore-server-portal/3-restore.png" alt-text="MySQL için Azure veritabanı-geri yükleme bilgileri":::
   - **Geri yükleme noktası**: geri yüklemek istediğiniz zaman noktasını seçin.
   - **Hedef sunucu**: yeni sunucu için bir ad sağlayın.
   - **Konum**: bölgeyi seçemezsiniz. Varsayılan olarak, kaynak sunucu ile aynıdır.
   - **Fiyatlandırma katmanı**: bir zaman içinde geri yükleme yaparken bu parametreleri değiştiremezsiniz. Kaynak sunucuyla aynıdır. 

4. Sunucuyu bir zaman noktasına geri yüklemek üzere geri yüklemek için **Tamam** ' a tıklayın. 

5. Geri yükleme tamamlandıktan sonra, verilerin beklendiği gibi geri yüklendiğini doğrulamak için oluşturulan yeni sunucuyu bulun.

Noktadan noktaya geri yükleme tarafından oluşturulan yeni sunucu, mevcut sunucu için geçerli olan Sunucu Yöneticisi oturum açma adı ve parolası ile aynı zamanda, Parolayı yeni sunucunun **genel bakış** sayfasından değiştirebilirsiniz.

Ayrıca, geri yükleme işlemi tamamlandıktan sonra, geri yükleme işleminden sonra varsayılan değerlere sıfırlanan (ve birincil sunucudan kopyalanmayan) iki sunucu parametresi vardır
*   time_zone-bu değer varsayılan değer **sistemine** ayarlanır
*   event_scheduler-event_scheduler, geri yüklenen sunucuda **kapalı** olarak ayarlanmıştır

Birincil sunucudan değeri kopyalamanız ve [sunucu parametresini](howto-server-parameters.md) yeniden yapılandırarak geri yüklenen sunucuda ayarlamanız gerekir

Geri yükleme sırasında oluşturulan yeni sunucu, özgün sunucuda var olan VNet hizmeti uç noktalarına sahip değildir. Bu kuralların bu yeni sunucu için ayrıca ayarlanması gerekir. Özgün sunucudan gelen güvenlik duvarı kuralları geri yüklendi.

## <a name="geo-restore"></a>Coğrafi geri yükleme
Sunucunuzu coğrafi olarak yedekli yedeklemeler için yapılandırdıysanız, var olan sunucunun yedeklemesinden yeni bir sunucu oluşturulabilir. Bu yeni sunucu, MySQL için Azure veritabanı 'nın kullanılabildiği herhangi bir bölgede oluşturulabilir.  

1. Portalın sol üst köşesinde bulunan **kaynak oluştur** düğmesini (+) seçin. **Veritabanları**  >  **MySQL için Azure veritabanı**' nı seçin.

   :::image type="content" source="./media/howto-restore-server-portal/1_navigate-to-mysql.png" alt-text="MySQL için Azure veritabanı 'na gidin.":::
 
2. Yeni sunucunun aboneliğini, kaynak grubunu ve adını sağlayın. 

3. **Veri kaynağı** olarak **yedekleme** ' yi seçin. Bu eylem, coğrafi olarak yedekli yedeklemelerin etkinleştirildiği sunucuların listesini sağlayan bir açılan menü yükler.
   
   :::image type="content" source="./media/howto-restore-server-portal/3-geo-restore.png" alt-text="Veri kaynağını seçin.":::
    
   > [!NOTE]
   > Sunucu ilk oluşturulduğunda coğrafi geri yükleme için hemen kullanılamayabilir. Gerekli meta verilerin doldurulması birkaç saat sürebilir.
   >

4. **Yedekleme** açılan listesini seçin.
   
   :::image type="content" source="./media/howto-restore-server-portal/4-geo-restore-backup.png" alt-text="Yedekleme açılan listesini seçin.":::

5. Geri yüklenecek kaynak sunucuyu seçin.
   
   :::image type="content" source="./media/howto-restore-server-portal/5-select-backup.png" alt-text="Yedekle 'yi seçin.":::

6. Sunucu, **sanal çekirdek** sayısı, **yedekleme saklama süresi**, **yedekleme artıklığı seçeneği**, **altyapı sürümü** ve **yönetici kimlik bilgileri** için varsayılan değerleri sağlar. **Devam**’ı seçin. 
   
   :::image type="content" source="./media/howto-restore-server-portal/6-accept-backup.png" alt-text="Yedeklemeye devam edin.":::

7. Formun geri kalanını tercihlerinize göre doldurun. Herhangi bir **konum** seçebilirsiniz.

    Konumu seçtikten sonra, **Işlem üretimini** güncelleştirmek Için **Sunucu Yapılandır** ' ı seçebilirsiniz (seçtiğiniz bölgede varsa), **sanal çekirdek** sayısı, **yedekleme saklama süresi** ve **yedek artıklık seçeneği**. Geri yükleme sırasında **fiyatlandırma katmanını** (temel, genel amaçlı veya bellek için iyileştirilmiş) veya **depolama** boyutunu değiştirme desteklenmiyor.

   :::image type="content" source="./media/howto-restore-server-portal/7-create.png" alt-text="Form Doldur."::: 

8. Seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** ' u seçin. 

9. Sunucuyu sağlamak için **Oluştur**’u seçin. Bu işlem birkaç dakika sürebilir.

Coğrafi geri yükleme tarafından oluşturulan yeni sunucu, geri yükleme başlatıldığı sırada mevcut sunucu için geçerli olan Sunucu Yöneticisi oturum açma adı ve parolaya sahiptir. Parola, yeni sunucunun **genel bakış** sayfasından değiştirilebilir.

Geri yükleme sırasında oluşturulan yeni sunucu, özgün sunucuda var olan VNet hizmeti uç noktalarına sahip değildir. Bu kuralların bu yeni sunucu için ayrıca ayarlanması gerekir. Özgün sunucudan gelen güvenlik duvarı kuralları geri yüklendi.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetin [yedeklemeleri](concepts-backup.md) hakkında daha fazla bilgi edinin
- [Çoğaltmalar](concepts-read-replicas.md) hakkında bilgi edinin
- [İş sürekliliği](concepts-business-continuity.md) seçenekleri hakkında daha fazla bilgi edinin