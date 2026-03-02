# Bike Store Medallion - CSV

Projeto de estudos de Engenharia de Dados com arquitetura Medallion no Databricks Free Edition.
Os dados são gerenciados diretamente no Databricks como managed storage, sem dependência de serviços externos.

## Objetivo
Desenvolver um processo de pipeline de dados, focando nos principais pontos, para aprendizado, sem se preocupar com custos
de cloud. Pense nesse projeto como um "Hello World" de Databricks focado em engenharia de dados. =)

## Fonte de Dados
Arquivos CSV de uma loja de bicicletas fictícia, armazenados em Volume do Unity Catalog.

## Notebooks

| Notebook | Camada | Descrição |
|---|---|---|
| `00_Setup` | - | Cria catálogo, schemas e volume no Unity Catalog |
| `01_Ingestao_Bronze` | Bronze | Lê os CSVs e salva como Delta Tables na camada Bronze |
| `02_Silver_Produtos` | Silver | Produtos enriquecidos com categoria, marca e estoque total |
| `03_Silver_Pedidos` | Silver | Pedidos com status mapeado, vendedor, loja e valor total |
| `04_Silver_Clientes` | Silver | Clientes filtrados com e-mail e telefone válidos |
| `05_Gold_Vendas_Diarias` | Gold | Soma de vendas diárias de pedidos entregues no estado NY |
| `06_Gold_Alertas_Pendentes` | Gold | Clientes com pedidos pendentes para notificação |
| `99_Validacao_Silver` | - | Valida se todas as tabelas Silver foram criadas com sucesso |
| `utils/qualidade_dados` | - | Funções utilitárias reutilizáveis de qualidade de dados |

## Jobs

| Job | Descrição | Agendamento |
|---|---|---|
| `job_bikes` | Pipeline completo Bronze → Silver → Gold | Diário às 08:00 — **pausado** |

### Pipeline
```
ingestao_bronze
├── silver_clientes  ┐
├── silver_pedidos   ├── validate_silver ┬── gold_vendas_diarias
└── silver_produtos  ┘                  └── gold_alertas_pendentes
```

## Como executar
1. Rodar `00_Setup` uma única vez para criar a infraestrutura
2. Fazer upload dos CSVs no Volume `bike_store/landing/raw_files`
3. Executar o job `job_ingestao_bronze` manualmente ou configurar retomada de disparo agendado
