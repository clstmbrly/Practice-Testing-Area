---
layout: default
title: Announcements
permalink: /announcements/
---

## New Microsoft and Google Policies Set To Impact the Federal Government
<!--New requirements don't say anything about Google.-->
Microsoft recently issued new Public Key Infrastructure (PKI) policy requirements set to impact 14 federal agencies' operations and budgets.<!--Missions also?--> The FPKI must meet Microsoft's requirements for how we operate, maintain, and issue certificates from our Federal PKI Certification Authorities. If the FPKI does not comply, in April 2018, Windows users will get errors when browsing with Microsoft Edge/IE or Chrome to intranet websites that use FPKI CA-issued SSL certificates.

{% include alert-info.html heading="Agencies use SSL certificates to secure intranet and public-facing, internet websites, per HTTPS mandate (BOD 18-01.<sup>[1](#1)</sup>)" %} 

We propose two options for responding to Microsoft and Google. 

### Two Options for Federal Agencies' Response to Microsoft<!--Does this have to be a unified government response?-->
<!--This information doesn't say anything about responding to Google.-->
Please recommend Option 1 or 2 and send any agency impacts or concerns by **January 26, 2018** to **_fpki@gsa.gov_**. 

> **Option 1 (Recommended).&nbsp;&nbsp;FPKI instructs Microsoft to remove the FCPCA (COMMON) Root certificate trust bit from the Microsoft trust store.**

* **Result:** Your users will get errors when browsing with Microsoft Edge/IE or Chrome to intranet<!--internet also?--> websites that use FPKI CA-issued SSL certificates.

* **How can we limit this impact?** Network domain administrators must distribute new group policies to restore the _pre-change_ behavior to Microsoft OS-based, government-managed equipment. (See _Option 1 FAQs_ and _Microsoft Certificate Trust Lists [CTL] recommended reading_.) 

**FAQs for Option 1**

1. Do I need to remove the baked-in version of the FCPCA Root certificate?<br>
> _No, don't remove this certificate if it's already installed._
2. Do I only need to add the FCPCA (COMMON) Root certificate to the “Trust Root Certification Authorities” store via GPO, or should I add it to the enterprise trust store?<br> 
> _If FCPCA (COMMON) is already installed, you don't need to reinstall or change its root store. However, if it's not installed, follow the PIV Guides_' "Network Authentication" steps: <https://piv.idmanagement.gov/networkconfig/>
3. Do I need to change any trust bit for the GPO?<br>
> **NOTE: Specific instructions to follow.**<!--Will these be added?-->
4. What Windows versions are affected?<br> 
> _All Windows versions (e.g., Windows 10, Server 2016, legacy client-server OSs)._
5. Will the GPO distribution affect IPSec certificates if the server authentication bit is enabled and when used with Microsoft OSs?<br> > _Yes, this could affect any certificate asserting server authentication._<!--Correct interpretation? What does engineer do if there is a problem?-->

> **Option 2 (Greatest potential impact on operations and mission-critical systems).&nbsp;&nbsp;Microsoft continues to distribute the FCPCA (COMMON) Root CA certificate with the server authentication trust bit enabled, but with an added _domain constraint_.**

* **Result 1:** With the added _domain constraint_, your users will get errors from Microsoft Edge/IE or Chrome for any server authentication certificate that validates to FCPCA (COMMON) Root, if it doesn't include a fully qualified domain name: _.gov_, _.us_, _.mil_, or IP address. The Microsoft Certificate Trust List (CTL) globally enforces this constraint through the Microsoft Certificate Trust List (CTL). Network domain administrators can't modify this constraint. 

* **Result 2:** Some agencies have identified Option 2 as detrimental to mission operations in the near-term, because issued certificates use intranet domain name aliases (e.g., intranetapp vs. intranetapp.agency.gov).

#### Microsoft Certificate Trust Lists (CTL) recommended reading

To prepare for these changes, please review these Microsoft documents:
1. [Microsoft Trusted Root Government CA Requirements](https://social.technet.microsoft.com/wiki/contents/articles/31635.microsoft-trusted-root-certificate-program-audit-requirements.aspx#Government_CA_Requirements)
2. [CTL Overview](https://msdn.microsoft.com/en-us/library/windows/desktop/aa376545(v=vs.85).aspx)
2. [How to configure CTL](https://technet.microsoft.com/en-us/library/dn265983.aspx)

-------
<a name="1">1</a>. Binding Operational Operational Directive 18-01,_Enhance Email and Web Security_, U.S. Department of Homeland Security, October 16, 2017, [BOD 18-01](https://cyber.dhs.gov/assets/report/bod-18-01.pdf){:target=_"blank"}. Additional information at: [https://cyber.dhs.gov/]{:target=_"blank"}<br>





