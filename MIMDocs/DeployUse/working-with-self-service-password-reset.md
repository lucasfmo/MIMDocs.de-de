---
# required metadata

title: Arbeiten mit der Self-Service-Kennwortzurücksetzung | Microsoft Identity Manager
description: Unter „Neues zur Self-Service-Kennwortzurücksetzung in MIM 2016“ erfahren Sie u.a., wie SSPR mit der mehrstufigen Authentifizierung funktioniert. 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Arbeiten mit der Self-Service-Kennwortzurücksetzung
Microsoft Identity Manager 2016 stellt zusätzliche Funktionalität für das Feature Self-Service-Kennwortzurücksetzung bereit. Diese Funktionalität wurde um einige wichtige Funktionen erweitert:

-   Im Portal für die Self-Service-Kennwortzurücksetzung und im Windows-Anmeldebildschirm können Benutzer nun ihre Konten entsperren, ohne ihre Kennwörter zu ändern oder Supportadministratoren anzurufen. Es gibt viele berechtigte Gründe, warum Konten für Benutzer gesperrt werden können, etwa wenn sie ein altes Kennwort eingeben, zweisprachige Computer verwenden und die Tastatur auf die falsche Sprache eingerichtet ist, oder versuchen, sich bei einer gemeinsam genutzten Arbeitsstation anzumelden, die bereits über das Konto einer anderen Person geöffnet ist.

-   Es wurde das neue Authentifizierungsgate Telefongate hinzugefügt. Dieses ermöglicht Benutzerauthentifizierung per Telefonanruf.

-   Es wurde Unterstützung für den Microsoft Azure Multi-Factor Authentication-Dienst (MFA-Dienst) hinzugefügt. Dieser Dienst kann sowohl für das vorhandene SMS-Einmalkennwort-Gate als auch für das neue Telefongate verwendet werden.

## Azure für Multi-Factor Authentication
Microsoft Azure Multi-Factor Authentication ist ein Authentifizierungsdienst, der von Benutzern verlangt, ihre Anmeldeversuche über eine mobile App, einen Telefonanruf oder eine SMS zu bestätigen. Der Dienst kann mit Microsoft Azure Active Directory sowie als Dienst für Cloud- und lokale Unternehmensanwendungen verwendet werden.

Azure MFA bietet einen zusätzlichen Authentifizierungsmechanismus, der vorhandene Authentifizierungsprozesse unterstützen kann, so beispielsweise den Prozess, der von MIM für die Self-Service-Anmeldeunterstützung ausgeführt wird.

Bei der Verwendung von MFA authentifizieren sich Benutzer zur Bestätigung ihrer Identität beim System, wenn sie versuchen, wieder Zugriff auf ihre Konten und Ressourcen zu erhalten. Eine Authentifizierung kann per SMS oder per Telefonanruf erfolgen.   Je stärker die Authentifizierung ist, desto größer ist die Gewissheit, dass es sich bei der Person, die Zugriff erhalten möchte, tatsächlich um den Besitzer dieser Identität handelt. Sobald der Benutzer authentifiziert ist, kann er ein neues Kennwort wählen, durch das das alte ersetzt wird.

## Voraussetzungen für das Einrichten der Self-Service-Kontoentsperrung und -Kennwortzurücksetzung mithilfe von MFA
Für diesen Abschnitt wird davon ausgegangen, dass Sie die Bereitstellung von Microsoft Identity Manager 2016 heruntergeladen und abgeschlossen haben, einschließlich den folgenden Komponenten und Diensten:

-   Ein Windows Server 2008 R2 oder höher wurde als Active Directory-Server eingerichtet, einschließlich Active Directory-Domänendienste und -Domänencontroller mit einer designierten Domäne (einer „Firmen“-Domäne)

-   Es ist eine Gruppenrichtlinie für Kontosperrungen definiert.

-   MIM 2016 Synchronization Service (Sync) ist installiert und läuft auf einem Server, der in die Active Directory-Domäne eingebunden ist

-   Der MIM 2016-Dienst &amp; das MIM-Portal einschließlich des SSPR-Registrierungs- und des SSPR-Zurücksetzungsportals sind auf einem Server installiert und werden dort ausgeführt (sie können auf demselben Server wie Sync ausgeführt werden)

-   MIM Sync ist für die Active Directory-MIM-Identitätssynchronisierung konfiguriert, einschließlich:

    -   Konfigurieren des Active Directory-Verwaltungs-Agenten (ADMA) für Verbindungen mit AD DS und für die Fähigkeit, Identitätsdaten aus Active Directory zu importieren und nach Active Directory zu exportieren.

    -   Konfigurieren des MIM-Verwaltungs-Agents (MIM-MA) für die Konnektivität mit der FIM-Dienst-Datenbank und für die Fähigkeit, Identitätsdaten aus der FIM-Datenbank zu importieren bzw. in die FIM-Datenbank zu exportieren.

    -   Konfigurieren von Synchronisierungsregeln im MIM-Portal, um für den MIM-Dienst Synchronisierung von Benutzerdaten sowie synchronisierungsbasierte Aktivitäten zu ermöglichen.

-   Die MIM 2016-Add-Ins &amp; -Erweiterungen inklusive des integrierten SSPR-Clients für die Windows-Anmeldung werden auf dem Server oder auf einem separaten Clientcomputer bereitgestellt.

## Vorbereiten von MIM für das Arbeiten mit der mehrstufigen Authentifizierung
Konfigurieren Sie MIM Sync so, dass Kennwortzurücksetzung und Kontoentsperrung unterstützt werden. Weitere Informationen finden Sie unter [Installing the FIM Add-ins and Extensions](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx) (Installieren der FIM-Add-Ins und -Erweiterungen), [Installing FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx) (Installieren von FIM SSPR), [SSPR Authentication Gates](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) (SSPR-Authentifizierungsgates) und im [SSPR Test Lab Guide](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx) (SSPR-Test Lab-Handbuch)

Im nächsten Abschnitt richten Sie Ihren Azure MFA-Anbieter in Microsoft Azure Active Directory ein. Bei diesem Vorgang generieren Sie eine Datei, die die von der MFA zum Kontaktieren von Azure MFA benötigten Authentifizierungsdaten enthält.  Um fortfahren zu können, benötigen Sie ein Azure-Abonnement.

### Registrieren Sie Ihren Anbieter für die mehrstufige Authentifizierung in Azure

1.  Melden Sie sich beim [klassischen Azure-Portal](http://manage.windowsazure.com) als Azure-Abonnementadministrator an.

2.  Klicken Sie in der unteren linken Ecke auf **Neu**.

3.  Klicken Sie auf **App-Dienste &gt; Active Directory &gt; Anbieter für mehrstufige Authentifizierung &gt; Schnellerfassung**.

![Bild: Schnelles Erstellen von MFA im Azure-Portal](media/MIM-SSPR-Azureportal.png)

4.  Geben Sie im Feld **Name** die Zeichenfolge **SSPRMFA** ein, und klicken Sie dann auf **Erstellen**.

![Bild: Azure-Portal-MFA](media/MIM-SSPR-Azureportal-2.png)

5.  Klicken Sie im klassischen Azure-Portalmenü auf **Active Directory**, und klicken Sie dann auf die Registerkarte **Anbieter für Multi-Factor Authentication**.

6.  Klicken Sie auf **SSPRMFA** und dann am unteren Rand des Bildschirms auf **Verwalten** .

    ![Azure-Portal-Symbol „Verwalten“](media/MIM-SSPR-ManageButton.png)

7.  Klicken Sie im neuen Fenster im linken Bereich unter **Konfigurieren**auf **Einstellungen**.

8.  Deaktivieren Sie unter **Betrugswarnung**die Option **Benutzer bei Betrugsmeldung sperren** . Dies geschieht, um zu verhindern, dass der gesamte Dienst blockiert wird.

9. Klicken Sie in dem sich öffnenden Fenster **Azure Multi-Factor Authentication** im linken Menü unter **Downloads** auf **SDK** .

10. Klicken Sie in der Spalte ZIP auf den Link **Herunterladen** für die Datei mit der Sprache **SDK for ASP.net 2.0 C#**.

    ![Bild: Herunterladen der Azure MFA-ZIP-Datei](media/MIM-SSPR-Azure-MFA.png)

11. Kopieren Sie die resultierende ZIP-Datei auf alle Systeme, auf denen der MIM-Dienst installiert ist.  Bedenken Sie dabei, dass die ZIP-Datei Schlüsselmaterial enthält, das zur Authentifizierung beim Azure MFA-Dienst verwendet wird.

### Aktualisieren Sie die Konfigurationsdatei.

1. Melden Sie sich auf dem Computer, auf dem der MIM-Dienst installiert ist, als der Benutzer an, der MIM installiert hat.

2. Erstellen Sie einen neuen Verzeichnisordner unter dem Verzeichnis, in dem der MIM-Dienst installiert wurde, z.B. **C:\Programmdateien\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Navigieren Sie in Windows Explorer zum Ordner **\pf\certs** der ZIP-Datei, die Sie im vorherigen Abschnitt heruntergeladen haben, und kopieren Sie die Datei **cert_key.p12** in das neue Verzeichnis.

4.  Öffnen Sie in der ZIP-Datei des SDKs im Ordner **\pf** die Datei **pf_auth.cs**.

5.  Suchen Sie die folgenden drei Parameter: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![Bild: „pf_auth.cs“-Code](media/MIM-SSPR-pFile.png)

6.  Öffnen Sie in **C:\Programme\Microsoft Forefront Identity Manager\2010\Service**die folgende Datei: **MfaSettings**.xml.

7.  Kopieren Sie die Werte aus den `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` -Parametern in die Datei „pf_aut.cs“ in ihre entsprechenden XML-Elemente der Datei „MfaSettings.xml“.

8.  Extrahieren Sie in der ZIP-Datei des SDKs unter „\pf\certs“ die Datei **cert_key.p12** , und geben Sie den vollständigen Pfad zu dieser Datei in der Datei „MfaSettings.xml“ in das XML-Element „ `<CertFilePath>` “ ein.

9. Geben Sie im „ `<username>` “-Element einen beliebigen Benutzernamen ein.

10. Geben Sie im `<DefaultCountryCode>`-Element Ihren standardmäßigen Ländercode ein. Sind für Benutzer Telefonnummern ohne einen Ländercode registriert, ist dies der Ländercode, den die Benutzer erhalten. Hat ein Benutzer einen internationalen Ländercode, muss dieser in die registrierte Telefonnummer einbezogen werden.

11. Speichern Sie die Datei „MfaSettings.xml“ unter demselben Namen am gleichen Speicherort.

#### Konfigurieren des Telefongates oder des SMS-Gates für das Einmalkennwort

1.  Starten Sie Internet Explorer, und navigieren Sie zum MIM-Portal. Authentifizieren Sie sich als MIM-Administrator, und klicken Sie auf der linken Navigationsleiste auf  **Workflows** .

    ![Bild: MIM-Portal-Navigation](media/MIM-SSPR-workflow.jpg)

2.  Aktivieren Sie die Option für den **AuthN-Workflow zur Kennwortzurücksetzung**.

    ![Bild: MIM-Portal-Workflows](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Klicken Sie auf die Registerkarte **Aktivitäten** , und führen Sie einen Bildlauf nach unten zu **Aktivität hinzufügen**aus.

4.  Wählen Sie **Telefongate** oder **SMS-Gate für das Einmalkennwort** aus, und klicken Sie auf **Auswählen** und dann auf **OK**.

Benutzer in Ihrer Organisation können sich jetzt für das Zurücksetzen von Kennwörtern registrieren.  Im Verlauf dieses Vorgangs müssen sie ihre Telefonnummer oder Mobiltelefonnummer eingeben, damit das System weiß, wie es sie anrufen oder ihnen SMS-Nachrichten senden kann.

#### Registrieren von Benutzern für das Zurücksetzen von Kennwörtern

1.  Ein Benutzer startet einen Webbrowser und navigiert zum MIM-Portal zum Registrieren für das Zurücksetzen von Kennwörtern.  (In der Regel wird dieses Portal mit Windows-Authentifizierung konfiguriert).  Im Portal geben sie ihren Benutzernamen und ihr Kennwort an, um ihre Identität zu bestätigen.

    Sie müssen das Portal für die Kennwortregistrierung öffnen und sich mithilfe ihres Benutzernamens und dem zugehörigen Kennwort authentifizieren.

2.  In das Feld **Telefonnummer** oder **Mobiltelefon**  müssen sie einen Ländercode, ein Leerzeichen und die Telefonnummer eingeben, und anschließend müssen sie auf **Weiter**klicken.

    ![Bild: MIM-Telefonüberprüfung](media/MIM-SSPR-PhoneVerification.JPG)

    ![Bild: MIM-Mobiltelefonüberprüfung](media/MIM-SSPR-mobilephoneverification.JPG)

## Wie funktioniert dies für die Benutzer?
Nachdem die Konfiguration abgeschlossen und das Gate aktiv ist, möchten Sie möglicherweise wissen, welche Schritte die Benutzer durchlaufen müssen, wenn sie ihre Kennwörter direkt vor dem Urlaub zurücksetzen, um dann zurückzukehren und festzustellen, dass sie ihre Kennwörter vollständig vergessen haben.

Es gibt zwei Möglichkeiten, wie ein Benutzer die Kennwortzurücksetzungs- und die Kontoentsperrungsfunktion nutzen kann: entweder vom Windows-Anmeldebildschirm oder vom Self-Service-Portal aus.

Indem Sie die MIM-Add-Ins und -Erweiterungen auf einem zur Domäne gehörigen Computer installieren, der über das Netzwerk Ihrer Organisation mit dem MIM-Dienst verbunden ist, können Benutzer ein vergessenes Kennwort in der Desktop-Anmeldungsumgebung wiederherstellen.  Die folgenden Schritte begleiten Sie durch diesen Vorgang.

#### In die Windows-Desktopanmeldung integrierte Kennwortzurücksetzung

1.  Falls Ihr Benutzer mehrmals ein falsches Kennwort eingibt, kann er im Anmeldebildschirm auf **Probleme bei der Anmeldung?** klicken. .

    ![Bild: Anmeldebildschirm](media/MIM-SSPR-problemsloggingin.JPG)

    Nachdem der Benutzer auf diesen Link geklickt hat, wird das Dialogfeld „MIM-Kennwortzurücksetzung“ angezeigt, in dem er sein Kennwort ändern oder sein Konto entsperren kann.

    ![Bild: MIM-Kennwortzurücksetzung](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  Der Benutzer wird aufgefordert, sich zu authentifizieren. Wenn MFA konfiguriert wurde, erhält der Benutzer einen Telefonanruf.

3.  Im Hintergrund passiert Folgendes: Azure MFA ruft die Telefonnummer an, die der Benutzer angegeben hat, als er sich für den Dienst angemeldet hat.

4.  Wenn der Benutzer den Anruf annimmt, wird er aufgefordert, auf dem Telefon die Rautetaste (#) zu drücken. Danach klickt der Benutzer im Portal auf **Weiter** .

    Wenn Sie auch andere Gates eingerichtet haben, wird der Benutzer in den nachfolgenden Bildschirmen aufgefordert, weitere Informationen bereitzustellen.

    > [!NOTE]
    > Wenn der Benutzer ungeduldig ist und auf **Weiter** klickt, bevor er die Raute-Taste (#) gedrückt hat, schlägt die Authentifizierung fehl.

5.  Nach erfolgreicher Authentifizierung hat der Benutzer zwei Optionen: Er kann sein Konto entsperren und sein aktuelles Kennwort beibehalten oder ein neues Kennwort festlegen.

6.  Der Benutzer muss dann ein neues Kennwort zweimal eingeben, damit das Kennwort zurückgesetzt wird.

#### Zugreifen aus dem Self-Service-Portal

1.  Benutzer können einen Webbrowser öffnen, zum **Portal für die Kennwortzurücksetzung** navigieren, ihren Benutzernamen eingeben und auf **Weiter**klicken.

    Wenn MFA konfiguriert wurde, erhält der Benutzer einen Telefonanruf. Im Hintergrund passiert Folgendes: Azure MFA ruft die Telefonnummer an, die der Benutzer angegeben hat, als er sich für den Dienst angemeldet hat.

    Wenn der Benutzer den Telefonanruf annimmt, wird er aufgefordert, auf dem Telefon die Rautetaste (#) zu drücken. Danach klickt der Benutzer im Portal auf **Weiter** .

2.  Wenn Sie auch andere Gates eingerichtet haben, wird der Benutzer in den nachfolgenden Bildschirmen aufgefordert, weitere Informationen bereitzustellen.

    > [!NOTE]
    > Wenn der Benutzer ungeduldig ist und auf **Weiter** klickt, bevor er die Raute-Taste (#) gedrückt hat, schlägt die Authentifizierung fehl.

3.  Der Benutzer muss wählen, ob er sein Kennwort zurücksetzen oder sein Konto entsperren möchte. Hat er gewählt, sein Konto zu entsperren, wird das Konto entsperrt.

    ![Bild: MIM-Anmelde-Assistent für die Kontoentsperrung](media/MIM-SSPR-accountUnlock.JPG)

4.  Nach erfolgreicher Authentifizierung hat der Benutzer zwei Optionen: Er kann sein aktuelles Kennwort beibehalten, oder er kann ein neues Kennwort festlegen.

5.  ![Bild: MIM-Kontoentsperrung erfolgreich](media/MIM-SSPR-account-unlock.JPG)

6.  Wenn der Benutzer sich für das Zurücksetzen seines Kennworts entscheidet, muss er sein neues Kennwort zweimal eingeben und auf **Weiter** klicken, um das Kennwort zu ändern.

    ![Bild: MIM-Anmelde-Assistent für die Kennwortzurücksetzung](media/MIM-SSPR-PR1.JPG)


<!--HONumber=Apr16_HO2-->


