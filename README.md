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

* Então, através de um Bucket, você consegue criar um repositório para importar os arquivos.
* entre no site da AWS para criar: https://aws.amazon.com/pt/s3/
* crie um nome unico para o Bucket e mude a região para São Paulo

![5](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/40c5906f-78e9-4d66-ae7d-1ccad1e0568f)


Com o Bucket criado podedemos Carregar os arquivos e criar pastas. A primeira pasta vai se chamar de Data Lake, onde vão ficar os dados tranformados. A segunda pasta vai servir para os dados temporários do processamento do Glue. A terceira pasta será onde ele vai salvar o script criado no processo de ETL. A Qaurta pasta vamos colocar os dados de origem.

![6](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/60c2dcbf-6a8f-4018-af28-72748b3603e7)

Agora é só carregar os arquivos CSV

![7](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/48cbe99f-2558-46c9-9ce8-df8440e2fc84)


# CRIANDO DATABESE

Vamos entrar no site da AWS e procurar pelo Glue na barra de pesquisa. `https://aws.amazon.com/pt/glue/`

Bom, a primeira coisa que a gente precisa começar fazendo aqui é criar um banco de dados, um database. Então o processo de criar um database é bem simples. A única informação obrigatória que se precisa é o nome.

![8](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/87bca637-60a9-4419-a006-906232dcc197)

o Database só uma forma de você agrupar logicamente as tabelas. Elas nada mais são do que referências para dados que estão fisicamente em outros locais.

# EXECUTANDO O CRAWLER
