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

Modelos:

- `stg_clientes`
- `stg_vendas`
- `stg_veiculos`
- `stg_vendedores`
- `stg_concessionarias`
- `stg_cidades`
- `stg_estados`

---

## Dimensions

Camada responsável pelas dimensões analíticas.

Modelos:

- `dim_clientes`
- `dim_veiculos`
- `dim_vendedores`
- `dim_concessionarias`
- `dim_cidades`
- `dim_estados`

---

## Facts

Camada responsável pelas tabelas fato e métricas analíticas.

Modelos:

- `fct_vendas`

---

## Analysis

Camada utilizada para consultas analíticas e exploração dos dados.

Consultas:

- `analise_vendas_concessionaria`
- `analise_vendas_temporal`
- `analise_vendas_veiculo`
- `analise_vendas_vendedor`

---

# Como Executar o Projeto

## Pré-requisitos

- Docker
- Docker Compose
- Python 3.13+
- Conta Snowflake
- Conta dbt
- Conta Google

---

# Clonar Repositório

```bash
git clone https://github.com/guilhermebp030504/projeto-engenharia-dados.git
```

---

# Subir Ambiente Airflow

```bash
docker compose up -d
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

Criar uma conexão do tipo `postgres` utilizando os seguintes parâmetros:

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

# Configuração Snowflake

O Snowflake é utilizado como Data Warehouse principal do projeto.

Criar uma conexão do tipo `snowflake` no Airflow utilizando os dados da sua conta Snowflake.

Exemplo:

| Campo | Valor |
|---|---|
| Connection Id | `snowflake_default` |
| Connection Type | `Snowflake` |
| Account | `<SEU_ACCOUNT>` |
| User | `<SEU_USER>` |
| Password | `<SEU_PASSWORD>` |
| Database | `NOVADRIVE` |
| Warehouse | `DEFAULT_WH` |
| Schema | `STAGE` |

---

# Script de Criação Snowflake

Executar o script abaixo no Snowflake para criação do ambiente utilizado no projeto.

```sql
CREATE DATABASE novadrive;

CREATE SCHEMA stage;

CREATE WAREHOUSE DEFAULT_WH;

CREATE TABLE veiculos (
    id_veiculos INTEGER,
    nome VARCHAR(255) NOT NULL,
    tipo VARCHAR(100) NOT NULL,
    valor DECIMAL(10, 2) NOT NULL,
    data_atualizacao TIMESTAMP_LTZ,
    data_inclusao TIMESTAMP_LTZ
);

CREATE TABLE estados (
    id_estados INTEGER,
    estado VARCHAR(100) NOT NULL,
    sigla CHAR(2) NOT NULL,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);

CREATE TABLE cidades (
    id_cidades INTEGER,
    cidade VARCHAR(255) NOT NULL,
    id_estados INTEGER NOT NULL,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);

CREATE TABLE concessionarias (
    id_concessionarias INTEGER,
    concessionaria VARCHAR(255) NOT NULL,
    id_cidades INTEGER NOT NULL,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);

CREATE TABLE vendedores (
    id_vendedores INTEGER,
    nome VARCHAR(255) NOT NULL,
    id_concessionarias INTEGER NOT NULL,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);

CREATE TABLE clientes (
    id_clientes INTEGER,
    cliente VARCHAR(255) NOT NULL,
    endereco TEXT NOT NULL,
    id_concessionarias INTEGER NOT NULL,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);

CREATE TABLE vendas (
    id_vendas INTEGER,
    id_veiculos INTEGER NOT NULL,
    id_concessionarias INTEGER NOT NULL,
    id_vendedores INTEGER NOT NULL,
    id_clientes INTEGER NOT NULL,
    valor_pago DECIMAL(10, 2) NOT NULL,
    data_venda TIMESTAMP_LTZ,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);
```

---

# Configuração dbt

O projeto dbt está localizado na pasta:

```text
dbt/
```

O dbt é responsável pelas transformações analíticas e modelagem dimensional dos dados armazenados no Snowflake.

---

# Executar dbt

## Instalar dependências

```bash
dbt deps
```

---

## Testar conexão

```bash
dbt debug
```

---

## Executar transformações

```bash
dbt run
```

---

## Executar testes

```bash
dbt test
```

---

# DAG Principal

A DAG principal realiza:

1. Leitura incremental do PostgreSQL
2. Identificação do último ID carregado
3. Extração de novos registros
4. Carga incremental no Snowflake
5. Execução das transformações analíticas utilizando dbt

---

# Fluxo Completo do Pipeline

1. Extração incremental dos dados via PostgreSQL
2. Orquestração utilizando Apache Airflow
3. Carga dos dados no Snowflake
4. Transformações analíticas utilizando dbt
5. Consumo analítico via Google Data Studio

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
