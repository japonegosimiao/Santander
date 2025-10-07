# Santander
Desafio do Código da DIO

# Simulação de Ataque de Brute Force com Medusa e Kali Linux

Este repositório documenta a execução de um desafio prático proposto pela [DIO (Digital Innovation One)](https://web.dio.me/), focado em simular ataques de força bruta para fins de aprendizado em cibersegurança (pentest). O objetivo é utilizar as ferramentas **Kali Linux** e **Medusa** em um ambiente controlado para identificar vulnerabilidades em diferentes serviços e, em seguida, propor medidas de mitigação eficazes.

## Objetivos de Aprendizagem

* Compreender o funcionamento de ataques de força bruta em serviços como FTP, Web (formulários) e SMB.
* Utilizar o Kali Linux e a ferramenta Medusa para realizar auditorias de segurança.
* Documentar processos técnicos de forma clara e estruturada.
* Reconhecer vulnerabilidades comuns e propor medidas de correção e prevenção.
* Utilizar o GitHub como um portfólio técnico para compartilhar evidências e aprendizados.

## Ambiente e Ferramentas

Para a realização deste desafio, foi configurado um ambiente de laboratório virtual, garantindo que todos os testes fossem realizados de forma segura e isolada.

* **Sistema Operacional (Atacante):** Kali Linux
* **Sistema Operacional (Alvo):** Metasploitable 2
* **Ferramenta de Ataque:** Medusa
* **Software de Virtualização:** VirtualBox
* **Configuração de Rede:** Rede Interna (Host-Only) para comunicação isolada entre as VMs.

## Cenários de Ataque Executados

Foram realizados três cenários de ataque de força bruta, cada um focado em um serviço diferente para explorar distintas superfícies de ataque.

---

### 1. Ataque de Força Bruta ao Serviço FTP

O serviço FTP (File Transfer Protocol) é frequentemente um alvo para atacantes, pois uma credencial comprometida pode dar acesso direto a arquivos sensíveis no servidor.

* **Objetivo:** Identificar um usuário e senha válidos no serviço FTP rodando na máquina Metasploitable 2.
* **Comando Executado:**
    ```bash
    medusa -h 192.168.56.102 -u msfadmin -P /usr/share/wordlists/rockyou.txt -M ftp
    ```
* **Resultados:** O ataque foi bem-sucedido, encontrando a credencial `msfadmin:msfadmin`.
* **Evidência:**

---

### 2. Ataque de Força Bruta a Formulário Web (DVWA)

Ataques a formulários de login em aplicações web são extremamente comuns e visam obter acesso a contas de usuários. Para este cenário, foi utilizada a ferramenta Hydra, mais adequada para a tarefa.

* **Objetivo:** Descobrir a senha do usuário `admin` na página de login do Damn Vulnerable Web Application (DVWA).
* **Comando Executado:**
    ```bash
    hydra -l admin -P /usr/share/wordlists/fasttrack.txt 192.168.56.102 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
    ```
* **Resultados:** A ferramenta identificou com sucesso a credencial `admin:password`.
* **Evidência:**
    

---

### 3. Ataque de Password Spraying ao Serviço SMB

Password Spraying é uma variação do ataque de força bruta onde o atacante utiliza uma ou poucas senhas comuns contra uma lista de múltiplos nomes de usuário.

* **Objetivo:** Identificar se algum usuário no sistema possui uma senha fraca e comum (`msfadmin`) no serviço SMB.
* **Comando Executado:**
    ```bash
    medusa -h 192.168.56.102 -U /caminho/para/sua/lista_de_usuarios.txt -p msfadmin -M smbnt
    ```
* **Resultados:** O ataque encontrou com sucesso a conta `msfadmin:msfadmin`.
* **Evidência:**

## Medidas de Mitigação e Prevenção

Com base nos resultados obtidos, as seguintes medidas de segurança são recomendadas para prevenir ataques de força bruta:

1.  **Política de Senhas Fortes:** Exigir que os usuários criem senhas com alta complexidade.
2.  **Bloqueio de Contas (Account Lockout):** Bloquear temporariamente uma conta após um número definido de tentativas de login malsucedidas.
3.  **Autenticação Multifator (MFA):** Ativar a MFA para adicionar uma camada extra de segurança.
4.  **Rate Limiting:** Limitar o número de tentativas de login a partir de um único IP.
5.  **Uso de CAPTCHA:** Implementar CAPTCHAs em formulários de login para diferenciar usuários de bots.
6.  **Monitoramento e Alertas:** Configurar sistemas para detectar e alertar sobre um volume anormal de falhas de login.

## 🏁 Conclusão

Este desafio prático demonstrou a fragilidade de serviços mal configurados e a importância de implementar políticas de segurança robustas. Ferramentas como o Medusa são valiosas para auditores de segurança na identificação de senhas fracas, permitindo que as organizações corrijam essas falhas antes que sejam exploradas por agentes maliciosos.
