---
layout: post
title: Panorama dos Acidentes Evitáveis de Trânsito no Brasil/SQL
date: 2024-10-23 12:05 -0300
permalink: /posts/projeto_transito_sql
---

Por meio do SQL, nesse projeto tentei descobrir as causas dos acidentes de trânsito que se situam do lado mais evitável dentro das possíveis causas dos acidentes de trânsito no Brasil. Para tal, utilizei as diversas ferramentas disponíveis no SQL para inserir a base de dados e analisar os dados disponíveis. 

<h2> Dados e ETL </h2>

A primeira parte foi pegar os [csv's disponíveis](https://www.kaggle.com/datasets/mcamera/brazil-highway-traffic-accidents), em que utilizei os dados de 2007 a 2016. Para carregar os dados, foi necessário limpar os dados por via do Excel e Power Query, em que podei os espaços em branco presentes, homogeneizei os valores que representam os nulos e inseri valores nulos aos valores não preenchidos. A partir disso, carreguei sequencialmente os arquivos no PostgreSQL por via do seguinte código:
```sql
--criação de tabela
CREATE TABLE acidentes (
    id INT,
    pesid INT,
    data_inversa DATE,
    dia_semana VARCHAR(50),
    horario TIME,
    uf CHAR(2),
    br INTEGER,
    km DECIMAL(8, 2),
    municipio VARCHAR(100),
    causa_acidente VARCHAR(255),
    tipo_acidente VARCHAR(255),
    classificacao_acidente VARCHAR(50),
    fase_dia VARCHAR(50),
    sentido_via VARCHAR(50),
    condicao_metereologica VARCHAR(50),
    tipo_pista VARCHAR(50),
    tracado_via VARCHAR(50),
    uso_solo VARCHAR(50),
    id_veiculo INTEGER,
    tipo_veiculo VARCHAR(50),
    marca VARCHAR(50),
    ano_fabricacao_veiculo INTEGER,
    tipo_envolvido VARCHAR(50),
    estado_fisico VARCHAR(50),
    idade INTEGER,
    sexo VARCHAR(50),
    nacionalidade VARCHAR(50),
    naturalidade VARCHAR(100)
);

--data dd/mm/aaaa
SET datestyle = 'DMY';

--inserção dos dados
COPY acidentes (
    id, 
    pesid, 
    data_inversa, 
    dia_semana, 
    horario, 
    uf, 
    br, 
    km, 
    municipio, 
    causa_acidente, 
    tipo_acidente, 
    classificacao_acidente, 
    fase_dia, 
    sentido_via, 
    condicao_metereologica, 
    tipo_pista, 
    tracado_via, 
    uso_solo, 
    id_veiculo, 
    tipo_veiculo, 
    marca, 
    ano_fabricacao_veiculo, 
    tipo_envolvido, 
    estado_fisico, 
    idade, 
    sexo, 
    nacionalidade, 
    naturalidade
)
FROM 'C:\\Program Files\\PostgreSQL\\16\\data\\acidentes2016.csv'
WITH (
    DELIMITER ',',
    FORMAT CSV,
    HEADER,
    ENCODING 'UTF8',
    NULL 'NA'
);

--conferir encodificação
SELECT DISTINCT dia_semana
FROM acidentes;

--conferir maximo de data
SELECT MAX(data_inversa)
FROM acidentes;
```

A tabela possui 3.369.428 registros de 3.369.305 pessoas diferentes envolvidas em 1.563.042 acidentes.

```sql
--numero de registros na tabela
SELECT 
	COUNT(*) AS num_registros,
	COUNT(DISTINCT pesid) AS num_pessoas,
	COUNT(DISTINCT id) AS num_acidentes
FROM acidentes;

--causas de acidentes
SELECT 
	causa_acidente, 
	COUNT(*),
	ROUND(COUNT(*) / (SELECT COUNT(*) FROM acidentes)::decimal, 4) AS proporcao,
	ROUND(SUM(COUNT(*)) OVER (ORDER BY COUNT(*) DESC) 
          / (SELECT COUNT(*) FROM acidentes)::decimal, 4) AS prop_cumulativa
FROM acidentes
GROUP BY causa_acidente
ORDER BY COUNT(*) DESC;
```

| num_registros | num_pessoas | num_acidentes |
| ------------- | ----------- | ------------- |
| 3,369,428     | 3,369,305   | 1,563,042     |

<h2> Análise Exploratória dos Dados </h2>

Dentre as causas mais comuns dos acidentes, somente a falta de atenção é a causa de 1/3 dos acidentes totais. Em conjunção com outras causas potencialmente evitáveis, temos quase 60% dos acidentes em rodovias que poderiam ser evitados:

| causa_acidente                     | count     | proporcao | prop_cumulativa |
| ---------------------------------- | --------- | --------- | --------------- |
| Falta de atenção                   | 1,155,447 | 0.3429    | 0.3429          |
| Outras                             | 904,461   | 0.2684    | 0.6114          |
| Não guardar distância de segurança | 358,843   | 0.1065    | 0.7179          |
| Velocidade incompatível            | 249,569   | 0.0741    | 0.7919          |
| Desobediência à sinalização        | 158,781   | 0.0471    | 0.8390          |
| Ingestão de álcool                 | 134,768   | 0.0400    | 0.8790          |
| Defeito mecânico em veículo        | 121,913   | 0.0362    | 0.9152          |
| Ultrapassagem indevida             | 100,902   | 0.0299    | 0.9452          |
| Dormindo                           | 77,196    | 0.0229    | 0.9681          |
| Animais na Pista                   | 64,059    | 0.0190    | 0.9871          |
| Defeito na via                     | 43,471    | 0.0129    | 1.0000          |

Com isso, criei uma *view* no SQL para facilitar o acesso tanto aos acidentes cujas causas foram evitáveis tanto nos acidentes cujas causas não foram evitáveis:

```sql
--view para analisar os acidentes evitaveis
CREATE VIEW acidentes_evitaveis AS
SELECT *
FROM acidentes
WHERE causa_acidente IN ('Falta de atenção', 
                         'Não guardar distância de segurança', 
                         'Velocidade incompatível', 
                         'Desobediência à sinalização', 
                         'Ingestão de álcool', 
                         'Ultrapassagem indevida', 
                         'Dormindo')

--view para analisar os acidentes não evitaveis
CREATE VIEW acidentes_nao_evitaveis AS
SELECT *
FROM acidentes
WHERE causa_acidente NOT IN ('Falta de atenção', 
                         'Não guardar distância de segurança', 
                         'Velocidade incompatível', 
                         'Desobediência à sinalização', 
                         'Ingestão de álcool', 
                         'Ultrapassagem indevida', 
                         'Dormindo');
```                    

Seguindo com a análise do perfil dos acidentes, o intuito foi de descobrir se havia algum padrão específico nesse tipo de acidente, dada sua predominância em relação ao total, com isso, a ênfase foi em descobrir os porquês dos acidentes: como, quando e onde ocorreram. Primeiro, tentei descobrir quando ocorriam:

```sql
--mais comuns durante o final de semana
SELECT dia_semana, COUNT(*)
FROM acidentes_evitaveis
GROUP BY dia_semana
ORDER BY COUNT(*) DESC;

--mais comuns em dezembro, seguidas por janeiro e julho 
--(essas ultimas podem nao ser estatisticamente significativas)
SELECT EXTRACT(MONTH FROM data_inversa), COUNT(*)
FROM acidentes_evitaveis
GROUP BY 1
ORDER BY 2 DESC;

--maioria no periodo da tarde e da manha
SELECT 
    CASE 
        WHEN EXTRACT(HOUR FROM horario) BETWEEN 0 AND 5 THEN 'Madrugada'
        WHEN EXTRACT(HOUR FROM horario) BETWEEN 6 AND 11 THEN 'Manhã'
        WHEN EXTRACT(HOUR FROM horario) BETWEEN 12 AND 17 THEN 'Tarde'
        WHEN EXTRACT(HOUR FROM horario) BETWEEN 18 AND 23 THEN 'Noite'
    END AS periodo,
    COUNT(*) AS total_acidentes
FROM acidentes_evitaveis
GROUP BY periodo
ORDER BY total_acidentes DESC;
```

O que se destaca do perfil temporal dos acidentes é que grande parte dos ocorridos são durante o final de semana, sugerindo que muitos dos acidentes que são evitáveis ocorrem devido a deslocações não laborais, como turismo ou visitas. O mês mais comum para os acidentes é dezembro, por uma larga margem, seguido por janeiro, dois meses comuns para férias. Em relação ao momento do dia, grande parte ocorre no período da manhã ou da tarde.

| dia_semana | count   |
| ---------- | ------- |
| Sexta      | 370,811 |
| Sábado     | 348,441 |
| Domingo    | 343,993 |
| Segunda    | 307,195 |
| Quinta     | 300,974 |
| Quarta     | 286,672 |
| Terça      | 277,420 |

| mes_ano | count   |
| ------- | ------- |
| 12      | 220,451 |
| 1       | 195,891 |
| 7       | 189,192 |
| 5       | 187,102 |
| 4       | 186,099 |
| 10      | 183,972 |
| 3       | 182,996 |
| 6       | 181,402 |
| 11      | 179,015 |
| 9       | 178,973 |
| 8       | 178,209 |
| 2       | 172,204 |

| periodo   | total_acidentes |
| --------- | --------------- |
| Tarde     | 772,393         |
| Manhã     | 673,348         |
| Noite     | 590,781         |
| Madrugada | 198,984         |

```sql
--grande parte dos acidentes sao colisao traseira e lateral
SELECT 
	tipo_acidente, 
	COUNT(DISTINCT id),
    ROUND(
        COUNT(DISTINCT id) 
        / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(DISTINCT id)) OVER (ORDER BY COUNT(DISTINCT id) DESC) 
        / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
	acidentes_evitaveis
GROUP BY 
	tipo_acidente
ORDER BY 
	COUNT(DISTINCT id) DESC;

-- 92% dos acidentes não têm vítimas fatais
-- Entretanto, 113,502 vítimas fatais evitáveis em um período de 10 anos é um valor muito elevado
SELECT 
    classificacao_acidente, 
    COUNT(DISTINCT pesid) AS num_pessoas,
    ROUND(
        COUNT(DISTINCT pesid) 
        / (SELECT COUNT(DISTINCT pesid) FROM acidentes_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(DISTINCT pesid)) OVER (ORDER BY COUNT(DISTINCT pesid) DESC) 
        / (SELECT COUNT(DISTINCT pesid) FROM acidentes_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_evitaveis
GROUP BY 
    classificacao_acidente
ORDER BY 
    num_pessoas DESC;

-- Grande parte dos acidentes são em linha reta ou em curva
SELECT
    tracado_via,
    COUNT(DISTINCT id) AS total_acidentes,
    ROUND(
        COUNT(DISTINCT id) / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(DISTINCT id)) OVER (ORDER BY COUNT(DISTINCT id) DESC)
        / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_evitaveis
GROUP BY 
    tracado_via
ORDER BY 
    total_acidentes DESC;
```

Partindo para análise de como os acidentes ocorreram, é perceptível que 1/3 desses acidentes são de colisão traseira ou lateral, que geralmente ocorrem por falta de atenção. Entre os acidentes que são comumente mais graves, temos colisões transversais, que tendem a ocorrer em cruzamentos, com 13%, e saídas de pista, com 12%.

| tipo_acidente                              | count   | proporcao | prop_cumulativa |
| ------------------------------------------ | ------- | --------- | --------------- |
| Colisão traseira                           | 336,994 | 0.3428    | 0.3428          |
| Colisão lateral                            | 183,074 | 0.1862    | 0.5291          |
| Colisão Transversal                        | 132,018 | 0.1343    | 0.6634          |
| Saída de Pista                             | 116,914 | 0.1189    | 0.7823          |
| Colisão com objeto fixo                    | 46,258  | 0.0471    | 0.8294          |
| Colisão frontal                            | 39,429  | 0.0401    | 0.8695          |
| Capotamento                                | 36,940  | 0.0376    | 0.9071          |
| Tombamento                                 | 29,276  | 0.0298    | 0.9369          |
| Atropelamento de pessoa                    | 23,275  | 0.0237    | 0.9605          |
| Queda de motocicleta / bicicleta / veículo | 18,584  | 0.0189    | 0.9794          |
| Colisão com bicicleta                      | 12,643  | 0.0129    | 0.9923          |
| Colisão com objeto móvel                   | 3,139   | 0.0032    | 0.9955          |
| Derramamento de Carga                      | 1,884   | 0.0019    | 0.9974          |
| Danos Eventuais                            | 1,648   | 0.0017    | 0.9991          |
| Atropelamento de animal                    | 758     | 0.0008    | 0.9999          |
| Incêndio                                   | 130     | 0.0001    | 1.0000          |

Mais de 94% dos acidentes não são demasiadamente graves. Entretanto, 5% dos acidentes tem vítimas fatais, correspondendo a 113,502 pessoas que perderam a vida no período por acidentes de carro por essas causas.

| classificacao_acidente | num_pessoas | proporcao | prop_cumulativa |
| ---------------------- | ----------- | --------- | --------------- |
| Sem Vítimas            | 1,196,027   | 0.5350    | 0.5350          |
| Com Vítimas Feridas    | 920,184     | 0.4116    | 0.9467          |
| Com Vítimas Fatais     | 113,502     | 0.0508    | 0.9974          |
| Ignorado               | 5,700       | 0.0025    | 1.0000          |

Quanto ao traçado da via, 91% dos acidentes são em retas ou em curvas, com 9% em cruzamentos.

| tracado_via | total_acidentes | proporcao | prop_cumulativa |
| ----------- | --------------- | --------- | --------------- |
| Reta        | 700,800         | 0.7129    | 0.7129          |
| Curva       | 196,762         | 0.2002    | 0.9131          |
| Cruzamento  | 85,400          | 0.0869    | 1.0000          |

```sql
-- 65% dos acidentes ocorrem com céu claro ou sol
-- 33% em nublado ou chuva
SELECT
    condicao_metereologica AS condicao_meteorologica,
    COUNT(DISTINCT id),
    ROUND(
        COUNT(DISTINCT id) 
        / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(DISTINCT id)) OVER (ORDER BY COUNT(DISTINCT id) DESC)
        / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_evitaveis
GROUP BY 
    condicao_meteorologica
ORDER BY 
    COUNT(*) DESC;

-- Nos não evitáveis, 59% ocorrem com céu claro ou sol
-- 37% em nublado ou chuva
SELECT
    condicao_metereologica AS condicao_meteorologica,
    COUNT(DISTINCT id),
    ROUND(
        COUNT(DISTINCT id) 
        / (SELECT COUNT(DISTINCT id) FROM acidentes_nao_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(DISTINCT id)) OVER (ORDER BY COUNT(DISTINCT id) DESC)
        / (SELECT COUNT(DISTINCT id) FROM acidentes_nao_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_nao_evitaveis
GROUP BY 
    condicao_meteorologica
ORDER BY 
    COUNT(*) DESC;
```

Nas condições meteorológicas, há um leve aumento de condições adversas nos acidentes não evitáveis: a chuva sobe 3% de 16% para 19% da condição meteorológica do momento do acidente e, tendo em vista a quantidade de registros, é quase certo que é uma relação estatisticamente significativa.

| acidentes evitáveis |
| condicao_meteorologica | count   | proporcao | prop_cumulativa |
| ---------------------- | ------- | --------- | --------------- |
| Céu Claro              | 476,835 | 0.4851    | 0.4851          |
| Nublado                | 179,793 | 0.1829    | 0.6680          |
| Chuva                  | 157,717 | 0.1605    | 0.8285          |
| Sol                    | 140,932 | 0.1434    | 0.9718          |
| Ignorada               | 15,475  | 0.0157    | 0.9876          |
| Nevoeiro/neblina       | 9,602   | 0.0098    | 0.9973          |
| Vento                  | 2,376   | 0.0024    | 0.9998          |
| Granizo                | 140     | 0.0001    | 0.9999          |
| Neve                   | 60      | 0.0001    | 1.0000          |

| acidentes não evitáveis |
| condicao_meteorologica | count   | proporcao | prop_cumulativa |
| ---------------------- | ------- | --------- | --------------- |
| Céu Claro              | 277,239 | 0.4779    | 0.4779          |
| Chuva                  | 113,718 | 0.1960    | 0.6740          |
| Nublado                | 103,490 | 0.1784    | 0.8524          |
| Sol                    | 63,883  | 0.1101    | 0.9625          |
| Ignorada               | 12,979  | 0.0224    | 0.9849          |
| Nevoeiro/neblina       | 6,640   | 0.0114    | 0.9964          |
| Vento                  | 1,907   | 0.0033    | 0.9996          |
| Granizo                | 152     | 0.0003    | 0.9999          |
| Neve                   | 45      | 0.0001    | 1.0000          |

```sql
-- 57% em locais urbanos e 42% em locais rurais
SELECT
    uso_solo,
    COUNT(DISTINCT id),
    ROUND(
        COUNT(DISTINCT id) / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(DISTINCT id)) OVER (ORDER BY COUNT(DISTINCT id) DESC)
        / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_evitaveis
GROUP BY 
    uso_solo
ORDER BY 
    COUNT(DISTINCT id) DESC;

-- o que se repete nos não evitáveis
SELECT
    uso_solo,
    COUNT(DISTINCT id),
    ROUND(
        COUNT(DISTINCT id) / (SELECT COUNT(DISTINCT id) FROM acidentes_nao_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(DISTINCT id)) OVER (ORDER BY COUNT(DISTINCT id) DESC)
        / (SELECT COUNT(DISTINCT id) FROM acidentes_nao_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_nao_evitaveis
GROUP BY 
    uso_solo
ORDER BY 
    COUNT(*) DESC;
```

Não aparenta haver diferenças entre a região ser urbana ou rural: ambos os tipos ocorrem na mesma proporção.

| evitáveis |
| uso_solo | count   | proporcao | prop_cumulativa |
| -------- | ------- | --------- | --------------- |
| Urbano   | 566,744 | 0.5766    | 0.5766          |
| Rural    | 416,220 | 0.4234    | 1.0000          |

| não evitáveis |
| uso_solo | count   | proporcao | prop_cumulativa |
| -------- | ------- | --------- | --------------- |
| Rural    | 331,578 | 0.5716    | 0.5716          |
| Urbano   | 248,485 | 0.4284    | 1.0000          |

```sql
-- 51% automóveis
-- 10% motos (menos proteção -> maior chance de acidentes graves)
-- 33% veículos grandes (mais inércia -> maior chance de acidentes graves)
SELECT
    tipo_veiculo,
    COUNT(*),
    ROUND(
        COUNT(*) / (SELECT COUNT(*) FROM acidentes_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(*)) OVER (ORDER BY COUNT(*) DESC)
        / (SELECT COUNT(*) FROM acidentes_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_evitaveis
GROUP BY 
    tipo_veiculo
ORDER BY 
    COUNT(*) DESC;

-- 44% automóveis
-- 10% motos
-- 36% veículos grandes
-- Acidentes não evitáveis são possivelmente mais graves que os evitáveis
SELECT
    tipo_veiculo,
    COUNT(*) AS num_acidentes,
    ROUND(
        COUNT(*) / (SELECT COUNT(*) FROM acidentes_nao_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(*)) OVER (ORDER BY COUNT(*) DESC) 
        / (SELECT COUNT(*) FROM acidentes_nao_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_nao_evitaveis
GROUP BY 
    tipo_veiculo
ORDER BY 
    COUNT(*) DESC;
```

Quanto a proporção dos tipos de veículos envolvidos nesses acidentes, os automóveis correspondem a mais da metade e, em relação aos acidentes não evitáveis, correspondem a um incremento de 7%. A proporção de veículos grandes também é menor para os acidentes evitáveis.

| acidentes evitáveis |
| tipo_veiculo         | count     | proporcao | prop_cumulativa |
| -------------------- | --------- | --------- | --------------- |
| Automóvel            | 1,148,493 | 0.5138    | 0.5138          |
| Motocicletas         | 220,806   | 0.0988    | 0.6125          |
| Caminhão             | 210,520   | 0.0942    | 0.7067          |
| Caminhão-Trator      | 205,824   | 0.0921    | 0.7988          |
| Caminhonete          | 163,715   | 0.0732    | 0.8720          |
| Ônibus               | 80,799    | 0.0361    | 0.9081          |
| Camioneta            | 76,737    | 0.0343    | 0.9425          |
| Veículo Desconhecido | 27,249    | 0.0122    | 0.9547          |
| Motoneta             | 21,958    | 0.0098    | 0.9645          |
| Microônibus          | 21,071    | 0.0094    | 0.9739          |
| Bicicleta            | 20,202    | 0.0090    | 0.9829          |
| Não identificado     | 16,665    | 0.0075    | 0.9904          |
| Utilitário           | 12,823    | 0.0057    | 0.9961          |
| Ciclomotor           | 3,748     | 0.0017    | 0.9978          |
| Trator de rodas      | 1,071     | 0.0005    | 0.9983          |
| Carroça              | 979       | 0.0004    | 0.9987          |
| Semi-Reboque         | 958       | 0.0004    | 0.9992          |
| Caminhão-Tanque      | 901       | 0.0004    | 0.9996          |
| Reboque              | 301       | 0.0001    | 0.9997          |
| Bonde / Trem         | 184       | 0.0001    | 0.9998          |
| Triciclo             | 157       | 0.0001    | 0.9998          |
| Charrete             | 127       | 0.0001    | 0.9999          |
| Trator misto         | 87        | 0.0000    | 0.9999          |
| Carro-de-mão         | 62        | 0.0000    | 1.0000          |
| Trator de esteiras   | 44        | 0.0000    | 1.0000          |
| Quadriciclo          | 12        | 0.0000    | 1.0000          |
| Motor-Casa           | 8         | 0.0000    | 1.0000          |
| Side-car             | 5         | 0.0000    | 1.0000          |

Dado que os veículos grandes têm uma inércia muito superior a carros de passeio e são mais comuns nos acidentes não evitáveis, é possível que haja mais acidentes graves nessa categoria.

| acidentes não evitáveis |
| tipo_veiculo         | num_acidentes | proporcao | prop_cumulativa |
| -------------------- | ------------- | --------- | --------------- |
| Automóvel            | 509,947       | 0.4497    | 0.4497          |
| Caminhão             | 125,416       | 0.1106    | 0.5603          |
| Caminhão-Trator      | 125,410       | 0.1106    | 0.6709          |
| Motocicletas         | 114,138       | 0.1007    | 0.7716          |
| Caminhonete          | 78,963        | 0.0696    | 0.8412          |
| Ônibus               | 56,723        | 0.0500    | 0.8913          |
| Camioneta            | 38,832        | 0.0342    | 0.9255          |
| Veículo Desconhecido | 25,604        | 0.0226    | 0.9481          |
| Microônibus          | 13,724        | 0.0121    | 0.9602          |
| Não identificado     | 12,615        | 0.0111    | 0.9713          |
| Motoneta             | 11,371        | 0.0100    | 0.9813          |


```sql
-- ranking dos estados
-- Liderado por MG, SC e PR
-- Surpreendentemente, SP em quinto lugar
SELECT 
    uf AS localizacao, 
    COUNT(DISTINCT id) AS quantidade,
    RANK() OVER (ORDER BY COUNT(DISTINCT id) DESC) AS ranking
FROM 
    acidentes_evitaveis
GROUP BY 
    localizacao
ORDER BY 
    quantidade DESC;

-- ranking dos municipios
SELECT 
    uf || ', ' || municipio AS localizacao, 
    COUNT(DISTINCT id) AS quantidade,
    RANK() OVER (ORDER BY COUNT(DISTINCT id) DESC) AS ranking
FROM 
    acidentes_evitaveis
GROUP BY 
    localizacao
ORDER BY 
    quantidade DESC
LIMIT
	10;

```

No ranking dos estados por acidentes evitáveis, temos MG, SC e PR como os três primeiros lugares por uma boa margem. Surpreendentemente, SP, que tem a maior população do país, está em quinto lugar. Outro destaque é o ES, que tem quase a mesma quantidade que o RJ, um estado 5x maior.

| localizacao  | quantidade | ranking |
| ------------ | ---------- | ------- |
| MG           | 136,834    | 1       |
| SC           | 131,631    | 2       |
| PR           | 115,815    | 3       |
| RS           | 91,105     | 4       |
| SP           | 62,586     | 5       |
| RJ           | 56,244     | 6       |
| ES           | 47,705     | 7       |
| BA           | 44,239     | 8       |
| GO           | 39,029     | 9       |
| PE           | 38,450     | 10      |
| PA           | 25,248     | 11      |
| MT           | 22,973     | 12      |
| PB           | 21,583     | 13      |
| RN           | 21,317     | 14      |
| RO           | 19,099     | 15      |
| CE           | 18,766     | 16      |
| MS           | 18,232     | 17      |
| PI           | 15,856     | 18      |
| MA           | 14,041     | 19      |
| AL           | 11,600     | 20      |
| SE           | 9,147      | 21      |
| DF           | 7,834      | 22      |
| TO           | 6,065      | 23      |
| AC           | 2,775      | 24      |
| RR           | 2,187      | 25      |
| AP           | 1,436      | 26      |
| AM           | 1,161      | 27      |
| Desconhecido | 6          | 28      |

Todos os municípios ou são capitais de estados ou são adjacentes às capitais, com destaque para SC, com duas cidades com alto fluxo de carros próxima a capital, e ES, também com duas cidades. Portanto, os acidentes evitáveis em rodovias federais ocorrem, em grande parte, em grandes centros urbanos.

| localizacao     | quantidade | ranking |
| --------------- | ---------- | ------- |
| PR, CURITIBA    | 16,795     | 1       |
| SC, SAO JOSE    | 15,105     | 2       |
| ES, SERRA       | 11,533     | 3       |
| MG, BETIM       | 10,083     | 4       |
| ES, CARIACICA   | 9,770      | 5       |
| SP, GUARULHOS   | 9,502      | 6       |
| PA, ANANINDEUA  | 9,323      | 7       |
| SC, PALHOCA     | 9,004      | 8       |
| PI, TERESINA    | 8,702      | 9       |
| RO, PORTO VELHO | 8,554      | 10      |

```sql
-- 53% ocorre em pistas simples
-- 38% em pistas duplas
-- 8% em pistas múltiplas
-- Conferir a fundo, pois pista simples -> 
-- maior chance de colisões frontais -> maior chance de acidentes graves
SELECT
    tipo_pista,
    COUNT(*),
    ROUND(
        COUNT(*) / (SELECT COUNT(*) FROM acidentes_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(*)) OVER (ORDER BY COUNT(*) DESC)
        / (SELECT COUNT(*) FROM acidentes_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_evitaveis
GROUP BY 
    tipo_pista
ORDER BY 
    COUNT(*) DESC;

-- 54% em pistas simples
-- 39% em pistas duplas
-- 6% em pistas múltiplas
SELECT
    tipo_pista,
    COUNT(*),
    ROUND(
        COUNT(*) / (SELECT COUNT(*) FROM acidentes_nao_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(*)) OVER (ORDER BY COUNT(*) DESC) 
        / (SELECT COUNT(*) FROM acidentes_nao_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_nao_evitaveis
GROUP BY 
    tipo_pista
ORDER BY 
    COUNT(*) DESC;

-- Predomínio de colisões traseiras sem vítimas
-- Vítimas fatais correspondem a menos de 2% do total
-- Entretanto, análise é prejudicada pois há mts valores faltantes na classificação do acidente
SELECT 
    tipo_acidente,
    classificacao_acidente, 
    COUNT(DISTINCT id),
    ROUND(
        COUNT(DISTINCT id) / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(DISTINCT id)) OVER (ORDER BY COUNT(DISTINCT id) DESC)
        / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_evitaveis
WHERE 
    tipo_pista = 'Simples'
GROUP BY 
    tipo_acidente, classificacao_acidente
ORDER BY 
    COUNT(*) DESC;
```

As pistas simples são a maioria dos acidentes em ambos os subtipos de acidentes. Entretanto, pistas duplas e pistas múltiplas, que evitam o contato entre sentidos diferentes, ainda assim possuem uma boa fração da quantidade de acidentes totais.

| evitáveis |
| tipo_pista   | count   | proporcao | prop_cumulativa |
| ------------ | ------- | --------- | --------------- |
| Simples      | 612,424 | 0.5401    | 0.5401          |
| Dupla        | 443,730 | 0.3913    | 0.9314          |
| Múltipla     | 77,744  | 0.0686    | 1.0000          |
| Desconhecido | 6       | 0.0000    | 1.0000          |

| não evitáveis |
| tipo_pista | count     | proporcao | prop_cumulativa |
| ---------- | --------- | --------- | --------------- |
| Simples    | 1,195,072 | 0.5346    | 0.5346          |
| Dupla      | 859,477   | 0.3845    | 0.9191          |
| Múltipla   | 180,950   | 0.0809    | 1.0000          |

Entre os tipos de colisões que mais geram acidentes de trânsito com feridos, temos a colisão transversal - em que a colisão se dá num ângulo de 90 graus, e a colisão traseira. O tipo de colisão mais perigoso é a colisão frontal, em que a chance de ser fatal é maior que a chance de não ser fatal. Entretanto, há muitos valores faltantes, e há somente cerca de 50% dos registros totais de acidentes com a classificação de gravidade.

| tipo_acidente           | classificacao_acidente | count   | proporcao | prop_cumulativa |
| ----------------------- | ---------------------- | ------- | --------- | --------------- |
| Colisão traseira        | Sem Vítimas            | 105,498 | 0.1073    | 0.1073          |
| Colisão Transversal     | Com Vítimas Feridas    | 48,865  | 0.0497    | 0.2108          |
| Colisão lateral         | Sem Vítimas            | 52,833  | 0.0537    | 0.1611          |
| Colisão traseira        | Com Vítimas Feridas    | 33,165  | 0.0337    | 0.3240          |
| Colisão Transversal     | Sem Vítimas            | 39,975  | 0.0407    | 0.2515          |
| Colisão lateral         | Com Vítimas Feridas    | 28,343  | 0.0288    | 0.3832          |
| Colisão frontal         | Com Vítimas Feridas    | 19,634  | 0.0200    | 0.4031          |
| Saída de Pista          | Com Vítimas Feridas    | 29,837  | 0.0304    | 0.3543          |
| Saída de Pista          | Sem Vítimas            | 38,126  | 0.0388    | 0.2902          |
| Colisão frontal         | Com Vítimas Fatais     | 9,142   | 0.0093    | 0.4440          |
| Capotamento             | Com Vítimas Feridas    | 11,034  | 0.0112    | 0.4144          |
| Atropelamento de pessoa | Com Vítimas Feridas    | 8,679   | 0.0088    | 0.4620          |
| Tombamento              | Com Vítimas Feridas    | 9,750   | 0.0099    | 0.4347          |
| Colisão frontal         | Sem Vítimas            | 6,243   | 0.0064    | 0.4844          |
| Colisão com bicicleta   | Com Vítimas Feridas    | 5,977   | 0.0061    | 0.4905          |
| Colisão com objeto fixo | Sem Vítimas            | 10,251  | 0.0104    | 0.4248          |

*Na tabela acima, foram removidos os registros que representam menos de 1% do total

```sql
SELECT 
    tipo_acidente,
    classificacao_acidente AS classf_acidente, 
    COUNT(DISTINCT id),
    ROUND(
        COUNT(DISTINCT id) / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(DISTINCT id)) OVER (ORDER BY COUNT(DISTINCT id) DESC)
        / (SELECT COUNT(DISTINCT id) FROM acidentes_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_evitaveis
WHERE 
    tipo_pista = 'Simples'
GROUP BY 
    tipo_acidente, classificacao_acidente
ORDER BY 
    COUNT(DISTINCT id) DESC;

-- Muitos dados faltantes para o estado físico dos passageiros
-- Mais da metade dos valores presentes são de colisões 
-- (traseiras, laterais e transversais) em que todos os membros não se machucaram
-- Menos de 3% dos valores presentes são de feridos graves
SELECT 
    tipo_pista,
    tipo_acidente,
    estado_fisico,
    COUNT(*) AS quantidade,
    ROUND(
        COUNT(*) / (SELECT COUNT(*) FROM acidentes_evitaveis)::decimal, 4
    ) AS proporcao,
    ROUND(
        SUM(COUNT(*)) OVER (ORDER BY COUNT(*) DESC)
        / (SELECT COUNT(*) FROM acidentes_evitaveis)::decimal, 4
    ) AS prop_cumulativa
FROM 
    acidentes_evitaveis
WHERE 
    tipo_pista = 'Simples'
GROUP BY 
    tipo_pista, tipo_acidente, estado_fisico
ORDER BY 
    COUNT(*) DESC;


```

Para as pistas simples, o panorama é semelhante: as colisões tranversais e frontais são as mais perigosas. Entretanto, há também muitos dados faltantes: menos de 60% dos acidentes totais.

| tipo_acidente                              | classf_acidente     | count   | proporcao | prop_cumulativa |
| ------------------------------------------ | ------------------- | ------- | --------- | --------------- |
| Colisão traseira                           | Sem Vítimas         | 105,498 | 0.1073    | 0.1073          |
| Colisão lateral                            | Sem Vítimas         | 52,833  | 0.0537    | 0.1611          |
| Colisão Transversal                        | Com Vítimas Feridas | 48,865  | 0.0497    | 0.2108          |
| Colisão Transversal                        | Sem Vítimas         | 39,975  | 0.0407    | 0.2515          |
| Saída de Pista                             | Sem Vítimas         | 38,126  | 0.0388    | 0.2902          |
| Colisão traseira                           | Com Vítimas Feridas | 33,165  | 0.0337    | 0.3240          |
| Saída de Pista                             | Com Vítimas Feridas | 29,837  | 0.0304    | 0.3543          |
| Colisão lateral                            | Com Vítimas Feridas | 28,343  | 0.0288    | 0.3832          |
| Colisão frontal                            | Com Vítimas Feridas | 19,634  | 0.0200    | 0.4031          |
| Capotamento                                | Com Vítimas Feridas | 11,034  | 0.0112    | 0.4144          |
| Colisão com objeto fixo                    | Sem Vítimas         | 10,251  | 0.0104    | 0.4248          |
| Tombamento                                 | Com Vítimas Feridas | 9,750   | 0.0099    | 0.4347          |
| Colisão frontal                            | Com Vítimas Fatais  | 9,142   | 0.0093    | 0.4440          |
| Tombamento                                 | Sem Vítimas         | 9,016   | 0.0092    | 0.4532          |
| Atropelamento de pessoa                    | Com Vítimas Feridas | 8,679   | 0.0088    | 0.4620          |
| Queda de motocicleta / bicicleta / veículo | Com Vítimas Feridas | 8,555   | 0.0087    | 0.4707          |
| Capotamento                                | Sem Vítimas         | 7,197   | 0.0073    | 0.4780          |
| Colisão frontal                            | Sem Vítimas         | 6,243   | 0.0064    | 0.4844          |
| Colisão com bicicleta                      | Com Vítimas Feridas | 5,977   | 0.0061    | 0.4905          |
| Colisão com objeto fixo                    | Com Vítimas Feridas | 5,885   | 0.0060    | 0.4965          |
| Colisão Transversal                        | Com Vítimas Fatais  | 3,170   | 0.0032    | 0.4997          |
| Atropelamento de pessoa                    | Com Vítimas Fatais  | 2,422   | 0.0025    | 0.5022          |

Vendo pela coluna de estado_fisico, que aparenta ter uma quantidade semelhante de valores preenchidos em relação à classificação do acidente, é possível perceber que um pouco mais de 1/4 dos passageiros saem ilesos dos acidentes de trânsito, o que é demasiadamente pouco tendo em vista que são acidentes que poderiam não ter ocorrido.

| tipo_pista | tipo_acidente       | estado_fisico | quantidade | proporcao | prop_cumulativa |
| ---------- | ------------------- | ------------- | ---------- | --------- | --------------- |
| Simples    | Colisão traseira    | Ileso         | 300,475    | 0.1344    | 0.1344          |
| Simples    | Colisão lateral     | Ileso         | 148,897    | 0.0666    | 0.2010          |
| Simples    | Colisão Transversal | Ileso         | 142,486    | 0.0637    | 0.2648          |
| Simples    | Colisão Transversal | Ferido Leve   | 58,809     | 0.0263    | 0.2911          |
| Simples    | Saída de Pista      | Ileso         | 57,554     | 0.0257    | 0.3168          |
| Simples    | Colisão frontal     | Ileso         | 44,134     | 0.0197    | 0.3365          |
| Simples    | Colisão traseira    | Ferido Leve   | 41,801     | 0.0187    | 0.3552          |
| Simples    | Saída de Pista      | Ferido Leve   | 39,899     | 0.0178    | 0.3731          |
| Simples    | Colisão frontal     | Ferido Leve   | 34,588     | 0.0155    | 0.3886          |
| Simples    | Colisão lateral     | Ferido Leve   | 33,770     | 0.0151    | 0.4037          |
| Simples    | Colisão frontal     | Ferido Grave  | 25,631     | 0.0115    | 0.4151          |
| Simples    | Colisão Transversal | Ferido Grave  | 22,968     | 0.0103    | 0.4254          |

Dado tanto o tamanho quanto a quantidade de registros da tabela, acredito ainda que seja possível tirar vários novos insights dessa tabela, em especial as tabelas a partir do ano de 2017, em que há registros de latitude e longitude, entre outras novas informações.
