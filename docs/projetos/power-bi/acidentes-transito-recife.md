# Análise de Incidentes de Trânsito — Cidade do Recife

<div class="video-container">
  <video class="video-doc" autoplay loop muted playsinline>
    <source src="../assets/navegacao-relatorio-incidentes-transito-recife.mp4" type="video/mp4">
  </video>
</div>

!!! info "Importante"
    A documentação deste projeto foi estruturada com base na metodologia **ESI (Entendimento, Solução e Implantação)**, criada e propagada pelo Mestre em Power BI <a href="https://www.linkedin.com/in/leokarpa/" target="_blank" rel="noopener">Leonardo Karpinski</a> em sua Formação em Análise de Dados na plataforma de ensino <a href="https://xperiun.com/ed/plataforma/" target="_blank" rel="noopener">Xperiun</a> e amplamente utilizada em projetos analíticos para organizar raciocínio, decisões técnicas e comunicação de resultados.

---

## Visão Geral

Este projeto realiza uma **análise descritiva dos Chamados de Sinistros (Acidentes) de Trânsito com e sem vitimas 2015 a 2024 em Recife/PE**, explorando padrões temporais, espaciais e relacionados aos modais envolvidos nos acidentes.


Para este projeto, é entendido como modal (ou modalidade) a forma como a pessoa locomove-se pela cidade.

O objetivo principal é auxiliar na identificação de **distribuições geográficas dos bairros, padrões de ocorrência ao longo do tempo e características associadas aos incidentes**, utilizando dashboards interativos desenvolvidos no Microsoft Power BI.

O projeto foi construído como **projeto de portfólio profissional**, aplicando práticas de:

- Modelagem dimensional

- Extração, Tratamento e Limpeza dos dados (ETL) com Power Query

- Construção de métricas em linguagem DAX

- Design de dashboards analíticos produzidos no Figma

- Documentação técnica estruturada utilizando MkDocs

---

## 1. Entendimento

### 1.1 Contexto do Problema

Sabemos que acidentes de trânsito representam um importante problema urbano, impactando:

- Segurança pública

- Sistema de saúde

- Mobilidade urbana

- Planejamento viário


A análise sistemática desses dados permite identificar:

- Áreas com maior concentração de incidentes

- Períodos com maior ocorrência

- Modais mais frequentemente envolvidos

- Impactos em termos de vítimas e fatalidades


Essas informações são essenciais para apoiar **políticas públicas, planejamento urbano e ações de prevenção**.

---

#### 1.1.1 Objetivo do Projeto

Desde o ano 2020, a Autarquia de Trânsito e Transporte Urbano do Recife (CTTU) divulga relatórios anuais de segurança viária <a href="https://cttu.recife.pe.gov.br/relatorios-anuais-de-seguranca-viaria" target="_blank" rel="noopener">(clique aqui para ver)</a>. Portanto, o relatório do ano 2024 foi utilizado como base para criação desse projeto. Como o relatório anual tem sua própria metodologia e utilização de visuais estatísticos, o projeto trouxe uma abordagem diferente focada em detalhes ao invés de trazer o que já foi apresentado.

Os principais objetivos analíticos foram:

- Identificar **distribuição geográfica dos incidentes por bairro**

- Analisar **evolução temporal das informações relacionadas aos acidentes por meses em um determinado ano**

- Avaliar **participação dos diferentes modais** nos incidentes

- Medir **impacto em vítimas e vítimas fatais em diferentes contextos**

- Calcular **taxa de mortalidade associada aos incidentes e quando há um modal envolvido**

- Explorar **padrões de ocorrência por dia e horário no mês**

Além disso, buscou-se estruturar um relatório com foco em:

- Comparabilidade temporal

- Exploração interativa

- Leitura analítica rápida


---

#### 1.1.2 Escopo e Requisitos

O escopo do projeto contempla:

- Análise descritiva dos incidentes

- Métricas de vítimas e fatalidades

- Segmentação por bairro

- Segmentação por modalidade

- Análise temporal por ano, mês, dia e horário

- Visualização geográfica

- Dashboards altamente interativos

Fora do escopo:

- Modelagem preditiva

- Identificação de causas dos acidentes

- Avaliação de políticas públicas

- Análise comportamental dos condutores


---

### 1.2 Dados Utilizados

Os dados utilizados correspondem a registros históricos de **incidentes de trânsito na cidade do Recife** disponibilizado no  <a href="https://dados.recife.pe.gov.br/dataset/acidentes-de-transito-com-e-sem-vitimas" target="_blank" rel="noopener">Portal de Dados Abertos da Prefeitura do Recife</a>.

Características principais da base:

- Granularidade principal: **um registro por incidente**

- Estrutura contendo informações sobre:

  - Data do incidente

  - Hora

  - Bairro

  - Modalidades envolvidas

  - Quantidade de vítimas

  - Quantidade de vítimas fatais

Há outras informações sobre condições da via, presenças de placas, velocidade máxima da via, tipo de acidente, etecera, que foram analisados, mas não foram inclusos no projeto.

A base possui particularidades importantes:

- Um incidente pode envolver **mais de um modal**

- Nem todos os incidentes possuem vítimas


#### 1.2.1 Inconsistências da base

Durante a análise dos dados antes de iniciar o processo de ETL foram identificadas algumas inconsistências nos dados disponibilizados:

- Há um incidente no ano 2015 que na coluna *ônibus* há uma quantidade de 404 ônibus envolvidos no incidente. Esse registro foi excluído para manter a consistência das estatísticas.

- O ano 2022 não possui horário em nenhum incidente da base. Mas, foram mantidos na base. 

- Para os anos 2023 e 2024 há somente incidentes até entre 01h00 e 12h59. Também foram mantidos. Apesa de impactar nas métricas de *Year over* (comparações com o ano anterior).

- Em vários anos, há incidentes que não foi informado a quantidade envolvida no modal, apesar da descrição do incidente fornecer detalhes. Esses incidentes, que resultam um total de 1.438 registros, foram excluídos da base no processo de modelagem dimensinal para não prejudicar cálculos de agregação.  

Somando os incidentes de todas as bases, há mais de 72 mil registros. Os incidentes foram filtrados para permanecer somente aqueles que possuíam informação que estava "finalizado" ou "em atendimento", elinminando os demais que possuíam registro como duplicado, cancelado ou com informação inválida.

Realizando todas as operações de ETL e modelagem, **ficaram 58.797 registros. Este é o número considerado para todo o relatório.**

Nos dasbhboards foram colocados alguns avisos, quando for pertinente o caso. 

---

## 2. Solução

### 2.1 Extração dos Dados

As bases de dados foram extraídas manualmente do <a href="https://dados.recife.pe.gov.br/dataset/acidentes-de-transito-com-e-sem-vitimas" target="_blank" rel="noopener">Portal de Dados Abertos da Prefeitura do Recife</a> em formato de arquivo *csv* e posteriormente foram importadas para o **Power BI Desktop**, onde foi realizada uma análise inicial para identificar:

- Estrutura da base

- Tipos de dados

- Campos disponíveis

- Possíveis inconsistências

Essa etapa permitiu compreender o **modelo relacional implícito na base original**.

---

### 2.2 Transformação dos Dados (ETL)

A etapa de transformação foi conduzida no **Power Query**.

Principais transformações realizadas:

- Padronização de tipos de dados

- Tratamento de valores nulos

- Criação de dimensões auxiliares

- Preparação da estrutura para modelagem dimensional


Também foi construída uma **tabela de modalidades** para representar corretamente a relação **muitos-para-muitos entre incidentes e modais**.

---

### 2.3 Modelagem dos Dados

Foi adotado um **modelo dimensional em estrela**, estruturado da seguinte forma:

Tabelas Fato

**fato_acidentes**

Granularidade: um registro por incidente. Apenas uma linha para o incidente. 

Contém métricas relacionadas a:

- quantidade de vítimas

- quantidade de vítimas fatais

---

**fato_modalidades**

Granularidade: um registro por **incidente e modalidade envolvida**. Várias linhas para o mesmo incidente dependendo da quantidade de modais envolvidos. 

Essa tabela permite analisar:

- participação dos modais

- contagem de incidentes por tipo de veículo


Nesta etapa ocorrem a defasagem dos dados. No cenário ideal, a tabela fato_acidentes e fato_modalidades deveriam ter a mesma quantidade de incidentes visto que ambas são exatamente iguais, mudando apenas a estrutura da granularidade. Como os incidentes sem registro da quantidade de modais não envolvidos foram excluídos, há incidentes na tabela fato_acidentes que não estão presentes na tabela fato_modalidades. Consequentemente, as quantidades de vítimas feridas e vítimas fatais também são reduzidas. 

---

Tabelas Dimensão

- **dim_calendário**

- **dim_hora**

- **dim_modalidade**

- **dim_bairro**

As dimensões permitem segmentação analítica por:

- tempo

- localização

- tipo de veículo envolvido


---

Estratégia de Integração entre Fatos

Como algumas métricas estão na tabela **fato_acidentes**, foi utilizada a função **TREATAS em DAX** para permitir que filtros aplicados na tabela **fato_modalidades** também afetem medidas provenientes da tabela **fato_acidentes**, visto que não é recomendado relacionamento físico entre tabelas fato.

Essa estratégia garante consistência nas análises quando o usuário filtra por modalidade.

---

### 2.4 Cálculos e Métricas

As métricas foram desenvolvidas utilizando **DAX**.

Principais indicadores implementados:

**Incidentes**

- Total de Incidentes

- Total de Incidentes Ano Anterior

- Variação Absoluta

- Variação Percentual


**Incidentes com Vítimas**

- Total de Incidentes com Vítimas

- Comparação com ano anterior

- Variação absoluta

- Variação percentual


**Quantidade de Vítimas**

- Quantidade total de vítimas

- Comparação com ano anterior

- Variação absoluta

- Variação percentual

**Quantidade de Vítimas Fatais**

- Quantidade de vítimas fatais

- Comparação com ano anterior

- Variação absoluta

- Variação percentual

- Taxa de Mortalidade


O cálculo dos incidentes com vítimas feridas e vítimas fatais foi realizado utilizando uma coluna da *natureza_acidente* enquanto a quantidade foi utilizada a coluna de *vitimas* e *vitimasfatais*. 

---

### 2.5 Visualização e Dashboards

O relatório foi estruturado em **dois dashboards principais**.

Todo o design foi desenvolvido previamente no **Figma**, garantindo consistência visual e organização das informações.

---

**Dashboard 1 — Panorama Geral**

Incidentes, Bairros e Modalidades

Este dashboard apresenta uma visão macro dos acidentes na cidade.

**Filtros disponíveis**

- Ano

- Modalidade

Por padrão, **todas as modalidades estão selecionadas**, garantindo visão consolidada dos incidentes.

---

**Indicadores principais**

O dashboard apresenta quatro indicadores principais em formato de **cartões analíticos**:

1. Total de Incidentes

2. Total de Incidentes com Vítimas

3. Quantidade de Vítimas

4. Quantidade de Vítimas Fatais


Cada cartão inclui:

- valor do ano atual

- valor do ano anterior

- variação absoluta

- variação percentual

No caso das vítimas fatais também é exibida:

- taxa de mortalidade

---

Comparação Temporal

Ao lado de cada cartão há um **gráfico de linha**, mostrando:

- evolução anual do indicador

- comparação entre **ano atual e ano anterior**

Esse recurso permite identificar tendências temporais.

---

Análise Geográfica por Bairro

O dashboard possui uma **matriz detalhada por bairro**, contendo:

- Ranking do bairro

- % de participação no total de incidentes

- total de incidentes

- incidentes com vítimas

- quantidade de vítimas

- quantidade de vítimas fatais

---

Visualização Geográfica

Dois botões permitem alternar entre:

- **tabela detalhada**

- **mapa com bolhas**

No mapa, cada bairro é representado por uma bolha proporcional ao volume de incidentes.

O **tooltip do mapa** exibe as mesmas métricas presentes na matriz.

---

**Dashboard 2 — Calendário e Horário**

Este dashboard explora a **distribuição temporal dos incidentes**.

---

Filtros disponíveis

- Ano

- Mês

- Modalidade

- Bairro

Por padrão:

- todas as modalidades estão selecionadas

- todos os bairros estão inclusos

---

Matriz Calendário

A matriz apresenta:

- **dias do mês nas linhas**

- **dias da semana nas colunas**

Cada célula contém:

- número do dia no mês

- quantidade de incidentes registrados

Esse visual permite identificar rapidamente **concentrações de incidentes em datas específicas**.

---

Análise por Horário

Também é apresentado um visual de **distribuição por horário**, permitindo observar:

- horários com maior incidência

- padrões ao longo do dia

---

## 3. Implantação

### 3.1 Compartilhamento

O relatório foi publicado como **projeto de portfólio**, permitindo navegação interativa pelos dashboards.

É possível visualizar o relatório público <a href="https://app.powerbi.com/view?r=eyJrIjoiYzYwNzQxMzgtYzcxOC00MDIyLWEzNmItNDUyNDUzOWQyM2Y0IiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9" target="_blank" rel="noopener">clicando aqui</a>

---

### 3.2 Atualização de Dados

A base não possui integração automatizada.

O modelo permite **atualização manual simples**, bastando substituir a base de dados e atualizar o relatório no Power BI.

---

### 3.3 Entregáveis

- Relatório interativo em Power BI

- Modelo dimensional estruturado

- Medidas DAX organizadas

- Dashboards analíticos

- Documentação técnica em Markdown

---

### 3.4 Considerações Finais

Este projeto demonstra a aplicação prática de análise de dados em um contexto urbano relevante, envolvendo **segurança viária e mobilidade urbana**.

O trabalho teve como foco:

- Organização analítica dos dados

- Construção de métricas consistentes

- Modelagem dimensional 

- Criação de dashboards claros e interativos

- Documentação técnica estruturada


Trata-se de um projeto descritivo orientado à **exploração e comunicação de dados**, com objetivo de demonstrar domínio técnico em Power BI, modelagem de dados e análise exploratória.

---

## Referências

- <a href="https://learn.microsoft.com/pt-br/power-bi/" target="_blank" rel="noopener">Documentação oficial do Power BI</a>

- <a href="https://squidfunk.github.io/mkdocs-material/" target="_blank" rel="noopener">Material for MkDocs</a>

---

## Anexos Técnicos

- <a href="https://app.powerbi.com/view?r=eyJrIjoiYzYwNzQxMzgtYzcxOC00MDIyLWEzNmItNDUyNDUzOWQyM2Y0IiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9" target="_blank" rel="noopener">Link público do relatório</a>