🛡️ ***Projeto Prático de Auditoria: Brute Force com Medusa***

<div style="text-align: center;">
  <img src="imagens/readme/Imagem1.png" alt="Missão Hacker Medusa" width="500px">
</div>

📝 Introdução

Este projeto é um desafio prático desenvolvido durante o bootcamp de ***Cibersegurança***, uma parceria entre a ***DIO*** e a ***Riachuelo***. O objetivo principal foi consolidar conhecimentos em Linux, redes e segurança ofensiva, simulando cenários reais de ataques de força bruta para entender como fortalecer as defesas de um sistema.

Através deste laboratório, foi possível evoluir no caminho do ***Hacking Ético***, identificando vulnerabilidades críticas e aplicando boas práticas de mitigação. Além da parte técnica, o projeto reforçou minha familiaridade com o terminal Linux e ferramentas de linha de comando, elementos essenciais na minha rotina profissional.



🛠️ ***Tecnologias e Ferramentas Utilizadas:***

    Virtualização: Oracle VM VirtualBox.

    Sistema Atacante: Kali Linux (Rolling Edition).

    Sistema Alvo (Vulnerável): Metasploitable 2.

    Ferramenta de Auditoria: Medusa (Login Brute-force Attack Tool).

    Serviços Auditados: FTP, SMB e Formulários Web (DVWA).



🚀 ***Metodologia***
    Configuração de Rede: Uso de rede *Host-Only* para isolamento.

    Enumeração: Identificação de serviços (Nmap).

    Ataque: Execução de força bruta em serviços como FTP e SMB.

    Análise: Documentação de resultados e vulnerabilidades encontradas.



🔒 ***Medidas de Mitigação:***

* Implementação de políticas de bloqueio de conta.
* Uso de Autenticação de Dois Fatores (2FA).
* Monitoramento com Fail2Ban.
* Senha longa e forte com alteração a cada 2 meses, ou sempre que necessário.


---


##  👊 Execução do Ataque: FTP Brute Force

Nesta seção, documento a execução técnica do ataque de força bruta contra o serviço FTP.


### Passo 1: Identificação do alvo.
Levantei o IP da máquina **Metasploitable2** para garantir que estou no alvo correto.
```Bash
ip a 
```

<div style="text-align: center;">
  <img src="imagens/ftp/passo1.png" alt="print identificando IP" width="500px">
</div>

### Passo 2: Teste de Conectividade e Alcance de Rede.
Validei a comunicação entre o sistema atacante (Kali Linux) e o sistema alvo para garantir que não existam bloqueios de rede.
```Bash
ping -c 3 [ip_da_maquina_alvo]
```

<div style="text-align: center;">
  <img src="imagens/ftp/passo2.png" alt="print teste de conectividade" width="500px">
</div>

### Passo 3: Enumeração de Serviços com Nmap.
Realizei uma varredura nas portas principais para confirmar quais serviços estão ativos e identificar as versões dos protocolos (como FTP, SSH e SMB).
```Bash
sudo nmap -sV -p 21,22,80,445,139 [ip_da_maquina_alvo]
```

<div style="text-align: center;">
  <img src="imagens/ftp/passo3.png" alt="print teste de enumeracao" width="500px">
</div>

### Passo 4: Verificação Manual de Acesso.
Tentativa de conexão manual ao serviço FTP para validar o banner do serviço e confirmar que as credenciais padrão ou acesso anônimo não estão disponíveis.
```Bash
ftp [ip_da_maquina_alvo]
```

<div style="text-align: center;">
  <img src="imagens/ftp/passo4.png" alt="print teste de verificacao" width="500px">
</div>

### Passo 5: Preparação de Dicionários.
Criação de listas personalizadas de usuários e senhas (Wordlists) baseadas em perfis comuns de sistemas Linux e credenciais padrão de fábrica.
```Bash
echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt
echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt
``` 
<div style="text-align: center;">
  <img src="imagens/ftp/passo5.png" alt="print teste dos arquivos" width="500px">
</div>

### Passo 6: Execução do Ataque de Força Bruta com Medusa.
Utilização da ferramenta ***Medusa*** para automatizar as tentativas de login no serviço FTP, utilizando paralelismo para acelerar a descoberta de credenciais válidas.
```Bash
medusa -h [ip_maquina_alvo] -U users.txt -P pass.txt -M ftp -t 6
```
<div style="text-align: center;">
  <img src="imagens/ftp/passo6.png" alt="print do ataque com medusa" width="500px">
</div>

### Passo 7: Validação da Vulnerabilidade e Acesso Remoto.
Após a descoberta das credenciais, realizei o acesso final para confirmar a eficácia da auditoria e validar o nível de privilégio obtido no servidor.
``` Bash
ftp [ip_maquina_alvo]
```
<div style="text-align: center;">
  <img src="imagens/ftp/passo7.png" alt="print login na maquina 2 com o kali" width="500px">
</div>


---

## 💀 Execução do Ataque: Automação de Formulário Web (DVWA)

Nesta etapa, simulei um ataque de força bruta contra uma aplicação web real (Damn Vulnerable Web App). O diferencial aqui é a necessidade de entender os campos do formulário para que a ferramenta saiba onde inserir as credenciais.

### Passo 1: Acesso à Aplicação Alvo.
Acessei a interface web do DVWA através do navegador para identificar o comportamento da página de autenticação. É importante que a sua vm tenha conecação com a internet.
```Bash
URL: http://[ip_da_maquina_alvo]/dvwa/login.php
```
<div style="text-align: center;">
  <img src="imagens/web/passo1.png" alt="print pagina web" width="500px">
</div>

### Passo 2: Inspeção de Requisições HTTP.
Utilizei as ferramentas de desenvolvedor do navegador (F12 > Network) para capturar a requisição POST. Esse passo é fundamental para identificar o nome dos campos de entrada (username e password) e a mensagem de erro retornada pelo servidor em caso de falha.

<div style="text-align: center;">
  <img src="imagens/web/passo2.png" alt="print pagina web" width="500px">
</div>

### Passo 3: Mapeamento de Parâmetros e Credenciais.
Com os dados da rede, mapeei os campos do formulário. Reutilizei as wordlists de usuários e senhas criadas anteriormente para alimentar o motor de ataque do ***Medusa***.

### Passo 4: Ataque de Força Bruta com Módulo HTTP
Executei o ***Medusa*** utilizando o módulo http. Configurei a página de login, os campos identificados no Passo 2 e a "assinatura de erro" (Login failed) para que a ferramenta saiba distinguir um login mal sucedido de um sucesso.
```Bash
medusa -h [ip_maquina_alvo] -U users.txt -P pass.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 6
```
📸 Galeria de Execução: Ataque Web (DVWA)

Para facilitar o acompanhamento do processo, as etapas de identificação e execução estão organizadas abaixo:

<div style=" test-align:center;">

| Etapa 1: Configuração do Ataque | Etapa 2: Resultados do Brute Force |
|:---:|:---:|
| <img src="imagens/web/passo4.1.png" width="400px"><br><sup>Definição de parâmetros e início do Medusa</sup> | <img src="imagens/web/passo4.2.png" width="400px"><br><sup>Identificação de múltiplas credenciais válidas</sup> |
| **Etapa 3: Validação Manual** | **Etapa 4: Acesso Concedido** |
| <img src="imagens/web/passo4.3.png" width="400px"><br><sup>Inserindo usuário e senha encontrados</sup> | <img src="imagens/web/passo4.4.png" width="400px"><br><sup>Painel administrativo logado com sucesso</sup> |

</div>