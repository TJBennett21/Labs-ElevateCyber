During this assessment, I identified a critical weakness in Kerberos service account management that allows an attacker to compromise a domain service account through Kerberoasting. By leveraging publicly available tools from Impacket to enumerate SPNs and request service tickets, an attacker can extract a ticket hash for the target account and crack it offline using Hashcat. This process revealed the plaintext password for the service account, resulting in full credential compromise. Once obtained, these credentials could be used for lateral movement, service impersonation, privilege escalation, and persistent access within the domain.

Using tools from Impacket, I executed the appropriate Kerberoasting command with previously obtained valid domain credentials (acquired during another attack phase). Because any authenticated domain user can request service tickets, this allowed me to enumerate Service Principal Names (SPNs) and identify accounts vulnerable to Kerberoasting.

FIGURE 1:

<img width="995" height="71" alt="AD Kerberoasting 1" src="https://github.com/user-attachments/assets/72244470-ad1c-4dd9-a8e9-d1c94138ce34" />

The enumeration process revealed a kerberoastable service account.

FIGURE 2:

<img width="570" height="294" alt="AD Kerberoasting 2" src="https://github.com/user-attachments/assets/ae806552-ea50-4630-8961-cd16c155c171" />

I copied the full Kerberos service ticket hash associated with the service account for offline cracking.

FIGURE 3:

<img width="1417" height="399" alt="AD Kerberoasting 3" src="https://github.com/user-attachments/assets/2fee943c-8305-44d6-9458-6e8323acb6ab" />

I then navigated to my Hashcat directory on the attack system.

FIGURE 4:

<img width="240" height="48" alt="AD Kerberoasting 4" src="https://github.com/user-attachments/assets/a3f4b599-214f-4e07-a133-c3960744c69d" />

From within the Hashcat folder, I launched a Git Bash session to prepare for offline password cracking.

FIGURE 5:

<img width="300" height="268" alt="AD Kerberoasting 5" src="https://github.com/user-attachments/assets/230fcac5-4953-4851-9b81-364c0ab41900" />

Pasting the captured hash into a text file using a standard text editor, I saved it for processing.

FIGURE 6:

<img width="1145" height="229" alt="AD Kerberoasting 6" src="https://github.com/user-attachments/assets/b88718b6-61f1-4ec1-a6a2-d43c343c8fe4" />

I executed the Hashcat cracking command against the saved hash file. This initiated an offline password-cracking attempt, with results configured to output to a file named "cracked.txt".

FIGURE 7:

<img width="698" height="60" alt="AD Kerberoasting 7" src="https://github.com/user-attachments/assets/2bb37b07-84c9-43e3-bc0b-c39995f047dc" />

The command completed successfully and indicated that the hash had been cracked.

FIGURE 8:

<img width="786" height="385" alt="AD Kerberoasting 8" src="https://github.com/user-attachments/assets/bc7dc1f4-5c11-4c74-a034-9346b8bccbef" />

Upon reviewing the cracked.txt file, I confirmed that the plaintext password for the service account had been recovered, resulting in full credential compromise of the account.

FIGURE 9:

<img width="1146" height="259" alt="AD Kerberoasting 9" src="https://github.com/user-attachments/assets/b8265cba-c57c-441d-b59a-11d9cf643a4b" />


Remediation tips would be as follows:

1. Implement strong password policies for all service accounts

  -Use long (20+ characters), random passwords to prevent offline cracking.

  -Immediately reset the password for the service account and other SPN-linked accounts.


2. Remove unused or unnecessary SPNs

  -List all accounts with SPNs and clean up any that aren’t actively used.

  -Limit who can register or modify SPNs in Active Directory.


3. Enforce modern Kerberos encryption (AES) and disable RC4

  -Switch service accounts to support only AES128/AES256.

  -Disable older RC4 encryption after testing for compatibility.


4. Monitor for suspicious Kerberos ticket activity

  -Alert on high volumes of TGS requests (Event ID 4769) or from unusual sources.

  -Set canary SPNs to detect Kerberoasting attempts early.
