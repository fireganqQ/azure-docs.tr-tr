---
title: İç yük dengeleyici (ıLB) uç noktası yapılandırma
titleSuffix: Azure Application Gateway
description: Bu makalede, Application Gateway özel bir ön uç IP adresi ile nasıl yapılandırılacağı hakkında bilgi sağlanır
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: how-to
ms.date: 04/16/2020
ms.author: victorh
ms.openlocfilehash: 64dfe284772faf2a345b7959f1a1bd6f474cd1bf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90562494"
---
# <a name="configure-an-application-gateway-with-an-internal-load-balancer-ilb-endpoint"></a>İç yük dengeleyici (ıLB) uç noktası ile uygulama ağ geçidi yapılandırma

Azure Application Gateway, Internet 'e yönelik bir VIP ile veya Internet 'e açık olmayan bir iç uç nokta ile yapılandırılabilir. İç uç nokta, *iç yük dengeleyici (ıLB) uç noktası*olarak da bilinen, ön uç için özel bir IP adresi kullanır.

Ön uç özel IP adresi kullanarak ağ geçidini yapılandırmak, Internet 'e açık olmayan iç iş kolu uygulamaları için yararlıdır. Ayrıca, Internet 'e açık olmayan, ancak daha önce Güvenli Yuva Katmanı (SSL), sonlandırma olarak bilinen ve Aktarım Katmanı Güvenliği (TLS) gerektiren bir güvenlik sınırında olan çok katmanlı bir uygulama içindeki hizmetler ve katmanlar için de kullanışlıdır.

Bu makale, Azure portal kullanarak bir ön uç özel IP adresi ile uygulama ağ geçidi yapılandırma adımlarında size rehberlik eder.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

<https://portal.azure.com> adresinden Azure portalında oturum açın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Azure 'un, oluşturduğunuz kaynaklar arasında iletişim kurması için bir sanal ağa ihtiyacı vardır. Yeni bir sanal ağ oluşturabilir veya var olan bir ağı kullanabilirsiniz. Bu örnekte, yeni bir sanal ağ oluşturursunuz. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz. Application Gateway örnekleri ayrı alt ağlarda oluşturulur. Bu örnekte iki alt ağ oluşturursunuz: bir tane uygulama ağ geçidi ve arka uç sunucuları için bir diğeri.

1. Portal menüsünü genişletin ve **kaynak oluştur**' u seçin.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Uygulama ağ geçidinin adı ve yeni kaynak grubu için *myResourceGroupAG* Için *myappgateway* yazın.
4. **Bölge**Için **(US) Orta ABD**seçin.
5. **Katman**için **Standart**' ı seçin.
6. **Sanal ağı Yapılandır** altında **Yeni oluştur**' u seçin ve ardından sanal ağ için şu değerleri girin:
   - *myVNet* - Sanal ağın adı.
   - *10.0.0.0/16* - Sanal ağın adres alanı.
   - *myAGSubnet* - Alt ağın adı.
   - *10.0.0.0/24* - Alt ağın adres alanı.
   - arka uç alt ağ adı için *Mybackendsubnet* .
   - *10.0.1.0/24* -arka uç alt ağ adres alanı için.

    ![Sanal ağ oluşturma](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-1.png)

6. Sanal ağı ve alt ağı oluşturmak için **Tamam ' ı** seçin.
7. **İleri ' yi seçin: ön uçlar**.
8. **Ön uç IP adresi türü**için **özel**' i seçin.

   Bu, varsayılan olarak dinamik bir IP adresi atamasıdır. Yapılandırılan alt ağın ilk kullanılabilir adresi, ön uç IP adresi olarak atanır.
   > [!NOTE]
   > Ayrıldıktan sonra, IP adresi türü (statik veya dinamik) daha sonra değiştirilemez.
9. **İleri ' yi seçin: Backenler**.
10. **Arka uç Havuzu Ekle**' yi seçin.
11. **Ad**Için *Appgatewaybackendpool*yazın.
12. **Hedefleri olmayan arka uç havuzu ekleme**için **Evet**' i seçin. Hedefleri daha sonra ekleyeceksiniz.
13. **Ekle**’yi seçin.
14. Ileri 'yi seçin **: yapılandırma**.
15. **Yönlendirme kuralları**altında **Kural Ekle**' yi seçin.
16. **Kural adı**Için *rrule-01*yazın.
17. **Dinleyici adı**Için, *Listener-01*yazın.
18. **Ön uç IP 'si**için **özel**' i seçin.
19. Kalan Varsayılanları kabul edin ve **arka uç hedefleri** sekmesini seçin.
20. **Hedef türü**Için **arka uç havuzu**' nu seçin ve ardından **appgatewaybackendpool**' u seçin.
21. **Http ayarı**Için **Yeni oluştur**' u seçin.
22. **Http ayar adı**için *http-Setting-01*yazın.
23. **Arka uç Protokolü**için **http**' yi seçin.
24. **Arka uç bağlantı noktası**için *80*yazın.
25. Kalan Varsayılanları kabul edin ve **Ekle**' yi seçin.
26. **Yönlendirme kuralı ekle** sayfasında **Ekle**' yi seçin.
27. **Sonraki: Etiketler**' i seçin.
28. **Sonraki: Gözden geçirme ve oluşturma**’yı seçin.
29. Özet sayfasındaki ayarları gözden geçirin ve ardından **Oluştur** ' u seçerek ağ kaynaklarını ve uygulama ağ geçidini oluşturun. Uygulama ağ geçidinin oluşturulması birkaç dakika sürebilir. Bir sonraki bölüme geçmeden önce Dağıtım başarıyla bitene kadar bekleyin.

## <a name="add-backend-pool"></a>Arka uç Havuzu Ekle

Arka uç havuzu, isteği sunan arka uç sunucularına istekleri yönlendirmek için kullanılır. Arka uç, NIC 'ler, sanal makine ölçek kümeleri, genel IP adresleri, iç IP adresleri, tam etki alanı adları (FQDN) ve Azure App Service gibi çok kiracılı arka uçlar olabilir. Bu örnekte, sanal makineleri hedef arka uç olarak kullanacaksınız. Var olan sanal makineleri kullanabilir ya da yeni bir tane oluşturabilirsiniz. Bu örnekte, Azure 'un uygulama ağ geçidi için arka uç sunucuları olarak kullandığı iki sanal makine oluşturursunuz.

Bunu yapmak için şunları yapın:

1. Arka uç sunucuları olarak kullanılan, *Myvm* ve *myVM2*olmak üzere iki yeni sanal makine oluşturun.
2. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS 'yi yükler.
3. Arka uç sunucularını arka uç havuzuna ekleyin.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. **Kaynak oluştur**’u seçin.
2. **İşlem** ' ı seçin ve ardından **sanal makine**' yi seçin.
4. Sanal makine için şu değerleri girin:
   - **kaynak grubu**için *myResourceGroupAG* öğesini seçin.
   - **sanal makine adı**Için *myvm* .
   - **Görüntü**Için **Windows Server 2019 Datacenter** ' u seçin.
   - geçerli bir **Kullanıcı adı**.
   - geçerli bir **parola**.
5. Kalan Varsayılanları kabul edin ve **İleri ' yi seçin: diskler**.
6. Varsayılanları kabul edin ve **İleri ' yi seçin: ağ**.
7. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun.
8. Kalan Varsayılanları kabul edin ve **İleri: yönetim**' i seçin.
9. Önyükleme tanılamayı devre dışı bırakmak için **Kapat** ' ı seçin.
10. Kalan Varsayılanları kabul edin ve **İleri: Gelişmiş**' i seçin.
11. Şunu seçin: **İleri: Etiketler**.
12. **İleri ' yi seçin: gözden geçir + oluştur**.
13. Özet sayfasında ayarları gözden geçirin ve ardından **Oluştur**' u seçin. VM 'nin oluşturulması birkaç dakika sürebilir. Bir sonraki bölüme geçmeden önce Dağıtım başarıyla bitene kadar bekleyin.

### <a name="install-iis"></a>IIS yükleme

1. Cloud Shell açın ve **PowerShell**olarak ayarlandığından emin olun.
    ![Ekran görüntüsü PowerShell kullanan açık bir Azure Cloud Shell konsol penceresini gösterir.](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-3.png)
2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın:

   ```azurepowershell
   Set-AzVMExtension `
   
     -ResourceGroupName myResourceGroupAG `
   
     -ExtensionName IIS `
   
     -VMName myVM `
   
     -Publisher Microsoft.Compute `
   
     -ExtensionType CustomScriptExtension `
   
     -TypeHandlerVersion 1.4 `

     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `

     -Location CentralUS `

   ```



3. İkinci bir sanal makine oluşturun ve yeni tamamladığınız adımları kullanarak IIS yükleyin. Set-Azvmexgerin adı ve VMName için myVM2 girin.

### <a name="add-backend-servers-to-backend-pool"></a>Arka uç sunucularını arka uç havuzuna Ekle

1. **Tüm kaynaklar**' ı ve ardından **myappgateway**' i seçin.
2. **Arka uç havuzlarını**seçin. **Appgatewaybackendpool**öğesini seçin.
3. **Hedef türü** ' nün altında **sanal makine** ' yi seçin ve **hedef**altında myvm ile ilişkili vNIC 'i seçin.
4. MyVM2 eklemek için tekrarlayın.
   ![Ekran görüntüsü, hedef türleri ve hedefleri vurgulanmış şekilde arka uç havuzunu Düzenle bölmesini gösterir.](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-4.png)
5. Kaydet ' i seçin **.**

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Portalın **ön uç IP yapılandırması** sayfasına tıklayarak atanan ön uç IP 'nizi denetleyin.
    ![Ekran görüntüsü, özel türü vurgulanmış ön uç IP yapılandırması bölmesini gösterir.](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-5.png)
2. Özel IP adresini kopyalayın ve ardından aynı VNet 'te veya şirket içinde bu VNet 'e bağlantısı olan Şirket içindeki bir VM 'deki tarayıcı adres çubuğuna yapıştırın ve Application Gateway erişmeyi deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Arka ucunuzun durumunu izlemek isterseniz, [Application Gateway Için arka uç sistem durumu ve tanılama günlükleri](application-gateway-diagnostics.md)bölümüne bakın.
