---
# required metadata

title: Hybride Identitätsverwaltungsberichte | Microsoft Identity Manager
description: Mit der Azure Active Directory-Hybridberichterstellung können Sie benutzerdefinierte Berichte erstellen, die sowohl Cloud- als auch lokale Ereignisse enthalten.
keywords:
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Hybride Identitätsverwaltungsberichte in Azure
Mit Azure Active Directory (AD) können Sie einen einzigen Bericht erstellen, um Identitätsverwaltungsaktivitäten zu überwachen, die entweder lokal oder in der Cloud erfolgen. Über diese Funktionalität können Sie sämtliche Ihrer Identitäts- und Zugriffsdaten an einem Ort verwalten, wodurch Zeit gespart wird und die Gesamtkosten verringert werden.

## Was ist die Azure AD-Hybridberichterstellung?
Die Hybridberichterstellung hilft IT-Experten dabei, allgemeine Herausforderungen bei der Berichterstellung für die Identitätsverwaltung zu meistern.

1. **Sammeln von Identitätsverwaltungsaktivitäten über verschiedene Systeme hinweg.** In Hybridberichten sehen Sie die Identitätsverwaltungsaktivitäten von Azure AD und Identity Manager.

2. **Exportieren von Berichtsdaten und Erstellen von benutzerdefinierten Berichten.** Zusätzlich zum Anzeigen der Berichte im Azure-Portal können Sie die Daten exportieren, um Ihre eigenen benutzerdefinierten Ansichten zu generieren.

3. **Verringern der Kosten für die Infrastruktur des Berichterstellungssystems.** Hybridberichterstellung in die Cloud bedeutet, dass Sie keine lokale Data Warehouse-Infrastruktur für die Berichterstellung mehr benötigen.

## Wie funktioniert es?

Um die lokalen Daten zu sammeln, installieren Sie zunächst einen Berichterstellungs-Agent auf dem Identity Manager-Server. Der Berichterstellungs-Agent wird von der Konfigurationsseite Ihres Verzeichnisses im [klassischen Azure-Portal](https://manage.windowsazure.com/) heruntergeladen.

Für die Hybridberichterstellung werden die folgenden Schritte ausgeführt:
1. Nachdem der Berichterstellungs-Agent installiert ist, werden die Identity Manager-Aktivitätsdaten an das Windows-Ereignisprotokoll gesendet.
2. Der Berichterstellungs-Agent verarbeitet die Ereignisse im Windows-Ereignisprotokoll und lädt diese nach Azure hoch.
3. Die Aktivitätsdaten werden in Azure für einen Monat gespeichert.
4. Wenn Sie einen Bericht anfordern, werden die Aktivitätsereignisse analysiert und für die gewünschten Berichte gefiltert.
5. Im klassischen Azure-Portal werden die Berichtsdaten abgerufen und als Aktivitätsbericht angezeigt.

## Weitere Informationen:
- Lesen Sie weitere Details zum [Arbeiten mit der Identity Manager-Hybridberichterstellung](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting).


<!--HONumber=May16_HO3-->


