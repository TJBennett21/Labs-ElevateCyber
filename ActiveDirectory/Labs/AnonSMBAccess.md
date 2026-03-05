During this assessment, I tested the target system for SMB access and discovered that guest authentication was permitted, allowing access to the IPC$ share. Using tools from the Impacket project, I leveraged this misconfiguration 
to perform an RID brute-force attack to enumerate domain user accounts through anonymous RPC/SMB calls. This process allowed me to identify multiple valid user SIDs and derive their corresponding usernames. 
I then compiled the discovered accounts into a users.txt wordlist, which could be used to support additional credential-based attacks such as password spraying.

Starting off, I ran the command shown (Figure 1) to test for any SMB access. Doing this showed an “ACCESS_DENIED” response.

FIGURE 1:

<img width="1485" height="153" alt="AD RID Bruteforce 1" src="https://github.com/user-attachments/assets/47f898d7-b6a8-41eb-816d-bf828d223ee9" />

Then, I tested for “guest” account access by running a similar command (Figure 2). After inputting this command I noticed that guest SMB account access is available.

FIGURE 2:

<img width="1486" height="136" alt="AD RID Bruteforce 2" src="https://github.com/user-attachments/assets/e508700a-7cef-4b11-8ead-285385b612f7" />

I then attempted to see what shares were available for the guest account. Nothing too special as these are default settings/permissions, however there was a read permission to the IPC$ 
share which means I could conduct an RID Bruteforce attack.

FIGURE 3:

<img width="1415" height="331" alt="AD RID Bruteforce 3" src="https://github.com/user-attachments/assets/61d4c783-6073-4332-a4b2-7fbfe74f2df1" />

Adding on “--rid-brute” onto the end of my command allowed me to enumerate multiple users, successfully completing the attack. From here, I made a wordlist (users.txt) of all the users found 
to be used in other potential exploits

FIGURE 4:

<img width="1427" height="387" alt="AD RID Bruteforce 4" src="https://github.com/user-attachments/assets/34618d31-a63f-4b5a-b579-cd373c64a5e8" />

Remediation tips would be as follows:

1. Disable anonymous/guest SMB access

-  Turn off the Guest account and remove anonymous access to shares (Local Users and Groups / GPO).

2. Restrict IPC$ and named pipe access via ACLs

-  Remove or tighten permissions on IPC$ and sensitive named pipes so only required privileged service/computer accounts can access them.

3. Harden SMB and legacy protocols

-  Disable SMBv1 and require SMB signing where possible; block weak protocols that allow legacy anonymous behavior.

4. Network segmentation and firewalling of SMB endpoints

-  Block SMB (TCP 445/139) from untrusted networks (internet, guest VLANs) and restrict to required hosts/subnets only.

-  Use internal firewall rules or ACLs to limit which systems can talk to domain controllers and file servers.

5. Detect and alert on enumeration activity

-  Alert on many anonymous/guest SMB/RPC queries, repeated Null/Anon binds, or high-volume RPC lookups from a single source — log to SIEM and investigate.

-  Periodically scan your domain for anonymous‑accessible SMB shares and document any exceptions as part of change control.
