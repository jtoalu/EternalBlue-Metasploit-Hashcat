# Deploy & hack into a Windows machine, leveraging common misconfigurations issues known as EternalBlue.
If you never heard of EternalBlue, check out the information on https://en.wikipedia.org/wiki/EternalBlue and visit https://learn.microsoft.com/en-us/security-updates/SecurityBulletins/2017/ms17-010 to mitigate the risk. I utilized the machines on https://tryhackme.com/dashboard to do this exercise.
1. First, I scanned the target machine with the following command $${\color{red}nmap \space \color{lightblue}-p \space 0-1000 \space -v \space --min-parallelism \space 100 \space -A \space -script \space vuln \space \color{orange}(target \space ip \space address)}$$
   Example: nmap -p 0-1000 -v --min-parallelism 100 -A -script vuln 10.10.38.165
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183423.png)
   If the target machine contains the EternalBlue vulnerability, the **nmap** shall give the ms017-010 in its result.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183608.png)
2. After detecting the **ms017-010** vulnerability, then *Metasploit* is utilized to initialize the exploitation.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183649.png)
   We search the **ms017-010** module and we shall utilize the module #0 from the search result.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183720.png)
   We set the *RHOST* to the ip address of the target machine, set the payload, and then we exploit the EternalBlue vulnerability in the target machine.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183757.png)
   We should obtain an access to the *system* in the target machine and we can test it by executing the *whoami* command.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183831.png)
   We can execute the *systeminfo* command to find out the specifications of the target machine.
   ![](https://github.com/jtoalu/EternalBlue-Metasploit-Hashcat/blob/main/Screenshot%202024-06-18%20183320.png)

# To be continued ...

Note to Self: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax and https://stackoverflow.com/questions/11509830/how-to-add-color-to-githubs-readme-md-file
