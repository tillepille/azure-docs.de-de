---
title: Erstellen eines Azure App Configuration-Speichers per ARM-Vorlage (Azure Resource Manager)
titleSuffix: Azure App Configuration
description: Es wird beschrieben, wie Sie einen Azure App Configuration-Speicher per ARM-Vorlage (Azure Resource Manager) erstellen.
author: ZhijunZhao
ms.author: zhijzhao
ms.date: 10/16/2020
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.openlocfilehash: feabac62564729338e41bf30eaf8d9f5a6317126
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148998"
---
# <a name="quickstart-create-an-azure-app-configuration-store-by-using-an-arm-template"></a>Schnellstart: Erstellen eines Azure App Configuration-Speichers per ARM-Vorlage

In dieser Schnellstartanleitung wird Folgendes beschrieben:

- Bereitstellen eines App Configuration-Speichers mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage).
- Erstellen von Schlüsselwerten in einem App Configuration-Speicher per ARM-Vorlage.
- Lesen von Schlüsselwerten in einem App Configuration-Speicher per ARM-Vorlage.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen** . Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store-kv%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-app-configuration-store-kv/). Hiermit wird ein neuer App Configuration-Speicher erstellt, der zwei Schlüsselwerte enthält. Anschließend wird die Funktion `reference` verwendet, um die Werte der beiden Schlüsselwertressourcen auszugeben. Wenn der Wert des Schlüssels auf diese Weise gelesen wird, kann er auch an anderen Stellen der Vorlage verwendet werden.

In der Schnellstartanleitung wird das Element `copy` verwendet, um mehrere Instanzen der Schlüsselwertressource zu erstellen. Weitere Informationen zum Element `copy` finden Sie unter [Ressourceniteration in ARM-Vorlagen](../azure-resource-manager/templates/copy-resources.md).

> [!IMPORTANT]
> Für diese Vorlage ist ein App Configuration-Ressourcenanbieter mit Version `2020-07-01-preview` oder höher erforderlich. In dieser Version wird die Funktion `reference` zum Lesen von Schlüsselwerten verwendet. Die Funktion `listKeyValue`, die in der vorherigen Version zum Lesen von Schlüsselwerten verwendet wurde, ist ab Version `2020-07-01-preview` nicht mehr verfügbar.

:::code language="json" source="~/quickstart-templates/101-app-configuration-store-kv/azuredeploy.json":::

Zwei Azure-Ressourcen sind in der Vorlage definiert:

- [Microsoft.AppConfiguration/configurationStores](/azure/templates/microsoft.appconfiguration/2020-06-01/configurationstores): Erstellen eines App Configuration-Speichers
- Microsoft.AppConfiguration/configurationStores/keyValues: Erstellen eines Schlüsselwerts im App Configuration-Speicher

> [!NOTE]
> Der Name der Ressource `keyValues` ist eine Kombination aus Schlüssel und Bezeichnung. Der Schlüssel und die Bezeichnung werden mit dem Trennzeichen `$` verknüpft. Die Bezeichnung ist optional. Im obigen Beispiel wird von der Ressource `keyValues` mit dem Namen `myKey` ein Schlüsselwert ohne Bezeichnung erstellt.
>
> Bei der Prozentcodierung, die auch als URL-Codierung bezeichnet wird, können Schlüssel oder Bezeichnungen Zeichen enthalten, die in Ressourcennamen von ARM-Vorlagen nicht zulässig sind. Da auch `%` kein zulässiges Zeichen ist, wird stattdessen `~` verwendet. Führen Sie die folgenden Schritte aus, um einen Namen richtig zu codieren:
>
> 1. Anwenden der URL-Codierung
> 2. Ersetzen Sie `~` durch `~7E`.
> 3. Ersetzen Sie `%` durch `~`.
>
> Um beispielsweise ein Schlüssel-Wert-Paar mit dem Schlüsselnamen `AppName:DbEndpoint` und der Bezeichnung `Test` zu erstellen, sollte der Ressourcenname `AppName~3ADbEndpoint$Test` lauten.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Klicken Sie auf das folgende Bild, um sich bei Azure anzumelden und eine Vorlage zu öffnen. Mit der Vorlage wird ein App Configuration-Speicher erstellt, der zwei Schlüsselwerte enthält.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store-kv%2Fazuredeploy.json)

Sie können die Vorlage auch mit dem folgenden PowerShell-Cmdlet bereitstellen. Die Schlüsselwerte sind in der Ausgabe der PowerShell-Konsole enthalten.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-app-configuration-store-kv/azuredeploy.json"

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

Read-Host -Prompt "Press [ENTER] to continue ..."
```

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Geben Sie im Azure-Portal im Suchfeld den Suchbegriff **App Configuration** ein. Wählen Sie in der Liste den Eintrag **App Configuration** aus.
1. Wählen Sie die neu erstellte App Configuration-Ressource aus.
1. Klicken Sie unter **Vorgänge** auf **Konfigurations-Explorer** .
1. Vergewissern Sie sich, dass zwei Schlüsselwerte vorhanden sind.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den App Configuration-Speicher und alle zugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Falls Sie planen, den App Configuration-Speicher weiter zu nutzen, können Sie das Löschen überspringen. Wenn Sie diesen Speicher nicht weiter verwenden möchten, löschen Sie alle in diesem Schnellstart erstellten Ressourcen, indem Sie das folgende Cmdlet ausführen:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem folgenden Artikel fort, um sich über die Erstellung anderer Anwendungen mit Azure App Configuration zu informieren:

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen einer ASP.NET Core-App mit Azure App Configuration](quickstart-aspnet-core-app.md)
