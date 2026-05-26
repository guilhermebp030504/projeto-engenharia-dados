# Pipeline de Engenharia de Dados com Airflow, Snowflake, dbt e Google Data Studio

## Sobre o Projeto

Este projeto foi desenvolvido como prática de Engenharia de Dados utilizando ferramentas modernas amplamente utilizadas no mercado para construção de pipelines ELT, Data Warehousing e Analytics Engineering.

O pipeline realiza:

- Extração de dados de um banco PostgreSQL remoto
- Orquestração das cargas utilizando Apache Airflow
- Armazenamento dos dados no Snowflake
- Transformações analíticas utilizando dbt
- Construção de dashboards no Google Data Studio

---

# Arquitetura do Projeto

```text
PostgreSQL
     ↓
Apache Airflow
     ↓
Snowflake Data Warehouse
     ↓
dbt Transformations
     ↓
Google Data Studio
```

---

# Tecnologias Utilizadas

- Python
- Apache Airflow
- Docker
- PostgreSQL
- Snowflake
- dbt (Data Build Tool)
- SQL
- Google Data Studio

---

# Estrutura do Projeto

```text
.
│   docker-compose.yaml
│
├───config
│       airflow.cfg
│
├───dags
│       novadrive.py
│
├───dbt
│   │   dbt_project.yml
│   │   README.md
│   │
│   ├───models
│   │   ├───analysis
│   │   ├───dimensions
│   │   ├───facts
│   │   └───stage
│   │
│   ├───macros
│   ├───tests
│   ├───snapshots
│   └───seeds
│
├───plugins
│
└───docs
```

---

# Pipeline ELT

## Extração

Os dados são extraídos de um banco PostgreSQL remoto utilizando tarefas Python no Apache Airflow.

## Carga

Os dados são carregados incrementalmente no Snowflake, utilizado como Data Warehouse principal do projeto.

## Transformação

As transformações são realizadas utilizando dbt com separação em camadas:

- Staging Layer
- Dimensions
- Facts
- Analysis

## Visualização

Os dados transformados são consumidos diretamente no Google Data Studio para criação de dashboards analíticos.

---

# Modelagem de Dados

O projeto utiliza modelagem dimensional para organização analítica dos dados.

## Tabela Fato

- `fct_vendas`

## Tabelas Dimensão

- `dim_clientes`
- `dim_veiculos`
- `dim_vendedores`
- `dim_concessionarias`
- `dim_cidades`
- `dim_estados`

---

# Estrutura dbt

## Stage

Responsável pela limpeza e padronização inicial dos dados.

Exemplos:

- `stg_clientes`
- `stg_vendas`
- `stg_veiculos`

## Dimensions

Camada responsável pelas dimensões analíticas do projeto.

## Facts

Camada responsável pelas métricas e consolidação dos fatos de negócio.

## Analysis

Camada utilizada para consultas analíticas e exploração dos dados.

---

# Como Executar o Projeto

## Pré-requisitos

- Docker
- Docker Compose
- Python 3.13+
- Conta Snowflake

---

# Clonar Repositório

```bash
git clone https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
```

---

# Subir Ambiente Airflow

```bash
docker compose up -d
```

---

# Inicializar Airflow

```bash
docker compose up airflow-init
```

---

# Acessar Airflow

```text
http://localhost:8080
```

---

# Configuração das Connections no Airflow

As credenciais utilizadas no projeto são configuradas diretamente nas Connections do Apache Airflow.

Acesse:

```text
Admin -> Connections
```

---

# Connection PostgreSQL

Criar uma conexão do tipo `Postgres` utilizando os seguintes parâmetros:

| Campo | Valor |
|---|---|
| Connection Id | `postgres_novadrive` |
| Connection Type | `Postgres` |
| Host | `159.223.187.110` |
| Schema | `novadrive` |
| Login | `etlreadonly` |
| Password | `novadrive376A@` |
| Port | `5432` |

---

# Connection Snowflake

Criar uma conexão do tipo `Snowflake`.

Exemplo:

| Campo | Valor |
|---|---|
| Connection Id | `snowflake_default` |
| Connection Type | `Snowflake` |
| Account | `<SEU_ACCOUNT>` |
| User | `<SEU_USER>` |
| Password | `<SEU_PASSWORD>` |
| Database | `<SEU_DATABASE>` |
| Warehouse | `<SEU_WAREHOUSE>` |
| Schema | `<SEU_SCHEMA>` |

---

# DAG Principal

A DAG principal realiza:

1. Leitura incremental do PostgreSQL
2. Identificação do último ID carregado
3. Extração de novos registros
4. Carga incremental no Snowflake
5. Execução das transformações analíticas

---

# Prints do Projeto

## Airflow DAG

Adicionar screenshot em:

```text
docs/airflow_dag.png
```

---

## Snowflake Tables

Adicionar screenshot em:

```text
docs/snowflake_tables.png
```

---

## dbt Lineage

Adicionar screenshot em:

```text
docs/dbt_lineage.png
```

---

## Dashboard

Adicionar screenshot em:

```text
docs/dashboard.png
```

---

# Melhorias Futuras

- Deploy em ambiente cloud
- Integração CI/CD
- Monitoramento do pipeline
- Testes automatizados dbt
- Incremental Models
- Observabilidade de dados
- Orquestração em Kubernetes

---

# Objetivo do Projeto

Este projeto teve como objetivo praticar conceitos modernos de Engenharia de Dados utilizando ferramentas amplamente utilizadas no mercado para:

- Construção de pipelines ELT
- Data Warehousing
- Analytics Engineering
- Orquestração de dados
- Modelagem dimensional
- Business Intelligence

---

# Autor

Projeto desenvolvido para fins educacionais e portfólio profissional.