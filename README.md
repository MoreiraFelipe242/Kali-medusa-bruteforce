Simulação de Ataques de Força Bruta com Medusa no Kali Linux

Sobre o Projeto
Este repositório documenta minha experiência prática utilizando Kali Linux, Metasploitable 2, DVWA e a ferramenta Medusa para simular ataques de força bruta em ambiente controlado.
O objetivo é compreender como esses ataques funcionam, identificar vulnerabilidades e aprender boas práticas de segurança para mitigá-las.
Todo o laboratório foi realizado exclusivamente em ambiente seguro e isolado, seguindo o propósito educacional da DIO.

Ambiente Utilizado
Foram utilizadas duas máquinas virtuais no VirtualBox:
Kali Linux como máquina atacante
Metasploitable 2 como máquina vulnerável
DVWA (Damn Vulnerable Web Application) para testes de força bruta em formulários web
Rede Host-Only, garantindo que as VMs se comuniquem apenas entre si, sem acesso externo
Esse ambiente permite realizar todos os testes de forma segura, sem qualquer impacto fora do laboratório.

Estrutura do Repositório
Você pode navegar pelos diretórios principais através dos links abaixo:
Wordlists (listas de usuários e senhas utilizadas nos testes)
https://github.com/SEU_USUARIO/kali-medusa-bruteforce/tree/main/wordlists

/kali-medusa-bruteforce│
├── README.md
├── wordlists/
│   ├── senhas.txt
│   └── usuarios.txt

Wordlists Utilizadas
Arquivo: wordlists/senhas.txt
Contém senhas simples usadas nos testes de força bruta.
Arquivo: wordlists/usuarios.txt
Contém nomes de usuários para enumeração e validação.

Testes Realizados
1. Ataque de força bruta em FTP com Medusa
Foi realizado um ataque de força bruta contra o serviço FTP da máquina Metasploitable 2, utilizando usuário definido e uma lista de senhas.
Exemplo de comando utilizado:
medusa -h 192.168.56.101 -u msfadmin -P wordlists/senhas.txt -M ftp
Resultado esperado: identificação de credenciais válidas.

2. Ataque de força bruta em formulário web (DVWA)
O DVWA foi configurado em modo Low para permitir testes de força bruta automatizados.
Comando utilizado com Hydra:
hydra 192.168.56.102 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed" -L wordlists/usuarios.txt -P wordlists/senhas.txt
Resultado esperado: identificação de login e senha válidos.

3. Testes em SMB (Password Spraying)
Inicialmente foi feita a enumeração de usuários:
enum4linux -a 192.168.56.101 | grep 'user'
Em seguida, password spraying utilizando a mesma senha para vários usuários:
medusa -h 192.168.56.101 -U wordlists/usuarios.txt -p 123456 -M smbnt

Medidas de Mitigação
Após os testes, foi possível identificar medidas importantes para evitar ataques semelhantes:
Utilizar senhas fortes e complexas
Bloquear tentativas após múltiplas falhas
Implementar autenticação em duas etapas
Limitar tentativas por IP
Desabilitar serviços desnecessários, como FTP e SMB
Revisar e monitorar logs de acesso regularmente

Conclusão
Este projeto permitiu compreender na prática como ataques de força bruta funcionam, como ferramentas como Medusa e Hydra automatizam tentativas e como ambientes vulneráveis respondem a esses ataques.
Além disso, reforça a importância de aplicar medidas de segurança para evitar acessos não autorizados.
