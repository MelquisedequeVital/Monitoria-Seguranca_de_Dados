# **Backup (Cópia de Segurança)**

O **backup** é definido como a cópia de dados de um dispositivo de armazenamento para outro, com o objetivo principal de permitir a sua **restauração** em caso de perda dos dados originais por qualquer motivo (falha de hardware, vírus, roubo ou erro humano).

---

## **1. O Pilar Garantido: Disponibilidade**

O backup é a contramedida fundamental para garantir a **Disponibilidade** da informação. Ele assegura que, mesmo diante de incidentes críticos, os sistemas e informações essenciais possam ser recuperados e fiquem acessíveis novamente.

---

## **2. A Política de Backup**

A perda de dados é frequentemente causada pela falta de uma política estruturada. Uma política eficiente deve compreender as seguintes etapas:

* **Análise e Mapeamento:** Identificar onde os dados estão, como estão organizados e descartar o "lixo digital".
* **Identificação de Dados Críticos:** Definir quais informações são realmente relevantes e indispensáveis para a continuidade do negócio.
* **Normatização:** Estabelecer regras de uso para funcionários e técnicos, padronizando onde e como a informação deve ser salva.
* **Armazenamento Seguro:** Considerar o uso de cofres anti-chamas e o armazenamento de cópias fora da instituição física para proteção contra desastres ambientais.
* **Revisão Periódica:** A política deve ser revista e testada pelo menos uma vez ao ano.

---

## **3. Tipos de Backup e Funcionamento**

Os backups operam manipulando a "marca" (bit de atributo) dos arquivos para identificar o que já foi copiado.

| Tipo de Backup | Arquivos Copiados | Ação na "Marca" (Atributo) |
| --- | --- | --- |
| **Completo (Normal)** | Todos os arquivos selecionados. | **Marca** todos os arquivos copiados. |
**Cópia** |	Todos os arquivos selecionados (idêntico ao Completo).	| **Não altera** a marca do arquivo (útil para duplicar sem quebrar o ciclo).
| **Incremental** | Apenas os criados ou alterados desde o último backup. | **Marca** os arquivos após a cópia. |
| **Diferencial** | Todos os novos ou alterados desde o último backup completo. | **Não altera** a marca do arquivo. |
| **Diário** | Todos os novos ou alterados diariamente. | **Não altera** a marca do arquivo. |

### **Diferencial vs. Incremental**

* **Incremental:** É o mais rápido e ocupa menos espaço, mas a restauração é lenta, pois exige o último backup completo e *todos* os incrementais subsequentes.
* **Diferencial:** É acumulativo; cada backup contém tudo o que mudou desde o último completo. A restauração é mais rápida, exigindo apenas o último completo e o último diferencial.

---

## **4. Estratégias de Armazenamento e Resiliência (Regras de Backup)**

Uma estratégia de backup é um plano estruturado que define como, onde e com que frequência as cópias de segurança serão armazenadas. Ela serve para garantir redundância e segurança, criando regras matemáticas de distribuição dos dados para que, se uma cópia falhar (por defeito no disco, incêndio ou ataque hacker), sempre exista outra alternativa segura e isolada para recuperar os sistemas.

### **A Regra Clássica 3-2-1**

* **3 Cópias dos dados:** Sendo 1 original (produção) e 2 cópias de backup.
* **2 Mídias diferentes:** Armazenar os backups em tipos de tecnologias distintas (ex: um em disco local/NAS e outro em fita LTO ou nuvem).
* **1 Cópia fora do site (Offsite):** Manter pelo menos uma das cópias geograficamente distante da empresa.

### **A Variação 3-2-2**

Indicada para cenários corporativos que demandam rápida recuperação local combinada com proteção geográfica redundante.

* **3 Cópias dos dados:** Sendo 1 original e 2 backups.
* **2 Mídias diferentes:** Dispositivos locais distintos (ex: servidores de backup separados).
* **2 Locais externos (Offsite):** Em vez de apenas um local fora da empresa, os dados são enviados para dois destinos externos diferentes (ex: dois provedores de nuvem distintos, como AWS e Azure, ou uma nuvem e um datacenter secundário).

### **A Regra Avançada 3-2-2-1**

É o modelo mais robusto de arquitetura híbrida e resiliência contra ameaças sofisticadas como Ransomware.

* **3 Cópias dos dados:** Sendo 1 de produção e 2 de backup.
* **2 Mídias diferentes:** Armazenamento local distribuído em tecnologias ou dispositivos independentes.
* **2 Cópias fora do site (Offsite):** Replicação para dois destinos externos ou nuvens independentes.
* **1 Cópia Imutável ou Offline (Air-Gapped):** Uma das cópias externas deve ser totalmente isolada da rede ou protegida por criptografia WORM (*Write Once, Read Many*), garantindo que ela não possa ser modificada, deletada ou criptografada por invasores.

---

## **5. Ferramentas e Implementação**

* **Windows Server:** Utiliza assistentes de agendamento e recuperação, permitindo otimizar o desempenho escolhendo entre backups completos ou incrementais por volume.
* **Linux (Comando TAR):** O utilitário `tar` (*tape archiver*) é o padrão histórico. Embora criado para fitas magnéticas, pode salvar backups em qualquer mídia.
* Exemplo: `tar cvf backup.tar /home/usuario` (onde **c** cria, **v** detalha o processo e **f** define o nome do arquivo).



---

Para realizar um backup de forma programática, as linguagens de programação oferecem diversas formas de manipular arquivos e diretórios. Abaixo estão exemplos práticos em **Python** e **Bash** (Linux), focando na lógica de cópia de segurança explicada anteriormente.

---

### **1. Exemplo em Python (Utilizando `shutil`)**

Python é excelente para automação devido à sua biblioteca padrão que lida com compressão e manipulação de arquivos.

```python
import shutil
import os
from datetime import datetime

# Configurações de origem e destino
diretorio_origem = '/home/usuario/documentos_importantes'
diretorio_destino = '/mnt/backup_disco_externo'
data_atual = datetime.now().strftime('%Y-%m-%d_%H-%M-%S')
nome_arquivo_backup = f"backup_completo_{data_atual}"

def realizar_backup():
    try:
        # Cria um arquivo compactado (ZIP) da origem no destino
        # Isso equivale a um Backup Completo
        caminho_final = os.path.join(diretorio_destino, nome_arquivo_backup)
        shutil.make_archive(caminho_final, 'zip', diretorio_origem)
        
        print(f"Backup realizado com sucesso em: {caminho_final}.zip")
    except Exception as e:
        print(f"Erro ao realizar backup: {e}")

if __name__ == "__main__":
    realizar_backup()

```

---

### **2. Exemplo em Bash / Linux (Utilizando `tar`)**

O comando `tar` (*tape archiver*) é a ferramenta padrão no Linux para gestão de backups.

```bash
#!/bin/bash

# Definição de variáveis
ORIGEM="/home/aluno"
DESTINO="/backup"
DATA=$(date +%Y-%m-%d_%H-%M-%S)
ARQUIVO="aluno-backup-$DATA.tar.gz"

# Executando o comando tar
# c: cria um novo arquivo
# v: modo verboso (mostra o progresso)
# z: compacta usando gzip
# f: define o nome do arquivo de destino
tar -cvzf $DESTINO/$ARQUIVO $ORIGEM

echo "Backup concluído: $DESTINO/$ARQUIVO"

```

---

### **3. Lógica para Backup Incremental (Python)**

Diferente do backup completo, o incremental verifica a data de modificação para copiar apenas o que mudou.

```python
import shutil
import os
import time

origem = 'dados_projeto'
destino = 'backup_incremental'
# Simulação do tempo do último backup (ex: 24 horas atrás)
tempo_ultimo_backup = time.time() - (24 * 60 * 60)

if not os.path.exists(destino):
    os.makedirs(destino)

# Itera sobre os arquivos para identificar alterações
for arquivo in os.listdir(origem):
    caminho_arq = os.path.join(origem, arquivo)
    
    # Verifica se o horário de alteração é mais recente que o último backup
    if os.path.getmtime(caminho_arq) > tempo_ultimo_backup:
        shutil.copy2(caminho_arq, destino) # copy2 preserva metadados
        print(f"Copiando arquivo alterado: {arquivo}")

```

### **Considerações Técnicas de 2026**

* **Integridade**: Em sistemas modernos, recomenda-se gerar um **Hash** (como SHA-256) do arquivo de backup logo após a criação para garantir que ele não foi corrompido ou alterado.
* **Imutabilidade**: Se estiver usando Python para enviar backups para a nuvem (como AWS S3), utilize políticas de **Object Lock** para garantir que o backup seja imutável contra Ransomware por um período determinado.

---

## **6. O Cenário em 2026: Imutabilidade e Ransomware**

Informações atuais complementares ao material de 2023:

* **Backups Imutáveis (WORM):** Em 2026, devido ao avanço dos Ransomwares que tentam apagar as cópias de segurança, a tecnologia de "Escrita Única, Múltiplas Leituras" tornou-se padrão. Uma vez escrito, o backup não pode ser deletado ou alterado por um período definido, nem mesmo por administradores.
* **Evolução das Regras (3-2-1-1 e 3-2-2-1):** A regra clássica expandiu-se. O foco agora está em garantir múltiplas saídas externas e imutabilidade cibernética obrigatória para barrar ataques lógicos em rede de movimento lateral.
* **Backup Gerenciado por IA:** Sistemas modernos usam IA para detectar anomalias no volume de dados alterados. Se um backup incremental de repente for muito maior que o normal, a IA sinaliza um possível ataque de criptografia (Ransomware) em curso.