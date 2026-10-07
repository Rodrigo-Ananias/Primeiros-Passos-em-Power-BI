Descrição do desafio de projeto

1\. Criação de uma instância na Azure para MySQL

2\. Criar o Banco de dados com base disponível no github

3\. Integração do Power BI com MySQL no Azure

4\. Verificar problemas na base a fim de realizar a transformação dos dados



Diretrizes para transformação dos dados

1\. Verifique os cabeçalhos e tipos de dados

2\. Modifique os valores monetários para o tipo double preciso

3\. Verifique a existência dos nulos e analise a remoção

4\. Os employees com nulos em Super\_ssn podem ser os gerentes. Verifique se há algum

colaborador sem gerente

5\. Verifique se há algum departamento sem gerente

6\. Se houver departamento sem gerente, suponha que você possui os dados e preencha

as lacunas

7\. Verifique o número de horas dos projetos

8\. Separar colunas complexas

9\. Mesclar consultas employee e departament para criar uma tabela employee com o

nome dos departamentos associados aos colaboradores. A mescla terá como base a

tabela employee. Fique atento, essa informação influencia no tipo de junção

10\. Neste processo elimine as colunas desnecessárias.

11\. Realize a junção dos colaboradores e respectivos nomes dos gerente . Isso pode ser

feito com consulta SQL ou pela mescla de tabelas com Power BI. Caso utilize SQL,

especifique no README a query utilizada no processo.

&#x09;	OBS: Pesquise a diferença entre Mesclar e Acrescentar/Combinar/ a consulta e explicar o porquê eu não pude usar o acrescentar nessa consulta.

12\. Mescle as colunas de Nome e Sobrenome para ter apenas uma coluna definindo os

nomes dos colaboradores

13\. Mescle os nomes de departamentos e localização. Isso fará que cada combinação

departamento-local seja único. Isso irá auxiliar na criação do modelo estrela em um

módulo futuro.

Explique o porquê de apenas podermos utilizar o mesclar e não o acrescentar consulta.



Respostas



**1. Verificação dos dados (Limpeza, transformação e verificação)**



&#x09;Identificamos a presença de outras colunas relacionadas a outras colunas de diferentes tabelas do banco de dados. Diante dessa situação, fizemos a remoção dessas colunas e deixamos as tabelas sem relacionamento. Conforme a seguinte ação:



Tabela (projeto xxxx), coluna removida > xxxx.



1. tabela projeto dependent, coluna removida > projeto.employee;
2. tabela projeto departamento, colunas removidas > projeto.dept\_locations, projeto.employee(Mgr\_ssn), projeto.employee(Mgr\_ssn)2 e projeto.project;
3. tabela projeto dept\_locations, coluna removida > projeto.departament;
4. tabela projeto employee, colunas removidas > projeto.departament(Ssn), projeto.departament(Ssn) 2, projeto.dependent, projeto.employee(Ssn), projeto.employee(Super\_snn) e projeto.works\_on.
5. tabela projeto Project, colunas removidas > projeto.departament e projeto.works\_on.
6. tabela projeto works\_on, colunas removidas > projeto.employee e projeto.project





&#x09;Em seguida, verificamos os cabeçalhos e os tipos de dados e identificamos e alteramos as seguintes tabelas/colunas:



1. projeto employee/Salary > Tipo Número Decimal alterado para Número Decimal Fixo;



&#x09;Seguimos com a verificação e identificamos na tabela "employee", coluna "Super\_snn" possui um dado **null**. Seguindo as diretrizes (4.), percebemos que se tratava do James E. Borg, possível gerente. Na seguinte diretriz (5.) constatamos que todas os departamentos possuem gerência. Por exemplo: James E. Borg (Headquartes), Jennifer S. Wallace (Administration) e, por fim, Franklin T. Wong (Research).

&#x09;Identificamos na tabela departament a coluna Mgr\_ssn que permite descobrir os gerentes dos respectivos departamentos. Sendo assim, aqueles que possuíam na tabela employee, coluna Super\_snn, o texto "888665555" e valores de Salary >= 40.000 reais potenciais gerentes. Sendo assim, substituímos o valor **null** por "888665555".

&#x09;Além disso, na tabela employee separamos a coluna Adress em 4 colunas (Street Number, Street, City e State) e mesclamos as colunas Fname, Minit e Lname em uma única coluna chamada Name, todas tipo Texto.



**2. Mesclagem das tabelas**



&#x09;1. Mesclamos as tabelas works\_on, Project e employee em uma única tabela, chamada Mescla works\_on x Project x employee. Essa mesclagem permite saber o número de horas investidas em cada projeto, quantidade de funcionários por projeto, tempo investido do funcionário por projeto e horas totais investidas no todo.

&#x09;2. Mesclamos a tabela employee com a tabela departament e criamos a tabela Mescla employee x departament.

&#x09;3. Mesclamos a tabela dependent x employee e identificamos a quantidade de funcionário que possuem dependentes. Em seguida, agrupamos por employee.Name para obter a quantidade de dependentes e nomeamos a coluna como Dependents. Por fim, nomeamos a tabela como Mescla dependent x employee.

&#x09;4. Mesclamos departament x dept-locations e nomeamos como Mescla departament x dept. Que nos mostra a localização do departamento. Usamos a mescla porque não é possível usar o acrescentar. Os dados não possuem a mesma estrutura interna, mas uma coluna-chave entre eles.

&#x09;5. Mesclamos employee x employee para obter junção de gerente por empregado. Foi feito uma mesclagem interna, onde usamos a coluna Super\_ssn (1) e a coluna Ssn para criar a mesclagem. Depois filtramos por Name e reordenamos as colunas. Em seguida, agrupamos por Manager\_Name para obter a quantidade de subordinados por gerente e nomeamos a coluna como Subordinates. Por fim, nomeamos a tabela como Mescla employee x employee (Manager\_Name x Subordinates).



**3.** **Diferença entre Mesclar e Acrescentar Consultas:**



&#x09;**Acrescentar consultas** é um método de organização que permite o empilhamento de linhas da tabela pela vertical. Para que isso ocorra é preciso atender a um critério, sendo ele: as tabelas devem ter estruturas semelhantes ou idênticas (colunas com nome parecidos ou idênticos, mas estrutura interna semelhante). Exemplo: criação da tabela Vendas, usando os relatórios financeiros dos últimos 3 meses (Jan, Fev, Mar - 1 tri de XXXX).

&#x09;**Mesclar consultas** é um outro método de organização que permite unir consultas lado a lado, em outras palavras, realiza a junção de colunas de duas tabelas na horizontal. Porém, para funcionar é necessário que haja uma coluna-chave em comum (por exemplo, adicionar o ID do cliente - presente na tabela cadastro - na tabela Vendas).

&#x09;Sendo assim, foi orientado usar a Mesclagem de consultas pois as tabelas não possuíam estruturas internas semelhantes, mas colunas-chaves em comum, permitindo uma mescla (horizontal) do que uma acréscimo de consulta. Em outras palavras, não eram tabelas de diferentes anos fiscais com a mesma estrutura interna. Eram tabelas distintas uma das outras com colunas-chaves entre elas.

