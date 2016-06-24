---
# required metadata

title: Arbeiten mit der Identity Manager-Hybridberichterstellung | Microsoft Identity Manager
description: Erfahren Sie, wie Sie lokale Daten und Clouddaten als Hybridberichte in Azure kombinieren und wie Sie diese Berichte verwalten und anzeigen können.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Arbeiten mit der Identity Manager-Hybridberichterstellung

## Verfügbare Hybridberichte
Die ersten drei in Azure AD verfügbaren Microsoft Identity Manager-Berichte (MIM) sind die **Aktivität „Zurücksetzen des Kennworts“**, die **Registrierung für das Zurücksetzen des Kennworts** und die **Self-Service-Gruppenaktivität**.

-   Die Aktivität „Zurücksetzen des Kennworts“ zeigt jede Instanz an, wenn ein Benutzer mithilfe von SSPR die Kennwortzurücksetzung durchgeführt hat und die für die Authentifizierung verwendeten Gates oder **Methoden** bereitstellt.

    ![Bild: Azure-Hybridberichterstellung – Aktivität „Zurücksetzen des Kennworts“](media/MIM-Hybrid-passwordreset.jpg)

-   Die Aktivität „Registrierung für Zurücksetzen des Kennworts“ wird jedes Mal angezeigt, wenn sich ein Benutzer für SSPR registriert und die **Methoden** zum Authentifizieren verwendet wurden, z. B. eine Mobiltelefonnummer oder Fragen und Antworten.
    Beachten Sie, dass bei der Registrierung für das Zurücksetzen des Kennworts kein Unterschied zwischen SMS-Gate und MFA-Gate gemacht wird. Beide werden als **Mobiltelefon**betrachtet.

-   Die Self-Service-Gruppenaktivität zeigt jeden Versuch einer Person an, sich selbst einer Gruppe hinzuzufügen oder sich aus einer Gruppe und einer Gruppenerstellung zu löschen.

> [!NOTE]
> Die Berichte stellen derzeit maximal die Daten für bis zu einem Monat dar.
>
> Wenn Sie Hybridberichte deinstallieren möchten, deinstallieren Sie den Agent „MIMreportingAgent.msi“.

## Voraussetzungen

1.  Installieren Sie Microsoft Identity Manager 2016 einschließlich des MIM-Diensts.

2.  Stellen Sie sicher, dass Sie über einen Azure AD Premium-Mandanten mit lizenziertem Administrator in Ihrem Verzeichnis verfügen.

3.  Stellen Sie sicher, dass Sie über eine ausgehende Internetverbindung vom Microsoft Identity Manager-Server zu Azure verfügen.

## Installieren der Microsoft Identity Manager-Berichterstellung in Azure AD
Nachdem der Agent für die Berichterstellung installiert wurde, werden die Daten der Microsoft Identity Manager-Aktivität von MIM in das Windows-Ereignisprotokoll exportiert. Der MIM-Berichterstellungs-Agent verarbeitet die Ereignisse und lädt diese in Azure hoch. Die Ereignisse werden in Azure hinsichtlich der erforderlichen Berichte analysiert, entschlüsselt und gefiltert.

1.  Installieren Sie Microsoft Identity Manager 2016.

2.  Laden Sie die Microsoft Identity Manager-Agents für die Berichterstellung herunter:

    1.  Melden Sie sich beim Azure AD-Verwaltungsportal an, und klicken Sie auf das Symbol für Active Directory.

    2.  Doppelklicken Sie auf das Verzeichnis, für das Sie als globaler Administrator angemeldet und ein Azure AD Premium-Abonnement besitzen.

    3.  Klicken Sie auf **Konfiguration** , und laden Sie den Agent für die Berichterstellung herunter.

3.  Installieren Sie den Microsoft Identity Manager-Agent für die Berichterstellung:

    1.  Erstellen Sie ein Verzeichnis auf dem Computer.

    2.  Extrahieren Sie die Dateien `MIMHybridReportingAgent.msi` und `tenant.cert` in diesem Verzeichnis.

    3.  Führen Sie das Installationsprogramm des Agents aus.

    4.  Stellen Sie sicher, dass der Agent-Dienst für die MIM-Berichterstellung ausgeführt wird

    5.  Starten Sie den MIM-Dienst neu.

4.  Überprüfen Sie, ob die Microsoft Identity Manager-Berichterstellung in Azure funktioniert.

    Sie können die Berichtsdaten über das Self-Service-Portal zum Zurücksetzen von Kennwörtern von Microsoft Identity Manager erstellen, um das Kennwort eines Benutzers zurückzusetzen. Stellen Sie sicher, dass das Zurücksetzen des Kennworts erfolgreich abgeschlossen wurde, und überprüfen Sie dann, ob die Daten im Azure AD-Verwaltungsportal angezeigt werden.

## Anzeigen von Hybridberichten im klassischen Azure-Portal

1.  Melden Sie sich mit Ihrem globalen Administratorkonto für den Mandanten beim [klassischen Azure-Portal](https://manage.windowsazure.com/) an.

2.  Klicken Sie auf das **Active Directory** -Symbol.

3.  Wählen Sie das Mandantenverzeichnis aus der Liste der verfügbaren Verzeichnisse für Ihr Abonnement aus.

4.  Klicken Sie auf **Berichte** und dann auf die **Aktivität „Zurücksetzen des Kennworts“**.

5.  Stellen Sie sicher, dass Sie **Identity Manager** im Dropdownmenü der Quelle auswählen.

> [!WARNING]
> Es kann einige Zeit dauern, bis Microsoft Identity Manager-Daten in Azure AD angezeigt werden.

## Beenden der Hybridberichterstellung
Falls Sie das Hochladen von Berichtsdaten von Microsoft Identity Manager in Azure Active Directory beenden möchten, deinstallieren Sie den Agent für die Hybridberichterstellung. Deinstallieren Sie die Microsoft Identity Manager-Hybridberichterstellung mithilfe des Windows-Tools **Programme hinzufügen oder entfernen**.

## Für die Hybridberichterstellung verwendete Windows-Ereignisse
Von Microsoft Identity Manager generierte Ereignisse werden im Windows-Ereignisprotokoll protokolliert und in der Ereignisanzeige unter „Anwendungs- und Dienstprotokolle“-&gt; **Identity Manager-Anforderungsprotokoll** angezeigt. Jede MIM-Anforderung wird als Ereignis in das Windows-Ereignisprotokoll in der JSON-Struktur exportiert. Dieses kann in Ihr SIEM exportiert werden.

|Ereignistyp|ID|Ereignisdetails|
|--------------|------|-----------------|
|Informationen|4121|Die MIM-Ereignisdaten, die alle Anforderungsdaten beinhalten.|
|Informationen|4137|Die Erweiterung des MIM-Ereignisses 4121, für den Fall, dass für ein einzelnes Ereignis zu viele Daten vorliegen. Der Header in diesem Ereignis liegt in der folgenden Form vor: `"Request: <GUID> , message <xxx> out of <xxx>`|


<!--HONumber=Apr16_HO2-->


