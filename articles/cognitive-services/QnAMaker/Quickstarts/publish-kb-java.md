---
title: 'Hızlı başlangıç: yayımlama bilgi tabanı, REST, Java-Soru-Cevap Oluşturma'
description: Bu Java REST tabanlı hızlı başlangıç bilgi tabanınızı yayımlar ve uygulamanızda veya sohbet bot 'ta çağrılabilecek bir uç nokta oluşturur.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27, devx-track-java
ms.topic: how-to
ms.openlocfilehash: 8e2e902e0563e0f4ae8c0c3d0dc795a8260c62db
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96351172"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-java"></a>Hızlı başlangıç: Java kullanarak Soru-Cevap Oluşturma’da bilgi bankası yayımlama

Bu REST tabanlı hızlı başlangıç, bilgi bankanızı (KB) programlı bir şekilde yayımlama konusunda size yol gösterir. Yayımlama, bilgi tabanının en son sürümünü adanmış bir Azure Bilişsel Arama dizinine gönderir ve uygulamanızda veya sohbet bot 'ta çağrılabilecek bir uç nokta oluşturur.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Publish](/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Önkoşullar

* [JDK SE](/azure/developer/java/fundamentals/java-jdk-long-term-support)  (Java Geliştirme Seti, Standart Sürüm)
* Bu örnek, HTTP bileşenlerinden Apache [http istemcisini](https://hc.apache.org/httpcomponents-client-ga/) kullanır. Projenize aşağıdaki Apache HTTP istemci kitaplıklarını eklemeniz gerekir:
    * HttpClient-4.5.3. jar
    * httpcore-4.4.6. jar
    * Commons-logging-1.2. jar
* [Visual Studio Code](https://code.visualstudio.com/)
* Bir [soru-cevap oluşturma hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı ve uç noktanızı (kaynak adını da içerir) almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin.
* `kbid`Sorgu dizesi PARAMETRESINDEKI URL 'de aşağıda gösterildiği gibi soru-cevap oluşturma Bilgi Bankası (KB) kimliği bulundu.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-csharp.md).

> [!NOTE]
> Tam çözüm dosyası (ler) [ **Azure-Samples/bilişsel hizmetler-qnamaker-Java** GitHub deposundan](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/tree/master/documentation-samples/quickstarts/publish-knowledge-base)alınabilir.

## <a name="create-a-java-file"></a>Java dosyası oluşturma

VSCode 'u açın ve adlı yeni bir dosya oluşturun `PublishKB.java` .

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Sınıfının en üstünde `PublishKB.java` ,, gerekli bağımlılıkları projeye eklemek için aşağıdaki satırları ekleyin:

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/PublishKB.java" id="dependencies":::

## <a name="create-publishkb-class-with-main-method"></a>Main yöntemiyle PublishKB sınıfı oluşturma

Bağımlılıklardan sonra aşağıdaki sınıfı ekleyin:

```Go
public class PublishKB {

    public static void main(String[] args)
    {
    }
}
```

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

**Main** yönteminde soru-cevap oluşturma erişmek için gereken sabitleri ekleyin. Değerleri kendi değerlerinizle değiştirin.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/PublishKB.java" id="constants":::

## <a name="add-post-request-to-publish-knowledge-base"></a>Bilgi Bankası yayınlamak için POST isteği ekleyin

Gerekli sabitlerden sonra, bir Bilgi Bankası yayımlamak ve yanıtı almak için Soru-Cevap Oluşturma API'si HTTPS isteği yapan aşağıdaki kodu ekleyin:

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/PublishKB.java" id="post":::

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. Kod 204 yanıtları için içerik ekler.

Diğer yanıtlarda döndürülen yanıt değiştirilmez.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Komut satırından programı derleyin ve çalıştırın. İstek otomatik olarak Soru-Cevap Oluşturma API'si gönderilir, ardından konsol penceresine yazdırılır.

1. Dosyayı oluşturun:

    ```bash
    javac -cp "lib/*" PublishKB.java
    ```

1. Dosyayı çalıştırın:

    ```bash
    java -cp ".;lib/*" PublishKB
    ```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi Bankası yayımlandıktan sonra, [bir yanıt oluşturmak için uç nokta URL 'sine](./get-answer-from-knowledge-base-java.md)ihtiyacınız vardır.

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](/rest/api/cognitiveservices/qnamaker4.0/knowledgebase)