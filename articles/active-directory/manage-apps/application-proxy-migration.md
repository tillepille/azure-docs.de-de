---
title: Durchführen eines Upgrades auf den Azure AD-Anwendungsproxy | Microsoft-Dokumentation
description: Wählen Sie aus, welche Proxylösung am besten für Sie geeignet ist, um ein Upgrade von Microsoft Forefront oder Unified Access Gateway durchzuführen.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: kenwith
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: cccabaf069a3027e615892e36e218f865a6c983a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90706661"
---
# <a name="compare-remote-access-solutions"></a>Vergleichen von Remotezugriffslösungen

Der Azure Active Directory-Anwendungsproxy ist eine der beiden von Microsoft angebotenen Remotezugriffslösungen. Das andere ist Webanwendungsproxy, die lokale Version. Diese beiden Lösungen ersetzen frühere Produkte, die Microsoft angeboten hat: Microsoft Forefront Threat Management Gateway (TMG) und Unified Access Gateway (UAG). In diesem Artikel erhalten Sie einen Vergleich dieser vier Lösungen. Benutzer, die weiterhin die veralteten Lösungen TMG oder UAG verwenden, können mithilfe dieses Artikels ihre Migration zu einem der Anwendungsproxys planen. 


## <a name="feature-comparison"></a>Funktionsvergleiche

Verwenden Sie diese Tabelle zum Vergleich von Threat Management Gateway (TMG), Unified Access Gateway (UAG), Webanwendungsproxy (WAP) und Azure AD-Anwendungsproxy (AP).

| Funktion | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Zertifikatauthentifizierung | Ja | Ja | - | - |
| Ausgewählte Veröffentlichung von Browser-Apps | Ja | Ja | Ja | Ja |
| Vorauthentifizierung und einmaliges Anmelden | Ja | Ja | Ja | Ja | 
| Layer 2/3-Firewall | Ja | Ja | - | - |
| Weiterleitungs-Proxyfunktionen | Ja | - | - | - |
| VPN-Funktionen | Ja | Ja | - | - |
| Umfangreiche Protokollunterstützung | - | Ja | Ja, bei Ausführung über HTTP | Ja, bei Ausführung über HTTP oder über Remotedesktopgateway |
| Dient als AD FS-Proxyserver | - | Ja | Ja | - |
| Ein Portal für den Anwendungszugriff | - | Ja | - | Ja |
| Übersetzung von Antworttext in Link | Ja | Ja | - | Ja | 
| Authentifizierung mit Headern | - | Ja | - | Ja, mit PingAccess | 
| Sicherheit auf Cloud-Ebene | - | - | - | Ja | 
| Bedingter Zugriff | - | Ja | - | Ja |
| Keine Komponenten in der demilitarisierten Zone (DMZ) | - | - | - | Ja |
| Keine eingehenden Verbindungen | - | - | - | Ja |

In den meisten Szenarien empfiehlt sich Azure AD-Anwendungsproxy als moderne Lösung. Webanwendungsproxy wird nur in Szenarien bevorzugt, bei denen ein Proxyserver für AD FS erforderlich ist. Außerdem können Sie in Azure Active Directory keine benutzerdefinierten Domänen verwenden. 

Azure AD-Anwendungsproxy bietet besondere Vorteile im Vergleich zu ähnlichen Produkten, darunter:

- Erweitern von Azure AD um lokale Ressourcen
   - Sicherheit und Schutz auf Cloud-Ebene
   - Einfach zu aktivierende Features wie bedingter Zugriff und mehrstufige Authentifizierung
- Keine Komponenten in der demilitarisierten Zone
- Keine Notwendigkeit eingehender Verbindungen
- Eine Seite „Meine Apps“, über die Ihre Benutzer all ihre Anwendungen erreichen, darunter Microsoft 365, in Azure AD integrierte SaaS-Apps und Ihre lokalen Web-Apps. 


## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen eines sicheren Remotezugriffs auf lokale Anwendungen mit Azure AD-Anwendungsproxy](application-proxy.md)
