---
title: Yedekleme merkezi 'ni kullanarak Öngörüler edinin
description: Geçmiş eğilimleri çözümlemeyi ve yedekleme merkezi ile yedeklemeleriniz hakkında daha derin Öngörüler elde etmeyi öğrenin.
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: 3086224e4dbb1be2ccfadbfcfcb7441b68a5ff6b
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100592009"
---
# <a name="obtain-insights-using-backup-center-preview"></a>Yedekleme merkezi (Önizleme) kullanarak Öngörüler edinin

Yedekleme merkezi, geçmiş eğilimleri analiz etmek ve yedeklemelerinize daha derin Öngörüler elde etmek için [Azure Izleyici günlüklerini](../azure-monitor/logs/data-platform-logs.md) ve [Azure çalışma kitaplarını](../azure-monitor/visualize/workbooks-overview.md)kullanan [yedekleme raporları (Önizleme)](configure-reports.md)için bir arabirim sağlar. Yedekleme raporları aşağıdaki özellikleri sunar:

- Tüketilen bulut depolamasını ayırma ve tahmin etme.

- Yedeklemeler ve geri yüklemeler denetimi.

- Farklı ayrıntı düzeyi düzeylerinde önemli eğilimleri tanımlama.

- Yedeklemeleriniz için maliyet iyileştirme fırsatlarıyla ilgili görünürlük ve Öngörüler elde edin.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

- Yedekleme raporları (Önizleme) Şu anda PostgreSQL için Azure veritabanı sunucu yedeklemesi için kullanılamıyor.

- Desteklenen ve desteklenmeyen senaryoların ayrıntılı bir listesi için [destek matrisine](backup-center-support-matrix.md) bakın.

## <a name="get-started"></a>başlarken

### <a name="configure-your-vaults-to-send-data-to-a-log-analytics-workspace"></a>Log Analytics çalışma alanına veri göndermek için vakalarınızı yapılandırma

[Kasalarınız için ölçekte tanılama ayarlarını yapılandırma hakkında bilgi edinin](./configure-reports.md#get-started)

### <a name="view-backup-reports-in-the-backup-center-portal"></a>Yedekleme merkezi portalında yedekleme raporlarını görüntüleme

Yedekleme merkezi 'nde **yedekleme raporları** menü öğesini seçme raporları açılır. Yedeklemelerinizin önemli bilgilerini görüntülemek ve analiz etmek için bir veya daha fazla Log Analytics çalışma alanı seçin.

![Yedekleme merkezi 'nde yedekleme raporları](./media/backup-center-obtain-insights/backup-center-backup-reports.png)

Kullanılabilen görünümler şunlardır:

1. **Özet** -bu sekmeyi, yedeklemeinize ilişkin üst düzey bir genel bakış almak için kullanın. [Daha fazla bilgi edinin](./configure-reports.md#summary)

1. **Yedekleme öğeleri** -bu sekmeyi, bir yedekleme öğesi düzeyinde tüketilen Bulut depolamada bilgi ve eğilimleri görmek için kullanın. [Daha fazla bilgi edinin](./configure-reports.md#backup-items)

1. **Kullanım** -yedeklemelerinizin anahtar faturalandırma parametrelerini görüntülemek için bu sekmeyi kullanın. [Daha fazla bilgi edinin](./configure-reports.md#usage)

1. **İşler** -bu sekmeyi, işlerinizde gerçekleşen başarısız iş sayısı ve iş hatasının en üst nedenleri gibi, işlere ilişkin uzun süre çalışan eğilimleri görüntülemek için kullanın. [Daha fazla bilgi edinin](./configure-reports.md#jobs)

1. **İlkeler** -bu sekmeyi, ilişkili öğe sayısı ve belirli bir ilke altında yedeklenen öğeler tarafından tüketilen toplam bulut depolama alanı gibi tüm etkin ilkeleriniz hakkındaki bilgileri görüntülemek için kullanın. [Daha fazla bilgi edinin](./configure-reports.md#policies)

1. **En Iyi duruma** getirme-yedeklemelerinize yönelik potansiyel maliyet iyileştirme fırsatlarıyla ilgili görünürlük elde etmek için bu sekmeyi kullanın. [Daha fazla bilgi edinin](./configure-reports.md#optimize)

## <a name="next-steps"></a>Sonraki adımlar

- [Yedeklemeleri izleme ve çalıştırma](backup-center-monitor-operate.md)
- [Yedeklemenizi yönetin](backup-center-govern-environment.md)
- [Yedekleme merkezi 'ni kullanarak eylem gerçekleştirme](backup-center-actions.md)