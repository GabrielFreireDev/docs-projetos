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

Este projeto realiza uma **análise descritiva dos incidentes de trânsito registrados na cidade do Recife**, explorando padrões temporais, espaciais e relacionados aos modais envolvidos nos acidentes.

O objetivo principal é identificar **distribuições geográficas, padrões de ocorrência ao longo do tempo e características associadas aos incidentes**, utilizando dashboards interativos desenvolvidos no Power BI.

O projeto foi construído como **projeto de portfólio profissional**, aplicando práticas de:

- Modelagem dimensional
- ETL com Power Query
- Construção de métricas em DAX
- Design de dashboards analíticos
- Documentação técnica estruturada

---

## 1. Entendimento

### 1.1 Contexto do Problema

Acidentes de trânsito representam um importante problema urbano, impactando:

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

Objetivo do Projeto

Os principais objetivos analíticos foram:

- Identificar **distribuição geográfica dos incidentes por bairro**
- Analisar **evolução temporal dos acidentes**
- Avaliar **participação dos diferentes modais**
- Medir **impacto em vítimas e vítimas fatais**
- Calcular **taxa de mortalidade associada aos incidentes**
- Explorar **padrões de ocorrência por dia e horário**

Além disso, buscou-se estruturar um relatório com foco em:

- **comparabilidade temporal**
- **exploração interativa**
- **leitura analítica rápida**

---

Escopo e Requisitos

O escopo do projeto contempla:

- Análise descritiva dos incidentes
- Métricas de vítimas e fatalidades
- Segmentação por bairro
- Segmentação por modalidade de transporte
- Análise temporal por ano, mês, dia e horário
- Visualização geográfica
- Dashboards interativos

Fora do escopo:

- Modelagem preditiva
- Identificação de causas dos acidentes
- Avaliação de políticas públicas
- Análise comportamental dos condutores

---

### 1.2 Dados Utilizados

Os dados utilizados correspondem a registros históricos de **incidentes de trânsito na cidade do Recife**.

Características principais da base:

- Granularidade principal: **um registro por incidente**
- Estrutura contendo informações sobre:
  - Data do incidente
  - Hora
  - Bairro
  - Modalidades envolvidas
  - Quantidade de vítimas
  - Quantidade de vítimas fatais

A base possui particularidades importantes:

- Um incidente pode envolver **mais de um modal**
- Nem todos os incidentes possuem vítimas
- Existem registros onde a informação de modal não foi preenchida

Durante a preparação dos dados foram identificados **incidentes sem registro de modalidades**, os quais foram removidos da tabela de fatos específica para manter consistência analítica nas métricas relacionadas aos modais.

---

## 2. Solução

### 2.1 Extração dos Dados

A base de dados foi importada para o **Power BI Desktop**, onde foi realizada uma análise inicial para identificar:

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
- Separação de informações temporais
- Criação de dimensões auxiliares
- Preparação da estrutura para modelagem dimensional

Foram criadas colunas derivadas de data e tempo, permitindo análises por:

- Ano
- Mês
- Dia do mês
- Dia da semana
- Hora

Também foi construída uma **tabela de modalidades** para representar corretamente a relação **muitos-para-muitos entre incidentes e modais**.

---

### 2.3 Modelagem dos Dados

Foi adotado um **modelo dimensional em estrela**, estruturado da seguinte forma:

Tabelas Fato

**fato_acidentes**

Granularidade: um registro por incidente.

Contém métricas relacionadas a:

- quantidade de vítimas
- quantidade de vítimas fatais

---

**fato_modalidades**

Granularidade: um registro por **incidente e modalidade envolvida**.

Essa tabela permite analisar:

- participação dos modais
- contagem de incidentes por tipo de veículo

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

Como algumas métricas estão na tabela **fato_acidentes**, foi utilizada a função **TREATAS em DAX** para permitir que filtros aplicados na tabela **fato_modalidades** também afetem medidas provenientes da tabela **fato_acidentes**.

Essa estratégia garante consistência nas análises quando o usuário filtra por modalidade.

---

### 2.4 Cálculos e Métricas

As métricas foram desenvolvidas utilizando **DAX**.

Principais indicadores implementados:

Incidentes

- Total de Incidentes
- Total de Incidentes Ano Anterior
- Variação Absoluta
- Variação Percentual

Incidentes com Vítimas

- Total de Incidentes com Vítimas
- Comparação com ano anterior
- Variação absoluta
- Variação percentual

Vítimas

- Quantidade total de vítimas
- Comparação com ano anterior
- Variação absoluta
- Variação percentual

Vítimas Fatais

- Quantidade de vítimas fatais
- Comparação com ano anterior
- Variação absoluta
- Variação percentual

Taxa de Mortalidade

A taxa foi calculada utilizando:


Essa métrica permite avaliar a **letalidade associada aos incidentes**.

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

- **taxa de mortalidade**

---

Comparação Temporal

Ao lado de cada cartão há um **gráfico de linha**, mostrando:

- evolução anual do indicador
- comparação entre **ano atual e ano anterior**

Esse recurso permite identificar tendências temporais.

---

Análise Geográfica por Bairro

O dashboard possui uma **matriz detalhada por bairro**, contendo:

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

Dashboard 2 — Calendário e Horário

Este dashboard explora a **distribuição temporal dos incidentes**.

---

Filtros disponíveis

- Ano
- Mês
- Modalidade
- Bairro

Por padrão:

- todas as modalidades
- todos os bairros

---

Matriz Calendário

A matriz apresenta:

- **dias do mês nas linhas**
- **dias da semana nas colunas**

Cada célula contém:

- número do dia
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

É possível visualizar o relatório público <a href="#" target="_blank" rel="noopener">clicando aqui</a>

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

- organização analítica dos dados
- construção de métricas consistentes
- modelagem dimensional adequada
- criação de dashboards claros e interativos
- documentação técnica estruturada

Trata-se de um projeto descritivo orientado à **exploração e comunicação de dados**, com objetivo de demonstrar domínio técnico em Power BI, modelagem de dados e análise exploratória.

---

## Referências

- <a href="https://learn.microsoft.com/pt-br/power-bi/" target="_blank" rel="noopener">Documentação oficial do Power BI</a>

- <a href="https://squidfunk.github.io/mkdocs-material/" target="_blank" rel="noopener">Material for MkDocs</a>

---

## Anexos Técnicos

- <a href="#" target="_blank" rel="noopener">Link público do relatório</a>