---
# required metadata

title: Installieren des MIM 2016&#58; MIM-Diensts und -Portals | Microsoft Identity Manager
description: Hier finden Sie die Schritte zum Konfigurieren und Installieren Erste Schritte zum Konfigurieren und Installieren des MIM-Diensts und -Portals für Microsoft Identity Manager 2016
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Installieren von MIM 2016: MIM-Dienst und -Portal

>[!div class="step-by-step"] 
[« MIM-Synchronisierungsdienst](install-mim-sync.md)
[Datenbanken synchronisieren»](install-mim-sync-ad-service.md)

> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Domänencontrollername: **mimservername**
> - Domänenname: **contoso**
> - Kennwort: **Pass@word1**
> - Name des Dienstkontos: **MIMService**

Falls Sie im letzten Schritt das MIM-Installationspaket nicht eingerichtet haben, kehren Sie zurück, und installieren Sie die Komponenten von Microsoft Identity Manager 2016, bevor Sie fortfahren:


## Konfigurieren des MIM-Diensts und -Portals für die Installation

1. Führen Sie das **Installationsprogramm für den MIM-Dienst und das -Portal** aus dem entpackten Unterordner **Dienst und Portal** aus.

2. Klicken Sie im Begrüßungsbildschirm auf **Weiter**.

3. Lesen Sie die Microsoft-Software-Lizenzbedingungen, und klicken Sie, sofern Sie die Lizenzbedingungen akzeptieren, auf **Weiter**.

4. Klicken Sie im MIM-Bildschirm für das **Programm zur Verbesserung der Benutzerfreundlichkeit** auf **Weiter**.

5. Stellen Sie beim Auswählen der Komponentenfunktionen für diese Bereitstellung sicher, dass Sie die MIM-Dienst- (mit Ausnahme von MIM-Berichterstellung) und die MIM-Portalfunktionen einschließen. Sie können auch das MIM-Kennwortzurücksetzungsportal und den MIM-Benachrichtigungsdienst für Kennwortänderungen auswählen.

6. Wählen Sie auf der Seite **Konfigurieren der MIM-Datenbankverbindung** die Option **Neue Datenbank erstellen** aus.

    ![Bild: Konfigurieren der MIM-Datenbankverbindung](media/MIM-Install10.png)

7. Geben Sie bei **Konfigurieren der E-Mail-Server-Verbindung**, den Namen Ihres Exchange-Servers als **Mailserver** an. Falls Sie keinen E-Mail-Server konfiguriert haben, verwenden Sie **localhost** als Namen für den E-Mail-Server, und deaktivieren Sie die oberen zwei Kontrollkästchen. Klicken Sie auf **Weiter**.

    ![Bild: Konfigurieren der E-Mail-Server-Verbindung](media/MIM-Install11.png)

8. Geben Sie an, dass Sie ein neues selbstsigniertes Zertifikat generieren möchten, oder wählen Sie das entsprechende Zertifikat.

9. Geben Sie den zu verwendenden Dienstkontonamen an, z.B. *MIMService*, sowie das Dienstkontokennwort, z.B. *Pass@word1*, Ihre Dienstkontodomäne, z.B. *contoso*, und das Dienst-E-Mail-Konto, z.B. *contoso*.

    ![Bild: Konfigurieren des MIM-Dienstkontos](media/MIM-Install12.png)

10. Beachten Sie, dass eine Warnung angezeigt werden kann, dass das Dienstkonto in seiner aktuellen Konfiguration nicht sicher ist.

11. Akzeptieren Sie die Standardeinstellungen für den Standort des Synchronisierungsservers, und geben Sie für das Konto „MIM-Verwaltungs-Agent“ *contoso\MIMsync* an.

    ![Bild: Konfigurieren des MIM-Diensts und -Portals](media/MIM-Install13.png)

12. Geben Sie *CORPIDM* (den Namen dieses Computers) als MIM-Dienstserveradresse für das MIM-Portal an.

13. Geben Sie *http://CorpIDM.contoso.local:82* als URL für die SharePoint-Websitesammlung an.

14. Geben Sie *http://CorpIDM.contoso.local:8080* als die URL zur Kennwortregistrierung an.

15. Geben Sie *http://CorpIDM.contoso.local:8088* als die URL zur Kennwortzurücksetzung an.

16. Aktivieren Sie das Kontrollkästchen, um die Ports 5725 und 5726 in der Firewall zu öffnen, sowie das Kontrollkästchen, um allen authentifizierten Benutzern den Zugriff auf das MIM-Portal zu gewähren.

## Konfigurieren des MIM- Kennwort-Registrierungsportals

1.  Legen Sie den Dienstkontonamen für die SSPR-Registrierung auf *contoso\MIMSSPR* und das zugehörige Kennwort auf *Pass@word1* fest.

2.  Geben Sie *CORPIDM* als den Hostnamen für die MIM-Kennwortregistrierung an, und legen Sie den Port auf **8080** fest. Aktivieren Sie die Option **Port in der Firewall öffnen**.

    ![Geben Sie die Konfigurationsinformationen ein, die vom IIS-Image verwendet werden](media/MIM-Install14.png)

3.  Eine Warnung wird angezeigt – lesen Sie sie, und klicken Sie auf **Weiter**.

4. Geben Sie im nächsten Konfigurationsbildschirm des MIM-Kennwortregistrierungsportals *http://CorpIDM.contoso.local* als Serveradresse für den MIM-Dienst für das Kennwortzurückregistrierungsportal an.

## Konfigurieren des MIM-Kennwortzurücksetzungsportals

1.  Legen Sie den Namen des Dienstkontos für die SSPR-Registrierung auf *Contoso\MIMSSPRService* und das Kennwort auf *Pass@word1* fest.

2.  Geben Sie *CORPIDM* als den Hostnamen für die MIM-Kennwortregistrierung an, und legen Sie den Port auf **8080** fest. Aktivieren Sie die Option **Port in der Firewall öffnen**.

    ![Geben Sie die Konfigurationsinformationen ein, die vom IIS-Image verwendet werden](media/MIM-Install15.png)

3.  Eine Warnung wird angezeigt – lesen Sie sie, und klicken Sie auf **Weiter**.

4. Geben Sie im nächsten Bildschirm der Konfiguration des MIM-Kennwortregistrierungsportals *CorpIDname http://CorpIDname.domain.local* an, als Serveradresse für den MIM-Dienst für das Kennwortzurücksetzungsportal an.

## Installieren des MIM-Diensts und -Portals

Wenn alle Vorinstallationsdefinitionen bereit sind, klicken Sie auf **Installieren**, um mit der Installation der ausgewählten **Dienst- und Portal**-Komponenten zu beginnen.

Nach Abschluss der Installation stellen Sie sicher, dass das MIM-Portal aktiv ist.

1. Starten Sie den Internet Explorer, und stellen Sie über *http://corpidm.contoso.local:82/identitymanagement* eine Verbindung mit dem MIM-Portal her. Beachten Sie, dass es beim ersten Besuch der Seite möglicherweise zu einer kurzen Verzögerung kommen kann.

    - Authentifizieren Sie sich in Internet Explorer bei Bedarf als *contoso\Administrator*.

2. Öffnen Sie in Internet Explorer die **Internetoptionen**, wechseln Sie zur Registerkarte **Sicherheit** , und fügen Sie die Website zur Zone **Lokales Intranet** hinzu, wenn sie noch nicht vorhanden ist.  Schließen Sie das Dialogfeld **Internetoptionen** .

3. Ermöglichen Sie Benutzern die Anzeige ihres eigenen Eintrags in MIM.

    1.  Klicken Sie unter Verwendung von Internet Explorer im **MIM-Portal**auf **Management-Richtlinienregeln**.

    2.  Suchen Sie nach der Verwaltungsrichtlinienregel **Benutzerverwaltung: Benutzer können eigene Attribute lesen**.

    3.  Wählen Sie diese Verwaltungsrichtlinienregel aus, und deaktivieren Sie **Richtlinie ist deaktiviert**.

    4.  Klicken Sie auf **OK** , und klicken Sie dann auf **Absenden**.

4.  Stellen Sie sicher, dass die Firewall eingehende Verbindungen an die TCP-Ports 5725 und 5726 zulässt.

    1.  Starten Sie **Verwaltung » Windows-Firewall** mit **Erweiterter Sicherheit**.

    2.  Klicken Sie auf **Eingehende Regeln**.

    3.  Stellen Sie sicher, dass die folgenden zwei Regeln angezeigt werden:

        -   Forefront Identity Manager-Dienst (STS).

        -   Forefront Identity Manager-Dienst (Webservice).

    4.  Schließen Sie den Assistenten ab, und schließen Sie die Anwendung **Windows-Firewall** .

    5.  Starten Sie **Systemsteuerung » Netzwerk und Internet » Netzwerkstatus und -aufgaben anzeigen**.

    6.  Stellen Sie sicher, dass ein aktives Netzwerk als "contoso.local" als Domänennetzwerk aufgeführt wird.

    7.  Schließen Sie die **Systemsteuerung**.

> [!NOTE] Optional: An dieser Stelle können Sie MIM-Add-Ins und -Erweiterungen installieren.

>[!div class="step-by-step"]  
[« MIM-Synchronisierungsdienst](install-mim-sync.md)
[Datenbanken synchronisieren »](install-mim-sync-ad-service.md)


<!--HONumber=May16_HO3-->


