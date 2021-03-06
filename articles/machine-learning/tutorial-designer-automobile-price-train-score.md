---
title: 'Öğretici: tasarımcı ile otomobil fiyatlarını tahmin etme'
titleSuffix: Azure Machine Learning
description: Doğrusal regresyon kullanarak otomobil fiyatlarını tahmin etmek için makine öğrenimi modelini eğitme. Bu öğretici, iki bölümden oluşan bir serinin birinci bölümüdür.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 09/28/2020
ms.custom: designer
ms.openlocfilehash: 46098b2b7ebbf219a3c17b4fa40e1fd52344f683
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98879805"
---
# <a name="tutorial-predict-automobile-price-with-the-designer"></a>Öğretici: Tasarımcıyla otomobil fiyatını tahmin etme


Bu iki bölümden oluşan öğreticide, her bir otomobil fiyatını tahmin eden bir makine öğrenimi modelini eğitme ve dağıtma için Azure Machine Learning tasarımcısını nasıl kullanacağınızı öğreneceksiniz. Tasarımcı, tek bir kod satırı olmadan makine öğrenimi modelleri oluşturmanıza olanak sağlayan bir sürükle ve bırak aracıdır.

Öğreticinin birinci bölümünde şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir işlem hattı oluşturun.
> * Verileri içeri aktarın.
> * Verileri hazırlayın.
> * Makine öğrenimi modelini eğitme.
> * Machine Learning modelini değerlendirin.

Öğreticinin [ikinci bölümünde](tutorial-designer-automobile-price-deploy.md) , size gönderilen teknik belirtimlere göre herhangi bir arabasının fiyatını tahmin etmek için modelinizi gerçek zamanlı bir zaman aşımı uç noktası olarak dağıtırsınız. 

> [!NOTE]
>Bu öğreticinin tamamlanmış sürümü örnek bir işlem hattı olarak sunulmaktadır.
>
>Bulmak için çalışma alanınızdaki tasarımcıya gidin. Yeni işlem **hattı** bölümünde **Örnek 1-gerileme: otomobil fiyat tahmini (temel)** öğesini seçin.

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="create-a-new-pipeline"></a>Yeni işlem hattı oluşturma

Azure Machine Learning işlem hatları, birden çok makine öğrenimi ve veri işleme adımını tek bir kaynakta düzenler. İşlem hatları, projeler ve kullanıcılar arasında karmaşık makine öğrenimi iş akışlarını düzenlemenize, yönetmenize ve yeniden kullanmanıza olanak sağlar.

Azure Machine Learning bir işlem hattı oluşturmak için bir Azure Machine Learning çalışma alanına ihtiyacınız vardır. Bu bölümde, bu kaynakların her ikisini de oluşturmayı öğreneceksiniz.

### <a name="create-a-new-workspace"></a>Yeni çalışma alanı oluşturma

Tasarımcıyı kullanmak için bir Azure Machine Learning çalışma alanı gerekir. Çalışma alanı Azure Machine Learning için en üst düzey kaynaktır, Azure Machine Learning içinde oluşturduğunuz tüm yapıtlarla çalışmak için merkezi bir yer sağlar. Çalışma alanı oluşturma yönergesi için bkz. [Azure Machine Learning çalışma alanları oluşturma ve yönetme](how-to-manage-workspace.md).

> [!NOTE]
> Çalışma alanınız bir sanal ağ kullanıyorsa, tasarımcıyı kullanmak için kullanmanız gereken ek yapılandırma adımları vardır. Daha fazla bilgi için bkz. [Azure sanal ağında Azure Machine Learning Studio 'Yu kullanma](how-to-enable-studio-virtual-network.md)

### <a name="create-the-pipeline"></a>İşlem hattını oluşturma

1. <a href="https://ml.azure.com?tabs=jre" target="_blank">Ml.Azure.com</a>'de oturum açın ve birlikte çalışmak istediğiniz çalışma alanını seçin.

1. **Tasarımcı**' yı seçin.

    ![Tasarımcıya nasıl erişediğinin gösterildiği görsel çalışma alanının ekran görüntüsü](./media/tutorial-designer-automobile-price-train-score/launch-designer.png)

1. Kullanımı **kolay önceden oluşturulmuş modüller**' i seçin.

1. Tuvalin üst kısmında, varsayılan işlem hattı adı ardışık düzen- **oluşturma**' yı seçin. Bunu, *otomobil fiyat tahmini* olarak yeniden adlandırın. Adın benzersiz olması gerekmez.

## <a name="set-the-default-compute-target"></a>Varsayılan işlem hedefini ayarla

İşlem hattı, çalışma alanınıza eklenmiş bir işlem kaynağı olan işlem hedefi üzerinde çalışır. Bir işlem hedefi oluşturduktan sonra, gelecekteki çalıştırmalar için onu yeniden kullanabilirsiniz.

Her modülün varsayılan olarak aynı işlem hedefini kullanmasını söyleyecek tüm işlem hattı için **varsayılan bir işlem hedefi** ayarlayabilirsiniz. Ancak, işlem hedeflerini modül başına temelinde belirtebilirsiniz.

1. İşlem hattı adının yanında,  ![ tuval simgesinin en üstündeki dişli simgesi ekran görüntüsünü seçerek ](./media/tutorial-designer-automobile-price-train-score/gear-icon.png) **Ayarlar** bölmesini açın.

1. Tuvalin sağındaki **Ayarlar** bölmesinde, **işlem hedefini seç**' i seçin.

    Zaten kullanılabilir bir işlem hedefi varsa, bu işlem hattını çalıştırmak için seçin.

    > [!NOTE]
    > Tasarımcı yalnızca Azure Machine Learning Işlem üzerinde eğitim denemeleri çalıştırabilir, ancak diğer işlem hedefleri gösterilmez.

1. İşlem kaynağı için bir ad girin.

1. **Kaydet**’i seçin.

    > [!NOTE]
    > İşlem kaynağının oluşturulması yaklaşık beş dakika sürer. Kaynak oluşturulduktan sonra yeniden kullanabilir ve gelecekteki çalıştırmalar için bu bekleme süresini atlayabilirsiniz.
    >
    > Maliyet tasarrufu için boşta kaldığında işlem kaynağı sıfır düğümlere otomatik olarak ölçeklendirilir. Bir gecikmeden sonra yeniden kullandığınızda, yeniden ölçeklendirilirken yaklaşık beş dakikalık bekleme süresi yaşayabilirsiniz.

## <a name="import-data"></a>Veri içeri aktarma

Üzerinde denemeler yapmanız için tasarımcıya dahil edilen birkaç örnek veri kümesi vardır. Bu öğretici için, **otomobil fiyat verileri (ham)** kullanın. 

1. Ardışık düzen tuvalinin sol tarafında bir veri kümesi ve modül paleti bulunur. Kullanılabilir örnek veri kümelerini görüntülemek için **örnek veri kümeleri** ' ni seçin.

1. Veri kümesi **otomobil fiyat verileri (ham)** öğesini seçin ve tuvale sürükleyin.

   ![Verileri tuvale sürükleyin](./media/tutorial-designer-automobile-price-train-score/drag-data.gif)

### <a name="visualize-the-data"></a>Verileri görselleştirme

Kullanacağınız veri kümesini anlamak için verileri görselleştirebilirsiniz.

1. **Otomobil fiyat verilerine (ham)** sağ tıklayın ve   >  **veri kümesi çıkışını** Görselleştir ' i seçin.

1. Her biri hakkındaki bilgileri görüntülemek için veri penceresinde farklı sütunları seçin.

    Her satır bir otomobil ve her bir otomobil ile ilişkili değişkenler sütun olarak görünür. Bu veri kümesinde 205 satır ve 26 sütun vardır.

## <a name="prepare-data"></a>Verileri hazırlama

Veri kümeleri, analiz etmeden önce genellikle bazı ön işleme gerektirir. Veri kümesini denetlediğinizde bazı eksik değerleri fark etmiş olabilirsiniz. Bu eksik değerler, modelin verileri doğru çözümleyebilmesi için temizlenmelidir.

### <a name="remove-a-column"></a>Sütun kaldırma

Bir modeli eğitedığınızda, eksik olan veriler hakkında bir şey yapmanız gerekir. Bu veri kümesinde, **normalleştirilmiş kayıplar** sütununda çok sayıda değer yok, bu nedenle bu sütunu modelden tamamen dışarıda bırakacaksınız.

1. Tuvalin solundaki modül paletinde, **veri dönüştürme** bölümünü genişletin ve veri **kümesindeki sütunları seç** modülünde bulun.

1. **Veri kümesindeki sütunları seçme** modülüne tuval üzerine sürükleyin. Modülün veri kümesi modülünün altına bırakın.

1. **Otomobil fiyat verileri (ham)** veri kümesini **veri kümesindeki sütunları seçme** modülüne bağlayın. Veri kümesinin çıkış bağlantı noktasından, tuvaldeki veri kümesinin en altında bulunan küçük daire, modülün en üstünde bulunan küçük bir daire olan **veri kümesindeki sütun seçme** giriş bağlantı noktasına sürükleyin.

    > [!TIP]
    > Bir modülün çıkış bağlantı noktasını başka bir giriş bağlantı noktasına bağladığınızda işlem hattınızla veri akışı oluşturursunuz.
    >

    ![Modülleri bağlama](./media/tutorial-designer-automobile-price-train-score/connect-modules.gif)

1. **Veri kümesi modülünde sütunları seç '** i seçin.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde **sütunu Düzenle**' yi seçin.

1. **Ekle**' nin yanındaki **sütun adları** açılan ' i genişletin ve **tüm sütunlar**' ı seçin.

1. **+** Yeni bir kural eklemek için öğesini seçin.

1. Aşağı açılan menülerden **Dışla** ve **sütun adlarını** seçin.
    
1. Metin kutusuna *normalleştirilmiş zararlar* girin.

1. Sütun seçiciyi kapatmak için sağ alt köşedeki **Kaydet** ' i seçin.

    ![Sütun dışlama](./media/tutorial-designer-automobile-price-train-score/exclude-column.png)

1. **Veri kümesi modülünde sütunları seç '** i seçin. 

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, **Açıklama** metin kutusunu seçin ve *normalleştirilmiş zararları hariç tut*' u girin.

    İşlem hattınızı düzenlemenize yardımcı olması için grafik üzerinde açıklamalar görüntülenecektir.

### <a name="clean-missing-data"></a>Eksik verileri temizle

**Normalleştirilmiş zararlar** sütununu kaldırdıktan sonra Veri kümeniz hala eksik değerler içeriyor. **Eksik verileri temizle** modülünü kullanarak kalan eksik verileri kaldırabilirsiniz.

> [!TIP]
> Giriş verilerinden eksik değerleri Temizleme, tasarımcıda birçok modülün kullanılmasına yönelik bir önkoşuldur.

1. Tuvalin solundaki modül paletinde, bölüm **veri dönüşümü**' ni genişletin ve **eksik veri modülünü temizle** ' yi bulun.

1. **Eksik verileri temizle** modülünü ardışık düzen tuvaline sürükleyin. **Veri kümesi modülündeki sütunları seçme** modülüne bağlayın. 

1. **Eksik verileri temizle** modülünü seçin.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde **sütunu Düzenle**' yi seçin.

1. Görüntülenen **sütunlar** penceresinde, **Ekle**' nin yanındaki açılan menüyü genişletin. Seç, **tüm sütunlar**

1. **Kaydet**’i seçin

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, **temizleme modu**' nun altındaki **tüm satırı Kaldır** ' ı seçin.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, **Açıklama** kutusunu seçin ve *eksik değer satırlarını kaldır*' ı girin. 

    İşlem hatlarınız şuna benzer şekilde görünmelidir:

    :::image type="content" source="./media/tutorial-designer-automobile-price-train-score/pipeline-clean.png"alt-text="Select-Column":::

## <a name="train-a-machine-learning-model"></a>Makine öğrenmesi modeli eğitme

Artık verileri işlemek için modüller olduğuna göre, eğitim modüllerini ayarlayabilirsiniz.

Bir sayı olan fiyatı tahmin etmek istediğiniz için, regresyon algoritmasını kullanabilirsiniz. Bu örnekte, doğrusal regresyon modeli kullanırsınız.

### <a name="split-the-data"></a>Verileri bölme

Verileri bölmek makine öğreniminde ortak bir görevdir. Verilerinizi iki ayrı veri kümesine bölecektir. Bir veri kümesi modeli eğitecektir ve diğeri de modelin ne kadar iyi gerçekleştirildiğini test eder.

1. Modül paletinde, **veri dönüştürme** bölümünü genişletin ve **bölünmüş veri** modülünü bulun.

1. **Verileri Böl** modülünü ardışık düzen tuvaline sürükleyin.

1. **Eksik verileri temizleme** modülünün sol bağlantı noktasını **bölünmüş veri** modülüne bağlayın.

    > [!IMPORTANT]
    > **Eksik verileri temizlemek** için sol çıkış bağlantı noktalarında **verileri ayırmak** üzere bağlandığından emin olun. Sol bağlantı noktası, temizlenen verileri içerir. Sağ bağlantı noktası discarted verilerini içerir.

1. **Bölünmüş veri** modülünü seçin.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, **ilk çıktı veri kümesindeki satır kesirini** 0,7 olarak ayarlayın.

    Bu seçenek, modeli eğitmek için verilerin yüzde 70 ' unu ve test için yüzde 30 ' unu böler. %70 veri kümesine sol çıkış bağlantı noktası üzerinden erişilebilecektir. Kalan veriler doğru çıkış bağlantı noktası üzerinden sunulacaktır.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, **Açıklama** kutusunu seçin ve *veri kümesini eğitim kümesi (0,7) ve test kümesi (0,3) olarak Böl*' i girin.

### <a name="train-the-model"></a>Modeli eğitme

Fiyatı içeren bir veri kümesi vererek modeli eğitme. Algoritma, eğitim verileri tarafından sunulan özellikler ve fiyat arasındaki ilişkiyi açıklayan bir model oluşturur.

1. Modül paletinde **Machine Learning algoritmaları**' nı genişletin.
    
    Bu seçenek, öğrenme algoritmalarını başlatmak için kullanabileceğiniz birkaç modül kategorisini görüntüler.

1. **Regresyon**  >  **Doğrusal regresyon**' i seçin ve işlem hattı tuvaline sürükleyin.

1. Modül paletinde, bölüm **modülü eğitimi**' ni genişletin ve **model eğitme** modülünü tuvale sürükleyin.

1. **Doğrusal regresyon** modülünün çıkışını **eğitme modeli** modülünün sol girişine bağlayın.

1. **Veri ayırma** modülünün eğitim verileri çıkışını (sol bağlantı noktası), **eğitme modeli** modülünün sağ girişine bağlayın.
    
    > [!IMPORTANT]
    > **Bölünmüş verilerin** sol çıkış bağlantı noktalarının **modeli eğmek** için bağlandığından emin olun. Sol bağlantı noktası eğitim kümesini içerir. Sağ bağlantı noktası, test kümesini içerir.

    :::image type="content" source="./media/tutorial-designer-automobile-price-train-score/pipeline-train-model.png"alt-text="Eğitim modeli modülünün doğru yapılandırmasını gösteren ekran görüntüsü. Doğrusal regresyon modülü, model eğitimi modülünün sol bağlantı noktasına bağlanır ve bölünmüş veri modülü, tren modelinin sağ bağlantı noktasına bağlanır.":::

1. **Model eğitme** modülünü seçin.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, **sütun seçiciyi Düzenle** ' yi seçin.

1. **Etiket sütunu** iletişim kutusunda, açılan menüyü genişletin ve **sütun adları**' nı seçin. 

1. Metin kutusuna modelinizin tahmin edilecek değeri belirtmek için *Fiyat* girin.

    >[!IMPORTANT]
    > Sütun adını tam olarak girdiğinizden emin olun. **Fiyatı** büyük harfle kaldırmayın. 

    İşlem hatlarınız şöyle görünmelidir:

    :::image type="content" source="./media/tutorial-designer-automobile-price-train-score/pipeline-train-graph.png"alt-text="Eğitim modeli modülü eklendikten sonra işlem hattının doğru yapılandırmasını gösteren ekran görüntüsü.":::

### <a name="add-the-score-model-module"></a>Puan modeli modülünü ekleme

Modelinize verilerin yüzde 70 'ini kullanarak eğdikten sonra, modelinizin ne kadar iyi olduğunu görmek için bu değeri, diğer 30 ' a puan almak üzere kullanabilirsiniz.

1. **Puan modeli** modülünü bulmak için arama kutusuna *puan modeli* girin. Modülü işlem hattı tuvaline sürükleyin. 

1. **Modeli Eğitme** modülünün çıkışını **Model Puanlama** modülünün sol giriş bağlantı noktasına bağlayın. **Verileri Bölme** modülünün test verileri çıkışını (sağ bağlantı noktası) **Model Puanlama** modülünün sağ giriş bağlantı noktasına bağlayın.

### <a name="add-the-evaluate-model-module"></a>Modeli değerlendir modülünü ekleme

Modelinizin test veri kümesini ne kadar iyi puanlaleceğini değerlendirmek için **modeli değerlendir** modülünü kullanın.

1. **Modeli değerlendir** modülünü bulmak için arama kutusuna *değerlendir* yazın. Modülü işlem hattı tuvaline sürükleyin. 

1. **Puan modeli** modülünün çıkışını **modeli değerlendir** modülünün sol girişine bağlayın. 

    Son işlem hattı şuna benzer görünmelidir:

    :::image type="content" source="./media/tutorial-designer-automobile-price-train-score/pipeline-final-graph.png"alt-text="İşlem hattının doğru yapılandırmasını gösteren ekran görüntüsü.":::

## <a name="submit-the-pipeline"></a>İşlem hattını gönderme

İşlem hattınızda tüm kurulum olduğuna göre, makine öğrenimi modelinizi eğitebilmeniz için bir işlem hattı çalıştırması gönderebilirsiniz. Geliştirme sırasında işlem hattınızdaki değişiklikleri gözden geçirmek için kullanılabilecek herhangi bir noktada geçerli bir işlem hattı çalıştırması gönderebilirsiniz.

1. Tuvalin üst kısmında **Gönder**' i seçin.

1. İşlem **hattı çalıştırmasını ayarla** iletişim kutusunda **Yeni oluştur**' u seçin.

    > [!NOTE]
    > Denemeleri Group benzer işlem hattı birlikte çalışır. Birden çok kez işlem hattı çalıştırırsanız, art arda çalıştırmalar için aynı denemeyi seçebilirsiniz.

    1. **Yeni deneme adı** için **öğretici-carfiyatlar** girin.

    1. **Gönder**’i seçin.
    
    Çalışma durumunu ve ayrıntılarını tuvalin sağ üst kısmında görüntüleyebilirsiniz.
    
    İlk çalıştırıldıysanız, işlem hattının çalışmasının tamamlanması 20 dakikaya kadar sürebilir. Varsayılan işlem ayarlarının minimum düğüm boyutu 0 ' dır ve bu, tasarımcının boşta kaldıktan sonra kaynakları ayırması gerektiği anlamına gelir. İşlem kaynakları zaten ayrıldığından tekrarlanan işlem hattı çalıştırmaları daha az zaman alır. Ayrıca tasarımcı, verimliliği artırmak için her modül için önbelleğe alınmış sonuçları kullanır.

### <a name="view-scored-labels"></a>Puanlanmış etiketleri görüntüle

Çalıştırma tamamlandıktan sonra, işlem hattı çalıştırmasının sonuçlarını görüntüleyebilirsiniz. İlk olarak, regresyon modeli tarafından oluşturulan tahmine göz atın.

1. **Puan modeli** modülüne sağ tıklayın ve   >  çıktısını görüntülemek için **puanlanmış veri kümesini** Görselleştir ' i seçin.

    Burada, tahmin edilen fiyatları ve test verilerinin gerçek fiyatlarını görebilirsiniz.

    :::image type="content" source="./media/tutorial-designer-automobile-price-train-score/score-result.png"alt-text="Puanlanmış etiket sütununu vurgulayan çıkış görselleştirmesinin ekran görüntüsü":::

### <a name="evaluate-models"></a>Modelleri değerlendir

Eğitim modelinin test veri kümesinde ne kadar iyi gerçekleştirildiğini görmek için **modeli değerlendir** ' i kullanın.

1. **Modeli değerlendir** modülünü sağ tıklatın ve   >  çıktısını görüntülemek için **değerlendirme sonuçlarını** Görselleştir ' i seçin.

Modeliniz için aşağıdaki istatistikler gösterilmektedir:

* Ortalama **mutlak hata (MAE)**: mutlak hataların ortalaması. Bir hata, tahmin edilen değer ve gerçek değer arasındaki farktır.
* **Kök ortalama kare hatası (rmo)**: test veri kümesinde yapılan tahmine dayalı ortalama kare şeklindeki hata sayısının kare kökü.
* **Relative Absolute Error (Göreli Mutlak Hata)**: Gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki mutlak hataların mutlak farka göreli ortalaması.
* **Relative Squared Error (Göreli Karesi Alınmış Hata)**: Gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki karesi alınmış hataların karesi alınmış farka göreli ortalaması.
* **Belirleme katsayısı**: R kare değeri olarak da bilinen bu istatistiksel ölçüm, modelin verilere ne kadar iyi uyduğunu gösterir.

Her bir hata istatistiği ne kadar küçük olursa o kadar iyidir. Daha küçük bir değer, tahminlerinin gerçek değerlere yakın olduğunu gösterir. Belirleme katsayısı için, değeri bir (1,0), daha iyi tahmine dayalı olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticinin 2. bölümü ile devam etmek istiyorsanız, [modelleri dağıtmak](tutorial-designer-automobile-price-deploy.md)istiyorsanız bu bölümü atlayın.

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

İkinci bölümde, modelinizi gerçek zamanlı bir uç nokta olarak dağıtmayı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Modelleri dağıtmaya devam et](tutorial-designer-automobile-price-deploy.md)
