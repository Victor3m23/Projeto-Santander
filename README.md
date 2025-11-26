# Projeto-Santander
Este projeto foi desenvolvido para praticar técnicas de força bruta usando Kali Linux e a ferramenta Medusa em um ambiente controlado. Utilizei o Metasploitable 2 e o DVWA para observar serviços vulneráveis, testar ataques automatizados e registrar comandos, wordlists e aprendizados.
Projeto Prático – Testes de Força Bruta com Kali Linux e Medusa

Este repositório documenta minha experiência prática utilizando o Kali Linux e a ferramenta Medusa para realizar testes de força bruta em um ambiente controlado.
Usei o Metasploitable 2 e o DVWA como alvos, o que me permitiu entender melhor como serviços vulneráveis se comportam quando estão mal configurados.

O foco deste projeto foi aprender, testar, errar, acertar e ver na prática como esses processos funcionam.

Objetivos Principais

Entender ataques de força bruta em diferentes serviços
Configurar e operar um laboratório controlado
Utilizar o Medusa de forma eficaz
Identificar e mitigar vulnerabilidades
Documentar o processo de forma clara
Ambiente Utilizado
Criei duas máquinas no VirtualBox:
Kali Linux (atacante)
Metasploitable 2 (alvo)
Ambas foram configuradas em rede Host-Only, garantindo isolamento e segurança durante os testes.
Para validar a comunicação, utilizei:

ifconfig
ping <IP-METASPLOITABLE>
Wordlists Utilizadas
Foram criadas duas listas simples:

Arquivo senhas.txt:

123
admin
password
root
msfadmin


Arquivo usuarios.txt:
msfadmin
user
service

Teste 1 – Ataque FTP com Medusa
O FTP do Metasploitable é vulnerável por padrão, então serviu como ponto inicial.

Comando:
medusa -h <IP-METASPLOITABLE> -u msfadmin -P senhas.txt -M ftp


Resultado:
A senha foi encontrada rapidamente, mostrando como credenciais fracas comprometem qualquer serviço.
Teste 2 – Ataque a Formulário Web (DVWA)

Acesso ao DVWA pelo navegador:
http://<IP-METASPLOITABLE>/DVWA


Login padrão:
admin / password


Nível de segurança ajustado para Low.

Comando do ataque via Medusa:
medusa -h <IP-METASPLOITABLE> -u admin -P senhas.txt -M web-form -m FORM:"/DVWA/vulnerabilities/brute/?username=&password=&Login=Login:username=^USER^&password=^PASS^:F=Login failed"


Resultado:
O Medusa conseguiu automatizar o ataque e localizar a senha, demonstrando como bruteforce em formulários funciona.

Teste 3 – Password Spraying em SMB

Primeiro, realizei uma enumeração de usuários:

enum4linux -U <IP-METASPLOITABLE>


Depois fiz password spraying, testando uma única senha para todos os usuários.

Comando:
medusa -h <IP-METASPLOITABLE> -U usuarios.txt -P senha_unica.txt -M smbnt


Resultado:
Foi possível encontrar ao menos um usuário com senha fraca, evidenciando como apenas um ponto vulnerável já compromete o ambiente.

Medidas de Mitigação

Algumas lições importantes identificadas:

Evitar senhas fracas ou padrão

Habilitar bloqueio após múltiplas tentativas

Implementar MFA sempre que possível

Utilizar ferramentas como Fail2ban

Monitorar logs e alertas de autenticação

Desativar serviços desnecessários (como FTP)

Preferir senhas complexas ou autenticação por chave

O Que Aprendi

Este projeto ajudou a entender como um atacante pensa e como defender melhor um ambiente. Trabalhar com ferramentas como Medusa, Enum4Linux e DVWA tornou o processo mais claro e didático.
Também reforçou a importância de boas práticas de configuração e testes contínuos.

