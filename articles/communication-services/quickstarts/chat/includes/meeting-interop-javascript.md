---
title: Hızlı başlangıç-bir takım toplantısına katılarak
author: askaur
ms.author: askaur
ms.date: 12/08/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 780ef2bbb7851d8bef5fc52a51421a7938043ecb
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98932296"
---
## <a name="join-the-meeting-chat"></a>Toplantı sohbetine katılın 

Takımlar birlikte çalışabilirliği etkinleştirildikten sonra, bir Iletişim Hizmetleri kullanıcısı, çağıran istemci kitaplığını kullanarak bir Konuk Kullanıcı olarak takımlar çağrısına katılabilir. Çağrının katılması, bunları, çağrı üzerinde diğer kullanıcılarla ileti gönderebilecekleri ve buradan toplantı sohbetine katılımcı olarak ekler. Kullanıcının, çağrıya katılmadan önce gönderilen sohbet iletilerine erişimi olmayacaktır. 

## <a name="get-a-teams-meeting-chat-thread-for-a-communication-services-user"></a>Bir Iletişim Hizmetleri kullanıcısına yönelik sohbet iş parçacığını toplantıya yönelik bir takım alın

İlk olarak, `ChatThreadClient` Toplantı sohbeti iş parçacığı için bir örneğini oluşturun. İş parçacığı KIMLIĞINI almak için toplantı bağlantısını ayrıştırın veya toplantı KIMLIĞIYLE Graph API 'Lerini kullanın. 

- Bir ekip toplantısı bağlantısı şöyle görünür: `https://teams.microsoft.com/l/meetup-join/meeting_chat_thread_id/1606337455313?context=some_context_here` . İş parçacığı KIMLIĞI, `meeting_chat_thread_id` bu bağlantıdaki nerede olacaktır. 
- Toplantı KIMLIĞINIZ varsa, iş parçacığı KIMLIĞINI almak için [Graph API](/graph/api/onlinemeeting-createorget?tabs=http&view=graph-rest-beta) kullanabilirsiniz. [Get API](/graph/api/onlinemeeting-get?tabs=http%22+%5c&view=graph-rest-beta) yanıtı `chatInfo` , içeren bir nesnesine sahip olur `threadID` . 

Sohbet iş parçacığı KIMLIĞI ' ne sahip olduktan sonra JavaScript sohbet istemci Kitaplığı ' nı kullanarak sohbet iş parçacığı istemcisini edinebilirsiniz: 

```javascript
let chatThreadClient = await chatClient.getChatThreadClient(threadId); 

console.log(`Chat Thread client for threadId:${chatThreadClient.threadId}`); 
```
  
`chatThreadClient`Sohbet iş parçacığındaki üyeleri listelemek, sohbet geçmişini almak ve ileti göndermek için kullanılabilir.  

## <a name="send-and-receive-messages"></a>İleti alma ve gönderme  

' Nı kullanarak `SendMessage` Toplantı sohbetine bir ileti gönderin. Gelen iletileri almak için, `chatMessageReceived` Bu senaryo için gerçek zamanlı sinyal henüz etkinleştirilmediği için, gibi olaylara abone olma özelliği desteklenmez. En son iletileri almak için API 'yi yoklayabilmeniz gerekir `ListMessages` . Birlikte çalışabilirlik senaryosunda, `ListMessages` API artık üç yeni ileti türü döndürmeyi desteklemektedir:
- `RichText/HTML`
- `ThreadActivity/MemberJoined`
- `ThreadActivity/MemberLeft` </br>

İleti türleri hakkında daha fazla bilgi için [buraya](../../../concepts/chat/concepts.md)bakın. 

**Note** -Şu anda yalnızca gönderme ve alma iletileri ekiplerle birlikte çalışabilirlik senaryolarında desteklenir. Takım toplantılarından diğer kullanıcıları eklemek veya kaldırmak için Kullanıcı ekleme ve yazma göstergeleri ve Iletişim Hizmetleri gibi diğer özellikler henüz desteklenmemektedir.  

