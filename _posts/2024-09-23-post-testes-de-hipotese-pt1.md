---
layout: post
title: Testes de Hipótese pt.1
date: 2024-09-23 11:05 -0300
math: true
---

Um teste de hipótese é um método de inferência estatística utilizado para decidir se os dados suportam alguma hipótese. Geralmente, calculamos uma estatística de teste e a comparamos ou com um **valor crítico** ou avaliando o **p-valor**. Com os testes de hipótese, podemos fazer inferências sobre uma população com uma amostra. 

Os testes de hipótese são organizados em **hipótese nula** ($$ H_0 $$) e **hipótese alternativa** ($$ H_1 $$ ou $$ H_A $$). A hipótese nula consiste na presunção de ausência de efeito ou diferença, ou seja, que os dados não conseguem estar de acordo com a hipótese formulada, enquanto a hipotese alternativa propõe que existe algum tipo de efeito.

A hipótese nula é uma afirmação que presume que qualquer diferença observada é resultado do acaso, e é a hipótese que queremos contrariar por via da hipótese alternativa. Formulamos a hipótese nula em conjunto com a hipótese alternativa. Um exemplo de teste de hipótese é:

$$ H_0 $$ : Não há diferenças estatisticamente significativas entre escolaridade e salário.
$$ H_1 $$ : Escolaridade tem um impacto no salário.

Com o resultado do teste, descobrimos se há evidências na amostra para rejeitar (ou não) a hipótese nula. Ao rejeitar a hipótese nula, consideramos que os dados fornecem suporte à hipótese alternativa. Agora, para fazer o teste, dado que temos a hipótese nula e a hipótese alternativa que são testáveis com os dados, é preciso de um **nível de significância** (representado por $$ \alpha $$), em que valores comuns são 5% e 1%.

1. Definir uma hipótese e a hipótese nula (presunção de ausência de efeito) que são testáveis com os dados;
2. Escolha de um teste estatístico apropriado com os tipos de dados e a hipótese;
3. Determinar a distribuição de probabilidades da estatística de teste assumindo que a hipótese nula é verdadeira;
4. Escolher um nível de significância ($$ \alpha $$), que representa a probabilidade de rejeitar a hipótese nula quando ela é verdadeira. Valores comuns são 5% e 1%;
5. Calcular a estatística de teste usando a amostra: utilizar estatísticas descritivas da amostra na fórmula do teste estatístico apropriado;
6. Rejeitar ou Não Rejeitar a $$ H_0 $$ : se a estatística de teste cair na região crítica, determinado pelo nível de significância, há evidências para rejeitar $$ H_0 $$ e os dados fornecem suporte a $$ H_1 $$. Ao não rejeitar $$ H_0 $$, a estatística de teste não cai na região crítica, e não temos razão para rejeitar a hipótese nula. A relação entre o p-valor e o nível de significância determina a rejeição ou não:
   -  O p-valor corresponde a probabilidade de observar uma estatística de teste tão ou mais extrema que a observada, assumindo que a hipótese nula é verdadeira;
   -  Se p-valor < $$ \alpha $$, rejeita-se a hipótese nula, caso contrário, não rejeita-se a hipótese nula, e há suporte para a hipótese alternativa.

<h3> Definição de hipóteses </h3>

A definição de hipóteses representa a primeira parte de um teste de hipóteses, e é representada como no exemplo:

- $$ H_0 $$ : Não há diferenças estatisticamente significativas entre escolaridade e salário.
- $$ H_1 $$ : Escolaridade tem um impacto no salário.

Outro caso de testes de hipóteses é:

- $$ H_0 $$ : Não há diferenças na proporção de homens e mulheres que nascem em um local em um determinado período de tempo.
- $$ H_A $$ : Nascem mais homens que mulheres em um determinado período de tempo.

Esse teste acima corresponde ao primeiro teste de hipótese já feito, cuja fonte foram os registros de nascimento de Londres no séc. XVII. Considerando a hipótese nula de que não há diferenças entre os nascimentos, o p-valor encontrado foi de 0.5^82, e, como o valor foi muito pequeno, rejeitou-se a hipótese nula. 
Existem três diferentes tipos de hipóteses alternativas, ao depender do sentido da comparação.

| Tipos de testes             | Região Crítica                                   | Exemplo            |
| --------------------------- | ------------------------------------------------ | ------------------ |
| Teste Unilateral à Esquerda | Região crítica à esquerda da distribuição        | X é menor que Y    |
| Teste Bilateral             | Região crítica em ambos os lados da distribuição | X é diferente de Y |
| Teste Unilateral à Direita  | Região crítica à direita da distribuição         | X é maior que Y    |


![Desktop View](https://github.com/eduardoguarienti/posts_imgs/blob/main/testes_hip_1.png?raw=true){: width="1000" height="800" }
_Figura 1: Código disponível aqui.[^1]_

Portanto, quando há um teste unilateral à esquerda, o objetivo é encontrar o ponto em que a nossa amostra, sujeita às formulas de teste, cai na curva de distribuição da estatística de teste. Caso o valor encontrado seja tão ou mais extremo que o valor crítico do nível de significância $$ \alpha $$, isto é, que esteja na região crítica, rejeitamos $$ H_0 $$. Para o teste unilateral à direita, a única diferença é o lado da distribuição que é objeto de análise.

No caso do teste bilateral, temos o valor crítico nos dois lados (linha pontilhada), cada um determinado por um nível de significância  $$ \alpha / 2 $$. Caso a estatística de teste esteja em qualquer um dos dois extremos, dentro das regiões críticas, rejeitamos $$ H_0 $$.

<h3> Escolha do teste estatístico </h3>

Para rejeitar a hipótese nula, é preciso de um nível de significância e um p-valor. O nível de significância é atribuído antes do teste, com valores que são, geralmente, 5% ou 1%. Para encontrar o p-valor, é preciso de um teste estatístico apropriado e condizente com o conjunto de dados.

<h4> Teste Z para a média da população </h4>

$$ Z = \frac{\bar{X} - \mu}{\frac{\sigma}{\sqrt{n}}} $$

Serve para comparar a média da amostra com a média da população. Para usar o teste Z, é preciso cumprir as condições:
- Variável contínua;
- Segue uma distribuição normal ou é uma amostra grande;
- A amostra é independente e identicamente distribuída;
- O desvio padrão populacional é conhecido.

Entretanto, exceto quando o dataset em si representa a população, raramente temos o desvio padrão populacional, sendo seu uso um pouco mais restrito.

Em um dataset com 1407 observações (=n), em que a média salarial (variável contínua) é igual a 6502 e o desvio padrão amostral é de 4707.95 e, assumindo por simplicidade que o desvio padrão amostral é igual ao desvio padrão populacional, se quisermos testar a hipótese nula de que o salário médio da população é de 6000 unidades a um nível de significância de 5%, fazemos o seguinte teste de hipótese:


$$ H_0 : \mu = 6000 $$

$$ H_1 : \mu \neq 6000 $$

Em que o código necessário é:

```python
from statsmodels.stats.weightstats import ztest
df['MonthlyIncome'].mean() #6502.93
df['MonthlyIncome'].std() #4707.95
len(df['MonthlyIncome']) #1470

estatistica_teste, p_valor = ztest(x1=df['MonthlyIncome'],
                                   value=6000,
                                   alternative='two-sided')

print(p_valor) #4.20e-05
print(estatistica_teste) #4.095
```
O p-valor resultante, de 4.20e-05, é bastante inferior ao limite de 5% e, por causa disso, rejeitamos a hipótese nula de que o salário médio populacional é de 6000. A população nesse caso, como estamos lidando com um dataset de uma determinada empresa, representa apenas a empresa. Portanto, é possível afirmar que o salário médio práticado na empresa é diferente de 6000.

Que é o mesmo do que fazer o seguinte de forma algébrica:

$$ Z = \frac{6502 - 6000}{\frac{4707.95}{\sqrt{1470}}} \approx 4.09 $$

Com o valor da estatística de teste $$ Z $$, é possível olhar numa tabela da distribuição normal e conferir a probabilidade associada a essa estatística de teste. Felizmente, o Python faz isso automaticamente, bastando usar o comando `print(p_valor)`.

Graficamente, esse teste seria o gráfico do meio da Figura 1 pois é um teste bilateral. Com o valor de 4.09, ficaria na curva azul à direita. Como o p-valor é muito baixo, de 4.2e-05, inferior a 5%, o ponto iria se situar dentro da região crítica vermelha, em que o p-valor corresponde à integral de 4.09 até o infinito, e é definida pela probabilidade de ocorrência de valores tão ou mais extremos que 4.09.

Um exercício interessante seria descobrir qual o valor limite em que não podemos afirmar que o salário médio populacional é diferente. Resposta disponível aqui.[^2]


<h3> Notas de rodapé </h3>


[^1]: Código da visualização de tipos de hipóteses alternativas

    ```python
    # importação de pacotes
    import numpy as np
    import plotly.figure_factory as ff
    from plotly.subplots import make_subplots

    # geração da distribuição normal
    x = np.random.randn(20000)

    # figuras do plotly
    fig1 = ff.create_distplot([x], ['Dist 1'], show_hist=False)
    fig2 = ff.create_distplot([x], ['Dist 1'], show_hist=False)
    fig3 = ff.create_distplot([x], ['Dist 1'], show_hist=False)

    #tabela de figuras
    fig = make_subplots(rows=1, cols=3,subplot_titles=("Unilateral à esquerda", 
                                                        "Bilateral", 
                                                        "Unilateral à direita"), 
                                                        column_widths=[50, 50, 50],
                                                        row_heights=[50])

    # figura 1
    for trace in fig1['data']:
        # distribuição normal
        fig.add_trace(trace, row=1, col=1)
        # linha vertical
        fig.add_vline(x=-2, row=1, col=1, line_width=2, line_dash='dash', line_color='darkred')
        # alfa
        fig.add_annotation(x=-2, y=0.25,
                    text="alfa",
                    showarrow=False, font=dict(color="darkred"),
                    row=1, col=1, xshift=-15, yshift=-15,)
        # retângulo região crítica
        fig.add_vrect(x0=-4, x1=-2, row=1, col=1,
                  line_width=0, fillcolor="red", opacity=0.2, 
                  annotation_text='Reg. <br> Crítica  ',
                  annotation_position='top right',
                  annotation_font_color='rgba(240, 240, 240, 0.94)')
    # figura 2
    for trace in fig2['data']:
        # distribuição normal
        fig.add_trace(trace, row=1, col=2)
        # linhas verticais
        fig.add_vline(x=-2, row=1, col=2, line_width=2, line_dash='dash', line_color='darkred')
        fig.add_vline(x=2, row=1, col=2, line_width=2, line_dash='dash', line_color='darkred')
        # alfas
        fig.add_annotation(x=-2, y=0.25,
                    text="alfa/2",
                    showarrow=False, font=dict(color="darkred"),
                    row=1, col=2, xshift=-20, yshift=-15)
        fig.add_annotation(x=2, y=0.25,
                    text="alfa/2",
                    showarrow=False, font=dict(color="darkred"),
                    row=1, col=2, xshift=20, yshift=-15)
        # retângulo região crítica
        fig.add_vrect(x0=-4, x1=-2, row=1, col=2,
                  line_width=0, fillcolor="red", opacity=0.2, 
                  annotation_text='Reg. <br> Crítica  ',
                  annotation_position='top right',
                  annotation_font_color='rgba(240, 240, 240, 0.94)')
        fig.add_vrect(x0=4, x1=2, row=1, col=2,
                  line_width=0, fillcolor="red", opacity=0.2,
                  annotation_text='   Reg. <br> Crítica',
                  annotation_position='top left',
                  annotation_font_color='rgba(240, 240, 240, 0.94)')
    # figura 3
    for trace in fig3['data']:
        # distribuição normal
        fig.add_trace(trace, row=1, col=3)
        # linha vertical
        fig.add_vline(x=2, row=1, col=3, line_width=2, line_dash='dash', line_color='darkred')
        # alfa
        fig.add_annotation(x=2, y=0.25,
                    text="alfa",
                    showarrow=False, font=dict(color="darkred"),
                    row=1, col=3, xshift=15, yshift=-15)
        # retângulo região crítica
        fig.add_vrect(x0=4, x1=2, row=1, col=3,
                  line_width=0, fillcolor="red", opacity=0.2,
                  annotation_text='   Reg. <br> Crítica',
                  annotation_position='top left',
                  annotation_font_color='rgba(240, 240, 240, 0.94)')

    # remoção dos ticks, ranges e linha dos eixos
    fig.update_yaxes(showticklabels=False, 
                     range=[0, 0.42],
                     zeroline=True,
                     zerolinewidth=1,
                     zerolinecolor='white')
    fig.update_xaxes(showticklabels=False,
                     range=[-3.5, 3.5],
                     zeroline=True,
                     zerolinewidth=1,
                     zerolinecolor='white')
    # remoção legenda, fundo do gráfico e da imagem
    fig.update_layout(showlegend=False,
                      plot_bgcolor= 'rgba(240, 240, 240, 0.94)',
                        title=dict(
                            text='Tipos de hipóteses alternativas',
                            ))

    fig.show()
    ```

[^2]: O valor limite corresponde ao valor de Z (1.96) cujo p-valor é de 0.05. Como o p-valor, nesse caso, **não é menor que o nível de significância**, não temos evidências suficientes para afirmar que a média populacional é diferente de 6262.269, apesar da média amostral ser de 6502.

    ```python
    from statsmodels.stats.weightstats import ztest

    estatistica_teste, p_valor = ztest(x1=df['MonthlyIncome'],
                                       value=6262.269,
                                       alternative='two-sided')

    print(p_valor) #0.05
    print(estatistica_teste) #1.96
    ```
