## What Should I Do?

These procedures are intended for Enterprise Administrators and/or Network Engineers. 

You will need to download the COMMON root CA certificate and install it on government-furnished Apple devices.

When downloading the COMMON root CA certificate by using the options below, you'll need to verify that it contains these details:

| **Federal Common Policy CA (FCPCA/COMMON)**  | **Certificate Details**                             |
| :--------  | :-------------------------------     |
| Federal Common Policy CA<br>(sometimes shown as _U.S. Government Common Policy_) | http://http.fpki.gov/fcpca/fcpca.crt |
| Distinguished Name | cn=Federal Common Policy CA, ou=FPKI, o=U.S. Government, c=US |
| SHA-1 Thumbprint | 90 5f 94 2f d9 f2 8f 67 9b 37 81 80 fd 4f 84 63 47 f6 45 c1 |
| SHA-256 Thumbprint | 89 4e bc 0b 23 da 2a 50 c0 18 6b 7f 8f 25 ef 1f 6b 29 35 af 32 a9 45 84 ef 80 aa f8 77 a3 a0 6e |

{% include alert-warning.html content="You should never install a root certificate without verifying it." %}

### macOS&nbsp;&mdash;&nbsp;Download COMMON Options

[Option 1. Download COMMON Using a Web Browser](#option-1-download-common-using-a-web-browser)

You can download COMMON to your macOS device by using one of these options: 

**Note:**&nbsp;&nbsp;For all options, replace _{DOWNLOAD_LOCATION}_ with your preferred file download location (e.g., `/Users/Sam.Jackson/Downloads`).

#### Option 1. Download COMMON Using a Web Browser
1. Open your web browser.
2. Navigate to the [COMMON root CA certificate](http://http.fpki.gov/fcpca/fcpca.crt){:target="_blank"}.
3. When prompted, save the certificate file to your download location.
4. Click the *Spotlight* icon and search for _terminal_. 
5. Double-click the Terminal icon (black monitor icon with white ">_") to open a window.
6. Verify that the certificate's hash matches the SHA-256 Thumbprint in the [certificate details](#what-should-i-do):

    ```
	$ shasum -a 256 {DOWNLOAD_LOCATION}/fcpca.crt
    ```

#### Option 2:&nbsp;&nbsp;Download COMMON Using Terminal
1. Click the *Spotlight* icon and search for _terminal_.
2. Double-click the Terminal icon (black monitor icon with white ">_") to open a window.
3. Download a copy of the COMMON root CA certificate:

    ```
	$ curl -o {DOWNLOAD_LOCATION}/fcpca.crt "http://http.fpki.gov/fcpca/fcpca.crt"
    ```

4. Verify that the certificate's hash matches the SHA-256 Thumbprint in the [certificate details](#what-should-i-do):

    ```
	$ shasum -a 256 {DOWNLOAD_LOCATION}/fcpca.crt
    ```

#### Option 3:&nbsp;&nbsp;Email Us

To request an out-of-band copy of the COMMON root CA certificate to download, email us at fpki@gsa.gov.

### macOS&nbsp;&mdash;&nbsp;Install COMMON Options

You can install COMMON in your macOS device's Trust Store by one of these options: 

#### Option 1:&nbsp;&nbsp;Install COMMON Using an Apple Configuration Profile

These procedures are intended for Enterprise Administrators. 

You can create Apple Configuration Profiles (XML files) to distribute trusted root certificates across an Enterprise's Apple devices. An example "Ready-To-Use" Apple Configuration Profile to install COMMON is presented beneath the configuration creation instructions.

##### Create Configuration Profiles to Install COMMON for macOS, iOS, and tvOS

One way you can create a Configuration Profile is by using Apple's free Configurator 2 application:

1. Download and install Configurator 2 from the Apple App Store.
2. Open Configurator and click *File* -> *New Profile*.
3. Under *General*, enter a unique profile *Name*. ("Federal Common Policy Certification Authority Profile" was used for the below example.)
4. Enter a unique profile *Identifier*. ("FCPCA-0001" was used for the below example.)
5. Browse to local *certificate* copies on your device. Select those that you'd like to add to the profile. (For the below example, a copy of the Federal Common Policy CA root certificate was used.)
6. Click *File* -> *Save* to save your profile to a preferred file location.
7. Close Configurator 2.
