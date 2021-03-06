---
title: 'Hızlı başlangıç: ARM şablonunu kullanarak FHıR için Azure API dağıtma'
description: Bu hızlı başlangıçta, bir Azure Resource Manager şablonu (ARM şablonu) kullanarak hızlı sağlık birlikte çalışabilirlik kaynakları (FHıR®) için Azure API 'yi dağıtmayı öğrenin.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.custom: subject-armqs, devx-track-azurecli
ms.author: cavoeg
ms.date: 10/06/2020
ms.openlocfilehash: 0cc31c26a71775dfdf3e9ee94852fda8af2c0cfc
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94844548"
---
# <a name="quickstart-use-an-arm-template-to-deploy-azure-api-for-fhir"></a>Hızlı başlangıç: FHıR için Azure API 'yi dağıtmak üzere ARM şablonu kullanma

Bu hızlı başlangıçta, hızlı sağlık ile birlikte çalışabilirlik kaynakları (FHıR®) için Azure API 'SI dağıtmak üzere bir Azure Resource Manager şablonu (ARM şablonu) kullanmayı öğreneceksiniz. FHıR için Azure API 'sini Azure portal, PowerShell veya CLı aracılığıyla dağıtabilirsiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Bu şablon, oturum açıldıktan sonra Azure portal açılır.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Azure portal bir ARM şablonu kullanarak FHıR hizmeti için Azure API 'sine dağıtım yapın.":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-azure-api-for-fhir%2fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

# <a name="portal"></a>[Portal](#tab/azure-portal)

Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/).

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/).
* Kodu yerel olarak çalıştırmak istiyorsanız, [Azure PowerShell](/powershell/azure/install-az-ps).

# <a name="cli"></a>[CLI](#tab/CLI)

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/).
* Kodu yerel olarak çalıştırmak istiyorsanız:
    * Bash kabuğu ( [Windows Için git](https://gitforwindows.org)'e dahil olan git Bash gibi).
    * [Azure CLI](/cli/azure/install-azure-cli).

---

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-azure-api-for-fhir/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-azure-api-for-fhir/azuredeploy.json":::

Şablon bir Azure kaynağı tanımlar:

* **Microsoft. Healthgelişme API 'leri/Hizmetleri**

<!--

Replace the line above with the following line once https://docs.microsoft.com/azure/templates/microsoft.healthcareapis/services goes live:

* [**Microsoft.HealthcareApis/services**](/azure/templates/microsoft.healthcareapis/services)

-->

## <a name="deploy-the-template"></a>Şablonu dağıtma

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal 'de ARM şablonunu kullanarak FHıR için Azure API 'sini dağıtmak üzere aşağıdaki bağlantıyı seçin:

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Azure portal ARM şablonunu kullanarak FHıR hizmeti için Azure API 'sine dağıtım yapın.":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-azure-api-for-fhir%2fazuredeploy.json)

**FHıR Için Azure API dağıtma** sayfasında:

1. İsterseniz, **aboneliği** varsayılan değerinden farklı bir aboneliğe değiştirin.

2. **Kaynak grubu** Için **Yeni oluştur**' u seçin, yeni kaynak grubu için bir ad girin ve **Tamam**' ı seçin.

3. Yeni bir kaynak grubu oluşturduysanız, kaynak grubu için bir **bölge** seçin.

4. Yeni bir **hizmet adı** girin ve fhır IÇIN Azure API 'sinin **konumunu** seçin. Konum, kaynak grubunun bölgesiyle aynı veya farklı olabilir.

    :::image type="content" source="./media/fhir-paas-arm-template-quickstart/deploy-azure-api-fhir.png" alt-text="Azure portal ARM şablonunu kullanarak FHıR için Azure API 'sini dağıtın.":::

5. **Gözden geçir + oluştur**’u seçin.

6. Hüküm ve koşulları okuyun ve ardından **Oluştur**' u seçin.

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

> [!NOTE]
> PowerShell betiklerini yerel olarak çalıştırmak istiyorsanız, ilk olarak `Connect-AzAccount` Azure kimlik bilgilerinizi kurmak için girin.

`Microsoft.HealthcareApis`Kaynak sağlayıcısı aboneliğiniz için zaten kayıtlı değilse, aşağıdaki etkileşimli kodla kaydedebilirsiniz. Kodu Azure Cloud Shell çalıştırmak için herhangi bir kod bloğunun üst köşesinde **deneyin** ' i seçin.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.HealthcareApis
```

ARM şablonunu kullanarak FHıR hizmeti için Azure API 'yi dağıtmak üzere aşağıdaki kodu kullanın. Kod, yeni FHıR hizmet adını, yeni bir kaynak grubunun adını ve bunların her birinin konumunu ister.

```azurepowershell-interactive
$serviceName = Read-Host -Prompt "Enter a name for the new Azure API for FHIR service"
$serviceLocation = Read-Host -Prompt "Enter an Azure region (for example, westus2) for the service"
$resourceGroupName = Read-Host -Prompt "Enter a name for the new resource group to contain the service"
$resourceGroupRegion = Read-Host -Prompt "Enter an Azure region (for example, centralus) for the resource group"

Write-Verbose "New-AzResourceGroup -Name $resourceGroupName -Location $resourceGroupRegion" -Verbose
New-AzResourceGroup -Name $resourceGroupName -Location $resourceGroupRegion
Write-Verbose "Run New-AzResourceGroupDeployment to create an Azure API for FHIR service using an ARM template" -Verbose
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
    -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-azure-api-for-fhir/azuredeploy.json `
    -serviceName $serviceName `
    -location $serviceLocation
Read-Host "Press [ENTER] to continue"
```

# <a name="cli"></a>[CLI](#tab/CLI)

`Microsoft.HealthcareApis`Kaynak sağlayıcısı aboneliğiniz için zaten kayıtlı değilse, aşağıdaki etkileşimli kodla kaydedebilirsiniz. Kodu Azure Cloud Shell çalıştırmak için herhangi bir kod bloğunun üst köşesinde **deneyin** ' i seçin.

```azurecli-interactive
az extension add --name healthcareapis
```

ARM şablonunu kullanarak FHıR hizmeti için Azure API 'yi dağıtmak üzere aşağıdaki kodu kullanın. Kod, yeni FHıR hizmet adını, yeni bir kaynak grubunun adını ve bunların her birinin konumunu ister.

```azurecli-interactive
read -p "Enter a name for the new Azure API for FHIR service: " serviceName &&
read -p "Enter an Azure region (for example, westus2) for the service: " serviceLocation &&
read -p "Enter a name for the new resource group to contain the service: " resourceGroupName &&
read -p "Enter an Azure region (for example, centralus) for the resource group: " resourceGroupRegion &&
params='serviceName='$serviceName' location='$serviceLocation &&
echo "CREATE RESOURCE GROUP:  az group create --name $resourceGroupName --location $resourceGroupRegion" &&
az group create --name $resourceGroupName --location $resourceGroupRegion &&
echo "RUN az deployment group create, which creates an Azure API for FHIR service using an ARM template" &&
az deployment group create --resource-group $resourceGroupName --parameters $params --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-azure-api-for-fhir/azuredeploy.json &&
read -p "Press [ENTER] to continue: "
```

---

> [!NOTE]
> Dağıtımın tamamlanabilmesi birkaç dakika sürer. Daha sonra dağıtılan kaynakları gözden geçirmek için kullandığınız FHıR hizmeti için Azure API 'sinin ve kaynak grubunun adlarını aklınızda bulabilirsiniz.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

# <a name="portal"></a>[Portal](#tab/azure-portal)

FHıR hizmetine yönelik yeni Azure API 'nize genel bir bakış için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com), **Fhır IÇIN Azure API 'yi** arayın ve seçin.

2. FHıR listesinde yeni hizmetinizi seçin. FHıR hizmeti için yeni Azure API 'SI için **genel bakış** sayfası görüntülenir.

3. Yeni FHıR API hesabının sağlandığını doğrulamak için fhır API 'SI özelliğini getirmek üzere **fhır meta veri uç noktası** ' nın yanındaki bağlantıyı seçin. Bağlantının biçimi vardır `https://<service-name>.azurehealthcareapis.com/metadata` . Hesap sağlandıysa, büyük bir JSON dosyası görüntülenir.

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

FHıR hizmeti için Azure API 'niz hakkındaki ayrıntıları görüntülemek için aşağıdaki etkileşimli kodu çalıştırın. Yeni hizmet ve kaynak grubunun adını girmeniz gerekir.

```azurepowershell-interactive
$serviceName = Read-Host -Prompt "Enter the name of your Azure API for FHIR service"
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Write-Verbose "Get-AzHealthcareApisService -ResourceGroupName $resourceGroupName -Name $serviceName" -Verbose
Get-AzHealthcareApisService -ResourceGroupName $resourceGroupName -Name $serviceName
Read-Host "Press [ENTER] to fetch the FHIR API capability statement, which shows that the new service has been provisioned"

$requestUri="https://" + $serviceName + ".azurehealthcareapis.com/metadata"
$metadata = Invoke-WebRequest -Uri $requestUri
$metadata.RawContent
Read-Host "Press [ENTER] to continue"
```

# <a name="cli"></a>[CLI](#tab/CLI)

FHıR hizmeti için Azure API 'niz hakkındaki ayrıntıları görüntülemek için aşağıdaki etkileşimli kodu çalıştırın. Yeni hizmet ve kaynak grubunun adını girmeniz gerekir.

```azurecli-interactive
read -p "Enter the name of your Azure API for FHIR service: " serviceName &&
read -p "Enter the resource group name: " resourceGroupName &&
echo "SHOW SERVICE DETAILS:  az healthcareapis service show --resource-group $resourceGroupName --resource-name $serviceName" &&
az healthcareapis service show --resource-group $resourceGroupName --resource-name $serviceName &&
read -p "Press [ENTER] to fetch the FHIR API capability statement, which shows that the new service has been provisioned: " &&
requestUrl='https://'$serviceName'.azurehealthcareapis.com/metadata' &&
curl --url $requestUrl &&
read -p "Press [ENTER] to continue: "
```

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubundaki kaynakları silen kaynak grubunu silin.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com), **kaynak gruplarını** arayıp seçin.

2. Kaynak grubu listesinde, kaynak grubunuzun adını seçin.

3. Kaynak grubunuzun **genel bakış** sayfasında **kaynak grubunu sil**' i seçin.

4. Onay iletişim kutusunda, kaynak grubunuzun adını yazın ve ardından **Sil**' i seçin.

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the name of the resource group to delete"
Write-Verbose "Remove-AzResourceGroup -Name $resourceGroupName" -Verbose
Remove-AzResourceGroup -Name $resourceGroupName
Read-Host "Press [ENTER] to continue"
```

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
read -p "Enter the name of the resource group to delete: " resourceGroupName &&
echo "DELETE A RESOURCE GROUP (AND ITS RESOURCES):  az group delete --name $resourceGroupName" &&
az group delete --name $resourceGroupName &&
read -p "Press [ENTER] to continue: "
```

---

ARM şablonu oluşturma sürecinde size kılavuzluk eden adım adım bir öğretici için, [Ilk ARM şablonunuzu oluşturma ve dağıtma öğreticisine](../azure-resource-manager/templates/template-tutorial-create-first-template.md) bakın

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç kılavuzunda, FHıR için Azure API 'sini aboneliğinize dağıttınız. FHıR için Azure API 'niz üzerinde ek ayarlar ayarlamak için, ek ayarlar nasıl yapılır Kılavuzu ' na ilerleyin. FHıR için Azure API 'yi kullanmaya başlamaya hazırsanız, uygulamaları kaydetme hakkında daha fazla bilgi edinin.

>[!div class="nextstepaction"]
>[FHıR için Azure API 'de ek ayarlar](azure-api-for-fhir-additional-settings.md)

>[!div class="nextstepaction"]
>[Uygulamaları kaydetme genel bakış](fhir-app-registration.md)
