---
# required metadata

title: Installieren von MIM 2016&#58; Synchronisieren von Active Directory und MIM-Dienst| Microsoft Identity Manager
description: Verwenden Sie Verwaltungs-Agents und MIM Synchronization Service, um Ihr Active Directory und Ihre MIM-Datenbanken zu synchronisieren.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Installieren von MIM 2016: Synchronisieren von Active Directory und MIM-Dienst

>[!div class="step-by-step"] 
[« MIM-Dienst und -Portal](install-mim-service-portal.md)

> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Domänencontrollername: **mimservername**
> - Domänenname: **contoso**
> - Kennwort: **Pass@word1**

Standardmäßig sind für MIM Synchronization Service (Sync) keine Connectors konfiguriert.  Ein typischer erster Schritt besteht darin, die MIM-Dienstdatenbank mithilfe von MIM Sync mit vorhandenen Active Directory-Konten aufzufüllen. Dazu verwenden Sie die MIM-Synchronisierungsdienstanwendung.

## Erstellen des MIM-Verwaltungs-Agents
Der MIM-Verwaltungs-Agent (Management Agent; MA) verbindet MIM Sync mit dem MIM-Dienst. Um diesen Connector zu erstellen, verwenden Sie den Assistenten "Verwaltungs-Agent erstellen".

Wenn Sie einen MIM-Verwaltungs-Agent konfigurieren, müssen Sie ein Benutzerkonto angeben. Dieses Dokument verwendet **MIMMA** als Namen für dieses Konto.

> [!NOTE] Das Konto, das Sie für Ihren MIM-Verwaltungs-Agent verwenden, muss dasselbe Konto sein wie das Konto, das Sie während der Installation des MIM-Diensts angegeben haben.

###So erstellen Sie den MIM MA

1.  Öffnen Sie den Synchronisierungsdienst-Manager.

2.  Um den Assistenten zum Erstellen des Verwaltungs-Agents zu öffnen, klicken Sie im Menü **Aktionen** auf **Erstellen**.

3.  Geben Sie auf der Seite **Verwaltungs-Agent erstellen** die folgenden Einstellungen an, und klicken Sie dann auf **Weiter**.

    -   Verwaltungs-Agent für: MIM-Dienstverwaltungs-Agent

    -   Name: MIMMA

4.  Geben Sie auf der Seite **Mit Datenbank verbinden** die folgenden Einstellungen an, und klicken Sie dann auf **Weiter**

    -   Server: localhost

    -   Datenbank: MIMService

    -   MIM-Dienstbasisadresse: http://localhost:5725

    -   Authentifizierungsmodus: Integrierte Windows-Authentifizierung

    -   Benutzername: mimma

    -   Kennwort: Pass@word

    -   Domäne: contoso

5.  Überprüfen Sie auf der Seite **Ausgewählte Objekttypen**, ob die Objekttypen ausgewählt sind, die unten aufgelistet sind, und klicken Sie dann auf **Weiter**

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Person

    -   Gruppe

6.  Stellen Sie auf der Seite **Ausgewählte Attribute** sicher, dass alle aufgeführten Attribute ausgewählt sind, und klicken Sie dann auf **Weiter**.

7.  Klicken Sie auf der Seite **Connector-Filter konfigurieren** auf **Weiter**.

8.  Fügen Sie auf der Seite **Objekttypzuordnungen konfigurieren** die folgende Zuordnung hinzu, und klicken Sie dann auf **Weiter**

    - Wählen Sie in der Liste **Objekttyp der Datenquelle** den Eintrag **Person** aus.
    - Klicken Sie auf **Zuordnung hinzufügen**, um das Dialogfeld „Zuordnung“ zu öffnen.
    - Wählen Sie in der Liste **Metaverse-Objekttyp** den Eintrag **Person** aus.
    - Klicken Sie auf **OK**, um das Dialogfeld „Zuordnung“ zu schließen.

9.  Wenden Sie auf der Seite **Attributfluss konfigurieren** die folgenden Attributflusszuordnungen an, und klicken Sie dann auf **Weiter**

    | **Datenquellenattribut** | **Flussrichtung** | **Metaverse-Attribut** |
    |-|-|-|
    | AccountName | Exportieren | accountName |
    | DisplayName | Exportieren | displayName |
    | Domain | Exportieren | Domäne |
    | EmployeeID | Exportieren | employeeID |
    | EmployeeTyp | Exportieren | employeeTyp |
    | E-Mail | Exportieren | mail |
    | FirstName | Exportieren | firstName |
    | LastName | Exportieren | lastName |
    | ObjectSID | Exportieren | objectSid |

10.  Wählen Sie als Objekttyp der Datenquelle **Person** aus.

    -   Wählen Sie als Metaverse-Objekttyp **Person** aus.

    -   Wählen Sie als Zuordnungstyp **Direkt** aus.

    -   Führen Sie für jede Zeile in der vorherigen Tabelle folgende Schritte aus:

        -   Wählen Sie die für diese Zeile in der Tabelle angezeigte **Flussrichtung** aus.

        -   Wählen Sie das für diese Zeile in der Tabelle angezeigte **Datenquellenattribut** aus.

        -   Wählen Sie das für diese Zeile in der Tabelle angezeigte **Metaverse-Attribut** aus.

        -   Um die Flusszuordnung anzuwenden, klicken Sie auf **Neu**.

    -   Wählen Sie **Gruppe** als Datenquellentyp und als Metaverse-Objekttyp aus.

    -   Wählen Sie als Zuordnungstyp **Direkt** aus.

    -   Führen Sie für jede Zeile in der folgenden Tabelle diese Schritte aus:

        -   Wählen Sie die für diese Zeile in der Tabelle angezeigte **Flussrichtung** aus.

        -   Wählen Sie das für diese Zeile in der Tabelle angezeigte **Datenquellenattribut** aus.

        -   Wählen Sie das für diese Zeile in der Tabelle angezeigte **Metaverse-Attribut** aus.

        -   Um die Flusszuordnung anzuwenden, klicken Sie auf **Neu**.

    | **Datenquellenattribut** | **Flussrichtung** | **Metaverse-Attribut** |
    |-|-|-|
    | AccountName | Exportieren | accountName |
    | DisplayName | Exportieren | displayName |
    | Domain | Exportieren | Domäne |
    | E-Mail | Exportieren | mail |
    | MailNickName | Exportieren | mailNickName |
    | Mitglied | Exportieren | Element |
    | ObjectSID | Exportieren | objectSid |
    | Bereich | Exportieren | scope |
    | Typ | Exportieren | type |
    | MitgliedshipAddWorkflow | Exportieren | membershipAddWorkflow |
    | MitgliedshipLocked | Exportieren | membershipLocked |
    | DisplayName | importieren | displayName |
    | Bereich | importieren | scope |
    | Typ | importieren | type |
    | Mitglied | importieren | Element |
    | AccountName | importieren | accountName |
    | DisplayedOwner | importieren | displayedOwner |
    | MailNickName | importieren | mailNickName |


11.  Klicken Sie auf der Seite **Aufheben der Bereitstellung konfigurieren** auf **Weiter**

12.  Um den Verwaltungs-Agent zu erstellen, klicken Sie auf der Seite **Erweiterungen konfigurieren** auf **Fertig stellen**.

## Erstellen des AD-Verwaltungs-Agents
Der Active Directory-Verwaltungs-Agent ist ein Connector für Active Directory-Domänendienste. Um diesen Connector zu erstellen, verwenden Sie den Assistenten "Verwaltungs-Agent erstellen".

1. Um den Assistenten zum Erstellen des Verwaltungs-Agents zu öffnen, klicken Sie im Menü **Aktionen** auf **Erstellen**.

2. Geben Sie auf der Seite **Verwaltungs-Agent erstellen** die folgenden Einstellungen an, und klicken Sie dann auf **Weiter**:

    - Verwaltungs-Agent für: Active Directory-Domänendienste (AD DS)
    - Name: ADMA

3. Geben Sie auf der Seite **Mit Active Directory-Gesamtstruktur verbinden** die folgenden Einstellungen an, und klicken Sie dann auf **Weiter**:

    - Gesamtstrukturname: contoso.local
    - Benutzername: administrator
    - Kennwort: &lt;das Kontokennwort&gt;
    - Domäne: contoso

4. Geben Sie auf der Seite **Verzeichnispartitionen konfigurieren** die folgenden Einstellungen an, und klicken Sie dann auf **Weiter**:

    - Wählen Sie in der Liste **Verzeichnispartitionen auswählen** **DC = CONTOSO, DC =local** aus.

    - Um das Dialogfeld „Container auswählen“ zu öffnen, klicken Sie auf **Container**.

    - Falls Sie den Container ändern möchten, damit sich MIM-Verwaltungsobjekte sich nur in einem bestimmten Container befinden, klicken Sie auf den **DC = CONTOSO, DC =local**-Knoten, und klicken Sie dann auf den Knoten für den betreffenden Container.

    - Um das Dialogfeld „Container auswählen“ zu schließen, klicken Sie auf **OK**.

5. Klicken Sie auf der Seite **Bereitstellungshierarchie konfigurieren** auf **Weiter**.

6. Geben Sie auf der Seite **Objekttypen auswählen** die folgenden Einstellungen an, und klicken Sie dann auf **Weiter**:

    - Wählen Sie in der Liste **Objekttypen** **Benutzer** und **Gruppe** aus.

7. Geben Sie auf der Seite **Attribute auswählen** die folgenden Einstellungen an, und klicken Sie dann auf **Weiter**:

    - Wählen Sie **Alles anzeigen** aus.

8. Wählen Sie in der Liste **Attribute** die folgenden Attribute aus:

    -   company
    -   displayName
    -   employeeID
    -   employeeTyp
    -   givenName
    -   groupTyp
    -   manager
    -   managedBy
    -   Element
    -   objectSid
    -   sAMAccountName
    -   sAMAccountTyp
    -   sn
    -   unicodePwd
    -   userAccountControl

9. Klicken Sie auf der Seite **Connector-Filter konfigurieren** auf **Weiter**.

10. Klicken Sie auf der Seite **Zusammenführungs- und Projektionsregeln konfigurieren** auf **Weiter**.

11. Klicken Sie auf der Seite **Attributfluss konfigurieren** auf **Weiter**.

12. Klicken Sie auf der Seite **Aufheben der Bereitstellung konfigurieren** auf **Weiter**.

13. Klicken Sie auf der Seite **Erweiterungen konfigurieren** auf **Fertig stellen**.


## Erstellen von Ausführungsprofilen

Erstellen Sie Ausführungsprofile für die ADMA- und MIMMA-Connectors.

### Erstellen von Ausführungsprofilen für den ADMA-Connector

Diese Tabelle zeigt Ihnen die fünf Ausführungsprofile, die Sie für den ADMA-Connector erstellen:

| Name | Typ |
| ---- | ---- |
| Profil1 | Vollständiger Import (nur Bereitstellung) |
| Profil2 | Vollständige Synchronisierung |
| Profil3 | Deltaimport (nur Bereitstellung) |
| Profil4 | Deltasynchronisierung |
| Profil5 | Exportieren |

So erstellen Sie Ausführungsprofile für den ADMA-Connector

1. Öffnen Sie den Synchronization Service Manager, und klicken Sie im Menü **Extras** auf **Verwaltungs-Agents**.

2. Wählen Sie in der Liste **Verwaltungs-Agents** den Eintrag **ADMA** aus.

3. Um das Dialogfeld „Ausführungsprofile konfigurieren“ zu öffnen, klicken Sie im Menü **Aktionen** auf **Ausführungsprofile konfigurieren**.

4. Schließen Sie die folgenden Schritte für jedes der Ausführungsprofile in der Tabelle ab:

    - Um den Assistenten „Ausführungsprofile konfigurieren“ zu öffnen, klicken Sie auf **Neues Profil**.

    - Geben Sie im Feld **Name** den Profilnamen aus der Tabelle ein, und klicken Sie auf **Weiter**.

    - Wählen Sie in der Liste **Typ** den in der Tabelle angezeigten Schritttyp aus, und klicken Sie auf **Weiter**.

    - Klicken Sie auf **Fertig stellen**, um das Ausführungsprofil zu erstellen.

5. Um das Dialogfeld „Ausführungsprofile konfigurieren“ zu schließen, klicken Sie auf **OK**.

### Erstellen von Ausführungsprofilen für den MIMMA-Connector

Diese Tabelle zeigt die passenden fünf Ausführungsprofile für den MIMMA-Connector an:

| Name | Typ |
| -------- | -------- |
| Profil1 | Vollständiger Import (nur Bereitstellung) |
| Profil2 | Vollständige Synchronisierung |
| Profil3 | Deltaimport (nur Bereitstellung) |
| Profil4 | Deltasynchronisierung |
| Profil5 | Exportieren |

So erstellen Sie Ausführungsprofile für den MIMMA-Connector

1. Öffnen Sie den Synchronization Service Manager, und klicken Sie im Menü **Extras** auf **Verwaltungs-Agents**.

2. Wählen Sie in der Liste **Verwaltungs-Agents** den Eintrag **MIMMA** aus.

3. Um das Dialogfeld „Ausführungsprofile konfigurieren“ zu öffnen, klicken Sie im Menü **Aktionen** auf **Ausführungsprofile konfigurieren**.

4. Schließen Sie die folgenden Schritte für jedes der Ausführungsprofile in der Tabelle ab:

    - Um den Assistenten „Ausführungsprofile konfigurieren“ zu öffnen, klicken Sie auf **Neues Profil**.

    - Geben Sie im Feld **Name** den Profilnamen aus der Tabelle ein, und klicken Sie auf **Weiter**.

    - Wählen Sie in der Liste **Typ** den in der Tabelle angezeigten Schritttyp aus, und klicken Sie auf **Weiter**.

    - Klicken Sie auf **Fertig stellen**, um das Ausführungsprofil zu erstellen.

5. Um das Dialogfeld „Ausführungsprofile konfigurieren“ zu schließen, klicken Sie auf **OK**.

## Konfigurieren des MIM-Diensts

Erstellen Sie mithilfe des MIM-Portals für den MIM-Dienst eine Synchronisierungsregel „AD-Benutzer eingehend“.

So erstellen Sie die Synchronisierungsregel „AD-Benutzer eingehend“:

1. Klicken Sie auf der MIM-Portalstartseite in der Navigationsleiste auf **Administration**.

2. Um die Seite „Synchronisierungsregeln“ zu öffnen, klicken Sie auf **Synchronisierungsregeln**.

3. Um den Assistenten für die Erstellung der Synchronisierungsregel zu öffnen, klicken Sie auf der Symbolleiste auf **Neu**.

4. Geben Sie auf der Registerkarte **Allgemein** die folgenden Informationen an, und klicken Sie dann auf **Weiter**:

    -   Anzeigename: Synchronisierungsregel "AD-Benutzer eingehend"
    -   Datenflussrichtung: Eingehend

5. Geben Sie auf der Registerkarte **Bereich** die folgenden Informationen an, und klicken Sie dann auf **Weiter**:

    -   Metaverseressourcentyp: person
    -   Externes System: ADMA
    -   Externer Systemressourcentyp: person

6. Geben Sie auf der Registerkarte **Beziehung** die folgenden Informationen an, und klicken Sie dann auf **Weiter**:

    -   Wählen Sie zum Konfigurieren der Beziehungskriterien **ObjectSID** aus der Liste „MetaverseObject:person(Attribute)“ und aus der Liste „ConnectedSystemObject:person(Attribute)“ aus.

    -   Wählen Sie **Ressource in MIM erstellen** aus.

7. Geben Sie auf der Seite **Eingehender Attributfluss** die folgenden Informationen an, und klicken Sie dann auf **Weiter**:

    | Flussregel | Quelle | Ziel |
    |-|-|-|
    |Regel 1|samAccountName|f|
    |Regel 2|displayName|displayName|
    |Regel 3|EmployeeTyp|EmployeeTyp|
    |Regel 4|givenName|givenName|
    |Regel 5|sn|lastName|
    |Regel 6|Manager|manager|
    |Regel 7|objectSID|ObjectSID|
    |Regel 8|"Contoso"|Domäne|

    Führen Sie für jede Zeile in dieser Tabelle die folgenden Schritte aus:

    - Um das Dialogfeld „Flussdefinition“ zu öffnen, klicken Sie auf **Neuer Attributfluss**.

    - Wählen Sie auf der Registerkarte **Quelle** das für diese Zeile in der Tabelle angezeigte Attribut aus.

    - Wählen Sie auf der Registerkarte **Ziel** das für diese Zeile in der Tabelle angezeigte Attribut aus.

    - Um die Attributflusskonfiguration anzuwenden, klicken Sie auf **OK**.

8. Klicken Sie in der Registerkarte **Zusammenfassung** auf **Absenden**.

## Initialisieren der Testumgebung
Vier Schritte sind erforderlich, bevor Sie die MIM-Konfiguration mit Active Directory-Daten testen können:

### Aktivieren der Bereitstellung

1. Öffnen Sie den Synchronisierungsdienst-Manager.

2. Um das Dialogfeld „Optionen“ zu öffnen, klicken Sie im Menü **Extras** auf **Optionen**.

3. Wählen Sie **Synchronisierungsregelbereitstellung aktivieren** aus.

4. Um das Dialogfeld „Optionen“ zu schließen, klicken Sie auf **OK**.

### Initialisieren des MIMMA

Führen Sie einen vollständigen Synchronisierungszyklus auf diesem Connector aus. Der vollständige Zyklus besteht aus den folgenden Ausführungsprofilen:

- Vollständiger Import
- Vollständige Synchronisierung
- Exportieren
- Deltaimport

Führen Sie diese Schritte aus, um alle vier Ausführungsprofile auszuführen.

1. Öffnen Sie den Synchronization Service Manager, und klicken Sie im Menü **Extras** auf **Verwaltungs-Agents**.

2. Wählen Sie in der Liste **Verwaltungs-Agents** den Eintrag **MIMMA** aus.

3. Um das Dialogfeld „Verwaltungs-Agent ausführen“ zu öffnen, klicken Sie im Menü **Aktionen** auf **Ausführen**.

4. Führen Sie jeden der folgenden Schritte für jedes oben aufgeführte Ausführungsprofil aus:

    - Um das Dialogfeld „Verwaltungs-Agent ausführen“ zu öffnen, klicken Sie im Menü **Aktionen** auf **Ausführen**.

    - Wählen Sie in der Liste **Ausführungsprofile** das Ausführungsprofil aus, das Sie ausführen möchten.

    - Um das Ausführungsprofil zu starten, klicken Sie auf **OK**.

#### Konfigurieren der Attributflussrangfolge

Während der Initialisierung des MIM-Connectors wurden die konfigurierten Synchronisierungsregeln in die Metaverse übertragen.

Passen Sie die Attributflussrangfolge für die von diesem Connector beigetragenen Attribute an, um sicherzustellen, dass bereits in AD vorhandene Attribute in die Metaverse und später auch in die MIM-Dienstdatenbank fließen können.

### Initialisieren des ADMA

Um den Active Directory Connector zu initialisieren, müssen Sie einen vollständigen Import und eine vollständige Synchronisierung für diesen ausführen. Der vollständige Import überträgt die vorhandenen Objekte aus AD in den Connectorbereich. Bei der vollständigen Synchronisierung werden die Synchronisierungsregeln aktualisiert, damit sie mit denjenigen des MIM-Connectors übereinstimmen.

1. Öffnen Sie den Synchronisierungsdienst-Manager, und klicken Sie im Menü **Extras** auf **Verwaltungs-Agents**.

2. Wählen Sie in der Liste **Verwaltungs-Agents** den Eintrag **ADMA** aus.

3. Um das Dialogfeld „Verwaltungs-Agent ausführen“ zu öffnen, klicken Sie im Menü **Aktionen** auf **Ausführen**.

4. Führen Sie jeden der folgenden Schritte für jedes oben aufgeführte Ausführungsprofil aus:

    - Um das Dialogfeld „Verwaltungs-Agent ausführen“ zu öffnen, klicken Sie im Menü **Aktionen** auf **Ausführen**.

    - Wählen Sie in der Liste **Ausführungsprofile** das Ausführungsprofil aus, das Sie ausführen möchten.

    - Um das Ausführungsprofil zu starten, klicken Sie auf **OK**.

### Auffüllen der MIM-Dienstdatenbank

Um die MIM-Dienstdatenbank mit den Objekten aufzufüllen, müssen Sie einen Synchronisierungszyklus für den MIMMA-Connector ausführen. Der Zyklus besteht aus:

- Exportieren
- Vollständiger Import
- Vollständige Synchronisierung

Führen Sie diese Schritte aus, um alle drei Ausführungsprofile auszuführen.

1. Öffnen Sie den Synchronisierungsdienst-Manager, und klicken Sie im Menü **Extras** auf **Verwaltungs-Agents**.

2. Wählen Sie in der Liste **Verwaltungs-Agents** den Eintrag **MIMMA** aus.

3. Klicken Sie im Menü **Aktionen** auf **Ausführen**, um das Dialogfeld „Verwaltungs-Agent ausführen“ zu öffnen.

4. Führen Sie jeden der folgenden Schritte für jedes oben aufgeführte Ausführungsprofil aus:

    - Klicken Sie im Menü **Aktionen** auf **Ausführen**, um das Dialogfeld „Verwaltungs-Agent ausführen“ zu öffnen.
    - Wählen Sie aus der Liste **Ausführungsprofile** das Ausführungsprofil aus, das Sie ausführen möchten.
    - Klicken Sie auf **OK**, um das Ausführungsprofil zu starten.

>[!div class="step-by-step"] 
[« MIM-Dienst und -Portal](install-mim-service-portal.md)


<!--HONumber=Jun16_HO1-->


