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

# Modelo Conceitual

![modelo dos dados](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/cf31c01b-6930-480e-9686-7ae14effe5fb)


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

# CRIANDO O JOB

Agora vou fazer a transformação dos dados. Para isso vou na área do Glue integração de dados e ETL, na opção Jobs. Nessa ferramenta ele abre uma interface onde eu posso escolher a forma que vou criar o job e a forma de transformação.
Nessa opção da ferramenta tenho alguma opções para o processo de ETL como: editor de código do Spark, editor de código do do Python, Jupyter Notebook, mas vou utilizar o Editor visual com Canvas em branco. Escolhi essa opção pela sua flexibilidade.

* O meu objetivo é pegar as cinco tabelas e criar uma tabela única.

![15](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/7eb0a2da-794f-4f90-802d-48aadc363960)

O primeiro passo é indicar a fonte dos dados, nesse caso a origem aqui é uma tabela que vem do catálogo de dados do Glue.

![16](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/3849e648-02ac-4212-855e-fd1d51b8ba36)


Cada elemento criados nessa ferramenta são chamados de nó, para cada nó eu tenho propriedades dos dados e o esquema de entrada e saída. O primeiro nó vou chamar de vendas e informar a fonte dos dados. Nesse caso vou informar o Data base que eu criei chamado de Vendas. Ou seja, todas as tabelas estão associadas a Vendas. A tabela desse primeiro nó vai ser o Vendas CSV.

![17](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/2728b496-01ce-4505-8dad-7ef01fcccefb)

Para esse nó vou adicionar um elemento de transformação, Como é um pipeline os elementos ficam vinculados entre eles. (Então eu vou selecionar para essa opção a opção de Apply Mapping).

* Observação sobre a ferramenta: É importante manter o elemento selecionado para ele fazer o link entre eles automático.

![18](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/809abae9-03ad-4c93-b98c-2746225eb06e)

Uma observação muito importante.  É que eu posso ter problemas com os jobs quando eu tiver campos repetidos. E se você olhar o nosso esquema de dados, vamos ter nomes repetidos quando  tem chave estrangeira. Então, quando eu fizer um job pode ter problema com o campo dos nomes que vai ser o mesmo. Para resolver isso vou alterar os nomes das colunas desse campo nas tabelas.

Então, no Apply Mapping renomeei para Vendas Mapping e mudei o nome do Idvendedor e Idcliente

![19](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/2e2fefc9-9c8c-47bf-bcca-d346aa74668a)

Então eu cliquei  em uma área branca do workflow pra ele não vincular com nada. E  adicionei  uma nova origem que chamei de itens venda. Mantive o mesmo processo do nó anterior. 

![20](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/89071b84-aa98-4e6d-bcbb-2731530dc332)

Novamente no Apply Mapping renomeei duas colunas diferentes, renomeei o Idproduto e o Idvenda.

![21](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/5a05c639-0637-4c02-a590-2e103720ed1f)

A próxima etapa são os Joins, vou fazer o join de vendas com clientes, o jovem de vendas com o itensvendas vendas, o join de vendas com vendedores e o join de itens venda com produtos. Vou fazer o join a partir dos dados já transformados. 

O primeiro join vai ser a partir do vendasMapping. Essa ferramenta é muito fácil de utilizar. basta manter o vendasmapping selecionado ir em transform e procurar por join.

![22](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/6c91d4c4-1989-4c1e-8292-2f5675b1c420)


Agora vou configurar ele, na aba Node Properties eu renomeei para joinvendas_itensvendas. 
É importante saber que por ser um join, eu tenho que selecionar dois nós, porque ele vai fazer o join entre esses dois nós. Então nessa mesma aba na opção Node parentes selecionei o intensvendaMapping. A próxima opção de configuração se chama: transforms. Essa opção serve para selecionar o tipo de join que vou utilizar, nesse caso vou utilizar o inner join. Também tenho que adicionar uma condição. Ou seja, ele vai fazer junção do join através de dois campos. 

![23](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/adc42365-8a75-4199-8b77-dc3bfb202c13)


A próxima opção de configuração se chama: transforms. Essa opção serve para selecionar o tipo de join que vou utilizar, nesse caso vou utilizar o inner join. Também tenho que adicionar uma condição. Ou seja, ele vai fazer junção do join através de dois campos. 

CONFIGURAÇÃO DO SOURCE (AWS GLUE DATA CATALOG) Produtos
| NODE PROPERTIES | DATA SOURSE PROPERTIES – DATA CATALOG
|--|--|
| NOME: VEndas| DATABASE: Vendas|
|NODE TYPE: AWS Glue Data Catalog| TABLE: Vendas_CSV|

**CONFIGURAÇÃO do JOIN VENDAS**

| NODE PARENTES | TRANSFORMS |
|-|-|
|**NAME**: joinvendas_itensvendas| **JOIN TYPE**: Inner Join|
|**NODE TYPE**: Inner Join| **JOIN CONDITIONS**: idvenda e idvenda_itensvendas |
|**NODE PARENTES**: VendasMapping e intensvendaMapping|-------------------|

![24](https://github.com/JulioMancini/ETL-e-Data-Crawler-Glue-e-Athena/assets/145502330/92111da0-e175-4af4-937c-e554f2c32221)


O próximo nó vou seguir o processo dos anteriores. Vou no SOURCE  procuro a opção AWS GLUE DATA CATALOG. Para configurar  basta renomear, definir o Database e a respectiva tabela com o nome de Clientes_CSV. Feito isso agora é so adicionar uma transformação (join) unindo com joinvendas_intesvendas 

CONFIGURAÇÂO DE Clientes

CONFIGURAÇÃO DO SOURCE (AWS GLUE DATA CATALOG) Clientes
| NODE PROPERTIES | DATA SOURSE PROPERTIES – DATA CATALOG
|--|--|
| NOME: Clientes| DATABASE: Vendas|
|NODE TYPE: AWS Glue Data Catalog| TABLE:  Clientes_CSV|

CONFIGURAÇÃO do JOIN CLIENTES
| NODE PARENTES | TRANSFORMS |
|-|-|
|NAME: Joinclientes |JOIN TYPE: Inner Join|
|NODE TYPE: Inner Join|JOIN CONDITIONS: Idcliente e Idclientes_vendas |
|NODE PARENTES: Clientes e Joinvendas_itensvendas|-------------------|

Agora fica faltando Produtos e vendedores, vou continuar na mesma linha de configuração. 


**CONFIGURAÇÂO de PRODUTOS**

CONFIGURAÇÃO DO SOURCE (AWS GLUE DATA CATALOG) Produtos
| NODE PROPERTIES | DATA SOURSE PROPERTIES – DATA CATALOG
|--|--|
| NOME: Produtos| DATABASE: Vendas|
|NODE TYPE: AWS Glue Data Catalog| TABLE:  Produtos_CSV|

CONFIGURAÇÃO do JOIN PRODUTOS
| NODE PARENTES | TRANSFORMS |
|-|-|
|NAME: Joinprodutos |JOIN TYPE: Inner Join|
|NODE TYPE: Inner Join|JOIN CONDITIONS: Idproduto e Idproduto_intesvendas |
|NODE PARENTES: Produtos e Joinclientes |-------------------|

**CONFIGURAÇÂO de VENDEDORES**

CONFIGURAÇÃO DO SOURCE (AWS GLUE DATA CATALOG) VENDEDORES
| NODE PROPERTIES | DATA SOURSE PROPERTIES – DATA CATALOG
|--|--|
| NOME: Vendedores| DATABASE: Vendas|
|NODE TYPE: AWS Glue Data Catalog| TABLE:  Vendedores_CSV|

CONFIGURAÇÃO do JOIN VENDEDORES
| NODE PARENTES | TRANSFORMS |
|-|-|
|NAME: Joinvendedores |JOIN TYPE: Inner Join|
|NODE TYPE: Inner Join|JOIN CONDITIONS: Idvendedor e Idvendedor_vendas |
|NODE PARENTES: Vendedores e Joinprodutos|-------------------| 



