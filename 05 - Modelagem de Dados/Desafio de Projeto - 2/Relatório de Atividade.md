Relatório de ações feitas para a produção do StarSchema com a Tabela Financials.



**Formatação de Tabelas:**



* Renomeação da tabela financials para financials\_origem. Com isso, definimos essa tabela como fonte dos dados, o backup.



* Criação da tabela da tabela D\_Produtos a partir da tabela financial origem.

  1. Foi feito uma agregação(agrupamento avançado) pela coluna Product, onde foram adicionadas as agregações: Valor máximo (Máx./Sale Price), Valor Mín. (Mín./Sale Price), Média do valor de vendas (Média/Sale Price) , Mediana do valor de vendas (Mediana/Sale Price) e; Média de Unidades Vendidas (Média/UnitsSold).
  2. Criado uma coluna índice chamado ID\_Produtos.
  3. Reordenado a ordem das colunas, onde a coluna Índice moveu para a primeira posição.



* Criação da tabela dimensão D\_Produtos\_Detalhes a partir da financials\_origem

  1. Remoção de todas as colunas, com exceção de Discount Band, Sale Price,  Units Sold, Manufactoring Price.
  2. Nova Coluna Condicional, ID\_Produtos.

     1. Criada a partir do mesmo passo anterior.
  3. Reordenamento das colunas.
  4. Criação e reordenamento da coluna Índice (ID\_Produtos).



* Criação da tabela D\_Descontos a partir da financials\_origem.

  1. Remoção de todas as colunas, com exceção de Product, Discount Band e Discounts. (Para agilizar esse processo usei o atalho de seleção SHIFT + Clique esquerdo do mouse e depois apertei em excluir colunas).
  2. Criação de uma Coluna condicional (ID\_Produtos)

     1. Se Product é igual a Carretara, então 0 e assim por diante.
     2. Reordenamento da coluna ID\_Produtos para primera posição e exclusão da coluna Product.
  3. Remoção de Duplicatas

     1. Selecionamos as 3 colunas (ID\_Produtos, Discount Band e Discounts) e removemos as duplicatas.



* Criação da tabela D\_Detalhes a partir da tabela financials\_origem.

  1. Exclusão das seguintes colunas: Product, Discount Band, Discounts, Units Sold, Manufacturing Price e Sale Price. 



* **Criação da tabela D\_Calendário com uso de DAX**

  1. Nova tabela com a função = CALENDAR(start date, end\_date)

     1. D\_Calendário = CALENDAR(DATE(2013,09,01), DATE(2014,12,01))
  2. Inclusão da coluna Year, com a função = Year()

     1. Year = YEAR('D\_Calendário'\[Date])
  3. Adição da coluna Month com a função = Month()

     1. Month = MONTH('D\_Calendário'\[Date])
  4. Utilização da função Format para obter o MonthName

     1. MonthName = FORMAT(DATE(1,'D\_Calendário'\[Month],1),"MMM")
  5. Busca do trimestre, com a função = Quarter()

     1. Quarter = QUARTER('D\_Calendário'\[Date])



* Criação da tabela fato F\_Vendas a partir da original

  1. Exclusão das colunas Manufacturing Price, COGS, Month Number, Gross Sales, Discounts.
  2. Reordenamento das colunas.
  3. Criação do índice SK\_ID
  4. Criação da coluna condicional ID\_Produtos e reordenamento de sua posição.



**Formulação dos Relacionamentos (Modelagem em StarSchema)**

Depois de ter criado e formatado todas as tabelas do exercício criamos a modelagem StarSchema do projeto. Fizemos 5 relacionamentos:



* D\_Produtos\[Product] 1:\* F\_Vendas\[Product]
* 
* D\_Calendário\[Date] 1:\* F\_Vendas\[Date]



* D\_Produtos\_Detalhes\[ID\_Produtos] \*:\* F\_Vendas\[ID\_Produtos]



* D\_Descontos\[Discount Band] \*:\* F\_Vendas\[Discount Band]



* D\_Detalhes\[Country] \*:\* F\_Vendas\[Country]

&#x20;



