---
layout: post
title: Modelo Manual de Regressão Linear/Python
date: 2024-10-09 09:36 -0300
description: Criação de funções que replicam o funcionamento de um modelo de Reg. Linear do Scikit-Learn.
image:
  path: "https://github.com/eduardoguarienti/eduardoguarienti.github.io/blob/main/assets/matplotlib.png?raw=true"
  alt: "Regressão Linear encontrada (visualização do matplotlib)"
---

Nesse pequeno projeto procurei replicar a maneira como uma regressão linear é feita na biblioteca `scikit-learn`, criando funções para separar o conjunto de dados entre treino e teste, criar uma regressão por meio dos quadrados mínimos ordinários via `numpy`, prever valores novos e, por fim, medir a acurácia e adequação do modelo por meio de funções que calculam o erro quadrático médio e o coeficiente de determinação. O notebook do projeto está disponível no repositório do [Github](https://github.com/eduardoguarienti/outros/tree/main).

Para visualizar a regressão resultante:

<iframe src="/assets/plotly_3d_graph.html" width="110%" height="650px"></iframe>

<h2> Especificação do Projeto </h2>

Os dados utilizados são o mesmo do projeto do excel, disponível em [Kaggle - Student Performance Factors](https://www.kaggle.com/datasets/lainguyn123/student-performance-factors).

- Função para dividir os dados em treino e teste por meio de amostragem aleatória replicável (utilizando `random_state`):
  ```python
  def dividir_treino_teste(X, y, prop_teste=0.3, random_state=None):
    """
    divide o conjunto de dados em treino e teste
    """
    X_train = X.sample(frac=1-prop_teste, random_state=random_state)
    X_test = X.drop(X_train.index)
    y_train = y.loc[X_train.index]
    y_test = y.loc[X_test.index]
    return X_train, X_test, y_train, y_test
  ```
- Função de regressão utilizando mínimos quadrados ordinários com um hiperparâmetro, a opção da habilitar ou desabilitar o intercepto:
  ```python
  def reg_ols(X_train, y_train, intercepto=True):
    """
    retorna os betas da regressao linear com
    minimos quadrados ordinarios
    """
    x = X_train.copy()
    if intercepto:
        #adicionar intercepto
        if 'intercepto' not in x.columns:
            x['intercepto'] = 1
    else:
        #remover intercepto caso falso
        if 'intercepto' in x.columns:
            x = x.drop('intercepto', axis=1)

    #conversao de x e y para formatos adequados
    X = np.array(x)
    y = np.array(y_train).reshape(-1,1)

    # beta = (X^T * X)^(-1) * X^T * y
    X_transposto = X.T
    b = np.linalg.inv(X_transposto.dot(X)).dot(X_transposto).dot(y)
    return b
  ```
- Função para fazer a predição de novos valores, em que é necessário especificar se foi utilizado intercepto ou não:
  ```python
  def predict_ols(X_test, beta, intercepto=True):
    """
    retorna a predicao de valores novos
    """
    x = X_test.copy()
    #adicionar intercepto caso nao tenha
    if intercepto:
        if 'intercepto' not in x.columns:
            x['intercepto'] = 1

    #converter p array para op. matematicas
    X = np.array(x)
    predicoes = X.dot(beta)
    return predicoes
  ```
- Funções para mensurar a acurácia do modelo:
  ```python
  def r2_ols(y_test, y_pred):
    """
    retorna o r2 score
    """
    #conversao p array e p mesmo shape
    y_test = np.array(y_test)
    y_pred = np.array(y_pred).flatten()
    
    #calculo do SSres
    SSres = np.sum((y_test - y_pred)**2)
    
    #calculo do SStot
    y_obs_mean = np.mean(y_test)
    SStot = np.sum((y_test-y_obs_mean)**2)

    #calculo do r2
    r2 = 1 - (SSres/SStot)
    return r2

  def mse(y_test, y_pred):
    """
    retorna o erro quadratico medio
    """
    #conversao p array
    y_test = np.array(y_test)
    y_pred = np.array(y_pred).flatten()
    return np.mean((y_test - y_pred) ** 2)
  ```
- Comparação dos resultados do modelo criado com o modelo da biblioteca sklearn:
  ```python
  r2_treino = r2_ols(y_train, y_pred_train)
  mse_treino = mse(y_train, y_pred_train)
  r2_teste = r2_ols(y_test, y_pred)
  mse_teste = mse(y_test, y_pred)

  print('---- modelo manual ----')
  print('intercepto:', beta[2])
  print('coeficientes:', beta[0], beta[1])
  print(f'mse (conj. teste): {mse_teste:.2f}')
  print(f'r2 score (conj. teste): {r2_teste:.2f}')
  print('----'*6)
  print('---- modelo sklearn ----')
  print('intercepto:', np.round(regr.intercept_,8))
  print('coeficientes:', regr.coef_)
  print(f'mse (conj. teste): {mean_squared_error(y_test, sklearn_y_pred):.2f}')
  print(f'r2 score (conj. teste) {r2_score(y_test, sklearn_y_pred):.2f}')
  print('----'*6)
  ```
  ```
  ---- modelo manual ----
  intercepto: [45.52872936]
  coeficientes: [0.29178043] [0.19864831]
  mse (conj. teste): 6.52
  r2 score (conj. teste): 0.56
  ------------------------
  ---- modelo sklearn ----
  intercepto: 45.52872936
  coeficientes: [0.29178043 0.19864831]
  mse (conj. teste): 6.52
  r2 score (conj. teste) 0.56
  ------------------------
  ```
