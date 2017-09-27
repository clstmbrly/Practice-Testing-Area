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

Parameter Name|Parameter Type|Description|
---|---|---|
-h, --help|None|Show help message and exit|
&nbsp;&nbsp;---------------------|----------------**_Basic Logistics_**----------------|----------------------------------&nbsp;&nbsp;|
--scvp_profile|{lightweight, long-term-record, batch}|Name of SCVP profile|
-x, --expectSuccess|Boolean value {true, false}|Indicates whether success is expected when validating the --target_cert. Defaults to true|
-l, --logging_conf|Full path and filename of log4j configuration file|Used to customize default logging behavior|
-n, --test_case_name|String value|Friendly name of test case|
-z, --signer_certs|Path to directory to receive certificate(s) used to validate SCVP responses|Save signer certificates as read from a validation policy response to a specified directory then exit|
--log_all_messages|None|Log all requests and responses to the artifacts log, not just those from failed tests. Off by default.|
&nbsp;&nbsp;---------------------|----------**_Target Certificate Details_**---------|----------------------------------&nbsp;&nbsp;|
-c, --target_cert|Full path and filename of binary DER encoded certificate|Certificate presented to responder for validation; not used when 
--scvp_profile is set to batch, required otherwise|
-b, --batch_folder|Full path of folder containing binary DER encoded certificates|Certificates presented to responder for validation; used when --scvp_profile is set to batch, not used otherwise|
-t, --trust_anchor|Full path and filename of binary DER encoded certificate|Certificate presented to responder as trust anchor to use for validation; omitted from request by default|
--batch_folder_success|Full path of folder containing binary DER encoded certificates|Certificates presented to responder for validation; used when --scvp_profile is set to batch, not used otherwise; all certificates are expected to validate successfully|
--batch_folder_failure|Full path of folder containing binary DER encoded certificates|Certificates presented to responder for validation; used when --scvp_profile is set to batch, not used otherwise; all certificates are expected to fail validation|
&nbsp;&nbsp;---------------------|------------**SCVP Request Details**-----------|----------------------------------&nbsp;&nbsp;|
-v, --validation_policy|Object identifier value expressed in dot notation form (i.e., 1.2.3.4.5)|Validation policy to include in request; default value is 1.3.6.1.5.5.7.19.1|
--wantBacks|One or more symbolic WantBack names {Cert, BestCertPath, RevocationInfo, PublicKeyInfo, AllCertPaths, EeRevocationInfo, CAsRevocationInfo}|WantBack value(s) to include in request; default is BestCertPath|
&nbsp;&nbsp;---------------------|**Certification Path Validation </br>Algorithm Inputs**|----------------------------------&nbsp;&nbsp;|
-p, --certificate_policy|One or more object identifiers expressed in dot notation form (i.e., 1.2.3.4.5)|Certificate policies to use as the user supplied policy set; omitted from request by default|
--inhibitAnyPolicy|Boolean value {true, false}|Boolean value to use as inhibitAnyPolicy; omitted from request by default|
--inhibitPolicyMapping|Boolean value {true, false}|Boolean value to use as inhibitPolicyMapping; omitted from request by default|
--requireExplicitPolicy|Boolean value {true, false}|Boolean value to use as requireExplicitPolicy; omitted from request by default|

Logging output is written to a location identified by the SCVP_OUTPUT_PATH environment variable.

Generally, the client need not be interacted with directly to execute test cases. A set of scripts are provided that drive execution of test scenarios in a variety of contexts. However, prior to using the scripts, the test client itself must be configured to interact with the RUT. A configuration file must be edited to provide the URL of the SCVP interface and a key store must be updated to include keys necessary to verify the SCVP responses. The configuration file is named vss.properties and is located in the /usr/local/tomcat/conf folder. The table below shows the settings that must be modified for test purposes.

Configuration element|Purpose|Example value|
---|---|---|
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

ScvpScriptGenerator v1.0.0 usage||
---|---|
-h [ --help ]|Print usage instructions|
-l [ --logging_conf ] arg|Logging configuration to support report generation|
--pkits_2048_folder arg|Folder containing PKITS 2048 edition (root of Renamed folder containing 0, 1, 2, etc. folders and all certificates)|
--pkits_4096_folder arg|Folder containing PKITS 4096 edition (root of Renamed folder containing 0, 1, 2, etc. folders and all certificates)|
--pkits_p256_folder arg|Folder containing PKITS p256 edition (root of Renamed folder containing 0, 1, 2, etc. folders and all certificates)|
--pkits_p384_folder arg|Folder containing PKITS p384 edition (root of Renamed folder containing 0, 1, 2, etc. folders and all certificates)|
--pdts_folder arg|Folder containing PDTS edition|
--mfpki_folder arg|Folder containing MFPKI edition|
--mfpki_ta arg|File containing the MFPKI trust anchor|
--output_folder arg|Folder to receive generated scripts|
-l [ --logging ] arg|Logging configuration for ScvpScriptGenerator logging purposes|
--want_back arg|List of OIDS in dot notation form (i.e., 1.2.3.4.5) to be passes as --wantBacks to the SCVP client|

The following script can be tailored to regenerate a full complement of scripts to support execution of the GSTP against a given SCVP responder:

```
./ScvpScriptGenerator --mfpki_folder /<path>/MFPKI/EEs --mfpki_ta /<path>/MFPKI/TAs/905F942FD9F28F679B378180FD4F846347F645C1.fake.der 
--output_folder /<path>/GSTP --want_back BestCertPath --want_back RevocationInfo
./ScvpScriptGenerator --pdts_folder /<path>/PDTS/Renamed --output_folder /<path>/GSTP --want_back BestCertPath --want_back RevocationInfo
./ScvpScriptGenerator --pkits_2048_folder /<path>/PKITS_2048/Renamed 
--output_folder /<path>/GSTP --want_back BestCertPath --want_back RevocationInfo
./ScvpScriptGenerator --pkits_4096_folder /<path>/PKITS_4096/Renamed 
--output_folder /<path>/GSTP --want_back BestCertPath --want_back RevocationInfo
./ScvpScriptGenerator --pkits_p256_folder /<path>/PKITS_P256/Renamed 
--output_folder /<path>/GSTP --want_back BestCertPath --want_back RevocationInfo
./ScvpScriptGenerator --pkits_p384_folder /<path>/PKITS_P256/Renamed 
--output_folder /<path>/GSTP --want_back BestCertPath --want_back RevocationInfo
```
The resulting output will be a set of scripts, as listed below. For the MFPKI and each PKITSv2 edition, a script targeting the default SCVP validation policy will be emitted both with and without trust anchor inclusion in the request for each SCVP profile type. PDTS will receive similar, except no batch scripts are emitted for PDTS. Similarly, for the MFPKI and each PKITSv2 edition, a script targeting a non-default SCVP validation policy will be emitted for each profile type. PDTS will receive similar, except no batch script is emitted. Fifty-one scripts are generated in total.

*	MFPKI_DEFAULT_OMIT_TA_batch.sh
*	MFPKI_DEFAULT_OMIT_TA_lightweight.sh
*	MFPKI_DEFAULT_OMIT_TA_longterm.sh
*	MFPKI_DEFAULT_WITH_TA_batch.sh
*	MFPKI_DEFAULT_WITH_TA_lightweight.sh
*	MFPKI_DEFAULT_WITH_TA_longterm.sh
*	MFPKI_NON_DEFAULT_batch.sh
*	MFPKI_NON_DEFAULT_lightweight.sh
*	MFPKI_NON_DEFAULT_longterm.sh
*	PDTS_DEFAULT_OMIT_TA_lightweight.sh
*	PDTS_DEFAULT_OMIT_TA_longterm.sh
*	PDTS_DEFAULT_WITH_TA_lightweight.sh
*	PDTS_DEFAULT_WITH_TA_longterm.sh
*	PDTS_NON_DEFAULT_lightweight.sh
*	PDTS_NON_DEFAULT_longterm.sh
*	PKITS_2048_DEFAULT_OMIT_TA_batch.sh
*	PKITS_2048_DEFAULT_OMIT_TA_lightweight.sh
*	PKITS_2048_DEFAULT_OMIT_TA_longterm.sh
*	PKITS_2048_DEFAULT_WITH_TA_batch.sh
*	PKITS_2048_DEFAULT_WITH_TA_lightweight.sh
*	PKITS_2048_DEFAULT_WITH_TA_longterm.sh
*	PKITS_2048_NON_DEFAULT_batch.sh
*	PKITS_2048_NON_DEFAULT_lightweight.sh
*	PKITS_2048_NON_DEFAULT_longterm.sh
*	PKITS_4096_DEFAULT_OMIT_TA_batch.sh
*	PKITS_4096_DEFAULT_OMIT_TA_lightweight.sh
*	PKITS_4096_DEFAULT_OMIT_TA_longterm.sh
*	PKITS_4096_DEFAULT_WITH_TA_batch.sh
*	PKITS_4096_DEFAULT_WITH_TA_lightweight.sh
*	PKITS_4096_DEFAULT_WITH_TA_longterm.sh
*	PKITS_4096_NON_DEFAULT_batch.sh
*	PKITS_4096_NON_DEFAULT_lightweight.sh
*	PKITS_4096_NON_DEFAULT_longterm.sh
*	PKITS_P256_DEFAULT_OMIT_TA_batch.sh
*	PKITS_P256_DEFAULT_OMIT_TA_lightweight.sh
*	PKITS_P256_DEFAULT_OMIT_TA_longterm.sh
*	PKITS_P256_DEFAULT_WITH_TA_batch.sh
*	PKITS_P256_DEFAULT_WITH_TA_lightweight.sh
*	PKITS_P256_DEFAULT_WITH_TA_longterm.sh
*	PKITS_P256_NON_DEFAULT_batch.sh
*	PKITS_P256_NON_DEFAULT_lightweight.sh
*	PKITS_P256_NON_DEFAULT_longterm.sh
*	PKITS_P384_DEFAULT_OMIT_TA_batch.sh
*	PKITS_P384_DEFAULT_OMIT_TA_lightweight.sh
*	PKITS_P384_DEFAULT_OMIT_TA_longterm.sh
*	PKITS_P384_DEFAULT_WITH_TA_batch.sh
*	PKITS_P384_DEFAULT_WITH_TA_lightweight.sh
*	PKITS_P384_DEFAULT_WITH_TA_longterm.sh
*	PKITS_P384_NON_DEFAULT_batch.sh
*	PKITS_P384_NON_DEFAULT_lightweight.sh
*	PKITS_P384_NON_DEFAULT_longterm.sh

### 2.3	Test SCVP client script runner

The following script can be used to execute all GSTP test cases when run from a folder containing the test SCVP client with all logs collecting in one location.

```
bash /<path>/MFPKI_DEFAULT_OMIT_TA_batch.sh
bash /<path>/MFPKI_DEFAULT_OMIT_TA_lightweight.sh
bash /<path>/MFPKI_DEFAULT_OMIT_TA_longterm.sh
bash /<path>/MFPKI_DEFAULT_WITH_TA_batch.sh
bash /<path>/MFPKI_DEFAULT_WITH_TA_lightweight.sh
bash /<path>/MFPKI_DEFAULT_WITH_TA_longterm.sh
bash /<path>/MFPKI_NON_DEFAULT_batch.sh
bash /<path>/MFPKI_NON_DEFAULT_lightweight.sh
bash /<path>/MFPKI_NON_DEFAULT_longterm.sh
bash /<path>/PDTS_DEFAULT_OMIT_TA_lightweight.sh
bash /<path>/PDTS_DEFAULT_OMIT_TA_longterm.sh
bash /<path>/PDTS_DEFAULT_WITH_TA_lightweight.sh
bash /<path>/PDTS_DEFAULT_WITH_TA_longterm.sh
bash /<path>/PDTS_NON_DEFAULT_lightweight.sh
bash /<path>/PDTS_NON_DEFAULT_longterm.sh
bash /<path>/PKITS_2048_DEFAULT_OMIT_TA_batch.sh
bash /<path>/PKITS_2048_DEFAULT_OMIT_TA_lightweight.sh
bash /<path>/PKITS_2048_DEFAULT_OMIT_TA_longterm.sh
bash /<path>/PKITS_2048_DEFAULT_WITH_TA_batch.sh
bash /<path>/PKITS_2048_DEFAULT_WITH_TA_lightweight.sh
bash /<path>/PKITS_2048_DEFAULT_WITH_TA_longterm.sh
bash /<path>/PKITS_2048_NON_DEFAULT_batch.sh
bash /<path>/PKITS_2048_NON_DEFAULT_lightweight.sh
bash /<path>/PKITS_2048_NON_DEFAULT_longterm.sh
bash /<path>/PKITS_4096_DEFAULT_OMIT_TA_batch.sh
bash /<path>/PKITS_4096_DEFAULT_OMIT_TA_lightweight.sh
bash /<path>/PKITS_4096_DEFAULT_OMIT_TA_longterm.sh
bash /<path>/PKITS_4096_DEFAULT_WITH_TA_batch.sh
bash /<path>/PKITS_4096_DEFAULT_WITH_TA_lightweight.sh
bash /<path>/PKITS_4096_DEFAULT_WITH_TA_longterm.sh
bash /<path>/PKITS_4096_NON_DEFAULT_batch.sh
bash /<path>/PKITS_4096_NON_DEFAULT_lightweight.sh
bash /<path>/PKITS_4096_NON_DEFAULT_longterm.sh
bash /<path>/PKITS_P256_DEFAULT_OMIT_TA_batch.sh
bash /<path>/PKITS_P256_DEFAULT_OMIT_TA_lightweight.sh
bash /<path>/PKITS_P256_DEFAULT_OMIT_TA_longterm.sh
bash /<path>/PKITS_P256_DEFAULT_WITH_TA_batch.sh
bash /<path>/PKITS_P256_DEFAULT_WITH_TA_lightweight.sh
bash /<path>/PKITS_P256_DEFAULT_WITH_TA_longterm.sh
bash /<path>/PKITS_P256_NON_DEFAULT_batch.sh
bash /<path>/PKITS_P256_NON_DEFAULT_lightweight.sh
bash /<path>/PKITS_P256_NON_DEFAULT_longterm.sh
bash /<path>/PKITS_P384_DEFAULT_OMIT_TA_batch.sh
bash /<path>/PKITS_P384_DEFAULT_OMIT_TA_lightweight.sh
bash /<path>/PKITS_P384_DEFAULT_OMIT_TA_longterm.sh
bash /<path>/PKITS_P384_DEFAULT_WITH_TA_batch.sh
bash /<path>/PKITS_P384_DEFAULT_WITH_TA_lightweight.sh
bash /<path>/PKITS_P384_DEFAULT_WITH_TA_longterm.sh
bash /<path>/PKITS_P384_NON_DEFAULT_batch.sh
bash /<path>/PKITS_P384_NON_DEFAULT_lightweight.sh
bash /<path>/PKITS_P384_NON_DEFAULT_longterm.sh
```

The following Python code (provided as GSTPScriptRunner.py) can be used to run the scripts listed above with logs moved in between each script.

```
import glob2
from optparse import OptionParser
import os
from os.path import join
import signal
from subprocess import PIPE, Popen
import sys
from time import gmtime, strftime

BASH_EXE = "/bin/bash"

bash_process = None


# noinspection PyUnusedLocal
def signal_handler(signal_param, frame_param):
    if bash_process:
        bash_process.kill()
        print('Killed GSTP test execution process')
    sys.exit(0)


def main():
    parser = OptionParser()
    parser.add_option("-i", "--inputFolder", dest="input_folder", default="",
                      help="Folder containing scripts to run")
    parser.add_option("-l", "--logFolder", dest="log_folder", default="",
                      help="Folder containing logs to move")
    parser.add_option("-d", "--destLogFolder", dest="dest_log_folder", default="",
                      help="Folder containing logs to move")
    parser.add_option("-p", "--product", dest="product", default="",
                      help="Short name of product under test (for use in naming relocated log folders)")

    (options, args) = parser.parse_args()

    signal.signal(signal.SIGINT, signal_handler)
    global bash_process

    log_folder = options.log_folder
    orig_dest_log_folder = options.dest_log_folder
    product = options.product

    if os.path.isfile(os.path.join(log_folder, 'artifacts.csv')):
        os.remove(os.path.join(log_folder, 'artifacts.csv'))
    if os.path.isfile(os.path.join(log_folder, 'results.csv')):
        os.remove(os.path.join(log_folder, 'results.csv'))
    if os.path.isfile(os.path.join(log_folder, 'client.txt')):
        os.remove(os.path.join(log_folder, 'client.txt'))
    if os.path.isfile(os.path.join(log_folder, 'validation_failures.txt')):
        os.remove(os.path.join(log_folder, 'validation_failures.txt'))
    if os.path.isfile(os.path.join(log_folder, 'profile_failures.txt')):
        os.remove(os.path.join(log_folder, 'profile_failures.txt'))

    if options.input_folder:
        only_files = glob2.glob(options.input_folder + '/*.sh')

        for filename in only_files:
            t = strftime("%Y%m%d%H%M%S", gmtime())
            print("Started " + filename + " at " + t)

            bash_command = BASH_EXE + " " + join(options.input_folder, filename)
            bash_process = Popen(bash_command, shell=True, stdout=PIPE)
            bash_process.wait()


# noinspection PyUnusedLocal
            process = None

            dest_log_folder = os.path.join(orig_dest_log_folder, product + "_" +
                                           os.path.splitext(os.path.basename(filename))[0] + "_" + t)
            os.mkdir(dest_log_folder)
            if os.path.isfile(os.path.join(log_folder, 'artifacts.csv')):
                os.rename(os.path.join(log_folder, 'artifacts.csv'), os.path.join(dest_log_folder, 'artifacts.csv'))
            if os.path.isfile(os.path.join(log_folder, 'results.csv')):
                os.rename(os.path.join(log_folder, 'results.csv'), os.path.join(dest_log_folder, 'results.csv'))

            art_files = glob2.glob(options.log_folder + '/artifacts*.csv')
            for art in art_files:
                if os.path.isfile(art):
                    os.rename(art, os.path.join(dest_log_folder, os.path.basename(art)))

            res_files = glob2.glob(options.log_folder + '/results*.csv')
            for res in res_files:
                if os.path.isfile(res):
                    os.rename(res, os.path.join(dest_log_folder, os.path.basename(res)))

            if os.path.isfile(os.path.join(log_folder, 'client.txt')):
                os.rename(os.path.join(log_folder, 'client.txt'), os.path.join(dest_log_folder, 'client.txt'))
            if os.path.isfile(os.path.join(log_folder, 'debug.txt')):
                os.rename(os.path.join(log_folder, 'debug.txt'), os.path.join(dest_log_folder, 'debug.txt'))
            if os.path.isfile(os.path.join(log_folder, 'validation_failures.txt')):
                os.rename(os.path.join(log_folder, 'validation_failures.txt'), os.path.join(dest_log_folder,
                                                                                            'validation_failures.txt'))
            if os.path.isfile(os.path.join(log_folder, 'profile_failures.txt')):
                os.rename(os.path.join(log_folder, 'profile_failures.txt'), os.path.join(dest_log_folder,
                                                                                         'profile_failures.txt'))

            t2 = strftime("%Y%m%d%H%M%S", gmtime())
            print("Completed " + filename + " at " + t2)


if __name__ == '__main__':
    main()




abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
abc|def|ghi|
