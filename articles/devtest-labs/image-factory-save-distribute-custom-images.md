---
title: Speichern und Verteilen von Images in Azure DevTest Labs | Microsoft-Dokumentation
description: In diesem Artikel werden die Schritte erläutert, mit denen Sie benutzerdefinierte Images bereits erstellter virtueller Computer (VMs) in Azure DevTest Labs speichern können.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: a5278626f8cdd4299912f3c952786422436fe916
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85476239"
---
# <a name="save-custom-images-and-distribute-to-multiple-labs"></a>Speichern von benutzerdefinierten Images und Verteilen an mehrere Labs
In diesem Artikel werden die Schritte erläutert, mit denen Sie benutzerdefinierte Images bereits erstellter virtueller Computer (VMs) speichern können. Ferner wird erläutert, wie diese benutzerdefinierten Images an andere DevTest Labs in der Organisation verteilt werden können.

## <a name="prerequisites"></a>Voraussetzungen
Folgende Elemente sollten bereits vorhanden sein:

- Ein Lab für die Image Factory in Azure DevTest Labs
- Ein Azure DevOps-Projekt, das für die Automatisierung der Image Factory verwendet wird
- Ein Quellcode-Speicherort mit den Skripts und der Konfiguration (in unserem Beispiel in dem im vorherigen Schritt erwähnten DevOps-Projekt)
- Eine Builddefinition, um Azure Powershell-Aufgaben zu orchestrieren

Führen Sie bei Bedarf die Schritte durch, die im Artikel [Run an image factory from Azure DevOps (Ausführen einer Image Factory aus Azure DevOps)](image-factory-set-up-devops-lab.md) beschrieben werden, um diese Elemente zu erstellen bzw. einzurichten. 

## <a name="save-vms-as-generalized-vhds"></a>Speichern von virtuellen Computern als generalisierte virtuelle Festplatten
Speichern Sie die vorhandenen virtuellen Computer als generalisierte virtuelle Festplatten (VHDs).  Es ist ein Beispiel-PowerShell-Skript vorhanden, mit dem die vorhandenen virtuellen Computer als generalisierte virtuelle Festplatten gespeichert werden können. Um das Skript zu verwenden, fügen Sie eine weitere **Azure Powershell**-Aufgabe zur Builddefinition hinzu, wie dies in der folgenden Abbildung gezeigt wird:

![Hinzufügen des Azure PowerShell-Schritts](./media/save-distribute-custom-images/powershell-step.png)

Sobald die neue Aufgabe in der Liste angezeigt wird, wählen Sie das Element aus, damit alle Details wie in der folgenden Abbildung gezeigt ausgefüllt werden können: 

![PowerShell-Einstellungen](./media/save-distribute-custom-images/powershell-settings.png)


## <a name="generalized-vs-specialized-custom-images"></a>Generalisierte vs. spezialisierte benutzerdefinierte Images
Wenn Sie im [Azure-Portal](https://portal.azure.com) ein benutzerdefiniertes Image eines virtuellen Computers erstellen, können Sie zwischen einem generalisierten oder einem spezialisierten benutzerdefinierten Image wählen.

- **Spezialisiertes benutzerdefiniertes Image:** Sysprep bzw. die Bereitstellungsaufhebung wurde NICHT auf dem Computer ausgeführt. Dies bedeutet, dass das Image eine exakte Kopie des Betriebssystemdatenträgers auf dem vorhandenen virtuellen Computer ist (d. h. eine Momentaufnahme).  Wenn wir einen neuen Computer aus diesem benutzerdefinierten Image erstellen, sind die gleichen Dateien, Anwendungen, Benutzerkonten und derselbe Computername usw. vorhanden.
- **Generalisiertes benutzerdefiniertes Image:** Sysprep bzw. die Bereitstellungsaufhebung wurde auf dem Computer ausgeführt.  Wenn dieser Prozess ausgeführt wird, werden Benutzerkonten, der Name des Computers, die Registrierungsstrukturen von Benutzern usw. entfernt. Dadurch soll das Image generalisiert werden, damit es angepasst werden kann, wenn Sie einen anderen virtuellen Computer erstellen.  Wenn Sie einen virtuellen Computer (durch Ausführen von Sysprep) generalisieren, zerstört dieser Prozess den aktuellen virtuellen Computer. Er ist dann nicht mehr funktionstüchtig.

Das Skript für das Andocken benutzerdefinierter Images in der Image Factory speichert VHDs für alle virtuellen Computer, die im vorherigen Schritt erstellt wurden. (Diese werden anhand eines Tags an der Ressource in Azure identifiziert.)

## <a name="update-configuration-for-distributing-images"></a>Aktualisieren der Konfiguration für die Verteilung von Images
Der nächste Prozessschritt besteht darin, die benutzerdefinierten Images mithilfe von Push aus dem Image Factory-Lab an jedes andere Lab zu übertragen, in dem sie benötigt werden. Zentrales Element dieser Prozesses ist die **labs.json**-Konfigurationsdatei. Sie finden diese Datei im Ordner **Configuration** der Image Factory.

Die labs.json-Konfigurationsdatei enthält zwei wichtige Informationen:

- Die eindeutige Identifizierung eines bestimmten Ziel-Labs mithilfe der Abonnement-ID und des Lab-Namens.
- Die konkreten Images, die mithilfe von Push als relative Pfade zum Konfigurationsstamm an das Lab übertragen werden sollen. Sie können auch den gesamten Ordner angeben (um alle Images in diesem Ordner abzurufen).

Das nachstehende Beispiel zeigt eine labs.json-Datei mit zwei Labs. In diesem Fall verteilen Sie Images an zwei verschiedene Labs.

```json
{
   "Labs": [
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2012R2",
               "Win2016/Datacenter.json"
         ]
      },
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2016/Datacenter.json"
         ]
      }
   ]
}
```

## <a name="create-a-build-task"></a>Erstellen einer Buildaufgabe
Mit denselben Schritten, die weiter oben in diesem Artikel beschrieben wurden, fügen Sie eine zusätzliche **Azure Powershell**-Buildaufgabe zu Ihrer Builddefinition hinzu. Geben Sie die Details wie in der folgenden Abbildung gezeigt ein: 

![Buildaufgaben für die Verteilung von Images](./media/save-distribute-custom-images/second-build-task-powershell.png)

Die Parameter lauten: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -SubscriptionId $(SubscriptionId) -DevTestLabName $(DevTestLabName) -maxConcurrentJobs 20`

Diese Aufgabe überträgt alle benutzerdefinierten Images, die in der Image Factory vorhanden sind, mithilfe von Push an alle in der Datei Labs.json definierten Labs.

## <a name="queue-the-build"></a>Einstellen des Builds in die Warteschlange
Sobald die Buildaufgabe für die Verteilung abgeschlossen ist, stellen Sie einen neuen Build in die Warteschlange, um sicherzustellen, dass alles funktioniert. Sobald der Build erfolgreich abgeschlossen wurde, werden die neuen benutzerdefinierten Images in dem Ziel-Lab angezeigt, das in der Labs.json-Konfigurationsdatei eingegeben wurde.

## <a name="next-steps"></a>Nächste Schritte
Im nächsten Artikel dieser Reihe aktualisieren Sie die Image Factory mit einer Aufbewahrungsrichtlinie und Bereinigungsschritten: [Set retention policy and run cleanup scripts (Festlegen der Aufbewahrungsrichtlinie und Ausführen von Bereinigungsskripts)](image-factory-set-retention-policy-cleanup.md).
