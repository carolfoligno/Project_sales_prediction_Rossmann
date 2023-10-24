<h1 style="text-align: center;">Previsão de Vendas das Lojas Rossmann</h1>

![](https://drive.google.com/uc?id=1J6vVNu-K-iEDaP8rCXKfkJtXmuVxrC6B)

Este projeto foi orientado pela [Comunidade DS](https://comunidadeds.com/), utilizando os dados disponíveis no [Kaggle](https://www.kaggle.com/competitions/rossmann-store-sales) da Rede de Lojas Rossmann.

**Produto final:**
- [Dashboard](https://dashboard-rossmann-ccf.streamlit.app/): Análise Exploratória de Dados
- [BotTelegram](t.me/Rossmann_cf_Bot): Previsão de Vendas com Aprendizado de Máquina (Machine Learning)

<h2>1. Entendendo o Problema de Negócio:</h2>

História fictícia:

> “O CFO (Diretor financeiro) da empresa fez uma reunião com todos os Gerentes de Loja e 
> pediu para que cada um deles trouxesse uma previsão diária das próximas 6 semanas de vendas.
> Depois dessa reunião, todos os Gerentes entraram em contato com a equipe de Data Science, 
> requisitando uma previsão de vendas de sua loja.”

Para tal tarefa se utiliza o Aprendizado de Máquina Supervisionada de Regressão - Time Series.

<h2>2. Planejamento:</h2>
Utilizou o método CRISP que é uma metodologia com etapas que fornece uma orientação de um processo de modelo genérico de projeto de mineração de dados de grande sucesso, sendo um circuito investigativo e exploratório.

Para mais informações, artigo disponível no [Medium](https://medium.com/comunidadeds/voc%C3%AA-tem-os-dados-tem-o-problema-de-neg%C3%B3cio-mas-e-agora-o-que-fazer-bf3b2d06482).

![](https://drive.google.com/uc?id=1pX1EdXrt4uc4bnznWosmcTYLOv-21OGe)

<h2>3. Premissas</h2>

**Colunas disponíveis no dataset extraído do Kaggle:**

**Descrição das colunas**


colunas | descrição
------- | ---------
store | um ID único para cada loja
day_of_week | o dia da semana
date | data do registro
sales | o volume de negócios para qualquer dia (é isso que você está prevendo)
customers | o número de clientes em um determinado dia
open | um indicador para saber se a loja estava aberta: 0 = fechado, 1 = aberto
promo | indica se uma loja está realizando uma promoção naquele dia
state_holiday | feriado estadual. a = feriado, b = feriado da Páscoa, c = Natal, 0 = nenhum
school_holiday | indica se a (Loja, Data) foi afetada pelo fechamento das escolas públicas
store_type | diferencia entre 4 modelos de lojas diferentes: a, b, c, d
assortment | descreve um nível de sortimento: a = básico, b = extra, c = estendido
competition_distance | distância em metros até a loja concorrente mais próxima
competition_open_since_month | indica o mês aproximados em que o concorrente mais próximo foi aberto
competition_open_since_year | indica o ano aproximados em que o concorrente mais próximo foi aberto
promo2 | promoção contínua e consecutiva para algumas lojas: 0 = não está participando, 1 = participando
promo2_since_week | descreve a semana em que a loja começou a participar do Promo2
promo2_since_year | descreve o ano em que a loja começou a participar do Promo2
promo_interval | descreve os intervalos consecutivos em que o Promo2 é iniciado

**Novas Features**

colunas | descrição
--------|------------
year | ano extraido da coluna data
month | mês extraido da coluna data
day | dia extraido da coluna data
week_of_year | semana do ano extraido da coluna data
competition_since | ano e mês extraido da coluna competition_open_since_year e competition_open_since_month
competition_time_month | quantidade de meses que existe a loja competidora
promo_since | ano e semana que começou a promoção
promo_time_week | quantidade de semanas que a loja está em promoção
is_promo | 0 = não está em promoção, 1 = está em promoção

**Preenchimento dos valores ausentes**

colunas | descrição
--------|------------
competition_distance | utilizando a lógica de que talvez  NA é pq a loja competidora está bem distante, preencher um valor qualquer [200000.0] (maior q o valor maximo do meu dataframe)
competition_open_since_month | preenchido de acordo com a data de registro
competition_open_since_year | preenchido de acordo com a data de registro
promo2_since_week | preenchido de acordo com a data de registro
promo2_since_year | preenchido de acordo com a data de registro
promo_interval | preenchido com o valor zero

<h2>4. Análise Exploratório de Dados</h2>

DASHBOARD disponivel pela Cloud Streamlit com algumas análises. 

Do EDA realizado foi possível extrair alguns Insights sobre o Negócio.

#### **1. Lojas com maior sortimentos deveriam vender mais.**

**Veradeiro** Lojas com MAIOR sortimento vendem mais em média.

- Lojas de sortimento extra equivale a menos de 1% dos nossos dados.
- A média de venda dessas lojas supera a média de vendas dos outros dois sortimentos.
- No entanto, no que vale ao retorno de vendas de todo o dataset, o sortimento Básico tem um maior somatório de vendas.
- Pode ser devido a sua grande quantidade de lojas de sortimento básico presente nos dados, equivale a 52%.

![](https://drive.google.com/uc?id=1iMabA9j0MH1P0nZK_KpOerZKp6ZNIWoV)

#### **2. Lojas deveriam vender mais ao longo dos anos.**

**FALSA**

- Ao longo dos anos, foi possivel observar um declinio de vendas das lojas.
- Como o ano de 2015 não foi finalizado, por isso há uma queda drástica no somatório de vendas.

![](https://drive.google.com/uc?id=1ClCyRZjgt2WJZFRGmtPVvuyaNzmQ_mL_)

#### **3. Lojas abertas durante o feriado de Natal deveriam vender mais.**

**VERDADEIRO**

- Numa análise geral dos anos de 2013 e 2014, o feriado natalino tem uma média de vendas maior que os outros feriados do ano.
- Já numa análise detalhada de cada ano, foi possivel observar que no ano de 2013, o feriado da pásco superou a media de vendas do Natal.
- Comparativo entre os outros feriados
- Como o ano de 2015 não foi finalizado, foi excluido da análise
- Como existem quantidade diferente de registro de cada feriado foi calculado a média e não o somatório de vendas de cada feriado.

![](https://drive.google.com/uc?id=1hO4OT55orxKRpunWrCgIqL8V944Jr2Zr)

<h2>5. Algoritmo de Machine Learning</h2>

Foram selecionados os seguintes algoritmos de Machine Learning para Regressão:

- Random Forest Regression
- Linear Regression
- Lasso
- XGBoots

Se utilizando de um método de Cross-Validation, que tem como fim pegar diferentes fatias do tempo dos dados de treino para que torne possível medir uma real Performance do Modelo.  Foi obtido as seguintes métricas:

Modelo | MAE | MAPE | RMSE
--------|------------ | --------- | ------
Random Forest Regression | 833.511708 | 0.120853 | 1238.412287
XGBoot Regression | 997.735638 | 0.143812 | 1439.025268
Average Model | 1354.800353 | 0.455051 | 1835.135542
Linear Regression - Lasso | 1891.712702 | 0.289104 | 2744.465343
Linear Regression | 2062.201665 | 0.301743 | 3076.085627

 - MAE → A soma da diferença entre o valor real e o valor da predição, dividido pelo numero de valor predito (média do error).

 - MAPE → É a porcentagem de error do MAE. Mostra o quão longe a predição está do valor real, na média, em porcentagem.

 - RMSE (Root Mean Square Error)→ Muito usado para calcular a performance do algoritmo Machine Learning, pois atribui peso, ou seja, atribui maior peso a erros maiores. Sendo rígido aos outliers.

Como observado, o algoritmo Random Forest Regression um porcentagem de erro MAPE menor dos outros, seguido pelo algoritmo XGBoost Regression. 

Nesse projeto, o algoritmo XGBoost foi escolhido para fazer a previsão de vendas dos dados, devido a sua rapidez para treinar e ajustar comparado ao Random Forest.

Com os ajustes os hiperparâmetros do algoritmo XGBoost, tendo como objetivo aumentar a precisão e velocidade de corrida. Para esse auto-ajuste foi utilizado o método RandomSearch (Pesquisa Aleatória). Depois do ajuste o modelo obteve as seguintes métricas:

Modelo | MAE | MAPE | RMSE
--------|------------ | --------- | ------
XGBoot Regression | 997.695478 | 0.14018 | 1421.43486

<h2>6. Resultados da Modelagem para o Negócio</h2>

Como comparativo de resultados, foi criado uma tabela com a previsão do modelo, assim como o pior e melhor cenário de previsão de acordo com as métricas de error. 

Logo abaixo contem uma pequena amostra desses resultados.
Loja | Previsão | Pior cenário | Melhor cenário | MAE | MAPE
--------|------------ | --------- | ------ | ------ | --------
381 | 32615.09 | 325691.15 | 326609.03 | 458.94 | 0.051
855 | 224139.95 | 223780.94 | 224498.96 | 359.00 | 0.056
489 | 277889.31 | 277412.83 | 278365.79 | 476.48 | 0.056
272 | 223818.07 | 223424.81 | 224211.33 | 393.26 | 0.058
1015 | 223437.73 | 223054.72 | 223820.74 | 383.01 | 0.060

No que se refere ao total de vendas nas próximas 6 semanas, este foi o resultado:
Cenário | Valores 
--------|------------ 
Previsão | 268,146,080.00 
Pior cenário | 267,032,717.00 
Melhor cenário | 269,259,431.38

<h2>7. Conclusão</h2>

Para melhorar o storytelling para que o CFO consiga obter com melhor clareza as informações. Os resultados obtidos, tanto pela Análise Exploratória de Dados como pelo Aprendizado de Máquina, foram colocados em produção. Disponibilizados de forma online. 

**Bot Telegram**

https://user-images.githubusercontent.com/80589529/204912637-4cbe0dea-ec62-4115-8bf0-8ab2a9faec45.mp4

