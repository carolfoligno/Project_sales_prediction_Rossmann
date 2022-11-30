<h1 style="text-align: center;">Previsão de Vendas das Lojas Rossmann</h1>

![](https://drive.google.com/uc?id=1J6vVNu-K-iEDaP8rCXKfkJtXmuVxrC6B)

Este projeto foi orientado pela [Comunidade DS](https://comunidadeds.com/), utilizando os dados disponíveis no [Kaggle](https://www.kaggle.com/competitions/rossmann-store-sales) da Rede de Lojas Rossmann.

**Produto final:**
- [Dashboard](https://carolfoligno-rossmann-project-dashboard-1lraay.streamlit.app/): Análise Exploratória de Dados
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
