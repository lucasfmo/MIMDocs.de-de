---
# required metadata

title: Erstellen von Softwarezertifikaten | Microsoft Identity Manager
description: Erfahren Sie, wie Sie den Zertifikat-Manager zum Erstellen und Erneuern von Softwarezertifikaten mit Profilvorlagen verwenden.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Erstellen von Softwarezertifikaten mit dem Zertifikat-Manager
Zum Registrieren und Erneuern von Softwarezertifikaten müssen Sie kein Administrator sein und es ist keine virtuelle Smartcard erforderlich. Beachten Sie, dass Sie im Verlauf dazu aufgefordert werden, einen Zertifikatvorgang zuzulassen. Das ist normal.

## Erstellen einer Softwarezertifikat-Profilvorlage im MIM 2016-Zertifikat-Manager

1.  Erstellen Sie eine Vorlage für das Zertifikat, das Sie für die virtuelle Smartcard anfordern werden. Öffnen Sie die MMC.

2.  Klicken Sie auf **Datei** und dann auf **Snap-In hinzufügen/entfernen**.

3.  Klicken Sie in der Liste „Verfügbare Snap-Ins“ auf **Zertifikatvorlagen**, und klicken Sie dann auf **Hinzufügen**.

4.  **Zertifikatvorlagen** wird in der MMC jetzt unter „Konsolenstamm“ angezeigt. Doppelklicken Sie auf diesen Eintrag, um alle verfügbaren Zertifikatvorlagen anzuzeigen.

5.  Klicken Sie mit der rechten Maustaste auf die **Benutzervorlage**, und klicken Sie dann auf **Vorlage duplizieren**.

6.  Auf der Registerkarte **Kompatibilität** wählen Sie unter „Zertifizierungsstelle“ die Option „Windows Server 2008“ aus, und wählen Sie unter „Zertifikatempfänger“ die Option „Windows 8.1/Windows Server 2012 R2“ aus.

    1.  Geben Sie auf der Registerkarte **Allgemein** im Feld „Anzeigename“ die Zeichenfolge **Archivierte Zertifikatvorlage**ein.

    2.  b.  Auf der Registerkarte **Anforderungsbehandlung** :

        1.  Legen Sie für **Zweck** „Signatur und Verschlüsselung“ fest.

        2.  Aktivieren Sie die Option **Vom Antragsteller zugelassene symmetrische Algorithmen einbeziehen**.

        3.  Wenn Sie den Schlüssel archivieren möchten, aktivieren Sie die Option **Privaten Schlüssel für die Verschlüsselung archivieren**.

        4.  Wählen Sie unter „Gehen Sie folgendermaßen vor...“ die Option **Benutzer zur Eingabe während der Registrierung auffordern**aus.

    3.  Auf der Registerkate **Kryptografie** :

        1.  Wählen Sie unter „Anbieterkategorie“ die Option **Schlüsselspeicheranbieter**aus.

        2.  Wählen Sie **Verwendung aller auf dem Computer des Antragstellers verfügbaren Anbieter für Anforderungen möglich**aus.

    4.  Fügen Sie auf der Registerkarte **Sicherheit** die Sicherheitsgruppe hinzu, der Sie den **Registrieren** -Zugriff zuweisen möchten. Wenn Sie beispielsweise allen Benutzern Zugriff gewähren möchten, wählen Sie die Gruppe **Authentifizierte Benutzer** und dann die Berechtigung **Registrieren** für sie aus.

    5.  Auf der Registerkarte **Antragstellername** :

        1.  Deaktivieren Sie die Option **E-Mail-Name im Antragstellernamen**.

        2.  Deaktivieren Sie unter **Informationen im alternativen Antragstellernamen einbeziehen**die Option **E-Mail-Name**.

    6.  Klicken Sie auf **OK** , um die von Ihnen vorgenommenen Änderungen abzuschließen und die neue Vorlage zu erstellen. Ihre neue Vorlage sollte jetzt in der Liste der Zertifikatvorlagen angezeigt werden.

    7.  Wählen Sie **Datei** aus, und klicken Sie auf **Snap-In hinzufügen/entfernen**, um das Zertifizierungsstelle-Snap-In zu Ihrer MMC-Konsole hinzuzufügen. Wenn Sie gefragt werden, welchen Computer Sie verwalten möchten, wählen Sie **Lokaler Computer**aus.

    8.  Erweitern Sie im linken Bereich der MMC den Eintrag **Zertifizierungsstelle (lokal)**, und erweitern Sie dann Ihren Zertifikat-Manager in der Liste „Zertifizierungsstelle“.

    9. Klicken Sie mit der rechten Maustaste auf **Zertifikatvorlagen**, klicken Sie auf **Neu**, und klicken Sie dann auf **Auszustellende Zertifikatvorlage**.

    10. Wählen Sie aus der Liste die neue Vorlage, die Sie soeben erstellt haben (**Archivierte Zertifikatvorlage**), und klicken Sie dann auf **OK**.

## Erstellen der Profilvorlage

1.  Melden Sie sich beim CM-Portal (Zertifikatverwaltung) als Benutzer mit Administratorrechten an.

2.  Wechseln Sie zu **Verwaltung &gt; Profilvorlagen verwalten** , und stellen Sie sicher, dass das Kontrollkästchen neben **MIM CM-Beispielsmartcard-Profilvorlage für Anmeldung** aktiviert ist, und klicken Sie dann auf **Ausgewählte Profilvorlage kopieren**.

3.  Geben Sie den Namen der Profilvorlage ein, und klicken Sie auf **OK**.

4.  Klicken Sie in der nächsten Anzeige auf **Neue Zertifikatvorlage hinzufügen** , und aktivieren Sie das Kontrollkästchen neben dem Namen der Zertifizierungsstelle.

5.  Aktivieren Sie das Kontrollkästchen neben dem Namen des archivierten Softwarezertifikats, und klicken Sie auf **Hinzufügen**.

6.  Entfernen Sie die Benutzervorlage, indem Sie das Kontrollkästchen neben diesem Eintrag aktivieren und dann auf **Ausgewählte Zertifikatvorlagen löschen** sowie auf **OK**klicken.

7.  Klicken Sie auf **Allgemeine Einstellungen ändern**.

8.  Aktivieren Sie die Kontrollkästchen auf der linken Seite von **Verschlüsselungsschlüssel auf dem Server generieren** , und klicken Sie auf **OK**. Klicken Sie im linken Bereich auf **Wiederherstellungsrichtlinie**.

9. Klicken Sie auf **Allgemeine Einstellungen ändern**.

10. Wenn Sie archivierte Zertifikate neu ausstellen möchten, aktivieren Sie die Kontrollkästchen auf der linken Seite von **Archivierte Zertifikate erneut ausstellen** , und klicken Sie auf **OK**.

11. Wenn Sie die Zertifikatsverwaltung mithilfe der virtuellen Smartcard verwenden, müssen Sie Datensammlungselemente deaktivieren, da diese nicht mit aktivierter Datensammlung funktioniert. Deaktivieren Sie die Datensammlung für jede Richtlinie, indem Sie im linken Bereich auf die Richtlinie klicken, dann das Kontrollkästchen neben **Beispieldatenelement** deaktivieren und schließlich auf **Datensammlungselemente löschen**klicken. Klicken Sie dann auf **OK**.


<!--HONumber=Apr16_HO2-->


