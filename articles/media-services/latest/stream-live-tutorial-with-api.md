---
Başlık: Media Services v3 ile canlı akış: Azure Media Services Açıklama: Azure Media Services v3 ile canlı akışı yapmayı öğrenin.
Hizmetler: Media-Services belgetationcenter: ' ' Yazar: ınridatmicrosoft Manager: femila Düzenleyicisi: ' '

MS. Service: Media-Services MS. Workload: Media ms.tgt_pltfrm: na MS. devlang: na MS. Topic: öğretici ms. Custom: "MVC, DevX-Track-CSharp" MS. Date: 06/13/2019 MS. Author: inhenkel

---

# <a name="tutorial-stream-live-with-media-services"></a>Öğretici: Media Services ile canlı akış

> [!NOTE]
> Öğretici [.NET SDK](/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet) örnekleri kullanıyor olsa da, genel adımlar [REST API](/rest/api/media/liveevents), [CLI](/cli/azure/ams/live-event?view=azure-cli-latest)veya desteklenen diğer [SDK](media-services-apis-overview.md#sdks)'lar için aynıdır.

Azure Media Services, canlı [Olaylar](/rest/api/media/liveevents) canlı akış içeriğini işlemekten sorumludur. Canlı bir olay, daha sonra canlı bir kodlayıcı için sağladığınız bir giriş uç noktası (alma URL 'SI) sağlar. Canlı olay, canlı kodlayıcıdan canlı giriş akışları alır ve bir veya daha fazla [akış uç noktası](/rest/api/media/streamingendpoints)aracılığıyla akış için kullanılabilir hale getirir. Canlı olaylar ayrıca, daha fazla işlem ve teslim yapmadan önce akışınızı önizlemek ve doğrulamak için kullandığınız bir önizleme uç noktası (önizleme URL 'SI) sağlar. Bu öğretici, **geçiş** türü bir canlı olay oluşturmak için .NET Core kullanmayı göstermektedir.

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Konusunda açıklanan örnek uygulamayı indirin.
> * Canlı akış gerçekleştiren kodu inceleyin.
> * [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) ile olayı izleyin [https://ampdemo.azureedge.net](https://ampdemo.azureedge.net) .
> * Kaynakları temizleyin.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir:

- Visual Studio Code veya Visual Studio 'Yu yükler.
- [Media Services hesabı oluşturun](./create-account-howto.md).<br/>Kaynak grubu adı ve Media Services hesap adı için kullandığınız değerleri anımsadığınızdan emin olun.
- [Azure CLI Ile Access Azure Media Services API 'sindeki](./access-api-howto.md) adımları izleyin ve kimlik bilgilerini kaydedin. API 'ye erişmek için bunları kullanmanız gerekir.
- Bir olayı yayımlamak için kullanılan bir kamera veya bir cihaz (dizüstü bilgisayar gibi).
- Kameradan gelen sinyalleri Media Services canlı akış hizmetine gönderilen akışlara dönüştüren, şirket içi bir Live Encoder, bkz. [Önerilen şirket içi canlı kodlayıcılar](recommended-on-premises-live-encoders.md). Akışın **RTMP** veya **Kesintisiz Akış** biçiminde olması gerekir.

> [!TIP]
> Devam etmeden önce [Media Services v3 ile canlı akış](live-streaming-overview.md) konusunu gözden geçirmeyi unutmayın. 

## <a name="download-and-configure-the-sample"></a>Örneği indirin ve yapılandırın

Aşağıdaki komutu kullanarak, akış .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

Canlı akış örneği [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp) klasöründe bulunabilir.

İndirilen projenizde [appsettings.js](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/appsettings.json) açın. Değerleri, [API 'lere erişirken](./access-api-howto.md)aldığınız kimlik bilgileriyle değiştirin.

> [!IMPORTANT]
> Bu örnek her kaynak için benzersiz bir sonek kullanır. Hata ayıklamayı iptal ederseniz veya uygulamayı üzerinden çalıştırmadan sonlandırdıysanız, hesabınızda birden çok canlı olayla karşılaşırsınız. <br/>Çalışan canlı olayları durdurduğunuzdan emin olun. Aksi takdirde **faturalandırılırsınız**!

## <a name="examine-the-code-that-performs-live-streaming"></a>Canlı akışı gerçekleştiren kodu inceleme

Bu bölümde, *MediaV3LiveApp* projesinin [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) dosyasında tanımlı işlevler incelenmektedir.

Örnek, her kaynak için benzersiz bir sonek oluşturur, böylece örnek birden çok kez temizlemeden çalıştırırsanız ad çarpışmaları olmaz.

> [!IMPORTANT]
> Bu örnek her kaynak için benzersiz bir sonek kullanır. Hata ayıklamayı iptal ederseniz veya uygulamayı üzerinden çalıştırmadan sonlandırdıysanız, hesabınızda birden çok canlı olayla karşılaşırsınız. <br/>
> Çalışan canlı olayları durdurduğunuzdan emin olun. Aksi takdirde **faturalandırılırsınız**!

### <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız kodda, **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alarak ServiceClientCredentials nesnesi oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Canlı etkinlik oluşturma

Bu bölümde, bir **geçişli** canlı etkinlik türünün nasıl oluşturulduğu gösterilir (LiveEventEncodingType None olarak ayarlanır). Kullanılabilir canlı etkinlik türleri hakkında daha fazla bilgi için bkz. [canlı olay türleri](live-events-outputs-concept.md#live-event-types). 
 
Canlı olay oluştururken belirtmek isteyebileceğiniz bazı şeyler şunlardır:

* Media Services konumu.
* Canlı olay için Akış Protokolü (Şu anda, RTMP ve Kesintisiz Akış protokolleri desteklenir).<br/>Canlı olay veya ilişkili canlı çıktıları çalışırken protokol seçeneğini değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa, her akış protokolü için ayrı canlı etkinlik oluşturun.  
* Alma ve önizleme için IP kısıtlamaları. Bu canlı olaya bir video almasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.<br/>Hiçbir IP adresi belirtilmemişse ve kural tanımı yoksa, hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.<br/>IP adresleri aşağıdaki biçimlerden birinde olmalıdır: dört sayı veya CıDR adres aralığı olan IPv4 adresi.
* Olayı oluştururken, başlatmayı belirtebilirsiniz. <br/>Autostart değeri true olarak ayarlandığında, canlı olay oluşturulduktan sonra başlatılır. Bu, canlı olay çalışmaya başladığı anda faturalandırma başladığı anlamına gelir. Daha fazla faturalandırmayı durdurmak için canlı olay kaynağında durdurmayı açıkça çağırmanız gerekir. Daha fazla bilgi için bkz. [canlı olay durumları ve faturalandırma](live-event-states-billing.md).
* Tahmine dayalı bir URL 'nin tahmin alınması için, "Vanity" modunu ayarlayın. Ayrıntılı bilgi için bkz. [canlı olay alma URL 'leri](live-events-outputs-concept.md#live-event-ingest-urls).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>Alma URL’leri alma

Canlı olay oluşturulduktan sonra, gerçek zamanlı kodlayıcıya sağlayacağınız içe alma URL 'Leri edinebilirsiniz. Kodlayıcı bu URL'leri canlı akış girişi için kullanır.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>Önizleme URL'sini alma

Kodlayıcıdan gelen girişin gerçekten alındığını önizlemek ve doğrulamak için PreviewEndpoint kullanın.

> [!IMPORTANT]
> Devam etmeden önce videonun önizleme URL 'sine akmasını sağlayın.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-live-events-and-live-outputs"></a>Canlı etkinlikler ve canlı çıktılar oluşturma ve yönetme

Akışın canlı olayına akışını tamamladıktan sonra bir varlık, canlı çıkış ve akış Bulucu oluşturarak akış olayını başlatabilirsiniz. Bu olay, akışı arşivler ve akışın Akış Uç Noktası aracılığıyla izleyiciler tarafından kullanılabilmesini sağlar.

#### <a name="create-an-asset"></a>Asset oluşturma

Canlı çıktının kullanması için bir varlık oluşturun.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

#### <a name="create-a-live-output"></a>Canlı çıkış oluştur

Canlı çıktılar oluşturma sırasında başlar ve silindiğinde durdurulur. Canlı çıktıyı sildiğinizde, ilgili varlık ve varlığın içeriğini silmezsiniz.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaming-locator"></a>Akış Bulucu oluşturma

> [!NOTE]
> Media Services hesabınız oluşturulduğunda hesabınıza **durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçeriğinizi akışa almak ve [dinamik paketleme](dynamic-packaging-overview.md) ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **çalışıyor** durumda olması gerekir.

Canlı çıkış varlığını bir akış Bulucuyu kullanarak yayımladığınızda, canlı olay (DVR pencere uzunluğuna kadar), akış bulucunun bitiş tarihi veya silme işlemi, hangisi önce geldiği sürece görüntülenebilir olmaya devam edecektir.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateStreamingLocator)]

```csharp

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Olayları akışa alma işlemini yaptıysanız ve daha önce sağlanan kaynakları temizlemek istiyorsanız aşağıdaki yordamı izleyin:

* Kodlayıcıdan akışı göndermeyi durdurun.
* Canlı etkinliği durdurun. Canlı etkinlik durdurulduktan sonra ücret ödemez. Tekrar başlatmanız gerektiğinde, aynı alma URL’sine sahip olacağından kodlayıcıyı yeniden yapılandırmanız gerekmez.
* Canlı olayınızın arşivini isteğe bağlı bir akış olarak sunmaya devam etmek istemiyorsanız Akış Uç Noktanızı durdurabilirsiniz. Canlı etkinlik durdurulmuş durumdaysa, hiçbir ücret ödemez.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

## <a name="watch-the-event"></a>Olayı izleme

Olayı izlemek için, akış Bulucu oluşturma bölümünde açıklanan kodu çalıştırdığınızda aldığınız akış URL 'sini kopyalayın. Seçtiğiniz bir medya oynatıcı kullanabilirsiniz. [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) , akışınızı test etmek için kullanılabilir https://ampdemo.azureedge.net .

Canlı olay durdurulduğunda olayları otomatik olarak isteğe bağlı içeriğe dönüştürür. Olayı durdurup sildikten sonra bile, kullanıcılar, varlığı silmemiş olduğu sürece arşivlenmiş içeriğinizi isteğe bağlı bir video olarak akışa alabilir. Bir varlık, bir olay tarafından kullanılıyorsa silinemez; önce olayın silinmesi gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.

Aşağıdaki CLı komutunu yürütün:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Canlı olaydan tasarruf eden faturalandırma maliyetlerini çalıştırmaya çıkılıyor. Göz önünde bulundurulması, proje/program kilitlenirse veya herhangi bir nedenden dolayı kapatılmışsa, bu durum bir faturalandırma durumunda çalışan canlı olayı bırakabilir.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Dosyaları akışla aktarma](stream-files-tutorial-with-api.md)
 
