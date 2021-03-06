---
title: Özel alt etki alanları
titleSuffix: Azure Cognitive Services
description: Her bilişsel hizmet kaynağı için özel alt etki alanı adları Azure portal, Azure Cloud Shell veya Azure CLı aracılığıyla oluşturulur.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 12/04/2020
ms.author: erhopf
ms.openlocfilehash: 4746aad2d7cd62cb309a1823f8c50487e6f7e87c
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97032975"
---
# <a name="custom-subdomain-names-for-cognitive-services"></a>Bilişsel hizmetler için özel alt etki alanı adları

Azure bilişsel hizmetler, [Azure Portal](https://portal.azure.com), [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/)veya [Azure CLI](/cli/azure/install-azure-cli)aracılığıyla oluşturulan her kaynak için özel alt etki alanı adları kullanır. Belirli bir Azure bölgesindeki tüm müşteriler için ortak olan bölgesel bitiş noktalarından farklı olarak, özel alt etki alanı adları kaynağa özeldir. Kimlik doğrulaması için Azure Active Directory (Azure AD) gibi özellikleri etkinleştirmek için özel alt etki alanı adları gereklidir.

## <a name="how-does-this-impact-existing-resources"></a>Bu, mevcut kaynakları nasıl etkiler?

1 Temmuz 2019 tarihinden önce oluşturulan bilişsel hizmetler kaynakları, ilişkili hizmet için bölgesel uç noktaları kullanır. Bu uç noktalar, mevcut ve yeni kaynaklarla çalışır.

Mevcut bir kaynağı özel alt etki alanı adlarından yararlanmak üzere geçirmek istiyorsanız, Azure AD gibi özellikleri etkinleştirebilmeniz için aşağıdaki yönergeleri izleyin:

1. Azure portal oturum açın ve özel bir alt etki alanı adı eklemek istediğiniz bilişsel hizmetler kaynağını bulun.
2. **Genel bakış** dikey penceresinde, **özel etki alanı adı oluştur**' u bulun ve seçin.
3. Bu, kaynağınız için benzersiz bir özel alt etki alanı oluşturmaya yönelik yönergeler içeren bir panel açar.
   > [!WARNING]
   > Özel bir alt etki alanı adı oluşturduktan **sonra bu ad değiştirilemez.**

## <a name="do-i-need-to-update-my-existing-resources"></a>Mevcut kaynaklarımı güncelleştirmem gerekiyor mu?

Hayır. Bölgesel uç nokta, yeni ve mevcut bilişsel hizmetler için çalışmaya devam edecektir ve özel alt etki alanı adı isteğe bağlıdır. Özel bir alt etki alanı adı eklendiyse bile bölgesel uç nokta kaynakla çalışmaya devam edecektir.

## <a name="what-if-an-sdk-asks-me-for-the-region-for-a-resource"></a>Bir SDK bana bir kaynak için bölge istediğinde ne olacak?

> [!WARNING]
> Konuşma Hizmetleri **yalnızca** özel [uç noktalarla](Speech-Service/speech-services-private-link.md) özel alt etki alanları kullanır. Diğer tüm durumlarda, konuşma Hizmetleri ve ilişkili SDK 'lar ile **Bölgesel uç noktaları** kullanır.

Bölgesel uç noktalar ve özel alt etki alanı adları desteklenir ve birbirlerinin yerine kullanılabilir. Ancak tam uç nokta gereklidir.

Bölge bilgileri, [Azure Portal](https://portal.azure.com)kaynağınızın **genel bakış** dikey penceresinde kullanılabilir. Bölgesel bitiş noktalarının tam listesi için bkz. [Bölgesel uç noktaların listesi var mı?](#is-there-a-list-of-regional-endpoints)

## <a name="are-custom-subdomain-names-regional"></a>Özel alt etki alanı adları bölgesel mi?

Evet. Özel bir alt etki alanı adının kullanılması bilişsel hizmetler kaynağınızın bölgesel yönlerini değiştirmez.

## <a name="what-are-the-requirements-for-a-custom-subdomain-name"></a>Özel bir alt etki alanı adı için gereksinimler nelerdir?

Özel bir alt etki alanı adı, kaynağınız için benzersizdir. Ad yalnızca alfasayısal karakterler ve `-` karakteri içerebilir; uzunluğu 2 ile 64 karakter arasında olmalıdır ve ile bitemez `-` .

## <a name="can-i-change-a-custom-domain-name"></a>Özel bir etki alanı adını değiştirebilir miyim?

Hayır. Özel bir alt etki alanı adı oluşturulduktan ve bir kaynakla ilişkilendirildikten sonra değiştirilemez.

## <a name="can-i-reuse-a-custom-domain-name"></a>Özel bir etki alanı adını yeniden kullanabilir miyim?

Her özel alt etki alanı adı benzersizdir, bu nedenle bilişsel hizmetler kaynağına atadığınız özel bir alt etki alanı adını yeniden kullanmak için mevcut kaynağı silmeniz gerekir. Kaynak silindikten sonra, özel alt etki alanı adını yeniden kullanabilirsiniz.

## <a name="is-there-a-list-of-regional-endpoints"></a>Bölgesel bitiş noktaları listesi var mı?

Evet. Bu, Azure bilişsel hizmetler kaynaklarıyla kullanabileceğiniz bölgesel bitiş noktalarının bir listesidir.

> [!NOTE]
> Translator hizmeti ve Bing Arama API'leri genel uç noktaları kullanır.

| Uç nokta türü | Bölge | Uç Nokta |
|---------------|--------|----------|
| Genel | Genel (Translator & Bing) | `https://api.cognitive.microsoft.com` |
| | Doğu Avustralya | `https://australiaeast.api.cognitive.microsoft.com` |
| | Brezilya Güney | `https://brazilsouth.api.cognitive.microsoft.com` |
| | Orta Kanada | `https://canadacentral.api.cognitive.microsoft.com` |
| | Central US | `https://centralus.api.cognitive.microsoft.com` |
| | Doğu Asya | `https://eastasia.api.cognitive.microsoft.com` |
| | Doğu ABD | `https://eastus.api.cognitive.microsoft.com` |
| | Doğu ABD 2 | `https://eastus2.api.cognitive.microsoft.com` |
| | Orta Fransa | `https://francecentral.api.cognitive.microsoft.com` |
| | Hindistan Orta | `https://centralindia.api.cognitive.microsoft.com` |
| | Doğu Japonya | `https://japaneast.api.cognitive.microsoft.com` |
| | Güney Kore - Orta | `https://koreacentral.api.cognitive.microsoft.com` |
| | Orta Kuzey ABD | `https://northcentralus.api.cognitive.microsoft.com` |
| | Kuzey Avrupa | `https://northeurope.api.cognitive.microsoft.com` |
| | Güney Afrika - Kuzey | `https://southafricanorth.api.cognitive.microsoft.com` |
| | Orta Güney ABD | `https://southcentralus.api.cognitive.microsoft.com` |
| | Güneydoğu Asya | `https://southeastasia.api.cognitive.microsoft.com` |
| | Güney Birleşik Krallık | `https://uksouth.api.cognitive.microsoft.com` |
| | Orta Batı ABD | `https://westcentralus.api.cognitive.microsoft.com` |
| | West Europe | `https://westeurope.api.cognitive.microsoft.com` |
| | Batı ABD | `https://westus.api.cognitive.microsoft.com` |
| | Batı ABD 2 | `https://westus2.api.cognitive.microsoft.com` |
| US Gov | US Gov Virginia | `https://virginia.api.cognitive.microsoft.us` |
| Çin | Çin Doğu 2 | `https://chinaeast2.api.cognitive.azure.cn` |
| | Kuzey Çin | `https://chinanorth.api.cognitive.azure.cn` |

## <a name="see-also"></a>Ayrıca bkz.

* [Bilişsel hizmetler nelerdir?](./what-are-cognitive-services.md)
* [Kimlik Doğrulaması](authentication.md)