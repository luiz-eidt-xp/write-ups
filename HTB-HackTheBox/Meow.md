# HTB - Meow

**Plataforma:** Hack the Box

**Máquina:** Meow

**Data:** 29/09/25

**Objetivo:** Capturar a flag

---

## Reconhecimento:

### =Varredura de Portas=

Usei a ferramenta **NMAP**, o site ja tinha me disponibilizado um alvo, o alvo era 10.129.77.193, eu escaneei as portas com o seguinte comando
```bash
nmap -sS 10.129.77.193
```
O resultado no terminal foi:
```bash
23/tcp  open telnet
```
---

## Evolução de Privilégios

### =Conexão Telnet=

Como é um serviço vulnerável e o site ja tinha me dado 3 opções de usuário, foi muito fácil adentrar ao ambiente, apenas segui esses passos:
```bash
telnet 10.129.77.193
```
Assim descobri que o serviço aceitava conexão direta sem muita autenticação

Fui para a tentativa de login, que foi simples, pois só nescessitava de ter o usuário que era **root**.
Assim tive total acesso como **root**

---

## Procurando a Flag

Fiz uma busca rápida pelos diretórios, so com o comando **ls** ja listava o arquivo **flag.txt**, e com o comando:
```bash
cat flag.txt
```
Tive acesso a flag, localização dela **/root/flag.txt**

---

## Vulnerabilidades

- [Serviço Telnet exposto na rede]
- [Autenticação com senha em branco]

### Recomendações de segurança

- [Desabilitar o serviço Telnet]
- [Implementar login com usuário e senha, e não deixar sem senha]
- [Usar em vez de Telnet, SSH com chaves criptogrfadas]

---

## Conclusão

Essa maquina foi bem simples, mas tbm sou iniciante ent é para meu nível, eu gostei de explorar vulnerabilidades assim, espero que quem esteja lendo tenha gostado do meu relatório, até mais.

