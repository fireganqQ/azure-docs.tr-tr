---
title: Tepesinde bölgesinde yük dengeli Azure Web uygulamaları barındırın
description: Tepesinde bölgesinde yük dengeli Web uygulamaları barındırmak için bir Azure DNS diğer ad kaydı kullanın
services: dns
author: rohinkoul
ms.service: dns
ms.topic: how-to
ms.date: 08/10/2019
ms.author: rohink
ms.openlocfilehash: 72adb2732eb0832589cbc25fb7e4288eb1899214
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94954520"
---
# <a name="host-load-balanced-azure-web-apps-at-the-zone-apex"></a>Tepesinde bölgesinde yük dengeli Azure Web uygulamaları barındırın

DNS protokolü, tepesinde bölgesinde bir veya AAAA kaydı dışında herhangi bir şeyin atanmasını engeller. Örnek bir bölge tepesinde contoso.com. Bu kısıtlama, Traffic Manager arkasındaki yük dengeli uygulamalara sahip olan uygulama sahipleri için bir sorun gösterir. Bölge tepesinde kaydından Traffic Manager profile işaret etmek mümkün değildir. Sonuç olarak, uygulama sahiplerinin geçici bir çözüm kullanması gerekir. Uygulama katmanında yeniden yönlendirme, Zone tepesinde 'tan başka bir etki alanına yeniden yönlendirmelidir. Örnek, contoso.com 'ten www contoso.com 'e yeniden yönlendirme örneğidir \. . Bu düzenleme, yeniden yönlendirme işlevi için tek bir hata noktası sunar.

Diğer ad kayıtlarıyla, bu sorun artık yok. Artık uygulama sahipleri, bölge tepesinde kayıtlarını dış uç noktalara sahip bir Traffic Manager profiline işaret edebilir. Uygulama sahipleri, DNS bölgesi içindeki diğer etki alanı için kullanılan aynı Traffic Manager profilini işaret edebilir.

Örneğin, contoso.com ve www \. contoso.com aynı Traffic Manager profilini işaret edebilir. Traffic Manager profilinin yalnızca yapılandırılmış dış uç noktaları olduğu sürece bu durum geçerlidir.

Bu makalede, etki alanı tepesinde için bir diğer ad kaydı oluşturmayı ve Web uygulamalarınız için Traffic Manager profili uç noktalarınızı yapılandırmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Birlikte test edilecek Azure DNS içinde barındırabileceğiniz bir etki alanı adınızın olması gerekir. Bu etki alanı üzerinde tam denetime sahip olmanız gerekir. Tam denetim, etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağını kapsar.

Azure DNS’te etki alanınızı barındırma yönergeleri için bkz. [Öğretici: Azure DNS’te etki alanınızı barındırma](dns-delegate-domain-azure-dns.md).

Bu öğreticide örnek olarak contoso.com etki alanı kullanılmaktadır ancak sizin kendi etki alanı adınızı kullanmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bu makalede kullanılan tüm kaynakları barındıracak bir kaynak grubu oluşturun.

## <a name="create-app-service-plans"></a>App Service planları oluşturma

Yapılandırma bilgileri için aşağıdaki tabloyu kullanarak kaynak grubunuzda iki Web App Service planı oluşturun. App Service planı oluşturma hakkında daha fazla bilgi için bkz. [Azure 'da App Service planını yönetme](../app-service/app-service-plan-manage.md).


|Name  |İşletim Sistemi  |Konum  |Fiyatlandırma Katmanı  |
|---------|---------|---------|---------|
|ASP-01     |Windows|Doğu ABD|Geliştirme ve test D1-Shared|
|ASP-02     |Windows|Central US|Geliştirme ve test D1-Shared|

## <a name="create-app-services"></a>Uygulama hizmetleri oluşturma

Her bir App Service planında bir tane olmak üzere iki Web uygulaması oluşturun.

1. Azure portal sayfanın sol üst köşesinde **kaynak oluştur**' u seçin.
2. Arama çubuğuna **Web uygulaması** yazın ve ENTER tuşuna basın.
3. **Web uygulaması**' nı seçin.
4. **Oluştur**’u seçin.
5. Varsayılan değerleri kabul edin ve iki Web uygulaması yapılandırmak için aşağıdaki tabloyu kullanın:

   |Name<br>(. azurewebsites.net içinde benzersiz olmalıdır)|Kaynak Grubu |Çalışma zamanı yığını|Region|Plan/konum App Service
   |---------|---------|-|-|-------|
   |Uygulama-01|Var olanı kullan<br>Kaynak grubunuzu seçin|.NET Core 2.2|Doğu ABD|ASP-01 (D1)|
   |Uygulama-02|Var olanı kullan<br>Kaynak grubunuzu seçin|.NET Core 2.2|Central US|ASP-02 (D1)|

### <a name="gather-some-details"></a>Bazı ayrıntılar toplayın

Şimdi Web Apps için IP adresi ve ana bilgisayar adı ' nı aklınızda olmanız gerekir.

1. Kaynak grubunuzu açın ve ilk Web uygulamanızı (Bu örnekte **App-01** ) seçin.
2. Sol sütunda **Özellikler**' i seçin.
3. **URL 'nin** altındaki adresi ve **giden IP adresleri** altındaki listedeki ilk IP adresini aklınızda yapın. Traffic Manager bitiş noktalarınızı yapılandırırken bu bilgileri daha sonra kullanacaksınız.
4. **App-02** için yineleyin.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Kaynak grubunuzda bir Traffic Manager profili oluşturun. Varsayılan değerleri kullanın ve trafficmanager.net ad alanı içinde benzersiz bir ad yazın.

Traffic Manager profili oluşturma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: yüksek düzeyde kullanılabilir bir Web uygulaması için Traffic Manager profili oluşturma](../traffic-manager/quickstart-create-traffic-manager-profile.md).

### <a name="create-endpoints"></a>Uç noktalar oluşturma

Artık iki Web uygulaması için uç noktalar oluşturabilirsiniz.

1. Kaynak grubunuzu açın ve Traffic Manager profilinizi seçin.
2. Sol sütunda **bitiş noktaları**' nı seçin.
3. **Ekle**’yi seçin.
4. Uç noktaları yapılandırmak için aşağıdaki tabloyu kullanın:

   |Tür  |Name  |Hedef  |Konum  |Özel üstbilgi ayarları|
   |---------|---------|---------|---------|---------|
   |Dış uç nokta     |Son-01|Uygulama için kaydettiğiniz IP adresi-01|Doğu ABD|konağının\<the URL you recorded for App-01\><br>Örnek: **Host: App-01.azurewebsites.net**|
   |Dış uç nokta     |End-02|App-02 için kaydettiğiniz IP adresi|Central US|konağının\<the URL you recorded for App-02\><br>Örnek: **Host: App-02.azurewebsites.net**

## <a name="create-dns-zone"></a>DNS bölgesi oluştur

Test için varolan bir DNS bölgesi kullanabilir veya yeni bir bölge oluşturabilirsiniz. Azure 'da yeni bir DNS bölgesi oluşturup atamak için bkz. [öğretici: etki alanınızı barındırma Azure DNS](dns-delegate-domain-azure-dns.md).

## <a name="add-a-txt-record-for-custom-domain-validation"></a>Özel etki alanı doğrulaması için bir TXT kaydı ekleme

Web uygulamalarınıza özel bir ana bilgisayar adı eklediğinizde, etki alanınızı doğrulamak üzere belirli bir TXT kaydına bakar.

1. Kaynak grubunuzu açın ve DNS bölgesini seçin.
2. **Kayıt kümesi**’ni seçin.
3. Aşağıdaki tabloyu kullanarak kayıt kümesini ekleyin. Değer için, daha önce kaydettiğiniz gerçek Web uygulaması URL 'sini kullanın:

   |Ad  |Tür  |Değer|
   |---------|---------|-|
   |@     |TXT|App-01.azurewebsites.net|


## <a name="add-a-custom-domain"></a>Özel etki alanı ekleme

Her iki Web uygulaması için özel bir etki alanı ekleyin.

1. Kaynak grubunuzu açın ve ilk Web uygulamanızı seçin.
2. Sol sütunda **özel etki alanları**' nı seçin.
3. **Özel etki alanları** altında **özel etki alanı Ekle**' yi seçin.
4. **Özel etki alanı** altında, özel etki alanı adınızı yazın. Örneğin, contoso.com.
5. **Doğrula**'yı seçin.

   Etki alanınız, **ana bilgisayar adı kullanılabilirliği** ve **etki alanı sahipliğinin** yanında doğrulamayı geçmelidir ve yeşil onay işaretlerini göstermelidir.
5. **Özel etki alanı ekle**'yi seçin.
6. **Siteye atanan ana bilgisayar adları** altındaki yeni ana bilgisayar adını görmek için tarayıcınızı yenileyin. Sayfadaki yenileme her zaman değişiklikleri hemen göstermez.
7. İkinci Web uygulamanız için bu yordamı tekrarlayın.

## <a name="add-the-alias-record-set"></a>Diğer ad kayıt kümesini ekleme

Şimdi bölge tepesinde için bir diğer ad kaydı ekleyin.

1. Kaynak grubunuzu açın ve DNS bölgesini seçin.
2. **Kayıt kümesi**’ni seçin.
3. Aşağıdaki tabloyu kullanarak kayıt kümesini ekleyin:

   |Ad  |Tür  |Diğer ad kayıt kümesi  |Diğer ad türü  |Azure kaynağı|
   |---------|---------|---------|---------|-----|
   |@     |A|Yes|Azure kaynağı|Traffic Manager-profiliniz|


## <a name="test-your-web-apps"></a>Web uygulamalarınızı test etme

Artık Web uygulamanıza ulaşabildiğinizden ve yük dengeli olduğundan emin olmak için test edebilirsiniz.

1. Bir Web tarayıcısı açın ve etki alanına gidin. Örneğin, contoso.com. Varsayılan Web uygulaması sayfasını görmeniz gerekir.
2. İlk Web uygulamanızı durdurun.
3. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
4. Web tarayıcınızı başlatın ve etki alanına gidin. Yine de varsayılan Web uygulaması sayfasını görmeniz gerekir.
5. İkinci Web uygulamanızı durdurun.
6. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
7. Web tarayıcınızı başlatın ve etki alanına gidin. Web uygulamasının durdurulduğunu belirten 403 hatasını görmeniz gerekir.
8. İkinci Web uygulamanızı başlatın.
9. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
10. Web tarayıcınızı başlatın ve etki alanına gidin. Varsayılan Web uygulaması sayfasını yeniden görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Diğer ad kayıtları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Öğretici: Azure genel IP adresine başvurmak için diğer ad kaydı yapılandırma](tutorial-alias-pip.md)
- [Öğretici: Traffic Manager ile tepe etki alanı adlarını desteklemek için diğer ad kaydı yapılandırma](tutorial-alias-tm.md)
- [DNS SSS](./dns-faq.md#alias-records)

Etkin bir DNS adını nasıl geçirebileceğinizi öğrenmek için bkz. [Azure App Service için etkin BIR DNS adı geçirme](../app-service/manage-custom-dns-migrate-domain.md).