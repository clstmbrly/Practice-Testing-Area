---
layout: default
title: Announcements
permalink: /announcements/
---

## Microsoft and Google Changes Impact Federal Government Agencies

Microsoft has released Public Key Infrastructure (PKI) policy requirements that impact Federal Government agencies' missions, operations, and budget. By April 2018, federal users may receive errors from Windows when using Microsoft Edge/IE and Google Chrome to browse to intranet <!--and internet?-->websites that use FPKI CA-issued SSL certificates. Lack of access to mission-critical systems could affect missions, operations, and budgets. These changes will start in April 2018 and could impact 14 Federal Executive Branch Agencies<!--correct?-->. We propose two options for responding to Microsoft and Google.  (See below.)

{% include alert-info.html heading="Agencies use SSL certificates to secure intranet and internet (public-facing) websites to implement HTTPS, per BOD 18-01.<sup>[1](#1)</sup>" %} 

### Description of Changes
<!--This doesn't say anything about Google's technical issue, if there is one.-->
Microsoft globally distributes the Federal Government's FPKI Federal Common Policy Certificate Authority (FCPCA) Root (aka, COMMON) certificate for its products through the Microsoft trust store. Microsoft will continue doing this if the FPKI meets new policy requirements for how our CAs operate, maintain, and issue certificates. **Relates to Secure Sockets Layer [SSL] server auth certs** 

### Options for Agency Response and Required Action
<!--This information doesn't say anything about responding to Google.-->
Agencies must respond to one of two options. Please send your feedback and any agency impacts or concerns, no later than **_January 26, 2018_**, to **_fpki@gsa.gov_**. 

**Option 1. FPKI instructs Microsoft to remove the FCPCA (COMMON) Root certificate trust bit from the Microsoft trust store. (Recommended)**

* **What affect could this have on agency operations and missions?** Federal users may get 404 errors when trying to access intranet/ internet websites _for which SSL certificates have historically been used_.

* **What actions can we take to limit impacts?** Agency network domain administrators must distribute new group policies to restore the _pre-change_ behavior to Microsoft OS-based, government-managed equipment. (See below for _Option 1 FAQs_ and _Microsoft Certificate Trust Lists [CTL] recommended reading_.)  

**FAQs for Option 1**

1. _Do I need to remove the baked-in version of the FCPCA Root certificate?_  No, don't remove this certificate if it's already installed.
2. _Do I only need to add the FCPCA (COMMON) Root certificate to the “Trust Root Certification Authorities” store via GPO, or should I add it to the enterprise trust store?_  If FCPCA (COMMON) is already installed, you don't need to reinstall or change its root store. However, if it's not installed, follow the _PIV Guides_' "Network Authentication" steps: <https://piv.idmanagement.gov/networkconfig/>
3. _Do I need to change any trust bit for the GPO?_ **NOTE: Specific instructions to follow.**<!--Will these be added?-->
4. _Is only Windows 10 affected? What about Windows Server 2016 or other legacy client-server OSs?_ All versions of Windows are affected. 
5. _Could the GPO distribution affect IPSec certificates when the server authentication bit is enabled and when used with Microsoft OSs?_ Yes, this could affect any certificate asserting server authentication.<!--Correct interpretation? What does engineer do if there is a problem?-->

**Option 2. Microsoft continues to distribute the FCPCA (COMMON) Root CA certificate with the enabled server authentication trust bit, but with an added _domain constraint_. (Greatest potential impact on operations and mission-critical systems.)**

The added _domain constraint_ will display a browser error in Microsoft Edge/IE or Google Chrome for any server authentication certificate that validates to FCPCA (COMMON) Root, if it doesn't include a fully qualified domain name: _.gov_, _.us_, _.mil_, or IP address. Agency network domain administrators cannot modify this constraint. It would be enforced globally through the Microsoft Certificate Trust List (CTL). Some agencies have identified Option 2 as detrimental to mission operations in the near-term, because intranet domain name aliases are used in currently issued certificates (e.g., intranetapp vs. intranetapp.agency.gov).

#### Microsoft Certificate Trust Lists (CTL) recommended reading

To prepare for these changes, please review these Microsoft documents:
1. [Microsoft Trusted Root Government CA Requirements](https://social.technet.microsoft.com/wiki/contents/articles/31635.microsoft-trusted-root-certificate-program-audit-requirements.aspx#Government_CA_Requirements)
2. [CTL Overview](https://msdn.microsoft.com/en-us/library/windows/desktop/aa376545(v=vs.85).aspx)
2. [How to configure CTL](https://technet.microsoft.com/en-us/library/dn265983.aspx)

-------
<a name="1">1</a>. Binding Operational Operational Directive 18-01,_Enhance Email and Web Security_, U.S. Department of Homeland Security, October 16, 2017, [BOD 18-01](https://cyber.dhs.gov/assets/report/bod-18-01.pdf){:target=_"blank"}. Additional information at: [https://cyber.dhs.gov/]{:target=_"blank"}<br>





