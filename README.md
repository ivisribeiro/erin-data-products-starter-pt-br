# {tenant} · Data Products (Silver / Gold)

Projeto **dbt** para Silver / Gold / Data Products. Consome tabelas Bronze
geradas pelo repo `data-ingestion` via `models/bronze/sources.yml`.

## Quickstart

```bash
pip install dbt-databricks dbt-utils
dbt deps
dbt seed
dbt build --select state:modified+
```

## Estrutura

```
models/
  bronze/sources.yml      ← AUTO-gerado quando spec Bronze é aprovada
  silver/                 ← stg_*, dim_*, fct_*  (do usuário)
  gold/                   ← marts e data products
data_products/
  {product_name}.product.yaml   ← consumers, SLA, owner, ODCS
seeds/                    ← UFs, motivos chargeback, refs
tests/                    ← singular tests SQL
macros/                   ← erin_test_freshness.sql, etc.
```

## Princípios

- Bronze é imutável aqui. Você só lê via `{{ source(...) }}`. Se quiser mudar
  schema Bronze, abra PR no repo `data-ingestion`.
- Cada produto Gold tem owner + SLA declarado em `data_products/*.yaml`.
- Tests dbt nativos (unique, not_null, relationships, accepted_values) + dbt-expectations.

## Veja também

- [CONTRIBUTING.md](./CONTRIBUTING.md)
- dbt-elementary dashboard para quality observability
