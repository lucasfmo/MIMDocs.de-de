---
# required metadata

title: Registrieren von Smartcards für Nichtadministratoren | Microsoft Identity Manager
description: Erfahren Sie, wie Sie Smartcards für Benutzer ohne Administratorrechte auf ihren Computern registrieren können, damit diese den Zertifikat-Manager verwenden können.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Registrieren von Smartcards für Nichtadministratoren
Wenn ein Benutzer auf seinem Computer kein lokaler Administrator ist, kann er standardmäßig keine Smartcard auf seinen eigenen Computern registrieren. Mithilfe des folgenden Verfahrens können Sie diese Einschränkung umgehen.

## Ermöglichen von Smartcard-Erneuerung für Nichtadministratoren im MIM 2016-Zertifikat-Manager

1.  **Entpacken Sie die APPX-Datei.**

    Rufen Sie ein Signaturzertifikat ab. Führen Sie die Schritte zum [Signing Windows 8 applications using an Internal PKI](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx) (Anmelden von Windows 8-Anwendung mithilfe einer internen PKI) aus. Halten Sie an, wenn Sie zu „Anwendung signieren“ gelangen. Benennen Sie die exportierte PFX-Datei. Führen Sie auch einen Export in eine CER-Datei durch, und importieren Sie diese dann mithilfe der CER-Datei des neuen Signaturzertifikats auf dem Client.

    Führen Sie Folgendes aus, um die APPX-Datei zu entpacken:

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **Ändern Sie die Konfigurationsdatei.**

    Benennen Sie die Datei namens `CustomDataExample.xml custom.data`um. Die CM-App sucht nach dem angegebenen Dateinamen.

    Bearbeiten Sie die Datei „custom.data“, und ändern Sie Folgendes:

    1.  Ändern Sie den Wert des Attributs im &lt;NonAdmin&gt;-Element zu TRUE.

    2.  Speichern Sie die Datei, und beenden Sie den Editor.

    3.  Löschen Sie die Datei namens „AppxSignature.p7x“.

    4.  Bearbeiten Sie die Datei namens „AppxManifest.xml“.

    5.  Ändern Sie den Wert des „Publisher“-Attributs im &lt;Identity&gt;-Element in den Antragsteller Ihres Signaturzertifikats, z.B. "CN=ABCD"

        Das Thema sollte hier mit dem Antragsteller im Signaturzertifikat identisch sein, das Sie zum Signieren der App verwenden.

    6.  Speichern Sie die Datei, und beenden Sie den Editor.

3.  **Packen Sie das Anwendungspaket (APPX-Datei) erneut, und signieren Sie es anschließend.**

    Führen Sie Folgendes aus, um die APPX-Datei zu packen und zu signieren:

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    s`igntool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  Duplizieren Sie die Profilvorlage, und fügen Sie den ursprünglichen Administratorschlüssel zum Konfigurieren des MIM-Servers hinzu:

    1.  Melden Sie sich beim CM-Portal (Zertifikatverwaltung) als Benutzer mit Administratorrechten an.

    2.  Wechseln Sie zu **Verwaltung** &gt; **Profilvorlagen verwalten** , und stellen Sie sicher, dass das Kontrollkästchen neben der erstellten Profilvorlage aktiviert ist, und klicken Sie dann auf „Ausgewählte Profilvorlage kopieren“.

    3.  Geben Sie den Namen der Profilvorlage ein, fügen Sie „nonAdmin“ hinzu, und klicken Sie dann auf **OK**.

    4.  Wenn die allgemeinen Einstellungen der Profilvorlage angezeigt werden, scrollen Sie nach ganz unten, und klicken Sie unter **Smartcard-Konfiguration**auf **Einstellungen ändern**.

    5.  Geben Sie unter **Administratorschlüssel-Anfangswert (Hex)** den standardmäßigen Administratorschlüssel ein: "010203040506070801020304050607080102030405060708"

    6.  Führen Sie einen Bildlauf nach unten aus, und klicken Sie auf **OK**.

5.  **Erstellen Sie ein Nicht-Administratorkonto auf dem Clientcomputer.**

    Benutzer ohne Administratorrechte können die virtuelle Smartcard nicht auf dem TPM erstellen, daher müssen Sie diese für sie erstellen.

6.  **Erstellen Sie eine virtuelle Smartcard mithilfe von „TpmVscMgr“.**

    Führen Sie die folgenden Schritte aus (nach wie vor als Administrator), um eine leere virtuelle Smartcard auf einem Computer zu erstellen. Dies kann über Intune, SCCM oder Gruppenrichtlinien erfolgen.

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **Installieren Sie die CM-App für das Nicht-Administratorkonto.**

8.  **Starten Sie die CM-App und Registrierung für eine virtuelle Smartcard.**


<!--HONumber=May16_HO3-->


