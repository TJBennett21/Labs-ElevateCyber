In this assessment, I performed a password spraying attack against the Active Directory environment using Kerbrute and a collected list of domain usernames. I attempted a commonly used password across multiple accounts 
to avoid triggering account lockouts while identifying weak or reused credentials. During this process, I successfully identified three accounts that authenticated using the sprayed password, demonstrating the 
presence of weak credential practices within the environment. With these valid credentials, I was able to obtain authenticated access as legitimate users, which could enable further lateral movement, privilege escalation, 
or unauthorized access to sensitive data.

I used a publicly available Github repository for this vulnerability called “kerbrute”

FIGURE 1:

<img width="1417" height="832" alt="AD password spray 1" src="https://github.com/user-attachments/assets/c387c485-194c-44c2-9604-4f0b34ac7022" />


Once downloaded, I moved that file into my /opt directory (Figure 2) and confirmed it was successfully moved (Figure 3).

FIGURE 2:

<img width="612" height="70" alt="AD password spray 2" src="https://github.com/user-attachments/assets/2459cf74-1f51-41d3-be2e-c77aa5e32c22" />

FIGURE 3:

<img width="446" height="100" alt="AD password spray 3" src="https://github.com/user-attachments/assets/aca9bcc5-ca00-4636-b91e-1e4894f8fa5f" />

To make things easier for typing, I renamed the file to "kerbrute" (Figure 4) and confirmed the name change (Figure 5). 

FIGURE 4:

<img width="568" height="89" alt="AD password spray 4" src="https://github.com/user-attachments/assets/028506ba-3439-41d0-aa83-369317fd451b" />

FIGURE 5:

<img width="333" height="117" alt="AD password spray 5" src="https://github.com/user-attachments/assets/d9cfc741-8947-4479-9bf5-ffe0498f9840" />

Using the command shown (Figure 6), I added execution ability to the file so that I may utilize its contents.

FIGURE 6:

<img width="328" height="92" alt="AD password spray 6" src="https://github.com/user-attachments/assets/9c8548e2-21a9-4843-87f4-8e42fec47101" />

The command shown (Figure 7) conducted a password spray against all users on the system contained in my user list collected in the RID Bruteforce section (users.txt) of the assessment using the 
password “***********!” (blocking for privacy) which is the password I wanted to spray 

FIGURE 7:

<img width="1320" height="104" alt="AD password spray 8" src="https://github.com/user-attachments/assets/fc2c6c9b-05fa-46b2-ba31-9a91212ff18f" />

Running the previous command will showed me 3 successful spray attempts (Figure 8). I now have valid login credentials for three user accounts

FIGURE 8:

<img width="741" height="217" alt="AD password spray 9" src="https://github.com/user-attachments/assets/83233556-0e36-4d3b-9ff3-f53663122e9b" />

Remediation tips would be as follows:

1. Enforce strong password policies and block common passwords

-  Require long, complex passwords and block known-bad/common passwords (password denylists).

-  Use adaptive password policy or banned-password lists (e.g., Microsoft banned password API / custom list).

2. Require multi‑factor authentication (MFA) for all interactive and privileged logins

-  Make MFA mandatory for remote access, VPN, RDP, Office365, and high‑risk accounts.

-  MFA stops attackers even if a sprayed password succeeds.

3. Monitor and alert on authentication anomalies

-  Alert on many failed auths across many accounts from the same IP or source, or many successful logins of low‑privilege accounts in a short window.

-  Log and surface failed login patterns to SIEM (IAM logs, domain controllers, VPNs, apps).

4. Harden accounts and reduce attack surface

-  Disable or remove stale/unused accounts; require unique service account passwords; do not reuse creds.

-  Limit exposure of authentication endpoints to internal networks or VPNs and require conditional access (network/location based).
