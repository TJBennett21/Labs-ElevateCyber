During testing, I successfully gained root level access to the system with a publicly available backdoor exploit using Metasploit. This demonstrates that the network currently lacks up-to-date versions of VSFTPD and scanning for active backdoors. If left unaddressed in a real-world scenario, this could allow attackers to gain full unauthorized access to the system resulting in a complete compromise in data and loss of confidentiality/integrity. 

After the initial nmap scan (Figure 1), I noticed vsftpd version 2.3.4 was running on port 21 (Figure 2)

FIGURE 1:

<img width="549" height="71" alt="VSFTPD 1" src="https://github.com/user-attachments/assets/58243563-9985-41e0-ae07-43f38124ca52" />

FIGURE 2:

<img width="579" height="383" alt="VSFTPD 2" src="https://github.com/user-attachments/assets/ac27d11e-b87c-4286-8f5d-eee66ccd8fb1" />


After somer research, I came across an article published by Rapid7 which covered a known vulnerability of an available backdoor. Using this backdoor would grant and attacker command execution on the service (Figure 3). Scrolling through the page there is a section that told me which Metasploit module to use (Figure 4)

FIGURE 3:

<img width="678" height="276" alt="VSFTPD 3" src="https://github.com/user-attachments/assets/9f0f8946-28df-4f5a-9c48-bd4011e329b8" />

FIGURE 4:

<img width="855" height="373" alt="VSFTPD 4" src="https://github.com/user-attachments/assets/3e91870e-c3f4-49f1-9907-5704aab072d1" />


Starting up Metasploit (Figure 5) and loading the module featured in the article (Figure 6), I set the "RHOSTS" to my target IP Address (Figure 7 & Figure 8)

FIGURE 5:

<img width="352" height="70" alt="VSFTPD 5" src="https://github.com/user-attachments/assets/309af2ff-d7e2-470c-bda5-1919de473f18" />

FIGURE 6:

<img width="783" height="94" alt="VSFTPD 6" src="https://github.com/user-attachments/assets/48a2b4d2-3ab6-45e5-835a-c184fcf99282" />

FIGURE 7:

<img width="1847" height="339" alt="VSFTPD 7" src="https://github.com/user-attachments/assets/fa9ea4c1-42f2-48d5-87b4-36cc53b78440" />

FIGURE 8:

<img width="855" height="80" alt="VSFTPD 8" src="https://github.com/user-attachments/assets/4e2c79ca-f855-48d9-9f52-2bbdb1d777a0" />


Running the module allowed me to attain root access in the command shell. I confirmed with the "whoami" command 

FIGURE 9:

<img width="1087" height="268" alt="VSFTPD 9" src="https://github.com/user-attachments/assets/8adba90b-c3ce-4e05-bcac-564baed1c9a9" />


Remediation tips would be as follows:

1. Ensure VSFTPD is updated to a trusted version
   
      -Remove any unverified or unofficial VSFTPD versions (especially 2.3.4); install only from official or distro-secure repositories.
  
2. Scan systems for backdoored versions
   
      -Use file integrity tools or hash checks to verify the VSFTPD binary matches trusted sources
  
3. Disable or remove VSFTPD if not needed

      -If FTP is not actively required, disable the service or uninstall it to eliminate attack surface.
