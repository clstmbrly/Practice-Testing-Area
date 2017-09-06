---
layout: default 
title: How To Use the FPKI Crawler
permalink: /fpkicrawler/
---

<!--The title in the nav block above will be the webpage title (level 1 heading) when rendered and published on website. Playbooks don't have an Introduction title, just a lead paragraph.-->
<!--Intro line from the FPKI-Guides' Useful Tools webpage about FPKI Crawler--good intro line.-->The [Federal Public Key Infrastructure (FPKI) Crawler](https://fpki-graph.fpki-lab.gov/crawler/){:target="_blank"}_ is an interactive website that allows you to see all the relationships between the hundreds of FPKI Certification Authorities (CAs). It also validates each CA’s certificate path to the Federal Common Policy CA (COMMON), provides CA certificate data views, and output files for reporting and analysis.<!--It also displays CA certificates that do not validate in the All Certificates Webpage. We need to say that also.-->  
<!--TOC for reader navigation--LaChelle prefers.-->

* [FPKI Graph](#fpki-graph)
* [FPKI Crawler Outputs](#fpki-crawler-outputs)
* [Public Certificates for Reporting and Analysis](#public-certificates-for-reporting-and-analysis)
* [Public Certificates for Download](#public-certificates-for-download)

## FPKI Graph

The Crawler provides an interactive FPKI Graph to help you visualize the following:
<!--Sounded like essentially the same concepts were repeated.-->
* The FPKI ecosystem.
* The interconnections among the CAs (inbound and outbound) and Bridges in the FPKI. <!--Relationships between CAs or certificates?-->
* Each CA's certificate validation path to the root, COMMON. <!--The report below shows those that do not validate to COMMON. How do we also refer to that here to provide a complete picture of what the Crawler offers?-->

Most CAs that validate to COMMON <!--"Should" implies that there a requirement for AIA extensions and that some CA are noncompliant.-->include an Authority Information Access (AIA) extension in their public certificates that gives a URL <!--One URL?-->to the signing CA's certificate<!--The Signing CA = COMMON?  Is this what you meant? A bit of a run-on, convoluted sentence.-->. Following each CA certificate’s AIA chain <!--"Chain" meaning--that multiple URLs provided in multiple AIAs may be required for you to get to the certificate you need?-->will help you to find the COMMON-certified certificate.<!--Is the user trying to find the certificate for Root, COMMON, or a CA's own certificate that COMMON signed?--> Each CA should also have a Subject Information Access (SIA) extension in its public certificate that gives a URL for all CA certificates that it has issued<!--Issued by what CA?-->. The Crawler uses these AIAs and SIAs to find all CA certificates.<!--It won't find all CAs if they don't have AIA and SIA extensions.  What happens then?-->

## FPKI Crawler Outputs

The FPKI Crawler generates output files that will help you to:

* Understand and administer your certificate Key Store 
* Build a Trust Store

These output files give information about certificates, including path validation, certificate policy validation, organized certificate lists, and comma-separated-value (CSV) spreadsheets (for review in Microsoft Excel or OpenOffice). These files are available at [FPKI Crawler](https://fpki-graph.fpki-lab.gov/crawler/){:target="_blank"}_.

These files categorize the CAs by Type and Agency or Company using information from the certificate's Distinguished Name (DN). The type can be U.S. Government, State, or Commercial:<!--No Tribal, Territorial, or International? These are also included in FPKI-Guides' Certification Authorities webpage.--> 

* For U.S. Government, the Agency is extracted from the DN. 
* For State Governments, the state name is extracted. 
* For commercial CAs, the company name is extracted.

## Public Certificates for Reporting and Analysis

### 1. Federal Common Policy Tree File<!--A significant number of prepositional phrases has a weakening effect on meaning.-->

In addition to the graphical view provided by the FPKI Crawler Graph, the _FederalCommonPolicyTree.csv_ file provides a data view <!--Describe what a "data view" is. Gives no insight into what info the files gives.-->of the relationships among the CAs that validate to COMMON and identifies those CAs that cross-certify with each each other. <!--Last sentence (deleted) covered most of the same ground already stated.-->

### 2. All Certificates Webpage (HTML)

For all certificates that validate to COMMON, the All Certificates Webpage at [AIACrawler.html](https://fpki-graph.fpki-lab.gov/crawler/AIACrawler.html){:target="_blank"}_ breaks down all certificates into three categories:

#### a. Certificates Found with Validated AIA Chains to COMMON

All valid paths are printed, and for each path, the FPKI certificate policies for which they validate are listed.

#### b. Certificates Found with Validated Chains to COMMON (not via AIA)

A valid path found using [Java Development Kit (JDK) PKIX] is listed, and the certificate policies for which they validate are listed.

#### c. Certificates Found with No Validated Chains to COMMON

The certificates that are found through AIA and SIA extensions (URLs) <!--I searched internet and couldn't find any usage of "chasing" in regard to certificate paths.-->that do not have a path that validates to COMMON are listed in this group. Only the certificate information is listed.<!--What is the missing information?--> These certificates tend to be cross-certificates issued to the FPKI that allow a partner PKI to make use of its own Root CA as the trust anchor instead of COMMON.

Each CA certificate is listed as a hyperlink to its detailed information. The data also provides the current validation status of each certificate. For example:

   ```
Issuer CN=Federal Bridge CA 2016,OU=FPKI,O=U.S. Government,C=US serial# 0x03F42   status GOOD
   ```

### 3. All Certificates File (CSV)

For each CA certificate the Crawler finds, the _allcerts.csv_ output file contains a line item. You can use this file for analyzing the list of certificates. The certificate details are presented as raw data in a spreadsheet format. 

* Columns include Subject DN, Issuer DN, Certificate Group, Serial Number, Signing Algorithm (e.g., SHA1 or SHA-256). The Subject Key as a hexadecimal number, Authority Key.

* Online Certificate Status Protocol (OCSP) URLs found in the certificate<!--Will also be in an AIA or SIA extension?-->, if any, is listed in a separate column. 

* Certificate Revocation List Distribution Point (CRLDP) URLs will be listed in one column for HTTP, one for LDAP CRLDP, and one Unknown/Error column.

* AIA and SIA URLs are listed in separate columns for each OID and HTTP, LDAP or Unknown/Error. 

* OIDs include id-ad-caIssuers, id-ad-caRepository, and id-ad-timeStamping.

### 4. Certificates with AIA Information in CSV Format

The file ‘allcertsfoundaturi.csv’ lists each AIA URL and either the error retrieving certificates or the list of CA certificates found at that AIA. It includes the following certificate information: Subject DN, Issuer DN, Serial Number, Signing Algorithm, Not Before, Subject Key and Authority Key as hexadecimal numbers.

### 5. Certificates with AIA Information in XML Format

The file ‘allcertsfoundaturi.xml’ lists each AIA URL in XML format. For each AIA URL, any error extracting the certificates will be listed; otherwise, all certificates found at that AIA are listed.

## Public Certificates for Download

The Crawler provides the public certificate information as binary data for download and analysis by any FPKI validating agency or organization. The data involves all certificates retrieved by the FPKI Crawler.

<!--This will be an alert warning box on the IDM.gov webpage.-->
{% include alert-warning.html heading = "Do Not Import These Certificates into a Trust Store before Analysis!" content="This certificate data is made available for analysis purposes. The certificates from these files should not be imported into a Trust Store prior to analysis to determine applicable trust relationships." %}

### 1. All CA Certificates in One File

CACertificatesValidatingToCommonPolicy.p7b contains all certificates retrieved by the FPKI Crawler. This file allows you to easily sort the certificates by expiration date, issuer or subject. 

### 2. All CA Certificates Broken Down into Eight Files

The certificates found in the files ‘CACertificatesValidatingToCommonPolicy_1.p7b’ through ‘CACertificatesValidatingToCommonPolicy_8.p7b’ contain all of the CA certificates found by the Crawler, broken evenly into eight files to simplify analysis.

### 3. Certificate Files by Groups

The CA certificates are partitioned into types and categories as defined in the Certificate Grouping section. For each of these groups, all CA certificates are organized into a single PKCS#7 file. In addition, another PKCS#7 file is generated containing all of the CA certificates plus all additional certificates required for path validation to COMMON.

As an example, the Federal Government’s Veterans Affairs Agency from the Verizon Shared Service Provider Certification Authority has two CAs that can be found in ‘US_Government__VA.p7b’. In order to path validate these CAs to COMMON, a Betrusted cross certificate is required. Therefore, the file ‘US_Government__VA_FullPath.p7b’ contains both the CAs plus the cross certificate.
