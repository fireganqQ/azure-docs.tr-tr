---
title: Azure Media Services hesabı oluşturma
description: Bu öğretici Azure Media Services hesap oluşturma adımlarında size yol gösterir.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/4/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: b9234b27e2f08e65f569393bde342cba3f37adee
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "95971366"
---
# <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Azure 'da medya içeriğini şifrelemeyi, kodlamayı, çözümlemeyi, yönetmeyi ve akışını başlatmak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabının bir veya daha fazla depolama hesabıyla ilişkilendirilmesi gerekir. Bu makalede yeni bir Azure Media Services hesabı oluşturmaya yönelik adımlar açıklanmaktadır.

[!INCLUDE [note 2020-05-01 API](./includes/note-2020-05-01-account-creation.md)]

 Bir Media Services hesabı oluşturmak için Azure portal ya da CLı kullanabilirsiniz. Kullanmak istediğiniz yöntem için sekmeyi seçin.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<!-- NOTE: The following are in the includes folder and are reused in other How To articles. All task based content should be in the includes folder with the task- prefix prepended to the file name. -->

## <a name="portal"></a>[Portal](#tab/portal/)

[!INCLUDE [the media services account and storage account must be in the same subscription](./includes/note-account-storage-same-subscription.md)]

[!INCLUDE [create a media services account in the portal](./includes/task-create-media-services-account-portal.md)]

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [the media services account and storage account must be in the same subscription](./includes/note-account-storage-same-subscription.md)]

## <a name="set-the-azure-subscription"></a>Azure aboneliğini ayarlama

[!INCLUDE [Set the Azure subscription with CLI](./includes/task-set-azure-subscription-cli.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create a resource group with CLI](./includes/task-create-resource-group-cli.md)]

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[!INCLUDE [Create a storage account with CLI](./includes/task-create-storage-account-cli.md)]

## <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

[!INCLUDE [Create a Media Services account with CLI](./includes/task-create-media-services-account-cli.md)]

---

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)