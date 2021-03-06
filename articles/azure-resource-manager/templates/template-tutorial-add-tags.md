---
title: Öğretici-şablondaki kaynaklara etiketler ekleme
description: Azure Resource Manager şablonunuzda dağıttığınız kaynaklara Etiketler ekleyin (ARM şablonu). Etiketler, kaynakları mantıksal olarak düzenlemenizi sağlar.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: ''
ms.openlocfilehash: 625a88c0ee946b1ca67737d9cc67b638699d12f0
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97107014"
---
# <a name="tutorial-add-tags-in-your-arm-template"></a>Öğretici: ARM şablonunuza etiketler ekleme

Bu öğreticide, Azure Resource Manager şablonunuzda (ARM şablonu) kaynaklara etiket eklemeyi öğreneceksiniz. [Etiketler](../management/tag-resources.md) , kaynaklarınızı mantıksal olarak düzenlemenize yardımcı olur. Etiket değerleri, maliyet raporlarında gösterilir. Bu öğreticinin tamamlandığı **8 dakika** sürer.

## <a name="prerequisites"></a>Önkoşullar

[Hızlı başlangıç şablonları hakkında öğreticiyi](template-tutorial-quickstart-template.md)tamamlamanızı öneririz, ancak bu gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell ya da Azure CLı ile Visual Studio Code olması gerekir. Daha fazla bilgi için bkz. [şablon araçları](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Şablonu gözden geçir

Önceki şablonunuz bir depolama hesabı, App Service planı ve Web uygulaması dağıttı.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.json":::

Bu kaynakları dağıttıktan sonra, maliyetleri izlemeniz ve bir kategoriye ait kaynakları bulmanız gerekebilir. Bu sorunları çözmeye yardımcı olmak için Etiketler ekleyebilirsiniz.

## <a name="add-tags"></a>Etiket ekleme

Kaynakların kullanım amacını daha kolay belirlemek için değerler ekleyebilirsiniz. Örneğin, ortamı ve projeyi listelemek için Etiketler ekleyebilirsiniz. Bir maliyet merkezini veya kaynağa sahip olan takımı tanımlayan etiketler ekleyebilirsiniz. Kuruluşunuz için anlamlı olan tüm değerleri kullanabilirsiniz.

Aşağıdaki örnek, şablonda yapılan değişiklikleri vurgular. Tüm dosyayı kopyalayın ve şablonunuzu içeriğiyle değiştirin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json" range="1-118" highlight="46-52,64,78,102":::

## <a name="deploy-template"></a>Şablon dağıtma

Şablonun dağıtılması ve sonuçlara bakmaları zaman alabilir.

Kaynak grubunu oluşturmadıysanız, bkz. [kaynak grubu oluşturma](template-tutorial-create-first-template.md#create-resource-group). Örnek, `templateFile` [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi, değişkeni şablon dosyası yolu olarak ayarlamış olduğunuzu varsayar.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addtags `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS `
  -webAppName demoapp
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI’nın [en son sürümüne](/cli/azure/install-azure-cli) sahip olmanız gerekir.

```azurecli
az deployment group create \
  --name addtags \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

> [!NOTE]
> Dağıtım başarısız olursa, `verbose` oluşturulan kaynaklarla ilgili bilgi almak için anahtarını kullanın. `debug`Hata ayıklama hakkında daha fazla bilgi edinmek için anahtarını kullanın.

## <a name="verify-deployment"></a>Dağıtımı doğrulama

Kaynak grubunu Azure portal inceleyerek dağıtımı doğrulayabilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol menüden **kaynak grupları**' nı seçin.
1. Dağıttığınız kaynak grubunu seçin.
1. Depolama hesabı kaynağı gibi kaynaklardan birini seçin. Artık etiketlere sahip olduğunu görürsünüz.

   ![Etiketleri göster](./media/template-tutorial-add-tags/show-tags.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiş yapıyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsa, kaynak grubunu silerek dağıttığınız kaynakları temizlemeniz gerekebilir.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kaynaklara Etiketler eklediniz. Sonraki öğreticide, değerleri şablona geçirmeyi kolaylaştırmak için parametre dosyalarını nasıl kullanacağınızı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Parametre dosyası kullan](template-tutorial-use-parameter-file.md)
