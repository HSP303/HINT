
#PORT SCAN
$ nmap IP -sV -T4

#SAMBA SMB
$ smbcliente -L //IP

#BRUTEFORCE DIRETÓRIOS
$ gobuster dir -u https://url -w wordlist.txt -x .php,.txt,.bak,.sql

 
