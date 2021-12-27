# Sales Forecast for Rossmann Stores (Time Series + Telegram Bot)

<img src="img/Rossmann_Schriftzug_mit_Centaur.jpg" alt="drawing" width="100%"/>

_This sales prediction project uses data from Rossmann, a Germany-based drug store chain with operations over more than 3,000 stores across seven European countries. The dataset is publicly available from a [Kaggle competition](https://www.kaggle.com/c/rossmann-store-sales/data)._

---
## 01. Brief Intro - Dirk Rossmann GmbH
A história da empresa começa em 1972, quando Dirk Roßmann abriu a primeira farmácia de autoatendimento na Alemanha. Hoje, a Dirk Rossmann GmbH, com 56.300 funcionários e 4.244 filiais, é uma das maiores redes de farmácias da Europa. Em 2020, o Grupo ROSSMANN gerou vendas de 10,35 bilhões de euros na Alemanha, Polônia, Hungria, República Tcheca, Albânia, Kosovo, Espanha e Turquia. Até hoje, Dirk Rossmann GmbH é um negócio familiar gerenciado pelo proprietário e internacionalmente ativo e é de propriedade majoritária da família Roßmann. Além disso, o Grupo A.S. Watson, globalmente ativo, tem uma participação de 40% na empresa.

---

## 02. Business Request
### a. The Business Situation
**Cenário**: “O CFO da empresa fez uma reunião com todos os gerentes de loja e pediu que cada um deles trouxesse uma previsão diária das próximas 6 semanas de vendas.
Depois da reunião, todos os gerentes entraram em contato, requisitando uma previsão de vendas de sua loja.”
### b. Questão de Negócio:
**Qual é o valor das vendas de cada loja nas próximas 6 semanas?**  

### c. Entendimento do Negócio:
- **Qual é a motivação?** A previsão de vendas foi requisitada pelo CFO em uma reunião mensal sobre os resultados das lojas.  
- **Qual é a causa raiz?** Dificuldade em determinar o valor do investimento para a reforma de cada loja.
- **Quem é o dono do problema?** Diretor Financeiro ( CFO ) da Rossmann.
- **Qual o formato da entrega?**
  -	**Granularidade**: Previsão de vendas por dia para cada loja para os próximos 42 dias / 6 semanas.
  -	**Tipo do Problema**: Previsão de Vendas.
  -	**Potenciais Métodos**: Séries Temporais.
  -	**Formato da Entrega**:
  	-	O valor total de vendas de cada loja no final da sexta semana.
    - Poder verificar as informações pelo celular.

---

## 03. Data Preparation
### a. O Objetivo da Descrição dos Dados
**“O quão desafiador é o problema que estou lidando”**. Precisamos saber a dimensão do problema que iremos enfrentar. Pode ser um problema que dure anos ou pode ser um problema que dure semanas.


### b. Qual a Quantidade de Dados?
**Servidores? Clusters? Spark? Hadoop?**
Eu tenho os recursos corretos para trabalhar?
Dependendo da quantidade de dados, o nosso computador pessoal não vai suportar e necessitaremos de servidores, cluster de servidores, linguagens preparadas para trabalhar com uma grande quantidade de dados, como por exemplo Spark e Hadoop.

### c. Tipos de Variáveis
**Quais são os tipos de variáveis? (% Numérica, % Categórica, % Temporal)**. Saber a porcentagem do tipo dos dados vai nos guiar à escolher as técnicas para trabalhar com os tipos de variáveis.

### d. Quantidade de Dados Faltantes
-	**Qual o volume de dados NA?**
  Necessitamos saber qual o volume de dados NA (não aplicável) ou vazio no conjunto de dados.
  Dependendo da quantidade de dados NA, podemos tomar duas decisões?
  
    -	Não fazer o projeto porque não temos dados suficiente.
    	
    -	Continuar com o projeto:
        -	Descartar as linhas que possuírem dados vazios.
  	         -	**Vantagem**: é rápido.
  	         -	**Desvantagem**: estamos jogando dado fora. As vezes descartas linhas não é uma boa estratégia, principalmente se temos poucos dados. As vezes apenas uma variável está NA, mas todas as outras variáveis estão completas e estas informações são muito importantes para o algoritmo aprender os padrões. Então, descartar as linhas pode prejudicar a performance do modelo.
        -	Preencher os dados vazios.
            -	Quando não temos informação de negócio: utilizando algoritmos de machine learning, utilizando mediana, media, etc...
            -	Entender o negócio: As vezes o NA está lá porque é uma lógica de negócio, foi definida por uma regra de negócio e soubermos a regra, podemos colocar valores nos NA´s e recuperar os dados.

### e. Resumo Geral dos Dados
A **Estatística Descritiva** nos fornece uma noção da grandeza dos dados, ter uma noção de limites mínimos e máximos das variáveis, mediana, estatísticas básicas para poder descrever os dados de uma maneira macro.


### f. Data Dimension
**Dados de treino**:
- Number of rows: 1017209
- Number of columns: 18

### g. Data Type
| Variable      | Data Type |
| ----------- | ----------- | 
|store|int64|
day_of_week|                       int64
date|                             object
sales|                             int64
customers|                         int64
open|                              int64
promo|                             int64
state_holiday|                    object
school_holiday|                    int64
store_type|                       object
assortment|                       object
competition_distance|            float64
competition_open_since_month|    float64
competition_open_since_year|     float64
promo2|                            int64
promo2_since_week|               float64
promo2_since_year|               float64
promo_interval|                   object


### h. Check NA
| Variable      | Qty |
| ----------- | ----------- | 
store|                                0
day_of_week|                          0
date|                                 0
sales|                                0
customers|                            0
open|                                 0
promo|                                0
state_holiday|                        0
school_holiday|                       0
store_type|                           0
assortment|                           0
competition_distance|              2642
competition_open_since_month|    323348
competition_open_since_year|     323348
promo2|                               0
promo2_since_week|               508031
promo2_since_year|               508031
promo_interval|                  508031

### i.	Fillout NA
-	**i. Competition Distance**: No site do Kagle [Rossmann Store Sales](https://www.kaggle.com/c/rossmann-store-sales/data) temos a seguinte definição para esta variável: _distance in meter to the nearest competitor store_.
Verifiquei que a distância em metros do competidor mais distante era de 75860. Eu assumi o valor **200000** para os valores faltantes.

-	**ii. Competition_open_since_month**: No site do Kagle [Rossmann Store Sales](https://www.kaggle.com/c/rossmann-store-sales/data) temos a seguinte definição para esta variável: _gives the approximate year and month of the time the nearest competitor was opened_. O valor pode estar vazio porque não possui competidor, ou porque não sabemos a data de abertura do concorrente. Pode ter sido aberta antes da inauguração da nossa loja ou abriu depois mas alguém esqueceu de anotar.
Eu assumi como premissa para os dados faltantes, o valor do mês da variável `date`.
Mantive este valor na variável, pois um competidor influencia no volume de vendas. Quando um competidor abre recentemente, o volume de vendas na nossa loja cai. Com o tempo, as vendas na nossa loja aumentam, porém não vão retornar ao patamar antes da inauguração do competidor.
Assumir o valor do mês do campo `date` pode não ter lógica, pois temos a influência do campo competition_distance.
Não era certeza que esta premissa iria funcionar. Por isso utilizei o método cíclico de gerenciamento de projeto chamado CRISP-DM (Cross Industry Standard Process for Data Mining). Na primeira iteração do CRISP-DM, utilizei esta premissa. Se o algoritmo não performasse bem, no próximo ciclo do CRISP-DM, poderia alterar esta premissa para esta feature.

-	**iii. Competition_open_since_year**: No site do Kagle [Rossmann Store Sales](https://www.kaggle.com/c/rossmann-store-sales/data) temos a seguinte definição para esta variável: _gives the approximate year and month of the time the nearest competitor was opened_. Eu assumi como premissa para os dados faltantes, o valor do ano da variável `date`. Foi utilizado o mesmo raciocínio descrito acima para a variável `Competition_open_since_month`.

-	**iv. promo2_since_week**: No site do Kagle [Rossmann Store Sales](https://www.kaggle.com/c/rossmann-store-sales/data) temos a seguinte definição para esta variável: _describes the year and calendar week when the store started participating in Promo2_. Eu substituí os valores faltantes pelo valor da semana da variável `date`.

-	**v. promo2_since_year**: No site do Kagle [Rossmann Store Sales](https://www.kaggle.com/c/rossmann-store-sales/data) temos a seguinte definição para esta variável: _describes the year and calendar week when the store started participating in Promo2_. Eu substituí os valores faltantes pelo valor do ano da variável `date`.

-	**vi. promo_interval**: No site do Kagle [Rossmann Store Sales](https://www.kaggle.com/c/rossmann-store-sales/data) temos a seguinte definição para esta variável: _describes the consecutive intervals Promo2 is started, naming the months the promotion is started anew. E.g. "Feb,May,Aug,Nov" means each round starts in February, May, August, November of any given year for that store_. Eu substituí os valores faltantes pelo valor 0.
    -	Foi criada a variável `month_map` com o valor do mês da variável `date`.
    - Foi criada a variável `is_promo` onde foram utilizadas as seguintes condições:
        - se o valor da variável `promo_interval` for igual à 0, atribui o valor 0 na variável `is_promo`, significando que a loja não está participando da promoção.
        - se o valor da variável `promo_interval` for diferente de 0, atribui o valor 1 na variável `is_promo`, se algum dos meses contidos no valor da variável `promo_interval` for igual ao valor do mês da variável `month_map`. Caso contrário, é atribuído o valor 0 na variável `is_promo`.

### j.	Change Type
-	Foram alterados os tipos das features `competition_open_since_month`, `competition_open_since_year` , `promo2_since_year` e `promo2_since_week` de float64 para int64.
-	Foi alterado o tipo da feature `date` de object para datetime64.


### k. Descriptive Statistics
-	**i. Numerical Attributes** 

<img src="img/numerical_attributes.jpg" alt="drawing" width="100%"/>

`competition_distance` and `competition_open_since_year` are heavily skewed;

`customers`, `competition_distance`, `competition_open_since_year` have a high kurtosis, which indicates a profusion of outliers;
 
 
-	**ii. Categorical Attributes**

<img src="img/categorical_attributes_1.jpg" alt="drawing" width="75%"/>

O gráfico acima exibe uma distribuição muito dispersa dos dados, pois a dimensão das variáveis está muito diferente.
Isto é muito provável de ocorrer, pois temos dias que são feriados, onde a loja está fechada e consequentemente a quantidade de vendas é zero.
Neste caso, a coluna 0 que representa dias sem feriados possui um volume de vendas bem maior que as outras três colunas a, b e c que são feriados: a = public holiday, b = Easter holiday, c = Christmas.
Desta maneira, criei dois boxplots separados, um para os dias normais e outro para os feriados.

<img src="img/categorical_attributes_2.jpg" />

O gráfico acima representa os dias que são feriados onde a loja estava aberta, isto é, com o valor da variável `sales` diferente de zero.

<img src="img/categorical_attributes_3.jpg" />

O gráfico acima representa os dias que não são feriados onde a loja estava aberta, isto é, com o valor da variável `sales` diferente de zero.
-	`state_holiday`, `store_type`, `assortment` have many outliers.

---

## 04. Feature Engineering
### a. O Mapa Mental de Hipóteses
A Análise Exploratória de Dados é um passo que pode ser tão complicado e tão detalhado quanto você queira.
A causa disso é que existem várias maneiras de fazer uma análise exploratória de dados e existem vários detalhamentos que podemos executar.

Podemos gastar muito tempo neste passo e temos que lembrar que estamos trabalhando em Ciclos de desenvolvimento e consequentemente, iremos executar novamente este passo. Então, não precisamos perder muito tempo neste passo, pois iremos detalhá-lo cada vez que executarmos um novo ciclo.

Para sabermos qual o mínimo de detalhamento que necessitamos ter, é necessário realizar um Mapa Mental de Hipóteses que é basicamente um roteiro que vai nos mostrar quais variáveis precisamos ter para fazer determinadas análises e validar as hipóteses.

O Mapa Mental de Hipóteses vai nos guiar na Análise Exploratória de Dados, possibilitando realizar este passo de forma muito mais rápida, mais direta e trazer Insights valiosos para aquele momento do ciclo do CRISP-DM.

-	**i. Fenômeno:**
Qual o fenômeno que estou modelando?
Aquilo que estou tentando medir ou modelar. Por exemplo: vendas, detecção de objetos em uma imagem, classificação da imagem entre gato e cachorro, Clusterização de clientes para criação de Personas, etc...
É o fenômeno que estamos modelando para ensinar os algoritmos de Machine Learning.

-	**ii. Agentes:**
Quem são os agentes que atuam sobre o fenômeno de interesse?
São todas as entidades que de alguma forma impactam no fenômeno.
Por exemplo: Você acha que os clientes impactam no fenômeno de vendas? A resposta é sim, pois se tivermos mais clientes, provavelmente teremos mais vendas. Lojas também são agentes, pois quanto mais lojas temos, mais iremos vender. Produto também é um agente, pois produto tem um preço que se subirmos o preço do produto venderemos menos e se diminuirmos venderemos mais.

-	**iii. Atributos do Agente:**
Qual é a descrição dos agentes?
Por exemplo: O cliente possui uma idade, escolaridade, estado civil, família, número de filhos, frequência que vai na loja, salário, profissão, etc ...

<img src="img/MindMapHipothesis.png" />

-	**iv. Lista de Hipóteses**.
O objetivo do Mapa Mental de Hipóteses é derivar uma lista de hipóteses e com esta lista iremos priorizar e fazer a análise para validar estas hipóteses.
Cada hipótese validada ou descartada é um Insight.
Um Insight é gerado através de duas formas:  
    -	**Através da surpresa**: Exemplo: Descobrimos que vendemos mais aos Sábados e ninguém sabia desta informação.
    -	**Contrapor uma crença**: Exemplo: o CFO diz ter certeza que acontecem mais vendas no final do ano. Durante a exploração dos dados é verificado que se vende menos no final do ano e isto choca a crença do CFO que esperava outra informação e ele gosta deste Insight que realizamos.
        
       Todas as informações que recebemos dos times de negócio que podem influenciar nas vendas, iremos utilizá-las para criar o Mapa Mental de Hipóteses e as suas respectivas afirmações de Hipóteses. Iremos anotar todos os agentes e perguntar para as pessoas quais são os atributos de cada agente.
       Hipóteses são APOSTAS. É o que você acha sobre alguma coisa:
    -	Lojas de MAIOR Porte deveriam Vender MAIS.
    -	Lojas com MAIOR Sortimento deveriam Vender MAIS.
    -	Lojas com MAIS Competidores deveriam Vender MENOS.
    
       É importante deixar claro dois pontos para as pessoas envolvidas:
    -	**Primeiro**: Deixar claro que estas informações de Hipótese são apostas, portanto iremos precisar primeiro dos dados para validar se uma Hipótese é correta ou não.
    -	**Segundo**: A Hipótese não é uma relação de causa e efeito, é apenas uma correlação. Não podemos afirmar que lojas de maior porte deveriam vender mais e então, aumentamos o tamanho de todas as lojas. Isto não é verdade porque o fenômeno de vendas, sofre o impacto de todas as outras entidades ao mesmo tempo. Utilizamos cada atributo do nosso conjunto de dados e relacionamos eles com a nossa variável resposta que no nosso caso são as vendas.

       Não podemos afirmar que lojas de maior porte deveriam vender mais e então, aumentamos o tamanho de todas as lojas. Isto não é verdade porque o fenômeno de vendas, sofre o impacto de todas as outras entidades ao mesmo tempo.
       Utilizamos cada atributo do nosso conjunto de dados e relacionamos eles com a nossa variável resposta que no nosso caso são as vendas.
       Estamos procurando com as hipóteses saber todos os pequenos efeitos que contribuem para aumentar ou diminuir a venda e desta maneira, poder ensinar o algoritmo de Machine Learning à aprender estes padrões.

#### **Hipóteses das Lojas**
1. Lojas com número maior de funcionários deveriam vender mais.
2. Lojas com maior capacidade de estoque deveriam vender mais.
3. Lojas com maior porte deveriam vender mais.
4. Lojas com maior sortimento deveriam vender mais.
5. Lojas com competidores mais próximos deveriam vender menos.
6. Lojas com competidores à mais tempo deveriam vender mais.


#### **Hipóteses dos Produtos**
1. Lojas que investem mais em Marketing deveriam vender mais.
2. Lojas com maior exposição de produto deveriam vender mais.
3. Lojas com produtos com preço menor deveriam vender mais.
4. Lojas com promoções mais agressivas (descontos maiores), deveriam vender mais.
5. Lojas com promoções ativas por mais tempo deveriam vender mais.
6. Lojas com mais dias de promoção deveriam vender mais.
7. Lojas com mais promoções consecutivas deveriam vender mais.

#### **Hipóteses de Tempo**
1. Lojas abertas durante o feriado de Natal deveriam vender mais.
2. Lojas deveriam vender mais ao longo dos anos.
3. Lojas deveriam vender mais no segundo semestre do ano.
4. Lojas deveriam vender mais depois do dia 10 de cada mês.
5. Lojas deveriam vender menos aos finais de semana.
6. Lojas deveriam vender menos durante os feriados escolares.

#### **Lista Final de Hipóteses**
Temos que verificar se possuímos ou não o dado disponível no momento da análise. Temos várias Hipóteses e podemos provar algumas com os dados que já possuímos.
Existirão Hipóteses que não possuímos os dados para validar. Então teremos que gastar um tempo para coletar estes dados e prepará-los. 
Validamos as hipóteses que possuem os seus respectivos dados. Após isto, se o modelo de ML estiver performando mal e verificarmos que outras variáveis sejam relevantes, então iremos coletá-las e utilizá-las na análise.

A lista final de Hipóteses abaixo considera as hipóteses criadas no item anterior que possuíam dados no conjunto de dados:
1. Lojas com maior sortimento deveriam vender mais.
2. Lojas com competidores mais próximos deveriam vender menos.
3. Lojas com competidores à mais tempo deveriam vender mais.
4. Lojas com promoções ativas por mais tempo deveriam vender mais.
5. Lojas com mais promoções consecutivas deveriam vender mais.
6. Lojas abertas durante o feriado de Natal deveriam vender mais.
7. Lojas deveriam vender mais ao longo dos anos.
8. Lojas deveriam vender mais no segundo semestre do ano.
9. Lojas deveriam vender mais depois do dia 10 de cada mês.
10. Lojas deveriam vender menos aos finais de semana.
11. Lojas deveriam vender menos durante os feriados escolares.

### b. Feature Enginnering
Foram criadas as seguintes variáveis:
-	`year` contendo o ano da variável `date`.
-	`month` contendo o mês da variável `date`.
-	`day` contendo o dia da variável `date`.
-	`week_of_year` contendo a semana do ano da variável `date`.
-	`year_week` contendo o número do ano e o número da semana da variável `date`.
-	`competition_since` contendo a informação do ano da variável `competition_open_since_year`, o valor do mês da variável `competition_open_since_month` e o valor do dia igual à 1.
-	`competition_time_month` contendo o valor em meses de quanto tempo existe um competidor. Este valor é a diferença da data da variável `date` com o valor da data da variável `competition_since`, dividindo o resultado por 30 para obter a quantidade de meses.
-	`promo_since` contendo o valor da variável `promo2_since_year` e `promo2_since_week`, onde ambos os valores são convertidos para string e entre estes valores é acrescentado o caractere "-". Por fim, esta variável é convertida para o formato de data.
-	`promo_time_week` contendo o valor em semanas de quanto tempo existe uma promoção. Este valor é a diferença da data da variável `date` com o valor da data da variável `promo_since`, dividindo o resultado por 7 para obter a quantidade de semanas.

Foram alterados os valores da feature `assortment` conforme descrito abaixo:
-	de **a** para **basic**.
-	de **b** para **extra**.
-	de **c** para **extended**.

Foi alterado os valores da feature `state_holiday` conforme descrito abaixo:
-	de **a** para **public_holiday**.
-	de **b** para **easter_holiday**.
-	de **c** para **christimas**.

### c. Filtrando Variáveis
-	**i. Restrições de Negócio**: As vezes começamos a fazer um projeto de ciência de dados e descobrimos no final do projeto que não conseguimos colocar o modelo em produção. E uma das maiores causas é que não consideramos as restrições de negócio no início do projeto. Uma solução para isto não ocorrer é considerar as restrições de negócio no início do projeto.

-	**ii.	Variáveis mais relevantes para o Modelo**: Está relacionado às variáveis mais relevantes para o modelo. O algoritmo vai verificar as correlações entre as variáveis e vai decidir quais variáveis são relevantes para o modelo. Porém, o algoritmo não leva em consideração as restrições de negócio. É papel do cientista verificar quais são as restrições que os times de negócio possuem para prover os dados que serão utilizados no modelo de Machine Learning.

Foram removidos os dados referentes à lojas fechadas e as lojas sem vendas, já que são irrelevantes para o objetivo de predição de vendas. Para isto, foram mantidas apenas as linhas que continham o valor da variável `open` diferente de 0, e o valor da variável `sales` maior do que 0.

Foi removida a variável `open` já que uma vez que selecionamos no passo anterior apenas as linhas da variável `open` que possuem valor diferente de 0, isto é, valor igual à 1, não teremos variabilidade de valor nesta variável, pois todos os valores são iguais à 1. Portanto, podemos excluir a variável `open`.

Foi removida a variável `customer` pois não temos disponível a informação de quantos clientes teremos nas lojas daqui à seis semanas. Teríamos que fazer um projeto separado para prever quantos clientes estariam nas lojas daqui à seis semanas, pegar este resultado e utilizá-lo como entrada neste projeto. Porém, não iremos fazer isto neste primeiro ciclo do projeto. Portanto, a variável `customer` é uma variável que não temos disponível no momento da predição, significando que é uma restrição de negócio.

Foi removida a variável `promo_interval` que foi utilizada para derivar a variável `is_promo`.

Foi removida a variável `month_map` que foi utilizada como uma variável auxiliar para a criação de outras variáveis.

---

## 05. Análise Exploratória de Dados
### a. O Objetivo da Análise Exploratória de Dados
**“Como as variáveis impactam o fenômeno?”**
e
**“Qual é a força desse impacto?”**.
A Análise Exploratória de Dados serve para medir o impacto das variáveis em relação à variável resposta e em muitas vezes, tentar quantificar este impacto.
A análise exploratória de dados nos fornece uma noção de como os dados se comportam e quais variáveis impactam o fenômeno que estamos prevendo, nos ajudando a selecionar as variáveis que serão utilizadas no modelo de Machine Learning, aumentando a assertividade do projeto.

### b. Quais são os objetivos da Análise Exploratória de Dados?
-	**i. Ganhar Experiência de Negócio**
Entender como o negócio funciona, o comportamento que o negócio possui através dos dados. O time de negócio já possui esta habilidade, pois ele está inserido na operação. Todo dia o time de negócio está discutindo KPI, métricas, medidas, etc...
Você como cientista de dados não possui este conhecimento pois fica em um time fora da operação.
Então, a AED é uma oportunidade de ganhar experiência para debater com o time de negócio, trocar ideias e trocas Insights.

-	**ii.	Validar Hipóteses de Negócio (Insights)**
Validar as hipóteses de negócio criadas no Mind Mapping de Hipóteses e com isto, gerar Insights.
Para gerar Insights necessitamos de duas coisas:
    -	**Gerar surpresa**: Fornecer uma informação que as pessoas não conheciam. 
    -	**Chocar uma crença de uma pessoa**: O time de negócios já possui muitos Insights de como a empresa funciona e consequentemente possuem muitas crenças. 
Exemplo: as pessoas do time de negócio sabem que no fim de semana vende mais, que no feriado vende menos, que determinado tipo de loja vende mais, etc...
São todas informações empíricas, pois provavelmente o time de negócio não parou para analisar os dados.
Se você apresenta para o time de negócio uma informação que contradiz uma crença, por exemplo: o time de negócio tem uma crença que um determinado tipo de loja vende mais, porém a análise dos dados mostra que este determinado tipo de loja vende menos,  e você apresenta isto, você está chocando uma crença da pessoa. Então a pessoa vai se pronunciar falando que sempre pensou que funcionava de uma maneira, mas agora estamos apresentando que funciona de outra maneira. Neste momento geramos um novo Insight, pois derrubamos uma crença.

-	**iii.	Perceber variáveis que são importantes para o modelo**
Quando fazemos análise exploratória de dados, ganhamos a sensibilidade de quais variáveis impactam o fenômeno. Desta forma, ajudaria o modelo à ser mais assertivo.
O algoritmo de Machine Learning possui funcionalidades para definir quais variáveis são relevantes para treinar o modelo. Porém, devemos julgar se esta seleção de variáveis realmente faz sentido. A análise exploratória de dados pode apontar como relevante algumas variáveis que o modelo não sugeriu e iremos saber da sua relevância através dos gráficos e dos resultados da análise exploratória de dados. Então, incluímos as variáveis apontadas pela análise exploratória de dados como complemento para a sugestão que o algoritmo fez. Portanto, a análise exploratória de dados é importante para termos o conhecimento das variáveis e realizarmos uma validação no que o algoritmo informou como variáveis relevantes.

### c. Os Tipos de Análise Exploratória de Dados
-	**i. Análise Univariada**: Se importa unicamente com a variável. Como é esta variável? (Min, Max, Distribuição, Range, etc..). Quais são os valores mínimo, máximos, distribuição, etc.. Estuda como é esta variável e como esta variável se comporta.

-	**ii. Análise Bivariada**: Analisamos o impacto de uma única variável em relação à variável resposta. Como a variável impacta a resposta? (Correlação, Validação de Hipóteses, etc..).

-	**iii. Análise Multivariada**: Analisamos o impacto de mais de uma variável em relação à variável resposta e também o impacto entre as variáveis. Como as variáveis se relacionam? (Qual a correlação entre elas?). Exemplo: temos o impacto de uma única variável em relação à variável resposta, porém quando esta variável se junta com outra variável, o impacto dobra.

     Os algoritmos de Machine Learning seguem algumas premissas. Uma dessas premissas é a teoria de Occan’s Razor ou Navalha de Occan. Ela é uma teoria que garante o aprendizado dos modelos. A teoria da Navalha de Occan diz que se tivermos vários modelos para escolher, temos que escolher sempre o modelo de menor complexidade, porque ele generaliza melhor o aprendizado.

    Existem várias formas de tornar o algoritmo mais complexo, uma delas é a dimensionalidade do seu Dataset. Podemos entender dimensionalidade como o número de colunas do Dataset. Quanto maior o número de colunas, maior é a dimensionalidade e mais complexo é o seu modelo.

    Para diminuir a dimensionalidade, podemos excluir colunas do Dataset.
    Um dos critérios utilizados para escolher quais colunas serão excluídas, é o quanto de informação cada coluna carrega. Por exemplo, se tivermos duas colunas que carregam o mesmo conteúdo de informação, podemos excluir uma delas e não teremos perda de informação.

    Como encontramos as variáveis que possuem o mesmo nível de informação, isto é, que carregam o mesmo nível de informação?
    Para fazer isto, existe um método utilizado na álgebra linear que são os vetores linearmente dependentes. Se tivermos dois vetores, neste caso duas colunas que são linearmente dependentes, podemos excluir uma das colunas e o conteúdo de informação se mantém.
    Uma das maneiras de encontrar se os vetores são linearmente dependentes, é olhar para a correlação entre as variáveis. Se uma variável estiver muito correlacionada com outra variável, podemos excluir uma das variáveis que o conteúdo de informação se mantém.

    A análise multivariada ajuda a identificar quais as variáveis que são correlacionadas e portanto, podem ser excluídas para diminuir a dimensionalidade do Dataset e desta maneira, diminuir a complexidade do modelo.

-	**iv. Correlação entre variáveis**
    -	**Variáveis numéricas**: O método Pearson é um teste estatístico para calcular a correlação entre duas variáveis numéricas. Os valores da correlação variam de 1 até -1.  Quanto mais próxima de 0, mais fraca é a correlação e quanto mais próximo de 1 ou -1, mais forte é a correlação.
 
    -	**Variáveis categóricas**: Utilizamos o método Cramer V, ou V de Cramer (https://en.wikipedia.org/wiki/Cram%C3%A9r%27s_V).

          _Cramér's V is computed by taking the square root of the chi-squared statistic divided by the sample size and the minimum dimension minus 1_:
          
          <img src="img/cramers2.jpg" alt="drawing" width="60%"/>
          

          Para calcular o X2 que deriva do método chi quadrado de Pearson, utilizamos a função da biblioteca scipy chamada chi2_contingency(Consufion Matrix/tabela de contingencia).
          Para determinar o Confusion Matrix / tabela de contingência, utilizamos a fórmula pd.crosstab abaixo que determina as possíveis combinações entre as variáveis categóricas informadas, contando o número de linhas para cada uma das combinações:
		
          _pd.crosstab(cat_attributes_v2['state_holiday'], cat_attributes_v2['store_type'])_
		
          <img src="img/cramer_exit.jpg" alt="drawing" width="50%"/>

          É necessário realizar uma correção na fórmula de V de Cramer:
	  
          _Bias correction: Cramér's V can be a heavily biased estimator of its population counterpart and will tend to overestimate the strength of association. A bias correction, using the above notation, is given by:_

          <img src="img/cramer_correction.jpg" alt="drawing" width="50%"/>

          O resultado de V de Cramer varia de zero até um, onde quanto mais perto do zero, menor é a correlação e quanto mais perto de um, maior é a correlação.


### d. Análise Univariada
- **i. Response Variable**: Exibe a distribuição da variável sales.

   <img src="img/response_variable.jpg" alt="drawing" width="50%"/>

   O gráfico acima mostra uma distribuição com uma skew positiva (cauda da distribuição para o lado direito). A maioria dos algoritmos de Machine Learning são criados baseados em premissas, onde uma destas premissas é que os dados tenham uma distribuição normal. Então quanto mais normal for a distribuição da variável resposta, melhor o algoritmo vai performar.

- **ii. Numerical Variable**

   <img src="img/numerical_variables.jpg" alt="drawing" width="100%"/>

- **iii. Categorical Variable**

   <img src="img/categorical_variables.jpg" alt="drawing" width="100%"/>

   **Highlights**:
   - `state_holiday`: there are more sales data points on public holidays than other holidays.
   - `store_type`: there are more sales data points for store of type "a", and fewer stores of type "b".
   - `assortment`: there are fewer sales data points for stores with assortment of type 'extra' than other assortment types.

### e. Análise Bivariada

- **H1. Lojas com maior sortimento deveriam vender mais.**

   <img src="img/h1_g1.jpg" alt="drawing" width="75%"/>
   <img src="img/h1_g2.jpg" alt="drawing" width="75%"/>

   Podemos observar no gráfico acima que a venda média do tipo de assortment=extra vem aumentando mais ao longo do tempo do que os outros dois tipos de assortment. 

   **VERDICT: FALSE**. Lojas com maior sortimento (assortment = extended) possuem uma venda média MENOR do que as lojas com o sortimento (assortment=extra). 

- **H2. Lojas com competidores mais próximos deveriam vender menos.**

   <img src="img/h2_g1.jpg" alt="drawing" width="75%"/>
   <img src="img/h2_g2.jpg" alt="drawing" width="75%"/>
   <img src="img/h2_g3.jpg" alt="drawing" width="75%"/>

   **VERDICT: FALSE**. As lojas com COMPETIDORES MAIS PRÓXIMOS vendem na MÉDIA.

- **H3. Lojas com competidores à mais tempo deveriam vender mais.**

   <img src="img/h3_g1.jpg" alt="drawing" width="75%"/>
   <img src="img/h3_g2.jpg" alt="drawing" width="75%"/>
   <img src="img/h3_g3.jpg" alt="drawing" width="75%"/>
   <img src="img/h3_g4.jpg" alt="drawing" width="75%"/>
   <img src="img/h3_g5.jpg" alt="drawing" width="75%"/>

   **VERDICT: FALSE**. Lojas com COMPETIDORES À MAIS TEMPO vendem na MÉDIA.

- **H4. Lojas com promoções ativas por mais tempo deveriam vender mais.**

   <img src="img/h4_g1.jpg" alt="drawing" width="100%"/>

   **VERDICT: FALSE**. Lojas com promoções ativas por mais tempo vendem MENOS depois de um certo período de promoção.

- **H5. Lojas com mais promoções consecutivas deveriam vender mais.**

   <img src="img/h5_g1.jpg" alt="drawing" width="75%"/>

   **VERDICT: FALSE**. No gráfico acima observamos que as lojas que participaram da promoção consecutiva, isto é, (tradicional + estendida) possuem um volume de vendas menor do que as lojas que apenas participaram da promoção tradicional.

- **H6. Lojas abertas durante o feriado de Natal deveriam vender mais.**

   <img src="img/h6_g1.jpg" alt="drawing" width="75%"/>
   <img src="img/h6_g2.jpg" alt="drawing" width="75%"/>

   O gráfico acima informa que o feriado de Natal obteve uma venda média maior no ano de 2014 do que no ano de 2013. O gráfico mostra também que o ano de 2015 não está completo, pois não existem dados referentes ao feriado de Natal em 2015.

   **VERDICT: FALSE**. Lojas abertas durante o feriado de Natal obtém uma venda média MENOR do que as lojas abertas durante o feriado de Páscoa.

- **H7. Lojas deveriam vender mais ao longo dos anos.**

   <img src="img/h7_g1.jpg" alt="drawing" width="100%"/>

   **VERDICT: TRUE**. Lojas vendem MAIS ao longo dos anos, lembrando que o ano de 2015 não está completo.

- **H8. Lojas deveriam vender mais no segundo semestre do ano.**

   <img src="img/h8_g1.jpg" alt="drawing" width="100%"/>

   **VERDICT: TRUE**. Lojas obtém uma venda média MAIOR no segundo semestre do ano.

- **H9. Lojas deveriam vender mais depois do dia 10 de cada mês.**

   <img src="img/h9_g1.jpg" alt="drawing" width="100%"/>

   **VERDICT: FALSE**. Lojas possuem uma venda média MENOR depois do dia 10 de cada mês.

- **H10. Lojas deveriam vender menos aos finais de semana.**

   <img src="img/h10_g1.jpg" alt="drawing" width="100%"/>

   **VERDICT: FALSE**. Lojas obtém uma venda média MENOR aos Sábados, porém obtém uma venda média MAIOR aos Domingos.

- **H11. Lojas deveriam vender menos durante os feriados escolares.**

   <img src="img/h11_g1.jpg" alt="drawing" width="75%"/>

   O gráfico acima mostra que a média das vendas durante os feriados escolares é um pouco superior que os dias que não são feriados escolares.

   <img src="img/h11_g2.jpg" alt="drawing" width="100%"/>

   O gráfico acima mostra a soma das vendas para os dias que são feriados escolares e para os dias que não são feriados escolares ao longo do ano. Podemos notar que os meses 7 e 8 que são as férias escolares, possuem um aumento no volume de vendas.

   **VERDICT: FALSE**. Lojas vendem MAIS durante os feriados escolares.

### f. Análise Multivariada

- **i. Numerical Attributes**

   <img src="img/multi_numerical.jpg" alt="drawing" width="100%"/>

   Na matriz acima, quanto mais escura for a cor, maior é a correlação negativa e quanto mais clara for a cor, maior é a correlação positiva.
   Esta matriz é simétrica, isto é, se dividirmos a matriz com uma diagonal, a parte de baixo é o espelho da parte de cima. 
   A correlação entre a mesma variável possui o valor 1 e está representada na diagonal da matriz.
   Quando observamos a correlação da variável `sales`, verificamos uma correlação forte com a variável `day_of_week` de -0,46.
   Temos uma correlação forte entre as variáveis `sales` e `customers` de 0,89, porém não conseguiremos utilizar a variável `customers`, pois necessitamos obter a quantidade de `customers` que estarão na loja nas próximas seis semanas, porém não possuímos esta previsão. Uma alternativa seria realizar um projeto de previsão de `customers` ao longo das próximas seis semanas e incorporar neste projeto. 

   **Correlações negativa forte**: `sales` x `day_of_week`, `open` x `day_of_week`, `promo` x `day_of_week`, `school_holiday` x `day_of_week`, `promo2_since_year` x `promo2`, `is_promo` x `promo2_since_year`.

   **Correlação positiva forte**: `is_promo` x `promo2`, `sales` x `open`, `sales` x `promo`, `sales` x `customers`, `open` x `customers`, `promo` x `customers`, `promo` x `open`.

- **ii. Categorical Attributes**

   <img src="img/multi_categorical.jpg" alt="drawing" width="75%"/>

   Existe uma correlação moderada entre as variáveis `store_type` e `assortment`.

---

## 06. Data Preprocessing
### a. O Objetivo da Preparação dos Dados
_“O aprendizado da maioria dos algoritmos de Machine Learning é facilitado com dados numéricos, na mesma escala”_.

A maioria dos algoritmos de Machine Learning foram criados seguindo alguns critérios e um desses critérios era que as variáveis fossem numéricas. O motivo disto é que os algoritmos de Machine Learning trabalham com métodos de otimização. Estes métodos de otimização encontram os melhores parâmetros para o seu conjunto de dados, utilizando em muitas vezes, cálculos com derivadas. As derivadas como outros tipos de cálculos utilizam variáveis numéricas e portanto, não é possível utilizar variáveis categóricas.

Outro problema que devemos tratar é com relação à variação dos valores (range) das variáveis numéricas. Alguns algoritmos de Machine Learning tendem à dar uma importância maior para variáveis com maior range. Exemplo: variável_A possui um range de 0 até 10, e a variável_B possui um range de 0 até 10.000. Neste caso, o algoritmo de Machine Learning dará uma importância maior para a variável_B que possui um range maior que a variável_A. Isto ocorre, devido à matemática utilizada nos modelos, por exemplo, as redes neurais, utilizam um método de otimização chamado Gradiente Descendente. O método Gradiente Descendente trabalha com derivadas parciais e estas derivadas beneficiam as variáveis com maior range. Ele tende a dar uma importância maior para as variáveis que possuem um maior range de valores. Necessitamos trazer todas as variáveis para o mesmo range de valores para que o aprendizado de máquina dê a mesma importância para todas as variáveis.

### b. Tipos de Preparação dos Dados
<img src="img/preparacao_dados_tabela.jpg" alt="drawing" width="100%"/> https://www.kaggle.com/discdiver/guide-to-scaling-and-standardizing<br />

- **i. Normalização**

   <img src="img/normalizacao.jpg" alt="drawing" width="25%"/>

   A normalização reescala o centro para 0 com desvio-padrão igual à 1. **Funciona muito bem para as variáveis que possuem uma distribuição Gaussiana, ou seja, que possuem uma distribuição normal**.

   Temos as seguintes distribuições no conjunto de dados:
   <img src="img/numerical_variables.jpg" alt="drawing" width="100%"/>
 
   Não existe nenhuma variável com característica de distribuição normal, portanto não foi executada a normalização para nenhuma das variáveis do conjunto de dados.

- **ii. Rescaling:**

   Rescaling funciona muito bem para as variáveis que não possuem uma distribuição Gaussiana, ou seja, que não possuem uma distribuição normal. Reescala para o intervalo entre 0 e 1. 

    -	**Min-Max Scaler**

          <img src="img/min_max_scaler.jpg" alt="drawing" width="25%"/>

          **A técnica Min-Max Scaler é muito sensível à Outliers**, pois considera em sua fórmula o valor máximo (X max). Quando temos Outliers na variável e aplicamos o Min-Max Scaler esta transformação tende a colocar todos os dados originais próximos do 0 na nova escala.

          Foi utilizado Min-Max Scaler nas variáveis `promo_time_week` e `year` por não possuírem Outliers relevantes.


    -	**Robust Scalerr**

          <img src="img/robust_scaler.jpg" alt="drawing" width="50%"/>

          **A técnica de rescaling Robust Scaler diminui a sensibilidade aos Outliers**, considerando em sua fórmula a diferença entre quartis, em vez de considerar o valor máximo como ocorre na técnica Min-Max Scaler.

          Foi utilizado Rescaling Robust Scaler nas variáveis `competition_distance` e `competition_time_month`, por possuírem Outliers relevantes.

- **iii. Transformação:**

    -	**Encoding**: Converte as Features categóricas para numéricas.

        - **One Hot Encoding**: One Hot Encoding cria uma coluna para cada valor (nível) da variável categórica.
  	         -	**Benefício**: simples de aplicar.
  	         -	**Desvantagens**: Cria muitas novas colunas no Dataset, aumentando a dimensionalidade do Dataset, podendo prejudicar o aprendizado do modelo, tornando-o “OverFitting”.
  	         -	**Quando utilizar**: Quando uma variável categórica possui a ideia de estado, por exemplo, feriado. No feriado, vivemos um estado de dias regulares e depois entramos em um estado de feriado. Estes dois estados têm comportamento muito diferentes, as pessoas agem de um jeito em dias normais e agem de outro jeito em feriados.   
se o valor da variável `promo_interval` for igual à 0, atribui o valor 0 na variável `is_promo`, significando que a loja não está participando da promoção.

          Foi utilizado One Hot Encoding na variável `state_holiday`.


        - **Label Encoding**: Label Encoding troca os níveis das variáveis categóricas por valores de maneira aleatória. Não existe uma relação entre os valores numéricos e os níveis das variáveis categóricas.
  	         -	**Quando utilizar**: Quando temos variáveis categóricas que são apenas nomes, por exemplo, nome de loja. Quando não existe uma relação entre os níveis da variável categórica.

          Foi utilizado Label Encoding na variável `store_type`.


        - **Ordinal Encoding**: Ordinal Encoding troca os níveis das variáveis categóricas por valores respeitando uma ordem.
  	         -	**Quando utilizar**: Quando os níveis (valores) de uma variável categórica possuem uma ordem, possuem uma hierarquia do que é maior e menor. Por exemplo: a temperatura é uma variável categórica que possui uma ordem nos seus níveis (valores). Altura (alto, médio, baixo) é outro exemplo de uma variável categórica que possui uma ordem em seus níveis (valores).

          Foi utilizado Ordinal Encoding na variável `assortment`.

    -	**Transformação de Grandeza**: O objetivo da transformação de grandeza é trazer a distribuição da variável resposta o mais próximo possível de uma distribuição normal.

        - **Logarithm Transformation**: Aplica o log em todos os valores da variável resposta. Transforma distribuições que possuem um skill muito à esquerda ou à direita em mais próximo possível de uma distribuição normal.

          Foi utilizada a transformação logarítmica na variável resposta `sales`.

    -	**Transformação de Natureza Cíclica**: Alguns dados relacionados ao tempo, como por exemplo a variável meses é uma variável cíclica, pois dentro de cada ano temos o mesmo conjunto de meses que varia de 1 até 12. Portanto, temos uma natureza cíclica.
    
          Porém, considerando estes valores, o mês de dezembro (12) parece estar muito longe do mês de janeiro (1) do ano seguinte, mas na realidade não está. A distância do mês de dezembro para o mês de janeiro do ano seguinte, é a mesma distância do mês de janeiro para fevereiro.

          Existe uma forma de transformar esta variação de 1 até 12 de uma forma que ela fique cíclica e então o modelo entenderá esta natureza cíclica e os intervalos entre os meses.
	  
          Utilizamos um círculo trigonométrico, calculando as medidas de seno e coseno para cada um dos meses, conforme ilustrado na figura abaixo:

          <img src="img/natureza_ciclica.jpg" alt="drawing" width="100%"/>

          Na ilustração acima temos:
	  
        - Janeiro: seno = 1 e coseno = 0.
	  
        - Fevereiro: seno = 0,92 e coseno = 0,4. 

         **Cada variável será transformada em duas variáveis: variável_seno e variável_coseno**, que combinadas representam a sua natureza cíclica da variável.

         Foi utilizada a transformação de natureza cíclica nas variáveis `month`, `day`, `week_of_year` e `day_of_week`.

---

## 07. Feature Selection
### a. O Objetivo da Feature Selection
_“A explicação mais simples sobre um fenômeno observado, deveria prevalecer sobre explicações mais complexas.” (Occam’s Razor – Navalha de Occam).”_

O princípio de Occam diz que se temos dois modelos que representam o mesmo fenômeno, dê preferência para aquele que é mais simples, pois provavelmente ele generaliza melhor, ele descreve a maior quantidade de situações existentes.

### b. Removendo as Variáveis Colineares
Para tornar mais simples a aprendizagem em Machine Learning, necessitamos utilizar modelos mais simples, isto é, modelos que possuem um número menor de variáveis.
A simplicidade do conjunto de dados está representada pela sua quantidade de colunas.
Quando explicamos um fenômeno, colhemos variáveis/features para poder explicar este fenômeno, onde cada feature explica um pedaço deste fenômeno.
Porém, pode ocorrer que duas ou mais features expliquem a mesma parte do fenômeno, ou seja, elas carregam o mesmo conteúdo de informação.
Quando duas ou mais variáveis/features explicam a mesma coisa, as chamamos de variáveis colineares. Desta forma, temos que identificar e remover as variáveis colineares.

- **i. Seleção por Subset (Wrapper Methods):**

   É um processo utilizado para determinar a relevância das variáveis:
   <img src="img/wrapper method.jpg" alt="drawing" width="75%"/>
   
   **a**. No conjunto de dados original seleciona aleatoriamente uma variável, mais a variável resposta.
   
   **b**. Utiliza-se um algoritmo de Machine Learning para treinar o modelo, onde geralmente é utilizado o algoritmo Random Forest.
   
   **c**. Calcula a performance do modelo que pode ser em termos de acuraria ou erro.
   
   **d**. Compara a performance deste modelo com a iteração anterior.
   
   **e**. Adiciona uma nova variável ao conjunto de dados.
   
   **f**. Executa o algoritmo de Machine Learning para treinar o modelo e compara a performance com a iteração anterior.
   
    -	I.	Caso a performance tenha melhorado, mantemos a variável que foi incluído por último. Isto significa que a variável que foi adicionada por último possui alguma informação sobre o fenômeno que está sendo modelado, portanto o modelo conseguiu aprender um pouco mais sobre o fenômeno utilizando esta nova variável.
   
    -	II.	Caso a performance tenha piorado ou se manteve igual, removemos a variável que foi incluído por último.
   
   **g**. Adiciona uma nova variável ao conjunto de dados e repete o processo novamente até utilizar todas as variáveis do conjunto de dados.
   
   **h**. No final do processo teremos um conjunto de dados composto apenas com as variáveis relevantes para o modelo.


- **ii. O Método Boruta**

   O algoritmo Boruta é utilizado para seleção de variáveis por Subset e funciona da seguinte maneira:
   <img src="img/boruta_method.jpg" alt="drawing" width="100%"/>
	  
	  
        - **One Hot Encoding**: One Hot Encoding cria uma coluna para cada valor (nível) da variável categórica.
  	         -	**Benefício**: simples de aplicar.
