---
title: Kullanıcı utslerini gözden geçirme-LUSıS
titleSuffix: Azure Cognitive Services
description: Etkin öğrenimi tarafından yakalanan bir şekilde gözden geçirerek, okuma ve varlık kullanımı için varlıkları seçin; değişiklikleri kabul edin, eğitme ve yayımlayın.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 12/08/2020
ms.openlocfilehash: ea2b44d05d25756a16b6b84f0734966b1f579848
ms.sourcegitcommit: 273c04022b0145aeab68eb6695b99944ac923465
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97007611"
---
# <a name="how-to-improve-the-luis-app-by-reviewing-endpoint-utterances"></a>Uç nokta utbotları inceleyerek LUO uygulamasını geliştirme

Doğru tahmine yönelik uç nokta dıklarını gözden geçirme işlemi, [etkin öğrenme](luis-concept-review-endpoint-utterances.md)olarak adlandırılır. Etkin öğrenme, uç nokta sorgularını yakalar ve kullanıcının gereken uç nokta utlerini seçer. Bu gerçek zamanlı olarak amacı belirlemek ve varlıkları işaretlemek için bu balonları gözden geçirin. Örnek ifadelerinizde bu değişiklikleri kabul edin, sonra da eğitim ve yayımlayın. LUO daha sonra, bir daha doğru şekilde daha doğru şekilde tanımlanır.

## <a name="log-user-queries-to-enable-active-learning"></a>Etkin öğrenmeyi etkinleştirmek için Kullanıcı sorgularını günlüğe kaydet

Etkin öğrenmeyi etkinleştirmek için Kullanıcı sorgularını günlüğe yazmanız gerekir. Bu, ENDPOINT parametresi ve değeri ile [Endpoint sorgusu](luis-get-started-create-app.md#query-the-v3-api-prediction-endpoint) çağırarak yapılır `log=true` .

Doğru uç nokta sorgusunu oluşturmak için LUO portalını kullanın.

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Yönet** bölümüne gidin ve **Azure kaynakları**' nı seçin.
1. Atanan tahmin kaynağı için **sorgu parametrelerini değiştir**' i seçin.

    > [!div class="mx-imgBorder"]
    > ![Ekran görüntüsü sorgu parametrelerini değiştir bağlantısını gösterir.](./media/luis-tutorial-review-endpoint-utterances/azure-portal-change-query-url-settings.png)

1. Kaydet ' i seçerek Kaydet **günlüklerine** geçiş **yapın.**

    > [!div class="mx-imgBorder"]
    > ![Etkin öğrenme için gerekli olan günlükleri kaydetmek için LUO portalını kullanın.](./media/luis-tutorial-review-endpoint-utterances/luis-portal-manage-azure-resource-save-logs.png)

     Bu eylem QueryString parametresini ekleyerek örnek URL 'YI değiştirir `log=true` . Çalışma zamanı uç noktasına tahmin sorguları yaparken değiştirilen örnek sorgu URL 'sini kopyalayın ve kullanın.

## <a name="correct-intent-predictions-to-align-utterances"></a>Söylemeleri hizalamak için amaç tahminleri doğru

Her söylük, **hizalanmış amaç** sütununda gösterilen önerilen bir amaca sahiptir.

> [!div class="mx-imgBorder"]
> [![LUO 'nun emin olduğu uç nokta utbotları gözden geçirin](./media/label-suggested-utterances/review-endpoint-utterances.png)](./media/label-suggested-utterances/review-endpoint-utterances.png#lightbox)

Bu amacı kabul ediyorsanız onay işaretini seçin. Öneriyi kabul etmiyorsanız, hizalanmış amaç açılan listesinden doğru amacı seçin, sonra hizalanmış amaç ' ın sağındaki onay işaretine tıklayın. Onay işaretini seçtikten sonra, söylenişi hedefe taşınır ve **Gözden geçirme uç noktası uttasları** listesinden kaldırılır.

> [!TIP]
> **Gözden geçirme uç noktası dıklarından** gelen tüm örnek varlık tahminlerini gözden geçirmek ve düzeltmek için amaç ayrıntıları sayfasına gitmeniz önemlidir.

## <a name="delete-utterance"></a>Söylenişi silme

Her bir söylik İnceleme listesinden silinebilir. Silindikten sonra listede bir daha görünmez. Bu, Kullanıcı uç noktadan aynı şekilde girdiğinde bile geçerlidir.

Söylenişi 'i silmeniz gerekip gerekmediğini bilmiyorsanız, bunu hiçbiri amacına taşıyın ya da gibi yeni bir amaç oluşturun `miscellaneous` ve bu amaca göre taşıyın.

## <a name="disable-active-learning"></a>Etkin öğrenmeyi devre dışı bırak

Etkin öğrenmeyi devre dışı bırakmak için Kullanıcı sorgularını günlüğe eklemeyin. Bu, [](luis-get-started-create-app.md#query-the-v2-api-prediction-endpoint) `log=false` varsayılan değer false olduğundan, QueryString parametresi ve değeri ile Endpoint sorgusu ayarlanarak ve QueryString değeri olmadan gerçekleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen söyleylerini etiketledikten sonra performansın nasıl artdığı test etmek için, üst panelde **Test** ' i seçerek test konsoluna erişebilirsiniz. Test konsolunu kullanarak uygulamanızı test etme hakkında yönergeler için bkz. [uygulamanızı eğitme ve test](luis-interactive-test.md)etme.
