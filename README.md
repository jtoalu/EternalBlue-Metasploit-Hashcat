# Deploy & hack into a Windows machine, leveraging common misconfigurations issues known as EternalBlue.
If you never heard of EternalBlue, check out the information on https://en.wikipedia.org/wiki/EternalBlue and visit https://learn.microsoft.com/en-us/security-updates/SecurityBulletins/2017/ms17-010 to mitigate the risk. I utilized the machines on https://tryhackme.com/dashboard to do this exercise.
1. First, I scanned the target machine with the following command $${\color{red}nmap \space \color{lightblue}-p \space 0-1000 \space -v \space --min-parallelism \space 100 \space -A \space -script \space vuln \space \color{orange}(target \space ip \space address)}$$
   Example: nmap -p 0-1000 -v --min-parallelism 100 -A -script vuln 10.10.38.165
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183423.png)
   If the target machine contains the EternalBlue vulnerability, the **nmap** shall give the ms17-010 in its result.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183608.png)
2. After detecting the **ms17-010** vulnerability, then *Metasploit* is utilized to initialize the exploitation.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183649.png)
   We search the **ms17-010** module and we shall utilize the module #0 from the search result.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183720.png)
   We set the *RHOST* to the ip address of the target machine, set the payload, and then we exploit the EternalBlue vulnerability in the target machine.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183757.png)
   We should obtain an access to the *system* in the target machine and we can test it by executing the *whoami* command.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183831.png)
   We can execute the *systeminfo* command to find out the specifications of the target machine.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183320.png)
3. We gained a foothold to the target machine. We shall put the gained shell into the background through *CTRL + Z* and we are going to convert it into a meterpreter shell by *search shell_to_meterpreter* module. There shall be only one matching module returned from the search and we *use 0* as the next step.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20124610.png)
   We set SESSION to the shell that we have just obtained and run the attack (exploit) to get a meterpreter session.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20124807.png)
   There shall be two active sessions listed when we execute *sessions -l* command and the meterpreter is listed as session 2. We select the meterpreter by executing *sessions 2* to use in our next step.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20124928.png)
   Verify that we have escalated to NT AUTHORITY\SYSTEM. Run getsystem to confirm this. Feel free to open a dos shell via the command *shell* and run *whoami*. It should return/confirm that we are indeed system.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20125028.png)
   Background this shell afterwards and select our meterpreter session for usage again. List all of the processes running via the *ps* command. Just because we are system doesn't mean our process is. Find a process towards the bottom of this list that is running as NT AUTHORITY\SYSTEM and write down the process id (far left column).
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20125106.png)
   We migrate to this process using the *migrate PROCESS_ID* command where the process id is the one we just wrote down in the previous step. This may take several attempts, migrating processes is not very stable. If this fails, we may need to re-run the conversion process or reboot the machine and start once again. If this happens, try a different process next time. In my case, I tried the *process id* from the second left column. Within our elevated meterpreter shell, run the command *hashdump*. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. 
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20125315.png)
   After obtaining the hashed passwords, we exit from the meterpreter.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20125521.png)
4. We utilize *hashcat* to crack the password of a user who has administrative privilege.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-21%20133909.png)
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-21%20133944.png)
5. We found *flag1* at the *system root* location.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20125614%20flag1.png)
   We found *flag2* at the the location where passwords are stored within Windows.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20125720%20flag2.png)
   We found *flag3* in an excellent location to loot. After all, administrators usually have pretty interesting things saved.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20125847%20flag3.png)

## The BLUE Badge is earned after completing all steps.
![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-20%20130121.png)

Note to Self: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax and https://stackoverflow.com/questions/11509830/how-to-add-color-to-githubs-readme-md-file
