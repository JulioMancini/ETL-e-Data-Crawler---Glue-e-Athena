# ETL-e-Data-Crawler-Glue-e-Athena
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

Vpu criar aqui uma função de administrador especificamente para o serviço do Glue. A primeiro coisa vou fazer é ir no site da AWS: `https://aws.amazon.com/pt/iam/` e procurar no barra de pesquisa por IAM.

Na barra lateral procurar pela opção funções e ir criar funções

* configuração das Funções

Tipo de Entidade: serviços da AWS
CAso de uso: tem que pesquisar por Glue na barra de pesquisa.

![2](https://github.com/JulioMancini/ETL-e-Data-Crawler---Glue-e-Athena/assets/145502330/288ee1c0-0cca-4cc7-bb9f-ec636a03349a)


agora vou escolher as políticas, vou dar uma pública de administração. Procurar por `Administratoraccess`

![3](https://github.com/JulioMancini/ETL-e-Data-Crawler---Glue-e-Athena/assets/145502330/d4ddfc75-9958-4eba-a7d7-9c3c8f5b8032)

Na proxima pagina renomear a função e finalizar criando ela.

# CRIANDO BUCKETS E PASTAS

* Então, através de um Bucket, você consegue criar um repositório para importar os arquivos.
* entre no site da AWS para criar: https://aws.amazon.com/pt/s3/
* criei um nome unico para o Bucket e mudei a região para São Paulo

![5](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/40c5906f-78e9-4d66-ae7d-1ccad1e0568f)


Com o Bucket criado posso Carregar os arquivos e criar pastas. A primeira pasta vai se chamar de Data Lake, onde vão ficar os dados tranformados. A segunda pasta vai servir para os dados temporários do processamento do Glue. A terceira pasta será onde ele vai salvar o script criado no processo de ETL. A Quarta pasta vou colocar os dados de origem.

![6](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/60c2dcbf-6a8f-4018-af28-72748b3603e7)

Agora é só carregar os arquivos CSV

![7](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/48cbe99f-2558-46c9-9ce8-df8440e2fc84)


# CRIANDO DATABESE

Vou entrar no site da AWS e procurar pelo Glue na barra de pesquisa. `https://aws.amazon.com/pt/glue/`

Bom, a primeira coisa  é criar um banco de dados, um database. Então o processo de criar um database é bem simples. A única informação obrigatória que se precisa é o nome.

![8](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/87bca637-60a9-4419-a006-906232dcc197)

o Database só uma forma de você agrupar logicamente as tabelas. Elas nada mais são do que referências para dados que estão fisicamente em outros locais.

# EXECUTANDO O CRAWLER

A função do Crawler é simples, é fazer uma descoberta do esquema desses dados onde eles estão. Eu poderia ter feito tabelas no proprio Glue mas optei pelo Crawler.

![9](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/e03a6701-3484-4f33-9697-51a99f04273c)

Na próxima etapa vamos adicionar um Data Source. A ferramenta da um leque de opçõs mas nesse caso vou escolher S3, onde está o banco de dados que criamos

![10](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/9c0b7bd9-d4d0-4a5f-ac0e-31216c260661)

No Path vou carregar os arquivos que coloquei no S3, Então, tenho que voltar la no S3 na pasta que criei especialmente para o Crawler atuar, dei o nome de Source Data, onde conta os arquivos também. Só copiar o URI e coloar no Path

![12](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/11d846fa-0bb1-43a9-b5a5-e2862d96c689)

depois de copiado é so colocar o Path

![11](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/b61d8e8d-59fe-4ad6-9860-6df927c2844c)

Na próxima etapa tenho que atribuir uma IAM role que criei. Para que o crawler possa atuar em meu nome e em outros serviços. Nesse casso precisamos que ele atue nos Três.

![13](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/00370bef-9f0e-4a85-af55-8ccb69a96f5a)

* Opções de segurança são opcionais, escolhi não colocar então passei para próxima etapa

Nessa etapa ele pede um database, coloquei o database que criei chamado de Vendas.

![14](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/e0435d80-88c6-4822-b178-5ec7663563f4)

Deixei a opção Frequency sobe demanda ( on demand ) e passei para a próxima e última etapa, que é a de revisão e criação. Apos a criação coloquei ele para executar.


