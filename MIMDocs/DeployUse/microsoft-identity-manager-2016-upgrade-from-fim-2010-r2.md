---
# required metadata

title: Upgrade von Forefront Identity Manager 2010 R2 | Microsoft Identity Manager
description: Erfahren Sie, wie Sie die FIM 2010 R2-Komponenten upgraden, und daraufhin die neuen MIM 2016-Komponenten installieren.
keywords:
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Upgrade aus Forefront Identity Manager 2010 R2

Wenn Sie eine Umgebung mit Forefront Identity Manager (FIM) 2010 R2 haben und Microsoft Identity Manager (MIM) 2016 ausprobieren möchten, verwenden Sie diesen Artikel als Anleitung. Es gibt drei Phasen in diesem Upgrade:

1.  Installieren Sie MIM 2016 Synchronization Service (Sync) auf einem Server, der Bestandteil Ihrer Active Directory-Domäne ist. Dadurch wird die FIM 2010 R2-Instanz von Sync ersetzt.

2.  Installieren Sie MIM-Dienst und -Portal. An diesem Punkt haben Sie auch die Wahl, das Registrierungsportal und Dienstportal für Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) zu installieren. Anschließend wird die Privileged Access Management-Featuregruppe installiert.

3.  Stellen Sie die MIM-Add-Ins und -Erweiterungen auf einem separaten Computer bereit. Hierzu gehört der integrierte Client für SSPR-Windows-Anmeldung.


In dieser Anleitung wird davon ausgegangen, dass Sie Folgendes bereits eingerichtet haben:
- FIM 2010 R2, bereitgestellt in einer Testumgebung
- Server mit Windows Server 2012, Windows Server 2012 R2 oder Windows Server 2008 R2
- Lokale und umgebungsbezogene Voraussetzungen (SQL Server, Exchange Server, SharePoint Services usw.), die für FIM 2010 R2 konfiguriert sind


## Vorbereitung

1.  Sichern Sie die Datenbank des FIM-Diensts, die FIM Sync-Datenbank, FIM Sync sowie die Dienstkonfiguration und die Software.

2.  Melden Sie sich auf jedem Server, auf dem FIM 2010 R2-Komponenten installiert sind – z.B. *CORPIDM* –, als „Contoso\Administrator“ an. In diesem Beispiel sind Administratorrechte erforderlich, um FIM 2010 R2 auf **MIM**zu aktualisieren.

3.  Laden Sie die MIM-Software herunter, oder entpacken Sie diese.

## Upgraden von Synchronization Service

1.  Melden Sie sich als Administrator bei einem Server an, auf dem der FIM 2010 R2-Synchronisierungsdienst (Sync) bereitgestellt wird.

2.  Sichern Sie die Datenbank, bevor Sie die nächsten Schritte ausführen.

3.  Öffnen Sie die Konsole **Dienste** , suchen Sie nach **Forefront Identity Manager-Synchronisierungsdienst**, und beenden Sie diesen.

    ![Bild: Services-Konsole](media/MIM-UpgFIM1.PNG)

4.  Führen Sie das **MIM-Synchronisierungsdienst-Installationsprogramm**aus. Das Installationsprogramm wird die vorhandene Version von Sync erkennen und ein Upgrade vorschlagen. Klicken Sie auf die Schaltfläche **Aktualisieren** .

5.  Wenn Sie den Lizenzbedingungen zustimmen, klicken Sie auf **Weiter** .

6.  Geben Sie das Kennwort für das Dienstkonto ein, das für Sync verwendet wird, und klicken Sie auf **Weiter**.

    ![Bild: Konfigurieren eines MIM Synchronization Service-Kontos](media/MIM-UpgFIM3.png)

7.  Vergewissern Sie sich, dass die Sicherheitsgruppennamen richtig sind, und klicken Sie auf **Weiter**.

    ![Bild: Konfigurieren der Sicherheitsgruppen für MIM Synchronization Service](media/MIM-UpgFIM4.png)

8.  Nehmen Sie keine Änderung an der Einstellung des Kontrollkästchens für Firewallregeln für eingehende RPC-Kommunikation vor.

9. Das Installationsprogramm kann FIM 2010 R2 nun auf MIM aktualisieren. Klicken Sie auf **Upgrade** , um den Upgradevorgang zu starten.

10. Der Upgradevorgang wird nun ausgeführt. Während der Ausführung des Upgradevorgangs dürfen Sie weder das Installationsprogramm beenden noch den Computer neu starten.

    ![Bild: Installationsstatus von MIM Sync](media/MIM-UpgFIM7.png)

11. Im Verlauf des Upgradevorgangs wird eine Warnung hinsichtlich der Aktualisierung des Sync-Datenbank angezeigt. Es wird empfohlen, die Datenbank vor diesem Anfang zu sichern.

12. Wenn der Upgradevorgang erfolgreich abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Bild: Installation von MIM Sync erfolgreich](media/MIM-UpgSP1.png)

13. Beachten Sie, dass der **Synchronisierungsdienst** neu gestartet wurde.

## Upgraden des Diensts und des Portals

1.  Melden Sie sich als Administrator bei einem Server an, auf dem der FIM 2010 R2-Dienst und das FIM 2010 R2-Portal bereitgestellt werden.

2.  Öffnen Sie die Konsole **Dienste** , suchen Sie nach **Forefront Identity Manager-Dienst**, und beenden Sie diesen.

    ![Bild: Services-Konsole](media/MIM-UpgFIM9.PNG)

3.  Führen Sie das Installationsprogramm für den MIM-Dienst und das MIM-Portal aus. Klicken Sie auf die Schaltfläche **Installieren**, um fortzufahren.

    ![Bild: Installieren des MIM-Diensts und Portals](media/MIM-UpgSP2.png)

4.  Wenn Sie den Lizenzbedingungen zustimmen, klicken Sie auf **Weiter** .

5.  Klicken Sie im MIM-Bildschirm für das Programm zur Verbesserung der Benutzerfreundlichkeit auf **Weiter** , um den Vorgang fortzusetzen.

6.  Wählen Sie die MIM-Funktionen und -Komponenten aus, die Sie installieren möchten. Klicken Sie dann auf **Weiter**.

    ![Bild: Benutzerdefinierte Installation](media/MIM-UpgSP4.png)

    1.  **MIM-Dienst**: Dieses Feature ist für mindestens einen Server obligatorisch und erfordert einen SQL Server-Datenbankserver, der sich entweder auf demselben oder auf einem anderen Server befindet.

    2.  **MIM-Portal:** Dieses Feature ist auf mindestens einem Server obligatorisch und erfordert SharePoint 2013 Foundation.

    3.  **MIM-Kennwortregistrierungsportal**: Diese Funktion wird für das selbstständige Zurücksetzen des Kennworts benötigt.

    4.  **MIM-Kennwortzurücksetzungsportal:** Dieses Fetaure ist für Kennwortzurücksetzung erforderlich.

7.  Geben Sie die Details des SQL-Servers an, der für die FIM-Datenbank verwendet wird. Wählen Sie die Option aus, mit der die vorhandene Datenbank erneut verwendet wird und die Daten beibehalten werden. Klicken Sie zum Fortfahren auf **Weiter** .

8. Wurde die Option zur Wiederverwendung der vorhandenen Datenbank ausgewählt, wird eine Erinnerung angezeigt, dass die Datenbank gesichert werden sollte.

9. Geben Sie die Details des E-Mail-Servers ein. Befindet sich der E-Mail-Server auf dem aktuellen Server, geben Sie „localhost“ als Standort des Mailservers ein. Klicken Sie zum Fortfahren auf **Weiter** .

    ![Bild: Konfigurieren der E-Mail-Server-Verbindung](media/MIM-UpgSP6.png)

10. Wählen Sie ein Zertifikat aus, das der Dienst verwenden soll, um Clients zu überprüfen. Sie sollten das vorhandene Zertifikat aus dem lokalen Zertifikatspeicher verwenden, das zuvor vom FIM-Dienst verwendet wurde.

    ![Bild: Dienstzertifikat konfigurieren](media/MIM-UpgSP7.png)

    1.  Ist die Speicheroption für lokale Zertifikate ausgewählt, klicken Sie auf die Schaltfläche **Zertifikat auswählen**, und wählen Sie im Popupfenster ein Zertifikat aus der Liste aus. Klicken Sie auf **OK** und anschließend auf **Weiter**.

        ![Bild: Popupfenster „Zertifikat auswählen“](media/MIM-UpgSP8.PNG)

11. Konfigurieren Sie die Dienstkonto-Anmeldeinformationen für den MIM-Dienst. Beachten Sie, dass das Dienstkonto nicht mit dem Dienstkonto identisch sein darf, das für den Synchronisierungsdienst verwendet wird. Dieses Dienstkonto sollte das Konto sein, das für den FIM-Dienst verwendet wurde.

    ![Bild: Konfigurieren des MIM-Dienstkontos](media/MIM-UpgSP9.png)

12. Konfigurieren Sie die Details des MIM Sync-Servers entsprechend der Bereitstellung des MIM-Diensts, die Sie in einem vorherigen Schritt konfiguriert haben.

    ![Bild: Konfigurieren des MIM-Diensts und -Portals (Sync)](media/MIM-UpgSP10.png)

13. Wenn Sie das MIM-Portal installieren, geben Sie die Adresse des MIM-Dienst-Servers an. Klicken Sie auf **Weiter**.

14. Bei der Installation des MIM-Portals geben Sie die URL der SharePoint-Websitesammlung an, in der das FIM-Portal derzeit gehostet wird. Klicken Sie auf **Weiter**.

## Installieren des MIM-Kennwortregistrierungsportals

1. Wenn Sie das MIM-Kennwortregistrierungsportal installieren, geben Sie die angeforderte URL für das Kennwortregistrierungsportal an. Klicken Sie auf **Weiter**.

2. Konfigurieren Sie für Clients und Endbenutzer die Möglichkeit, den Dienst und das Portal zu nutzen.

    1.  Überprüfen Sie, ob **Ports 5725 und 5726 in der Firewall öffnen**aktiviert werden soll.

    2.  Überprüfen Sie, ob **Grant authenticated users access to the MIM Portal site** (Authentifizierten Benutzern Zugriff auf die MIM-Portalwebsite gewähren) aktiviert werden soll.

    3.  Klicken Sie auf **Weiter**.

3. Stellen Sie die Zugriffsdetails und Anmeldedaten für die MIM-Kennwortregistrierung bereit.

    ![Bild: MIM-Kennwortregistrierungsportal konfigurieren](media/MIM-UpgSP15.png)

    1.  Geben Sie den Dienstkontonamen (einschließlich Domäne) und das Kennwort für die MIM-Kennwortregistrierung an.

    2.  Geben Sie die Details des Hosts – Name und Port (etwa 8080) – des Kennwortregistrierungsportals an.

    3.  Aktivieren Sie die Option **Port in der Firewall öffnen** .

    4.  Klicken Sie auf **Weiter**.

4. Gehen Sie im nächsten Konfigurationsbildschirm der MIM-Kennwortregistrierung wie folgt vor:

    1.  Teilen Sie der MIM-Kennwortregistrierung mit, wo der MIM-Dienst installiert ist ( in der Regel auf demselben System).

    2.  Bestimmen Sie, ob Extranet- und Intranetbenutzer oder, wie dies zuvor für FIM-Kennwortzurücksetzung konfiguriert war, nur Intranetbenutzer auf dieses Portal zugreifen können.

## Installation des Portals zum Zurücksetzen des MIM-Kennworts

1. Wenn Sie das MIM-Kennwortzurücksetzungsportal installieren, geben Sie die Zugriffsdetails und Anmeldeinformationen für die MIM-Kennwortzurücksetzung an.

    ![Bild: Portal zum Zurücksetzen des MIM-Kennworts](media/MIM-UpgSP17.png)

    1.  Geben Sie den Dienstkontonamen (einschließlich Domäne) und das Kennwort für die MIM-Kennwortzurücksetzung an.

    2.  Geben Sie die Details des Hosts – Name und Port (etwa 8088) – des Kennwortzurücksetzungsportals an.

    3.  Aktivieren Sie die Option **Port in der Firewall öffnen** .

    4.  Klicken Sie auf **Weiter**.

2. Gehen Sie im nächsten Konfigurationsbildschirm der MIM-Kennwortzurücksetzung wie folgt vor:

    1.  Teilen Sie der MIM-Kennwortzurücksetzung mit, wo der MIM-Dienst installiert ist.

    2.  Geben Sie an, ob Extranet- und Intranetbenutzer oder nur Intranetbenutzer auf dieses Portal zugreifen können.

## Installation und Upgrade abschließen

1. Sobald alle Konfigurationsdefinitionen erfolgreich beendet wurden, wird eine Installationsseite aufgerufen. Klicken Sie auf **Installieren** , damit die Installation von MIM-Dienst und -Portal begonnen wird.

2. Die Installation von MIM-Dienst und -Portal wird jetzt ausgeführt. Während der Installation dürfen Sie weder das Installationsprogramm abbrechen noch den Computer neu starten.

3. Sobald die Installation (das Upgrade) des MIM-Diensts und -Portals abgeschlossen ist, wird ein Bestätigungsfenster aufgerufen. Klicken Sie auf **Fertig stellen** , um die Installation abzuschließen und das Installationsprogramm zu beenden.

4. Beachten Sie, dass der **Forefront Identity Manager-Dienst** neu gestartet wurde.

Hinweis: Wenn die FIM-Add-Ins und -Erweiterungen derzeit auf den Computern der Benutzer für SSPR bereitgestellt werden, konfigurieren Sie auf keinen Fall die neuen MFA-Telefongates zur Kennwortzurücksetzung. Dies darf erst geschehen, nachdem alle FIM-Add-Ins und -Erweiterungen auf MIM 2016 aktualisiert wurden.  Da die FIM 2010 und FIM 2010 R2-Add-ins und -Erweiterungen die neuen Telefongates nicht erkennen, geben sie einen Fehler zurück. Ein Benutzer ist dann nicht in der Lage, die Zurücksetzung des Kennworts abzuschließen.


<!--HONumber=May16_HO3-->


