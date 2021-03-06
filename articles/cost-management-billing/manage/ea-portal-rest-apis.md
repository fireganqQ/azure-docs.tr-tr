---
title: Azure Kurumsal REST API'leri
description: Bu makalede Azure kurumsal kaydınızla kullanılabilecek REST API'ler açıklanır.
author: bandersmsft
ms.author: banders
ms.date: 01/21/2021
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: enterprise
ms.reviewer: boalcsva
ms.openlocfilehash: 1fdf64053a55eb33d80ed461c231e8c6dd84d63b
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98677740"
---
# <a name="azure-enterprise-rest-apis"></a>Azure Kurumsal REST API'leri

Bu makalede Azure kurumsal kaydınızla kullanılabilecek REST API'ler açıklanır. Ayrıca REST API'lerle ilgili yaygın sorunların nasıl çözüleceği de açıklanır.

## <a name="consumption-and-usage-apis"></a>Tüketim ve Kullanım API'leri

Microsoft Kurumsal Azure müşterileri REST API'ler aracılığıyla kullanım ve faturalama bilgilerini alabilir. Rol sahibi (Kuruluş Yöneticisi, Bölüm Yöneticisi, Hesap Sahibi) Azure EA portalında bir anahtar oluşturarak API'ye erişimi etkinleştirmelidir. Ardından kayıt numarasının ve anahtarın verildiği herkes API aracılığıyla verilere erişebilir.

### <a name="available-apis"></a>Kullanılabilir API’ler

**Bakiye ve Özet -** [Bakiye ve Özet API'si](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) bakiyeler, yeni satın almalar, Azure Market hizmeti ücretleri, düzeltmeler ve fazla kullanım ücretleri hakkındaki bilgilerin aylık özetini sağlar. Daha fazla bilgi için bkz. [Enterprise müşterileri için Raporlama API'leri - Bakiye ve Özet](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary).

**Kullanım Ayrıntıları -** [Kullanım Ayrıntıları API'si](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) bir kayıt tarafından tüketilen miktarların ve bunların tahmini ücretlerinin günlük dökümünü sağlar. Sonuçlarda örnekler, ölçümler ve departmanlar hakkında bilgiler de yer alır. Faturalama dönemine veya belirtilen başlangıç ve bitiş tarihine göre API'yi sorgulayabilirsiniz. Daha fazla bilgi için bkz. [Enterprise müşterileri için Raporlama API'leri - Kullanım Ayrıntıları](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail).

**Market Mağaza Ücreti -** [Market Mağaza Ücreti API'si](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) kullanıma bağlı market ücretlerinin belirtilen faturalama dönemi veya başlangıç ve bitiş tarihleri için günlük dökümünü döndürür. Daha fazla bilgi için bkz. [Enterprise müşterileri için Raporlama API'leri - Market Mağaza Ücreti](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge).

**Fiyat Listesi -** The [Fiyat Listesi API'si](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) kayıt ve faturalama dönemi için her ölçümün geçerli fiyatını sağlar. Daha fazla bilgi için bkz. [Enterprise müşterileri için Raporlama API'leri - Fiyat Listesi](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet).

**Faturalama Dönemleri -** [Faturalama Dönemleri API'si](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) bir kayıt için ters kronolojik sırada tüketim bilgilerini içeren bir faturalama dönemleri listesi döndürür. Her dönemin dört veri kümesi (BalanceSummary, UsageDetails, MarketplaceCharges ve PriceSheet) için API yoluna işaret eden bir özelliği vardır. Daha fazla bilgi için bkz. [Enterprise müşterileri için Raporlama API'leri - Faturalama Dönemleri](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods).

### <a name="enable-api-data-access"></a>API veri erişimini etkinleştirme

Rol sahipleri Azure EA portalında aşağıdaki adımları uygulayabilir. **Raporlar** > **Kullanımı İndir** > **API Erişim Anahtarı**'na gidebilir. Sonra şunları yapabilir:

- Birincil ve ikincil erişim anahtarlarını oluşturma.
- Erişim anahtarlarını devre dışı bırakma.
- Erişim anahtarlarının başlangıç ve bitiş tarihlerini görüntüleme.

### <a name="generate-or-retrieve-the-api-key"></a>API Anahtarını oluşturma veya alma

1. Kuruluş yöneticisi olarak oturum açın.
2. Sol gezinti penceresinde **Raporlar**'a ve sonra da **Kullanımı İndir** sekmesine tıklayın.
3. **API Erişim Anahtarı**'na tıklayın.
4. **Kayıt Erişim Anahtarları**'nın altında anahtar oluşturma simgesini seçerek birincil veya ikincil anahtar oluşturun.
5. Oluşturulan API erişim anahtarının tamamını görüntülemek için **Anahtarı Genişlet**'i seçin.
6. Hemen kullanmak üzere API erişim anahtarını almak için **Kopyala**'yı seçin.

![API Erişim Anahtarı sayfasını gösteren örnek](./media/ea-portal-rest-apis/ea-create-generate-or-retrieve-the-api-key.png)

Kaydınızda kuruluş yöneticisi olmayan kişilere API erişim anahtarlarını vermek istiyorsanız aşağıdaki adımları uygulayın:

1. Sol gezinti penceresinde **Yönet**'e tıklayın.
2. **DA view charges** (Bölüm Yöneticisi ücretlerini görüntüle) öğesinin yanındaki kalem simgesine tıklayın.
3. **Etkinleştir**'i seçin ve ardından **Kaydet**'e tıklayın.
4. **AO view charges** (Hesap Sahibi ücretlerini görüntüle) öğesinin yanındaki kalem simgesine tıklayın.
5. **Etkinleştir**'i seçin ve ardından **Kaydet**'e tıklayın.

![DA ve AO ücretlerini görüntülemenin etkinleştirilmesini gösteren örnek](./media/ea-portal-rest-apis/create-ea-generate-or-retrieve-api-key-enable-ao-do-view.png) Önceki adımlar API erişim anahtarı sahiplerinin kullanım raporlarındaki maliyet ve fiyatlandırma bilgilerine erişmesini sağlar.

### <a name="pass-keys-in-the-api"></a>API'de anahtarları geçirme

Kimlik doğrulaması ve yetkilendirme için her çağrıda API anahtarını geçirin. Aşağıdaki özelliği HTTP üst bilgilerine geçirin:

| İstek üst bilgisi anahtarı | Değer |
| --- | --- |
| Yetkilendirme | Değeri şu biçimde belirtin: **bearer {API\_KEY}**
Örnek: bearer \&lt;APIKey\&gt; |

### <a name="swagger"></a>Swagger

Aşağıdaki API'ler için [Enterprise Reporting v3 API'lerinde](https://consumption.azure.com/swagger/ui/index) bir Swagger uç noktası sağlanır. Swagger API'nin incelenmesine yardımcı olur. [AutoRest](https://github.com/Azure/AutoRest) veya [Swagger CodeGen](https://swagger.io/swagger-codegen/) aracılığıyla istemci SDK'ları oluşturmak için Swagger kullanın. 1 Mayıs 2014'ten sonra sağlanan veriler API aracılığıyla kullanılabilir.

### <a name="api-response-codes"></a>API yanıt kodları

Bir API kullanırken yanıt durum kodları gösterilir. Aşağıdaki tabloda bunlar açıklanır.

| Yanıt durum kodu | İleti | Açıklama |
| --- | --- | --- |
| 200 | Tamam | Hata yok |
| 401 | Yetkisiz | API Anahtarı bulunamadı, Geçersiz, Süresi Doldu vb. |
| 404 | Kullanılamaz | Rapor uç noktası bulunamadı |
| 400 | Hatalı İstek | Geçersiz parametreler – Tarih aralıkları, Kurumsal Anlaşma numaraları vb. |
| 500 | Sunucu Hatası | İstek işlenirken beklenmeyen hata oluştu |

### <a name="usage-and-billing-data-update-frequency"></a>Kullanım ve faturalama verileri güncelleştirme sıklığı

Kullanım ve faturalama veri dosyaları, geçerli faturalama ayı için her 24 saatte bir güncelleştirilir. Öte yandan üç güne kadar veri gecikmesi oluşabilir. Örneğin kullanım Pazartesi günü tahakkuk ettiyse veriler Perşembe gününe kadar veri dosyasında görüntülenmeyebilir.

### <a name="azure-service-catalog"></a>Azure hizmet kataloğu

Tüm Azure hizmetleri bir Azure depolama blogunda CSV biçimindeki bir kataloğa gönderilir. Sisteminiz için tüm Azure hizmetlerinin seçmeli bir kataloğunu oluşturmanız gerekiyorsa bu katalog yararlı olur. Geçerli katalog [https://azurecatalog.blob.core.windows.net/catalog/AzureCatalog.csv](https://azurecatalog.blob.core.windows.net/catalog/AzureCatalog.csv) .

### <a name="csv-data-file-details"></a>CSV veri dosyası ayrıntıları

Aşağıdaki bilgilerde API raporlarının özellikleri açıklanır.

#### <a name="usage-summary"></a>Kullanım özeti

CSV raporundan JSON biçimi oluşturulur. Sonuç olarak bu biçim özet CSV biçimiyle aynıdır. Sütun adı korunur, dolayısıyla JSON özet verilerini kullanırken bir veri tablosunda seri durumdan çıkarmanız gerekir.

| CSV sütun adı | JSON sütun adı | JSON yeni sütunu | Yorum |
| --- | --- | --- | --- |
| AccountOwnerId | AccountOwnerLiveId | AccountOwnerLiveId |   |
| Hesap Adı | AccountName | AccountName |   |
| ServiceAdministratorId | ServiceAdministratorLiveId | ServiceAdministratorLiveId |   |
| SubscriptionId | SubscriptionId | SubscriptionId |   |
| SubscriptionGuid | MOCPSubscriptionGuid | SubscriptionGuid |   |
| Abonelik Adı | SubscriptionName | SubscriptionName |   |
| Tarih | Tarih | Tarih | Hizmet kataloğu raporunun çalıştırıldığı tarihi gösterir. Biçimi, zaman damgası içermeyen bir tarih dizesidir. |
| Ay | Ay | Ay |   |
| Gün | Gün | Gün |   |
| Yıl | Yıl | Yıl |   |
| Ürün | BillableItemName | Ürün |   |
| Ölçüm kimliği | ResourceGUID | MeterId |   |
| Sayaç Kategorisi | Hizmet | MeterCategory | Hizmetleri bulmaya yardımcı olmak açısından yararlıdır. Birden çok ServiceType değeri olan hizmetler için uygundur. Örneğin Sanal Makineler. |
| Sayaç Alt Kategorisi | ServiceType | MeterSubCategory | Hizmet için ikinci bir ayrıntı düzeyi sağlar. Örneğin A1 VM (Windows Dışı).  |
| Tarife Bölgesi | ServiceRegion | MeterRegion | Hizmet için gereken üçüncü ayrıntı düzeyi. ResourceGUID değerinin bölge bağlamını bulma açısından yararlıdır. |
| Sayaç Adı | ServiceResource | MeterName | Hizmetin adı. |
| Tüketilen Miktar | ResourceQtyConsumed | ConsumedQuantity |   |
| ResourceRate | ResourceRate | ResourceRate |   |
| ExtendedCost | ExtendedCost | ExtendedCost |   |
| Kaynak Konumu | ServiceSubRegion | ResourceLocation |   |
| Tüketim Hizmeti | ServiceInfo | ConsumedService |   |
| Örnek Kimliği | Bileşen | InstanceId |   |
| ServiceInfo1 | ServiceInfo1 | ServiceInfo1 |   |
| ServiceInfo2 | ServiceInfo2 | ServiceInfo2 |   |
| AdditionalInfo | AdditionalInfo | AdditionalInfo |   |
| Etiketler | Etiketler | Etiketler |   |
| Depolama Hizmeti Tanımlayıcısı   | OrderNumber | StoreServiceIdentifier   |   |
| Bölüm Adı | DepartmentName | DepartmentName |   |
| Maliyet Merkezi | CostCenter | CostCenter |   |
| Ölçü Birimi | UnitOfMeasure | UnitOfMeasure | Örnek değerler: saatler, GB, olaylar, gönderim, birim, Birim Saatleri, MB, günlük birimler |
| adlı yönetilen örnek, | adlı yönetilen örnek, | adlı yönetilen örnek, |   |

#### <a name="azure-marketplace-report"></a>Azure Market raporu

| CSV sütun adı | JSON sütun adı | JSON yeni sütunu |
| --- | --- | --- |
| AccountOwnerId | AccountOwnerId | AccountOwnerId |
| Hesap Adı | AccountName | AccountName |
| SubscriptionId | SubscriptionId | SubscriptionId |
| SubscriptionGuid | SubscriptionGuid | SubscriptionGuid |
| Abonelik Adı | SubscriptionName |  SubscriptionName |
| Tarih | BillingCycle |  Date (Yalnızca Tarih Dizesi. Zaman damgası yok)
| Ay | Ay |  Ay |
| Gün | Gün |  Gün |
| Yıl | Yıl |  Yıl |
| Ölçüm kimliği | MeterResourceId |  MeterId |
| Yayımcı Adı | PublisherFriendlyName |  PublisherName |
| Teklif Adı | OfferFriendlyName |  OfferName |
| Plan Adı | PlanFriendlyName |  PlanName |
| Tüketilen Miktar | BilledQty |  ConsumedQuantity |
| ResourceRate | ResourceRate | ResourceRate |
| ExtendedCost | ExtendedCost | ExtendedCost |
| Ölçü Birimi | UnitOfMeasure | UnitOfMeasure |
| Örnek Kimliği | InstanceId | InstanceId |
| Ek Bilgi | AdditionalInfo | AdditionalInfo |
| Etiketler | Etiketler | Etiketler |
| Sipariş Numarası | OrderNumber | OrderNumber |
| Bölüm Adı | DepartmentNames | DepartmentName |
| Maliyet Merkezi | CostCenters |  CostCenter |
| Kaynak Grubu | adlı yönetilen örnek, |  adlı yönetilen örnek, |

#### <a name="price-sheet"></a>Fiyat listesi

| CSV sütun adı | JSON sütun adı | Yorum |
| --- | --- | --- |
| Hizmet | Hizmet |  Fiyatta değişiklik yok |
| Ölçü Birimi | UnitOfMeasure |   |
| Fazla Kullanım Parça Numarası | ConsumptionPartNumber |   |
| Fazla Kullanım Birim Fiyatı | ConsumptionPrice |   |
| Para Birimi Kodu | CurrencyCode |     |

### <a name="common-api-issues"></a>Yaygın API sorunları

Azure Enterprise REST API'lerini kullanırken aşağıdaki yaygın sorunlardan biriyle karşılaşabilirsiniz.

Yetkilendirme türü doğru olmayan bir API anahtarı kullanmaya çalışıyor olabilirsiniz. API anahtarlarını oluşturanlar:

- Kuruluş Yöneticisi
- Bölüm Yöneticisi (DA)
- Hesap Sahibi (AO)

EA yöneticisi tarafından oluşturulan anahtar söz konusu kaydın tüm bilgilerine erişim verir. Salt okuma erişimli EA yöneticisi API anahtarı oluşturamaz.

DA veya AO tarafından oluşturulan anahtar bakiye ücret ve fiyat listesi bilgilerine erişim vermez.

API anahtarının süresi her altı ayda bir dolar. Süresi dolduğunda yeniden oluşturmanız gerekir.

Zaman aşımı hatası alırsanız, zaman aşımı eşiği sınırını artırarak bu hatayı düzeltebilirsiniz.

401 (yetkisiz) süre sonu hatası alabilirsiniz. Bu hataya normalde süresi dolan anahtarlar neden olur. Süresi dolan anahtarı yeniden oluşturabilirsiniz.

Seçilen tarih aralığına ilişkin geçerli veriler olmadığında API çağrısından 400 ve 404 (kullanılamıyor) hataları döndürülebilir. Örneğin, kısa süre önce bir kayıt aktarımının başlatılmış olması nedeniyle bu hata oluşabilir. Belirli bir tarihe ve sonrasına ait veriler artık yeni kayıtta bulunmaktadır. Bunun dışında, eski kayıtta bulunan bilgileri almak için yeni bir kayıt numarası kullanmanız durumunda da bu hata oluşabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure EA portalı yöneticilerinin yaygın yönetim görevleri hakkında bilgi edinmek için [Azure EA portalı yönetimi](ea-portal-administration.md) makalesini okumaları gerekir.
- Azure EA portalı sorunlarını gidermek için yardıma ihtiyacınız varsa [Azure EA portalı erişim sorunlarını giderme](ea-portal-troubleshoot.md) konusuna bakın.