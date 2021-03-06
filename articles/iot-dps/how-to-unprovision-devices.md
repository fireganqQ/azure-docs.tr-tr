---
title: Azure IoT Hub cihaz sağlama hizmeti ile sağlanan cihazların sağlamasını kaldırma
description: Azure IoT Hub cihaz sağlama hizmeti (DPS) ile sağlanan cihazların sağlamasını kaldırma
author: wesmc7777
ms.author: wesmc
ms.date: 05/11/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 37099b551e004ebfc702dce37c2a1499aa46ff10
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94950745"
---
# <a name="how-to-deprovision-devices-that-were-previously-auto-provisioned"></a>Daha önce otomatik olarak sağlanan cihazların sağlamasını kaldırma 

Daha önce cihaz sağlama hizmeti aracılığıyla otomatik olarak sağlanan cihazların sağlanması için gerekli olduğunu fark edebilirsiniz. Örneğin, bir cihaz, farklı bir IoT Hub 'ına satılabilir veya taşınabilir ya da kaybolmuş, çalınmış veya başka bir şekilde tehlikeye girmiş olabilir. 

Genel olarak, bir cihazın sağlamasını kaldırma işlemi iki adımdan oluşur:

1. Gelecekteki otomatik sağlamayı engellemek için cihazı sağlama hizmetinizden kaydedin. Erişimi geçici olarak veya kalıcı olarak iptal etmek isteyip istemediğinize bağlı olarak, bir kayıt girişini devre dışı bırakmak veya silmek isteyebilirsiniz. X. 509.440 kanıtlama kullanan cihazlarda, mevcut kayıt gruplarınızın hiyerarşisindeki bir girişi devre dışı bırakmak/silmek isteyebilirsiniz.  
 
   - Bir cihazın kaydını nasıl kaydedeceğinizi öğrenmek için bkz. [Azure IoT Hub cihaz sağlama hizmeti 'nden cihaz ayırma](how-to-revoke-device-access-portal.md).
   - Sağlama hizmeti SDK 'larının birini kullanarak bir cihazın programlı olarak nasıl kaydedileceğini öğrenmek için bkz. cihaz kayıtlarını [hizmet SDK 'ları Ile yönetme](how-to-manage-enrollments-sdks.md).

2. Gelecekteki iletişimleri ve veri aktarımını engellemek için IoT Hub cihazın kaydını silin. Yeniden devre dışı bırakıp, kimlik kayıt defterinde cihazın girişini, sağlandığı IoT Hub için geçici olarak devre dışı bırakabilir veya kalıcı olarak silebilirsiniz. Disablement hakkında daha fazla bilgi için bkz. [cihazları devre dışı bırakma](../iot-hub/iot-hub-devguide-identity-registry.md#disable-devices) . [Azure portal](https://portal.azure.com)IoT Hub kaynağınız Için "cihaz yönetimi/IoT cihazları" başlığına bakın.

Bir cihazın sağlamasını kaldırmak için alacağınız tam adımlar, kanıtlama mekanizmasına ve sağlama hizmetinizdeki ilgili kayıt girişine bağlıdır. Aşağıdaki bölümlerde, kayıt ve kanıtlama türüne göre işlem için genel bir bakış sağlanmaktadır.

## <a name="individual-enrollments"></a>Bireysel kayıtlar
Bir yaprak sertifikayla TPM kanıtlama veya X. 509.440 kanıtlama kullanan cihazlar, tek bir kayıt girişi aracılığıyla sağlanır. 

Tek bir kaydı olan bir cihazı sağlamayı kaldırmak için: 

1. Cihazı sağlama hizmetinizden kaldır:

   - TPM kanıtlama kullanan cihazlarda, cihazın sağlama hizmetine erişimini kalıcı olarak iptal etmek için ayrı kayıt girişini silin veya erişimi geçici olarak iptal etmek için girişi devre dışı bırakın. 
   - X. 509.440 kanıtlama kullanan cihazlarda, girdiyi silebilir veya devre dışı bırakabilirsiniz. Bunun farkında olun, ancak X. 509.440 kullanan bir cihaz için tek bir kaydı silerseniz ve bu cihazın sertifika zincirindeki bir imzalama sertifikası için etkinleştirilmiş bir kayıt grubu varsa, cihaz yeniden kaydolabilir. Bu tür cihazlarda, kayıt girişinin devre dışı bırakılması daha güvenli olabilir. Bunun yapılması, imza sertifikalarından biri için etkinleştirilmiş bir kayıt grubunun olup olmamasına bakılmaksızın cihazın yeniden kaydedilmesini engeller.

2. Sağlanan IoT Hub 'ının kimlik kayıt defterindeki cihazı devre dışı bırakın veya silin. 


## <a name="enrollment-groups"></a>Kayıt grupları
X. 509.440 kanıtlama ile, cihazlar bir kayıt grubu aracılığıyla da sağlanabilir. Kayıt grupları bir imzalama sertifikasıyla, ara veya kök CA sertifikasıyla yapılandırılır ve sertifika zincirinde bu sertifikaya sahip cihazlar için sağlama hizmetine erişimi denetler. Sağlama hizmeti ile kayıt grupları ve X. 509.440 sertifikaları hakkında daha fazla bilgi edinmek için bkz. [X. 509.440 sertifika kanıtlama](concepts-x509-attestation.md). 

Bir kayıt grubu aracılığıyla sağlanmış cihazların listesini görmek için kayıt grubunun ayrıntılarını görüntüleyebilirsiniz. Bu, her bir cihazın hangi IoT Hub 'ına sağlandığını anlamanın kolay bir yoludur. Cihaz listesini görüntülemek için: 

1. Azure portal oturum açın ve sol taraftaki menüden **tüm kaynaklar** ' a tıklayın.
2. Kaynak listesinde sağlama hizmetinize tıklayın.
3. Sağlama hizmetinizde kayıtları **Yönet**' e ve ardından **kayıt grupları** sekmesini seçin.
4. Kayıt grubuna tıklayarak açın.

   ![Portalda kayıt grubu girişini görüntüleme](./media/how-to-unprovision-devices/view-enrollment-group.png)

Kayıt gruplarıyla, dikkate alınması gereken iki senaryo vardır:

- Bir kayıt grubu aracılığıyla sağlanmış tüm cihazların sağlanması için:
  1. İmzalama sertifikasına izin vermemek için kayıt grubunu devre dışı bırakın. 
  2. Her bir cihazı ilgili IoT Hub 'ının kimlik kayıt defterinden devre dışı bırakmak veya silmek için, bu kayıt grubu için sağlanan cihazların listesini kullanın. 
  3. Tüm cihazları ilgili IoT Hub 'larından devre dışı bıraktıktan veya sildikten sonra, isteğe bağlı olarak kayıt grubunu silebilirsiniz. Bu durumda, kayıt grubunu silerseniz ve bir veya daha fazla cihazın sertifika zincirinde daha yüksek bir imzalama sertifikası için etkinleştirilmiş bir kayıt grubu varsa, bu cihazların yeniden kaydolabileceği farkında olun. 

- Bir kayıt grubundan tek bir cihazı sağlamayı kaldırmak için:
  1. Yaprak (cihaz) sertifikası için devre dışı bir bireysel kayıt oluşturun. Bu, bu cihaz için sağlama hizmetine erişimi iptal ederken, kayıt grubunun imzalama sertifikasına sahip diğer cihazlara erişim izni almaya devam eder. Cihazın devre dışı bırakılmış kaydını silmeyin. Bunun yapılması, cihazın kayıt grubuna yeniden kaydolmasını sağlar. 
  2. Cihazın sağlandığı IoT Hub 'ını bulmak için bu kayıt grubu için sağlanan cihazların listesini kullanın ve bu hub 'ın kimlik kayıt defterinde devre dışı bırakın ya da silin.