---
title: Rol atamalarını Azure Active Directory bir gruptan Kaldır | Microsoft Docs
description: Kimlik yönetimi temsilcisi seçme için özel Azure AD rollerini önizleyin. Azure portal, PowerShell veya Graph API Azure rollerini yönetin.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: article
ms.date: 11/05/2020
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89fa3bb94f72ab04c2ea68641b8d1dff7695aa53
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98741035"
---
# <a name="remove-role-assignments-from-a-group-in-azure-active-directory"></a>Rol atamalarını Azure Active Directory gruptan Kaldır

Bu makalede, bir BT yöneticisinin gruplara atanan Azure AD rollerini nasıl kaldıraabileceği açıklanır. Azure portal, artık hem doğrudan hem de dolaylı rol atamalarını bir kullanıcıya kaldırabilirsiniz. Bir kullanıcıya bir grup üyeliği tarafından bir rol atanırsa rol atamasını kaldırmak için kullanıcıyı gruptan kaldırın.

## <a name="using-azure-admin-center"></a>Azure Yönetim Merkezi 'ni kullanma

1. Azure AD kuruluşunda ayrıcalıklı rol yöneticisi veya genel yönetici izinleriyle [Azure AD Yönetim merkezinde](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) oturum açın.

1. **Roller ve yöneticiler** > **_rol adı_* _ ' i seçin.

1. Rol atamasını kaldırmak istediğiniz grubu seçin ve _ * Atamayı Kaldır * * ' ı seçin.

   ![Seçili bir gruptan rol atamasını Kaldır.](./media/groups-remove-assignment/remove-assignment.png)

1. Eyleminizi onaylamanız istendiğinde **Evet**' i seçin.

## <a name="using-powershell"></a>PowerShell’i kullanma

### <a name="create-a-group-that-can-be-assigned-to-role"></a>Role atanabilecek bir grup oluşturun

```powershell
$group = New-AzureADMSGroup -DisplayName "Contoso_Helpdesk_Administrators" -Description "This group is assigned to Helpdesk Administrator built-in role in Azure AD." -MailEnabled $true -SecurityEnabled $true -MailNickName "contosohelpdeskadministrators" -IsAssignableToRole $true
```

### <a name="get-the-role-definition-you-want-to-assign-the-group-to"></a>Gruba atamak istediğiniz rol tanımını alın

```powershell
$roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Helpdesk Administrator'"
```

### <a name="create-a-role-assignment"></a>Rol ataması oluşturma

```powershell
$roleAssignment = New-AzureADMSRoleAssignment -ResourceScope '/' -RoleDefinitionId $roleDefinition.Id -PrincipalId $group.objectId
```

### <a name="remove-the-role-assignment"></a>Rol atamasını Kaldır

```powershell
Remove-AzureAdMSRoleAssignment -Id $roleAssignment.Id 
```

## <a name="using-microsoft-graph-api"></a>Microsoft Graph API 'sini kullanma

### <a name="create-a-group-that-can-be-assigned-an-azure-ad-role"></a>Azure AD rolü atanabilen bir grup oluşturun

```powershell
POST https://graph.microsoft.com/beta/groups

{
"description": "This group is assigned to Helpdesk Administrator built-in role of Azure AD",
"displayName": "Contoso_Helpdesk_Administrators",
"groupTypes": [
"Unified"
],
"mailEnabled": true,
"securityEnabled": true
"mailNickname": "contosohelpdeskadministrators",
"isAssignableToRole": true,
}
```

### <a name="get-the-role-definition"></a>Rol tanımını al

```powershell
GET https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions?$filter = displayName eq ‘Helpdesk Administrator’
```

### <a name="create-the-role-assignment"></a>Rol atamasını oluşturma

```powershell
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
{
"principalId":"<Object Id of Group>",
"roleDefinitionId":"<Id of role definition>",
"directoryScopeId":"/"
}
```

### <a name="delete-role-assignment"></a>Rol atamasını Sil

```powershell
DELETE https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/<Id of role assignment>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Rol atamalarını yönetmek için bulut gruplarını kullanma](groups-concept.md)
- [Bulut gruplarına atanan rollerle ilgili sorunları giderme](groups-faq-troubleshooting.md)
