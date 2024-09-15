---
layout: post
title: 'Python: EDA e Regressão'
description: Análise do impacto das mudanças climáticas na agricultura por meio de um modelo de Regressão Linear.
date: 2024-09-15 12:38 -0300
image:
  path: "https://www.kaggleusercontent.com/kf/195966573/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..PbTr1Q_WqN2AKQRNx0tamQ.gK5BL7D66a5RKcNGN3xZsd_lulIYsWJGs9yIwnlIIc5rlNpsNSrcfa_hz2E9EW_SbaYReQUKpD66iD0Nph6NOgQJCw4XRVoOtocD3PdARZlTJUDa1kiMN4t3tDabzd2jEfl6Yx0EkKYYJ5ONJz8EhxgjQtydQwid0SeDaP8Nz5rEK6gmg2kRReYI2ukuyNFFZP89oYzbarTnkCFvw_lF1X5qB8pt_WfESpQF7VbWba2xS5QS48G3xeD4Of5BKsMSZxAzB1hzfjMKjkUfZNwVsCjF4XGEGNC1vMgnNeVE-KfmDgslrWRYTv1dlrTpU_saB035XBuk3LGEq_tb037S--fOfhiZczi9DjM6cJ-FDkdVkC-b4rqb4jaHENlEcTIzOSIA2ZssrNRT6N7d1JssWimptRyELmHDJ1noTNXHtno2NqgCqzom7wC5hlivVy_BEQLJ9S0gPipc8bC2_UEt5LOTyascfoRVyT1FaHJRD6ydvWXKgtGBA0NYQ0AL90sWXdKXrwhIBiNCyrHva5JOh48bSE5Z0Dyxsb5E9gM9PBr0oDAtCoyvKyFVxRz_cW5tbERd4edz6pT3PnlSJ3rL11foXrw11fnPH4taoXa4oSl0NkJ8muS3ju946aWmiKKhEB5buTs5UyY1W4mJhnc2fzIQn7CY34iOW9_hPGyxCnTuhU7vKA7ZIfkL1qDQtCxY.66540WmO5Pchq5LKp314ZA/__results___files/__results___35_1.png"
  alt: "Relação encontrada entre Eventos Climáticos Extremos e Número de Estratégias de Adaptação"
categories: [Portfolio, Python]
tags: pandas visualização regressão 
math: true
permalink: /posts/projeto_python
---

<h2> Especificação e Resultados do Projeto </h2>

Com um conjunto de dados de agricultura e mudanças climáticas, procurei entender como as variáveis se relacionavam por meio de uma análise exploratória do conjunto por via do pacote `pandas` e visualizões com `matplotlib.pyplot` e `seaborn`. Na análise, descobri uma relação positiva entre médias de temperatura e produção, sugerindo que temperaturas mais altas tendem a produzir mais, que, por sua vez, correlaciona-se com o impacto econômico, sugerindo que o impacto económico é afetado em grande parte pela quantidade de commodities vendidas.

Além disso, quis identificar qual seria a relação entre o número de estratégias de adaptação, dado que faz sentido ajustar as estruturas de adaptação e resiliência climática com o surgimento de eventos climáticos adversos: com o surgimento de secas, sistemas de gerenciamento de água tornam-se importantes para a sustentabilidade do sistema produtivo.

Descobri uma relação positiva entre o número de estratégias de adaptação e o número de eventos climáticos extremos, dada por:

$$
\text{Estratégias de Adaptação} = 43 + 0.1255 \times \text{Extreme Weather Events}
$$

- São necessários, em média, 8 efeitos climáticos extremos para aumentar em 1 unidade as estratégias de adaptação.
- Com um R-quadrado ajustado de 0.599, 60% da variação em estratégias de adaptação são explicadas por eventos climáticos extremos.
- A F-Statistic é de 59 com um p-valor próximo de 0, indicando uma relação estatisticamente significativa.
- Ambos os coeficientes tem um p-valor abaixo de 0.05 indicando relação válida.

O projeto completo está disponível em [Kaggle - Quantifying Climate Resilience: A Linear Analysis](https://www.kaggle.com/code/eduardoguarienti/quantifying-climate-resilience-a-linear-analysis)

<h2> Processos e ferramentas </h2>
- Análise preliminar dos dados:
  ```python 
  df.sample(), df.shape, df.info(), df.describe()...
  ```
- Conversão dos dados de objeto para categóricos por facilidade de manuseio:
  ```python
  df['Country'] = df['Country'].astype('category')
  df['Region'] = df['Region'].astype('category')
  df['Crop_Type'] = df['Crop_Type'].astype('category')
  df['Adaptation_Strategies'] = df['Adaptation_Strategies'].astype('category')
  ```
  
- Análise descritiva: 
  ```python
  #frequência de variáveis categóricas
  df['Crop_Type'].value_counts()

  #estatísticas descritivas de variáveis numéricas
  df_num = df.select_dtypes(include=['int64', 'float64'])
  df_num.describe()
  df_num.skew()
  ```
- Visualização de distribuições:
  ```python
  # estilo e quantidade de figuras
  sns.set_style('darkgrid')
  fig, axes = plt.subplots(2, 2, figsize=(12,12))
  # gráficos de distribuição
  sns.histplot(df['Average_Temperature_C'], color='olive', kde=True, ax=axes[0,0])
  axes[0,0].set_title('Temperature Distribution')
  axes[0,0].set_xlabel('Average Temperature (Celsius)')
  axes[0,0].set_ylabel('Count')

  sns.histplot(df['Total_Precipitation_mm'], kde=True, ax=axes[0,1], color='steelblue')
  axes[0,1].set_title('Precipitation Distribution')
  axes[0,1].set_xlabel('Total Precipitation (milimeters)')
  axes[0,1].set_ylabel('Count')

  sns.histplot(df['CO2_Emissions_MT'], kde=True, ax=axes[1,0], color='dimgray')
  axes[1,0].set_title('Emissions Distribution')
  axes[1,0].set_xlabel('CO2 Emissions (tons)')
  axes[1,0].set_ylabel('Count')

  sns.histplot(df['Crop_Yield_MT_per_HA'], kde=True, ax=axes[1,1], color='orange')
  axes[1,1].set_title('Productivity Distribution')
  axes[1,1].set_xlabel('Crop Productivity (ton/ha)')
  axes[1,1].set_ylabel('Count')
  ```
![Desktop View](https://www.kaggleusercontent.com/kf/195966573/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..PbTr1Q_WqN2AKQRNx0tamQ.gK5BL7D66a5RKcNGN3xZsd_lulIYsWJGs9yIwnlIIc5rlNpsNSrcfa_hz2E9EW_SbaYReQUKpD66iD0Nph6NOgQJCw4XRVoOtocD3PdARZlTJUDa1kiMN4t3tDabzd2jEfl6Yx0EkKYYJ5ONJz8EhxgjQtydQwid0SeDaP8Nz5rEK6gmg2kRReYI2ukuyNFFZP89oYzbarTnkCFvw_lF1X5qB8pt_WfESpQF7VbWba2xS5QS48G3xeD4Of5BKsMSZxAzB1hzfjMKjkUfZNwVsCjF4XGEGNC1vMgnNeVE-KfmDgslrWRYTv1dlrTpU_saB035XBuk3LGEq_tb037S--fOfhiZczi9DjM6cJ-FDkdVkC-b4rqb4jaHENlEcTIzOSIA2ZssrNRT6N7d1JssWimptRyELmHDJ1noTNXHtno2NqgCqzom7wC5hlivVy_BEQLJ9S0gPipc8bC2_UEt5LOTyascfoRVyT1FaHJRD6ydvWXKgtGBA0NYQ0AL90sWXdKXrwhIBiNCyrHva5JOh48bSE5Z0Dyxsb5E9gM9PBr0oDAtCoyvKyFVxRz_cW5tbERd4edz6pT3PnlSJ3rL11foXrw11fnPH4taoXa4oSl0NkJ8muS3ju946aWmiKKhEB5buTs5UyY1W4mJhnc2fzIQn7CY34iOW9_hPGyxCnTuhU7vKA7ZIfkL1qDQtCxY.66540WmO5Pchq5LKp314ZA/__results___files/__results___19_1.png){: width="600" height="600" }

- Análise de correlações com `sns.heatmap`:
  ```python
  fig, ax = plt.subplots(figsize = (10,10))
  sns.set(color_codes = True)
  sns.heatmap(df_num.corr(), ax=ax, annot = True, linewidths = 0.05, fmt ='0.2f')
  plt.show()
  ```
- Criação de nova coluna especificando país e região para não haver duplicatas:
  ```python
  #we have to change the type of both country and region to object to group them
  df3 = df.copy()
  df3['Country'] = df3['Country'].astype('object')
  df3['Region'] = df3['Region'].astype('object')
  df3['Country-Region'] = df3['Country'] + '-' + df3['Region']
  df3 = df3.drop(['Country', 'Region'], axis=1)
  df3.head()
  ```
- Agrupamento dos dados por país e região e contagem do número de estratégias de adaptação e eventos climáticos adversos:
  ```python
  df3['Adaptation'] = np.where(df3['Adaptation_Strategies'] == 'No Adaptation', 0, 1)
  df_region = df3.groupby('Country-Region').agg({'Adaptation': 'sum', 'Extreme_Weather_Events': 'sum'}) \
        .sort_values(by='Extreme_Weather_Events', ascending=False)
  df_region.head()
  ```
- Modelo de regressão linear para quantificar a relação entre as variáveis:
  ```python
  import statsmodels.api as sm

  df_region['intercept'] = 1
  x = df_region[['intercept', 'Extreme_Weather_Events']]
  y = df_region['Adaptation']

  model = sm.OLS(y, x).fit()
  print(model.summary())
  ```
  ```
                              OLS Regression Results                            
==============================================================================
Dep. Variable:             Adaptation   R-squared:                       0.609
Model:                            OLS   Adj. R-squared:                  0.599
Method:                 Least Squares   F-statistic:                     59.17
Date:                Mon, 09 Sep 2024   Prob (F-statistic):           2.90e-09
Time:                        17:53:35   Log-Likelihood:                -149.12
No. Observations:                  40   AIC:                             302.2
Df Residuals:                      38   BIC:                             305.6
Df Model:                           1                                         
Covariance Type:            nonrobust                                         
==========================================================================================
                             coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------------------
intercept                 43.1477     20.379      2.117      0.041       1.893      84.402
Extreme_Weather_Events     0.1255      0.016      7.692      0.000       0.092       0.159
==============================================================================
Omnibus:                        6.585   Durbin-Watson:                   1.965
Prob(Omnibus):                  0.037   Jarque-Bera (JB):                2.746
Skew:                           0.332   Prob(JB):                        0.253
Kurtosis:                       1.901   Cond. No.                     1.56e+04
==============================================================================
Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The condition number is large, 1.56e+04. This might indicate that there are
strong multicollinearity or other numerical problems.
```

![Desktop View](https://www.kaggleusercontent.com/kf/195966573/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..PbTr1Q_WqN2AKQRNx0tamQ.gK5BL7D66a5RKcNGN3xZsd_lulIYsWJGs9yIwnlIIc5rlNpsNSrcfa_hz2E9EW_SbaYReQUKpD66iD0Nph6NOgQJCw4XRVoOtocD3PdARZlTJUDa1kiMN4t3tDabzd2jEfl6Yx0EkKYYJ5ONJz8EhxgjQtydQwid0SeDaP8Nz5rEK6gmg2kRReYI2ukuyNFFZP89oYzbarTnkCFvw_lF1X5qB8pt_WfESpQF7VbWba2xS5QS48G3xeD4Of5BKsMSZxAzB1hzfjMKjkUfZNwVsCjF4XGEGNC1vMgnNeVE-KfmDgslrWRYTv1dlrTpU_saB035XBuk3LGEq_tb037S--fOfhiZczi9DjM6cJ-FDkdVkC-b4rqb4jaHENlEcTIzOSIA2ZssrNRT6N7d1JssWimptRyELmHDJ1noTNXHtno2NqgCqzom7wC5hlivVy_BEQLJ9S0gPipc8bC2_UEt5LOTyascfoRVyT1FaHJRD6ydvWXKgtGBA0NYQ0AL90sWXdKXrwhIBiNCyrHva5JOh48bSE5Z0Dyxsb5E9gM9PBr0oDAtCoyvKyFVxRz_cW5tbERd4edz6pT3PnlSJ3rL11foXrw11fnPH4taoXa4oSl0NkJ8muS3ju946aWmiKKhEB5buTs5UyY1W4mJhnc2fzIQn7CY34iOW9_hPGyxCnTuhU7vKA7ZIfkL1qDQtCxY.66540WmO5Pchq5LKp314ZA/__results___files/__results___35_1.png){: width="600" height="600" }

- Análise da distrbuição dos resíduos da regressão linear - os resíduos precisam se assemelhar a uma distribuição normal, ilustrada pela linha vermelha:
```python
  from statsmodels.api import qqplot

  # qqplot of residuals to see if they follow a normal distribution
  fig, ax = plt.subplots(figsize=(8, 6))
  qqplot(data=model.resid, fit=True, line='45', ax=ax)
  plt.title('Q-Q Plot of Residuals')
  ```
![Desktop View](https://www.kaggleusercontent.com/kf/195966573/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..PbTr1Q_WqN2AKQRNx0tamQ.gK5BL7D66a5RKcNGN3xZsd_lulIYsWJGs9yIwnlIIc5rlNpsNSrcfa_hz2E9EW_SbaYReQUKpD66iD0Nph6NOgQJCw4XRVoOtocD3PdARZlTJUDa1kiMN4t3tDabzd2jEfl6Yx0EkKYYJ5ONJz8EhxgjQtydQwid0SeDaP8Nz5rEK6gmg2kRReYI2ukuyNFFZP89oYzbarTnkCFvw_lF1X5qB8pt_WfESpQF7VbWba2xS5QS48G3xeD4Of5BKsMSZxAzB1hzfjMKjkUfZNwVsCjF4XGEGNC1vMgnNeVE-KfmDgslrWRYTv1dlrTpU_saB035XBuk3LGEq_tb037S--fOfhiZczi9DjM6cJ-FDkdVkC-b4rqb4jaHENlEcTIzOSIA2ZssrNRT6N7d1JssWimptRyELmHDJ1noTNXHtno2NqgCqzom7wC5hlivVy_BEQLJ9S0gPipc8bC2_UEt5LOTyascfoRVyT1FaHJRD6ydvWXKgtGBA0NYQ0AL90sWXdKXrwhIBiNCyrHva5JOh48bSE5Z0Dyxsb5E9gM9PBr0oDAtCoyvKyFVxRz_cW5tbERd4edz6pT3PnlSJ3rL11foXrw11fnPH4taoXa4oSl0NkJ8muS3ju946aWmiKKhEB5buTs5UyY1W4mJhnc2fzIQn7CY34iOW9_hPGyxCnTuhU7vKA7ZIfkL1qDQtCxY.66540WmO5Pchq5LKp314ZA/__results___files/__results___39_1.png){: width="600" height="600" }
- Análise de Homoscedasticidade - os resíduos precisam ter a mesma variância (por exemplo, não aumentar ou diminuir ao longo do eixo x) e não pode haver algum padrão claro no gráfico, pois os erros não devem apresentar padrões:
  ```python
  # Get the fitted values from the regression model
  fitted_values = model.fittedvalues

  # Create a scatter plot of residuals vs fitted values
  # This plot helps check for homoscedasticity (constant variance of residuals)
  plt.scatter(fitted_values, model.resid)
  plt.xlabel('Fitted Values')
  plt.ylabel('Residuals')
  plt.title('Residuals vs Fitted Values')
  plt.axhline(y=0, color='r', linestyle='--')  # Add a horizontal reference line at y=0
  plt.show()
  ```
![Desktop View](https://www.kaggleusercontent.com/kf/195966573/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..PbTr1Q_WqN2AKQRNx0tamQ.gK5BL7D66a5RKcNGN3xZsd_lulIYsWJGs9yIwnlIIc5rlNpsNSrcfa_hz2E9EW_SbaYReQUKpD66iD0Nph6NOgQJCw4XRVoOtocD3PdARZlTJUDa1kiMN4t3tDabzd2jEfl6Yx0EkKYYJ5ONJz8EhxgjQtydQwid0SeDaP8Nz5rEK6gmg2kRReYI2ukuyNFFZP89oYzbarTnkCFvw_lF1X5qB8pt_WfESpQF7VbWba2xS5QS48G3xeD4Of5BKsMSZxAzB1hzfjMKjkUfZNwVsCjF4XGEGNC1vMgnNeVE-KfmDgslrWRYTv1dlrTpU_saB035XBuk3LGEq_tb037S--fOfhiZczi9DjM6cJ-FDkdVkC-b4rqb4jaHENlEcTIzOSIA2ZssrNRT6N7d1JssWimptRyELmHDJ1noTNXHtno2NqgCqzom7wC5hlivVy_BEQLJ9S0gPipc8bC2_UEt5LOTyascfoRVyT1FaHJRD6ydvWXKgtGBA0NYQ0AL90sWXdKXrwhIBiNCyrHva5JOh48bSE5Z0Dyxsb5E9gM9PBr0oDAtCoyvKyFVxRz_cW5tbERd4edz6pT3PnlSJ3rL11foXrw11fnPH4taoXa4oSl0NkJ8muS3ju946aWmiKKhEB5buTs5UyY1W4mJhnc2fzIQn7CY34iOW9_hPGyxCnTuhU7vKA7ZIfkL1qDQtCxY.66540WmO5Pchq5LKp314ZA/__results___files/__results___40_0.png){: width="600" height="600" }
