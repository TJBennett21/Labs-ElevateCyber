During testing, I identified multiple accounts configured with the Kerberos DONT_REQUIRE_PREAUTH flag enabled, making them vulnerable to AS-REP roasting. Using Impacket from a Kali Linux system, I queried 
the domain controller and retrieved AS-REP responses containing password-derived encrypted blobs for three user accounts. I extracted these hashes and successfully cracked them offline with John the 
Ripper, recovering plaintext credentials. 
This demonstrates a high-impact risk, as the compromised accounts could be leveraged for unauthorized access, lateral movement, privilege escalation, or persistent domain compromise.

FIGURE 1:

<img width="717" height="75" alt="AD ASREP 1" src="https://github.com/user-attachments/assets/787d87d7-222f-4fa3-8ed6-034b97fec3f3" />

Running the command seen in Figure 1 showed me all of the users in my list collected in the RID Bruteforce section of testing that do not have ‘DONT_REQUIRE_PREAUTH” set.

FIGURE 2:

<img width="1418" height="415" alt="AD ASREP 2" src="https://github.com/user-attachments/assets/1c734296-9eda-48f8-a8ba-3c6db7e92991" />

The output shows me 3 different users who don’t have ‘DONT_REQUIRE_PREAUTH” set and their hashed passwords.

FIGURE 3:

<img width="1411" height="192" alt="AD ASREP 3" src="https://github.com/user-attachments/assets/e19fdf50-3f37-41e0-97d9-c366bc7e31df" />

I copied the full hash of each of the users.

FIGURE 4:

<img width="239" height="63" alt="AD ASREP 4" src="https://github.com/user-attachments/assets/d5c38d0e-fe3c-4670-b987-e17b82c803ab" />

Then pasted the previously copied hashes into a separate text folder (I named mine “hashes.txt.” for simplicity).

FIGURE 5:

<img width="737" height="582" alt="AD ASREP 5" src="https://github.com/user-attachments/assets/db074c4c-3a25-414c-8828-35eb12845d00" />

Finally, the command shown in Figure 5 will decode the hashes located in my created text file and shows me the output (passwords) for each of the users above.

Remediation tips would be as follows:

1. Re‑enable Kerberos pre‑authentication for all accounts

-  Find accounts with pre‑auth disabled and clear the DONT_REQUIRE_PREAUTH flag.

2. Remove or replace legacy accounts that require pre‑auth disabled

-  Identify service/legacy accounts that had pre‑auth turned off and migrate them to supported solutions (gMSA, virtual accounts, or application-specific managed identities).

-  If an app truly needs special handling, isolate that account, document the exception, and put compensating controls around it.

3. Enforce long, unique passwords and rotate immediately if exposed

-  Require strong, high‑entropy passwords (or managed account passwords) for all users, and rotate passwords for any accounts found with pre‑auth disabled.

-  Treat exposed accounts as incidents: change passwords, investigate usage, and consider credential reuse across systems.

4. Require MFA / stronger authentication for high‑risk and remote access

-  Apply multi‑factor authentication for privileged users and for remote access paths (VPN, RDP, cloud consoles). MFA prevents simple replay/abuse even if a password is recovered.

-  Prioritize accounts that can access sensitive systems or hold elevated privileges.

5. Monitor and alert on AS‑REP/AS‑REQ enumeration and brute attempts

-  Create SIEM alerts for suspicious Kerberos activity (unusual volumes of AS‑REQ/AS‑REP, many requests for accounts that typically don’t authenticate, or queries consistent with GetNPUsers.py).

-  Deploy canary accounts with pre‑auth disabled only to detect enumeration, and log/alert when they are probed.
