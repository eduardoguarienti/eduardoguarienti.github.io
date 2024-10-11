---
layout: post
title: Report Absenteísmo/Power BI
date: 2024-10-11 11:39 -0300
image:
  path: "https://github.com/eduardoguarienti/posts_imgs/blob/main/report.png?raw=true"
  alt: "Pág. 1 do Report"
---

*[Repositório do Github do Report](https://github.com/eduardoguarienti/report_absenteismo_pbi)*

Nesse projeto procurei descobrir as causas do Absenteísmo em um conjunto de dados de uma firma brasileira de correios entre os anos de 2007-2010, em que descobri que grande parte das horas de trabalho perdidas foram justificadas, em grande parte a enfermidades que acometeram os funcionários, e apenas 4.5% das faltas foram injustificadas.

No que tange a temporalidade, o mês com mais tempo de ausências é julho, durante o inverno, e durante a semana centra-se nos primeiros dias da semana de trabalho.

As enfermidades, que corresponderam a 60% das ausências, podem ter causas relacionadas à natureza do trabalho: 70% dos trabalhadores moram a longas ou muito longas distâncias do local de trabalho e, dada a pequena discrepâncias entre gastos de transporte, possivelmente viajam por transporte público por longos períodos de tempo. Dentre as condições específicas que acometem os funcionários, as doenças do sistema osteomuscular e lesões de causas externas correspondem a 36.5% das horas faltadas. Tendo em vista que estamos lidando com uma firma de transporte e correios, seria adequado investigar as condições de trabalho dos funcionários, com atenção a possíveis esforços demasiados e a questões de ergonomia. Além disso, mais de 1/4 dos funcionários têm ao menos o primeiro grau de obesidade (conforme os critérios da OMS) o que requer condições de trabalho que evitem esforços físicos excessivos.

Ao avaliar as ausências por tempo total, as causas mais frequentes de faltas são por fisioterapia e por consultas médicas, revelando um cenário de acompanhamento e recuperação das capacidades físicas. Além disso, grande parte dos funcionários estão na faixa dos 30 anos, o que revela que esforços físicos médios até pesados não seriam um grande problema, desde que estejam fisicamente aptos e em forma. Além disso, também é importante ressaltar que, dentre os funcionários que mais faltam em horas, as falhas disciplinares são 2% menos frequentes em relação aos que menos faltam.

<iframe title="report_abs" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiMGFlNTM1MTYtZDdiZS00ODk4LWE3NTMtYjk5ZjE5YWY0ODVjIiwidCI6IjBiYzA2NzRiLWZmMWEtNDVkZC05ZThiLTg5NTIwZGUzMzMxYSJ9&pageName=e378dd438e83460453c1" frameborder="0" allowFullScreen="true"></iframe>

<h2> Exemplos de fórmulas DAX utilizadas </h2>

```
_ProporcaoAusenciaInjustificada = 
DIVIDE(
    CALCULATE(COUNTROWS(Absenteeism_at_work), Absenteeism_at_work[Reason for absence] = 26),
    COUNTROWS(Absenteeism_at_work),
    0
)

_ProporcaoOsteomuscularLesoes = 
DIVIDE(
    CALCULATE(
        COUNTROWS(Absenteeism_at_work), 
        Absenteeism_at_work[Reason for absence] IN {13, 19}
    ),
    COUNTROWS(Absenteeism_at_work),
    0
)

CategoriaDistancia = 
SWITCH(
    TRUE(),
    Absenteeism_at_work[Distance from Residence to Work] <= 15, "Média (5-15km)",
    Absenteeism_at_work[Distance from Residence to Work] <= 30, "Longa Distância (15-30km)",
    Absenteeism_at_work[Distance from Residence to Work] <= 52, "Muito Longa Distância (30-52km)"
)

TotalAbsenteismoPorID = 
CALCULATE(
    SUM(Absenteeism_at_work[Absenteeism time in hours]),
    ALLEXCEPT(Absenteeism_at_work, Absenteeism_at_work[ID])
)


descricao_ausencia = 
SWITCH(
    TRUE(),
    Absenteeism_at_work[Reason for absence] = 1, "Certas doenças infecciosas e parasitárias",
    Absenteeism_at_work[Reason for absence] = 2, "Neoplasias",
    Absenteeism_at_work[Reason for absence] = 3, "Doenças do sangue e dos órgãos hematopoéticos e alguns transtornos envolvendo o mecanismo imunológico",
    Absenteeism_at_work[Reason for absence] = 4, "Doenças endócrinas, nutricionais e metabólicas",
    Absenteeism_at_work[Reason for absence] = 5, "Transtornos mentais e comportamentais",
    Absenteeism_at_work[Reason for absence] = 6, "Doenças do sistema nervoso",
    Absenteeism_at_work[Reason for absence] = 7, "Doenças do olho e anexos",
    Absenteeism_at_work[Reason for absence] = 8, "Doenças do ouvido e processo mastoide",
    Absenteeism_at_work[Reason for absence] = 9, "Doenças do sistema circulatório",
    Absenteeism_at_work[Reason for absence] = 10, "Doenças do sistema respiratório",
    Absenteeism_at_work[Reason for absence] = 11, "Doenças do sistema digestivo",
    Absenteeism_at_work[Reason for absence] = 12, "Doenças da pele e do tecido subcutâneo",
    Absenteeism_at_work[Reason for absence] = 13, "Doenças do sistema osteomuscular e do tecido conjuntivo",
    Absenteeism_at_work[Reason for absence] = 14, "Doenças do sistema geniturinário",
    Absenteeism_at_work[Reason for absence] = 15, "Gravidez, parto e puerpério",
    Absenteeism_at_work[Reason for absence] = 16, "Algumas afecções originadas no período perinatal",
    Absenteeism_at_work[Reason for absence] = 17, "Malformações congênitas, deformidades e anomalias cromossômicas",
    Absenteeism_at_work[Reason for absence] = 18, "Sintomas, sinais e achados anormais de exames clínicos e laboratoriais, não classificados em outra parte",
    Absenteeism_at_work[Reason for absence] = 19, "Lesões, envenenamentos e algumas outras consequências de causas externas",
    Absenteeism_at_work[Reason for absence] = 20, "Causas externas de morbidade e mortalidade",
    Absenteeism_at_work[Reason for absence] = 21, "Fatores que influenciam o estado de saúde e o contato com os serviços de saúde",
    Absenteeism_at_work[Reason for absence] = 22, "Acompanhamento do paciente",
    Absenteeism_at_work[Reason for absence] = 23, "Consulta médica",
    Absenteeism_at_work[Reason for absence] = 24, "Doação de sangue",
    Absenteeism_at_work[Reason for absence] = 25, "Exame laboratorial",
    Absenteeism_at_work[Reason for absence] = 26, "Ausência injustificada",
    Absenteeism_at_work[Reason for absence] = 27, "Fisioterapia",
    Absenteeism_at_work[Reason for absence] = 28, "Consulta odontológica"
)



```
