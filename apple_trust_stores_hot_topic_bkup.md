---
layout: default
navtitle: Federal Common Policy CA Removal from Apple Trust Stores Impact
title: Federal Common Policy CA Removal from Apple Trust Stores Impact
pubDate: June 8, 2018
collection: announcements
permalink: announcements/applepkichanges/
description: Upcoming changes regarding Apple's Root Certificate Program could impact your agency. The Federal PKI Policy Authority has elected to remove our U.S. Government Root CA certificate (Federal Common Policy CA) from the Apple Operating System Trust Stores.  This change will impact government users of Apple iOS, macOS, and tvOS, starting in **September-October 2018**. <br><br> This change will cause government users to receive errors when encountering instances of a Federal PKI CA-issued certificate. You can mitigate the impact for government intranets and government-furnished equipment by using configuration management tools for federal devices.
---
MY CHANGED VERSION.  LACHELLE WAS MAKING CHANGES AT THE SAME TIME, SO THIS VERSION WAS NOT COMMITTED.
REVIEWED HER CHANGES WITH RYAN AND AGREED.

Upcoming changes regarding Apple's Root Certificate Program could impact your agency. The Federal PKI Policy Authority (FPKIPA) has elected to remove our U.S. Government Root CA certificate (Federal Common Policy CA [FCPCA/COMMON]) from Apple's pre-installed Operating System Trust Stores. 

{% include alert-info.html content="This announcement will be updated as more information and additional procedures become available. Please watch for updates from the Federal PKI listservs, ICAM listservs, and the ICAM Subcommittee." %}

Starting in **September-October 2018**, government users of Apple iOS, macOS, and tvOS devices will receive errors when encountering instances of a Federal PKI CA-issued certificate. You can mitigate the impact for government intranets and government-furnished equipment by using configuration management tools for federal devices.

{% include alert-info.html content="The FPKIPA has also elected to remove the Federal Common Policy CA root certificate from Microsoft's Certificate Trust List." %}

- [How Does this Work?](#how-does-this-work)
- [What Will Be Impacted?](#what-will-be-impacted)
- [What Should I Do?](#what-should-i-do)
- [Frequently Asked Questions](#frequently-asked-questions)
- [Additional Resources](#additional-resources)

## How Does This Work?

Apple currently distributes the Federal Common Policy CA (FCPCA/COMMON) through its pre-installed Operating System Trust Stores for iOS, macOS, and tvOS. 

Three root CA certificate _types_ reside in Apple's Trust Stores:

- _Trusted Certificates_ &mdash; Trusted certificates that establish a chain of trust.
- _Always Ask_ &mdash; Untrusted certificates that are not blocked. If a resource (e.g., website or signed email) chains to one of these certificates, the Apple Operating System will ask you to choose whether or not to trust it.
- _Blocked_ &mdash; Potentially compromised certificates that will never be trusted.

These certificate types are stored within Apple _Keychains_:

- _Login Keychain_ &mdash; Certificates associated with a user account logged into a device.
- _System Keychain_ &mdash; Certificates associated with all user accounts on a device (similar to the Microsoft Windows' _Local Machine_ certificate store). 
- _System Roots Keychain_ &mdash; Includes Apple's _pre-installed_, trusted root CA certificates. COMMON will be removed from this Keychain.
 
## What Will Be Impacted?

These Apple Operating System versions will be impacted:

|**macOS**|**iOS**|**tvOS**|
| :-------- |:-------- |:-------- |
| High Sierra (10.13) |	 iOS11 | tvOS11 |


Government users will receive errors on government-furnished equipment if any of these are true:

1. Logging into a government network with a PIV credential 
2. Authenticating to a government Virtual Private Network (VPN) endpoint with a PIV credential
3. Authenticating to an internet-facing, government collaboration portal with a PIV credential 
4. Browsing with Safari, Chrome, or Edge (iOS) to a government **intranet** website that uses a Federal PKI CA-issued TLS/SSL certificate
5. Opening an Apple Mail or Microsoft Outlook email that was digitally signed using a Federal PKI CA-issued certificate
6. Opening a Microsoft Office document that was digitally signed with a Federal PKI CA-issued certificate

This change will also impact Federal Government partners that rely on COMMON&mdash;for example, a Department of Defense employee sending a digitally signed email to a business partner. 

You can mitigate the risk to government missions, intranets, applications, and government-furnished equipment.

{% include alert-info.html content="If you are unsure whether your applications will be affected, email us at fpki@gsa.gov." %} 

## What Should I Do?

These procedures are intended for Enterprise Administrators and/or Network Engineers.

### macOS&nbsp;&mdash;&nbsp;Download COMMON Options

You will need to download the COMMON root CA certificate and install it on government-furnished Apple devices by using one of these options:

* [Option 1. Download COMMON Using a Web Browser](#option-1-download-common-using-a-web-browser)
* [Option 2. Download COMMON Using Terminal](#option-2-download-common-using-terminal)
* [Option 3. Email Us](#option-3-email-us)

When downloading the COMMON root CA certificate, you'll need to verify that it contains these details:

| **Federal Common Policy CA (FCPCA/COMMON)**  | **Certificate Details**                             |
| :--------  | :-------------------------------     |
| Federal Common Policy CA<br>(sometimes shown as _U.S. Government Common Policy_) | http://http.fpki.gov/fcpca/fcpca.crt |
| Distinguished Name | cn=Federal Common Policy CA, ou=FPKI, o=U.S. Government, c=US |
| SHA-1 Thumbprint | 90 5f 94 2f d9 f2 8f 67 9b 37 81 80 fd 4f 84 63 47 f6 45 c1 |
| SHA-256 Thumbprint | 89 4e bc 0b 23 da 2a 50 c0 18 6b 7f 8f 25 ef 1f 6b 29 35 af 32 a9 45 84 ef 80 aa f8 77 a3 a0 6e |

{% include alert-warning.html content="You should never install a root certificate without verifying it." %} 

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

#### Option 2. Download COMMON Using Terminal

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

#### Option 3. Email Us

To request an out-of-band copy of the COMMON root CA certificate to download, email us at fpki@gsa.gov.

### macOS&nbsp;&mdash;&nbsp;Install COMMON Options

You can install COMMON in your macOS device's Trust Store by using one of these options: 

* [Option 1. Install COMMON Using an Apple Configuration Profile](#option-install-common-using-an-apple-configuration-profile)
* [Option 2. Install COMMON Using Command Line](#option-2-install-common-using-command-line)
* [Option 3. Install COMMON Using Apple Keychain](#option-3-install-common-using-apple-keychain)

#### Option 1. Install COMMON Using an Apple Configuration Profile

These procedures are intended for Enterprise Administrators. 

You can create Apple Configuration Profiles (XML files) to distribute trusted root certificates across an Enterprise's Apple devices. A [Ready-To-Use Apple Configuration Profile](#ready-to-use-configuration-profile-to-install-common) is also presented in this section for installing COMMON.

* [Create Configuration Profiles to Install COMMON](#create-configuration-profiles-to-install-common)
* [Ready-To-Use Configuration Profile To Install COMMON](#ready-to-use-configuration-profile-to-install-common)
* [Distribute Configuration Profiles To Install COMMON](#distribute-configuration-profiles-to-install-common)

##### Create Configuration Profiles to Install COMMON

One way to create a Configuration Profile for macOS, iOS, and tvOS is by using Apple's free Configurator 2 application:

1. Download and install Configurator 2 from the Apple App Store.
2. Open Configurator and click *File* -> *New Profile*.
3. Under *General*, enter a unique profile *Name*. ("Federal Common Policy Certification Authority Profile" was used for the below example.)
4. Enter a unique profile *Identifier*. ("FCPCA-0001" was used for the below example.)
5. Browse to local *certificate* copies on your device. Select those that you'd like to add to the profile. (For the below example, a copy of the Federal Common Policy CA root certificate was used.)
6. Click *File* -> *Save* to save your profile to a preferred file location.
7. Close Configurator 2.

##### Ready-To-Use Configuration Profile To Install COMMON

The example configuration profile below (created with Configurator 2) will install COMMON as a trusted root CA for _both macOS and iOS_. Before using the profile, you should verify its suitability for your agency. 

To repurpose the profile, save it with a `.mobileconfig` extension. 

For more information, see: [Apple Configuration Profile Reference](https://developer.apple.com/library/content/featuredarticles/iPhoneConfigurationProfileRef/Introduction/Introduction.html){:target="_blank"}

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>PayloadContent</key>
	<array>
		<dict>
			<key>PayloadCertificateFileName</key>
			<string>fcpca.crt</string>
			<key>PayloadContent</key>
			<data>
			MIIEYDCCA0igAwIBAgICATAwDQYJKoZIhvcNAQELBQAwWTELMAkG
			A1UEBhMCVVMxGDAWBgNVBAoTD1UuUy4gR292ZXJubWVudDENMAsG
			A1UECxMERlBLSTEhMB8GA1UEAxMYRmVkZXJhbCBDb21tb24gUG9s
			aWN5IENBMB4XDTEwMTIwMTE2NDUyN1oXDTMwMTIwMTE2NDUyN1ow
			WTELMAkGA1UEBhMCVVMxGDAWBgNVBAoTD1UuUy4gR292ZXJubWVu
			dDENMAsGA1UECxMERlBLSTEhMB8GA1UEAxMYRmVkZXJhbCBDb21t
			b24gUG9saWN5IENBMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIB
			CgKCAQEA2HX7NRY0WkG/Wq9cMAQUHK14RLXqJup1YcfNNnn4fNi9
			KVFmWSHjeavUeL6wLbCh1bI1FiPQzB6+Duir3MPJ1hLXp3JoGDG4
			FyKyPn66CG3G/dFYLGmgA/Aqo/Y/ISU937cyxY4nsyOl4FKzXZbp
			sLjFxZ+7xaBugkC7xScFNknWJidpDDSPzyd6KgqjQV+NHQOGgxXg
			VcHFmCye7Bpy3EjBPvmE0oSCwRvDdDa3ucc2Mnr4MrbQNq4iGDGM
			UHMhnv6DOzCIJOPpwX7e7ZjHH5IQip9bYi+dpLzVhW86/clTpyBL
			qtsgqyFOHQ1O5piF5asRR12dP8QjwOMUBm7+nQIDAQABo4IBMDCC
			ASwwDwYDVR0TAQH/BAUwAwEB/zCB6QYIKwYBBQUHAQsEgdwwgdkw
			PwYIKwYBBQUHMAWGM2h0dHA6Ly9odHRwLmZwa2kuZ292L2ZjcGNh
			L2NhQ2VydHNJc3N1ZWRCeWZjcGNhLnA3YzCBlQYIKwYBBQUHMAWG
			gYhsZGFwOi8vbGRhcC5mcGtpLmdvdi9jbj1GZWRlcmFsJTIwQ29t
			bW9uJTIwUG9saWN5JTIwQ0Esb3U9RlBLSSxvPVUuUy4lMjBHb3Zl
			cm5tZW50LGM9VVM/Y0FDZXJ0aWZpY2F0ZTtiaW5hcnksY3Jvc3ND
			ZXJ0aWZpY2F0ZVBhaXI7YmluYXJ5MA4GA1UdDwEB/wQEAwIBBjAd
			BgNVHQ4EFgQUrQx6dVzl85jEeZgOrCj9l/TnAvwwDQYJKoZIhvcN
			AQELBQADggEBAI9z2uF/gLGH9uwsz9GEYx728Yi3mvIRte9UrYpu
			GDco71wb5O9Qt2wmGCMiTR0mRyDpCZzicGJxqxHPkYnos/UqoEfA
			FMtOQsHdDA4b8Idb7OV316rgVNdF9IU+7LQd3nyKf1tNnJaK0KIy
			n9psMQz4pO9+c+iR3Ah6cFqgr2KBWfgAdKLI3VTKQVZHvenAT+0g
			3eOlCd+uKML80cgX2BLHb94u6b2akfI8WpQukSKAiaGMWMyDeiYZ
			dQKlDn0KJnNR6obLB6jI/WNaNZvSr79PMUjBhHDbNXuaGQ/lj/Rq
			DG8z2esccKIN47lQA2EC/0rskqTcLe4qNJMHtyznGI8=
			</data>
			<key>PayloadDescription</key>
			<string>Adds a CA root certificate</string>
			<key>PayloadDisplayName</key>
			<string>Federal Common Policy CA</string>
			<key>PayloadIdentifier</key>
			<string>com.apple.security.root.1EB75E7D-C3BC-46C2-AF42-51D80A2E12FC</string>
			<key>PayloadType</key>
			<string>com.apple.security.root</string>
			<key>PayloadUUID</key>
			<string>1EB75E7D-C3BC-46C2-AF42-51D80A2E12FC</string>
			<key>PayloadVersion</key>
			<integer>1</integer>
		</dict>
	</array>
	<key>PayloadDisplayName</key>
	<string>Federal Common Policy Certification Authority Profile</string>
	<key>PayloadIdentifier</key>
	<string>FCPCA-0001</string>
	<key>PayloadRemovalDisallowed</key>
	<false/>
	<key>PayloadType</key>
	<string>Configuration</string>
	<key>PayloadUUID</key>
	<string>AAD17D9A-DA41-4197-9F0F-3C3C6B4512F9</string>
	<key>PayloadVersion</key>
	<integer>1</integer>
</dict>
</plist>
``` 
##### Distribute Apple Configuration Profiles To Install COMMON

Use any of these options to distribute your configuration profiles: 

1. Via Apple's free _Configurator 2_ application with devices connected via USB 
2. In an email message 
3. On a webpage 
4. [Over-the-air profile delivery and configuration](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/iPhoneOTAConfiguration/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009505){:target="_blank"}
5. [Over-the-air using a Mobile Device Management server](https://developer.apple.com/library/archive/documentation/Miscellaneous/Reference/MobileDeviceManagementProtocolRef/6-MDM_Best_Practices/MDM_Best_Practices.html#//apple_ref/doc/uid/TP40017387-CH5-SW2){:target="_blank"} 

#### Option 2. Install COMMON Using Command Line
1. Install the COMMON root CA certificate as a Trusted Root:

    ```
	$ sudo security add-trusted-cert -d -r trustRoot -k "/Library/Keychains/System.keychain" {DOWNLOAD_LOCATION}/fcpca.crt
    ```

#### Option 3. Install COMMON Using Apple Keychain
1. Browse to your downloaded copy of the COMMON root CA certificate.
2. Double-click on the file.
3. When prompted, enter your password to install the certificate.

Non-administrative users may follow the steps above to install COMMON in the Login Keychain, which is specific to their accounts. This will not impact other user accounts on a device. 

### iOS&nbsp;&mdash;&nbsp;Install COMMON Options

You can install COMMON in your iOS device's Trust Store by using one of these options:

* [Option 1. Install COMMON Using Safari Web Browser](#option-1-install-common-using-safari-web-browser)
* [Option 2. Install COMMON Using Apple Configuration Profile](#option-2-install-common-using-apple-configuration-profile)

#### Option 1. Install COMMON Using Safari Web Browser
1. Open Safari.
2. Navigate to the [COMMON root CA certificate](http://http.fpki.gov/fcpca/fcpca.crt){:target="_blank"}<br>
> System message appears: *The website is trying to open Settings to show you a configuration profile. Do you want to allow this?*<br>
3. Click *Allow*.<br>
> The COMMON Configuration Profile appears.<br> 
4. Click *More Details* and then the COMMON certificate entry.
5. Scroll down to *Fingerprints* and verify the certificate's SHA-256 hash.
6. At top left of screen, click *Back* and *Install Profile*. Then, click *Install* (top right).
7. When prompted, enter your device passcode.
8. Click *Install* (top right), and *Install* again.
9. Click *Done*.

Next, you'll need to enable full trust for COMMON:

1. From the iOS device's *Home* screen, go to *Settings* -> *General* -> *About* -> *Certificate Trust Settings*.
2. Beneath *Enable Full Trust for Root Certificates*, toggle _ON_ for the Federal Common Policy CA entry. 
3. When certificate appears, click *Continue*.
4. You can now successfully navigate to an intranet site whose TLS certificate was issued by a Federal PKI CA.


#### Option 2. Install COMMON Using Apple Configuration Profile
You can use the macOS configuration profile procedures above to install COMMON on iOS devices: [Apple Configuration Profiles](#install-using-an-apple-configuration-profile).


### tvOS&nbsp;&mdash;&nbsp;Install COMMON Options
If you would like to install COMMON on tvOS government-furnished equipment, email us at fpki@gsa.gov.

## Frequently Asked Questions

1) Now is the time
2) And now is the time
3) Now is always the time

### 1.&nbsp;&nbsp;Is PIV network login impacted?
Yes. 

### 2.&nbsp;&nbsp;What versions are affected?
Please see [What Will Be Impacted?](#what-will-be-impacted). 

## Additional Resources
1. [COMMON Removal from Microsoft Certificate Trust List)](https://fpki.idmanagement.gov/announcements/mspkichanges/){:target="_blank"}.
1. [macOS Available Trusted Root Certificates List](https://support.apple.com/en-us/HT202858){:target="_blank"}
1. [iOS Available Trusted Root Certificates List](https://support.apple.com/en-us/HT204132){:target="_blank"}
1. [tvOS Available Trusted Root Certificates](https://support.apple.com/en-us/HT207231){:target="_blank"}
1. [Apple Keychains](https://developer.apple.com/documentation/security/keychain_services){:target="_blank"}
1. [Apple Configuration Profile Reference](https://developer.apple.com/library/content/featuredarticles/iPhoneConfigurationProfileRef/Introduction/Introduction.html){:target="_blank"}
1. [Over-the-Air Profile Delivery and Configuration](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/iPhoneOTAConfiguration/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009505){:target="_blank"}
1. [Mobile Device Management Best Practices](https://developer.apple.com/library/archive/documentation/Miscellaneous/Reference/MobileDeviceManagementProtocolRef/6-MDM_Best_Practices/MDM_Best_Practices.html#//apple_ref/doc/uid/TP40017387-CH5-SW2){:target="_blank"}
