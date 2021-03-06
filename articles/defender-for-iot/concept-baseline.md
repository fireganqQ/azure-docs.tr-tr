---
title: Taban çizgisi ve özel denetimler
description: IoT temeli için Azure Defender kavramı hakkında bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2019
ms.author: mlottner
ms.openlocfilehash: 04fe87cd69efc4c064b8fbdc596a5f9e187abbb1
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100102249"
---
# <a name="azure-defender-for-iot-baseline-and-custom-checks"></a>IoT için Azure Defender temel ve özel denetimler

Bu makalede IoT taban çizgisi için Defender açıklanmakta ve temel özel denetimlerin tüm ilişkili özellikleri özetlenmektedir.

## <a name="baseline"></a>Taban çizgisi

Taban çizgisi her cihaz için standart davranış oluşturur ve beklenen Norms 'den olağan dışı davranış veya sapma oluşturmayı kolaylaştırır.

## <a name="baseline-custom-checks"></a>Taban çizgisi özel denetimleri

Taban çizgisi özel denetimleri, cihazın **modül kimliği ikizi** kullanarak her bir cihaz temeli için özel bir denetim listesi kurar.

## <a name="setting-baseline-properties"></a>Taban çizgisi özelliklerini ayarlama

1. IoT Hub değiştirmek istediğiniz cihazı bulun ve seçin.

1. Cihaza tıklayın ve sonra **azureiotsecurity** modülüne tıklayın.

1. **Modül kimliği ikizi**' na tıklayın.

1. Ana **hat özel denetim** dosyasını cihaza yükleyin.

1. Güvenlik modülüne temel özellikler ekleyin ve **Kaydet**' e tıklayın.

### <a name="baseline-custom-check-file-example"></a>Temel özel denetim dosyası örneği

Temel özel denetimleri yapılandırmak için:

   ```json
    "desired": {
      "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration": {
        "baselineCustomChecksEnabled": {
          "value" : true
        },
        "baselineCustomChecksFilePath": {
          "value" : "/home/user/full_path.xml"
        },
        "baselineCustomChecksFileHash": {
          "value" : "#hashexample!"
        }
      }
    },
   ```

## <a name="baseline-custom-check-properties"></a>Taban çizgisi özel denetim özellikleri

| Name| Durum | Geçerli değerler| Varsayılan değerler| Description |
|------|-----|------|-----|-----|
|baselineCustomChecksEnabled|Gerekli: true |Geçerli değerler: **Boolean** |Varsayılan değer: **false** |Yüksek öncelikli iletiler gönderilmeden önce en uzun zaman aralığı.|
|baselineCustomChecksFilePath |Gerekli: true|Geçerli değerler: **String**, **null** |Varsayılan değer: **null** |Taban çizgisi XML yapılandırmasının tam yolu|
|baselineCustomChecksFileHash |Gerekli: true|Geçerli değerler: **String**, **null** |Varsayılan değer: **null** |`sha256sum` XML yapılandırma dosyası. Ek bilgi için [SHA256sum başvurusunu](https://linux.die.net/man/1/sha256sum) kullanın. |

Ek temel örnekleri gözden geçirmek için bkz. [özel taban çizgisi örneği-1](https://ascforiot.blob.core.windows.net/public/custom_baseline_example_hyperv_ubuntu1804.xml) ve [özel taban çizgisi örneği-2](https://ascforiot.blob.core.windows.net/public/oms_audits.xml).

## <a name="next-steps"></a>Sonraki adımlar

- [Ham güvenlik verilerinize](how-to-security-data-access.md) erişin
- [Cihazı araştırma](how-to-investigate-device.md)
- [Güvenlik önerilerini](concept-recommendations.md) anlayın ve araştırın
- [Güvenlik uyarılarını](concept-security-alerts.md) anlama ve araştırma
