---
layout: post
title: "Análise da Performance de Estudantes/Excel"
date: 2024-10-01 11:26 -0300
description: Análise Exploratória de Dados de um conjunto de estudantes e determinantes do desempenho.
permalink: /posts/projeto_excel
image:
  path: "https://github.com/eduardoguarienti/projeto_excel/blob/main/rel_notas_horas_estudo.png?raw=true"
  alt: "Relação entre Notas e Horas de Estudo"
---

<h2> Resultados do Projeto </h2>

Nesse projeto fiz uma análise de dados no Excel para entender quais fatores são determinantes para o aumento da nota dos estudantes. Descobri que os fatores determinantes para o aumento da nota dos estudantes são tanto a frequência nas aulas quanto uma boa quantidade de horas de estudo. 
Entretanto, para estar no limiar superior dos alunos do conjunto, é necessário morar perto da escola, não ter dificuldades de aprendizado, ter pais com alto nível de escolaridade e com poder aquisitivo suficiente para prover um bom acesso a recursos de estudo e, além disso, altamente envolvidos com o projeto de estudos do filho.

Em média, um aluno que estuda 40hrs por semana, frequenta todas as aulas, tem um alto nível de acesso a recursos e pais altamente envolvidos no processo de aprendizado, além de ter bons professores e estar motivado terá 23 pontos a mais no exame do que um aluno que não cumpre esses requisitos, muitos que não estão no controle nem do estudante nem da escola.

É importante ressaltar que, para todos esses casos, houve exceções, como alunos com dificuldades de aprendizado ou com pais de baixa escolaridade que apresentaram boas notas. No entanto, a existência de boas redes de suporte tende a facilitar o processo.

A conclusão é do projeto é que o desempenho dos estudantes depende de diversas variáveis, e o esforço do estudante melhora seu desempenho até certo nível, com o restante dependendo de fatores não estando sob o seu controle. Outro aspecto importante de ressaltar é que raramente um estudante tem as condições perfeitas para maximizar seu desempenho: pode não ter nenhuma dificuldade de aprendizados e se dedicar bastante, mas pode ter professores médios e um baixo nível de acesso a recursos. 

<h2> Especificação do Projeto </h2>

Para analisar os dados, utilizei o Excel e suas ferramentas de análise de dados, como utilização de tabelas dinâmicas para cruzar e analisar os dados numéricos e gráficos para visualizar distribuições e relações. Além disso, utilizei ferramentas estatísticas como testes t para averiguar a existência de diferenças entre as médias de notas de grupos diferentes e testes ANOVA de um fator para ver se diversas categorias diferentes tinham a mesma média ou não.

<h2> Projeto Detalhado </h2>

Os dados podem ser encontrados no [Kaggle - Student Performance Factors](https://www.kaggle.com/datasets/lainguyn123/student-performance-factors) e também no [Repositório do projeto no Github](https://github.com/eduardoguarienti/projeto_excel).

![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/dist_notas.png?raw=true){: width="650" height="650" }

O primeiro passo foi analisar a distribuição da variável-alvo, as notas dos estudantes, que aparentam seguir uma distribuição normal com a presença de alguns outliers à direita do gráfico: poucos alunos tem notas superiores a 80, e grande parte tem notas entre 57 e 76.

![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/rel_notas_horas_estudo.png?raw=true){: width="650" height="650" }

Há uma relação positiva entre horas de estudo e notas, com as horas de estudo explicando cerca de 20% das notas, e um aluno que estuda 40 horas tem, em média, 10 pontos a mais que um aluno que estuda entre 0 e 5 horas. Entretanto, não existe relação clara entre os alunos de melhor desempenho e mais horas de estudo.

![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/dist_notas_genero.png?raw=true){: width="650" height="650" }

Quanto ao gênero, não foi encontrada uma discrepância estatisticamente significativa, com o valor-p unilateral da diferença das médias entre o gênero feminino e o masculino de 0.34 > 0.05 (alfa).

| Teste-t: duas amostras presumindo variâncias diferentes | Var 1 (Fem) | Var 2 (Masc) |
| ------------------------------------------------------- | ----------- | ------------ |
| Média                                                   | 67.25842294 | 67.21902017  |
| Variância                                               | 16.4534518  | 14.17581214  |
| Observações                                             | 2790        | 3817         |
| Hipótese da diferença de média                          | 0           |              |
| gl (graus de liberdade)                                 | 5743        |              |
| Stat t                                                  | 0.401919318 |              |
| P(T<=t) uni-caudal                                      | 0.343879181 |              |
| t crítico uni-caudal                                    | 1.645118996 |              |
| P(T<=t) bi-caudal                                       | 0.687758362 |              |
| t crítico bi-caudal                                     | 1.960377142 |              |

![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/dist_notas_distancia.jpg?raw=true){: width="650" height="650" }

A distância da escola parece ser um fator determinante para o desempenho: os alunos próximos tem as melhores notas, enquanto os alunos que moram longe são os mais prejudicados.


![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/rel_notas_frequencia.png?raw=true){: width="650" height="650" }

Existe uma relação mais abrangente entre notas e frequência, e, em média, os alunos que mais comparecem tem uma nota 7 pontos maior do que os que menos comparecem às aulas. A frequência nas aulas explica cerca de 33% da variação das notas.

| Média de Notas        | Acesso a Recursos |        |       |
| Envolvimento Parental | Low   | Medium | High  |
| --------------------- | ----- | ------ | ----- |
| High                  | 66.92 | 67.98  | 69.13 |
| Medium                | 66.09 | 67.00  | 67.94 |
| Low                   | 65.23 | 66.32  | 67.05 |

O envolvimento parental e o acesso a recursos têm influência na nota final: um aluno com alto envolvimento parental e alto nível de acesso a recursos tem, em média, uma nota 4 pontos maior do que um aluno com baixo envolvimento parental e baixo acesso a recursos.
O acesso a recursos aparenta ser potencializado pelo envolvimento parental: quanto mais presentes os pais e maior o acesso a recursos, maior a nota final.

| Média de Notas         | Qualidade dos Professores |               |               |
| Nível de Motivação | Low   | Medium | High  |
| ------------------ | ----- | ------ | ----- |
| High               | 67.03 | 67.55  | 68.28 |
| Medium             | 66.99 | 67.25  | 67.63 |
| Low                | 65.95 | 66.57  | 67.34 |

Tanto a qualidade dos professores quanto o nível de motivação afetam positivamente a nota final, porém em um nível um pouco inferior em relação à tabela anterior.

![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/rel_notas_c_anteriores.png?raw=true){: width="650" height="650" }

![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/rel_notas_sono.png?raw=true){: width="650" height="650" }

Um insight menos intuitivo é que não foi encontrado relação entre as horas de sono e o desempenho nos exames, e não houve relação também com as notas anteriores.

![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/dist_notas_dific.png?raw=true){: width="650" height="650" }

Os alunos que não apresentam que não apresentam dificuldades de aprendizado tendem a ter notas maiores, cuja diferença é estatisticamente significativa. Para verificar, fiz o teste t unilateral para média de dois grupos, em que o valor-p foi de 3.75e-11, bastante inferior ao limiar de alpha = 0.05.

| Teste-t: duas amostras com variâncias diferentes | Var 1 (Média Sem Dific) | Var 2 (Média Com Dific.) |
| ------------------------------------------------ | ----------------------- | ------------------------ |
| Média                                            | 67.3482747              | 66.27769784              |
| Variância                                        | 14.84890922             | 16.5726287               |
| Observações                                      | 5912                    | 695                      |
| Hip. da diferença de média                       | 0                       |                          |
| gl                                               | 847                     |                          |
| Stat t                                           | 6.59429701              |                          |
| P(T<=t) uni-caudal                               | 3.75701E-11             |                          |
| t crítico uni-caudal                             | 1.646654627             |                          |
| P(T<=t) bi-caudal                                | 7.51402E-11             |                          |
| t crítico bi-caudal                              | 1.962768716             |                          |

![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/dist_notas_escola.png?raw=true){: width="650" height="650" }

Apesar da leve diferença por via do gráfico, o teste estatístico não indicou haver relação entre nota e o tipo de colégio, seja ele público ou privado.

| Teste-t: duas amostras presumindo variâncias diferentes | Var 1 (Privado) | Var 2 (Público) |
| ------------------------------------------------------- | --------------- | --------------- |
| Média                                                   | 67.28871129     | 67.21259501     |
| Variância                                               | 14.83314586     | 15.26865106     |
| Observações                                             | 2002            | 4605            |
| Hipótese da diferença de média                          | 0               |                 |
| gl                                                      | 3857            |                 |
| Stat t                                                  | 0.734991522     |                 |
| P(T<=t) uni-caudal                                      | 0.231194671     |                 |
| t crítico uni-caudal                                    | 1.645248788     |                 |
| P(T<=t) bi-caudal                                       | 0.462389343     |                 |
| t crítico bi-caudal                                     | 1.96057923      |                 |

![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/dist_notas_esc_pais.png?raw=true){: width="650" height="650" }

Há uma diferença estatisticamente significativa entre a escolaridade dos pais, e alunos com pais de maior escolaridade tendem a ter notas um pouco maiores.

| Anova: fator único - Escolaridade dos pais |              |          |             |               |
| ------------------------------------------ | ------------ | -------- | ----------- | ------------- |
| **RESUMO**                                 | **Contagem** | **Soma** | **Média**   | **Variância** |
| College                                    | 1293         | 87302    | 67.51894818 | 15.1228992    |
| High School                                | 1293         | 86993    | 67.27996906 | 15.51443591   |
| Postgraduate                               | 1293         | 87887    | 67.97138438 | 13.68571302   |
|                                            |              |          |             |               |
| **ANOVA**                                  | **SQ**       | **gl**   | **MQ**      | **F**         | **valor-P** | **F crítico** |
| Entre grupos                               | 318.8816705  | 2        | 159.4408353 | 10.7917331    | 2.11939E-05 | 2.998048847   |
| Dentro dos grupos                          | 57265.37819  | 3876     | 14.77434938 |               |             |               |
| Total                                      | 57584.25986  | 3878     |             |               |             |               |

![Desktop View](https://github.com/eduardoguarienti/projeto_excel/blob/main/dist_notas_inf_pares.png?raw=true){: width="650" height="650" }

A influência dos pares impacta positivamente na nota, e alunos com as notas mais elevadas tendem a ter ao menos uma neutralidade dos pares.

| Anova: fator único - Influência dos Pares |              |          |             |               |
| ----------------------------------------- | ------------ | -------- | ----------- | ------------- |
| **RESUMO**                                | **Contagem** | **Soma** | **Média**   | **Variância** |
| Positive                                  | 1360         | 91777    | 67.48308824 | 16.66196544   |
| Neutral                                   | 1360         | 91329    | 67.15367647 | 15.575336     |
| Negative                                  | 1360         | 90552    | 66.58235294 | 14.76142492   |
|                                           |              |          |             |               |
| **ANOVA**                                 | **SQ**       | **gl**   | **MQ**      | **F**         | **valor-P** | **F crítico** |
| Entre grupos                              | 564.9651961  | 2        | 282.482598  | 18.03129276   | 1.59786E-08 | 2.997934582   |
| Dentro dos grupos                         | 63871.26912  | 4077     | 15.66624212 |               |             |               |
| Total                                     | 64436.23431  | 4079     |             |               |             |               |
