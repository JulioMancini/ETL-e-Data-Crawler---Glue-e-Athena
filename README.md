# ETL-e-Data-Crawler---Glue-e-Athena
Projeto com o objetivo de criar um Datalake com dados de vendas para consulta.

# script do projeto

* Criar IAM role
* Glue
* Criar Banco de Dados
* Criar e executar Crawler
* Criar tabelas associadas ao banco de dados: Glue Data Catalog
* Criar e execurar job
* Tranformar dados: tratamento de nomes de campos / joins / salvar no S3 como parquet com partição (Status)
* criar banco de dados e Crawler para o Data Lake
* Configurar Athena para Realizar Consultas no Data Lake

# IAM PARA O GLUE

Vamos vai criar aqui uma função de administrador especificamente para o serviço do Glue. A primeiro coisa temos que ir no site da AWS: `https://aws.amazon.com/pt/iam/` e procurar no barra de pesquisa por IAM.

Na barra lateral procurar pela opção funções e ir criar funções

* configuração das Funções

Tipo de Entidade: serviços da AWS
CAso de uso: tem que pesquisar por Glue na barra de pesquisa.

![2](https://github.com/JulioMancini/ETL-e-Data-Crawler---Glue-e-Athena/assets/145502330/288ee1c0-0cca-4cc7-bb9f-ec636a03349a)


agora vamos escolher as políticas, vamos dar uma pública de uma política de administração. Procurar por `Administratoraccess`

![3](https://github.com/JulioMancini/ETL-e-Data-Crawler---Glue-e-Athena/assets/145502330/d4ddfc75-9958-4eba-a7d7-9c3c8f5b8032)

Na proxima pagina podemos dar o nome da função e finalizar criando ela.

# CRIANDO BUCKETS E PASTAS











