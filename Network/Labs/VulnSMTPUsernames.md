During testing, I successfully enumerated multiple users on the network. This was achieved by using a publicly available enumeration module on Metasploit. 
Only usernames were exposed and not full credentials. Data was not able to be altered and no systems were crashed or resulted in denying other users access.


Upon loading metasploit (Figure 1), I searched for a module involving smtp user enumeration by typing the command shown in Figure 2. I chose the auxiliary scanner module (located in spot “1”)

FIGURE 1:

<img width="404" height="61" alt="SMTP 1" src="https://github.com/user-attachments/assets/df9915b1-7ae9-425d-a0a6-cf98774006b9" />

FIGURE 2:

<img width="1899" height="293" alt="SMTP 2" src="https://github.com/user-attachments/assets/a80d353b-44dd-4254-9490-fdb31ca5917a" />


Once I chose the module, I configured its settings to use against my target IP Address (Figures 3 & 4)

FIGURE 3: 

<img width="1873" height="386" alt="SMTP 3" src="https://github.com/user-attachments/assets/97f2e693-8f06-4cd6-a6ea-5f831e6d5de7" />

FIGURE 4:

<img width="804" height="68" alt="SMTP 4" src="https://github.com/user-attachments/assets/3a7f6303-0a29-4870-8a1e-d541b0982a22" />


After running the module, you can see that the scanner completed successfully and I have found users on the system 

FIGURE 5:

<img width="957" height="87" alt="SMTP 5" src="https://github.com/user-attachments/assets/4f580752-c40b-49a7-9c76-f6394e0587d6" />


Remediation tips would be as follows:

1. Enable rate limiting and connection throttling

-Configure the server to limit repeated authentication attempts to slow down brute force or enumeration tools.


2. Enable SMTP authentication only for trusted IP addresses

-Restrict AUTH LOGIN or similar mechanisms to internal or VPN subnets if external auth is unnecessary.
