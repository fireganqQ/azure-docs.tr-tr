---
title: Özel DNS sunucusu kullan
titleSuffix: Azure Machine Learning
description: Özel bir DNS sunucusunu bir Azure Machine Learning çalışma alanıyla ve özel uç noktayla çalışacak şekilde yapılandırma.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: jhirono
author: jhirono
ms.date: 11/20/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 2215c47fcd250a9ac1d6621f7e4b434bd33b3832
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98871104"
---
# <a name="how-to-use-your-workspace-with-a-custom-dns-server"></a>Çalışma alanınızı özel DNS sunucusuyla kullanma

Özel bir uç nokta ile Azure Machine Learning çalışma alanı kullanırken, [DNS ad çözümlemesini işlemek için birkaç yol](../private-link/private-endpoint-dns.md)vardır. Varsayılan olarak, Azure, çalışma alanınız ve özel uç noktanız için ad çözümlemesini otomatik olarak işler. Bunun yerine _kendi özel DNS sunucunuzu kullanıyorsanız_, DNS girdilerini el ile oluşturmanız veya çalışma alanı için koşullu ileticiler kullanmanız gerekir.

> [!IMPORTANT]
> Bu makalede, bu girdilerin yalnızca tam etki alanı adını (FQDN) ve IP adreslerini bulmak için DNS kayıtlarını yapılandırma hakkında bilgi sağlamayan bilgiler yer alır. Kayıt ekleme hakkında bilgi edinmek için DNS yazılımınızın belgelerine bakın.

## <a name="prerequisites"></a>Önkoşullar

- [Kendı DNS sunucunuzu](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)kullanan bir Azure sanal ağı.

- Özel uç noktası olan bir Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

- [Eğitim & çıkarımı sırasında ağ yalıtımının](./how-to-network-security-overview.md)kullanılmasıyla ilgili benzerlik.

- [Azure özel uç nokta DNS bölge yapılandırmasıyla](../private-link/private-endpoint-dns.md) benzerlik

- İsteğe bağlı olarak, [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/install-az-ps).

## <a name="fqdns-in-use"></a>Kullanımdaki FQDN 'Ler
### <a name="these-fqdns-are-in-use-in-the-following-regions-eastus-southcentralus-and-westus2"></a>Bu FQDN 'Ler şu bölgelerde kullanımda: eastus, Güneydoğu ABD ve westus2.
Aşağıdaki liste, çalışma alanınız tarafından kullanılan tam etki alanı adlarını (FQDN) içerir:

* `<workspace-GUID>.workspace.<region>.cert.api.azureml.ms`
* `<workspace-GUID>.workspace.<region>.api.azureml.ms`
* `<workspace-GUID>.workspace.<region>.experiments.azureml.net`
* `<workspace-GUID>.workspace.<region>.modelmanagement.azureml.net`
* `<workspace-GUID>.workspace.<region>.aether.ms`
* `ml-<workspace-name>-<region>-<workspace-guid>.notebooks.azure.net`
* Bir işlem örneği oluşturursanız, `<instance-name>.<region>.instances.azureml.ms` çalışma alanı özel uç noktasının özel IP 'si ile için bir giriş de eklemeniz gerekir.

    > [!NOTE]
    > İşlem örneklerine yalnızca sanal ağ içinden erişilebilir.
    
### <a name="these-fqdns-are-in-use-in-all-other-regions"></a>Bu FQDN 'Ler diğer tüm bölgelerde kullanımda
Aşağıdaki liste, çalışma alanınız tarafından kullanılan tam etki alanı adlarını (FQDN) içerir:

* `<workspace-GUID>.workspace.<region>.cert.api.azureml.ms`
* `<workspace-GUID>.workspace.<region>.api.azureml.ms`
* `ml-<workspace-name>-<region>-<workspace-guid>.notebooks.azure.net`
* `<instance-name>.<region>.instances.azureml.ms`

    > [!NOTE]
    > İşlem örneklerine yalnızca sanal ağ içinden erişilebilir.

## <a name="find-the-ip-addresses"></a>IP adreslerini bulma

VNet 'teki FQDN 'lerin iç IP adreslerini bulmak için aşağıdaki yöntemlerden birini kullanın:

> [!NOTE]
> Tam etki alanı adları ve IP adresleri, yapılandırmanıza göre farklı olacaktır. Örneğin, etki alanı adı içindeki GUID değeri çalışma alanınıza özgü olacaktır.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az network private-endpoint show --endpoint-name <endpoint> --resource-group <resource-group> --query 'customDnsConfigs[*].{FQDN: fqdn, IPAddress: ipAddresses[0]}' --output table
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell
$workspaceDns=Get-AzPrivateEndpoint -Name <endpoint> -resourcegroupname <resource-group>
$workspaceDns.CustomDnsConfigs | format-table
```

# <a name="azure-portal"></a>[Azure portalı](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com), Azure Machine Learning __çalışma alanınızı__ seçin.
1. __Ayarlar__ bölümünde, __Özel uç nokta bağlantıları__' nı seçin.
1. Görüntülenen __Özel uç nokta__ sütunundaki bağlantıyı seçin.
1. Çalışma alanı özel uç noktası için tam etki alanı adlarının (FQDN) ve IP adreslerinin listesi sayfanın en altında bulunur.

:::image type="content" source="./media/how-to-custom-dns/private-endpoint-custom-dns.png" alt-text="Portalda FQDN 'lerin listesi":::

---

Tüm yöntemlerden döndürülen bilgiler aynıdır; kaynaklar için FQDN ve özel IP adresi listesi.

| FQDN | IP Adresi |
| ----- | ----- |
| `fb7e20a0-8891-458b-b969-55ddb3382f51.workspace.eastus.api.azureml.ms` | `10.1.0.5` |
| `ml-myworkspace-eastus-fb7e20a0-8891-458b-b969-55ddb3382f51.notebooks.azure.net` | `10.1.0.6` |

> [!IMPORTANT]
> Bazı FQDN 'Ler özel uç nokta tarafından listelenmemektedir, ancak eastus, Güneydoğu ve westus2 içinde çalışma alanı için gereklidir. Bu FQDN 'Ler aşağıdaki tabloda listelenmiştir ve ayrıca DNS sunucunuza ve/veya bir Azure Özel DNS bölgesine eklenmelidir:
>
> * `<workspace-GUID>.workspace.<region>.cert.api.azureml.ms`
> * `<workspace-GUID>.workspace.<region>.experiments.azureml.net`
> * `<workspace-GUID>.workspace.<region>.modelmanagement.azureml.net`
> * `<workspace-GUID>.workspace.<region>.aether.ms`
> * İşlem örneğiniz varsa, öğesini kullanın; `<instance-name>.<region>.instances.azureml.ms` burada, `<instance-name>` işlem örneğinizin adıdır. Lütfen çalışma alanı özel uç noktasının özel IP adresini kullanın. Lütfen işlem örneğine yalnızca sanal ağ içinden erişilebilir.
>
> Bu IP adreslerinin tümü için, `*.api.azureml.ms` önceki adımlardan döndürülen girişlerle aynı adresi kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Sanal Ağla Azure Machine Learning kullanma hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](how-to-network-security-overview.md).

Özel uç noktaları DNS yapılandırmanızla tümleştirme hakkında daha fazla bilgi için bkz. [Azure özel uç nokta DNS yapılandırması](../private-link/private-endpoint-dns.md).