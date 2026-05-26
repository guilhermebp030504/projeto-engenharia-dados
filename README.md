# Pipeline de Engenharia de Dados com Airflow, Snowflake, dbt e Data Studio

## Sobre o Projeto

Este projeto foi desenvolvido como parte de um estudo prático de Engenharia de Dados, utilizando uma arquitetura moderna de pipeline ELT com orquestração, armazenamento em Data Warehouse, transformação de dados e visualização analítica.

O pipeline realiza:

- Extração de dados de um banco PostgreSQL remoto
- Orquestração das cargas utilizando Apache Airflow
- Armazenamento dos dados no Snowflake
- Transformações e modelagem analítica com dbt
- Criação de dashboards no Google Data Studio

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
├── airflow/
│   ├── dags/
│   ├── config/
│   ├── plugins/
│   └── docker-compose.yaml
│
├── dbt/
│   ├── models/
│   ├── macros/
│   ├── tests/
│   ├── snapshots/
│   └── dbt_project.yml
│
├── docs/
│
├── .env.example
├── .gitignore
└── README.md
```

---

# Pipeline ETL

## Extração

Os dados são extraídos de um banco PostgreSQL remoto utilizando operadores Python no Apache Airflow.

## Carga

Os dados são carregados incrementalmente no Snowflake, utilizado como Data Warehouse principal do projeto.

## Transformação

As transformações são realizadas com dbt utilizando:

- Staging Layer
- Dimensional Modeling
- Fact Tables
- Analytical Views

## Visualização

Os dados transformados são consumidos no Google Data Studio para criação de dashboards analíticos.

---

# Modelagem de Dados

O projeto utiliza modelagem dimensional contendo:

## Tabelas Fato

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

## Staging

Responsável pela padronização e limpeza inicial dos dados.

Exemplos:
- `stg_clientes`
- `stg_vendas`
- `stg_veiculos`

## Dimensions

Responsável pelas dimensões analíticas.

## Facts

Responsável pelas métricas de negócio e consolidação analítica.

## Analysis

Camada utilizada para consultas analíticas e exploração de dados.

---

# Como Executar o Projeto

## Pré-requisitos

- Docker
- Docker Compose
- Python 3.13+
- Conta Snowflake
- Conta dbt Cloud (opcional)

---

## Clonar Repositório

```bash
git clone https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
```

---

## Configurar Variáveis de Ambiente

Criar arquivo `.env` baseado no `.env.example`

Exemplo:

```env
POSTGRES_HOST=
POSTGRES_USER=
POSTGRES_PASSWORD=

SNOWFLAKE_ACCOUNT=
SNOWFLAKE_USER=
SNOWFLAKE_PASSWORD=
SNOWFLAKE_DATABASE=
```

---

## Subir Ambiente Airflow

```bash
docker compose up -d
```

---

## Acessar Airflow

```text
http://localhost:8080
```

---

# DAG Principal

A DAG principal realiza:

1. Leitura incremental do PostgreSQL
2. Identificação do último ID carregado
3. Extração de novos registros
4. Carga no Snowflake
5. Execução das transformações dbt

---

# Prints do Projeto

## Airflow DAG

Adicionar screenshot em:

```text
docs/airflow_dag.png
```

---

## Snowflake

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
- Orquestração com Kubernetes
- Incremental models no dbt
- Observabilidade de dados

---

# Objetivo do Projeto

Este projeto teve como objetivo praticar conceitos modernos de Engenharia de Dados utilizando ferramentas amplamente utilizadas no mercado para:

- Orquestração de pipelines
- Data Warehousing
- Transformação de dados
- Modelagem dimensional
- Analytics Engineering
- Business Intelligence

---

# Autor

Projeto desenvolvido para fins educacionais e portfólio profissional.