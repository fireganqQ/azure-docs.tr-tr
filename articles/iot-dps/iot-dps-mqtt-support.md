---
title: Azure IoT cihaz sağlama hizmeti MQTT desteğini anlama | Microsoft Docs
description: Geliştirici Kılavuzu-MQTT protokolünü kullanarak Azure IoT cihaz sağlama hizmeti (DPS) cihazına yönelik bir uç noktaya bağlanan cihazlar için destek.
author: rajeevmv
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: ravokkar
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 0a7ec2f4f8fdf631a6bc5096296275291ec41751
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94967134"
---
# <a name="communicate-with-your-dps-using-the-mqtt-protocol"></a>MQTT protokolünü kullanarak DPS ile iletişim kurma

DPS, cihazların şu kullanarak DPS cihaz uç noktasıyla iletişim kurmasını sağlar:

* 8883 numaralı bağlantı noktasında [MQTT v 3.1.1](https://mqtt.org/)
* 443 numaralı bağlantı noktasında WebSocket üzerinden [MQTT v 3.1.1](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html#_Toc398718127) .

DPS tam özellikli bir MQTT Aracısı değildir ve MQTT v 3.1.1 Standard 'da belirtilen tüm davranışları desteklemez. Bu makalede, cihazların DPS ile iletişim kurmak için desteklenen MQTT davranışlarını nasıl kullanabileceği açıklanır.

DPS ile tüm cihaz iletişimi, TLS/SSL kullanılarak güvenli hale getirilmelidir. Bu nedenle, DPS bağlantı noktası 1883 üzerinden güvenli olmayan bağlantıları desteklemez.

 > [!NOTE] 
 > DPS, MQTT protokolü üzerinden TPM [kanıtlama mekanizmasını](./concepts-service.md#attestation-mechanism) kullanan cihazları Şu anda desteklememektedir.

## <a name="connecting-to-dps"></a>DPS 'e bağlanma

Bir cihaz, aşağıdaki seçeneklerden herhangi birini kullanarak bir DPS 'e bağlanmak için MQTT protokolünü kullanabilir.

* [Azure IoT sağlama SDK](../iot-hub/iot-hub-devguide-sdks.md#microsoft-azure-provisioning-sdks)'larının kitaplıkları.
* MQTT protokolü doğrudan.

## <a name="using-the-mqtt-protocol-directly-as-a-device"></a>MQTT protokolünü doğrudan kullanma (cihaz olarak)

Bir cihaz, cihaz SDK 'larını kullanalamazsanız, bağlantı noktası 8883 üzerindeki MQTT protokolünü kullanarak ortak cihaz uç noktalarına bağlanabilir. **Bağlantı** paketinde, cihaz aşağıdaki değerleri kullanmalıdır:

* **ClientID** alanı Için, **RegistrationId** kullanın.

* **Kullanıcı adı** alanı için, `{idScope}/registrations/{registration_id}/api-version=2019-03-31` ,, `{idScope}` DPS 'nin [ıdscope](./concepts-service.md#id-scope) değeri olan öğesini kullanın.

* **Parola** alanı IÇIN bir SAS belirteci kullanın. SAS belirtecinin biçimi hem HTTPS hem de AMQP protokolleriyle aynıdır:

  `SharedAccessSignature sr={URL-encoded-resourceURI}&sig={signature-string}&se={expiry}&skn=registration` ResourceURI, biçiminde olmalıdır `{idScope}/registrations/{registration_id}` . İlke adı olmalıdır `registration` .

  > [!NOTE]
  > X. 509.952 sertifikası kimlik doğrulamasını kullanıyorsanız SAS belirteç parolaları gerekmez.

  SAS belirteçleri oluşturma hakkında daha fazla bilgi için bkz. [DPS 'e erişimi denetleme](how-to-control-access.md#security-tokens)güvenlik belirteçleri bölümü.

Aşağıda, DPS uygulamaya özgü davranışların bir listesi verilmiştir:

 * DPS, **Cleansession** bayrağının **0** olarak ayarlanmakta olan işlevleri desteklemez.

 * Bir cihaz uygulaması **QoS 2** ile bir konuya abone olduğunda, DPS, **suback** paketinde maksimum QoS düzey 1 ' i verir. Bundan sonra, DPS QoS 1 kullanarak cihaza ileti gönderir.

## <a name="tlsssl-configuration"></a>TLS/SSL yapılandırması

MQTT protokolünü doğrudan kullanmak için, istemciniz TLS 1,2 *üzerinden bağlanmalıdır.* Bu adımı atlama denemeleri bağlantı hatalarıyla başarısız olur.


## <a name="registering-a-device"></a>Cihaz kaydetme

Bir cihazı DPS 'e kaydetmek için bir cihaz, `$dps/registrations/res/#` **Konu filtresi** olarak kullanarak abone olmalıdır. Konu filtresindeki çok düzeyli joker karakter `#` Yalnızca cihazın konu adında ek özellikler almasına izin vermek için kullanılır. DPS, alt `#` `?` konuların filtrelenmesi için veya joker karakterlerinden kullanılmasına izin vermez. DPS genel amaçlı bir yayın-Sub mesajlaşma Aracısı olmadığından, yalnızca belgelenen konu adlarını ve konu filtrelerini destekler.

Cihaz, `$dps/registrations/PUT/iotdps-register/?$rid={request_id}` **Konu adı** olarak kullanarak DPS 'e bir Register iletisi yayımlamalıdır. Yük, JSON biçiminde [cihaz kayıt](/rest/api/iot-dps/runtimeregistration/registerdevice#deviceregistration) nesnesini içermelidir.
Başarılı bir senaryoda cihaz, bu `$dps/registrations/res/202/?$rid={request_id}&retry-after=x` konuda x 'in yeniden deneme-sonraki değeri saniye olarak olduğu konu adı üzerinde bir yanıt alır. Yanıtın yükü, JSON biçiminde [Registrationoperationstatus](/rest/api/iot-dps/runtimeregistration/registerdevice#registrationoperationstatus) nesnesini içerecektir.

## <a name="polling-for-registration-operation-status"></a>Kayıt işlemi durumu için yoklama

Cihaz, cihaz kayıt işleminin sonucunu almak için hizmeti düzenli aralıklarla yoklamalıdır. Yukarıda belirtildiği gibi, cihazın konuya zaten abone olduğu varsayılırsa `$dps/registrations/res/#` , konu adına bir get OperationStatus iletisi yayımlayabilir `$dps/registrations/GET/iotdps-get-operationstatus/?$rid={request_id}&operationId={operationId}` . Bu iletideki işlem KIMLIĞI, önceki adımda bulunan RegistrationOperationStatus yanıt iletisinde alınan değer olmalıdır. Başarılı durumda, hizmet konuya yanıt verir `$dps/registrations/res/200/?$rid={request_id}` . Yanıtın yükü RegistrationOperationStatus nesnesini içerecektir. Yanıt kodu, yeniden deneme dönemine eşit bir gecikmeden sonra 202 ise, cihaz hizmeti yoklamaya devam etmelidir. Hizmet bir 200 durum kodu döndürürse cihaz kayıt işlemi başarılı olur.

## <a name="connecting-over-websocket"></a>WebSocket üzerinden bağlanma
WebSocket üzerinden bağlanılırken, alt protokolü olarak belirtin `mqtt` . [RFC 6455](https://tools.ietf.org/html/rfc6455)' i izleyin.

## <a name="next-steps"></a>Sonraki adımlar

MQTT protokolü hakkında daha fazla bilgi edinmek için [MQTT belgelerine](https://mqtt.org/)bakın.

DPS 'in yeteneklerini daha fazla incelemek için bkz.:

* [IoT DPS hakkında](about-iot-dps.md)