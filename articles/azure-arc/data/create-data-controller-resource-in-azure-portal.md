---
title: Erstellen eines Azure Arc-Datencontrollers im Azure-Portal
description: Erstellen eines Azure Arc-Datencontrollers im Azure-Portal
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 297efa83fb1563e3a360f652a6ac1bc2b1679998
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90931613"
---
# <a name="create-an-azure-arc-data-controller-in-the-azure-portal"></a>Erstellen eines Azure Arc-Datencontrollers im Azure-Portal

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="introduction"></a>Einführung

Sie können das Azure-Portal verwenden, um einen Azure Arc-Datencontroller zu erstellen.

Viele der Erstellungsfunktionen für Azure Arc beginnen im Azure-Portal, obwohl sich die zu erstellende oder zu verwaltende Ressource außerhalb der Azure-Infrastruktur befindet. Das Muster für die Benutzerfunktionalität besteht in diesen Fällen darin, das Azure-Portal zu verwenden, um ein Skript zu generieren, das dann in Ihrer Umgebung heruntergeladen und ausgeführt werden kann, um wiederum eine sichere Verbindung mit Azure herzustellen. Dies gilt insbesondere dann, wenn keine direkte Verbindung zwischen Azure und Ihrer Umgebung besteht. Beispielsweise wird bei der [Erstellung von Azure Arc-fähigen Servern](../servers/onboard-portal.md) das folgende Muster befolgt.

Da die Vorschauversion bisher nur den Modus „Indirekt verbunden“ von Azure Arc-fähigen Datendiensten unterstützt, können Sie im Azure-Portal ein Notebook generieren, das dann heruntergeladen und in Azure Data Studio für Ihren Kubernetes-Cluster ausgeführt werden kann. Wenn in Zukunft der Modus „Direkt verbunden“ verfügbar ist, können Sie den Datencontroller direkt vom Azure-Portal aus bereitstellen. Weitere Informationen finden Sie unter [Konnektivitätsmodi](connectivity.md).

## <a name="use-the-azure-portal-to-create-an-azure-arc-data-controller"></a>Erstellen eines Azure Arc-Datencontrollers im Azure-Portal

Führen Sie die folgenden Schritte aus, um mithilfe des Azure-Portals und Azure Data Studio einen Azure Arc-Datencontroller zu erstellen.

1. Melden Sie sich zuerst beim [Marketplace im Azure-Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Marketplace/MarketplaceOffersBlade/selectedMenuItemId/home/searchQuery/azure%20arc%20data%20controller) an.  Die Suchergebnisse im Marketplace werden gefiltert, damit nur Ergebnisse zum Azure Arc-Datencontroller angezeigt werden.
2. Wenn im ersten Schritt nicht die Suchkriterien eingegeben wurden, geben Sie diese ein, wechseln zu den Suchergebnissen und klicken auf den Azure Arc-Datencontroller.
3. Wählen Sie im Marketplace die Kachel des Azure-Datencontrollers aus.
4. Klicken Sie auf die Schaltfläche **Erstellen**.
5. Überprüfen Sie die Anforderungen für die Erstellung eines Azure Arc-Datencontrollers, und installieren Sie fehlende erforderliche Software wie Azure Data Studio und kubectl.
6. Klicken Sie auf die Schaltfläche **Data Controller Details** (Datencontroller-Details).
7. Wählen Sie ein Abonnement, eine Ressourcengruppe und einen Azure-Standort wie für jede andere Ressource aus, die Sie im Azure-Portal erstellen würden. In diesem Fall entspricht der von Ihnen ausgewählte Azure-Standort dem Ort, an dem die Metadaten zur Ressource gespeichert werden.  Die Ressource selbst wird in der von Ihnen gewählten Infrastruktur erstellt. Dabei muss es sich nicht um eine Azure-Infrastruktur handeln.
8. Geben Sie einen Namen für den Datencontroller ein.
9. Derzeit wird in der Vorschau nur der Modus „Indirekt verbunden“ unterstützt.
10. Wählen Sie ein Profil für die Bereitstellungskonfiguration aus.
11. Klicken Sie auf die Schaltfläche **Open in Azure Studio** (In Azure Studio öffnen).
12. Im nächsten Bildschirm werden eine Zusammenfassung Ihrer Auswahl und ein Notebook angezeigt, das generiert wird.  Sie können auf die Schaltfläche **Download provisioning notebook** (Bereitstellungsnotebook herunterladen) klicken, um das Notebook herunterzuladen.
13. Öffnen Sie das Notebook in Azure Data Studio, und klicken Sie oben auf die Schaltfläche **Alle ausführen**.
14. Befolgen Sie die Aufforderungen und Anweisungen im Notebook, um die Erstellung des Datencontrollers abzuschließen.

## <a name="monitoring-the-creation-status"></a>Überwachen des Erstellungsstatus

Das Erstellen des Controllers dauert einige Minuten. Mithilfe der folgenden Befehle können Sie den Status in einem anderen Terminalfenster überwachen:

> [!NOTE]
>  Bei den folgenden Beispielbefehlen wird davon ausgegangen, dass Sie einen Datencontroller und Kubernetes-Namespace mit dem Namen „arc“ erstellt haben.  Wenn Sie einen anderen Namespace-/Datencontrollernamen verwendet haben, können Sie „arc“ durch diesen Namen ersetzen.

```console
kubectl get datacontroller/arc --namespace arc
```

```console
kubectl get pods --namespace arc
```

Sie können auch den Erstellungsstatus eines bestimmten Pods überprüfen, indem Sie einen Befehl wie den folgenden ausführen.  Dies ist besonders bei der Problembehandlung hilfreich.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/control-2g7bl --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Beheben von Problemen bei der Erstellung

Wenn Probleme bei der Erstellung auftreten, finden Sie weitere Informationen im [Handbuch zur Problembehandlung](troubleshoot-guide.md).
