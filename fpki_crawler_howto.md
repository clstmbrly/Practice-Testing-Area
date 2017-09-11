---
layout: default 
title: How To Use the FPKI Crawler
permalink: /fpkicrawler/
---

The FPKI Crawler is an interactive website that will help you to:

* See the relationships between the hundreds of Certification Authorities (CAs) in the Federal Public Key Infrastructure (FPKI)
* Understand how each CA’s certificate path validates to the Root, Federal Common Policy CA (COMMON)
* Obtain public CA certificate information for reporting and analysis
* Download public CA certificates

### Table of Contents
<!--This intro didn't fully encapsulate all that the FPKI Crawler does, so I added the rest--hopefully correct. -->  
<!--This is a TOC for reader navigation--LaChelle prefers.-->
* [FPKI Graph](#fpki-graph)
* [FPKI Crawler Outputs](#fpki-crawler-outputs)
  * [Public Certificates for Reporting and Analysis](#public-certificates-for-reporting-and-analysis)
  * [Public Certificates for Download](#public-certificates-for-download)

## FPKI Graph

The [FPKI Graph](https://fpki-graph.fpki-lab.gov/){:target="_blank"}_ is useful visualization tool that shows you the relationships between all of the CAs and Bridges in the FPKI ecosystem. The Graph also shows you how each CA certificate is linked to the Root, COMMON.

* Click on any dot in the FPKI Graph to see a CA's inbound and outbound relationships. 

## FPKI Crawler Output Files

The [FPKI Crawler](https://fpki-graph.fpki-lab.gov/crawler/){:target="_blank"}_ (_AIA Crawler Results_ webpage) offers output files to help you understand and administer your certificate Key Store and build a Trust Store: 

* CSV, HTML, and XML-formatted files give path and certificate validation data 
* P7B files give public CA certicates for download

{% include alert-info.html heading="The FPKI Crawler uses AIAs and SIAs to find all CA certificates." content="Each CA public certificate should contain Authority Information Access (AIA) and a Subject Information Access (SIA) extensions. An AIA chain will lead to the COMMON-certified certificate for download. The SIA will give the URL to a list <!--A list?-->of all CA certificates that the CA has issued." %} 

### Public Certificates for Reporting and Analysis

All Crawler output files categorize CA certificates by _Type_ (_U.S. Government_, _State_, or _Company_). _Agency_, _State Name_, and _Company Name_ are extracted from the Distinguished Name (DN).

### 1. Federal Common Policy Tree File (_FederalCommonPolicyTree.csv_)

The _FederalCommonPolicyTree.csv_ (Microsoft Excel) gives a data view of all CAs that validate to COMMON and those that cross-certify with them. <!--Below in allcerts.csv we say "raw data in a spreadsheet." For consistency which is better?-->

### 2. All Certificates (_AIACrawler.html_)<!--Unclear why is this section called "All Certificates"...?-->

The all-inclusive file in 4 sections, _AIACrawler.html_, lists all CA certificates found, according to the search methods described below:

* **Certificates Found with Validated AIA Chains to COMMON Policy &mdash;** All certificates with validated paths to COMMON and the certificate policies to which they validate. 

* **Certificates Found with Validated Chains to COMMON Policy, Not Found through AIA &mdash;** All certificates with validated paths to COMMON and the certificate policies to which they validate. (These certificates are found through using Java Development Kit [JDK] Public Key Infrastructure for X.509 Certificates [PKIX].)  <!--Why is the JDX PKIX method used in addition to AIA and SIA extensions? Does it find certificates that wouldn't otherwise be found for some reason?--> 

* **Certificates Found with NO Validated Chains to COMMON Policy &mdash;** All certificates with **NO** validated path to COMMON. (These certificates are found through using AIA and SIA extensions.) This file lists only the certificate information. (These tend to be cross-certificates issued to FPKI CAs<!--CAs?--> that allow a partner PKI to use its own Root CA as the trust anchor instead of COMMON.)

* **All Certificates &mdash;** All certificates, whether or not they have a validated path to COMMON. <!--How are these found--AIA and SIA extensions or other method?-->

> Click on any certificate hyperlink in the file to see its detailed information and validation status. For example:

   ```
  Issuer CN=Federal Bridge CA 2016,OU=FPKI,O=U.S. Government,C=US serial# 0x03F42   status GOOD
   ```

**XML file should be here acc. to AIA Crawler Results webpage order**<!--XML format output file should be here, based on order on FPKI Crawler (AIA Crawler Results) webpage shows this precedes CSV file.-->

### 3. All Certificates File (_allcerts.csv_)

The _allcerts.csv_ (Microsoft Excel) file lists all CA certificates found by the Crawler. You can use this file to analyze certificates. The columns for each certificate line item are:<!--Is is necessary to list the column headers?-->

(**Note: OIDs** are id-ad-caIssuers, id-ad-caRepository, and id-ad-timeStamping.)
<!--These column headers are out of order and some were missing that I added-->

* Name 
* Group
* Groups
* Subject Key <!--I see "Subject Key" but no "Subject DN"-->
* Issuer DN
* Certificate Group <!--Same as header that says "Group"?-->
* Subject DN
* Serial <!--Column header just says "Serial"-->
* Issuer DN
* Status
* Status Issue
* Signing Algorithm (e.g., SHA1 or SHA-256)
* Not After
* Online Certificate Status Protocol (OCSP) HTTP and LDAP (URLs, if found) 
* Subject Key (hexadecimal number)
* Authority Key
* Certificate Revocation List Distribution Point (CRLDP) URLs will be listed in one column for HTTP, one for LDAP CRLDP, and one Unknown/Error column.
* AIA and SIA URLs are listed in separate columns for each OID and HTTP, LDAP or Unknown/Error. <!--Don't understand. For SIA Column headings = SIA has 3 HTTP column (headers), 2 LDAP column (headers), and 1 CRLDP/AIA and SIA Errors (header) column. 
For AIA, there are 2 AIA Len column headers, 1 AIA http, 1 AIA ldap, 1 AIA repository. ... For CRLDP there are 2 columns headers (one HTTP and one LDAP).  Will there be 4 columns for OID, HTTP, LDAP, and Unknown/Error? Are 2 columns for AIA and SIA URLs? Unclear.-->

### 4. Certificates with AIA Information (_allcertsfoundaturi.csv_)

The _allcertsfoundaturi.csv_ (Microsoft Excel) lists each AIA URL and either the error retrieving certificates or the list of CA certificates found at that AIA. It includes the following certificate information: Subject DN, Issuer DN, Serial Number, Signing Algorithm, Not Before, Subject Key and Authority Key as hexadecimal numbers.

### 5. Certificates with AIA Information (_allcertsfoundaturi.xml_)   **This comes before the _allcerts.csv_ file on webpage**
<!--This file shows that it "doesn't have any style associated with it" so it's hard to figure out what's what. How will the user read it?-->

The file _allcertsfoundaturi.xml_ lists each AIA URL in XML format. For each URL, any error extracting the certificates will be listed; otherwise, all certificates found at that AIA are listed.

### Public Certificates for Download

The FPKI Crawler provides the public certificate information as binary data for download and analysis by any FPKI validating agency or organization. The data involves all certificates retrieved by the FPKI Crawler.

<!--This will be an alert warning box on the IDM.gov webpage.-->
{% include alert-warning.html heading = "Do Not Import These Certificates into a Trust Store before Analysis!" content="The certificates from these files are made available for analysis purposes only. These certificates should not be imported into a Trust Store prior to analysis to determine applicable trust relationships." %}

### 1. All CA Certificates in One File (_CACertificatesValidatingToCommonPolicy.p7b_)

The _CACertificatesValidatingToCommonPolicy.p7b_ contains all certificates retrieved by the FPKI Crawler. This file allows you to easily sort the certificates by expiration date, issuer or subject. 

### 2. All CA Certificates Broken Down into Eight Files (_CACertificatesValidatingToCommonPolicy_1.p7b_ &mdash; _8.p7b_)

The certificates found in the files ‘CACertificatesValidatingToCommonPolicy_1.p7b’ through ‘CACertificatesValidatingToCommonPolicy_8.p7b’ contain all of the CA certificates found by the Crawler, broken evenly into eight files to simplify analysis.

### 3. Certificate Files by Groups <!--Define "Groups"-->

The CA certificates are partitioned into types <!--As in "Type" described FPKI Crawler Output Files section? What is this referencing?-->and categories<!--Do you mean the "All Certificates (_AIACrawler.html_)" section?-->, as defined in the Certificate Grouping sections<!--What section is this referencing?-->. For each of these groups<!--Types and Categories?-->, all CA certificates are organized into a single **PKCS#7** file. In addition, another **PKCS#7** file is generated containing all of the CA certificates plus all additional certificates required for path validation to COMMON. For example:

> The U.S. Department of Veterans Affairs from the <!--"issued by"? "from the" doesn't sound right-->Verizon Shared Service Provider (SSP) CA has two CA certificates that can be found in **_US_Government__VA.p7b_**. In order to validate the CAs' paths to COMMON, a Betrusted cross-certificate is required. Therefore, the file **_US_Government__VA_FullPath.p7b_** contains both CA certificates and the cross-certificate.
