---
layout: default
title: Announcements
permalink: /announcements/
---

### Microsoft and Google Changes Impact Intranet/Internet Access to Mission-Critical Systems

* [Affected federal agencies](#affected-federal-agencies)
* [When](#when)
* [Potential impacts](#potential-impacts)
* [Description of changes](#description-of-changes)
* [Options for agency response and required action](#options-for-agency-response-and-required-action)

### Affected Federal Agencies

If your agency uses the following, it is affected:

* Microsoft Edge/Internet Explorer (IE)
* Google Chrome
* FPKI CA-issued server authentication (Secure Sockets Layer [SSL]) certificates for intranet or internet websites

These changes could impact 14 Federal Executive Branch Agencies<!--correct?-->.  

### When

Starting April 2018.

### Potential Impacts

With these changes, federal employees who use Windows OSs could lose access to intranet/internet websites (i.e., 404 errors) and mission-critical systems. Lack of access to critical resources could affect agency mission, operations, and budget.

### Description of Changes
<!--This doesn't say anything about Google's technical issue, if there is one.-->
Microsoft globally distributes the Federal Government's FPKI Federal Common Policy Certificate Authority (FCPCA) Root (aka, COMMON) certificate for its products through the Microsoft trust store. Microsoft will continue doing this if the FPKI meets new policy requirements for how our CAs operate, maintain, and issue certificates. **Relates to Secure Sockets Layer [SSL] server auth certs** 

{% include alert-info.html heading="Agencies use SSL certificates to secure intranet and internet (public-facing) websites to implement HTTPS, per BOD 18-01.<sup>[1](#1)</sup>" %} 

### Options for Agency Response and Required Action
<!--This information doesn't say anything about responding to Google.-->
Agencies must respond to one of two options. Please send your feedback and any agency impacts or concerns, no later than **_January 26, 2018_**, to **_fpki@gsa.gov_**. 

#### Option 1 (Recommended)

**FPKI instructs Microsoft to remove the FCPCA (COMMON) Root certificate trust bit from the Microsoft trust store.**

* **What affect could this have on agency operations and missions?** Federal users will get 404 errors when trying to access intranet and internet websites _for which SSL certificates historically been used_.

* **What actions must agencies take to limit impacts on operations and access to mission-critical systems/applications?**
Agency network domain administrators must distribute new group policies to restore the _pre-change_ behavior to Microsoft OS-based, government-managed equipment.  

##### OPTION 1 FAQs

1. _Do I need to remove the baked-in version of the FCPCA Root certificate somehow?_  No, do not remove FCPCA Root certificate if it is already installed.
2. _Do I need only add the FCPCA (COMMON) Root certificate to the “Trust Root Certification Authorities” store via GPO, or should I add it to the enterprise trust store?_  If FCPCA (COMMON) is already installed, you don't need to reinstall or change its root store. However, if COMMON is not installed, follow the _PIV Guides_' "Network Authentication" steps: <https://piv.idmanagement.gov/networkconfig/>
3. _Does any trust bit manipulation need to be done for the GPO?_ **Specific instructions to follow.**
4. _Is only Windows 10 affected, or is Windows Server 2016 or other legacy client-server OS affected?_ All versions of Windows are affected. 
5. _Could this affect IPSec certificates when the server authentication bit is enabled and when used with Microsoft OSs?_ Yes, this affects any certificate asserting server authentication.

#### Option 2 (Greatest potential impact on agency operations and mission-critical systems)  

**Microsoft continues to distribute the FCPCA (COMMON) Root CA certificate with the enabled server authentication trust bit, but with an added _domain constraint_.**

The added _domain constraint_ will display a browser error in Microsoft Edge/IE or Google Chrome for any server authentication certificate that validates to FCPCA (COMMON) Root, if it doesn't include a fully qualified domain name: _.gov_, _.us_, _.mil_, or IP address. Agency network domain administrators cannot modify this constraint. It would be enforced globally through the Microsoft Certificate Trust List (CTL). Some agencies have identified Option 2 as detrimental to mission operations in the near-term, because intranet domain name aliases are used in currently issued certificates (e.g., intranetapp vs. intranetapp.agency.gov).

#### Microsoft Certificate Trust Lists (CTL) recommended reading

To prepare for these changes, please review these Microsoft documents:
1. [Microsoft Trusted Root Government CA Requirements](https://social.technet.microsoft.com/wiki/contents/articles/31635.microsoft-trusted-root-certificate-program-audit-requirements.aspx#Government_CA_Requirements)
2. [CTL Overview](https://msdn.microsoft.com/en-us/library/windows/desktop/aa376545(v=vs.85).aspx)
2. [How to configure CTL](https://technet.microsoft.com/en-us/library/dn265983.aspx)

-------
<a name="1">1</a>. Binding Operational Operational Directive 18-01,_Enhance Email and Web Security_, U.S. Department of Homeland Security, October 16, 2017, [BOD 18-01](https://cyber.dhs.gov/assets/report/bod-18-01.pdf){:target=_"blank"}. Additional information at: [https://cyber.dhs.gov/]{:target=_"blank"}<br>





