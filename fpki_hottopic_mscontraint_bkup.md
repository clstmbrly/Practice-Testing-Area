---
layout: default
title: Announcements
permalink: /announcements/
---

## Microsoft Government Domain Constraints

Microsoft has released Public Key Infrastructure (PKI) policy requirements that impact Federal Government Agencies from an operational and cost perspective. One of the requirements will take effect in April 2018 and will impact SSL certificates issued by CAs in the Federal PKI. Agencies use SSL certificates to secure internal and public-facing websites to implement HTTPS as per DHS Binding Operational Directive 18-01. As a result, by April 2018, agency employees and contractors may receive errors when browsing to internal (intranet) websites that uses SSL certificates from Federal PKI CAs, Windows 10, and Microsoft Internet Explorer. 

Summaries of the requirement and two options are below. Please send your feedback on the recommendation to proceed with Option 1, and any additional agency impacts or concerns, to fpki@gsa.gov by January 26, 2018.  

### Summary: 
The Federal Government COMMON root CA Certificate is globally distributed in Microsoft products through their trust store. To continue having our root CA certificate distributed in the Microsoft trust store, the Federal PKI is required to meet Microsoft’s policy requirements for how we operate, maintain, and issue certificates from our Federal PKI Certification Authorities.  

There are two options agencies can implement to comply by April 2018. Based on initial agency feedback, Option 1 will have the least impact on mission-critical applications and operations. Option 1 will require agency network domain administrators to distribute new group policies to government managed Microsoft OS devices.  

**Option 1: The Federal PKI instructs Microsoft to remove the server authentication trust bit from the Federal Government COMMON root CA certificate that is distributed through the Microsoft trust store.** 

This change impacts agencies who have issued and use server authentication (SSL) certificates for internal (intranet) websites or public websites if the server authentication certificates were issued from any of the Federal PKI Certificate Authorities that validate to COMMON. The Federal PKI community estimates there are fourteen (14) agencies impacted.  The impacted agencies will have the ability to use network domain group policies to restore the pre-change behavior for government managed equipment, and limit impact to mission operations. 

**Option 2:  Microsoft continues to distribute COMMON root CA certificate with the server authentication trust bit enabled but with the addition of a domain constraint.**

This constraint will display a browser error in Microsoft Edge/IE or Google Chrome for any server authentication certificates validating to COMMON if it does not include fully qualified domain names ending in .gov, .us, or .mil or an IP addresses. This constraint can not be modified by agency network domain administrators and would be enforced globally through the Microsoft Certificate Trust List (CTL). Some of the fourteen (14) impacted agencies have identified Option 2 as detrimental to mission operations in the near-term due to the use of intranet domain name aliases in currently issued certificates (for example: intranetapp versus intranetapp.agency.gov).

### Frequently Asked Questions for Option 1 - Disable Server Authentication for COMMON
1. Do we need to remove the baked in version of the FCPCA root somehow?
- No, do not remove COMMON if it is already installed.
2. Do we only need to add the FCPCA root to “Trust Root Certification Authorities” store via GPO or do we need to add it to the Enterprise Trust store?
- If COMMON is already installed, no need to reinstall or change its root store.
- If COMMON is not installed, follow the PIV Guide - Network Authentication directions <https://piv.idmanagement.gov/networkconfig/>
3. Is there some specific trust bit manipulation that needs to occur for the GPO?
- Specific instructions to follow.
4. Is only Windows 10 affected or is Server 2016 and other legacy client/server operating systems affected?
- All versions of Windows is affected. 
5. Can this affect IPSec certificates that have the Server Authentication bit enabled and that are used with Microsoft Operating Systems?
- Yes, this impacts any certificate asserting Server Authentication.

#### Microsoft Certificate Trust Lists (CTL) Recommended Reading
1. [Microsoft Trusted Root Government CA Requirements](https://social.technet.microsoft.com/wiki/contents/articles/31635.microsoft-trusted-root-certificate-program-audit-requirements.aspx#Government_CA_Requirements)
2. [CTL Overview](https://msdn.microsoft.com/en-us/library/windows/desktop/aa376545(v=vs.85).aspx)
2. [How to configure CTL](https://technet.microsoft.com/en-us/library/dn265983.aspx)
