

## Worksheet Traceability Matrix

| **Certificate Profile**           | **Shared<BR>Service<BR>Provider (SSP)<sup>[1](#1)</sup> Worksheet Numbers  | **Federal<BR>Bridge<BR>Certification<BR>Authority (FBCA)<sup>[2](#2)</sup>**     | **Personal<BR>Identity<BR>Verification<BR>- Interoperable<BR>(PIV-I)<sup>[3](#3)</sup>**     | **Current**   |
| :----------------------------------  | :---------:  | :-----------:    | :-----------:      | :-----------:      |
|                       | Worksheet Numbers                                     |
| Self-Signed CA                       | 1            | 1                |               | 1             |
| Key Rollover CA                      | 2             | 2               |  1            | 2             |
| Peer-to-Peer Cross-Certificate       | 3             | 3                |  2            | 3             |
| Intermediate or Subordinate CA       | 3              | 3               |  2            | 4             |
| CRL                                  | 4              | 4               |  3            | 5             |
| End Entity Signature       |                | 5        |                 | 6             |
| Common End Entity Signature       | 5              |              |               | 7             |
| PIV-I End Entity Signature       |                |              |  6            | 8             |
| End Entity Key Management       |                |  6           |               | 9             |
| Common End Entity Key Management       | 6               |             |               | 10             |
| PIV-I End Entity Key Management       |                |             | 7              | 11             |
| PIV Card Authentication       | 8               |             |               | 12             |
| PIV-I Card Authentication       |                |             |  4             | 13             |
| PIV Authentication       |  9              |             |               | 14             |
| Common Derived PIV Authentication       |  11              |             |               | 15             |
| PIV-I Authentication       |                |             |  5             | 16             |
| Common PIV Content Signing       | 10               |             |               | 17             |
| PIV-I Content Signing       |                |             |  8             | 18             |
| Computing and Communications Devices       | 7               |             |               | 19             |
| Delegated OCSP Responder       | 12               |             | 9             | 20             |


------------
<a name="1">1</a>. RFC 5280, _Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile_, D. Cooper, S. Santessen<BR>
<a name="2">2</a>. FBCA Certificate Profile
<a name="3">3</a>. PIV-I Certificate Profile
