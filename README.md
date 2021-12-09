# Sales Prediction with Telegram Bot
_This sales prediction project uses data from Rossmann, a Germany-based drug store chain with operations over more than 3,000 stores across seven European countries. The dataset is publicly available from a [Kaggle competition](https://www.kaggle.com/c/rossmann-store-sales/data)._

---
## 01. Brief Intro - Dirk Rossmann GmbH
A história da empresa começa em 1972, quando Dirk Roßmann abriu a primeira farmácia de autoatendimento na Alemanha. Hoje, a Dirk Rossmann GmbH, com 56.300 funcionários e 4.244 filiais, é uma das maiores redes de farmácias da Europa. Em 2020, o Grupo ROSSMANN gerou vendas de 10,35 bilhões de euros na Alemanha, Polônia, Hungria, República Tcheca, Albânia, Kosovo, Espanha e Turquia. Até hoje, Dirk Rossmann GmbH é um negócio familiar gerenciado pelo proprietário e internacionalmente ativo e é de propriedade majoritária da família Roßmann. Além disso, o Grupo A.S. Watson, globalmente ativo, tem uma participação de 40% na empresa.

---

## 02.	Business Request
### a.	The Business Situation
- Cenário: “O CFO da empresa fez uma reunião com todos os gerentes de loja e pediu que cada um deles trouxesse uma previsão diária das próximas 6 semanas de vendas.
Depois da reunião, todos os gerentes entraram em contato com você, requisitando uma previsão de vendas de sua loja.”
### b.	Questão de Negócio:
- Qual é o valor das vendas de cada loja nas próximas 6 semanas.  

### c.	Entendimento do Negócio:
- Qual é a motivação? A previsão de vendas foi requisitada pelo CFO em uma reunião mensal sobre os resultados da loja.  
- Qual é a causa raiz? Dificuldade em determinar o valor do investimento para Reforma de cada Loja.
- Quem é o dono do problema? Diretor Financeiro ( CFO ) da Rossmann.
- Qual o formato da entrega?
•	Granularidade: Previsão de vendas por dia para cada loja para os próximas 42 dias / 6 semanas.
•	Tipo do Problema: Previsão de Vendas.
•	Potenciais Métodos: Séries Temporais.
•	Formato da Entrega:
a.	O valor total de vendas de cada loja no final da sexta semana.
b.	Poder verificar as informações pelo celular.
