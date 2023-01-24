# Projeto de ETL de dados comerciais 📈
 ETL de dados com Data Factory.

O objetivo do projeto foi atender a solicitação de um cliente em migrar cargas de trabalho e reestruturar seu banco de dados, facilitando as análises das variáveis de negócio.

O processo se deu em Migrar cargas de trabalho que estavam no <a href="https://nifi.apache.org/">Apache Nifi</a> para o <a href="https://azure.microsoft.com/pt-br/free/databricks/?&ef_id=CjwKCAiAoL6eBhA3EiwAXDom5tPEYhTu9MkzpehHAc7z6uWZeyIKNV5PC4JuU97p_6UCBo1rKwTRgRoCF90QAvD_BwE:G:s&OCID=AIDcmmzmnb0182_SEM_CjwKCAiAoL6eBhA3EiwAXDom5tPEYhTu9MkzpehHAc7z6uWZeyIKNV5PC4JuU97p_6UCBo1rKwTRgRoCF90QAvD_BwE:G:s&gclid=CjwKCAiAoL6eBhA3EiwAXDom5tPEYhTu9MkzpehHAc7z6uWZeyIKNV5PC4JuU97p_6UCBo1rKwTRgRoCF90QAvD_BwE">Azure Databricks</a>. Além disso, estruturar o Data Warehouse.
O objetivo é melhorar o processamento dos dados, facilitando a geração de relatórios gerenciais, onde
o cliente pretende ter 4 visões, a saber:

* Ver o acumulado de vendas do último ano por região e país;
* Visualizar a quantidade de vendas dos útimos 10 dias;
* Enxergar a quantidade de vendas e a quantidade acumulada de vendas dos últimos 30 dias;
* E ter uma visão acumulada das vendas do último ano por canal e país, podendo analisar a venda
do país selecionado por canal de vendas.

Inicialmente os dados foram migrados em formato .csv para o <a href="https://azure.microsoft.com/en-us/products/storage/blobs/?&ef_id=CjwKCAiAoL6eBhA3EiwAXDom5uK6OvefZQqZmSeysc74ATyOVgFIZCPlcBrZUXO9aggFS-3y1gSOyhoCcM8QAvD_BwE:G:s&OCID=AIDcmmzmnb0182_SEM_CjwKCAiAoL6eBhA3EiwAXDom5uK6OvefZQqZmSeysc74ATyOVgFIZCPlcBrZUXO9aggFS-3y1gSOyhoCcM8QAvD_BwE:G:s&gclid=CjwKCAiAoL6eBhA3EiwAXDom5uK6OvefZQqZmSeysc74ATyOVgFIZCPlcBrZUXO9aggFS-3y1gSOyhoCcM8QAvD_BwE">Azure Blob Storage</a>. 

Após a extração, os dados foram tratados renomeando-se as colunas, convertendo-se algumas colunas string em datetime, interger, subistituindo vírgula por ponto. As colunas que não necessitaram de conversão de tipo, foram mantidas como string. Após tratamento, os dados foram disponibilizados no data warehouse para disponibilidade analítica em formato de dashboard.

Linguagem utilizada | Descrição do Projeto | Ferramentas utilizadas 
---|---|---
<a href="https://www.python.org/">Python</a> e SQL | ETL de dados e criação de Dashboard para análise dos requisitos de negócio | <a href="https://azure.microsoft.com/en-us/products/storage/blobs/?&ef_id=CjwKCAiAoL6eBhA3EiwAXDom5uK6OvefZQqZmSeysc74ATyOVgFIZCPlcBrZUXO9aggFS-3y1gSOyhoCcM8QAvD_BwE:G:s&OCID=AIDcmmzmnb0182_SEM_CjwKCAiAoL6eBhA3EiwAXDom5uK6OvefZQqZmSeysc74ATyOVgFIZCPlcBrZUXO9aggFS-3y1gSOyhoCcM8QAvD_BwE:G:s&gclid=CjwKCAiAoL6eBhA3EiwAXDom5uK6OvefZQqZmSeysc74ATyOVgFIZCPlcBrZUXO9aggFS-3y1gSOyhoCcM8QAvD_BwE">Azure Blob Storage</a>, <a href="https://azure.microsoft.com/pt-br/products/databricks">Azure Databrics</a>, <a href="https://azure.microsoft.com/pt-br/products/data-factory/">Azure Data Factory (orquestração)</a>, <a href="https://azure.microsoft.com/pt-br/services/sql-database/campaign/">Azure SQL Server</a> e <a href="https://powerbi.microsoft.com/pt-br/">Power BI</a>.

A Figura 1 apresenta a arquitetura da solução proposta levando em consideração o levantamento de
requisitos e entendimento do negócio.

<img width="737" alt="arquitetura lab3" src="https://user-images.githubusercontent.com/98350733/214310251-3c4eeb39-8de9-402e-a6e3-cedb13ce8629.png">

Para fazer a extração dos dados, que se encontravam armazenados dentro de um contâiner no Azure Blob Storage, foi utilizado um notebook do Databricks, que utiliza pyspark para o processamento distribuído a partir de clusterização. A imagem abaixo mostra os dados já armazenados dentro do meu contâiner, no Azure Blob Storage.

<img width="960" alt="blob Storage" src="https://user-images.githubusercontent.com/98350733/214313259-78f640ad-c2aa-47d5-b505-5d52c842d516.png">

Para que essa integração ocorra, dentro do notebook do Databricks, foi necessário criar um mount point que faz essa conexão com o blob storage. Para desenvolver o código, fiz importações de pacotes do pyspark para converter os dados para um dataframe e utilizar as bibliotecas para o tratamento dos dados bruto para dados legíveis para apresentação posterior. Além disso, faço a confguração para que o Notebook seja conectado com o banco de dados do Azure SQL Server. E por fim, configuro o unmount para o encerramento entre blob e databricks. 


No exemplo abaixo, verifico como os dados estão dispostos utilizando um dataframe.

<img width="960" alt="Databricks" src="https://user-images.githubusercontent.com/98350733/214312258-f1a6a61e-221d-4eed-994f-33315569a974.png">

Com os dados tratados, crio uma tabela Stage dentro do Azure SQL Server, onde à populo com os dados extraídos.

<img width="287" alt="tabelaStage" src="https://user-images.githubusercontent.com/98350733/214315210-c7a33317-d02f-4943-a359-9b23c515c0e1.png">

Para facilitar a disponibilizade dos dados para análise com o Power BI, dentro do Data Warehouse crio uma tabela fato e suas tabelas dimensão, utilizando o conceito de modelagem multidimensional star schema. Abaixo, as tabelas no DW.

<img width="298" alt="tabelasDW" src="https://user-images.githubusercontent.com/98350733/214316212-fb29fe56-334f-44d8-a746-593479ad8503.png">

Para orquestrar esse processo de extração, tratamento e carregamento dos dados para o DW, utilizo o Data factory, criando uma pipeline de dados. Utilizo dentro do data factory um caderno do databricks e crio um procedimento armazenado para que toda vez que os dados sejam extraídos, a tabela Stage e as tabelas do DW sejam truncadas e os dados não sejam duplicados. Abaixo segue a pipeline depurada com sucesso.

<img width="960" alt="data factory" src="https://user-images.githubusercontent.com/98350733/214320643-0aa738a6-8f28-4976-8aec-032ed807bbec.png">

Após a disponibilidade dos dados dentro do DW, a última etapa foi atender as solicitações do cliente para vizualizar esses dados. Sendo assim, segue abaixo o que foi solicitado e seus respectivos relatórios:

- [x] No dashboard abaixo, foram criados 4 gráficos e 5 filtros:

 <img width="671" alt="Dash" src="https://user-images.githubusercontent.com/98350733/214322016-eebce9d6-a29b-423e-a24e-5194a727b3d8.png">
 
- [x] Gráfico de linhas: Com este gráfico é possivel vizualizar a receita total ao longo do tempo. No dashboard, o gráfico exibe as informações dispostas no filtro data do pedido que compreende os pedidos entre 28/07/2016 e 28/07/2017;
 
- [x] Mapa mundi: Neste modelo de relatório, é possível vizualizar o acumulado de vendas por região e País conforme imagem abaixo. Neste relatório a primeira imagem mostra o acumulado de vendas por região. Na segunda imagem, o acumulado de vendas por país. No relatório é possível fazer Drill-down e Drill-up.
 
 <img width="860" alt="mapaRegiao" src="https://user-images.githubusercontent.com/98350733/214323412-fd2b9e9d-54c3-470d-86e7-41eefcbf0907.png">
 
 <img width="857" alt="mapaPais" src="https://user-images.githubusercontent.com/98350733/214323443-37d83168-af7c-4197-9bab-0e7cc21ed77b.png">
 
- [x] Gráfico de colunas número de vendas por dia: Neste gráfico é possível vizualizar o número de
vendas nos últimos 10 dias , filtrando a data do pedido informando as datas entre 18/07/2017 e
28/07/2017, conforme abaixo, e o total é destacado em forma de cartão no canto superior direito,
o que mostra o total de vendas de 249 mil unidades vendidas, conforme a segunda imagem abaixo.

 <img width="857" alt="vendas por dia" src="https://user-images.githubusercontent.com/98350733/214325153-e839aeee-b73e-493b-8ea4-523e577d8e4a.png">
 
 <img width="661" alt="vendas por dia total" src="https://user-images.githubusercontent.com/98350733/214325194-456e75c0-2c18-4cdc-b574-5b2d22c78a3b.png">
 
- [x] Cartões informativos: Uma das solicitações foi vizualizar a receita e a quantidade de vendas dos
últimos 30 dias. Para isso, usando o filtro de data do pedido seleciona-se o período entre 28/06/2017
e 28/07/2017, o que retorna as informações destacadas na figura abaixo:

 <img width="670" alt="30dias" src="https://user-images.githubusercontent.com/98350733/214325679-274bacf1-d853-4abc-8bb4-edc10eb61237.png">
 
- [x] Tabela: Neste relatório, expõem-se as vendas do último ano por canal e país. Utilizando o filtro
de canal e país, é possível selecionar especificamente o que se pretende verificar. Na figura,
exemplifica-se filtrando o canal online, analizando a tabela por país, mantendo o filtro de data do
pedido conforme a imagem:

 <img width="281" alt="vendasUltimoAno" src="https://user-images.githubusercontent.com/98350733/214326666-9bba1b02-6417-481b-9e55-ca2e11f4c5bc.png">

Para concluir, foi entregue a documentação do projeto utilizando a plataforma <a href="https://pt.overleaf.com/">Overleaf</a>, que utiliza linguaguem <a href="https://pt.wikipedia.org/wiki/LaTeX">LaTeX</a>.





 
 



