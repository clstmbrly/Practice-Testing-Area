---
layout: default
title: Authentication Mechanism Assurance
collection: networkconfig
permalink: networkconfig/AMA/
---
**Add draft paragraphs in from 9/22/2017**

**IG paragraph** As a system administrator, you may need to grant higher privilege levels to certain users. **This provides for greater protections when a user has higher level privileges?** For increased security, you can use the Authentication Mechanism Assurance (AMA) feature of Microsoft Windows Active Directory (AD) Domain Services (DS) for Windows Server 2008 R2 and 2012. When you enable AMA, it will allow you to increase a user's privileges by adding a global group membership, based on the PIV's certificate policy, and to their Kerberos token when they log into the agency's enterprise(?) their PIV cards.

From Centrify:  So how do we force the use of smart cards for everything and eliminate passwords?  One method is to modify a user’s account in Active Directory by selecting the “smart card is required for interactive logon” checkbox, or by doing this for all users via an AD Group Policy.  
Authentication Mechanism Assurance (AMA) places a user into a dynamic user group whenever they authenticate using a smart card, so therefore it knows the difference between Kerberos tickets obtained via a password and those obtained via a smart card.  Again the ultimate goal is usually to enforce smart card authentication directly from within the Active Directory account itself.


{% include info-alert.html content=" AMA does not offer an option to require a specific login method (e.g., PIV login)." %}

**CB Draft Text -- need to fix cert policy and OID words and examples**

Access to an agency SharePoint site so he can read specific documents...

So here's how that would work. Let's say that Joe needs Read-Only access to a SharePoint site to view specific documents and Julie needs Write acccess to this SharePoint site. For both Joe and Julie, you'd add these policies and OIDs to Windows AD: 

* Joe's PIV-I must contain the Object Identifier (OID) plus certificate policy of _2.16.840.1.101.3.2.1.12.6 id-eca-medium-hardware-pivi_. 
* Julie's PIV must contain the OID plus certificate policy of _2.16.840.1.101.3.2.1.3.16 id-fpki-common-high_. 

So, for both Joe and Julie, you'll need to add<!--add?--> these two policies to Windows AD by using one of the methods below. Then, you'll assign group memberships based on the policy OIDs so Joe can read and Julie can write to these SharePoint documents.<!--Where is SharePoint located? Does it matter? Cloud, https website, shared drive?-->

**IG paragraph** As an example, you want to allow Joe read access to sharepoint documents if Joe has a certificate with a policy OID 2.16.840.1.101.3.2.1.12.6 and policy name 'id-eca-medium-hardware-pivi'. You also want Julie to have write privileges since she has a certificate with a policy OID 2.16.840.1.101.3.2.1.3.16 and name id-fpki-common-high. You will setup these 2 policies in AD using any of the available methods listed below and then assign group memberships based on these policy OIDs to read and write access to sharepoint.

{% include info-alert.html content=" Do not use AMA to provide privileged access to servers." %}

## Windows Server® 2012 AD DS and Later

* Use the Windows Registry Editor to set the _AMA Priority_ above _Most Recently Issued Superior Certificate Heuristic_:

            [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\kdc]
            "ChainWithIssuancePolicyOIDs"=dword:00000001

* Use this PowerShell script for the Federal Common and DoD Certificate Policies, which simplifies Microsoft TechNet's AMA steps for Windows Server 2008 R2:&nbsp;&nbsp;[AMA Certificate Issuance OIDs](https://github.com/GSA/ficam-scripts/tree/auth-mech-assurance/_AMA){:target="_blank"}. <!--This link will change after the pull request is merged with staging.  "C.S.' stated "TechNet article for Windows Server 2012" was wrong. I can find no such TechNet article for Windows Server 2012. The only TechNet article about AMA is for Windows Server 2008 R2, after extensive searching. Does the TechNet Windows Server 2008 R2 article apply also to Windows Server 2012? Maybe the only difference in AMA implementation between 2012 and 2008 R2 is that 2008 R2 requires the KDC Patch and MS fixed this for 2012...?-->

## Windows Server® 2008 R2 AD DS

* Go to:&nbsp;&nbsp;[Windows Server 2008 R2 AD DS Step-by-Step Guide](https://technet.microsoft.com/en-us/library/dd378897(v=WS.10).aspx)[:target=_"blank"}.
* Set the _Domain Functional Level_ to _Windows Server 2008 R2_.
* Add a group membership to the user’s Kerberos token. When a user's PIV login is authenticated, it will activate the group membership.

### Hotfix for Windows® 2008 R2 Key Distribution Center (KDC) Error

* A Windows Server® 2008 R2-based Domain Hotfix corrects the known KDC error, "Access tokens are not updated correctly when you enable authentication mechanism assurance in a Windows Server 2008 R2-based domain." [Microsoft Hotfix](https://support.microsoft.com/en-us/help/2771254/access-tokens-are-not-updated-correctly-when-you-enable-authentication){:target="_blank"}. 

