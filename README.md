# Bike Store Medallion - CSV

Projeto de estudos de Engenharia de Dados com arquitetura Medallion no Databricks Free Edition.
Os dados são gerenciados diretamente no Databricks como managed storage, sem dependência de serviços externos.

## Objetivo
Desenvolver um processo de pipeline de dados, focando nos principais pontos, para aprendizado, sem se preocupar com custos
de cloud. Pense nesse projeto como um "Hello World" de Databricks focado em engenharia de dados. =)

## Fonte de Dados
Arquivos CSV de uma loja de bicicletas fictícia, armazenados em Volume do Unity Catalog.

## Arquitetura
```
bike_store/
├── landing/raw_files   → Volume com os CSVs originais
├── bronze/             → Tabelas brutas ingeridas dos CSVs
├── silver/             → Tabelas limpas e tratadas
└── gold/               → Tabelas agregadas para análise
```

## Notebooks

| Notebook | Descrição |
|---|---|
| `00_Setup` | Cria catálogo, schemas e volume no Unity Catalog |
| `01_Ingestao_Bronze` | Lê os CSVs e salva como Delta Tables na camada Bronze |
| `02_Tratamento_Silver` | Limpeza e padronização dos dados |
| `03_Agregacao_Gold` | Agregações e regras de negócio |

## Jobs

| Job | Notebook | Agendamento |
|---|---|---|
| `job_ingestao_bronze` | `01_Ingestao_Bronze` | Diário **pausado** |

## Como executar
1. Rodar `00_Setup` uma única vez para criar a infraestrutura
2. Fazer upload dos CSVs no Volume `bike_store/landing/raw_files`
3. Executar o job `job_ingestao_bronze` manualmente ou liberar o agendamento e aguardar