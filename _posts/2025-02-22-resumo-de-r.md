---
layout: post
title: Resumo de R
date: 2025-02-22 10:31 -0300
math: true
---


## R básico

- assinalar variável

```R
#variáveis
income <- 100
taxes <- 20

#operações com variáveis
net_income <- income - taxes

#variáveis podem variar
income <- 120

#tbm pode ser assim
35 -> taxes
```
- vetores
  
```R
#idades dos clientes de uma livraria
idades <- c(28, 48, 47, 71, 22, 80, 48, 30, 31)

#operações são aplicadas em todos os elementos da lista
idades_em_meses <- idades * 12
>[1] 336 576 564 852 264 960 576 360 372

#compras dos clientes de uma livraria
compras <- c(20, 59, 2, 12, 22, 160, 34, 34, 29)

#clientes e suas compras
bookstore <- data.frame(idades,compras)

#começa em 1
> bookstore
  idades compras
1     28      20
2     48      59
3     47       2
4     71      12
5     22      22
6     80     160
7     48      34
8     30      34
9     31      29

#p/ criar vetor (1,2,3,4,5)
x <- 1:5

#p/ criar vetor (5,4,3,2,1)
y <- 5:1

#p/ criar vetor (1,2,3,4,5,5,4,3,2,1)
z <- c(1:5,5:1)
```
- funções
  
```R
#tipicamente funções seguem padrão função(variável)

#média de idades
idades <- c(28, 48, 47, 71, 22, 80, 48, 30, 31)
mean(idades)
>[1] 45

#algumas funções requerem mais de uma variável

#correlação entre idades e compras
compras <- c(20, 59, 2, 12, 22, 160, 34, 34, 29)
cor(idades,compras)
>[1] 0.589402
```
- cor() utiliza a correlação de pearson, que assume valores entre -1 e 1, sendo que os valores das extremidades indicam uma correlação perfeita (negativa e positiva, respectivamente), e 0 indica ausência de correlação linear. como o resultado do coeficiente de correlação foi 0.59, isso implica que clientes mais velhos tendem a gastar mais.
- entretanto, o cliente mais velho gastou 160 reais, valor muito superior aos restantes, influenciando demasiadamente o coeficiente de Pearson, sensível a outliers. seria interessante também computar uma correlação menos sensível a outliers, como a correlação de spearman.
  
```R
#correlação de spearman
cor(idades,compras, method='spearman')
>[1] 0.3487395
#relação mais fraca!!
	
#quais argumentos inserir em uma função? -> documentação
#para acessar documentação: ?nome_da_função
?cor

#algumas funções não requerem variáveis:
#printa o horário do sistema
Sys.time()
>[1] "2025-02-15 13:30:35 -03"

#documentação da função length
?

#vetores de altura e peso
height <- c(158, 170, 172, 181, 196)
weight <- c(45, 80, 62, 75, 115)

#tamanho da lista (número de elementos)
length(height)
>[1] 5

#alturas ordenadas
sort(height) #por padrão decreasing=FALSE
>[1] 158 170 172 181 196
sort(height, decreasing=TRUE)
>[1] 196 181 172 170 158
```
operações matemáticas

```R
#adição: +
#subtração: -
#multiplicação: *
#divisão: /

#parênteses são avaliados antes: 
#1 + 2*(8/2) = 1 + 2*4 = 1 + 8 = 9

#exemplos de funções utilizadas numa variável x e num número a
abs(x) # retorna o número absoluto
sqrt(x) # raiz quadrada
log(x) # logaritmo de x com base e
log(x, base=a) # logaritmo de x com base a
a^x # a elevado a x
exp(x) # e elevado a x
sin(x) # seno de x
x %% a # operação módulo (encontra o resto da divisão de x por a)
factorial(x) # x!
choose(n,k) #combinação simples de n e k 


#quando x é um vetor
sum(x) # soma todos os elementos de x - somatório de c(x1,x2,x3,...,xn)
prod(x) # multiplica todos os elementos de x - produtório de c(x1,x2,x3,...,xn)

pi # pi
```

- para fazer as seguintes operações no R:
1. \$$ \sqrt \pi $$

2. \$$ e^2 \cdot \log(4) $$

```R
# 1
sqrt(pi)
>[1] 1.772454
# 2
exp(2) * log(4)
>[1] 10.24341
```

respostas não numéricas (ocorrem quando resultado é infinito ou indeterminado)

```R
# respostas não numéricas
1/0
>Inf

0/0
>NaN

sqrt(-1)
>Mensagen de aviso:
>In sqrt(-1) : NaNs produzidos
```

## pacotes

- conteúdos que não vêm de fábrica no R → pacotes adicionam novas funcionalidades à linguagem (ex: funções, datasets…)
- exemplos de novas funcionalidades: plotar os dados num gráfico, rodar códigos C++ no R para apressar o processamento e rodar testes estatísticos desconhecidos e obscuros
- +17.000 pacotes, grande parte disponível em CRAN, repositório oficial do R

para instalar o ggplot2, que adiciona capacidades gráficas adicionais no R, é possível usar a interface em Tools > Install Packages e escrever o nome do pacote ou simplesmente usar o seguinte código:

```R
install.packages('ggplot2')

# depois de (pouco) tempo
>pacote ggplot2 desempacotado com sucesso e somas MD5 verificadas
```

em caso de problemas de instalação:

- problemas comuns de instalação de pacotes: https://support.posit.co/hc/en-us/articles/200554786-Problem-Installing-Packages-in-the-RStudio-IDE
- para lidar com erros comuns no R: https://www.modernstatisticswithr.com/thebasics.html#troubleshooting

no entanto, não basta apenas instalar o pacote para utilizar suas funcionalidades, também é preciso carregá-lo no começo de cada sessão no R, por via da função `library`

```R
# habilitando ggplot para uso
library(ggplot2)

# para instalar o pacote palmerpenguins -> dataset p/ exploração e visualização
# instalação do pacote
install.packages('palmerpenguins')

# carregando e explorando palmerpenguins
library(palmerpenguins)
?penguins
```

o pacote _palmerpenguins_ corresponde a um dataset de medidas de espécies de pinguins, e contém 344 registros de 8 variáveis, como espécie, ilha e peso. o objetivo desse dataset é ser alternativa ao velho conhecido iris  para tarefas de exploração e visualização de dados.

```R
> str(penguins)
tibble [344 × 8] (S3: tbl_df/tbl/data.frame)
 $ species          : Factor w/ 3 levels "Adelie","Chinstrap",..: 1 1 1 1 1 1 1 1 1 1 ...
 $ island           : Factor w/ 3 levels "Biscoe","Dream",..: 3 3 3 3 3 3 3 3 3 3 ...
 $ bill_length_mm   : num [1:344] 39.1 39.5 40.3 NA 36.7 39.3 38.9 39.2 34.1 42 ...
 $ bill_depth_mm    : num [1:344] 18.7 17.4 18 NA 19.3 20.6 17.8 19.6 18.1 20.2 ...
 $ flipper_length_mm: int [1:344] 181 186 195 NA 193 190 181 195 193 190 ...
 $ body_mass_g      : int [1:344] 3750 3800 3250 NA 3450 3650 3625 4675 3475 4250 ...
 $ sex              : Factor w/ 2 levels "female","male": 2 1 1 NA 1 2 1 2 NA NA ...
 $ year             : int [1:344] 2007 2007 2007 2007 2007 2007 2007 2007 2007 2007 ...
 
 # 344 observações, 8 variáveis
 # variáveis podem ser numerical, factor (categóricas)
 # ou int, numéricas que só aceitam números inteiros
 
 > summary(penguins)
      species          island    bill_length_mm  bill_depth_mm   flipper_length_mm
 Adelie   :152   Biscoe   :168   Min.   :32.10   Min.   :13.10   Min.   :172.0    
 Chinstrap: 68   Dream    :124   1st Qu.:39.23   1st Qu.:15.60   1st Qu.:190.0    
 Gentoo   :124   Torgersen: 52   Median :44.45   Median :17.30   Median :197.0    
                                 Mean   :43.92   Mean   :17.15   Mean   :200.9    
                                 3rd Qu.:48.50   3rd Qu.:18.70   3rd Qu.:213.0    
                                 Max.   :59.60   Max.   :21.50   Max.   :231.0    
                                 NA's   :2       NA's   :2       NA's   :2        
  body_mass_g       sex           year     
 Min.   :2700   female:165   Min.   :2007  
 1st Qu.:3550   male  :168   1st Qu.:2007  
 Median :4050   NA's  : 11   Median :2008  
 Mean   :4202                Mean   :2008  
 3rd Qu.:4750                3rd Qu.:2009  
 Max.   :6300                Max.   :2009  
 NA's   :2     
 
 # contagem das variáveis categóricas
 # noções da distribuição de variáveis numéricas
 # quantidade de NA's em cada variável
 ```

## estatística descritiva

Também utilizado um dataset contido no pacote ggplot2 chamado msleep, com hábitos de sono de 83 mamíferos, além do palmerpenguins.

```R
#msleep automaticamente carregado junto com ggplot2
library(ggplot2)

> msleep
# A tibble: 83 × 11
   name         genus vore  order conservation sleep_total sleep_rem sleep_cycle awake  brainwt
   <chr>        <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>    <dbl>
 1 Cheetah      Acin… carni Carn… lc                  12.1      NA        NA      11.9 NA      
 2 Owl monkey   Aotus omni  Prim… NA                  17         1.8      NA       7    0.0155 
 3 Mountain be… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6 NA      
 4 Greater sho… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1  0.00029
 5 Cow          Bos   herbi Arti… domesticated         4         0.7       0.667  20    0.423  
 6 Three-toed … Brad… herbi Pilo… NA                  14.4       2.2       0.767   9.6 NA      
 7 Northern fu… Call… carni Carn… vu                   8.7       1.4       0.383  15.3 NA      
 8 Vesper mouse Calo… NA    Rode… NA                   7        NA        NA      17   NA      
 9 Dog          Canis carni Carn… domesticated        10.1       2.9       0.333  13.9  0.07   
10 Roe deer     Capr… herbi Arti… lc                   3        NA        NA      21    0.0982 
# ℹ 73 more rows
# ℹ 1 more variable: bodywt <dbl>
# ℹ Use `print(n = ...)` to see more rows
```

- 83 observações e 11 colunas (variáveis)
- NA significa Not Available, utilizado pelo R para se referir a dados faltantes

```R
# para visualizar os dados de uma forma melhor
View(msleep)

# retorna as primeiras 6 linhas
head(msleep)

# retorna as últimas 6 linhas
tail(msleep)

# retorna qtd de linhas e colunas
dim(msleep)

# retorna informações sobre as variáveis, em especial o tipo
str(msleep) # chr (character) e num (numerical) no caso do dataset

# retorna o nome das variáveis
names(msleep)

# como faz parte de um pacote, é possível acessar a documentação
?msleep

# para listar msleep na lista de variáveis do ambiente:
data(msleep)
```

### variáveis numéricas

A função `summary(dataset)` retorna uma noção da distribução das variáveis numéricas (com informações equivalentes aos pontos do boxplot), além das frequências das variáveis categóricas.

```r
# modo conveniente de arranjar algumas infos
summary(msleep)

# para cada coluna numérica:
  sleep_rem  
Min.   :0.100 # valor mínimo
1st Qu.:0.900 # primeiro quartil
Median :1.500 # mediana
Mean   :1.875 # média
3rd Qu.:2.400 # terceiro quartil
Max.   :6.600 # valor máximo
NA's   :22 # quantidade de valores faltantes
```

- por vezes o objetivo pode ser calcular apenas uma estatística descritiva ou alguma outra estatística não inclusa em `summary()` .

- para acessar uma variável específica do dataset utiliza-se o esquema `nome_dataframe$nome_vetor` . então, para utilizar apenas a coluna `sleep_total` :

```R
# para acessar vetor sleep_total
msleep$sleep_total

# estatísticas descritivas do vetor
mean(msleep$sleep_total) # média
median(msleep$sleep_total) # mediana
max(msleep$sleep_total) # valor máximo
min(msleep$sleep_total) # valor mínimo
sd(msleep$sleep_total) # desvio padrão
var(msleep$sleep_total) # variância
quantile(msleep$sleep_total) # quartis 0, 0.25, 0.5, 0.75, 1.0

# quantos animais dormem mais de 8 horas por dia?
sum(msleep$sleep_total > 8) #número de animais q dormem >8h
>[1] 61
mean(msleep$sleep_total > 8) #proporção de animais q dormem >8h
>[1] 0.7349398
```

- `msleep$sleep_total > 8` confere se a quantidade total de sono de cada mamífero é maior que 8 horas

```R
#média de sono REM para mamíferos
> mean(msleep$sleep_rem)
[1] NA # retorna NA pois há valores faltantes

> ?mean #documentação -> para ignorar NAs utilizar na.rm = TRUE

#média dos valores existentes
> mean(msleep$sleep_total, na.rm = TRUE)
[1] 10.43373

#obs: NAs não foram removidos do dataframe, o R ignorou apenas no cálculo da média

> cor(msleep$sleep_rem, msleep$sleep_total)
[1] NA # mesmo erro

# cor() não utiliza na.rm, e sim use
> cor(msleep$sleep_rem, msleep$sleep_total, use='complete.obs')
[1] 0.751755
```

### variáveis categóricas

- variáveis como `vore` (hábitos de alimentação) e `conservation` (status de conservação) são categóricas.
- para obter informações de variáveis categóricas (chamadas factors), é possível criar uma tabela de frequências:

```R
# contagem de cada categoria
> table(msleep$vore)
  carni   herbi insecti    omni 
     19      32       5      20
     
# proporções de cada categoria
> proportions(table(msleep$vore))
     carni      herbi    insecti       omni 
0.25000000 0.42105263 0.06578947 0.26315789

# proporções por linha
# soma de cada linha = 1
> proportions(table(msleep$vore, msleep$conservation),
+             margin = 1)
         
                  cd domesticated         en         lc         nt         vu
  carni   0.07142857   0.14285714 0.07142857 0.35714286 0.07142857 0.28571429
  herbi   0.03846154   0.26923077 0.07692308 0.38461538 0.11538462 0.11538462
  insecti 0.00000000   0.00000000 0.33333333 0.66666667 0.00000000 0.00000000
  omni    0.00000000   0.11111111 0.00000000 0.88888889 0.00000000 0.00000000
  
# hábitos alimentares pelo status de conservação  
# 7,1% dos carnívoros são cd (conservation dependent)
# 14% dos carnívoros são domesticados
# ...


# proporções por coluna
> proportions(table(msleep$vore, msleep$conservation),
+             margin = 2)
         
                 cd domesticated        en        lc        nt        vu
  carni   0.5000000    0.2000000 0.2500000 0.2000000 0.2500000 0.5714286
  herbi   0.5000000    0.7000000 0.5000000 0.4000000 0.7500000 0.4285714
  insecti 0.0000000    0.0000000 0.2500000 0.0800000 0.0000000 0.0000000
  omni    0.0000000    0.1000000 0.0000000 0.3200000 0.0000000 0.0000000
  
# 20% das espécies LC (least concern) são carnívoros,
# 40% herbívoros
# ...
```

## tipos e estruturas

diferentes processos para cada tipo de dados

- dados numéricos → boxplots e médias, por exemplo
- dados categóricos → bar charts e frequências

o tipo de dado de uma variável descreve qual formato do objeto assinalado a ele.

- variável `a` pode conter números, textos ou um dataframe
- de modo a tratar `a` corretamente, o R precisa saber qual tipo o objeto tem. R identifica o tipo automaticamente, mas pode ser necessário alterar caso a variável seja identificada como um tipo diferente do que é

tipos de dados:

- `numeric` : números como 1 e 16.823 (às vezes também chamado de `double`)
- `logical` : `TRUE` ou `FALSE`
- `character` : formato de textos, descrições, nomes, emails..
- `integer` : números inteiros, denotados no R com a letra `L` , exemplos: `1L`, `55L`
- `complex` : números complexos, raramente utilizados em estatística
- `raw` : utilizado para guardar bites brutos, não é comum

os tipos de dados podem ser combinados para formar estruturas de dados, como vetores e dataframes. exemplos de estruturas de dados incluem o `factor` , utilizado para guardar dados categóricos, e o `POSIXct` , utilizado para guardar dados de data e hora.

para verificar o tipo de uma variável basta utilizar a função `class()`

```R
numero <- 83
texto <- "Brasil"
bool <- TRUE

> class(numero)
[1] "numeric"
> class(texto)
[1] "character"
> class(bool)
[1] "logical"

#utilizando class() em um vetor
numbers <- c(6, 9, 12)

> class(numbers)
[1] "numeric"

#e se dentro do vetor existir diferentes tipos de dados?
numero_texto_bool <- c(numero, texto, bool)
numero_texto_bool
class(numero_texto_bool)

> class(numero_texto_bool)
[1] "character"
# R muda o numero e bool para texto
```

## dataframes

```R
## dataframes

idades <- c(28, 48, 47, 71, 22, 80, 48, 30, 31)
compras <- c(20, 59, 2, 12, 22, 160, 34, 34, 29)
bookstore <- data.frame(idades, compras)
View(bookstore)

# Animal sleep data
library(ggplot2)
View(msleep)
```

- num dataframe: cada coluna representa uma variável (como idades) e cada linha representa uma observação (por exemplo, um indivíduo)
- a tabela em si pode ter diferentes formatos, e cada formato tem um conjunto de funções específicos ao formato. os tipos de formato mais utilizados são:
    - `matrix` : uma tabela em que todas as colunas tem o mesmo tipo de dados, como tudo numeric ou tudo character. usam menos memória e permitem cálculos mais rápidos. pode ser difícil de utilizar para algumas manipulações, gráficos e análises.
    - `data.frame` : tipo mais comum, onde diferentes colunas podem conter diferentes tipos (por exemplo, com uma coluna numérica e outra de character)
    - `data.table` : versão turbinada de data.frame
    - `tbl_df` (”tibble”): outra versão turbinada de data.frame
- na maioria dos casos, não vai importar qual o tipo da tabela, e todas têm uma aparência semelhante.

```R
# tabelas WorldPhones e airquality vem no R base

# dados guardados em matriz
?WorldPhones
class(WorldPhones)
View(WorldPhones)

# dados guardados em data frame
?airquality
class(airquality)
View(airquality)

# dados guardados em tibble
library(ggplot2)
?msleep
class(msleep)
View(msleep)
```
em alguns casos, funções utilizadas requerem que o input seja uma matriz, e outras funções podem não funcionar ou se comportar de maneira diferente caso o input seja um tibble ao invés de um data frame normal. para isso, podemos converter os objetos em outros tipos de tabelas:

```R
WorldPhonesDF <- as.data.frame(WorldPhones)
class(WorldPhonesDF)

airqualityMatrix <- as.matrix(airquality)
class(airqualityMatrix)
```
operações entre diferentes tipos de dados:
```R
#aspas únicas são equivalentes a aspas duplas
a <- "texto"
class(a)

b <- 'texto'
class(b)

# inteiros e numerics
class(1 + 2) # numeric
class(1L + 2) # numeric
class (1L + 2L) # integer

# numero + texto
a <- "hello" + 2
class(a)
>"Error in "hello" + 2 : argumento não-numérico para operador binário"

#numeric + logical
a <- TRUE * 10
a
[1] 10
class(a)
[1] "numeric"

# operações numéricas com TRUE e FALSE vão resultar num numeric
# FALSE tratado como 0
# TRUE tratado como 1 
```
algumas funções de dataframes:
```R
#ncol, nrow, ndim, names, row.names

#criação df
airquality_df <- data.frame(airquality)

#número de colunas
ncol(airquality_df)
[1] 6

#número de linhas
nrow(airquality_df)
[1] 153

#shape do df (num linhas, num colunas)
dim(airquality_df)
[1] 153   6

#nome das colunas
names(airquality_df)
[1] "Ozone"   "Solar.R" "Wind"    "Temp"    "Month"   "Day"   

#nome das linhas
row.names(airquality_df)
```
matrizes no R:

$$ 1.\begin{pmatrix}
  1 & 2 & 3\\
  4 & 5 & 6
\end{pmatrix} \space 2.\space \begin{pmatrix}
  1 & 4\\
  2 & 5\\
  3 & 6
\end{pmatrix} $$

```R
> ?matrix

# 1
> matrix(data=1:6, nrow=2, ncol=3, byrow=TRUE)
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6

# 2
> matrix(data=1:6, nrow=3, ncol=2)
     [,1] [,2]
[1,]    1    4
[2,]    2    5
[3,]    3    6
```
dataset airquality:

- `Ozone` : concentração de ozônio em ppb
- `Solar.R` : radiação solar
- `Wind` : velocidade dos ventos (mph)
- `Temp` : temperatura máxima (F)
- `Month` : mês, numérico (maio = 5, junho = 6…)
- `Day` : dia, numérico

questões sobre o dataset:

- qual a temperatura média durante o período?
- qual dia mais quente? e com mais ventos?
- quais dias a temperatura excedeu os 90F?

necessário acessar os vetores do dataframe para responder a essas perguntas →
```R
> # Compute the mean temperature:
> mean(airquality$Temp)
[1] 77.88235
```
para acessar um elemento em particular de um vetor, se utiliza o índice ao redor de colchetes. o primeiro elemento tem o índice 1, o segundo 2, e o último n (em que n é o número de observações)

```R
# para acessar temperatura do quinto elemento de airquality
# nesse dataset, uma observação corresponde a um dia, quinto elemento = quinto dia
> airquality$Temp[5]
[1] 56
```
os colchetes podem ser diretamente aplicados ao dataframe: `airquality[i,j]` resgata o elemnto na linha `i` e na coluna `j` do dataframe. para extrair toda uma linha ou toda uma coluna, basta não utilizar o elemento i ou o elemento j.

```R
# conferir nomes das colunas
names(airquality)
[1] "Ozone"   "Solar.R" "Wind"    "Temp"    "Month"   "Day"  ## Temp é coluna 4

# acessar quinto elemento da quarta coluna
> airquality[5, 4]
[1] 56

# acessar quinta linha
airquality[5,]
  Ozone Solar.R Wind Temp Month Day
5    NA      NA 14.3   56     5   5

# acessar quarta coluna
airquality[,4] # mesmo que airquality$Temp
airquality[[4]] # mesmo que airquality$Temp

# acessar coluna 2, 4 e 6
airquality[, c(2,4,6)]

# acessar todas as colunas exceto a 2
airquality[, -2]

# acessar colunas Wind e Temp
airquality[, c("Wind", "Temp")]

# acessar 5 primeiras linhas
airquality[1:5,]

# correlação entre Temp e Wind sem utilizar $
cor(airquality[,3], airquality[,4])
[1] -0.4579879

# extrair todas as colunas exceto Temp e Wind
airquality[, -c(3, 4)]
```
- adicionando e alterando dados com $

```R
age <- c(28, 48, 47, 71, 22, 80, 48, 30, 31)
purchase <- c(20, 59, 2, 12, 22, 160, 34, 34, 29)
bookstore <- data.frame(age, purchase)

# segundo cliente na vdd tinha 18 anos
bookstore$age[2] <- 18

# ou
bookstore[1,2] <- 18

# para mudar uma coluna inteira (ex. idade em meses)
bookstore$age <- bookstore$age * 12
bookstore

# adicionando novos dados -> duração da visita na livraria
bookstore$visit_length <- c(5, 2, 20, 22, 12, 31, 9, 10, 11)
> bookstore
  age purchase visit_length
1  28       20            5
2  48       59            2
3  47        2           20
4  71       12           22
5  22       22           12
6  80      160           31
7  48       34            9
8  30       34           10
9  31       29           11

# adicionando nova variável rev_per_minute 
# receita por minuto visitado
bookstore$rev_per_minute <- bookstore$purchase / bookstore$visit_length
> bookstore
  age purchase visit_length rev_per_minute
1  28       20            5      4.0000000
2  48       59            2     29.5000000
3  47        2           20      0.1000000
4  71       12           22      0.5454545
5  22       22           12      1.8333333
6  80      160           31      5.1612903
7  48       34            9      3.7777778
8  30       34           10      3.4000000
9  31       29           11      2.6363636

# erro nos dados ! compra do cliente com 80 anos na verdade foi 16
bookstore[6,2] <- 16
> bookstore
  age purchase visit_length rev_per_minute
1  28       20            5      4.0000000
2  48       59            2     29.5000000
3  47        2           20      0.1000000
4  71       12           22      0.5454545
5  22       22           12      1.8333333
6  80       16           31      5.1612903
7  48       34            9      3.7777778
8  30       34           10      3.4000000
9  31       29           11      2.6363636
```
- filtrando com condições
  
```R
# procurando temperatura máxima em airquality
> max(airquality$Temp)
[1] 97

# como visualizar qual observação do dataset corresponde à temperatura máxima? 
# opção 1: visualizar manualmente com View(airquality)
# opção 2: which.max retorna o índice que corresponde ao máximo de airquality$Temp 
which.max(airquality$Temp) # opção 2 é + prática!!
[1] 120

#conferindo registro 120
> airquality[120,]
    Ozone Solar.R Wind Temp Month Day
120    76     203  9.7   97     8  28

# como which.max(airquality$Temp) retorna 120:
airquality[which.max(airquality$Temp),] # retorna o mesmo resultado

# qualquer função que retorna os índices pode ser usada dessa forma
# possível também utilizar condições
airquality[airquality$Temp > 90, ]
```

- `airquality$Temp > 90` retorna TRUE ou FALSE para cada elemento de airquality; utilizando a condição dentro dos colchetes, apenas será selecionado os índices que correspondem a TRUE
- a função `subset()` performa a mesma ação (com uma sintaxe diferente):

```R
# dataset airquality, temperatura > 90
> subset(airquality, Temp > 90)
    Ozone Solar.R Wind Temp Month Day
42     NA     259 10.9   93     6  11
43     NA     250  9.2   92     6  12
69     97     267  6.3   92     7   8
70     97     272  5.7   92     7   9
75     NA     291 14.9   91     7  14
102    NA     222  8.6   92     8  10
120    76     203  9.7   97     8  28
121   118     225  2.3   94     8  29
122    84     237  6.3   96     8  30
123    85     188  6.3   94     8  31
124    96     167  6.9   91     9   1
125    78     197  5.1   92     9   2
126    73     183  2.8   93     9   3
127    91     189  4.6   93     9   4

# operadores lógicos
a == b # confere se a é igual a b
a > b # confere se a é maior que b
a < b # confere se a é menor que b
a >= b # confere se a é maior ou igual a b
a <= b # confere se a é menor ou igual a b
a != b # confere se a é diferente de b
is.na(a) # confere se a é NA
a %in% c(1,4,9) # confere se a é igual a 1 ou 4 ou 9

# para conferir os índices que obedecem a condição (nesse caso, Temp > 90)
> which(airquality$Temp > 90)
[1]  42  43  69  70  75 102 120 121 122 123 124 125 126 127

# para conferir se todas as observações obedecem a condição
> all(airquality$Temp > 90)
[1] FALSE

# para conferir se pelo menos uma observação obedece a condição
> any(airquality$Temp > 90)
[1] TRUE

# para conferir Quantos elementos cumprem uma condição
> sum(airquality$Temp > 90)
[1] 14

# airquality$Temp retorna TRUE para as observações que cumprem a condição
# FALSE para as que não cumprem
# TRUE = 1
# FALSE = 0
# feita a soma dos elementos que obedecem a condição

# para conferir a Proporção de elementos que cumprem uma condição
> mean(airquality$Temp > 90)
[1] 0.09150327
```
