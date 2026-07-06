# **Segurança de Rede: Firewalls e Sistemas de Detecção e Prevenção de Intrusão (IDS/IPS)**

A proteção de uma rede de computadores funciona como a segurança de um prédio comercial. Não adianta apenas trancar as portas; é preciso ter uma portaria que controla quem entra e sai (Firewall) e uma equipe de monitoramento que analisa se alguém lá dentro está se comportando de forma suspeita (IDS/IPS).

---

## **1. O Pilar Garantido: Disponibilidade e Confidencialidade**

Essas tecnologias são as principais responsáveis por defender as fronteiras da rede, garantindo dois pilares fundamentais:

* **Confidencialidade:** O Firewall e o IPS impedem que pessoas de fora invadam a rede para espiar ou roubar dados confidenciais (como o banco de dados da empresa).
* **Disponibilidade:** Protegem os servidores contra ataques de negação de serviço (DDoS) — que são tentativas de derrubar um sistema por sobrecarga. Bloqueando os acessos maliciosos, o sistema continua funcionando normalmente para os usuários legítimos.

---

## **2. O que é e como funciona o Firewall (Iptables/Netfilter)**

No Linux, o **Iptables** é a ferramenta que o administrador usa para criar as regras de segurança, e o **Netfilter** é o motor que fica dentro do sistema executando essas regras.

Imagine o firewall como um sistema de triagem. Para organizar o fluxo, ele divide as regras em **Tabelas** (o que fazer com o dado) e **Cadeias/Chains** (em que momento analisar o dado).

### **As Principais Tabelas**

* **Filter:** É a tabela padrão. Serve estritamente para decidir se um dado pode passar ou deve ser bloqueado.
* **NAT:** Serve para traduzir endereços de rede. É usada, por exemplo, para fazer com que vários computadores da rede interna naveguem na internet usando um único IP público.
* **Mangle:** Serve para fazer alterações especiais nos dados (como marcar pacotes para dar prioridade ao tráfego de voz ou vídeo).

### **As Cinco Cadeias Nativas (Chains)**

As cadeias são os "postos de controle" por onde os dados passam:

* `PREROUTING`: O dado acabou de chegar da rua (internet), antes mesmo de o sistema saber para onde ele vai.
* `INPUT`: O dado é destinado especificamente para o próprio servidor.
* `FORWARD`: O dado não é para o servidor; ele está apenas passando por ele para ir para outro computador da rede.
* `OUTPUT`: Um dado que o próprio servidor criou e está tentando enviar para fora.
* `POSTROUTING`: O dado já passou por todas as checagens e está saindo pela placa de rede rumo ao seu destino.

### **As Decisões do Firewall (Targets)**

Quando um dado se encaixa em uma regra, o firewall toma uma dessas três atitudes:

1. `ACCEPT`: Deixa o dado passar livremente.
2. `DROP`: Bloqueia o dado de forma silenciosa. Quem enviou fica esperando uma resposta até a conexão cair por tempo limite (*timeout*). É a melhor opção contra ataques.
3. `REJECT`: Bloqueia o dado, mas envia uma mensagem de volta dizendo: "Acesso negado".

---

## **3. Detectar vs. Prevenir Intrusões: IDS e IPS**

Enquanto o firewall olha apenas "as etiquetas" dos pacotes (de onde vem, para onde vai e qual a porta), o IDS e o IPS abrem o pacote para ler o que está escrito dentro dele, procurando vírus, códigos maliciosos ou comportamentos suspeitas.

### **As Diferenças entre IDS e IPS**

* **IDS (Sistema de Detecção de Intrusão):** Ele funciona de forma **passiva**. Fica conectado na rede recebendo uma cópia de todo o tráfego. Se ele encontrar algo perigoso, ele **apenas gera um alerta** para o administrador. Ele não interrompe o tráfego, ou seja, não tem impacto na velocidade da rede.
* **IPS (Sistema de Prevenção de Intrusão):** Ele funciona de forma **ativa**. Fica posicionado diretamente no meio do caminho do cabo de rede. Se ele detectar uma ameaça, ele tem o poder de **bloquear o ataque na hora**. Por inspecionar tudo em tempo real, ele pode deixar a rede um pouco mais lenta se não for bem configurado.

### **Como eles descobrem um ataque?**

* **Por Assinatura:** Funciona como um antivírus comum. Ele tem uma lista de "impressões digitais" de ataques conhecidos. Se o tráfego for igual a uma assinatura, ele avisa. É muito preciso, mas não descobre ataques inéditos (Dia-Zero).
* **Por Anomalia:** Ele aprende como a sua rede funciona no dia a dia (o comportamento normal). Se de repente houver um pico gigante de acessos em um horário estranho, ele desconfia e bloqueia. É ótimo para descobrir novos ataques, mas pode gerar alarmes falsos se a rotina da empresa mudar.

---

## **4. O Cenário em 2026: Firewalls Inteligentes e Automação**

* **Firewalls de Nova Geração (NGFW):** Em 2026, os firewalls antigos que só bloqueavam portas foram substituídos. Os NGFW unem o firewall tradicional e o IPS em uma única ferramenta inteligente. Agora, as regras não são mais por números, mas por comportamento e identidade (ex: "Permitir que o setor de RH use o LinkedIn, mas bloquear o acesso ao Facebook").
* **IA contra Criptografia:** Hoje em dia, quase todo o tráfego da internet é criptografado (escondido). Os sistemas de IDS/IPS modernos usam Inteligência Artificial para analisar o comportamento do tráfego oculto. Eles conseguem descobrir se há um vírus se comunicando dentro de um canal criptografado sem precisar quebrar a privacidade do usuário.
* **Resposta Automatizada (SOAR):** O IDS/IPS e o Firewall agora trabalham juntos e sozinhos. Se o IPS detecta um ataque em andamento na madrugada de um domingo, ele avisa o sistema central, que altera as regras do Firewall automaticamente para bloquear o invasor em segundos, sem precisar esperar um técnico acordar.

---

## **5. Implementação Prática (Códigos e Comandos)**

Conforme o modelo, aqui estão os exemplos técnicos de como essas regras são escritas na prática utilizando o terminal do Linux:

### **Regras de Filtro (Bloqueios e Liberações)**

```bash
# 1. Tranca tudo por padrão (Segurança Máxima: o que não for explicitamente permitido, será bloqueado)
iptables -P INPUT DROP

# 2. Garante que respostas de conexões que você mesmo iniciou consigam voltar para o seu PC
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# 3. Libera o funcionamento da rede local interna da própria máquina (loopback)
iptables -A INPUT -i lo -j ACCEPT

# 4. Libera o acesso administrativo (SSH - porta 22) apenas para os computadores da equipe de TI (rede 192.168.10.0/24)
iptables -A INPUT -s 192.168.10.0/24 -p tcp --dport 22 -j ACCEPT

# 5. Bloqueia imediatamente um computador hacker específico (IP 203.0.113.50) que está atacando a rede
iptables -A INPUT -s 203.0.113.50 -j DROP

```

### **Regras de Roteamento e Redirecionamento (Tabela NAT)**

```bash
# 1. Redirecionamento de Portas: Quem tentar acessar a porta 8080 do Firewall por fora, será jogado para o Servidor Web interno (IP 10.0.0.5 na porta 80)
iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 10.0.0.5:80

# 2. Compartilhamento de Internet: Mascara os computadores da rede interna para que todos naveguem usando o IP público da placa de rede externa (eth0)
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

```