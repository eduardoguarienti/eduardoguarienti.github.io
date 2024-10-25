---
layout: post
title: Report e EDA no Power BI
description: report e análise exploratória de vendas no Power BI
date: 2024-09-15 11:31 -0300
image:
  path: "https://github.com/eduardoguarienti/projeto_powerbi/blob/main/proj_powerbi1.png?raw=true"
  alt: "Página 3 do Report"
permalink: /posts/projeto_powerbi
---

<h2> Sobre o projeto </h2>

Em meu projeto de Power BI, procurei entender como as variáveis-chave do negócio se relacionavam, além de buscar a melhor visualização e interatividade possível para os diferentes dashboards, tentando encontrar o equilíbrio entre riqueza de informações e clareza, usando diferentes tipos de gráficos e tabelas. Também fiz uso de várias medidas DAX para entender de forma sintética a performance do negócio, como margens, quantidade de clientes e vendas totais.

<h2> Medidas utilizadas </h2>

- `M Avg Sales per Transaction = [M: Total Sales]/ DISTINCTCOUNT('Sample - Superstore'[Order ID])`
- `M Customer Count = DISTINCTCOUNT('Sample - Superstore'[Customer ID])`
- `M Profit Margin = DIVIDE([M Total Profits],[M: Total Sales])`
- `M Total Profits = SUM('Sample - Superstore'[Profit])`
- `M Total Units Sold = SUM('Sample - Superstore'[Quantity])`
- `M Total Sales = SUM('Sample - Superstore'[Sales])`
  
<h2> Dados utilizados </h2>

Utilizei os dados de um grande varejo estadunidense especializado diferentes gamas de produtos de escritório, que, dentre outras informações, continha ID do pedido, data do pedido, segmento e localização do cliente, além de informações sobre vendas e lucros. Disponível em [Kaggle - Superstore Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final/code?datasetId=1940216&sortBy=voteCount)

<h2> Report </h2>

<iframe title="projeto_powerbi_loja" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiZTYxNTU1ZmUtOGMyMy00NWYyLTk1Y2MtZjNiMTRiYjE4YWJjIiwidCI6IjBiYzA2NzRiLWZmMWEtNDVkZC05ZThiLTg5NTIwZGUzMzMxYSJ9&pageName=52d3a55840e8fff3227b" frameborder="0" allowFullScreen="true"></iframe>


