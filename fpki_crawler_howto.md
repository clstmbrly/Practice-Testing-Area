---
layout: default 
title: How To Use the FPKI Crawler
permalink: /fpkicrawler/
---

Your agency or organizations may need to understand the complex relationships between the Certification Authorities (CAs) within the Federal Public Key Infrastructure (FPKI). To help you navigate through this information, the FPKI Crawler offers some useful tools to:

* Visualize these relationships
* Understand how CAs validate to the Federal Common Policy CA (COMMON)
* Access CA certificate data for reporting and analysis
* Download certificate files

### Table of Contents

* [FPKI Graph](#fpki-graph)
* [FPKI Crawler Outputs](#fpki-crawler-outputs)
  * [Public Certificates for Reporting and Analysis](#public-certificates-for-reporting-and-analysis)
  * [Public Certificates for Download](#public-certificates-for-download)

## FPKI Graph

The FPKI Crawler provides an [FPKI Graph](https://fpki-graph.fpki-lab.gov/){:target="_blank"}_ visualization tool to help you see how all the CAs and Bridges interconnect within the FPKI ecosystem and how each CA validates to the Root, COMMON.

* Click on any dot in the FPKI Graph to see that CA's inbound and outbound relationships. 

## FPKI Crawler Output Files

The [FPKI Crawler](https://fpki-graph.fpki-lab.gov/crawler/){:target="_blank"}_ (_AIA Crawler Results_ webpage) offers output files in CSV, HTML, XML, P7B, etc., to help you understand and administer your certificate Key Store and build a Trust Store. 

{% include alert-info.html heading="The FPKI Crawler uses AIAs and SIAs to find all CA certificates." content="Each CA public certificate should contain Authority Information Access (AIA) and Subject Information Access (SIA) extensions. An AIA chain will lead to the COMMON-certified certificate for download. The SIA gives a URL where you can see all CA-issued certificates." %} 

> **Note:**&nbsp;&nbsp;The Crawler categorizes CA certificates by _Type_ (_U.S. Government_, _State_, or _Company_). _Agency_, _State Name_, and _Company Name_ are extracted from the Distinguished Name (DN).

### Public Certificates for Reporting and Analysis

### 1. Federal Common Policy Tree File (_FederalCommonPolicyTree.csv_)

The _FederalCommonPolicyTree.csv_ (Microsoft Excel) gives a data view of all CAs that validate to COMMON and the cross-certified CAs. 

### 2. All Certificates (_AIACrawler.html_)

The _AIACrawler.html_ file gives all FPKI CA certificates found by the Crawler (in four Sections):

* **Certificates Found with Validated AIA Chains to COMMON &mdash;** All certificates with validated paths to COMMON and the certificate policies to which they validate.<!--Does this mean: "the certificates policies to which they validate" if NOT COMMON?--> 

* **Certificates Found with Validated Chains to COMMON, NOT Found through AIA &mdash;** All certificates with validated paths to COMMON and the certificate policies to which they validate. (These certificates are found via searching with Java Development Kit [JDK] Public Key Infrastructure for X.509 Certificates [PKIX] when no validating chain is found via AIA.)   

* **Certificates Found with NO Validated Chains to COMMON &mdash;** All certificates with NO validated path to COMMON. (These certificates are found through using AIA and SIA extensions.) This file lists only the certificate information. (These tend to be cross-certificates issued to FPKI CAs<!--CAs?--> that allow a partner PKI to use its own Root CA as the trust anchor instead of COMMON.)

* **All Certificates &mdash;** All CA certificates in the FPKI.

> For each CA certificate listed in the _AIACrawler.html_ file, you will see the _Cert_ and _Issuer_ data and status.  For example:

   ```
  Issuer CN=Federal Bridge CA 2016,OU=FPKI,O=U.S. Government,C=US serial# 0x03F42   status GOOD
   ```
> For detailed data about certificates or issuers, click on any link in the _AIACrawler.html_ file.

### 3. Certificates with AIA Information (_allcertsfoundaturi.xml_)

The _allcertsfoundaturi.xml_ file lists all AIA URLs and extraction errors in XML format.

### 4. Certificates with AIA Information (_allcertsfoundaturi.csv_)

The _allcertsfoundaturi.csv_ (Microsoft Excel) lists all AIA URLs and the CA certificates found, or certificate-retreival errors. 

* The key columns include&nbsp;&nbsp;Error (if any), Certificate DN, Issuer (DN), Serial Number, Not Before, Sig Alg (Signing Algorithm), Subject Key, and Authority Key.

### 5. All Certificates File (_allcerts.csv_)

The _allcerts.csv_ (Microsoft Excel) file lists all CA certificates found by the Crawler. You can use this file to analyze certificates. 

* The key columns include:&nbsp;&nbsp;Subject DN, Issuer DN, (Certificate) Group, Serial (Number), Sig Alg (Signing Algorithm) (typically SHA1 or SHA-256), Subject Key (hexadecimal number), and Authority Key.

* If found in a certificate, the Online Certificate Status Protocol (OCSP) URL will be in the OCSP HTTP column. The Certificate Revocation List Distribution Point (CRLDP) URLs will be in the CDRLDP HTTP column; the CDRLDP LDAP column; and the CRLDP, AIA and SIA ERRORS column.

* The AIA and SIA URLs for Object Identifiers (OIDs) (i.e., id-ad-caIssuers, id-ad-caRepository, and id-ad-timeStamping) will be in these columns:&nbsp;&nbsp;AIA id-ad-caIssuers HTTP; AIA id-ad-caIssuers LDAP; AIA id-ad-caRepository LDAP; SIA id-ad-caIssuers HTTP; SIA id-ad-caRepository HTTP; SIA id-ad-caRepository LDAP; SIA id-ad-caRepository LDAP; SIA id-ad-timeStamping HTTP; SIA id-ad-timeStamping LDAP; and CRLDP, AIA and SIA ERRORS. 

### Public Certificates for Download

The FPKI Crawler provides all retrieved CA public certificates for download and analysis.

{% include alert-warning.html heading = "Do Not Import These Certificates into a Trust Store before Analysis!" content="The certificates from these files are made available for analysis purposes only. These certificates should not be imported into a Trust Store prior to analysis to determine applicable trust relationships." %}

### 1. All CA Certificates in One File (_CACertificatesValidatingToCommonPolicy.p7b_)

The _CACertificatesValidatingToCommonPolicy.p7b_ contains all certificates retrieved by the FPKI Crawler. This file allows you to easily sort certificates by expiration date, issuer, or subject. 

### 2. All CA Certificates Broken Down into Eight Files (_CACertificatesValidatingToCommonPolicy_1.p7b_&mdash;_8.p7b_)

The certificates found in the files ‘CACertificatesValidatingToCommonPolicy_1.p7b’ through ‘CACertificatesValidatingToCommonPolicy_8.p7b’ contain all of the CA certificates found by the Crawler, broken into eight files to simplify analysis.<!--Will user be analyzing certificate download files?-->

### 3. Certificate Files by Groups <!--Define "Groups"-->

The CA certificates are partitioned into types <!--As in "Type" described FPKI Crawler Output Files section? What is this referencing?-->and categories<!--Do you mean the "All Certificates (_AIACrawler.html_)" section?-->, as defined in the Certificate Grouping sections<!--What section is this referencing?-->. For each of these groups<!--Types and Categories?-->, all CA certificates are organized into a single **PKCS#7** file. In addition, another **PKCS#7** file is generated containing all of the CA certificates plus all additional certificates required for path validation to COMMON. For example:

> The U.S. Department of Veterans Affairs from the <!--"issued by"? "from the" doesn't sound right-->Verizon Shared Service Provider (SSP) CA has two CA certificates that can be found in **_US_Government__VA.p7b_**. In order to validate the CAs' paths to COMMON, a Betrusted cross-certificate is required. Therefore, the file **_US_Government__VA_FullPath.p7b_** contains both CA certificates and the cross-certificate.
