🔐 Simulação de Ataques de Força Bruta com Medusa no Kali Linux
📌 Descrição

Este projeto faz parte do desafio da DIO e tem como objetivo simular ataques de força bruta em um ambiente seguro utilizando:
Kali Linux como máquina atacante
Metasploitable 2 e DVWA como máquinas vulnerávei
Medusa, Hydra e ferramentas auxiliares para testes
Ambiente isolado via VirtualBox
Todos os experimentos foram realizados exclusivamente em ambiente controlado, com finalidade educacional e de auditoria, seguindo boas práticas de Segurança da Informação.

🖥 Ambiente Utilizado
✔ Máquinas Virtuais

Kali Linux 2024.x

Metasploitable 2

DVWA (Damn Vulnerable Web Application)

✔ VirtualBox
Configurações:
Rede: Host-Only ou Rede Interna
As VMs se comunicam entre si, mas não têm acesso à internet, garantindo segurança.

✔ Objetivo do Ambiente
Testar ataques de força bruta sem riscos
Demonstrar vulnerabilidades reais
Explorar formas de mitigar ataques

📂 Estrutura do Repositório
/kali-medusa-bruteforce
│
├── README.md
├── wordlists/
│   ├── senhas.txt
│   └── usuarios.txt
└── images/
    ├── ftp-attack.png
    ├── dvwa-bruteforce.png
    └── smb-passwordspray.png

📚 Wordlists Utilizadas
wordlists/senhas.txt
123
1234
12345
123456
password
toor
admin

wordlists/usuarios.txt
root
msfadmin
admin
user

🚀 Ataques Realizados
🔵 1. Ataque FTP com Medusa (Força Bruta)
✔ Objetivo
Testar logins no serviço FTP vulnerável da Metasploitable 2.

✔ Comando usado
(seguro e permitido porque foi executado em ambiente controlado)
medusa -h 192.168.56.101 -u msfadmin -P wordlists/senhas.txt -M ftp

✔ Resultado esperado
Identificação de credenciais válidas
Prints no terminal serão adicionados na pasta /images

🔵 2. Ataque Web (DVWA) – Força Bruta com Hydra
✔ Pré-requisitos
DVWA configurado em:
http://192.168.56.102/dvwa
Segurança do DVWA definida como: Low

✔ Comando usado
hydra 192.168.56.102 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed" -L wordlists/usuarios.txt -P wordlists/senhas.txt

✔ Resultado esperado
Hydra retorna combinação válida
Print salvo em /images/dvwa-bruteforce.png

🔵 3. SMB – Password Spraying + Enumeração
✔ Enumeração de usuários
enum4linux -a 192.168.56.101 | grep 'user'

✔ Password spraying com Medusa
Testando uma senha para vários usuários:
medusa -h 192.168.56.101 -U wordlists/usuarios.txt -p 123456 -M smbnt

✔ Resultado esperado
Um ou mais usuários autenticados com senha fraca
Prints salvos na pasta /images
Medidas de Mitigação
Após observar os ataques, foram identificadas medidas eficazes para evitar esse tipo de invasão:
Fortalecimento de Senhas
Exigir senhas longas e complexas
Não permitir senhas comuns (ex: “123456”, “admin”)
Bloqueio de Tentativas
Implementar bloqueio automático após X falhas
Delay progressivo entre tentativas
Para Web (DVWA)
Captcha
MFA (autenticação em duas etapas)
Rate limiting
Para FTP / SMB
Desabilitar serviços que não são usados
Permitir acessos só via VPN/rede interna
Monitorar logs constantemente

Conclusão Este laboratório permitiu compreender:
Como funciona um ataque de força bruta em diferentes protocolos
Como o Medusa e Hydra automatizam tentativas de login
A importância da segurança de senhas
Como medidas de mitigação evitam invasões reais
A construção de um ambiente seguro e isolado no VirtualBox
Todo o estudo foi documentado detalhadamente para apoio de estudos e práticas futuras.
