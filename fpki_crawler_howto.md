---
layout: default 
title: How To Use the FPKI Crawler
permalink: /fpkicrawler/
---

The FPKI Crawler is an interactive website that will help you to:

* Visualize the relationships between the hundreds of Certification Authorities (CAs) in the Federal Public Key Infrastructure (FPKI)
* Understand how each CA’s certificate path validates to the Root, Federal Common Policy CA (COMMON)<!--I don't see that the Graph shows you how each CA validates to COMMON.--> 
* Obtain output files with public certificate information for reporting and analysis
* Download public certificates

### Table of Contents
<!--This intro didn't fully encapsulate all that the FPKI Crawler does, so I added the rest--hopefully correct. -->  
<!--This is a TOC for reader navigation--LaChelle prefers.-->
* [FPKI Graph](#fpki-graph)
* [FPKI Crawler Outputs](#fpki-crawler-outputs)
* [Public Certificates for Reporting and Analysis](#public-certificates-for-reporting-and-analysis)
* [Public Certificates for Download](#public-certificates-for-download)

## FPKI Graph

The [FPKI Graph](https://fpki-graph.fpki-lab.gov/){:target="_blank"}_ is useful for visualizing the relationships between all of the CAs and Bridges in the FPKI ecosystem. The Graph also shows you how each CA certificate is linked to the Root, COMMON.<!--I don't see the COMMON linkage in the Graph.--> 

* Click on any dot in the Graph to see a CA's inbound and outbound relationships. 

## FPKI Crawler Outputs:&nbsp;&nbsp;Public Certificates for Reporting and Analysis

The [FPKI Crawler](https://fpki-graph.fpki-lab.gov/crawler/){:target="_blank"}_ (i.e., _AIA Crawler Results_ webpage) generates output files that will help you to understand and administer your certificate Key Store and build a Trust Store.

* These files give detailed information about certificates for reporting and analysis, including:

  * Certificate Policy validation
  * Organized certificate lists in comma-separated-value (CSV) spreadsheets (viewable in Microsoft Excel or OpenOffice)<!--The files are in different formats--csv, html, xml, etc.-->.

* The P7B files give you certicates for download.

The Crawler uses AIAs and SIAs to find all CA certificates. Most CAs that validate to COMMON include an Authority Information Access (AIA) extension in their public certificates that gives a URL to the signing CA's certificate. Following a CA certificate’s AIA chain will lead you to a COMMON-certified certificate for download.<!--Isn't that what the P7B files are for?--> Each CA should also have a Subject Information Access (SIA) extension in its public certificate that gives a URL for all CA certificates that the CA has issued.

{% include alert-info.html heading="CAs are categorized by Type and Agency or Company" content="

* The **Type** is either U.S. Government, State, or Commercial.
* For U.S. Government, the Agency is extracted from the Distinguished Name (DN). 
* For State Governments, the State Name is extracted. 
* For commercial CAs, the Company Name is extracted." %}

### Federal Common Policy Tree File (CSV) (_FederalCommonPolicyTree.csv_)

The _FederalCommonPolicyTree.csv_ provides a relational, data view of <!--All?-->the CAs that validate to COMMON, as well as the CAs that cross-certify with them. <!--If All, then this is also an All Certificates output file and summary 3 bullets below belong above this section.-->

### All Certificates (HTML) (AIACrawler.html)

<!--Real Description: "Crawler Output as HTML
Lists Certificate Paths to Common Policy and **Validating Policies**"-->

The AIA Crawler Results webpage separates all CA certificates that validate to COMMON into three categories.The data found at https://fpki-graph.fpki-lab.gov/crawler/AIACrawler.html provides lists of all certificates that have a valid certificate chain with COMMON.  <!--Third one below is those that do NOT validate to COMMON.--> (**Note:** The file descriptions are summaried here for brevity.)

* **Certificates Found with Validated AIA Chains to COMMON** &mdash; <!--What is the file format? Other Files give the format.>This list gives all CA certificates with validated paths to COMMON. <!--Give URL to this File. The File title implies that COMMON is the policy.-->

* **Certificates Found with Validated Chains to COMMON** &mdash; This list gives all CA certificates with validated paths by using Java Development Kit (JDK) PKIX, and the certificate policies for which they validate are listed.  <!--Give URL to this File. The File title implies that COMMON is the policy.> 

* **Certificates Found with No Validated Chains to COMMON** &mdash; This list gives all certificates that are found without a valid path to COMMON (via AIA and SIA extensions). Only the certificate information is listed. These tend to be cross-certificates issued to the FPKI that allow a partner PKI to use of its own root CA as the trust anchor instead of COMMON.<!--Explain how cross-certificates can be issued to the whole FPKI itself. Meaning is cross-certificates between FPKI CAs and partner PKI CAs?--> These tend to be cross-certified FPKI and partner PKI certificates. In these cased, the partner PKI is allowed to use its own root CA as its trust anchor instead of COMMON.

Each CA certificate is listed as a hyperlink to its detailed information. <!--Aren't those above in other lists also hyperlinked? Not stated in those cases.-->This list also provides each certificate's current validation status. For example:

   ```
Issuer CN=Federal Bridge CA 2016,OU=FPKI,O=U.S. Government,C=US serial# 0x03F42   status GOOD
   ```

<!--XML format output file should come here - order on webpage shows this precedes CSV file.-->

### All Certificates File (CSV) (_allcerts.csv_)

For each CA certificate the Crawler finds, the _allcerts.csv_ output file contains a line item. You can use this file for analyzing the list of certificates. The certificate details are presented as raw data in a spreadsheet format. 

* Columns include Subject DN, Issuer DN, Certificate Group, Serial Number, Signing Algorithm (e.g., SHA1 or SHA-256). The Subject Key as a hexadecimal number, Authority Key.

* Online Certificate Status Protocol (OCSP) URLs found in the certificate<!--Will also be in an AIA or SIA extension?-->, if any, is listed in a separate column. 

* Certificate Revocation List Distribution Point (CRLDP) URLs will be listed in one column for HTTP, one for LDAP CRLDP, and one Unknown/Error column.

* AIA and SIA URLs are listed in separate columns for each OID and HTTP, LDAP or Unknown/Error. 

* OIDs include id-ad-caIssuers, id-ad-caRepository, and id-ad-timeStamping.

### Certificates with AIA Information (CSV) (_allcertsfoundaturi.csv_)

The file _allcertsfoundaturi.csv_ lists each AIA URL and either the error retrieving certificates or the list of CA certificates found at that AIA. It includes the following certificate information: Subject DN, Issuer DN, Serial Number, Signing Algorithm, Not Before, Subject Key and Authority Key as hexadecimal numbers.

### Certificates with AIA Information (XML) (_allcertsfoundaturi.xml_)   **This comes before the CSV format file on webpage**

The file _allcertsfoundaturi.xml_ lists each AIA URL in XML format. For each AIA URL, any error extracting the certificates will be listed; otherwise, all certificates found at that AIA are listed.

## Public Certificates for Download

The FPKI Crawler provides the public certificate information as binary data for download and analysis by any FPKI validating agency or organization. The data involves all certificates retrieved by the FPKI Crawler.

<!--This will be an alert warning box on the IDM.gov webpage.-->
{% include alert-warning.html heading = "Do Not Import These Certificates into a Trust Store before Analysis!" content="This certificate data is made available for analysis purposes. The certificates from these files should not be imported into a Trust Store prior to analysis to determine applicable trust relationships." %}

**1. All CA Certificates in One File (_CACertificatesValidatingToCommonPolicy.p7b_)**

The _CACertificatesValidatingToCommonPolicy.p7b_ contains all certificates retrieved by the FPKI Crawler. This file allows you to easily sort the certificates by expiration date, issuer or subject. 

**2. All CA Certificates Broken Down into Eight Files**

The certificates found in the files ‘CACertificatesValidatingToCommonPolicy_1.p7b’ through ‘CACertificatesValidatingToCommonPolicy_8.p7b’ contain all of the CA certificates found by the Crawler, broken evenly into eight files to simplify analysis.

3. Certificate Files by Groups

The CA certificates are partitioned into types and categories as defined in the Certificate Grouping section. For each of these groups, all CA certificates are organized into a single PKCS#7 file. In addition, another PKCS#7 file is generated containing all of the CA certificates plus all additional certificates required for path validation to COMMON.

As an example, the Federal Government’s Veterans Affairs Agency from the Verizon Shared Service Provider Certification Authority has two CAs that can be found in ‘US_Government__VA.p7b’. In order to path validate these CAs to COMMON, a Betrusted cross certificate is required. Therefore, the file ‘US_Government__VA_FullPath.p7b’ contains both the CAs plus the cross certificate.
