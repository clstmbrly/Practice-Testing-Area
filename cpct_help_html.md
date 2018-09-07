<hr />

<p>layout: default
title: Certificate Profile Conformance Tool (CPCT) Help
collection: docs</p>

<h2 id="permalinkdocsuserguide">permalink: docs/userguide/</h2>

<h2>What is the Certificate Profile Conformance Tool (CPCT)?</h2>

<p>Do you often need to analyze Federal PKI certificates for conformance to certificate profiles? If your answer is _yes_, then the Certificate Profile Conformance Tool (CPCT) is for you.</p>

<p>CPCT is a friendly tool that instantaneously analyzes a certificate and displays its conformance results. Not only that&nbsp;-&nbsp;CPCT clearly explains the reason for any nonconformance. What's more, you can download a formatted Test Report (.xls or .pdf) to submit as part of an Federal PKI Annual Review package or retain for your organization's needs.</p>

<p></p>

<h2>Unordered List with Disc Bullets</h2>

<ul style="list-style-type:disc">
<li>[Who Needs CPCT?](#who-needs-cpct)</li>
<li>[Operating System Requirements](#operating-system-requirements)</li>
<li>[How Does This Work?](#how-does-this-work)</li>
<li>[Detailed Steps](#detailed-steps)</li>
<li>[Troubleshooting](#troubleshooting)</li>
<li>[Feature Request](#feature-request)</li></ul>

<p></p>

<h2>Who Needs CPCT?</h2>

<ol>
<li><b>Agencies/organizations submitting FPKI Annual Review Packages</b> - Use it to test certificates and download Test Reports.</li>
<li><b>PIV or SSL/TLS Certificate Issuers</b> - Use it to test certificates as part of a QA process.</li>
<li><b>Subscribers</b> - Use it to determine who should correct certificate failures.</li>
<li><b>Anyone who analyzes FPKI certificates for conformance</b>.</li>
</ol>

<p>
<br></p>

<h2>Operating System Requirements</h2>

<ul style="list-style-type:disc"> 
<li>Windows or macOS</li>
<li>iOS - Not recommended for CPCT</li>
<li>Android - Not recommended for CPCT</li>
</ul>

<p></p>

<h2>How Does This Work?</h2>

<p>{% include alert-info.html content="In-depth experience with Federal PKI certificates and certificate profiles is recommended." %}</p>

<p>The basic steps are:</p>

<p></p>

<ul style="list-style-type:disc">
<li>At the <a href="https://cpct.app.cloud.gov/" target="_blank">CPCT main screen</a>, pick the <b>Profile Document</b>, <b>Document Version</b>, and <b>Certificate Profile</b> related to a certificate you want to test. Then, upload the certificate.</li> 

<li>CPCT displays the certificate's test results. A status banner displays as <i>green</i> (if the certificate conforms) or <i>red</i> (if the certificate doesn't conform). Each field and extension will show either a <i>PASS</i> (a checkmark) or <i>FAIL (with explanation)</i>.</li> 

<li>You can download a formatted Test Report (.xls or .pdf) to submit as part of an Federal PKI Annual Review packages or to retain for your organization's needs.</li> 
</ul>

<p></p>

<h2>Detailed Steps</h2>

<!--The short names aren't ideal. Neither "Common Policy" nor "Federal Bridge" appear in the actual policies' titles. For normal publications, ideally prior to short name use (or at least in a footnote as I have added at the end), the full titles should be defined.-->

<h4 id="selectprofiledocuments">Select Profile Documents</h4>

<ul>
<li>Navigate to <a href="https://cpct.app.cloud.gov/" target="_blank">CPCT</a>.</li>

<li>From the 3 drop-downs, pick:
 o    <strong>Profile Document</strong> (The options are short names for the Profile Documents: <em>Common Policy SSP Program</em><sup><a href="#1">1</a></sup>; <em>Federal PKI/Federal Bridge</em><sup><a href="#2">2</a></sup>; <em>PIV Interoperable (PIV-I)</em><sup><a href="#3">3</a></sup>.)<br>
 o    <strong>Document Version</strong> (The most recent Version will set automatically when you select the Profile Document.)<br>
 o    <strong>Certificate Profile</strong> (e.g., PIV Authentication)<br></li>
</ul>

<h4 id="uploadacertificate">Upload a Certificate</h4>

<ul>
<li><p>Next, upload your certificate (as a .ctr, .pem, .cer, or .der file) using either of these options:</p>

<p>o <strong>Drag-and-drop</strong> your certificate to anywhere on the CPCT main screen. <em>Test results display for the uploaded certificate.</em><br>
 o Click the <strong>Upload Certificate</strong> button and browse to the certificate. Click it, and then click <strong>Open.</strong> <em>Test results display for the uploaded certificate.</em><br></p></li>
</ul>

<p>{% include alert-info.html content="Notice that the Test Results screen includes the CPCT drop-downs for easy upload of more certificates." %}</p>

<h4 id="reviewcertificatetestresults">Review Certificate Test Results</h4>

<ul>
<li><p>At the top of the Test Results screen, a status banner in <em>green</em> (conforms) or <em>red</em> (doesn't conform) banner gives the test summary: </p></li>

<li><p><strong>Tested [n] fields: No Problems detected</strong> 
<em>OR</em> </p></li>

<li><p><strong>Tested [n] fields: [n] problems detected</strong></p></li>
</ul>

<p>The Test Results columns are:</p>

<ul>
<li><strong>Field</strong> - Lists fields AND extensions.</li>

<li><strong>Content</strong> - Lists field and extension details.</li>

<li><strong>Analysis</strong> - For each field and extension, this column shows a checkmark (for <em>PASS</em>) or <em>FAIL (with an explanation)</em>.</li>
</ul>

<h4 id="downloadatestreport">Download a Test Report</h4>

<ul>
<li>To download a formatted Test Report, click the <strong>XLS</strong> or <strong>PDF</strong> button below the status banner. </li>
</ul>

<h2>Troubleshooting</h2>

<h3>Certificate Errors</h3>

<ul>
<li><strong>Tested [n] fields: [n] problems detected</strong>. <em>1. One or more fields or extensions do not conform; 2. The wrong Profile Document, Document Version, and/or Certificate Profile were selected.</em>; or 3. Another problem exists.</li>

<li><strong>FAIL (with explanation)</strong> shown in the <strong>Analysis</strong> column. <em>The field or extension doesn't conform.</em></li>
</ul>

<h3>Possible Error Messages</h3>

<ul>
<li><strong>You can't upload files of this type.</strong> <em>CPCT doesn't recognize the certificate file type. The allowable file types are: .crt, .cer, .pem, and .der.</em></li>
</ul>

<h3>Other Problems or Discrepancies</h3>

<p>If you encounter a problem or discrepancy:</p>

<ul>
<li>Check to make sure that you selected the right Profile Document, Document Version, and Certificate Profile.</li>

<li>CPCT doesn't recognize the certificate file type. <em>Allowable file types are: .ctr, .pem, cer., and .der.</em></li>

<li>Check the certificate's Validity Period. The test certificate may have expired.<!--Would this show up as a "problem" in the status banner with a "FAIL" for Validity Period"?--> </li>

<li>Think there might be an application error?  Please <a href="https://github.com/GSA/fpkilint/blob/dev/docs/cpct_contact_us.md" target="_blank">contact  us</a>.</li>

<li>Think a test result may be incorrect (e.g., "false positive" or "false negative")? Please <a href="https://github.com/GSA/fpkilint/blob/dev/docs/cpct_contact_us.md" target="_blank">contact  us</a>. </li>
</ul>

<h3>What If I Can't Resolve an Issue?</h3>

<ul>
<li>Create a GitHub issue in the <a href="https://github.com/GSA/fpkilint" target="_blank">CPCT Respository</a> and attach the certificate to the issue. (<strong>Note:</strong>&nbsp;&nbsp;You will need a GitHub account to do this: <a href="https://github.com/join" target="_blank">create a GitHub account link</a>.)<br>
<em>OR</em><br></li>

<li>Email us at fpki@gsa.gov and attach your certificate. (<strong>Note:</strong>&nbsp;&nbsp;Please rename your certificate with <strong>.txt</strong> file extension.) </li>
</ul>

<p>We will respond as soon as possible.</p>

<h2>Feature Request</h2>

<p>If you would like to request a new CPCT feature, please <a href="https://github.com/GSA/fpkilint/blob/dev/docs/cpct_contact_us.md" target="_blank">contact  us</a>.</p>

<hr />

<p><a name="1">1</a>. Short name for <em>X.509 Certificate and Certificate Revocation List (CRL) Extensions Profile for the Shared Service Providers (SSP) Program Policy</em>.<br>
<a name="2">2</a>. Short name for <em>Federal Public Key Infrastructure (PKI) X.509 Certificate and CRL Extensions Profile</em>.<br>
<a name="3">3</a>. Short name for <em>X.509 Certificate and Certificate Revocation List (CRL) Extensions Profile for Personal Identity Verification Interoperable (PIV-I) Cards</em>.<br></p>