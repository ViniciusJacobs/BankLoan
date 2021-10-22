
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
df_bank_train <- dbGetQuery(con, 'SELECT * FROM bank_test')
```

##### Ajustando os nomes das variáveis

``` r
df_bank_train <-  janitor::clean_names(df_bank_train)
```

##### Info dos dados

``` r
glimpse(df_bank_train)
#> Rows: 10,353
#> Columns: 18
#> $ loan_id                      <chr> "f738779f-c726-40dc-92cf-689d73af533d", "~
#> $ customer_id                  <chr> "ded0b3c3-6bf4-4091-8726-47039f2c1b90", "~
#> $ current_loan_amount          <dbl> 611314, 266662, 153494, 176242, 321992, 2~
#> $ term                         <chr> "Short Term", "Short Term", "Short Term",~
#> $ credit_score                 <dbl> 747, 734, 709, 727, 744, 741, 733, NA, 73~
#> $ annual_income                <dbl> 2074116, 1919190, 871112, 780083, 1761148~
#> $ years_in_current_job         <chr> "10+ years", "10+ years", "2 years", "10+~
#> $ home_ownership               <chr> "Home Mortgage", "Home Mortgage", "Rent",~
#> $ purpose                      <chr> "Debt Consolidation", "Debt Consolidation~
#> $ monthly_debt                 <dbl> 42000.83, 36624.40, 8391.73, 16771.87, 39~
#> $ years_of_credit_history      <dbl> 21.8, 19.4, 12.5, 16.5, 26.0, 13.8, 15.3,~
#> $ months_since_last_delinquent <dbl> NA, NA, 10, 27, 44, NA, NA, NA, NA, 56, 5~
#> $ number_of_open_accounts      <dbl> 9, 11, 10, 16, 14, 6, 42, 9, 2, 8, 4, 12,~
#> $ number_of_credit_problems    <dbl> 0, 0, 0, 1, 0, 0, 0, 0, 0, 2, 0, 0, 0, 0,~
#> $ current_credit_balance       <dbl> 621908, 679573, 38532, 156940, 359765, 25~
#> $ maximum_open_credit          <dbl> 1058970, 904442, 388036, 531322, 468072, ~
#> $ bankruptcies                 <dbl> 0, 0, 0, 1, 0, 0, 0, 0, 0, 2, 0, 0, 0, 0,~
#> $ tax_liens                    <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,~
```

##### Existem dados faltantes no conjunto de dados?

``` r
nas <- freq.na(df_bank_train)
kable(nas)
```

|                                 | missing |   % |
|:--------------------------------|--------:|----:|
| months\_since\_last\_delinquent |    5659 |  55 |
| credit\_score                   |    2334 |  23 |
| annual\_income                  |    2334 |  23 |
| bankruptcies                    |     375 |   4 |
| tax\_liens                      |     354 |   3 |
| loan\_id                        |     353 |   3 |
| customer\_id                    |     353 |   3 |
| current\_loan\_amount           |     353 |   3 |
| term                            |     353 |   3 |
| years\_in\_current\_job         |     353 |   3 |
| home\_ownership                 |     353 |   3 |
| purpose                         |     353 |   3 |
| monthly\_debt                   |     353 |   3 |
| years\_of\_credit\_history      |     353 |   3 |
| number\_of\_open\_accounts      |     353 |   3 |
| number\_of\_credit\_problems    |     353 |   3 |
| current\_credit\_balance        |     353 |   3 |
| maximum\_open\_credit           |     353 |   3 |
