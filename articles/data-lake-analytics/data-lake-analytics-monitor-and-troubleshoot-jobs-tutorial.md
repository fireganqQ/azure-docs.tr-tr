---
title: İzleme Azure Data Lake Analytics Azure portal
description: Bu makalede Azure Data Lake Analytics işlerinin sorunlarını gidermek için Azure portal nasıl kullanılacağı açıklanır.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: troubleshooting
ms.date: 12/05/2016
ms.openlocfilehash: ddcf41a333d261bd9c5f669fde724a25eeba670e
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92220287"
---
# <a name="monitor-jobs-in-azure-data-lake-analytics-using-the-azure-portal"></a>Azure Portalını kullanarak Azure Data Lake Analytics işlerini izleme

## <a name="to-see-all-the-jobs"></a>Tüm işleri görmek için

1. Azure portal sol üst köşedeki **Microsoft Azure** ' e tıklayın.

2. Data Lake Analytics hesap adınızı içeren kutucuğa tıklayın.  İş özeti **Iş yönetimi** kutucuğunda gösterilir.

   ![Azure Data Lake Analytics iş yönetimi](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    İş yönetimi size iş durumu için bir bakış sağlar. Başarısız bir iş olduğunu unutmayın.
3. İşleri görmek için **Iş yönetimi** kutucuğuna tıklayın. İşler **çalışıyor**, **kuyruğa alındı**ve **tamamlandı**olarak kategorilere ayrılır. Başarısız işi **bitti** bölümünde görürsünüz. Listenin ilk adı olacaktır. Çok sayıda iş olduğunda, işleri bulmanıza yardımcı olması için **filtre** ' yi tıklatabilirsiniz.

   ![Azure Data Lake Analytics Filtre işleri](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)

4. İş ayrıntılarını açmak için listeden başarısız işi tıklatın:

   ![Başarısız iş Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Yeniden **Gönder** düğmesine dikkat edin. Sorunu düzelttikten sonra işi yeniden gönderebilirsiniz.

5. Hata ayrıntılarını açmak için önceki ekran görüntüsünde vurgulanan Bölüm ' e tıklayın.  Şöyle bir şey göreceksiniz:

   ![Başarısız iş Azure Data Lake Analytics ayrıntıları](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

   Kaynak klasörün bulunamadığını söyler.

6. **Yinelenen betik**' e tıklayın.

7. **Kimden** yolundan şu şekilde güncelleştir:`/Samples/Data/SearchLog.tsv`

8. **İşi Gönder**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics genel bakış](data-lake-analytics-overview.md)
* [Azure PowerShell'i kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Azure portal kullanarak Azure Data Lake Analytics yönetme](data-lake-analytics-manage-use-portal.md)
