---
title: Arama dizini oluşturma için Azure SQL VM bağlantısı
titleSuffix: Azure Cognitive Search
description: Şifrelenmiş bağlantıları etkinleştirin ve güvenlik duvarını Azure Bilişsel Arama bir dizin oluşturucudan bir Azure sanal makinesinde (VM) SQL Server bağlantılara izin verecek şekilde yapılandırın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/12/2020
ms.openlocfilehash: a13f78b6aa4fc3cb6f6777c76bc762ec565624fc
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91951324"
---
# <a name="configure-a-connection-from-an-azure-cognitive-search-indexer-to-sql-server-on-an-azure-vm"></a>Azure Bilişsel Arama Dizin oluşturucudan Azure sanal makinesinde SQL Server bağlantı yapılandırma

Azure [SQL veritabanı 'nı Dizinleyicileri kullanarak azure bilişsel arama bağlama](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq)bölümünde belirtildiği gibi, Azure sanal makinelerinde (veya short Için **SQL Azure vm 'ler** ) **SQL Server** karşı dizin oluşturma işlemi Azure bilişsel arama tarafından desteklenir, ancak ilk olarak ele almanız gereken güvenlikle ilgili birkaç önkoşul vardır. 

Azure Bilişsel Arama sanal makine üzerindeki SQL Server bağlantı, genel bir internet bağlantısıdır. Normalde bu bağlantılar için takip ettiğiniz tüm güvenlik önlemleri burada da geçerlidir:

+ Azure VM 'de SQL Server örneğinin tam etki alanı adı için bir [sertifika yetkilisi sağlayıcısından](https://en.wikipedia.org/wiki/Certificate_authority#Providers) sertifika alın.
+ Sertifikayı VM 'ye yükler ve ardından bu makaledeki yönergeleri kullanarak VM 'de şifreli bağlantıları etkinleştirin ve yapılandırın.

## <a name="enable-encrypted-connections"></a>Şifrelenmiş bağlantıları etkinleştir
Azure Bilişsel Arama, genel bir internet bağlantısı üzerinden tüm Dizin Oluşturucu istekleri için şifreli bir kanal gerektirir. Bu bölümde, bu işi yapma adımları listelenir.

1. Konu adının Azure VM 'nin tam etki alanı adı (FQDN) olduğunu doğrulamak için sertifikanın özelliklerini denetleyin. Özellikleri görüntülemek için CertUtils veya Sertifikalar ek bileşeni gibi bir araç kullanabilirsiniz. FQDN 'yi VM hizmeti dikey penceresinin temel bileşenler bölümünden [Azure Portal](https://portal.azure.com/) **genel IP adresi/DNS ad etiketi** alanından edinebilirsiniz.
   
   * Yeni **Kaynak Yöneticisi** şablonu kullanılarak oluşturulan VM 'ler IÇIN, FQDN şöyle biçimlendirilir `<your-VM-name>.<region>.cloudapp.azure.com`
   * **Klasik** VM olarak oluşturulan eski VM 'ler için FQDN olarak biçimlendirilir `<your-cloud-service-name.cloudapp.net>` .

2. SQL Server kayıt defteri Düzenleyicisi 'Ni (regedit) kullanarak sertifikayı kullanacak şekilde yapılandırın. 
   
    Bu görev için genellikle SQL Server Yapılandırma Yöneticisi kullanılmasına karşın, bu senaryo için kullanamazsınız. Azure üzerindeki VM 'nin FQDN 'si VM tarafından belirlendiği şekilde FQDN ile eşleşmediğinden, içeri aktarılan sertifika bulmaz (etki alanını yerel bilgisayar veya katıldığı ağ etki alanı olarak tanımlar). Adlar eşleşmezse, sertifikayı belirtmek için regedit ' i kullanın.
   
   * Regedit ' de bu kayıt defteri anahtarına gidin: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate` .
     
     `[MSSQL13.MSSQLSERVER]`Bölüm, sürüm ve örnek adına göre farklılık gösterir. 
   * **Sertifika** anahtarı değerini VM 'ye aktardığınız TLS/SSL sertifikasının **parmak izine** ayarlayın.
     
     Parmak izini almanın birkaç yolu vardır, bazıları diğerlerinden daha iyidir. Bunu MMC 'deki **Sertifikalar** ek bileşeninden kopyalarsanız, büyük olasılıkla [Bu destek makalesinde açıklandığı gibi](https://support.microsoft.com/kb/2023869/)görünmez bir öndeki karakter seçerek bir bağlantı girişiminizde hata oluşur. Bu sorunu düzeltmek için birkaç geçici çözüm vardır. En kolay geri alma işlemi yapılır ve sonra, regedit içindeki anahtar değer alanındaki baştaki karakteri kaldırmak için parmak izinin ilk karakterini yeniden yazın. Alternatif olarak, parmak izini kopyalamak için farklı bir araç kullanabilirsiniz.

3. Hizmet hesabına izin verin. 
   
    SQL Server hizmet hesabına TLS/SSL sertifikasının özel anahtarı üzerinde uygun izin verildiğinden emin olun. Bu adımı ortaya geçirirseniz SQL Server başlamaz. Bu görev için **Sertifikalar** ek bileşenini veya **certutils** ' i kullanabilirsiniz.
    
4. SQL Server hizmetini yeniden başlatın.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>VM 'de SQL Server bağlantısını yapılandırma
Azure Bilişsel Arama için gereken şifreli bağlantıyı ayarladıktan sonra, Azure VM 'lerinde SQL Server ek yapılandırma adımları vardır. Daha önce yapmadıysanız, sonraki adım Bu makalelerden birini kullanarak yapılandırmayı tamamlamadır:

* **Kaynak Yöneticisi** VM için bkz. [Kaynak Yöneticisi kullanarak Azure 'Da SQL Server sanal makinesine bağlanma](../azure-sql/virtual-machines/windows/ways-to-connect-to-sql.md). 
* **Klasik** bir VM için bkz. [Azure 'Da SQL Server sanal makinesine bağlanma](/previous-versions/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect).

Özellikle, "internet üzerinden bağlantı" için her makaledeki bölümü gözden geçirin.

## <a name="configure-the-network-security-group-nsg"></a>Ağ güvenlik grubunu yapılandırma (NSG)
Azure VM 'nizi diğer taraflarca erişilebilir hale getirmek için NSG ve ilgili Azure uç noktası veya Access Control listesi (ACL) yapılandırmak olağan dışı değildir. Bu işlemi, kendi uygulama mantığınızın SQL Azure sanal makinenize bağlanmasına izin vermeden önce yaptık. Bu, SQL Azure sanal makinenize yönelik bir Azure Bilişsel Arama bağlantısı için farklı değildir. 

Aşağıdaki bağlantılar, VM dağıtımları için NSG yapılandırması hakkında yönergeler sağlar. Bu yönergeleri, IP adresini temel alarak bir Azure Bilişsel Arama uç noktası ACL 'sine yönelik olarak kullanın.

> [!NOTE]
> Arka plan için bkz. [ağ güvenlik grubu nedir?](../virtual-network/network-security-groups-overview.md)
> 
> 

* **Kaynak Yöneticisi** VM için bkz. [ARM dağıtımları için NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md). 
* **Klasik** bir VM için bkz. [Klasik dağıtımlar için NSG oluşturma](/previous-versions/azure/virtual-network/virtual-networks-create-nsg-classic-ps).

IP adresleme, sorunu ve olası geçici çözümleri fark ediyorsanız kolayca ele alınması gereken birkaç zorluk ortaya çıkarabilir. Kalan bölümler, ACL 'deki IP adresleriyle ilgili sorunları işlemeye yönelik öneriler sağlar.

#### <a name="restrict-access-to-the-azure-cognitive-search"></a>Azure Bilişsel Arama erişimi kısıtlama
`AzureCognitiveSearch`SQL Azure VM 'lerinizi tüm bağlantı isteklerine açık hale getirmek yerine, arama HIZMETINIZIN IP adresine ve ACL 'deki [HIZMET etiketinin](../virtual-network/service-tags-overview.md#available-service-tags) IP adresi aralığına erişimi kısıtlamanız önemle önerilir.

Arama hizmetinizin FQDN 'sini (örneğin,) ping yaparak IP adresini bulabilirsiniz `<your-search-service-name>.search.windows.net` .

Hizmet etiketinin IP adresi aralığını `AzureCognitiveSearch` [service tag](../virtual-network/service-tags-overview.md#available-service-tags) [Indirilebilir JSON dosyalarını](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files) veya [hizmet etiketi bulma API 'si](../virtual-network/service-tags-overview.md#use-the-service-tag-discovery-api-public-preview)aracılığıyla bulabilirsiniz. IP adresi aralığı haftalık olarak güncelleştirilir.

#### <a name="managing-ip-address-fluctuations"></a>IP adresi dalgalanmaları yönetme
Arama hizmetinizin yalnızca bir arama birimi (yani bir çoğaltma ve bir bölüm) varsa, hizmet yeniden başlatmaları sırasında IP adresi değişir ve bu, arama hizmetinizin IP adresine sahip mevcut bir ACL 'yi geçersiz kılmıştır.

Sonraki bağlantı hatasından kaçınmak için bir yol, Azure Bilişsel Arama 'de birden fazla çoğaltma ve bir bölüm kullanmaktır. Bunun yapılması maliyeti artırır, ancak IP adresi sorununu da çözer. Azure Bilişsel Arama 'de, birden fazla arama biriminiz olduğunda IP adresleri değişmez.

İkinci bir yaklaşım bağlantının başarısız olmasına izin vermek ve ardından NSG 'deki ACL 'Leri yeniden yapılandırmak. Ortalama olarak, IP adreslerinin her birkaç haftada bir değiştirilmesini bekleyebilir. Bu yaklaşım, seyrek olarak dizin oluşturmayı denetleyen müşteriler için önemli olabilir.

Bir üçüncü uygun (özellikle güvenli değil) yaklaşım, arama hizmetinizin sağlandığı Azure bölgesinin IP adresi aralığını belirtmektir. Genel IP adreslerinin Azure kaynaklarına ayrıldığı IP aralıklarının listesi, [Azure veri MERKEZI IP aralıklarında](https://www.microsoft.com/download/details.aspx?id=41653)yayımlanır. 

#### <a name="include-the-azure-cognitive-search-portal-ip-addresses"></a>Azure Bilişsel Arama portal IP adreslerini ekleme
Bir dizin oluşturucu oluşturmak için Azure portal kullanıyorsanız, Azure Bilişsel Arama portal mantığının oluşturma sırasında SQL Azure sanal makinenize erişmesi da gerekir. Azure Bilişsel Arama portal IP adresleri, ping işlemi tarafından bulunabilir `stamp2.search.ext.azure.com` .

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma sayesinde artık Azure VM 'de bir Azure Bilişsel Arama Dizin Oluşturucu için veri kaynağı olarak bir SQL Server belirtebilirsiniz. Daha fazla bilgi için bkz. [Dizin oluşturucular kullanarak Azure SQL veritabanı 'Nı azure bilişsel arama bağlama](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) .