---
# required metadata

title: Einrichten eines Identitätsverwaltungsservers&#58; SQL Server 2014 | Microsoft Identity Manager
description: Installieren von SQL Server 2014 als Vorbereitung für die Installation von MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Einrichten eines Identitätsverwaltungsservers: SQL Server 2014

>[!div class="step-by-step"]

> [!NOTE]
> « Windows Server 2012 R2 SharePoint » Diese exemplarische Vorgehensweise verwendet Beispielnamen und -werte eines Unternehmens namens Contoso.
> - Ersetzen Sie diese durch eigene Namen und Werte.
> - Beispiel:
> - Domänencontrollername: **mimservername**

## Domänenname: **contoso**

1. Kennwort: **Pass@word1**

2. Installieren von **SQL Server 2014 Standard Edition**

3. Starten Sie **PowerShell** als Domänenadministrator.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>Navigieren Sie in das Verzeichnis, in dem sich das SQL Server-Setupprogramm befindet.  
Geben Sie die folgenden Befehle ein.


<!--HONumber=May16_HO3-->


