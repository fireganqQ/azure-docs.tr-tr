---
title: Unity örneğini Masaüstüne dağıtma
description: Bir masaüstü BILGISAYAR üzerinde Unity örneğinin nasıl alınacağını gösteren hızlı başlangıç
author: christophermanthei
ms.author: chmant
ms.date: 03/20/2020
ms.topic: quickstart
ms.openlocfilehash: 26531e80292a53400671ac3507b47371c1fbbcad
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "88796825"
---
# <a name="quickstart-deploy-unity-sample-to-desktop"></a>Hızlı başlangıç: Unity örneğini masaüstüne dağıtma

Bu hızlı başlangıçta, bir masaüstü BILGISAYARA Unity için hızlı başlangıç örneği uygulamasının nasıl dağıtılacağı ve çalıştırılacağı ele alınmaktadır.

Bu hızlı başlangıçta şunları nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
>
>* Masaüstü için hızlı başlangıç örnek uygulamasını oluşturma
>* Örneği bir BILGISAYARA dağıtma
>* Örneği bir bılgısayarda çalıştırın

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta örnek projeyi hızlı başlangıç konumundan dağıtacağız [: Unity ile model işleme](render-model.md).

Kimlik bilgilerinizin sahneye doğru şekilde kaydedildiğinden emin olun ve Unity Düzenleyicisi içinden bir oturuma bağlanabilirsiniz.

## <a name="disable-virtual-reality-support"></a>Sanal gerçeklik desteğini devre dışı bırak

Şu anda masaüstünde yalnızca düz masaüstü uygulamaları desteklenmektedir. bu nedenle, VR desteği devre dışı bırakılmalıdır.

1. *> proje ayarlarını Düzenle 'yi aç...*
1. Sol taraftaki **oynatıcı** ' yı seçin.
1. **Evrensel Windows platformu ayarları** sekmesini seçin.
1. **XR ayarlarını**genişletin.
1. **Desteklenen sanal gerçeklik**devre dışı. \
    ![Oynatıcı ayarları](./media/unity-disable-xr.png)
1. *XR ayarları*' nın üzerinde **Yayımlama ayarları**' nı genişletin.
1. **Desteklenen cihaz ailelerinde** **Masaüstü 'nün** işaretli olduğundan emin olun.

## <a name="build-the-sample-project"></a>Örnek projeyi oluşturma

1. *Dosya > derleme ayarları*'nı açın.
1. *Platformu* **Evrensel Windows platformu** olarak değiştirin (**PC standalone** de desteklenir ancak burada kullanılmaz, bkz. [Platform sınırlamaları](../reference/limits.md#platform-limitations)).
1. *Hedef cihazı* **bilgisayar**olarak ayarlayın.
1. *Mimariyi* **x86**olarak ayarlayın.
1. *Yapı türünü* **D3D projesi**olarak ayarla. \
  ![Derleme ayarları](./media/unity-build-settings-pc.png)
1. **Platforma geç**' i seçin.
1. **Derleme** (veya ' derleme ve çalıştırma ') tuşlarına basıldığında çözümün depolanması gereken bir klasör seçmeniz istenir.
1. Visual Studio ile oluşturulan **hızlı başlangıç. sln** 'yi açın.
1. Yapılandırmayı **Release** ve **x86**olarak değiştirin.
1. Hata ayıklayıcı modunu **yerel makineye**değiştirin. \
  ![Çözüm yapılandırması](./media/unity-deploy-config-pc.png)
1. Çözümü derleyin.

## <a name="launch-the-sample-project"></a>Örnek projeyi başlatma

Visual Studio 'da hata ayıklayıcıyı başlatın (F5). Uygulamayı otomatik olarak BILGISAYARA dağıtır.

Örnek uygulama, yeni bir oturum başlatıp başlatmalıdır. Bir süre sonra, oturum hazırlanıyor ve uzaktan işlenen model sizin sizin için görünür olacaktır.
Daha sonra örneği ikinci kez başlatmak istiyorsanız, Şimdi Başlat menüsünden de bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki hızlı başlangıçta, özel bir modeli dönüştürmeye göz atacağız.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: bir modeli işleme için dönüştürme](convert-model.md)
