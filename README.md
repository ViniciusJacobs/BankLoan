
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Análise de Crédito

<!-- badges: start -->
<!-- badges: end -->

##### Pacotes utilizados na análise

``` r
library(odbc)
library(tidyverse)
library(questionr)
library(knitr)
```

##### Estabelece a conexão com o banco de dados SQL Server 2017, para extração dos dados.

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

##### Info dos dados

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
nas <- freq.na(df_bank_train)
kable(nas)
```

|                                 | missing |   % |
|:--------------------------------|--------:|----:|
| months\_since\_last\_delinquent |   53655 |  53 |
| credit\_score                   |   19668 |  20 |
| annual\_income                  |   19668 |  20 |
| bankruptcies                    |     718 |   1 |
| tax\_liens                      |     524 |   1 |
| maximum\_open\_credit           |     516 |   1 |
| loan\_id                        |     514 |   1 |
| customer\_id                    |     514 |   1 |
| loan\_status                    |     514 |   1 |
| current\_loan\_amount           |     514 |   1 |
| term                            |     514 |   1 |
| years\_in\_current\_job         |     514 |   1 |
| home\_ownership                 |     514 |   1 |
| purpose                         |     514 |   1 |
| monthly\_debt                   |     514 |   1 |
| years\_of\_credit\_history      |     514 |   1 |
| number\_of\_open\_accounts      |     514 |   1 |
| number\_of\_credit\_problems    |     514 |   1 |
| current\_credit\_balance        |     514 |   1 |
