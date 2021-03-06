---
title: U-SQL veritabanını dışarı aktarma-Visual Studio için Azure Data Lake Araçları
description: Visual Studio için Azure Data Lake Araçları kullanarak U-SQL veritabanını dışarı aktarma ve yerel bir hesaba otomatik olarak içeri aktarma hakkında bilgi edinin.
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: how-to
ms.date: 11/27/2017
ms.openlocfilehash: 0a768990a33f0d60a3222bd3ad323edcc53c9ec0
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92219097"
---
# <a name="export-a-u-sql-database"></a>U-SQL veritabanını dışarı aktarma

Bu makalede, bir U-SQL veritabanını tek bir U-SQL betiği olarak ve indirilen kaynakları kullanarak dışarı aktarmak için [Visual Studio için Azure Data Lake araçları](https://aka.ms/adltoolsvs) nasıl kullanacağınızı öğrenin. Dışarı aktarılan veritabanını aynı işlemdeki bir yerel hesaba aktarabilirsiniz.

Müşteriler genellikle geliştirme, test ve üretim için birden çok ortamı korur. Bu ortamlar hem bir yerel hesapta, hem de bir geliştiricinin yerel bilgisayarında ve Azure 'daki bir Azure Data Lake Analytics hesabında barındırılır. 

Geliştirme ve test ortamlarında U-SQL sorguları geliştirirken ve ayarladığınızda, geliştiricilerin genellikle işlerini bir üretim veritabanında yeniden oluşturması gerekir. Veritabanı dışarı aktarma Sihirbazı bu işlemi hızlandırmaya yardımcı olur. Geliştiriciler, Sihirbazı kullanarak var olan veritabanı ortamını ve örnek verileri diğer Data Lake Analytics hesaplarına kopyalayabilir.

## <a name="export-steps"></a>Dışarı aktarma adımları

### <a name="step-1-export-the-database-in-server-explorer"></a>1. Adım: veritabanını Sunucu Gezgini dışarı aktarma

İzinlerine sahip olduğunuz tüm Data Lake Analytics Hesapları Sunucu Gezgini listelenmiştir. Veritabanını dışarı aktarmak için:

1. Sunucu Gezgini, dışarı aktarmak istediğiniz veritabanını içeren hesabı genişletin.
2. Veritabanına sağ tıklayın ve ardından **dışarı aktar**' ı seçin. 
   
    ![Sunucu Gezgini-veritabanını dışarı aktarma](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

     **Dışa aktarma** menü seçeneği kullanılabilir değilse, [Aracı, artırılmış sürüme güncelleştirmeniz](https://aka.ms/adltoolsvs)gerekir.

### <a name="step-2-configure-the-objects-that-you-want-to-export"></a>2. Adım: dışarı aktarmak istediğiniz nesneleri yapılandırma

Büyük bir veritabanının yalnızca küçük bir bölümüne ihtiyacınız varsa, dışa aktarma sihirbazında dışarı aktarmak istediğiniz nesnelerin bir alt kümesini yapılandırabilirsiniz. 

Dışarı aktarma eylemi bir U-SQL işi çalıştırılarak tamamlanır. Bu nedenle, bir Azure hesabından dışarı aktarmak biraz maliyet doğurur.

![Veritabanı dışarı aktarma Sihirbazı-nesneleri dışarı aktar ' ı seçin](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-other-configurations"></a>3. Adım: nesneler listesini ve diğer konfigürasyonları denetleme

Bu adımda, **nesne ver liste** kutusunda seçili nesneleri doğrulayabilirsiniz. Herhangi bir hata varsa, geri dönüp dışarı aktarmak istediğiniz nesneleri doğru şekilde yapılandırmak için **önceki** 'ni seçin.

Dışa aktarma hedefi için diğer ayarları da yapılandırabilirsiniz. Yapılandırma açıklamaları aşağıdaki tabloda listelenmiştir:

|Yapılandırma|Açıklama|
|-------------|-----------|
|Hedef adı|Bu ad, dışarıya aktarılmış veritabanı kaynaklarını nereye kaydetmek istediğinizi belirtir. Derlemeler, ek dosyalar ve örnek veriler örnektir. Yerel veri kök klasörünüz altında bu ada sahip bir klasör oluşturulur.|
|Proje dizini|Bu yol, aktarılmış U-SQL betiğini nereye kaydetmek istediğinizi tanımlar. Tüm veritabanı nesnesi tanımları bu konuma kaydedilir.|
|Yalnızca şema|Bu seçeneği belirlerseniz, yalnızca veritabanı tanımları ve kaynakları (derlemeler ve ek dosyalar gibi) verilir.|
|Şema ve Veri|Bu seçeneği belirlerseniz, veritabanı tanımları, kaynaklar ve veriler verilir. Tabloların ilk N satırı aktarılmalıdır.|
|Yerel veritabanına otomatik olarak aktar|Bu seçeneği belirlerseniz dışarı aktarma işlemi tamamlandığında dışarı aktarılan veritabanı yerel veritabanınıza otomatik olarak aktarılır.|

![Veritabanı dışarı aktarma Sihirbazı-nesne listesini ve diğer konfigürasyonları dışarı aktarma](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>4. Adım: dışarı aktarma sonuçlarını denetleme

Dışarı aktarma işlemi tamamlandığında, dışarı aktarılan sonuçları sihirbazdaki günlük penceresinde görüntüleyebilirsiniz. Aşağıdaki örnekte derlemeler, ek dosyalar ve örnek veriler de dahil olmak üzere, aktarılmış U-SQL betiği ve veritabanı kaynaklarının nasıl bulunacağı gösterilmektedir:

![Veritabanı dışarı aktarma Sihirbazı-sonuçları dışarı aktarma](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="import-the-exported-database-to-a-local-account"></a>Dışarı aktarılan veritabanını bir yerel hesaba aktar

Dışarı aktarılan veritabanını içeri aktarmanın en kolay yolu, adım 3 ' teki dışarı aktarma işlemi sırasında **yerel veritabanına otomatik olarak aktar** onay kutusunu seçmelidir. Bu kutuyu görmüyorsanız, ilk olarak dışa aktarma günlüğünde dışarı aktarılmış U-SQL betiğini bulun. Ardından, yerel hesabınıza veritabanını içeri aktarmak için U-SQL betiğini yerel olarak çalıştırın.

## <a name="import-the-exported-database-to-a-data-lake-analytics-account"></a>Dışarı aktarılan veritabanını bir Data Lake Analytics hesabına aktarın

Veritabanını farklı bir Data Lake Analytics hesabına aktarmak için:

1. Derlemeler, ek dosyalar ve örnek veriler de dahil olmak üzere dışarı aktarılan kaynakları, içeri aktarmak istediğiniz Data Lake Analytics hesabının varsayılan Azure Data Lake Store hesabına yükleyin. Aktarılmış kaynak klasörünü yerel veri kök klasörü altında bulabilirsiniz. Tüm klasörü varsayılan Data Lake Store hesabının köküne yükleyin.
2. Karşıya yükleme tamamlandığında, dışarı aktarılan U-SQL betiğini veritabanını içeri aktarmak istediğiniz Data Lake Analytics hesabına gönderebilirsiniz.

## <a name="known-limitations"></a>Bilinen sınırlamalar

Şu anda adım 3 ' te **şema ve veri** seçeneğini belirlerseniz, araç, tablolarda depolanan verileri dışarı aktarmak Için bir U-SQL işi çalıştırır. Bu nedenle, verileri dışarı aktarma işlemi yavaş olabilir ve maliyetlerden karşılaşabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [U-SQL veritabanları hakkında bilgi edinin](/u-sql/data-definition-language-ddl-statements) 
* [Yerel çalıştırma ve Azure Data Lake U-SQL SDK’sını kullanarak U-SQL işlerini test etme ve hatalarını ayıklama](data-lake-analytics-data-lake-tools-local-run.md)


