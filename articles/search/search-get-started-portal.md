---
title: Azure portal arama dizini oluşturma
titleSuffix: Azure Cognitive Search
description: Bu Azure portal hızlı başlangıçta, Azure Bilişsel Arama 'de ilk arama dizininizi oluşturmak, yüklemek ve sorgulamak için veri alma Sihirbazı 'nı kullanın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 12/12/2020
ms.openlocfilehash: 1e9d63c88cf0cd6f65db99b2bc878797770d53cd
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97368639"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal Azure Bilişsel Arama dizini oluşturma

**Veri alma** Sihirbazı 'nı ve kurgusal otel verilerinden oluşan yerleşik bir örnek veri kaynağını kullanarak ilk dizininizi oluşturun. Sihirbaz bir arama dizini (oteller-örnek-dizin) oluşturma sürecinde size kılavuzluk eder. böylece, dakikalar içinde ilginç sorgular yazabilirsiniz. 

Bu hızlı başlangıçta seçenekleri kullanamazsınız, ancak görüntü dosyalarından metin ve yapıyı ayıklayabilmeniz için, sihirbaz AI zenginleştirme için bir sayfa içerir. AI zenginleştirme içeren benzer bir anlatım için bkz. [hızlı başlangıç:](cognitive-search-quickstart-blob.md)Bilişsel Beceri oluşturma.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilere sahip olmanız gerekir:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/).

+ Bir Azure Bilişsel Arama hizmeti. Geçerli aboneliğiniz kapsamında [bir hizmet oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

### <a name="check-for-space"></a>Alan denetleme

Birçok müşteri ücretsiz hizmetle başlar. Bu sürüm üç dizin, üç veri kaynağı ve üç dizin oluşturucu ile sınırlıdır. Başlamadan önce ek öğeler için yeriniz olduğundan emin olun. Bu öğreticide her nesneden birer tane oluşturulur.

Hizmet panosundaki bölümler, kaç Dizin, Dizin Oluşturucu ve veri kaynağı olduğunu gösterir. 

:::image type="content" source="media/search-get-started-portal/tiles-indexers-datasources.png" alt-text="Dizin, Dizin oluşturucular ve veri kaynakları listesi":::

## <a name="create-an-index-and-load-data"></a><a name="create-index"></a> Dizin oluşturma ve veri yükleme

Arama sorguları aranabilir veriler, meta veriler ve arama davranışlarını iyileştiren ek yapılar içeren bir [*dizin*](search-what-is-an-index.md) kullanır.

Bu öğreticide, [ **veri alma** Sihirbazı](search-import-data-portal.md)aracılığıyla bir [*Dizin Oluşturucu*](search-indexer-overview.md) kullanılarak gezinilebilen yerleşik bir örnek veri kümesi kullanırız. Dizin oluşturucu, kaynağa özgü bir gezgindir ve desteklenen Azure veri kaynaklarındaki meta verileri ve içeriği okuyabilir. Normalde, Dizin oluşturucular programlı olarak kullanılır, ancak portalda **verileri Içeri aktarma** Sihirbazı aracılığıyla erişebilirsiniz. 

### <a name="step-1---start-the-import-data-wizard-and-create-a-data-source"></a>1. adım-veri alma Sihirbazı 'nı başlatın ve bir veri kaynağı oluşturun

1. Azure hesabınızla [Azure portalında](https://portal.azure.com/) oturum açın.

1. [Arama hizmetinizi bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/) ve genel bakış sayfasında, bir arama dizini oluşturmak ve doldurmak için komut çubuğunda **verileri içeri aktar** ' a tıklayın.

   :::image type="content" source="media/search-get-started-portal/import-data-cmd.png" alt-text="Verileri içeri aktar komutu":::

1. Sihirbazda, **veri**  >    >  **örneklerimize Bağlan oteller-örnek**' e tıklayın. Bu veri kaynağı yerleşik olarak bulunur. Kendi veri kaynağınızı oluşturuyorsanız ad, tür ve bağlantı bilgilerini belirtmeniz gerekir. Oluşturulan kaynak, diğer içeri aktarma işlemlerinde yeniden kullanılabilecek bir “mevcut veri kaynağı” olur.

   :::image type="content" source="media/search-get-started-portal/import-datasource-sample.png" alt-text="Örnek veri kümesi seçme":::

1. Sonraki sayfaya devam edin.

### <a name="step-2---skip-the-enrich-content-page"></a>2. adım-"zenginleştirme içerik" sayfasını atlayın

Sihirbaz bilişsel hizmetler AI algoritmalarını dizine eklemek için bir [AI zenginleştirme ardışık düzeni](cognitive-search-concept-intro.md) oluşturmayı destekler. 

Bu adımı şimdilik atlayacağız ve **hedef dizini özelleştirmek** için doğrudan üzerine ilerliyoruz.

   :::image type="content" source="media/search-get-started-portal/skip-cog-skill-step.png" alt-text="Bilişsel beceri adımını atlama":::

> [!TIP]
> Bir [hızlı başlangıç](cognitive-search-quickstart-blob.md) veya [öğreticide](cognitive-search-tutorial-blob.md)bir AI dizin oluşturma örneği aracılığıyla ilerlemesini sağlayabilirsiniz.

### <a name="step-3---configure-index"></a>3. adım-dizini yapılandırma

Genellikle, dizin oluşturma, veri yüklenmeden önce tamamlanan kod tabanlı bir uygulamadır. Ancak, Bu öğreticinin gösterdiği gibi, sihirbaz gezinebileceği herhangi bir veri kaynağı için temel bir dizin oluşturabilir. Dizin için en azından bir ad ve alan koleksiyonu gerekir ve her belgenin benzersiz olarak tanımlanabilmesi için bir alanın belge anahtarı olarak işaretlenmiş olması gerekir. Ayrıca, otomatik tamamlama veya önerilen sorguları isterseniz dil Çözümleyicileri veya öneri araçları belirtebilirsiniz.

Alanların veri türleri ve öznitelikleri vardır. Üstteki onay kutuları, alanın nasıl kullanılacağını denetleyen *dizin öznitelikleridir*.

* **Alınabilir**, arama sonuçları listesinde çıktığı anlamına gelir. Bu onay kutusunu temizleyerek, örneğin yalnızca filtre ifadelerinde kullanılan alanlar için, bu onay kutusunu temizleyerek, tek tek alanları, arama sonuçları için sınırlı olarak işaretleyebilirsiniz.
* **Anahtar** , benzersiz belge tanımlayıcısıdır. Her zaman bir dizedir ve gereklidir.
* **Filtrelenebilir**, **sıralanabilir** ve çok **yönlü tablo** , alanların bir filtre, sıralama veya çok yönlü gezinti yapısında kullanılıp kullanılmadığını belirtir.
* **Aranabilir**, bir alanın tam metin aramasına dahil olduğu anlamına gelir. Dizelerde arama yapılabilir. Sayısal alanlar ve Boolean alanları genellikle aranamaz olarak işaretlenir.

Depolama gereksinimleri, seçiminizin sonucu olarak farklılık gösterir. Örneğin, birden çok alanda **alınabilir** özniteliğini ayarlarsanız, depolama gereksinimleri güncel değildir.

Varsayılan olarak sihirbaz tarafından anahtar alanının temeli olarak benzersiz tanımlayıcıların bulunması için veri kaynağı taranır. *Dizeler* **alınabilir** ve **aranabilir** olarak atanır. *Tamsayılar* **alınabilir**, **filtrelenebilir**, **sıralanabilir** ve çok **yönlü tablo** olarak atanır.

1. Varsayılanları kabul edin.

   Sihirbazı ikinci kez mevcut bir oteller veri kaynağını kullanarak yeniden çalıştırırsanız, dizin varsayılan özniteliklerle yapılandırılmaz. Gelecekteki içeri aktarmalardan öznitelikleri el ile seçmeniz gerekir. 

   :::image type="content" source="media/search-get-started-portal/hotelsindex.png" alt-text="Oluşturulan oteller dizini":::

2. Sonraki sayfaya devam edin.


### <a name="step-4---configure-indexer"></a>4. adım-Dizin oluşturucuyu yapılandırma

**Verileri içeri aktarma** sihirbazından çıkmadan **Dizin Oluşturucu** > **Ad**’a tıklayın ve dizin oluşturucu için bir ad yazın.

Bu nesne, yürütülebilir bir işlemi tanımlar. Yinelenen bir zamanlamaya göre koyabilirsiniz, ancak şimdilik Dizin oluşturucuyu hemen çalıştırmak için varsayılan seçeneği kullanın.

Dizin oluşturucuyu oluşturmak ve aynı anda çalıştırmak için **Gönder** ' e tıklayın.

  :::image type="content" source="media/search-get-started-portal/hotels-indexer.png" alt-text="oteller Dizin Oluşturucusu":::

## <a name="monitor-progress"></a>İlerlemeyi izleme

Sihirbaz sizi ilerlemeyi izleyebileceğiniz Dizin oluşturucular listesine götürebilmelidir. Kendi kendine gezinme için genel bakış sayfasına gidin ve **Dizin oluşturucular**' ye tıklayın.

Portalın sayfayı güncelleştirmesi birkaç dakika sürebilir, ancak "devam ediyor" veya Success, "sürüyor" veya Success (Dizin oluşturulmuş belge sayısıyla birlikte) durumuyla yeni oluşturulan dizin oluşturucuyu görmeniz gerekir.

   :::image type="content" source="media/search-get-started-portal/indexers-inprogress.png" alt-text="Dizin oluşturucu ilerleme durumu iletisi":::

## <a name="view-the-index"></a>Dizini görüntüleme

Ana hizmet sayfası, Azure Bilişsel Arama hizmetinizde oluşturulan kaynaklara bağlantılar sağlar.  Yeni oluşturduğunuz dizini görüntülemek için bağlantılar listesinden **dizinler** ' e tıklayın. 

Portal sayfasının yenilenmesini bekleyin. Birkaç dakika sonra, dizini bir belge sayısı ve depolama boyutuyla görmeniz gerekir.

   :::image type="content" source="media/search-get-started-portal/indexes-list.png" alt-text="Hizmet panosundaki dizinler listesi":::

Bu listeden, yeni oluşturduğunuz *oteller-örnek* dizinine tıklayabilirsiniz, Dizin şemasını görüntüleyebilirsiniz. ve isteğe bağlı olarak yeni alanlar ekleyin. 

**Alanlar** sekmesi Dizin şemasını gösterir. Yeni bir alan girmek için listenin en altına gidin. Çoğu durumda, mevcut alanları değiştiremezsiniz. Mevcut alanlar Azure Bilişsel Arama bir fiziksel gösterimine sahiptir ve bu nedenle kodda bile değil, değiştirilemeyen değildir. Var olan bir alanı temel olarak değiştirmek için yeni bir dizin oluşturun ve özgün olanı bırakarak.

   :::image type="content" source="media/search-get-started-portal/sample-index-def.png" alt-text="Örnek dizin tanımı":::

Puanlama profilleri ve CORS seçenekleri gibi diğer yapılar herhangi bir noktada eklenebilir.

Dizin tasarımı sırasında düzenleme yapabileceğiniz ve yapamayacağınız alanları kavramak için birkaç dakikanızı ayırarak dizin tanımı seçeneklerini görüntüleyin. Gri renkli seçenekler, bir değerin değiştirme veya silme işlemleri için uygun olmadığını gösterir. 

## <a name="query-using-search-explorer"></a><a name="query-index"></a> Arama Gezgini 'ni kullanarak sorgulama

Artık yerleşik [**Arama gezgini**](search-explorer.md) sorgu sayfasını kullanarak sorgulayabileceğiniz bir arama dizinine sahipsiniz. Bu sayfada rastgele sorgu dizelerini test etmek için kullanabileceğiniz bir arama kutusu bulunur.

**Arama Gezgini** yalnızca [REST API isteklerini](/rest/api/searchservice/search-documents)işleyecek şekilde donatılmıştır, ancak hem [basit sorgu söz dizimi](/rest/api/searchservice/simple-query-syntax-in-azure-search) hem de [tam Lucene sorgu ayrıştırıcısı](/rest/api/searchservice/lucene-query-syntax-in-azure-search)için sözdizimi kabul eder, ayrıca arama belgesinde bulunan tüm arama parametreleri ve [Arama belgesi REST API](/rest/api/searchservice/search-documents#bkmk_examples) işlemleri.

> [!TIP]
> Aşağıdaki adımlar, 6m08s ' de [Azure bilişsel arama genel bakış videosu ' ne](https://channel9.msdn.com/Events/Connect/2016/138)gösterilmiştir.
>

1. Komut çubuğunda **Arama gezgini**'ne tıklayın.

   :::image type="content" source="media/search-get-started-portal/search-explorer-cmd.png" alt-text="Search gezgini komutu":::

2. **Dizin** açılan listesinden *oteller-örnek-dizin*' i seçin. Hangi REST API 'Lerinin kullanılabilir olduğunu görmek için **API sürümü** açılan listesine tıklayın. Aşağıdaki sorgular için genel kullanıma sunulan sürümü (2020-06-30) kullanın.

   :::image type="content" source="media/search-get-started-portal/search-explorer-changeindex.png" alt-text="Dizin ve API komutları":::

3. Arama çubuğunda, aşağıdaki sorgu dizelerini yapıştırın ve **Ara**' ya tıklayın.

   :::image type="content" source="media/search-get-started-portal/search-explorer-query-string-example.png" alt-text="Sorgu dizesi ve arama düğmesi":::

## <a name="example-queries"></a>Örnek sorgular

Bing veya Google aramasında yapabileceklerinize benzer şekilde hüküm ve ifadeler girebilir veya tam olarak belirtilen sorgu ifadeleriyle karşılaşabilirsiniz. Sonuçlar ayrıntılı JSON belgeleri olarak döndürülür.

### <a name="simple-query-with-top-n-results"></a>İlk N sonucu içeren basit sorgu

#### <a name="example-string-query-searchspa"></a>Örnek (dize sorgusu): `search=spa`

* **Arama** parametresi, tam metin araması için bir anahtar sözcük arama girişi yapmak için kullanılır, bu durumda, belgedeki aranabilir herhangi bir alanda *Spa* 'yı içeren kişiler için otel verileri döndürür.

* **Search gezgini** sonuçları JSON biçiminde döndürülür. Bu biçim ayrıntılı olmakla birlikte, belgelerin yoğun bir yapısı varsa okunması zordur. Bu bilerek yapılır; belgenin tamamına ilişkin görünürlük, özellikle test sırasında geliştirme amaçlarıyla önemlidir. Daha iyi bir kullanıcı deneyimi sunmak için [arama sonuçlarını işleyerek](search-pagination-page-layout.md) önemli öğeleri öne çıkaran bir kod yazmanız gerekir.

* Belgeler, dizinde "alınabilir" olarak işaretlenmiş tüm alanlardan oluşur. Portalda Dizin özniteliklerini görüntülemek için, **dizinler** listesinde *oteller-Sample* ' a tıklayın.

#### <a name="example-parameterized-query-searchspacounttruetop10"></a>Örnek (parametreli sorgu): `search=spa&$count=true&$top=10`

* **&** Simge, herhangi bir sırada belirtilebilen arama parametrelerini eklemek için kullanılır.

* **$Count = true** parametresi, döndürülen tüm belgelerin toplam sayısını döndürür. Bu değer arama sonuçlarının en üstüne yakın bir konumda görünür. **$count=true** tarafından bildirilen değişiklikleri izleyerek filtre sorgularını doğrulayabilirsiniz. Daha küçük sayılar filtrenizin çalıştığını gösterir.

* **$Top = 10** , toplamda en yüksek dereceli 10 belgeyi döndürür. Varsayılan olarak, Azure Bilişsel Arama en iyi 50 eşleşme döndürür. **$top** ile bu miktarı artırabilir veya azaltabilirsiniz.

### <a name="filter-the-query"></a><a name="filter-query"></a> Sorguyu filtreleme

**$filter** parametresini eklediğinizde, arama isteklerine filtreler de eklenir. 

#### <a name="example-filtered-searchbeachfilterrating-gt-4"></a>Örnek (filtrelenmiş): `search=beach&$filter=Rating gt 4`

* **$filter** parametresi, sağladığınız ölçütlerle eşleşen sonuçları döndürür. Bu durumda, 4 ' ten büyük derecelendirmeler.

* Filtre söz dizimi bir OData yapısıdır. Daha fazla bilgi edinmek için bkz. [OData söz dizimini filtreleme](/rest/api/searchservice/odata-expression-syntax-for-azure-search).

### <a name="facet-the-query"></a><a name="facet-query"></a> Sorguyu modelleme

Model filtreleri arama isteklerine dahil edilir. Sağladığınız model değeriyle eşleşen belgelerin toplam sayısını döndürmek için facet parametresini kullanabilirsiniz.

#### <a name="example-faceted-with-scope-reduction-searchfacetcategorytop2"></a>Örnek (kapsamı azaltılarak modellenmiş): `search=*&facet=Category&$top=2`

* **Search =** _ boş bir aramadır. Boş aramalar her şeyi arar. Boş sorgu göndermenin nedenlerinden biri, belge kümesinin tamamını filtrelemek veya görüntülemektir. Örneğin, dizin içindeki tüm otellerden oluşan bir gezinti yapısının olmasını istersiniz.
_ **modeli** , bir UI denetimine geçirebilmeniz için bir gezinti yapısı döndürür. Kategorileri ve bir sayımı döndürür. Bu durumda, kategoriler *Kategori* olarak adlandırılan bir alanı temel alır. Azure Bilişsel Arama 'de toplama yoktur, ancak toplama yoluyla `facet` , her kategoride belge sayısı veren bir dizi belge elde edebilirsiniz.

* **$top=2** iki belge getirir ve sonuçları azaltmak veya artırmak için `top` kullanabileceğinizi gösterir.

#### <a name="example-facet-on-numeric-values-searchspafacetrating"></a>Örnek (sayısal değerlerle modelleme): `search=spa&facet=Rating`

* Bu sorgu, *Spa*'nın metin aramasında derecelendirme için bir model. Alan, dizinde alınabilir, *filtrelenebilir ve* çok yönlü tablo olarak işaretlendiğinden ve içerdiği değerler (sayısal, 1 ' den 5 ' e kadar), listelerin gruplar halinde kategorilere ayrılması için uygundur.

* Yalnızca filtrelenebilir alanlardan görünüm oluşturulabilir. Yalnızca getirilebilir alanlar sonuçlarda döndürülebilir.

* *Derecelendirme* alanı çift duyarlıklı kayan nokta ve gruplama kesin değere göre yapılır. Aralığa göre gruplandırma hakkında daha fazla bilgi için (örneğin, "3 yıldız derecelendirmesi," "4 yıldız derecelendirmesi" vb.), bkz. [Azure bilişsel arama 'da çok yönlü gezintiyi uygulama](./search-faceted-navigation.md#filter-based-on-a-range).


### <a name="highlight-search-results"></a><a name="highlight-query"></a> Arama sonuçlarını vurgulama

İsabet vurgulama, belirli bir alanda eşleşme bulunduğunda anahtar sözcükle eşleşen metinlere biçimlendirme eklenmesini ifade eder. Arama teriminiz uzun bir açıklamanın belirsiz bir yerindeyse, terimi bulmayı kolaylaştırmak için isabet vurgulama ekleyebilirsiniz.

#### <a name="example-highlighter-searchbeachhighlightdescription"></a>Örnek (vurgulama): `search=beach&highlight=Description`

* Bu örnekte, biçimlendirilen sözcük *plaj* , açıklama alanında daha kolay olacak.

#### <a name="example-linguistic-analysis-searchbeacheshighlightdescription"></a>Örnek (dilbilimsel analiz): `search=beaches&highlight=Description`

* Tam metin araması, Word formlarında temel çeşitlemeleri tanır. Bu durumda, arama sonuçları "plaj" için vurgulanmış metni, bu sözcüğü "Güzeller" üzerinde bir anahtar sözcük aramasına yanıt olarak, bu sözcüğe sahip olan oteller için "plaj" için vurgulanmış metni içerir. Dilbilimsel analiz nedeniyle sonuçlarda aynı kelimenin farklı biçimleri görüntülenebilir. 

* Azure Bilişsel Arama, Lucene ve Microsoft 'tan 56 Çözümleyicileri destekler. Azure Bilişsel Arama tarafından kullanılan varsayılan değer standart Lucene çözümleyicisidir.

### <a name="try-fuzzy-search"></a><a name="fuzzy-search"></a> Belirsiz aramayı deneme

Varsayılan olarak, "Seattle" için *Seatle* gibi yanlış yazılmış sorgu terimleri, tipik aramada eşleşmeleri döndürmeyebilir. Aşağıdaki örnek sonuç döndürmez.

#### <a name="example-misspelled-term-unhandled-searchseatle"></a>Örnek (yanlış yazılmış terim, işlenmiyor): `search=seatle`

Yazım hatalarını işlemek için belirsiz aramayı kullanabilirsiniz. Belirsiz arama, tam Lucene sorgu söz dizimini kullandığınızda etkinleştirilir ve bunun için yapmanız gereken iki işlem vardır: sorguda **queryType=full** belirtin ve arama sorgusunun sonuna **~** ekleyin.

#### <a name="example-misspelled-term-handled-searchseatlequerytypefull"></a>Örnek (yanlış yazılmış terim, işleniyor): `search=seatle~&queryType=full`

Bu örnek şimdi "Seattle" ile eşleşmeleri içeren belgeleri geri döndürüyor.

**queryType** belirtildiğinde varsayılan basit sorgu ayrıştırıcı kullanılır. Basit sorgu ayrıştırıcı daha hızlıdır ancak belirsiz arama, normal ifadeler, yakınlık araması ya da diğer gelişmiş sorgu türlerini kullanmanız gerekiyorsa tam söz dizimi gereklidir.

Belirsiz arama ve joker karakter araması, arama sonucunu etkiler. Bu sorgu biçimlerinde dilbilimsel analiz gerçekleştirilmez. Benzer ve joker karakter aramasını kullanmadan önce, [tam metin aramasının Azure bilişsel arama 'de nasıl çalıştığını](search-lucene-query-architecture.md#stage-2-lexical-analysis) gözden geçirin ve sözcük temelli analize yönelik özel durumlar hakkındaki bölümü bulun.

Tam sorgu ayrıştırıcısı tarafından etkinleştirilen sorgu senaryoları hakkında daha fazla bilgi için bkz. [Azure bilişsel arama 'de Lucene sorgu söz dizimi](/rest/api/searchservice/lucene-query-syntax-in-azure-search).

### <a name="try-geospatial-search"></a><a name="geo-search"></a> Jeo-uzamsal aramayı deneme

Koordinat içeren bir alanda [edm.GeographyPoint veri türü](/rest/api/searchservice/supported-data-types) aracılığıyla jeo-uzamsal arama desteklenir. Coğrafi arama, [OData söz dizimini filtrele](/rest/api/searchservice/odata-expression-syntax-for-azure-search) seçeneğinde belirtilen bir tür filtredir.

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-12212-4767-le-5"></a>Örnek (coğrafi koordinat filtreleri): `search=*&$count=true&$filter=geo.distance(Location,geography'POINT(-122.12 47.67)') le 5`

Örnek sorgu tüm sonuçları konumsal verilere göre filtreler ve belirli bir noktaya 5 kilometreden daha yakın olan sonuçlar (enlem ve boylam koordinatları olarak belirtilir) döndürülür. **$count** ekleyerek mesafeyi veya koordinatları değiştirdiğinizde döndürülen sonuç sayısını görebilirsiniz.

Arama uygulamanız "yakınımda bul" özelliği içeriyorsa ya da harita navigasyonu kullanıyorsa jeo-uzamsal arama kullanışlıdır. Ancak tam metin arama değildir. Ada göre şehir veya ülke/bölge aramak için Kullanıcı gereksinimleriniz varsa, koordinatlara ek olarak şehir veya ülke/bölge adlarını içeren alanları ekleyin.

## <a name="takeaways"></a>Paketler

Bu öğretici, Azure Bilişsel Arama Azure portal kullanılarak hızlı bir giriş sağlamıştır.

**Verileri içeri aktar** sihirbazını kullanarak arama dizini oluşturmayı öğrendiniz. [Dizin oluşturucuların](search-indexer-overview.md) yanı sıra [yayımlanmış dizinler için desteklenen değişiklikler](/rest/api/searchservice/update-index) gibi dizin tasarımıyla ilgili temel iş akışı hakkında bilgi edindiniz.

Azure portalda **Arama gezginini** kullanarak filtreler, sonuç vurgulama, belirsiz arama ve coğrafi arama gibi önemli özelliklerin gösterildiği örneklerle temel sorgu söz dizimini öğrendiniz.

Ayrıca, portalda Dizin, Dizin oluşturucular ve veri kaynaklarını bulmayı de öğrendiniz. Gelecekte yeni verilerle karşılaştığınızda portalı kullanarak tanımlarını veya alan koleksiyonlarını hızlı bir şekilde denetleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bir tarayıcıda çalışan kullanıma yönelik bir Web uygulaması oluşturmak için Portal Sihirbazı 'nı kullanın. Bu Sihirbazı, az önce oluşturduğunuz küçük dizinde deneyebilir veya daha zengin bir arama deneyimi için yerleşik örnek veri kümelerinden birini kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Portalda demo uygulaması oluşturma](search-create-app-portal.md)