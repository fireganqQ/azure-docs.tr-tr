---
title: Azure HDInsight 'ta Hive için güvenlik seçenekleri
description: Standart ve ESP kümelerinde Hive için güvenlik seçenekleri.
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/02/2020
ms.openlocfilehash: 1189a320d0dc700756c9f7664d0a6303be5dab51
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98940890"
---
# <a name="security-options-for-hive-in-azure-hdinsight"></a>Azure HDInsight 'ta Hive için güvenlik seçenekleri

Bu belge, HDInsight 'ta Hive için önerilen güvenlik seçeneklerini açıklamaktadır. Bu seçenekler, ambarı aracılığıyla yapılandırılabilir.

![' Hive için güvenlik seçenekleri '](./media/hdinsight-security-options-for-hive/security-options-hive.png "Hive için güvenlik seçenekleri")

## <a name="hiveserver2-authentication"></a>HiveServer2 kimlik doğrulaması

Standart kümeler için, HiveServer2 kimlik doğrulaması için önerilen ayar, None olan varsayılan ayardır. Kimlik doğrulamasını etkinleştirmek için bir [ESP](../domain-joined/hdinsight-security-overview.md) (kurumsal güvenlik paketi) kümesine yükseltmeniz önerilir. 

ESP kümelerinde, [Kerberos](https://web.mit.edu/Kerberos/) kimlik doğrulaması varsayılan olarak etkindir. Takılabilir kimlik doğrulama modülleri (PAM) ve özel kimlik doğrulama şemaları desteklenmez.

## <a name="hiveserver2-authorization"></a>HiveServer2 yetkilendirmesi

Standart kümeler için varsayılan ayar None ' dır. [Sqlstdaduth (SQL standartları tabanlı yetkilendirme)](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+based+hive+authorization) etkinleştirilebilir. [Apache Ranger](https://ranger.apache.org/) aracılığıyla yetkilendirme standart kümeler için desteklenmez. Ranger yetkilendirmesi için bir ESP kümesine yükseltmeniz önerilir. 

ESP kümelerinde, Ranger aracılığıyla yetkilendirme varsayılan olarak etkindir. 


## <a name="ssl-encryption-for-hiveserver2"></a>HiveServer2 için SSL şifrelemesi

Hiveserver2 SSL 'nin etkinleştirilmesi, standart veya ESP kümeleri için önerilmez. Bunun yerine SSL, ağ geçidinde etkindir. [Aktarım sırasında şifreleme](../domain-joined/encryption-in-transit.md) , [Internet Protokolü güvenliği (IPSec)](https://en.wikipedia.org/wiki/IPsec)kullanan küme düğümleri arasındaki iletişimleri şifrelemek için etkinleştirilebilir.


## <a name="next-steps"></a>Sonraki adımlar
* [HiveServer2 kimlik doğrulamasına genel bakış](https://cwiki.apache.org/confluence/display/Hive/Setting+up+HiveServer2#SettingUpHiveServer2-Authentication/SecurityConfiguration)
* [HiveServer2 yetkilendirmesi 'ne genel bakış](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Authorization)
* [SQL standartlarına dayalı Hive yetkilendirmesini etkinleştirme](https://community.cloudera.com/t5/Community-Articles/Getting-started-with-SQLStdAuth/ta-p/244263)
* [Hive ile Apache Ranger](../domain-joined/apache-domain-joined-run-hive.md)
