# System Administration

Created: December 21, 2022 9:00 PM
Last edited time: January 18, 2023 11:03 AM
Status: Done

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
> “my name  is ${user}” is equal to “my name is rahul” in my case so just change it accordingly and same with ${ip}
> 

And one more important setting you need to make before performing practicals, set your network connection type to bridged type, that is mandatory.

If you are performing practicals on your laptop it might not support “bridged” in that case you need to select “internal” at the bottom 

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
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Done!

</aside>

---

### Install & Config Telnet

<aside>
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> “xinetd” is server and “telnet” is client so we first need to install xinetd

</aside>

```bash
sudo apt install telnetd xinetd
sudo ufw allow 23
sudo nano /etc/xinetd.d/telnet
```

<aside>
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> File will open up, navigate using arrow keys and add following lines

</aside>

> service telnet
> 
> 
> {
> disable = no
> flags = REUSE
> socket_type = stream
> wait = no
> user = root
> server = /usr/sbin/in.telnetd
> log_on_failure += USERID
> }
> 

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
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Done!

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
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Now file will open up, just add the following line at the bottom (so that it can allow upload as well)

</aside>

> write_enable=YES
> 

> Ctrl+s
> 

> Ctrl+x
> 

<aside>
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Now add user using following command

</aside>

```bash
sudo adduser ${user}
```

<aside>
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Now open CMD in client machine and type following

</aside>

```bash
ftp ${ip}
```

<aside>
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Perform login and it will open up so basically done!

</aside>

### Put, Get FTP

<aside>
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Create any name file but I name it test.txt in ubuntu and save at location “home/${user}/”, basically after doing that come back to cmd where ftp is open and write following command

</aside>

```bash
cd /home/${user}
get test.txt
put test.txt
```

<aside>
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Basically done!

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
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> File will open up just go at the end, and add following lines

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
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Now it's done, you can check it by simply entering address in this format in the add network location in file explorer at network.

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
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Now go to windows browser and enter your ubuntu ip, and if it loads Apache page, everything working perfectly fine!

</aside>

---

### Squid Proxy Server

```bash
sudo apt install squid
sudo ufw allow 3128
sudo nano /etc/squid/squid.conf
```

<aside>
<img src="https://www.notion.so/icons/report_gray.svg" alt="https://www.notion.so/icons/report_gray.svg" width="40px" /> Now pay attention, by default it will deny all http access so you need to navigate to “http_access deny all” and make it “http_access allow all” additionally you need to add two more line at the top as follows

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
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> File will open up, now add urls like

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
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Now it's done! Just change the proxy server in your client as your ubuntu ip and port as 3128

</aside>

---

<aside>
<img src="https://www.notion.so/icons/chat_gray.svg" alt="https://www.notion.so/icons/chat_gray.svg" width="40px" /> Still if you face difficulty then you refer my screenshots as below

</aside>

[https://drive.google.com/drive/folders/18CeqwV2jTGLutnCHRCxysdSZsYP96PzT](https://drive.google.com/drive/folders/18CeqwV2jTGLutnCHRCxysdSZsYP96PzT)

---

<aside>
<img src="https://www.notion.so/icons/heart-outline_gray.svg" alt="https://www.notion.so/icons/heart-outline_gray.svg" width="40px" /> Have a Great Day! ^⁠_⁠^

> therahuldange
> 
</aside>