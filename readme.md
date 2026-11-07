# Projeto AWS → Fabric → Power BI

Projeto de portfólio pessoal que simula um pipeline de dados completo em
nuvem, do dado bruto até o relatório final, com foco em governança: acesso
separado por perfil e um único modelo semântico servindo todos os relatórios.

## Objetivo

Praticar ferramentas que eu ainda não dominava (AWS) em um cenário próximo da rotina de um analista de dados, cobrindo
ingestão, transformação, catalogação, modelagem e visualização.

## Arquitetura

AWS (IAM + S3 + RDS + Glue + Athena) → Microsoft Fabric (Lakehouse via
Shortcut + Notebook + Pipeline + Modelo Semântico) → Power BI

## Etapas

### 1. Ambiente AWS

**IAM: usuário e função**
- Toda a configuração da infraestrutura (bucket, banco, jobs e crawler) foi
  feita pela conta root.
- Criei um usuário dedicado (`pedro_silva_projeto`), usado apenas para rodar
  as consultas no Athena. Assim, o perfil de análise fica separado da conta
  de administração.
- Criei uma **função (role) do IAM** para o AWS Glue, concedendo ao serviço
  permissão de ler e gravar no S3 durante a execução do job e do crawler.

**S3: data lake**
- Bucket `lake-projeto-pedro` organizado em pastas, separando o dado bruto
  (`dados/raw`) do dado tratado (`dados/etl_dados_processados`).

**RDS (Aurora PostgreSQL), prática de provisionamento**
- Provisionei uma instância Aurora PostgreSQL (`pedro-banco-aprendizado`)
  para aprender o processo de criação e configuração de um banco no RDS.
- Conectei pelo **DBeaver**, usando o endpoint da instância e as
  credenciais definidas no RDS, e criei uma tabela de teste. A instância
  não faz parte do fluxo de dados do projeto.

### 2. Pipeline de dados (Glue + Athena)

- **AWS Glue (Visual ETL):** job `job_projeto_pedro_aws` com o fluxo
  origem no S3 → remoção de duplicatas → remoção de colunas → gravação do
  resultado em parquet no S3.
- **Crawler do Glue:** catalogação automática da tabela tratada no Data
  Catalog, sem criar o schema manualmente.
- **Amazon Athena:** consultas SQL de validação e análise direto sobre os
  arquivos no S3, como máximo, mínimo e média salarial por senioridade.

### 3. Microsoft Fabric

- **Lakehouse** conectado aos dados do S3 por **shortcut (atalho)**, sem
  copiar nem duplicar o armazenamento.
- **Notebook** responsável pela atualização e transformação dos dados.
- **Pipeline** orquestrando a atualização de ponta a ponta.
- **Modelo semântico** criado direto no Fabric, sem depender do Power BI
  Desktop. Em um cenário real, todos os relatórios se conectam a esse mesmo
  modelo, o que evita indicadores com métricas definidas de formas
  diferentes.

### 4. Power BI

- Relatório conectado ao vivo ao modelo semântico do Fabric, sem importar
  dados para dentro do arquivo.
- Análises de total de colaboradores, salário total e médio, por
  senioridade, porte de empresa e empresa.

## Tecnologias

AWS IAM · S3 · RDS/Aurora (PostgreSQL) · Glue · Athena · DBeaver ·
Microsoft Fabric (Lakehouse, Shortcuts, Notebook, Pipeline, Modelo
Semântico) · Power BI

## Demonstração

Vídeo de 3 minutos mostrando neste repositório