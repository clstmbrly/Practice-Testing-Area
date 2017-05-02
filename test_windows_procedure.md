
## How do I install, set up, and log into Putty-CAC?

The majority of Unix-like systems are configured to use the **Secure Shell (SSH) protocol** for remote access; however, most SSH client applications do not support PIV, as required by Federal Government policy <!-- Which policy? -->. **PuTTY-CAC**, a GitHub fork of the Open Source PuTTY SSH client, resolves this issue.

> **Note:** Van Dyke Software's SecureCRT® (a commercial, remote-access client) also supports **an PIV SSH login capability** for Windows, Mac, and Linux. <!-- Modified this based on review of product on company website. Doesn't say PIV SSH on website.-->

### Required environment: 

In order for these steps to work, you'll need the following equipment and applications <!-- Is this wording correct? -->:

  * **Domain Controller 1 (DC1) Jumpbox** (also referred to as a jump server, jump host, or secure administrative host)
  * **DC2 Jumpbox**
  * **Amazon Web Services (AWS) Jumpbox**
  * **PuTTY-CAC** - PuTTY is a terminal emulator and authentication agent that can use several protocols and can transfer files. [1 - From www.file.net].  PuTTY-CAC is an SSH open-source client for Windows that supports PIV authentication, particularly using the U.S. Department of Defense (DoD) Common Access Card (CAC). [1 - mostly quoted from Risacher.org] <!-- So PIV works the same as CAC? -->  
  * **Pageant** - Pageant is a secure method for connecting to Unix or Linux machines by using PuTTY. [2 - sentences mostly quoted from www.file.net] <!-- How is Pageant used for Windows-based systems? -->




Installing PuTTY-CAC
1.	Launch Software Center from the Start menu (Start > All Programs > Microsoft System Center > Software Center). 
2.	Click the “Find additional applications from the Application Catalog” link. From the internet page that opens, search PuTTY-CAC and click Install.
Note: You cannot use Internet Explorer 11 to access the Application Catalog. You must use IE 9, Firefox, or Chrome.
3.	Verify the version of PuTTY that was installed by opening the application and clicking About in the lower left corner.

4.	Launch pageant from the PuTTY install directory which typically will be C:\Program Files\Putty-CAC. Pageant will appear in the taskbar on the bottom right of your desktop;it will not open a window. 
5.	You must now insert the CAPI Key and configure PuTTY-CAC. Follow the steps below.

Insert CAPI Key into Pageant
1.	Open your Windows Explorer or click Start > Computer.
2.	Open the pageant by clicking C: > Program Files > PuTTY-CAC > Pageant.

3.	A window will not open but on menu bar, the Pageant icon will appear. Right-click the icon and select View Keys.
 
4.	The Pageant Key List window will appear. Click Add CAPI Cert.

5.	Select your Smart Card Logon certificate from the Windows Security window.

To ensure you are selecting the correct certificate, click the link “Click here to view certificate properties,” click “Details,” scroll half-way, and locate Enhanced Key Usage. It should begin with “Smart Card Logon;” this indicates it is the correct certificate

6.	Note: If multiple certificates exist, you may want to clear out the expired or revoked certificates by following How To – PIV Card – Clear certificate store. Click OK to close the details window.
7.	With the correct Smart Card certificate highlighted, click OK.
8.	The Pageant Window will populate with the certificate information.
9.	Click Close.
10.	You must now configure PuTTY-CAC. Follow the steps below.


Note: The certificate needs to be re-added every time pageant is started.

Configure PuTTY-CAC
1.	Right-click the Pageant icon again from the menu bar and select New Session. This will launch PuTTY.

2.	From within PuTTY, enter the IP address of your jumpbox in Host Name (or IP address) textbox to setup a new profile, or if you already have profiles setup, load that profile. 

Note: If you have multiple jumpbox profiles, you will have to do the following steps for each profile

3.	Enter a descriptive name under Saved Sessions textbox (if setting up a new profile).

4.	On left panel, select Connection > SSH > CAPI thencheck the box beside the words Attempt “CAPI Certificate” (Key-only) auth (SSH-2).



5.	From within PuTTY, select Connection > SSH > Auth then select both “Allow agent forwarding” and “Allow attempted changes of username in SSH-2.”



6.	Click Session, then Save. This has setup this profile for PIV logon.



7.	To get your PIV card’s SSH key, in PuTTY, go to Connection > SSH > CAPI  and select the browse button on the right side. This will automatically fill in the “Cert” and “SSH keystring” fields. 
8.	Copy and paste the SSH keystring value from PuTTY into Notepad as you will need to include the SSH key when you contact the jumpbox support team or create a service ticket.


9.	For DC1 jumpboxes, submit an email to the SDCEMOC at sdcemoc@hq.dhs.gov and request that they add your PIV card’s SSH key to your account on the jumpbox and create a configuration file (as described below) for SSH key forwarding to other systems beyond the initial jumpbox. Include the IP address of the jumpbox you are using, your account name, and the SSH key derived from your PIV card. 

For DC2 jumpboxes, send a support request email to dc2servicedesk@hq.dhs.gov and request that they add your PIV card’s SSH key to your account on the jumpbox and create a configuration file (as described below) for SSH key forwarding to other systems beyond the initial jumpbox. Include the IP address of the jumpbox you are using, your account name, and the SSH key derived from your PIV card.
For other jumpboxes, submit a service ticket to that support group and include the IP address of the jumpbox you are using, your account name, and the SSH key derived from your PIV card.

The configuration file should contain “Host *” and “ForwardAgent yes” and exist in the same folder where they place the SSH key.

10.	In Saved Sessions, click Save to save your configuration.

11.	Wait for the SDCEMOC, DC2 Support, or other support group to get your account setup with your SSH key on the jumpbox, then continue. Once your account has been set up, you can then try to login via PIV card.

12.	Open.



13.	Enter your remote Unix/Linux account name, and you should be prompted for your PIV PIN.



14.	Enter your PIN, click OK and you should be logged in.
15.	Once logged in, run ‘ssh-add –l’ to ensure that the forwarding agent is working. If you do not see the key printed when you run this command, something is wrong and you will not be prompted for your PIN if you ssh further into the environment.



16.	Both the cert key that was pasted into the .ssh/authorized_keys and the config file need to be copied or scp’d to all the servers you will connect to in the data center. If the forwarding agent is working when you ssh to a server beyond the jumphost, you should be prompted for the PIN again.
17.	After each server you ‘jump’ to, the output of ssh-add –l should always show the key. If not, either permissions are wrong or a file is mislabeled, or missing.
18.	For additional help or troubleshooting, please contact the team in charge of the jumpbox you are trying to connect to or the Service Desk.


