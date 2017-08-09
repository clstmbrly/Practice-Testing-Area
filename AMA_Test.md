---
layout: default
title: Authentication Mechanism Assurance
collection: networkconfig
permalink: networkconfig/AMA/
---

# Authentication Mechanism Assurance

Requiring PIV login for all users who access sensitive resources is essential for information security. 
You can protect sensitive resources even more by using Windows Active Directory (AD) domain service’s Authentication Mechanism Assurance (AMA).
AMA, when used with PIV login, adds a group membership to a user's security identifier attributes (SIDs). The group membership grants that user access to a sensitive resource.
Windows versions that include AMA and configuration guidelines are below:
## Specific Implementations
* **Windows Server 2012 and Later &mdash;** No patch is required.  Enable _AMA Priority_ above _Most Recently Issued Superior Certificate Heuristic_ by using the Windows Registry Editor:

            [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\kdc]
            "ChainWithIssuancePolicyOIDs"=dword:00000001

* Power Shell Script for Common Federal/DoD Certificate Policies simplifies this implementation as compared to TechNet Step-by-Step Guide:  
    
    https://github.com/GSA/piv-guides/files/621976/CertificateIssuanceOIDs.ps1.txt
    
* **Windows Server 2008 R2 AD DS** &mdash; When the domain functional level is set to _Windows Server 2008 R2_, you can use AMA. AMA adds an administrator-designated, global group membership to a user’s Kerberos token when the user’s credentials are authenticated during a PIV login.

* Windows Server 2008 R2 AD DS 

    https://technet.microsoft.com/en-us/library/dd378897(v=WS.10).aspx

   The Windows Server® 2008 R2 patch to correct the KDC not setting the certificate issuance policy when the KDC validates the smartcard certificate is now available via the following link. 

    http://support.microsoft.com/kb/2771254

