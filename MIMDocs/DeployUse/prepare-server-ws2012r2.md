---
# required metadata

title: Einrichten eines Identitätsverwaltungsservers&#58; Windows Server 2012 R2 | Microsoft Identity Manager
description: Erfahren Sie mehr über die ersten Schritte und die Mindestanforderungen für die Vorbereitung von Windows Server 2012 R2 für MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Einrichten eines Identitätsverwaltungsservers: Windows Server 2012 R2

>[!div class="step-by-step"]

> [!NOTE]
> « Vorbereiten einer Domäne SQL Server 2014 » Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso.
> - Ersetzen Sie diese durch eigene Namen und Werte.
> - Beispiel:
> - Domänencontrollername: **mimservername**

## Domänenname: **contoso**

Kennwort: **Pass@word1** Hinzufügen von Windows Server 2012 R2 zu Ihrer Domäne

1. Beginnen Sie mit einem Computer mit Windows Server 2012 R2 mit mindestens 8 GB RAM.

2. Geben Sie bei der Installation die Edition „Windows Server 2012 R2 Standard (Server mit grafischer Benutzeroberfläche) x64“ an. Melden Sie sich bei dem neuen Computer als Administrator an.  Geben Sie dem Computer mithilfe der Systemsteuerung eine statische IP-Adresse im Netzwerk.

3. Konfigurieren Sie die Netzwerkschnittstelle so, dass DNS-Abfragen an die IP-Adresse des Domänencontrollers im vorherigen Schritt gesendet werden, und legen Sie den Computernamen auf **CORPIDM** fest.  Dies erfordert einen Neustart des Servers.  Öffnen Sie die Systemsteuerung, und fügen Sie den Computer der Domäne *contoso.local* hinzu, die Sie im letzten Schritt konfiguriert haben.

4. Dies beinhaltet die Bereitstellung des Benutzernamens und der Anmeldeinformationen eines Domänenadministrators wie *Contoso\Administrator*.

5. Nachdem die Willkommensnachricht angezeigt wird, schließen Sie das Dialogfeld, und starten Sie den Server erneut neu.

    ```
    gpupdate /force /target:computer
    ```

    Melden Sie sich auf dem Computer *CorpIDM* als Domänenadministrator an, z.B. als *Contoso\Administrator*.

6. Starten Sie ein PowerShell-Fenster als Administrator, und geben Sie den folgenden Befehl ein, um den Computer mit den Gruppenrichtlinieneinstellungen zu aktualisieren.

    ![Nach spätestens einer Minute wird der Vorgang mit der Meldung „Die Aktualisierung der Computerrichtlinie wurde erfolgreich abgeschlossen“ beendet.](media/MIM-DeployWS2.png)

7. Fügen Sie die Rollen **Webserver (IIS)** und **Anwendungsserver**, die **.NET Framework** 3.5-, 4.0- und 4.5-Features und das **Active Directory-Modul für Windows PowerShell** hinzu. Bild: PowerShell-Features Geben Sie in PowerShell die folgenden Befehle ein:

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## Möglicherweise ist es notwendig, dass Sie einen anderen Speicherort für die Quelldateien für **.NET Framework** 3.5-Features angeben.

Diese Funktionen sind in der Regel nicht vorhanden, wenn Windows Server installiert wird. Sie sind aber auf dem Betriebssystem-Installationsdatenträger im Ordner „Sources“ im parallelen Ordner (SxS) verfügbar, beispielsweise „D:\Sources\SxS\*“.

1. Konfigurieren der Serversicherheitsrichtlinie

2. Richten Sie die Serversicherheitsrichtlinien ein, damit die neu erstellten Konten als Dienste ausgeführt werden können.

3. Starten Sie das Programm „Lokale Sicherheitsrichtlinie“.

    ![Navigieren Sie zu **Lokale Richtlinien > Zuweisen von Benutzerrechten**.](media/MIM-DeployWS3.png)

4. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Anmelden als Dienst**, und wählen Sie dann **Eigenschaften**aus.

5. Bild: Lokale Sicherheitsrichtlinien

6.  Klicken Sie auf **Benutzer oder Gruppe hinzufügen**, und geben Sie im Textfeld `contoso\mimsync; contoso\mimma; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\mimsspr` ein. Klicken Sie dann auf **Namen überprüfen** und anschließend auf **OK**.

7. Klicken Sie auf **OK**, um das Eigenschaftenfenster **Anmelden als Dienst** zu schließen.

8. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Zugriff vom Netzwerk auf diesen Computer verweigern**, und wählen Sie **Eigenschaften** aus.

9. Klicken Sie zunächst auf **Benutzer oder Gruppe hinzufügen**, geben Sie in das Textfeld `contoso\MIMSync; contoso\MIMService` ein, und klicken Sie auf **OK**.

10. Klicken Sie auf **OK**, um das Eigenschaftenfenster **Zugriff vom Netzwerk auf diesen Computer verweigern** zu schließen.

11. Klicken Sie im Detailbereich mit der rechten Maustaste auf **Lokal anmelden verweigern**, und wählen Sie dann **Eigenschaften** aus.

12. Klicken Sie zunächst auf **Benutzer oder Gruppe hinzufügen**, geben Sie in das Textfeld `contoso\MIMSync; contoso\MIMService` ein, und klicken Sie auf **OK**.


## Klicken Sie auf **OK**, um das Eigenschaftenfenster **Lokal anmelden verweigern** zu schließen.

1.  Schließen Sie das Fenster „Lokale Sicherheitsrichtlinien“.

2.  Ändern Sie den IIS-Windows-Authentifizierungsmodus.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>Öffnen Sie ein PowerShell-Fenster.  
Beenden Sie IIS mit dem Befehl *iisreset/STOP*.


<!--HONumber=May16_HO3-->


