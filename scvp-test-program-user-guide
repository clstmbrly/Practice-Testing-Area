---
layout: default
title: Server-based Certificate Validation Protocol Test Program User's Guide
permalink: /1_scvpuser/
---
### Revision History

Date|Revision No.|Pages Revised
---|---|---|---|---
2017-06-08|0|First draft|
2017-07-13|0|Edited draft|
2017-08-08|0|Added notes about setting SCVP_OUTPUT_PATH environment variable and alternative vss.properties file location|
2017-08-11|0|Merged in sections on virtual machines and artifact hosting|

## Table of Contents (Add in)

Space for Table of Contents

## Overview

This document provides an overview of the artifacts and utilities employed by the GSA SCVP Test Program (GSTP). The GSTP aims to confirm an Server-based Certificate Validation Protocol (SCVP) responder is capable of providing accurate certification path validation results in environments with comparable complexity to the U.S. Federal PKI. The test materials do not facilitate confirmation that a product is conformant with all aspects of the SCVP as defined in RFC 5005. Instead, conformance to the SCVP profiles identified for use by GSA [TREAS] is demonstrated.

The GSTP is composed of seven primary components that are used to exercise an SCVP responder under test (RUT):

*	Test SCVP client
*	Test SCVP client scripts 
*	Test SCVP client script generator
*	Test SCVP client script runner (optional)
*	Test artifacts, i.e., certificates and revocation information
*	Sample environment for hosting certificates and revocation information
*	Hosts file to resolve names hosted by sample environment

The test SCVP client is provided along with a set of scripts to cause the client to use test artifacts to interact with a RUT. The test client will emit several streams of information including a summary of test results, basic logging information regarding test client operation, scripts to facilitate re-testing scenarios that failed to yield the expected results, debugging information and, optionally, request and response files for analysis.

A script generator is supplied to generate scripts to drive the test SCVP client. The script generator emits scripts for each set of test artifacts with several variations for interacting with the RUT.

Three distinct sets of test artifacts will be used to test certification path development and certification path validation capabilities:

1. NIST’s Public Key Infrastructure (PKI) Interoperability Test Suite v2 (PKITSv2)
2. NIST’s Path Development Test Suite v2 (PDTSv2)
3. Mock Federal PKI (MFPKI)

All AIA and CRL DP URIs included in the test artifacts feature names that are not routable on the public Internet. A Linux virtual machine that hosts artifacts via HTTP server and OCSP responder instances is available, along with a host file that can be tailored for use in resolving names during certification path processing.

## 2. GSTP Components

### 2.1 Test SCVP Client

The GSTP test client is based on an SCVP client available from GSA on GitHub at: [GSA/VSS](https://github.com/GSA/vss){:target="_blank"}. <!--This link gives 404 error due to Private Repo.--> The GSTP client will also be available via GitHub at a TBD location.  The command line parameters accepted by the client are as follows:

Parameter Name|Parameter Type|Description
---|---|---|---|---
-h, --help|None|Show help message and exit|
Basic Logistics|||
--scvp_profile|{lightweight, long-term-record, batch}|Name of SCVP profile|
-x, --expectSuccess|Boolean value {true, false}|Indicates whether success is expected when validating the --target_cert. Defaults to true|
-l, --logging_conf|Full path and filename of log4j configuration file|Used to customize default logging behavior|
-n, --test_case_name|String value|Friendly name of test case|
-z, --signer_certs|Path to directory to receive certificate(s) used to validate SCVP responses|Save signer certificates as read from a validation policy response to a specified directory then exit|
--log_all_messages|None|Log all requests and responses to the artifacts log, not just those from failed tests. Off by default.|
Target Certificate Details|||
-c, --target_cert|Full path and filename of binary DER encoded certificate|Certificate presented to responder for validation; not used when 
--scvp_profile is set to batch, required otherwise|
-b, --batch_folder|Full path of folder containing binary DER encoded certificates|Certificates presented to responder for validation; used when --scvp_profile is set to batch, not used otherwise|
-t, --trust_anchor|Full path and filename of binary DER encoded certificate|Certificate presented to responder as trust anchor to use for validation; omitted from request by default|
--batch_folder_success|Full path of folder containing binary DER encoded certificates|Certificates presented to responder for validation; used when --scvp_profile is set to batch, not used otherwise; all certificates are expected to validate successfully|
--batch_folder_failure|Full path of folder containing binary DER encoded certificates|Certificates presented to responder for validation; used when --scvp_profile is set to batch, not used otherwise; all certificates are expected to fail validation|
SCVP Request Details|||
-v, --validation_policy|Object identifier value expressed in dot notation form (i.e., 1.2.3.4.5)|Validation policy to include in request; default value is 1.3.6.1.5.5.7.19.1|
--wantBacks|One or more symbolic WantBack names {Cert, BestCertPath, RevocationInfo, PublicKeyInfo, AllCertPaths, EeRevocationInfo, CAsRevocationInfo}|WantBack value(s) to include in request; default is BestCertPath|
Certification Path Validation Algorithm Inputs|||
-p, --certificate_policy|One or more object identifiers expressed in dot notation form (i.e., 1.2.3.4.5)|Certificate policies to use as the user supplied policy set; omitted from request by default|
--inhibitAnyPolicy|Boolean value {true, false}|Boolean value to use as inhibitAnyPolicy; omitted from request by default|
--inhibitPolicyMapping|Boolean value {true, false}|Boolean value to use as inhibitPolicyMapping; omitted from request by default|
--requireExplicitPolicy|Boolean value {true, false}|Boolean value to use as requireExplicitPolicy; omitted from request by default|

Logging output is written to a location identified by the SCVP_OUTPUT_PATH environment variable.

Generally, the client need not be interacted with directly to execute test cases. A set of scripts are provided that drive execution of test scenarios in a variety of contexts. However, prior to using the scripts, the test client itself must be configured to interact with the RUT. A configuration file must be edited to provide the URL of the SCVP interface and a key store must be updated to include keys necessary to verify the SCVP responses. The configuration file is named vss.properties and is located in the /usr/local/tomcat/conf folder. The table below shows the settings that must be modified for test purposes.

Configuration element|Purpose|Example value|
VSS_TRUSTSTORE_SCVP_SIGNER_ISSUER_LABEL|Provides label of SCVP responder’s certificate in the keystore|someresponder|
VSS_SCVP_SERVER_URI|Provides the URI to which SCVP requests are sent|http://example.com/scvp|
VSS_SCVP_DER_ENCODE_DEFAULTS|Determines whether the client DER encodes default fields (some responders require presence of fields the DER requires to be absent)|False|
VSS_SCVP_TEST_CLIENT|Governs custom test client behavior that is only appropriate in a test client|True|

Alternatively, the location of the vss.properties file can be provided as a Java system variable when the client is launched as shown below (which also shows temporarily reassigning the SCVP_OUTPUT_PATH environment variable for a single run):

```
SCVP_OUTPUT_PATH=/<some path>/SCVP_OUTPUT_PATH2 java -Dvss.configLocation=/<some path>/vss.properties -jar vss2.jar --scvp_profile lightweight -n 4.1.1 -c /<some path>/ValidCertificatePathTest1EE.crt --wantBacks BestCertPath
```
Once the configuration file edits have been performed, the RUT’s certificate must be added to the keystore.ks file located in the /usr/local/tomcat/conf folder. If the RUT’s certificate is not handy and the RUT supports validation policy requests, the test client can be used to retrieve the certificate via the following command:

```
java –jar vss2.jar –s /path/to/receive/certificate.der 
```
The certificate may then be imported into the keystore using:

```
keytool -keystore /usr/local/tomcat/conf/vssTrustStore.jks -importcert -file /path/to/receive/certificate.der -alias someresponder 
```
The test client will write logs to the location identified by the SCVP_OUTPUT_PATH environment variable.

### 2.2 Test SCVP client scripts and script generator

During the execution of the GSTP, the test SCVP client will be executed hundreds of times. To simplify execution of the test cases, a set of scripts are provided that reference a target certificate or collection of target certificates and provide a set of appropriate command line parameters. These scripts can be modified for the environment in which the test client will be used. Scripts may be manually altered or regenerated to change paths to test artifacts, to change output folder location or to change the list of wantBacks.

### 2.3 Test SCVP client script generator

The script generator utility takes the following command line parameters:

**STOPPED HERE** BIG CODE BLOCK STARTS SCVPSCRIPTCODEGENERATOR V1.0.0 usage

abc|def|ghi|
abc|def|ghi|
