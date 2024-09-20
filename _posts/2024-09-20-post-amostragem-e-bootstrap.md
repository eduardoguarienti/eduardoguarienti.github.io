---
layout: post
title: Amostragem e Bootstrap
date: 2024-09-20 10:02 -0300
math: true
media_subpath: eduardoguarienti.github.io/assets/img/
---

Para retirar uma amostra de um conjunto de dados, podemos fazer por via da amostragem simples, em que os valores retirados não podem ser repetidos, ou por via do bootstrap, em que os valores retirados podem ser repetidos. O seu uso varia de acordo com a necessidade e com a abrangência do conjunto de dados.

<h3> Amostragem simples sem repetição </h3>

Ao lidar com conjuntos de dados que representam a totalidade da população, isto é, que representam totalmente o objeto de análise, e, caso tenhamos muitos registros a ponto da análise do conjunto populacional não ser prático, é possível fazer uma amostragem simples sem repetição, em que, com apenas uma pequena parte do conjunto de dados é possível obter métricas próximas dos valores da população. 

Para fazer uma amostragem simples sem repetição no pandas:
```python
amostra = df_populacao.sample(frac=0.5, replace=False)
```
> Com o método `.sample`, é possível especificar o tamanho da amostra tanto por via de uma fração do conjunto original, usando `frac` , quanto por via do tamanho da amostra, com `n`.
{: .prompt-info}

<h3> Bootstrap </h3>

O caso mais comum é que os dados não representem a população, e são apenas uma amostra de uma hipotética população. Isso ocorre porque muitas vezes coletar a totalidade dos dados é impraticável, e, quando possível, pode ter custos elevados. Para isso, temos o Bootstrap, ou simplesmente uma amostragem com repetição, em que os dados amostrados podem ser coletados novamente.

O processo de bootstrap é como se fosse o oposto de retirar uma amostra sem repetição do dataset:
- Ao tirarmos uma amostra sem repetição do dataset, tratamos o dataset enquanto população, e a amostra resultante será menor que o dataset;
- Com o processo de bootstrap, tratamos o dataset como uma amostra representativa da população e, por meio da reamostragem, tentamos estimar os valores da população.

Como na maioria das vezes os dados não representam a população, ao repetir os valores presentes, teremos uma aproximação da estatística desejada (em geral, média ou mediana) e de sua distribuição. O processo de amostragem com repetição no pandas se dá da seguinte forma:

```python
lista_reamostragem = []
for i in range(5000):
  lista_reamostragem.append(
    np.mean(df_amostra.sample(frac=1, replace=True)['coluna_numerica'])
  )
```

![Desktop View](https://github.com/eduardoguarienti/posts_imgs/blob/main/post_graf1.png?raw=true){: width="800" height="800" }
_Exemplo de reamostragem feito com um dataset de vinhos_

> Depois do processo de reamostragem, é interessante fazer um histograma da distribuição e ver sua proximidade com uma distribuição normal, pois, de acordo com o Teorema do Limite Central, a distribuição das médias amostrais tende a seguir uma distribuição normal
{: .prompt-tip}

O processo de bootstrap, em sua teoria, é:
1. Resampling: retirada de diversas amostras aleatórias do dataset com repetição, cada uma com o mesmo tamanho do conjunto de dados original;
2. Cálculo de uma estatística de interesse, como média, mediana ou desvio padrão;
3. Repetição do processo diversas vezes: com isso, teremos uma distribuição empírica da estatística, que serve de aproximação de um processo de amostragem feito diretamente da população.
4. Com a distribuição feita com o processo de bootstrap, é possível **estimar o erro padrão, estimar o intervalo de confiança e testar hipóteses**, de modo a entender melhor a variabilidade da estatística que é objeto de análise.

Como o bootstrap é um processo de coletar múltiplas amostras com repetição e mensurar alguma estatística delas, a média do bootstrap será semelhante a média do dataset. Se o dataset não for representativo da população, a média gerada pelo bootstrap será enviesada. Portanto, é importante conhecer os dados e seu processo de coleta (um dos vieses mais comuns é o de seleção), de modo que os valores se aproximem, o máximo possível, de serem independentes e identicamente distribuídos.
   
> É importante que o conjunto de dados seja representativo da população: caso tenhamos vieses presentes na amostra, o processo de bootstrap não será uma boa estimativa para a população.
{: .prompt-warning}

O desvio padrão do bootstrap é chamado de erro padrão. Como temos diversos processos de amostragens e coletas de estatísticas dessas amostras, ao mensurar a variabilidade dessa distribuição, teremos uma métrica de incerteza, e por isso se chama erro padrão.

```python
#para mensurar o erro padrão
#como o bootstrap é uma lista, usamos o método do numpy
np.std(lista_reamostragem, ddof=1)
> 0.026
```

Para estimar o desvio padrão da população, é preciso multiplicar o erro padrão pela raiz quadrada do tamanho da amostra (no caso do código acima, seria `n=5000`):



$$
\sigma = \text{erro padrão} \times \sqrt{n}
$$

```python
n = len(df_amostra)
#estimação do desvio padrão da população
#como lidamos com uma amostra, o valor de ddof é 1
np.std(lista_reamostragem, ddof=1) * np.sqrt(n)
> 1.054

np.std(df_amostra['alcohol'])
> 1.065
```

Conseguimos por meio do bootstrap obter a estimativa de um parâmetro populacional que desejamos e saber o seu erro padrão, além de estimar o desvio padrão da população. Com ela, é possível também estimar o intervalo de confiança e testar hipóteses. 

O intervalo de confiança serve para captar grande parte dos valores que a média pode assumir, e podemos fazer seu cálculo a 95% da seguinte forma:

O intervalo de confiança serve para estimar uma faixa de valores na qual a média populacional provavelmente se encontra com um determinado nível de confiança, em geral 95%. 

```python
limite_inferior = np.quantile(lista_reamostragem, 0.025)
print(limite_inferior)
> 10.371

limite_superior = np.quantile(lista_reamostragem, 0.975)
print(limite_superior)
> 10.474
```

![Desktop View](https://github.com/eduardoguarienti/posts_imgs/blob/main/post_graf2.png?raw=true){: width="800" height="800" }
_Média e intervalo de confiança da distribuição bootstrap, com IC a 95% = [10.371, 10.474]_

Portanto, cada uma das duas ferramentas têm suas utilidades, cada uma para casos bem distintos. Enquanto a amostragem aleatória simples de um dataset serve para questões de otimização e rapidez de análise, o bootstrap serve, dados os pressupostos e ausência de possíveis vieses, para estimar os parâmetros da população a partir de amostras, além de quantificar a incerteza e possibilitar a realização de testes.




