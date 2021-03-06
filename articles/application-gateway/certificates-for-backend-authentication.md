---
title: Arka uç sunucularına izin vermek için gerekli sertifikalar
titleSuffix: Azure Application Gateway
description: Bu makalede, bir TLS/SSL sertifikasının Azure Application Gateway arka uç örneklerine izin vermek için gerekli olan kimlik doğrulama sertifikasına ve güvenilen kök sertifikaya nasıl dönüştürülebileceği hakkında örnekler verilmektedir.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: how-to
ms.date: 06/17/2020
ms.author: absha
ms.openlocfilehash: 874e554063f64ddefce99a223678d64b2e0774c3
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397731"
---
# <a name="create-certificates-to-allow-the-backend-with-azure-application-gateway"></a>Azure Application Gateway arka uca izin vermek için sertifikalar oluşturma

Application Gateway bitiş için TLS, kimlik doğrulama/güvenilen kök sertifikaları karşıya yükleyerek arka uç örneklerine izin verilmesini gerektirir. V1 SKU 'SU için kimlik doğrulama sertifikaları gereklidir, ancak sertifikalara izin vermek için v2 SKU 'SU güvenilen kök sertifikalarının olması gerekir.

Bu makalede şunları öğreneceksiniz:


- Bir arka uç sertifikasından kimlik doğrulama sertifikasını dışarı aktar (v1 SKU 'SU için)
- Güvenilen kök sertifikayı bir arka uç sertifikasından dışarı aktar (v2 SKU 'SU için)

## <a name="prerequisites"></a>Ön koşullar

Application Gateway ile arka uç örneklerine izin vermek için gereken kimlik doğrulama sertifikalarını veya güvenilir kök sertifikaları oluşturmak için mevcut bir arka uç sertifikası gerekir. Arka uç sertifikası, TLS/SSL sertifikasıyla aynı veya ek güvenlik için farklı olabilir. Application Gateway, size bir TLS/SSL sertifikası oluşturma veya satın alma mekanizması sağlamaz. Test amacıyla, otomatik olarak imzalanan bir sertifika oluşturabilirsiniz, ancak bunu üretim iş yükleri için kullanmamalısınız. 

## <a name="export-authentication-certificate-for-v1-sku"></a>Kimlik doğrulama sertifikasını dışa aktarma (v1 SKU 'SU için)

Application Gateway v1 SKU 'sunda arka uç örneklerine izin vermek için bir kimlik doğrulama sertifikası gerekir. Kimlik doğrulama sertifikası, Base-64 ile kodlanmış X. 509.440 (. CER) biçiminde. Bu örnekte, arka uç sertifikası için bir TLS/SSL sertifikası kullanacaksınız ve ortak anahtarını kimlik doğrulama sertifikası olarak kullanılacak şekilde dışarı aktaracaksınız. Ayrıca, bu örnekte, gerekli sertifikaları dışarı aktarmak için Windows Sertifika Yöneticisi aracını kullanacaksınız. Uygun olan herhangi bir aracı kullanmayı seçebilirsiniz.

TLS/SSL sertifikaınızdan ortak anahtar. cer dosyasını (özel anahtarı değil) dışarı aktarın. Aşağıdaki adımlar Base-64 ile kodlanmış X. 509.440 (. cer) dosyasına dışarı aktarma yapmanıza yardımcı olur. CER) sertifikanızın biçimi:

1. Sertifikadan bir .cer dosyası almak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Sertifikayı, genellikle ' sertifikalar-geçerli kullanıcı \ kişisel \ Sertifikalar ' içinde bulun ve sağ tıklayın. **Tüm Görevler** ’e tıklayın ve ardından **Dışarı Aktar** ’a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır. Geçerli Kullanıcı \ kişisel \ sertifikalar altındaki sertifikayı bulamazsanız, yanlışlıkla "Sertifikalar-Geçerli Kullanıcı" yerine "Sertifikalar-Yerel bilgisayar" seçeneğini açmış olabilirsiniz. PowerShell kullanarak geçerli kullanıcı kapsamında sertifika yöneticisi 'ni açmak istiyorsanız konsol penceresine *certmgr* yazın.

   ![Ekran görüntüsü, seçili sertifikaların bulunduğu sertifika yöneticisini ve tüm görevleri içeren bir bağlamsal menüyü gösterir ve ardından seçili olarak dışa aktarın.](./media/certificates-for-backend-authentication/export.png)

2. Sihirbazda, **İleri** ' ye tıklayın.

   ![Sertifikayı dışarı aktarma](./media/certificates-for-backend-authentication/exportwizard.png)

3. **Hayır, özel anahtarı dışarı aktarma** ’yı seçin ve **İleri** ’ye tıklayın.

   ![Özel anahtarı dışarı aktarma](./media/certificates-for-backend-authentication/notprivatekey.png)

4. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin ve **İleri** ’ye tıklayın.

   ![Base-64 kodlu](./media/certificates-for-backend-authentication/base64.png)

5. **Dışarı aktarılacak dosya** için, sertifikayı dışarı aktarmak istediğiniz konuma **gidin** . **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri** ' ye tıklayın.

   ![Ekran görüntüsü, dışarı aktarılacak bir dosyayı belirttiğiniz sertifika dışarı aktarma Sihirbazı ' nı gösterir.](./media/certificates-for-backend-authentication/browse.png)

6. Sertifikayı dışarı aktarmak için **Son** 'a tıklayın.

   ![Dosya dışarı aktarmayı tamamladıktan sonra, ekran görüntüsü sertifika dışarı aktarma Sihirbazı ' nı gösterir.](./media/certificates-for-backend-authentication/finish.png)

7. Sertifikanız başarıyla verildi.

   ![Ekran görüntüsü, bir başarı iletisi ile sertifika dışarı aktarma Sihirbazı 'nı gösterir.](./media/certificates-for-backend-authentication/success.png)

   Verdiğiniz sertifika şuna benzer:

   ![Ekran görüntüsünde bir sertifika simgesi gösterilir.](./media/certificates-for-backend-authentication/exported.png)

8. Not defteri 'Ni kullanarak, dışarıya aktarılmış sertifikayı açarsanız, bu örneğe benzer bir şey görürsünüz. Blue bölümünde Application Gateway 'e yüklenen bilgiler yer alır. Sertifikanızı Not defteri ile açarsanız ve bu şuna benzemezse genellikle bu, temel-64 kodlu X. 509.440 (. CER) biçiminde. Ayrıca, farklı bir metin Düzenleyicisi kullanmak istiyorsanız, bazı düzenleyicilerin arka planda istenmeden biçimlendirme getirebileceğini anlayın. Bu, metni Bu sertifikadan Azure 'a yüklerken sorun oluşturabilir.

   ![Not defteri ile aç](./media/certificates-for-backend-authentication/format.png)

## <a name="export-trusted-root-certificate-for-v2-sku"></a>Güvenilen kök sertifikayı dışarı aktarma (v2 SKU 'SU için)

Uygulama ağ geçidi v2 SKU 'sunda arka uç örneklerine izin vermek için güvenilen kök sertifika gereklidir. Kök sertifika, Base-64 ile kodlanmış bir X. 509.440 (. CER) arka uç sunucu sertifikalarından kök sertifikayı biçimlendirin. Bu örnekte, arka uç sertifikası için bir TLS/SSL sertifikası kullanacağız, ortak anahtarını dışarı aktarıp güvenilir CA 'nın kök sertifikasını Base64 kodlamalı biçimde ortak anahtardan dışarı aktararak güvenilen kök sertifikayı alırsınız. Ara sertifikalar sunucu sertifikasıyla paketlenmiş ve arka uç sunucusuna yüklenmiş olmalıdır.

Aşağıdaki adımlar, sertifikanız için. cer dosyasını dışarı aktarmaya yardımcı olur:

1. Arka uç sertifikaınızdan ortak anahtarı dışarı aktarmak için önceki bölümde belirtilen [kimlik doğrulama sertifikası (v1 SKU 'su için)](#export-authentication-certificate-for-v1-sku) ile ilgili 1-8 adımları kullanın.

2. Ortak anahtar verildikten sonra dosyayı açın.

   ![Yetkilendirme Sertifikası aç](./media/certificates-for-backend-authentication/openAuthcert.png)

   ![Sertifika hakkında](./media/certificates-for-backend-authentication/general.png)

3. Sertifika yetkilisini görüntülemek için sertifika yolu görünümüne geçin.

   ![sertifika ayrıntıları](./media/certificates-for-backend-authentication/certdetails.png)

4. Kök sertifikayı seçin ve **sertifikayı görüntüle** ' ye tıklayın.

   ![Sertifika yolu](./media/certificates-for-backend-authentication/rootcert.png)

   Kök sertifika ayrıntılarını görmeniz gerekir.

   ![Sertifika bilgileri](./media/certificates-for-backend-authentication/rootcertdetails.png)

5. **Ayrıntılar** görünümüne gidin ve **Dosyaya Kopyala...** seçeneğine tıklayın.

   ![kök sertifikayı Kopyala](./media/certificates-for-backend-authentication/rootcertcopytofile.png)

6. Bu noktada, kök sertifikanın ayrıntılarını arka uç sertifikasından ayıkladık. **Sertifika dışarı aktarma Sihirbazı** 'nı görürsünüz. Bundan sonra, temel-64 kodlamalı X. 509.440 () içindeki güvenilen kök sertifikayı dışarı aktarmak için yukarıdaki **bir arka uç sertifikasından (v1 SKU 'su için) kimlik doğrulama sertifikasını dışarı aktarma** bölümünde bahsedilen 2-9 adımları kullanın. CER) biçiminde.

## <a name="next-steps"></a>Sonraki adımlar

Artık Base-64 ile kodlanmış X. 509.440 () kimlik doğrulama sertifikasına/güvenilen kök sertifikaya sahipsiniz. CER) biçiminde. Arka uç sunucularınızın uçtan uca TLS şifrelemesini sağlamak için bunu Application Gateway 'e ekleyebilirsiniz. Bkz. [PowerShell ile Application Gateway kullanarak uçtan uca TLS Yapılandırma](./application-gateway-end-to-end-ssl-powershell.md).