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

The [FPKI Crawler](https://fpki-graph.fpki-lab.gov/crawler/){:target="_blank"}_ (_AIA Crawler Results_ webpage) produces output files that can help you understand and administer your certificate Key Store and build a Trust Store. These files give detailed CA certificate information, including path and certificate policy validation in list and spreadsheet formats. that you can view in Microsoft Excel or OpenOffice. The P7B files provide public CA certicates that you can download.

{% include alert-info.html heading="The FPKI Crawler uses AIAs and SIAs to find all CA certificates." content="Each CA public certificate should contain Authority Information Access (AIA) and a Subject Information Access (SIA) extensions. The CA certificate’s AIA chain will lead to the COMMON-certified certificate for download. The SIA extension will give a URL for all CA certificates that the CA has issued." %} 

{% include alert-info.html heading="CAs are categorized by Type and Agency or Company" content="

* The **Type** is either U.S. Government, State, or Commercial.
* For U.S. Government, the Agency is extracted from the Distinguished Name (DN). 
* For State Governments, the State Name is extracted. 
* For commercial CAs, the Company Name is extracted." %}

### Public Certificates for Reporting and Analysis

### 1. Federal Common Policy Tree File (_FederalCommonPolicyTree.csv_)

The _FederalCommonPolicyTree.csv_ (Microsoft Excel) provides a data view of all CAs that validate to COMMON and those that cross-certify with them.

### 2. All Certificates (_AIACrawler.html_)

The [_AIACrawler.html_](https://fpki-graph.fpki-lab.gov/crawler/AIACrawler.html){:target=_"blank"]_ This all-inclusive file in 4 parts lists all CA certificates that both validate to COMMON and those that do not. These have been found through searching for AIA and SIA extensions. The hyperlink for each certificate will take you to its detailed information and current validation status. For example:

   ```
Issuer CN=Federal Bridge CA 2016,OU=FPKI,O=U.S. Government,C=US serial# 0x03F42   status GOOD
   ```
   
The 4 parts to this file are:

* **Certificates Found with Validated AIA Chains to COMMON Policy** &mdash; Gives all certificates with validated paths to COMMON and the certificate policies to which they validate. 

* **Certificates Found with Validated Chains to COMMON Policy, Not Found through AIA** &mdash; Gives all certificates with validated paths to COMMON found by using Java Development Kit (JDK) Public Key Infrastructure for X.509 Certificates (PKIX) and the certificate policies to which they validate.  <!--Is the JDX PKIX method superior (since it is mentioned)? Does it find certificate information that the other method(s) don't? What is the methods for finding parts 1 and 3 certificates?--> 

* **Certificates Found with NO Validated Chains to COMMON Policy** &mdash; Gives all certificates that have **NO** validated path to COMMON (via AIA and SIA extensions). Only the certificate information is listed. These tend to be cross-certificates issued to the FPKI that allow a partner PKI to use of its own root CA as the trust anchor instead of COMMON.<!--Explain how cross-certificates can be issued to the whole FPKI itself. Meaning is cross-certificates between FPKI CAs and partner PKI CAs?--> These tend to be cross-certified FPKI and partner PKI certificates. In these cased, the partner PKI is allowed to use its own root CA as its trust anchor instead of COMMON. <!--Why is "via AIA and SIA extensions" mentioned for this list but not for the other 2? Aren't AIAs and SIAs searched for all 3 lists?

* **All Certificates** &mdash; Gives all certificates regardless of whether or not they have validated paths to COMMON.

<!--XML format output file should come here - order on webpage shows this precedes CSV file.-->

### 3. All Certificates File (CSV) (_allcerts.csv_)

For each CA certificate the Crawler finds, the _allcerts.csv_ output file contains a line item. You can use this file to analyze the certificate details (i.e., raw data in a spreadsheet). 

* Columns include Subject DN, Issuer DN, Certificate Group, Serial Number, Signing Algorithm (e.g., SHA1 or SHA-256), and Online Certificate Status Protocol (OCSP) URLs found in the certificate (if any found). 

* The Subject Key as a hexadecimal number, Authority Key.<!--I don't see this in the file. Where does it appear?-->

* Certificate Revocation List Distribution Point (CRLDP) URLs will be listed in one column for HTTP, one for LDAP CRLDP, and one Unknown/Error column.

* AIA and SIA URLs are listed in separate columns for each OID and HTTP, LDAP or Unknown/Error. 

* OIDs include id-ad-caIssuers, id-ad-caRepository, and id-ad-timeStamping.

### 4. Certificates with AIA Information (CSV) (_allcertsfoundaturi.csv_)

The file _allcertsfoundaturi.csv_ (Microsoft Excel) lists each AIA URL and either the error retrieving certificates or the list of CA certificates found at that AIA. It includes the following certificate information: Subject DN, Issuer DN, Serial Number, Signing Algorithm, Not Before, Subject Key and Authority Key as hexadecimal numbers.

### 5. Certificates with AIA Information (XML) (_allcertsfoundaturi.xml_)   **This comes before the CSV format file on webpage**

The file _allcertsfoundaturi.xml_ lists each AIA URL in XML format. For each AIA URL, any error extracting the certificates will be listed; otherwise, all certificates found at that AIA are listed.

### Public Certificates for Download

The FPKI Crawler provides the public certificate information as binary data for download and analysis by any FPKI validating agency or organization. The data involves all certificates retrieved by the FPKI Crawler.

<!--This will be an alert warning box on the IDM.gov webpage.-->
{% include alert-warning.html heading = "Do Not Import These Certificates into a Trust Store before Analysis!" content="The certificates from these files are made available for analysis purposes only. These certificates should not be imported into a Trust Store prior to analysis to determine applicable trust relationships." %}

### 1. All CA Certificates in One File (_CACertificatesValidatingToCommonPolicy.p7b_)

The _CACertificatesValidatingToCommonPolicy.p7b_ contains all certificates retrieved by the FPKI Crawler. This file allows you to easily sort the certificates by expiration date, issuer or subject. 

### 2. All CA Certificates Broken Down into Eight Files (_CACertificatesValidatingToCommonPolicy_1.p7b_ &mdash; _8.p7b_)

The certificates found in the files ‘CACertificatesValidatingToCommonPolicy_1.p7b’ through ‘CACertificatesValidatingToCommonPolicy_8.p7b’ contain all of the CA certificates found by the Crawler, broken evenly into eight files to simplify analysis.

### 3. Certificate Files by Groups

The CA certificates are partitioned into types and categories as defined in the Certificate Grouping section. For each of these groups, all CA certificates are organized into a single PKCS#7 file. In addition, another PKCS#7 file is generated containing all of the CA certificates plus all additional certificates required for path validation to COMMON.

> For example: the U.S. Department of Veterans Affairs from the Verizon Shared Service Provider Certification Authority has two CAs that can be found in ‘US_Government__VA.p7b’. In order to validate the CAs' paths to COMMON, a Betrusted cross-certificate is required. Therefore, the file ‘US_Government__VA_FullPath.p7b’ contains both the CAs and the cross-certificate.
