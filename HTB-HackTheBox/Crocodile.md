# Hack the Box - Crocodile


---
## * Informações

☐ **Descrição:** Uma maquina com servidor ftp com login anonimo exposto e um servidor http apache

☐ **Ip:** 10.129.1.15

☐ **Serviços:**  FTP e HTTP Apache

---
## ! Vulnerabilidades

| ID | Serviço | Descrição |
|----|------|-----------|
| V-01 | FTP | Servidor FTP exposto na rede com login anônimo habilitado |
|V-02 | HTTP/Apache | Server Apache deixa fazer ataque brute force de busca de diretórios ocultos |

---
## + Enumeração
### =NMAP=

Eu ja sabia que a maquina tinha um serviço FTP e um HTTP, ent eu só escaneei a porta 21 e a 80 pra ser mais rapido.
Usei o comando:

```bash
nmap -sV -sC -p  21-80  10.129.1.15
```
 Resultado:
```bash
PORT   STATE SERVICE VERSION
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
Service Info: OS: Unix
```

O parâmetro -sC ja conecta em qualquer servidor FTP que tenha servidor anônimo habilitado!

---
## $ Exploração
### =Servidor FTP=

Agora nós dentro do server, vamos listar o arquivos(Eu sei que isso já foi feito no scan do NMAP, mas quero deixar essa etapa aqui :p )

```bash
ftp> ls
229 Entering Extended Passive Mode (|||42446|)
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
226 Directory send OK.
```
E agora vamos baixar esses dois arquivos, eu usei o comando **get "nome do arquivo"** e baixei os dois no meu pc

Vamos ver o conteúdo deles usando **cat**
```bash
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
```


### =Servidor Apache=

O site tinha essa interface principal:
![](./screenshots/crocodile/http-page.png)

E é ela que vamos explorar

### =Gobuster=

Agora no gobuster, eu usei:

```bash
gobuster dir -u http://10.129.1.15/ -w /usr/share/wordlists/dirb/common.txt -x php  
```

Saída:
```bash
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
```

Descobri varios diretorios, a que me chamou atenção foi a login.php

A pagina do login.php:
![](./screenshots/crocodile/http-login.png)

---
## ^ Escalação de Privilégios
### =login.php=

Usando aquela pagina, eu fui testando as credenciais, e uma delas deram certo, mas isso eu não vou mostrar pois pode dar respostas da maquina

Agora na pagina de root a flag já é encontrada:
![](./screenshots/crocodile/flag.png)

---
## {Conclusão}

### Melhorias na Segurança

| ID | Serviço | Melhoria |
|----|------|-----------|
| V-01 | FTP | Desabilite o login anônimo |
|V-02 | HTTP/Apache | Bloqueei muitas requisições seguidas, pra evitar brute force |

### Opnião Pessoal

Achei essa maquina muito fácil msm na real, é muito simples resolver ela :)

---
>Write-up por Luiz A. | Hack The Box - Crocodile | 25/10/25
