---
title: Azure Service Fabric CLı-sfctl sa-küme
description: Azure Service Fabric komut satırı arabirimi olan sfctl hakkında bilgi edinin. Tek başına kümelerin yönetilmesi için komutların bir listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 7e9f4b55945afc04e5c826b26632d68cb75e502f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86260270"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Tek başına Service Fabric kümelerini yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| config | Tek başına Service Fabric küme yapılandırmasını alın. |
| yapılandırma-yükseltme | Tek başına Service Fabric küme yapılandırmasını yükseltmeye başlayın. |
| yükseltme-durum | Service Fabric tek başına kümesinin küme yapılandırması yükseltme durumunu alın. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-küme yapılandırması
Tek başına Service Fabric küme yapılandırmasını alın.

Küme yapılandırması küme üzerinde farklı düğüm türlerini, güvenlik yapılandırmalarını, hata ve yükseltme etki alanı topolojilerini vb. dahil olmak üzere kümenin özelliklerini içerir.

### <a name="arguments"></a>Bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Configuration-api-Version [gerekli] | Tek başına küme JSON yapılandırmasının API sürümü. |
| --timeout-t | İşlemi saniye cinsinden gerçekleştirmek için sunucu zaman aşımı. Bu zaman aşımı, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametre için varsayılan değer 60 saniyedir.  Varsayılan \: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa-küme yapılandırması-yükseltme
Tek başına Service Fabric küme yapılandırmasını yükseltmeye başlayın.

Sağlanan yapılandırma yükseltme parametrelerini doğrulayın ve parametreler geçerliyse küme yapılandırmasını yükseltmeye başlayın.

### <a name="arguments"></a>Bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Cluster-config [gerekli] | Küme yapılandırması. |
| --uygulama-sistem durumu-ilkeler | Hata dağıtılmadan önce uygulama türü adı çiftlerinin ve en yüksek yüzdelerin JSON kodlu sözlüğü. |
| --Delta-sağlıksız-düğümler | Yükseltme sırasında izin verilen Delta sistem durumu azalmanın en yüksek yüzdesi. İzin verilen değerler 0 ile 100 arasında bir tamsayı değerlerdir. |
| --sistem durumu-denetimi-yeniden dene | Uygulama veya küme sağlıklı değilse sistem durumu denetimleri gerçekleştirme denemeleri arasındaki süre uzunluğu.  Varsayılan \: PT0H0M0S. |
| --Sistem Durumu-Çek-kararlı | Yükseltme bir sonraki yükseltme etki alanına geçmeden önce uygulamanın veya kümenin sağlıklı kalması gereken süre.  Varsayılan \: PT0H0M0S. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Başarısız olursa, Toplam milisaniye sayısını temsil eden bir sayı olarak yorumlanır. |
| --Sistem Durumu-Çek-bekle | Durum denetimleri işlemini başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra beklenecek sürenin uzunluğu.  Varsayılan \: PT0H0M0S. |
| --timeout-t | Varsayılan \: 60. |
| --sağlıksız-uygulamalar | Yükseltme sırasında, sağlıksız uygulamaların izin verilen en büyük yüzdesi. İzin verilen değerler 0 ile 100 arasında bir tamsayı değerlerdir. |
| --sağlıksız-düğümler | Yükseltme sırasında, sağlıksız düğümlerin izin verilen en büyük yüzdesi. İzin verilen değerler 0 ile 100 arasında bir tamsayı değerlerdir. |
| --Upgrade-Domain-Delta-sağlıksız-Nodes | Yükseltme sırasında etki alanı Delta durumu düşüşünün izin verilen maksimum yüzdesi. İzin verilen değerler 0 ile 100 arasında bir tamsayı değerlerdir. |
| --Upgrade-etki alanı zaman aşımı | Her yükseltme etki alanının, FailureAction yürütülmeden önce tamamlaması gereken süre miktarı.  Varsayılan \: PT0H0M0S. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Başarısız olursa, Toplam milisaniye sayısını temsil eden bir sayı olarak yorumlanır. |
| --Upgrade-Timeout | FailureAction yürütülmeden önce genel yükseltmenin tamamlaması gereken süre miktarı.  Varsayılan \: PT0H0M0S. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Başarısız olursa, Toplam milisaniye sayısını temsil eden bir sayı olarak yorumlanır. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |

### <a name="examples"></a>Örnekler

Bir küme yapılandırma güncelleştirmesi Başlat
```
sfctl sa-cluster config-upgrade --cluster-config <YOUR CLUSTER CONFIG> --application-health-
policies "{"fabric:/System":{"ConsiderWarningAsError":true}}"
```

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-küme yükseltme-durum
Service Fabric tek başına kümesinin küme yapılandırması yükseltme durumunu alın.

Service Fabric tek başına kümesinin küme yapılandırması yükseltme durumu ayrıntılarını alın.

### <a name="arguments"></a>Bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --timeout-t | İşlemi saniye cinsinden gerçekleştirmek için sunucu zaman aşımı. Bu zaman aşımı, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametre için varsayılan değer 60 saniyedir.  Varsayılan \: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Hata Ayıkla | Tüm hata ayıklama günlüklerini göstermek için günlük ayrıntı düzeyini artırın. |
| --Yardım-h | Bu yardım iletisini gösterin ve çıkın. |
| --çıkış-o | Çıkış biçimi.  İzin verilen \: JSON, jsonc, tablo, TSV değerleri.  Varsayılan \: JSON. |
| --sorgu | JMESPath sorgu dizesi. \:Daha fazla bilgi ve örnek için bkz. http//jmespath.org/. |
| --ayrıntılı | Günlük ayrıntı düzeyini artırın. Tam hata ayıklama günlükleri için--Debug kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric CLı 'yi [ayarlayın](service-fabric-cli.md) .
- [Örnek betikleri](./scripts/sfctl-upgrade-application.md)kullanarak Service Fabric CLI 'nın nasıl kullanılacağını öğrenin.
