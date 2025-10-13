# HTB - Fawn

**Plataforma:** Hack the Box

**Máquina:** Fawn

**Data:** 13/10/25

**Objetivo:** Capturar a flag

---
## Reconhecimento:

### =Varredura de Portas=

**Ferramenta Usada: [[NMAP]]**
O HTB já tinha oferecido o ip que seria o nosso alvo, comecei primeiro escaneando as portas do ip, procedimento padrão, usei:

```bash
nmap -sV 10.129.87.96
```

Esse comando lista todos os serviços e versãos de protocolo do alvo
o resultado no terminal foi:

```bash
21/tcp open ftp vsftpd 3.0.3
Service Info: OS: Unix
```

---
## Evolução de Privilégios

### =Servidor FTP=

Após descobrir que o servidor FTP estava aberto, eu fui tentar entrar nele
usei:

```bash
ftp -a
```

O parâmetro **-a**, serve para fazer login de forma anônima sem conta, depois prossegui com o processo normal para fazer a entrada

```bash
ftp> ftp
(to) 10.129.87.96
220 (vsFTPd 3.0.3)
331 Please specify the password
230 login suceessful
Remote system type is UNIX
Using binary mode to transfer files
```

Depois disso tudo eu podia andar livremente pelos arquivos e diretórios do servidor FTP.

---
## Procurando a [[Flag]]

Agora com acesso total ao servidor eu simplismente escrevi

```bash
ls
```

E apareceu que o flag.txt estava naquele diretório, eu tentei usar o comando:
```bash
cat flag.txt
```
Mas não deu certo
E pesquisando mais descobri que o comando pra baixar arquivos de servidores ftp era o comando **get**, então usei:

```bash
get flag.txt
```

E o arquivo foi baixado no meu computador, então foi só eu abrir e lá estava a flag

---

## Vulnerabilidades

- Apenas com a autenticação anônima era capaz acessar o servidor inteiro
- Porta 21 FTP aberta exposta na rede

## Recomendações de segurança

- Melhore a segurança do servidor FTP, tirando a autenticação com conta anônima e colocando senhas fortes.
- Ou Substituir FTP puro por SFTP (via SSH) ou FTPS (FTP sobre TLS).
- Também restringir o acesso com Firewall, assim só IPs ou Redes permitidas poderão ter acesso.

---

## Conclusão

Essa maquina também é bem simples, e as vulnerabilidades são coisas simples, mas que em muitas empresas ocorrem por falta de investimento em segurança.

**OBS:** Preciso melhorar o jeito como escrevo relatórios mas isso ainda vou ver