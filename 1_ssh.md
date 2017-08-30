---
layout: default
title: PIV Authentication for SSH to UNIX-like Servers
permalink: /userconfig/ssh
collection: userconfig
---

You often need to remotely access UNIX-like servers on your network by using SSH. Authenticating with your PIV is the most secure way to do this. Your PIV chip offers strong security features (e.g., tamper-resistance, protected private keys, encryption, biometrics, etc.) that are difficult to exploit. Attackers are much more likely to target software methods of authentication (e.g., username and password login).

**Note:**  Other authentication methods are available, such as Linux' solution that ties Pluggable Authentication Modules (PAM) to directories.

{% include alert-info.html heading = "Your PIV contains an authentication key pair and public certificate. Using a PIV key pair and public certificate is exactly like using a key pair and self-signed certificate for SSH remote access." %}

**CELESTE:  The setup for card reader, etc., here is only for Windows--Linux and macOs procedures don't include card reader set up** 

**CELESTE:  Validate these**  For Windows, Linux, and macOS, this guide will help you to:

1. Ensure that your OS recognizes and authenticates your PIV. <!--Is the order correct here? Does this encapsulate all of the procedures/total purpose within each OS section?-->
2. Configure a UNIX-like server.
3. SSH to a UNIX-like server.

Click the link for your OS-specific steps. Please also review _Configure a UNIX-like Server for Remote Access_.

**CELESTE -- Change "Use" to "PIV Authentication for SSH from OS," etc.???  Really first about Getting OS to Recognize your card reader and PIV--- then using PIV for authentication to SSH to server**

* [Authenticate PIV for SSH from Windows](#authenticate-piv-for-ssh-from-windows) 
* [Authenticate PIV for SSH from Linux](#authenticate-piv-for-ssh-from-linux)
* [Authenticate PIV for SSH from macOS](#authenticate-piv-for-ssh-from-macOS)
* [Configure a UNIX-like Server for Remote Access](#configure-a-unix-like-server-for-remote-access)

## Authenticate PIV for SSH from Windows

* [Hardware and software requirements](#hardware-and-software-requirements)
* [Install PuTTY-CAC](#install-putty-cac)
* [Use PIV to insert Microsoft CAPI key into Pageant](#use-piv-to-insert-microsoft-capi-key-into-pageant)
* [Configure PuTTY](#configure-putty)
* [Verify PuTTY login and proceed with SSH](#verify-putty-login-and-proceed-with-ssh)

### Hardware and software requirements

* A PIV
* Windows computer
* A card reader
* PuTTY-CAC application (an open-source SSH client that supports PIV authentication)
* Pageant application (an SSH authentication agent used with PuTTY-CAC)

### Install PuTTY-CAC

1. Download and install [**PuTTY-CAC**](https://www.github.com/NoMoreFood/putty-cac/releases){:target="_blank"}_.
  
> _PuTTY will normally be installed at **C:\Program Files\PuTTY**._

### Use PIV to insert Microsoft CAPI key into Pageant 

1. Insert your **PIV** into the card reader, and go to:  **C: &gt; Program Files &gt; PuTTY &gt; Pageant**.
2. Click the **Pageant** icon from the Windows taskbar, and select **View Keys &amp; Certs**.

     > _The Pageant **Key/CAPI Cert List** window will open._

4. Click **Add Cert**.
5. From the **Windows Security** window, select your **Smart Card Logon** certificate.
6. To ensure that this is the right certificate, click **Click here to view certificate properties &gt; Details**.
7. Click **Enhanced Key Usage**. 
8. At the **Smart Card Logon** window, click on the **Smart Card certificate** (i.e., **CAPI key**). Then click **OK** and **Close**. 

> _The Pageant window will populate with the certificate information. (Clear any expired or revoked certificates.) Whenever you start Pageant, you will need to re-add the certificate._

### Configure PuTTY

#### Set up a PIV login profile

1. From the Windows taskbar, click the **Pageant** icon and select **New Session** to launch **PuTTY**.
2. From **Category**, select **Connection &gt; SSH &gt; CAPI**. 
3. Click the checkbox for **Attempt "CAPI Certificate" (Key-only) auth (SSH-2)**.
4. From the **PuTTY Configuration** window, select **Connection &gt; SSH &gt; Auth**. Next, click the checkboxes for **Allow agent forwarding** and **Allow attempted changes of username in SSH-2** and save your session.
5. To create new profiles for additional UNIX-like servers, repeat these steps.

#### Obtain PIV SSH key  ******CELESTE STOPPED 8/29/2017***

1. Go to the **PuTTY Configuration** window. Click on **Connection &gt; SSH&nbsp;&gt; CAPI**. 
2. Under **Authentication Parameters**, click the **Browse** button.  

> _This automatically fills in the **Cert** and **SSH keystring** textboxes._

2. Copy and paste the **SSH keystring**&nbsp;**_value_** (i.e., SSH key) into **Microsoft Notepad** and save it.  
3. Provide your SSH key to the SSH server administrator and ask that it be added to your SSH server account. <!--Is the "SSH server" the same thing as the remote UNIX-like server? Unclear.-->

> _Once the SSH server account has been set up with your SSH key, you can log in with your PIV. 
> For additional SSH servers, submit a service ticket to the administrator with the server's IP address, your account name, and your PIV's SSH key._ <!--Doesn't read like the same process as Step 3, but is this the same process?-->

### Verify PuTTY login and proceed with SSH

1. Run **PuTTY** and select your **Saved Session**. Click **Load** and then **Open**.
2. Enter your **remote UNIX/Linux account name**.  
3. At the prompt, enter your **PIV card PIN** and click **OK** to log into the remote server.
4. Once logged in, run the command: **ssh-add –l** to display the SSH key.  

> _Use **ssh-add –l** for each server to display the SSH key. You may then **ssh** to any other host in the environment._

## Authenticate PIV for SSH from Linux

* [Hardware and software requirements](#hardware-and-software-requirements)
* [Obtain and save public key from PIV](#obtain-and-save-public-key-from-PIV)
* [SSH to log into UNIX-like server](#SSH-to-log-into-unix-like-server)

### Hardware and software requirements

  * A PIV
  * A card reader
  * A Linux computer configured for PIV login (For one method of doing this with a Linux OS, go to [**OpenSC**](https://www.github.com/OpenSC/OpenSC/wiki/Download-latest-OpenSC-stable-release){:target="_blank"}_.)

### Obtain and save public key from PIV

  1. Insert your **PIV** into your computer's card reader.
  2. To save your **public SSH key** to a file, enter:

        ```
			ssh-keygen -D /usr/lib64/opensc-pkcs11.so > mykey.pub
        ```  
  3. Submit the file with your public SSH key to the SSH server administrator.

### SSH to log into UNIX-like server

  1. Insert your **PIV** into your card reader.
  2. To log into the remote server, enter:

        ```
			ssh -I /usr/lib64/opensc-pkcs11.so <remote-host>
        ```    

  3. At the PIV card password prompt, enter your **PIN**.

     > _The **remote-host shell prompt** appears._

{% include alert-warning.html heading = "The card reader may flash. **Do not** remove the PIV until the login process has been completed." %} 

## Authenticate PIV for SSH from macOS (10.12 Sierra)

* [Hardware and software requirements](#hardware-and-software-requirements)
* [Obtain and save public key from PIV](#obtain-and-save-public-key-from-PIV)
* [SSH to log into UNIX-like server](#SSH-to-log-into-unix-like-server)

### Hardware and software requirements

  * A PIV
  * A card reader
  * A Mac (macOS 10.12 Sierra) computer configured for PIV login. (For one method of doing this with a macOS, go to [**OpenSC**](https://www.github.com/OpenSC/OpenSC/wiki/Download-latest-OpenSC-stable-release){:target="_blank"}_.)

### Obtain and save public key from PIV

  1. Insert your **PIV** into your computer's card reader.
  2. Use the following command to save the **user's public SSH key** to a file and submit it to SSH server administrator.

        ```
			ssh-keygen -D /Library/OpenSC/lib/pkcs11/opensc-pkcs11.so > mykey.pub
        ```

### SSH to log into UNIX-like server

  1. Insert your **PIV** into your computer's card reader.
  2. Use the following command to log into the remote server:

        ```
		 	ssh -I /Library/OpenSC/lib/pkcs11/opensc-pkcs11.so <remote-host>
        ```

  3. At the PIV password prompt, enter your **PIN**.

     > _The **remote-host shell prompt** appears._

{% include alert-warning.html heading = "The card reader may flash. **Do not** remove the PIV until the login process has been completed." %}

## Configure a UNIX-like Server for Remote Access

These steps will help you to configure a UNIX-like server for remote access. 

  1. Change the configuration in the **/etc/ssh/sshd_config** file and restart the **sshd**:

      ```
		AuthorizedKeysFile /etc/sshd/authorized_keys/%u
		PasswordAuthentication No
      ```

  2. Create the directory, **/etc/sshd/authorized_keys**:

        ```
			mkdir /etc/sshd/authorized_keys
        ```

  3. To allow one user to have access, place the user's PIV's SSH public key in this directory, according to the username: **/etc/sshd/authorized_keys/[login ID]**. 
  
       >_Only a **root user** may modify this directory and its files. This ensures that access requirements are enforced._  
  
  4. Disable any alternative means of access (i.e., passwords), as needed.
