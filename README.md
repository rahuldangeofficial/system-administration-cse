# System Administration

## Important Note -

 

> replace ${user} with your name
> 
> 
> replace ${ip} with your ip
> 
> While performing execution commands 
> 
> Example - 
> 
> ```jsx
> var name=rahul;
> console.log(`i am ${name}`) //i am rahul
> var ip=x;
> console.log(`my ip is ${ip}`) //my ip is x
> ```
> 
> 'my name  is \$ \{user\}' is equal to 'my name is rahul' in my case so just change it accordingly and same with \$ \{ip\}
> 
> And one more important setting you need to make before performing practicals, set your network connection type to bridged type, that is mandatory.
>
> If you are performing practicals on your laptop it might not support “bridged” in that case you need to select “internal” at the bottom 
>

---

### Check IP

```bash
ip a
```

### Ubuntu Firewall

```bash
sudo ufw status 
sudo ufw enable
sudo ufw status
```

### Install & config SSH

```bash
sudo apt-get update
sudo apt install openssh-server
sudo ufw allow ssh
sudo systemctl start ssh
sudo systemctl status ssh
```

> Ctrl+c
> 

```bash
sudo adduser ${user}
```

> Install putty on windows, open it put your ip and select ssh and run it.
> 

<aside>
 Done!
</aside>

---

### Install & Config Telnet

<aside>
“xinetd” is server and “telnet” is client so we first need to install xinetd
<br/><br/>
</aside>

```bash
sudo apt install telnetd xinetd
sudo ufw allow 23
sudo nano /etc/xinetd.d/telnet
```

<aside>
File will open up, navigate using arrow keys and add following lines
<br/><br/>
</aside>

```text
service telnet
{
 disable = no
 flags = REUSE
 socket_type = stream
 wait = no
 user = root
 server = /usr/sbin/in.telnetd
 log_on_failure += USERID
}
  
``` 

> Ctrl+s
> 

> Ctrl+x
> 

```bash
sudo systemctl start xinetd.service
sudo systemctl status xinetd.service
```

> Install putty on windows, open it put your ip and select telnet and run it.
> 

<aside>
Done!

</aside>

---

### Install FTP Server

```bash
sudo apt install vsftpd
sudo ufw allow OpenSSH
sudo ufw allow 20/tcp
sudo ufw allow 21/tcp
sudo ufw allow 990/tcp
sudo ufw allow 40000:50000/tcp
sudo ufw status
sudo nano /etc/vsftpd.conf
```

<aside>
Now file will open up, just add the following line at the bottom (so that it can allow upload as well)
<br/><br/>
</aside>

> write_enable=YES
> 

> Ctrl+s
> 

> Ctrl+x
> 

<aside>
Now add user using following command
<br/><br/>
</aside>

```bash
sudo adduser ${user}
```

<aside>
Now open CMD in client machine and type following
<br/><br/>
</aside>

```bash
ftp ${ip}
```

<aside>
Perform login and it will open up so basically done!
<br/><br/>
</aside>

### Put, Get FTP

<aside>
Create any name file but I name it test.txt in ubuntu and save at location “home/${user}/”, basically after doing that come back to cmd where ftp is open and write following command
<br/><br/>
</aside>

```bash
cd /home/${user}
get test.txt
put test.txt
```

<aside>
Basically done!

</aside>

---

### Samba Server

```bash
sudo apt -y install samba 
sudo ufw allow 'Samba'
mkdir /home/${user}/public/
mkdir /home/${user}/private/
sudo nano /etc/samba/smb.conf
```

<aside>
File will open up just go at the end, and add following lines
<br/><br/>
</aside>

```
[public]
  comment = public 
  path = /home/${user}/public 
  read only = no
  browseable = yes 
  guest ok = yes
[private]
  comment = private 
  path = /home/${user}/private 
  read only = no
  browseable = yes 
  guest ok = no
```

> Ctrl+s
> 

> Ctrl+x
> 

```bash
sudo service smbd start
sudo smbpasswd -a ${user}
sudo service smbd status 
```

<aside>
Now it's done, you can check it by simply entering address in this format in the add network location in file explorer at network.
<br/><br/>
</aside>

> //${ip}/
> 

---

### Install HTTP Server

```bash
sudo apt-get update
sudo apt-get install apache2
sudo ufw allow 'Apache'
sudo systemctl start apache2.service
sudo systemctl status apache2.service
```

<aside>
Now go to windows browser and enter your ubuntu ip, and if it loads Apache page, everything working perfectly fine!

</aside>

---

### Squid Proxy Server

```bash
sudo apt install squid
sudo ufw allow 3128
sudo nano /etc/squid/squid.conf
```

<aside>
Now pay attention, by default it will deny all http access so you need to navigate to “http_access deny all” and make it “http_access allow all” additionally you need to add two more line at the top as follows
<br/><br/>
</aside>

```
acl blocked dstdomain "/etc/squid/blocked.txt"
http_access deny blocked
```

> Ctrl+s
> 

> Ctrl+x
> 

```bash
cd /etc/squid/
sudo touch blocked.txt
sudo nano blocked.txt
```

<aside>
 File will open up, now add urls like
<br/><br/>
</aside>

```
www.facebook.com
www.twitter.com
```

> Ctrl+s
> 

> Ctrl+x
> 

```bash
sudo systemctl start squid
sudo systemctl enable squid
sudo systemctl status squid
```

<aside>
Now it's done! Just change the proxy server in your client as your ubuntu ip and port as 3128

</aside>

---

> therahuldange 
