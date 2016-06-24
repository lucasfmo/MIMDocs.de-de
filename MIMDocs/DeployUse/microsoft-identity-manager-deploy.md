---
# required metadata

title: Bereitstellen von MIM 2016 | Microsoft Identity Manager
description: Hier erhalten Sie die vollständige Liste der für die Bereitstellung von Microsoft Identity Manager 2016 notwendigen Schritte, von der Vorbereitung der Umgebung bis zur Konfiguration der Portale.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Bereitstellen MIM 2016
Die Artikel in diesem Abschnitt bieten Schritt-für-Schritt-Anleitungen für die Bereitstellung von Microsoft Identity Manager (MIM) 2016 für Endbenutzer-Self-Service-Szenarien. Es wird dabei von einer Bereitstellung auf einem neuen Server ausgegangen, auf dem zuvor weder FIM noch MIM bereitgestellt wurden.

> [!NOTE]
> Die in diesem Abschnitt beschriebene Bereitstellungstopologie ist nur für die ersten Schritte und das Kennenlernen von MIM vorgesehen.  Die [Anleitung zur Kapazitätsplanung](/microsoft-identity-manager/plan-design/capacity-planning-guide) bietet weitere Informationen zu Topologien für Produktionsbereitstellungen.  Es wird empfohlen, dass Sie diese Dokumentation vor der Bereitstellung von MIM für die Produktion lesen.

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## Erstens: Vorbereiten einer Domäne
MIM funktioniert über Active Directory (AD). Führen Sie daher diese Schritte aus, um Ihre Active Directory-Domänencontroller zu konfigurieren.
- [Einrichten der Domäne](preparing-domain.md)

## Nächster Schritt: Vorbereiten eines Servers für die Identitätsverwaltung
Sobald Ihre Domäne eingerichtet und konfiguriert ist, bereiten Sie den Server vor, der Ihre Corporate Identity verwaltet. Dies beinhaltet:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (optional)

## Installieren Sie zu guter Letzt die Komponenten von Microsoft Identity Manager 2016.
Nachdem Sie die Domäne und den Server eingerichtet haben, können Sie die MIM-Komponenten installieren und konfigurieren, um sie mit Active Directory zu synchronisieren.
- [MIM Synchronization Service](install-mim-sync.md)
- [MIM-Dienst und -Portal](install-mim-service-portal.md)
- [Synchronisieren von Active Directory mit MIM-Dienstdatenbanken](install-mim-sync-ad-service.md)


<!--HONumber=May16_HO3-->


