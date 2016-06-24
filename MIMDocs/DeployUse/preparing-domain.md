---
# required metadata

title: Einrichten einer Domäne | Microsoft Identity Manager
description: Einrichten eines Active Directory-Domänencontrollers vor der Installation von MIM 2016
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Einrichten einer Domäne

>[!div class="step-by-step"]  
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

Microsoft Identity Manager (MIM) arbeitet mit Ihrer Active Directory-Domäne. Sie müssen Active Directory bereits installiert haben und sicherstellen, dass Sie in Ihrer Umgebung über einen Domänencontroller für eine Domäne verfügen, die Sie verwalten können.

Dieser Artikel führt Sie durch die Schritte, in denen Sie Ihre Domäne für eine Verwendung von MIM vorbereiten.

## Erstellen von Benutzerkonten und -gruppen

Alle Komponenten Ihrer MIM-Bereitstellung benötigen ihre eigenen Identitäten in der Domäne. Dazu gehören sowohl die MIM-Komponenten „Service“ und „Sync“ als auch SharePoint und SQL.

> [!NOTE]
> Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso. Ersetzen Sie diese durch eigene Namen und Werte. Beispiel:
> - Domänencontrollername: **mimservername**
> - Domänenname: **contoso**
> - Kennwort: **Pass@word1**

1. Melden Sie sich auf dem Domänencontroller als Domänenadministrator an (*z.B. Contoso\Administrator*).

2. Erstellen Sie die folgenden Benutzerkonten für MIM-Dienste. Starten Sie PowerShell, und geben Sie das folgende PowerShell-Skript ein, um die Domäne zu aktualisieren.

    ```
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    ```

2.  Erstellen Sie für alle Gruppen Sicherheitsgruppen.

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global       –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global         –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global          –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

3.  Fügen Sie SPNs hinzu, um die Kerberos-Authentifizierung für Dienstkonten zu aktivieren.

    ```
    setspn -S http/mimservername.contoso.local Contoso\SharePoint
    setspn -S http/mimservername Contoso\SharePoint
    setspn -S MIMService/mimservername.contoso.local Contoso\MIMService
    setspn -S MIMSync/mimservername.contoso.local Contoso\MIMSync
    ```

>[!div class="step-by-step"]  
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)


<!--HONumber=May16_HO3-->


