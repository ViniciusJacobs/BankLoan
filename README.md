
<!-- README.md is generated from README.Rmd. Please edit that file -->

<img src="imgs/credito.jpg" width="800" height="200" />

# Análise de Crédito

<!-- badges: start -->
<!-- badges: end -->

#### O objetivo desse estudo consiste na análise dos dados históricos de empréstimos e na elaboração de um modelo de classificação para concessão de crédito.

#### Os dados foram coletados no site: <https://www.kaggle.com/zaurbegiev/my-dataset> e armazenados no banco de dados SQL Server 2017.

##### Etapas:

##### 1 - Análise exploratória do conjunto de dados.

##### Pacotes utilizados na análise

``` r
library(odbc)
library(tidyverse)
library(questionr)
library(knitr)
```

##### Estabelecendo conexão com o banco de dados SQL Server 2017, para extração/leitura dos dados.

``` r
con <- dbConnect(odbc(),
                 Driver = "ODBC Driver 17 for SQL Server",
                 Server = "##",
                 Database = "projetobank",
                 UID = "##",
                 PWD = '##',
                 Port ='##')
```

##### Carregando os dados do banco pro R.

``` r
df_bank_train <- dbGetQuery(con, 'SELECT * FROM bank_train')
```

##### Ajustando os nomes das variáveis

``` r
df_bank_train <-  janitor::clean_names(df_bank_train)
```

##### Info dos dados:

##### Foram observadas 100.514 observações e um conjunto de 19 variáveis.

##### A variável target para esse estudo será (loan\_status).

``` r
glimpse(df_bank_train)
#> Rows: 100,514
#> Columns: 19
#> $ loan_id                      <chr> "0305335e-fb77-4ba0-b74a-de817b2fe445", "~
#> $ customer_id                  <chr> "66316030-867a-48fe-befc-8b74dc359582", "~
#> $ loan_status                  <chr> "Fully Paid", "Fully Paid", "Fully Paid",~
#> $ current_loan_amount          <dbl> 99999999, 266244, 222970, 182204, 196526,~
#> $ term                         <chr> "Short Term", "Short Term", "Short Term",~
#> $ credit_score                 <dbl> 699, NA, 716, 7380, 708, 6010, NA, NA, 71~
#> $ annual_income                <dbl> 806949, NA, 577695, 1498720, 565725, 1974~
#> $ years_in_current_job         <chr> "10+ years", "10+ years", "10+ years", "3~
#> $ home_ownership               <chr> "Rent", "Rent", "Home Mortgage", "Rent", ~
#> $ purpose                      <chr> "Debt Consolidation", "Debt Consolidation~
#> $ monthly_debt                 <dbl> 11902.55, 14490.92, 13046.35, 20232.72, 9~
#> $ years_of_credit_history      <dbl> 12.8, 18.6, 20.5, 21.3, 9.8, 17.6, 9.5, 1~
#> $ months_since_last_delinquent <dbl> 40, NA, 41, 30, NA, 5, 6, 76, 7, 17, NA, ~
#> $ number_of_open_accounts      <dbl> 9, 8, 18, 10, 12, 10, 10, 7, 18, 6, 12, 1~
#> $ number_of_credit_problems    <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,~
#> $ current_credit_balance       <dbl> 80693, 207005, 210216, 213769, 113905, 21~
#> $ maximum_open_credit          <dbl> 347336, 394856, 448272, 342848, 181170, 2~
#> $ bankruptcies                 <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,~
#> $ tax_liens                    <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,~
```

##### Existem dados faltantes no conjunto de dados?

``` r
freq.na(df_bank_train)
#>                              missing  %
#> months_since_last_delinquent   53655 53
#> credit_score                   19668 20
#> annual_income                  19668 20
#> bankruptcies                     718  1
#> tax_liens                        524  1
#> maximum_open_credit              516  1
#> loan_id                          514  1
#> customer_id                      514  1
#> loan_status                      514  1
#> current_loan_amount              514  1
#> term                             514  1
#> years_in_current_job             514  1
#> home_ownership                   514  1
#> purpose                          514  1
#> monthly_debt                     514  1
#> years_of_credit_history          514  1
#> number_of_open_accounts          514  1
#> number_of_credit_problems        514  1
#> current_credit_balance           514  1
```

##### Como a maior parte das variáveis apresenta 514 dados faltantes, verifiquei se são linhas inteiras de NA.

##### Visto que o teste resultou em um conjunto de dados faltantes uniformes, optei pelo descarte.

##### A tabela abaixo representa o novo conjunto de dados.

``` r
df_bank_train <- df_bank_train %>% 
                 filter(!is.na(loan_id))

freq.na(df_bank_train)
#>                              missing  %
#> months_since_last_delinquent   53141 53
#> credit_score                   19154 19
#> annual_income                  19154 19
#> bankruptcies                     204  0
#> tax_liens                         10  0
#> maximum_open_credit                2  0
#> loan_id                            0  0
#> customer_id                        0  0
#> loan_status                        0  0
#> current_loan_amount                0  0
#> term                               0  0
#> years_in_current_job               0  0
#> home_ownership                     0  0
#> purpose                            0  0
#> monthly_debt                       0  0
#> years_of_credit_history            0  0
#> number_of_open_accounts            0  0
#> number_of_credit_problems          0  0
#> current_credit_balance             0  0
```

##### Após esse filtro ficamos com dados ausentes nas variáveis

###### 53% months\_since\_last\_delinquent (Meses desde a última inadimplência)

###### 19% credit\_score (score de crédito)

###### 19% annual\_income (renda anual)

###### 204 obs. bankruptcies (falência)

###### 10 obs. tax\_liens (linhas de impostos)

###### 2 obs. maximum\_open\_credit (abertura máxima de crédito)
