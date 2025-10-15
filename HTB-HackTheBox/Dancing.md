# HTB - Dancing

**Plataforma:** Hack the Box

**Máquina:** Dancing

**Data:** 15/10/25

**Objetivo:** Capturar a flag

---

## 1 - Descrição

Essa maquina usa o SMB(Server Message Block), é um protocolo de compartilhamento de arquivos da microsoft, desenvolvido pra maquinas windows, ou seja o alvo era um windows.

---

## 2 - Reconhecimento

###  =Varredura de Portas=

Novamente usei o [[NMAP]] pra escanear as portas e serviços daquele IP.
Como esqueci o IP que o site forneceu, irei usar 0.0.0.0 como exemplo:

```bash
nmap -sV 0.0.0.0
```

Usei -sV pra escanear os serviços e encontrei o servidor SMB usando a porta 445, e a porta estava aberta.

---

## 3 - Exploração

### =Servidor SMB=

Usei o comando:

```bash
smbclient -L 0.0.0.0
```

Eai apareceu os usuarios alguma coisa assim, e tinha 3 usuarios, um deles não precisava de senha pra entrar então fui conectar nele, o diretorio/usuario era o WorkShares

```bash
smbclient //0.0.0.0/WorkShares
```

Conectou e eu dei o comando **ls** e listou todos os arquivos, tinham duas pastas, a pasta 1 com um nome de funcionário e outra pasta 2 que era de outro usuario
Eu conectado no servidor usei **cd** pra entrar em cada pasta e listar os arquivos dentro delas, entrei na primeira pasta e não tinha nada.

Mas na segunda eu usei **ls** e apareceu que naquela pasta tinha o arquivo **flag.txt** e usei o seguinte comando:

```bash
get flag.txt
```

E o arquivo foi baixado no meu computador e nele continha a [[Flag]]

---

## Vulnerabilidades

- Servidor SMB exposto
- Usuário com senha em branco

## Recomendações de Segurança

- Não deixar nenhum usuário com senha em branco, sempre coloque senha
- Configurar um firewall pra esconder a porta do serviço SMB
- Ou trocar por algum serviço mais seguro e atual

---

## Conclusão

Maquina super fácil também, aprendi oq é um serviço SMB, estou cada vez mais me familiarizando com o linux e o terminal, e acho que vou continuar com esse modelo de escrever write-ups msm.
