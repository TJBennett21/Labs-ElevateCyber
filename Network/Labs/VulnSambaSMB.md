During this assessment, I identified a critical vulnerability in the network’s Samba SMB functionality that allowed me to gain root access to the system. 
By utilizing a publicly available Usermap_script exploit, an attacker is able to infiltrate and gain root access to the network. 
Once root access is attained, any and all files, users, and data are compromised.


After loading up metasploit (Figure 1), I searched for a Samba SMB module (Figure 2). These are exploit modules.

FIGURE 1:

<img width="362" height="61" alt="Network SMB 1" src="https://github.com/user-attachments/assets/ac24e543-cc08-4ba1-ac00-41a1e378af14" />

FIGURE 2:

<img width="472" height="76" alt="Network SMB 2" src="https://github.com/user-attachments/assets/357235b8-b10d-4c89-bd6f-2b632d3debbe" />


I noticed two different potential modules and used the one labeled “0”, which is the usermap_script module.

FIGURE 3:

<img width="1619" height="346" alt="Network SMB 3" src="https://github.com/user-attachments/assets/2acf719e-cdf4-4a96-a88a-9ceba0de7129" />

Next, I configured the options (Figure 4) and set the module to focus on the target IP address of the scope of this assessment (Figure 5)

FIGURE 4:

<img width="1852" height="576" alt="Network SMB 4" src="https://github.com/user-attachments/assets/f0a43239-d0ab-472b-aaaf-a11c6c53cc35" />

FIGURE 5:

<img width="829" height="77" alt="Network SMB 5" src="https://github.com/user-attachments/assets/941f9a73-8334-4110-9076-f8231105557f" />

Once the module was ran, a command shell opened. I confirmed by typing “whoami” to see that we have root access into the system. Other commands 
such as “ls” gave further proof to this

FIGURE 6:

<img width="665" height="677" alt="Network SMB 6" src="https://github.com/user-attachments/assets/417fc69a-f20a-42b0-8b67-68fd0c7e344e" />

Remediation tips would be as follows:

1.) Disable Username Map Script

  - Unless explicitly required, comment it out or remove from smb.conf to eliminate the remote code execution risk.

2.) Use username map (not username map script) for static user mapping

3.) Patch Samba to the latest version

  - Ensure Samba is fully updated to patch any known exploits related to username map script and others.

4.) Restrict access to Samba services

  - Use firewalls or Samba’s built-in access controls to limit who can reach the SMB ports.
