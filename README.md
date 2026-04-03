# 📡 Big Data Storage Optimization — Telecom Analytics

## 🚀 Arquitetura de Armazenamento para Sistema Analítico em Larga Escala

Projeto académico focado no **design de uma arquitetura de armazenamento para Big Data**, com suporte a **terabytes diários de dados** e consultas analíticas em tempo quase real.

---

## 🎯 Objetivo

Projetar uma solução robusta para:

- 📊 Armazenar grandes volumes de dados (CDR + Internet)
- ⚡ Garantir alto desempenho em consultas analíticas
- 🛡️ Assegurar tolerância a falhas e durabilidade
- 💰 Otimizar custo vs performance

---

## 🌍 Cenário

Empresa de telecomunicações com:

- Milhões de clientes
- Dados gerados continuamente (~5 TB/dia)
- Retenção de dados por 5 anos
- Consultas analíticas complexas (tempo, região, serviço)

---

## 📦 Volume de Dados

| Métrica | Valor |
|--------|------|
| Geração diária | 5 TB |
| Período | 5 anos |
| Volume bruto | 9.1 PB |
| Com overhead | ~11.9 PB |
| Após compressão | ~4–5 PB |

---

## 🔥 Arquitetura de Armazenamento (Tiered Storage)

### 🧠 Estratégia por Camadas

| Camada | Tecnologia | Uso |
|-------|----------|----|
| 🔴 Quente | NVMe SSD (RAID 10) | Dados recentes (0–90 dias) |
| 🟠 Morna | SATA SSD (RAID 5) | 3–12 meses |
| 🔵 Fria | HDD (RAID 6) | Histórico (1–5 anos) |
| ⚫ Logs | NVMe (RAID 1) | WAL / Metadados |

---

## ⚙️ Configuração RAID

| Tipo de Dados | RAID | Justificação |
|--------------|------|-------------|
| Logs | RAID 1 | Alta segurança e baixa latência |
| Dados quentes | RAID 10 | Máximo desempenho |
| Dados mornos | RAID 5 | Equilíbrio custo/performance |
| Dados frios | RAID 6 | Alta tolerância a falhas |

---

## 📊 Estratégia de Particionamento

- 📅 Particionamento por **tempo (mensal)**
- 🌍 Sub-particionamento por **região**
- ⚡ Partition pruning → menos I/O

### Exemplo:

CDR_2024_06_Norte
CDR_2024_06_Sul
CDR_2024_06_Centro


---

## 🧠 Gestão de Buffer

- Buffer pool: **75–80% da RAM**
- Política: **LRU + Read-Ahead**
- Estratégias:
  - Pré-carregamento (warming)
  - Pinning de dados críticos
  - Separação leitura/escrita

---

## 🗄️ Implementação SQL (MySQL)

### 📌 Tabela CDR

- Particionada por data
- Índices otimizados para analytics
- Suporte a milhões de registos

### 🔧 Funcionalidades:

✔ Particionamento por RANGE  
✔ Índices compostos  
✔ Inserção em massa (batch)  
✔ Queries analíticas otimizadas  

---

## 📈 Exemplos de Consultas

### 🔹 Análise por região e período
```sql
SELECT Regiao, SUM(BytesTransf)
FROM CallDetailRecords
GROUP BY Regiao;

SELECT IDCliente, SUM(BytesTransf)
FROM CallDetailRecords
GROUP BY IDCliente;



