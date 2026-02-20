During this assessment, I identified a critical vulnerability in the organization’s Microsoft Office macro handling that allows an attacker to gain remote code execution on a Windows system. By leveraging a publicly available 
Nishang framework, I generated a malicious Word document that, once macros were enabled, downloaded and executed a staged PowerShell payload from my attacker-controlled host. This resulted in a reverse shell connection to 
my Kali Linux system, granting execution in the context of the logged-in user. Once access was established, user-level files, credentials, and system resources were exposed, creating significant risk for lateral 
movement and data compromise.

I used Out-Word.ps1 contained within the Nishang tool set to conduct this attack

FIGURE 1:

<img width="293" height="34" alt="AD Maldoc 1" src="https://github.com/user-attachments/assets/32cb7358-410b-4c90-be8a-a6332094469e" />

The Out-Word file can be viewed within the folders shown in Figure 1

FIGURE 2:

<img width="115" height="99" alt="AD Maldoc 2" src="https://github.com/user-attachments/assets/2fa5e930-d71a-43ec-837b-f06baef25ccf" />

Right clicking the open page of the Client folder shown in Figure 1, I opened a cmd shell as admin. 

FIGURE 3:

<img width="221" height="368" alt="AD Maldoc 3" src="https://github.com/user-attachments/assets/35f50848-7a75-4bfc-bb35-042fa0f01a67" />

I then 'dot-sourced' the Out-Word.ps1 file because it has functions that I need to have available to me

FIGURE 4:

<img width="298" height="29" alt="AD Maldoc 4" src="https://github.com/user-attachments/assets/eb1e4c74-1142-4924-99bd-01494b0ef7b6" />

Running the command displayed in Figure 5 downloaded a file called “Invoke-PowerShellTcp.ps1” from my simulated attacker machine (the IP address in the command). 
It then spawned a reverse shell from the victim machine onto my attacker machine using port 4444. Finally, it saved all of this to a Word document (seen at the end -OutputFile…). 
Doing this populated the Word doc labeled “test123” on the attack box

FIGURE 5:

<img width="756" height="57" alt="AD Maldoc 5" src="https://github.com/user-attachments/assets/781e3d79-184c-4ebc-835b-98aa7c8ac702" />

Next, on the attacker’s host machine, I acquired the previously mentioned Invoke-PowerShell script. I did so by searching for “nishang” on Github using Google (Figure 6).
Then went to the “Shells” folder of the Github repository for nishang (Figure 7), clicked on the “Invoke-PowershellTcp.ps1” file (Figure 8), and viewed it in its "raw" file format (Figure 9).

FIGURE 6:

<img width="585" height="218" alt="AD Maldoc 6" src="https://github.com/user-attachments/assets/d442639c-37d7-4ab6-8587-d3b4038c936f" />

FIGURE 7:

<img width="632" height="539" alt="AD Maldoc 7" src="https://github.com/user-attachments/assets/93743ad8-5a0f-4d26-a28b-ae5d90681ded" />

FIGURE 8:

<img width="680" height="293" alt="AD Maldoc 8" src="https://github.com/user-attachments/assets/fab43c15-cc3f-4372-b914-4fa548cb031a" />

FIGURE 9:

<img width="975" height="335" alt="AD Maldoc 9" src="https://github.com/user-attachments/assets/938f378b-be3a-4a95-a7d9-fa4b92d7a533" />

After copying the URL from the page (Figure 10), I downloaded it onto my attacker machine (Figure 11)

FIGURE 10:

<img width="627" height="38" alt="AD Maldoc 10" src="https://github.com/user-attachments/assets/1ee5f356-2785-468a-a901-c3ae7cf99657" />

FIGURE 11:

<img width="1227" height="74" alt="AD Maldoc 11" src="https://github.com/user-attachments/assets/ef479258-69d4-4472-a309-d73915a75687" />

Opening the file to view its contents (Figure 12), I added the line shown (Figure 13) to the bottom of the file.

FIGURE 12:

<img width="402" height="75" alt="AD Maldoc 12" src="https://github.com/user-attachments/assets/42f5c1a5-277b-4091-bd83-c5a2f4cd2f08" />

FIGURE 13:

<img width="863" height="239" alt="AD Maldoc 13" src="https://github.com/user-attachments/assets/dda7ccba-a28a-49b8-beee-0262ffb3dad5" />

Then, I started a python server on port 80 (Figure 14) and a netcat listener on port 443 (Figure 15) in the same directory that I just downloaded the nishang script to.

FIGURE 14:

<img width="677" height="75" alt="AD Maldoc 14" src="https://github.com/user-attachments/assets/1b0959d6-9069-46a8-bfeb-fc09053564cd" />

FIGURE 15:

<img width="329" height="89" alt="AD Maldoc 15" src="https://github.com/user-attachments/assets/2ae99e06-f021-4753-8744-83b79ce7432f" />

On the victim machine, I ran the “test123” maldoc (this simulated the victim clicking a malicious link through some form of Social Engineering such as a phishing email)

FIGURE 16:

<img width="151" height="137" alt="AD Maldoc 16" src="https://github.com/user-attachments/assets/dec2d63f-0b0d-48cf-a0d1-d0303856ef76" />

Nearing the end of the process, I clicked the “Enable Content” button in the Word document. In many real world malicious documents, there will be text attempting to motivate/trick the user into clicking this button

FIGURE 17:

<img width="1107" height="467" alt="AD Maldoc 17" src="https://github.com/user-attachments/assets/73e5a52a-5e2d-436e-9a5a-753937311753" />

Clicking the "Enable Content" button (Figure 17) called on the attacker’s web server to retrieve the file and then show a successful reverse shell connection to the victim’s machine. 
We can see this at the bottom of Figure 18 as the “PS” stands for PowerShell

FIGURE 18:

<img width="762" height="226" alt="AD Maldoc 18" src="https://github.com/user-attachments/assets/d2af7558-00dc-4509-96af-8c72885f9312" />

To confirm this, I ran the command “whoami” to see that I was the victim user.

FIGURE 19:

<img width="362" height="60" alt="AD Maldoc 19" src="https://github.com/user-attachments/assets/6f8b2c03-9781-43c4-be60-4e6f4e8e4476" />

Remediation tips would be as follows:

1. Block unsigned macros and macros from the Internet

  -Configure Office/Group Policy to disable all VBA macros except those signed by trusted publishers, and enable “Block macros from running in Office files from the Internet”.


2. Use Application Control (AppLocker / WDAC) to prevent unauthorized PowerShell and executables

  -Allow only approved scripts/binaries to run (allowlist approach). Block PowerShell from user-writable locations and disallow powershell.exe/pwsh.exe execution for non-admins where feasible.

  -This prevents staged payloads from executing even if a macro downloads them.

3. Email/web filtering and content scanning

  -Block or quarantine attachments with macros at the email gateway and strip or mark files downloaded from the web (especially docm, dotm, docx with active content).

  -Use URL reputation and attachment sandboxing to stop links to malicious hosts used to stage payloads.

4. Improve detection and response for PowerShell/Office abuse

  -Alert on unusual PowerShell network activity, encoded/obfuscated scripts, Office spawning PowerShell, or Invoke-WebRequest/bitsadmin from user contexts.

  -Have an IR playbook: isolate the host, collect memory/PS logs, reset credentials if needed, and run forensics.

5. Educate employees

  -Consistently train all employees on security risks (such as phishing or accessing untrusted sites) that could pose a threat to the system and its data

