Maquina: Crocodile
ip: 10.129.1.15

---

Recon:
Eu ja sabia que a maquina tinha um serviço FTP e um HTTP, ent eu só escaneei a porta 21 e a 80 pra ser mais rapido.

[nmap] nmap -sV -sC -p 21-80 10.129.1.15 

{ PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
|_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.148
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Smash - Bootstrap Business Template
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Unix }

O comando do nmap -sC ja conecta no server de forma anonima

agora nós dentro do server, vamos listar o arquivos

ftp> ls
229 Entering Extended Passive Mode (|||42446|)
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
226 Directory send OK.

E agora vamos baixar esses dois arquivos

eu usei o comando get <nome do arquivo> e baixei os dois no meu pc
agora vamos para o server http

entrando no meu navegador com http://10.129.1.15
deu em uma pagina

mas antes de qualquer coisa, eu dei um cat no dois arquivos que baixei

┌──(xpfaint㉿kali)-[~]
└─$ cat allowed.userlist
aron
pwnmeow
egotisticalsw
admin
                                                                                                                                      
┌──(xpfaint㉿kali)-[~]
└─$ cat allowed.userlist.passwd
root
Supersecretpassword1
@BaASD&9032123sADS
rKXM59ESxesUFHAd

---

Agora no gobuster, eu usei:

gobuster dir -u http://10.129.1.15/ -w /usr/share/wordlists/dirb/common.txt -x php   

pra encontrar arquivos php

eu achei as seguintes informações uteis

/.htaccess            (Status: 403) [Size: 276]
/.hta.php             (Status: 403) [Size: 276]
/.hta                 (Status: 403) [Size: 276]
/.htaccess.php        (Status: 403) [Size: 276]
/.htpasswd            (Status: 403) [Size: 276]
/.htpasswd.php        (Status: 403) [Size: 276]
/assets               (Status: 301) [Size: 311] [--> http://10.129.1.15/assets/]
/config.php           (Status: 200) [Size: 0]
/css                  (Status: 301) [Size: 308] [--> http://10.129.1.15/css/]
/dashboard            (Status: 301) [Size: 314] [--> http://10.129.1.15/dashboard/]
/fonts                (Status: 301) [Size: 310] [--> http://10.129.1.15/fonts/]
/index.html           (Status: 200) [Size: 58565]
/js                   (Status: 301) [Size: 307] [--> http://10.129.1.15/js/]
/login.php            (Status: 200) [Size: 1577]
/logout.php           (Status: 302) [Size: 0] [--> login.php]
/server-status        (Status: 403) [Size: 276]

---

Descobri varios diretorios, a unicas duas que me interessei foram, login.php e assest

comecei a tentar logar com os usuarios que conegui no server ftp, no login.php

as credenciais que deram certo foram

admin
rKXM59ESxesUFHAd

Agora na pagina de admin, vou achar a flag

flag{c7110277ac44d78b6a9fff2232434d16}
