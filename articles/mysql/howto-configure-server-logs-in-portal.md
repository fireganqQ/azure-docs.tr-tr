---
title: Yavaş sorgu günlüklerine erişme-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portal MySQL için Azure veritabanı 'nda yavaş günlüklerin nasıl yapılandırılacağı ve erişebileceği açıklanır.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 4/13/2020
ms.openlocfilehash: 5ad4ffa99a7af592e3e93e53673d254956807c40
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94541632"
---
# <a name="configure-and-access-slow-query-logs-from-the-azure-portal"></a>Azure portal yavaş sorgu günlüklerini yapılandırma ve erişme

[MySQL Için Azure veritabanı yavaş sorgu günlüklerini](concepts-server-logs.md) Azure Portal yapılandırabilir, listeleyebilir ve indirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki adımlarda, [MySQL Için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-portal.md)gerekir.

## <a name="configure-logging"></a>Günlüğe kaydetmeyi yapılandırma
MySQL yavaş sorgu günlüğüne erişimi yapılandırın. 

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. MySQL için Azure veritabanı sunucunuzu seçin.

3. Kenar çubuğu 'ndaki **izleme** bölümünde **sunucu günlükleri** ' ni seçin. 
   :::image type="content" source="./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png" alt-text="Sunucu günlüğü seçeneklerinin ekran görüntüsü":::

4. Sunucu parametrelerini görmek için, **günlükleri etkinleştirmek ve günlük parametrelerini yapılandırmak için buraya tıklayın ' ı** seçin.

5. **Slow_query_log** **açın.**

6. **Log_output** kullanarak günlüklerin nereye çıkışını seçin. Günlükleri hem yerel depolama hem de Azure Izleyici tanılama günlüklerine göndermek için **Dosya** ' yı seçin. 

7. Gerekli diğer parametreleri değiştirin. 

8. **Kaydet** ’i seçin. 

   :::image type="content" source="./media/howto-configure-server-logs-in-portal/3-save-discard.png" alt-text="Yavaş sorgu günlüğü parametrelerinin ekran görüntüsü ve kaydetme.":::

**Sunucu parametreleri** sayfasında, sayfayı kapatarak Günlükler listesine geri dönebilirsiniz.

## <a name="view-list-and-download-logs"></a>Listeyi görüntüle ve günlükleri indir
Günlüğe kaydetme başladıktan sonra, kullanılabilir yavaş sorgu günlüklerinin bir listesini görüntüleyebilir ve günlük dosyalarını tek tek indirebilirsiniz.

1. Azure portalını açın.

2. MySQL için Azure veritabanı sunucunuzu seçin.

3. Kenar çubuğu 'ndaki **izleme** bölümünde **sunucu günlükleri** ' ni seçin. Sayfa, günlük dosyalarınızın bir listesini gösterir.

   :::image type="content" source="./media/howto-configure-server-logs-in-portal/4-server-logs-list.png" alt-text="Günlük listesi vurgulanmış şekilde sunucu günlükleri sayfasının ekran görüntüsü":::

   > [!TIP]
   > Günlüğün adlandırma kuralı **MySQL-yavaş-sunucu adınızın>-yyyymmddhh. log <**. Dosya adında kullanılan tarih ve saat, günlüğün verildiği zaman. Günlük dosyaları her 24 saatte bir veya 7,5 GB döndürülür, hangisi önce gelir. 

4. Gerekirse, tarih ve saate göre belirli bir günlüğe hızlıca daraltmak için arama kutusunu kullanın. Arama günlüğün adı üzerinde.

5. Ayrı günlük dosyalarını indirmek için tablo satırındaki her bir günlük dosyasının yanındaki aşağı ok simgesini seçin.

   :::image type="content" source="./media/howto-configure-server-logs-in-portal/5-download.png" alt-text="Aşağı ok simgesi vurgulanmış şekilde sunucu günlükleri sayfasının ekran görüntüsü":::

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

1. Kenar çubuğu 'ndaki **izleme** bölümünde **Tanılama ayarları**  >  **Tanılama Ayarları Ekle** ' yi seçin.

   :::image type="content" source="./media/howto-configure-server-logs-in-portal/add-diagnostic-setting.png" alt-text="Tanılama ayarları seçeneklerinin ekran görüntüsü":::

1. Bir tanılama ayarı adı belirtin.

1. Yavaş sorgu günlüklerinin (depolama hesabı, Olay Hub 'ı veya Log Analytics çalışma alanı) hangi veri havuzlarını gönderileceğini belirtin.

1. Günlük türü olarak **Mysqlyavaşlogs** ' u seçin.
:::image type="content" source="./media/howto-configure-server-logs-in-portal/configure-diagnostic-setting.png" alt-text="Tanılama ayarları yapılandırma seçeneklerinin ekran görüntüsü":::

1. Yavaş sorgu günlüklerini kanala yönelten veri havuzlarını yapılandırdıktan sonra **Kaydet** ' i seçin.
:::image type="content" source="./media/howto-configure-server-logs-in-portal/save-diagnostic-setting.png" alt-text="Tanılama ayarları yapılandırma seçeneklerinin, kayıt vurgulanmış olarak ekran görüntüsü":::

1. Bunları yapılandırdığınız veri havuzları içinde inceleyerek yavaş sorgu günlüklerine erişin. Günlüklerin görünmesi 10 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
- Yavaş sorgu günlüklerinin programlama yoluyla nasıl indirileceği hakkında bilgi edinmek için bkz. [CLI 'deki yavaş sorgu günlüklerine erişme](howto-configure-server-logs-in-cli.md) .
- MySQL için Azure veritabanı 'nda [yavaş sorgu günlükleri](concepts-server-logs.md) hakkında daha fazla bilgi edinin.
- Parametre tanımları ve MySQL günlüğü hakkında daha fazla bilgi için, [günlüklerdeki](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)MySQL belgelerine bakın.