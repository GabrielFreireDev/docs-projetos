# Análise de Incidentes com Tubarões — Litoral de Pernambuco

<div class="video-container">
  <video class="video-doc" autoplay loop muted playsinline>
    <source src="../assets/navegacao-relatorio-tubaroes.mp4" type="video/mp4">
  </video>
</div>

!!! info "Importante"
    A documentação deste projeto foi estruturada com base na metodologia **ESI (Entendimento, Solução e Implantação)**, criada e propagada pelo Mestre em Power BI <a href="https://www.linkedin.com/in/leokarpa/" target="_blank" rel="noopener">Leonardo Karpinski</a> em sua Formação em Análise de Dados na plataforma de ensino <a href="https://xperiun.com/ed/plataforma/" target="_blank" rel="noopener">Xperiun </a> e  amplamente utilizada em projetos analíticos para organizar raciocínio, decisões técnicas e comunicação de resultados.
---

## Visão Geral
Este projeto realiza uma **análise descritiva dos incidentes com tubarões no litoral de Pernambuco**, utilizando dados oficiais disponibilizados pelo **CEMIT (Comitê Estadual de Monitoramento de Incidentes com Tubarões)**.

O objetivo principal é explorar padrões temporais, geográficos, ambientais e comportamentais associados aos incidentes, organizando as informações em dashboards interativos e orientados à análise exploratória.

O projeto foi desenvolvido com foco em **portfólio profissional**, aplicando boas práticas de ETL, modelagem de dados, DAX e visualização no Power BI.

---

### 1. Entendimento

#### 1.1 Contexto do Problema
O estado de Pernambuco é nacionalmente conhecido pela recorrência de incidentes envolvendo tubarões, especialmente na Região Metropolitana do Recife.

O monitoramento desses eventos é realizado pelo **CEMIT**, que disponibiliza publicamente informações estruturadas sobre:

- Data do incidente

- Município 

- Praia 

- Local do ocorrido

- Atividade praticada

- Origem da vítima

- Sexo 

- Faixa etária

- Condição do evento (fatal ou não fatal)

- Informações ambientais associadas

Trata-se de um tema sensível, com impacto social, turístico e ambiental, o que exige clareza analítica e responsabilidade na apresentação dos dados.

---

#### 1.2 Objetivo do Projeto
Os principais objetivos foram:

- Realizar uma **análise descritiva histórica dos incidentes**

- Identificar padrões temporais (ano, mês, dia da semana)

- Avaliar influência de variáveis ambientais (fases da lua e estações do ano)

- Analisar perfil das vítimas (sexo, faixa etária, origem)

- Explorar a relação entre atividade aquática e ocorrência

- Mapear distribuição geográfica no litoral pernambucano

- Construir dashboards interativos e organizados por visão analítica

---

#### 1.3 Escopo e Requisitos
O escopo contempla:

- Análises históricas e exploratórias

- Segmentação por município, praia e período

- Métrica de letalidade associada aos incidentes

- Visualização geográfica customizada

- Organização das análicas em dois dashboards principais


Fora do escopo:

- Modelagem preditiva

- Inferência causal

- Avaliação biológica das espécies

- Estudos oceanográficos aprofundados

---

#### 1.4 Dados Utilizados
- Fonte oficial: <a href="https://semas.pe.gov.br/cemit/" target="_blank" rel="noopener">CEMIT — Comitê Estadual de Monitoramento de Incidentes com Tubarões</a>

- Tipo de dados: Base pública preenchida manualmente

- Granularidade principal: **um registro por incidente**

- Período: conforme disponibilizado na base oficial com última atualização em 29/01/2026 para este relatório

Além disso, foi criada uma **tabela auxiliar contendo todas as praias e municípios do litoral de Pernambuco**, utilizada para:

- Padronização geográfica

- Apoio à modelagem

- Construção do visual sinóptico

- Deixar a base de dados preparada para futuras ocorrências em áreas que ainda não aconteceram

Essa tabela auxiliar foi estruturada com apoio do ChatGPT para garantir completude das localidades.

---

### 2. Solução

#### 2.1 Extração dos Dados
A base foi obtida diretamente no site oficial do CEMIT.

Após a obtenção, os dados foram importados para o **Power BI Desktop**, onde iniciou-se a etapa de análise exploratória inicial para:

- Verificação de estrutura 

- Tipos de dados

- Consistência dos registros

- Volume total de incidentes  

---

#### 2.2 Transformação dos Dados (ETL)
A etapa de transformação foi realizada no **Power Query**.

Principais procedimentos adotados:

- Padronização de tipos de dados

- Tratamento de campos textuais (município e praia)

- Normalização de categorias (atividade, origem, sexo)

- Criação de colunas derivadas de data:

    - Ano

    - Mês

    - Dia da semana

- Classificação de:

    - Fase da lua

    - Estação do ano

- Integração com tabela auxiliar de praias e municípios

O objetivo foi garantir consistência analítica e facilitar a criação das medidas em DAX.

---

#### 2.3 Modelagem dos Dados
Foi adotada uma modelagem orientada à análise descritiva, com:

- Tabela fato principal: **Incidentes**

- Dimensão Calendário

- Dimensão Localidade (município, praia, zona, litoral)

- Dimensões auxiliares para categorização

Relacionamentos estruturados para evitar ambiguidade de contexto.

A modelagem priorizou:

- Simplicidade

- Clareza

- Facilidade de manutenção

- Performance adequada para volume histórico


---

#### 2.4 Cálculos e Métricas
As métricas foram desenvolvidas em **DAX**.

Principais indicadores:

- Total de Incidentes

- Total de Incidentes Fatais

- Taxa de Letalidade

- Média de Incidentes por Ano

- Média de Incidentes por Mês

- Média de Incidentes por Dia da Semana


Todos os visuais (exceto os relacionados a eventos da natureza e o visual sinóptico do mapa) possuem **tooltip padronizado exibindo a taxa de letalidade**, garantindo suporte contextual à análise.

---

#### 2.5 Visualização e Dashboards
O material disponibilizado pelo CEMIT junto com a base de dados já consta com alguns visuais em um relatório PDF e eles foram incorporados no projeto. 

O relatório foi estruturado em **dois dashboards principais** e todo design foi criado no Figma:

---

##### 2.5.1 Visão Geral
Filtros disponíveis:

- Município

- Praia

- Intervalo de Anos

Principais análises:

- Total de incidentes por:

    - Ano

    - Mês

    - Dia da Semana

- Matriz cruzada:

    - Mês vs Dia da Semana

- Eventos da natureza:

    - Fases da Lua

    - Estações do Ano

- Atividade aquática

- Origem da vítima

- Faixa etária

- Sexo


O design prioriza leitura rápida, comparabilidade temporal e identificação de padrões.

---

##### 2.5.2 Localidade
Filtros disponíveis:

- Zona

- Litoral

- Intervalo de Anos


Contém dois visuais principais:

1. **Gráfico sinóptico customizado com as cidades do litoral de Pernambuco**

2. **Matriz detalhada** permitindo análise por:

   - Município

   - Praia

   - Local específico do ocorrido


Essa visão permite aprofundamento geográfico e detalhamento granular dos incidentes.

---

##### 2.6 Visuais Customizados
Foram utilizados visuais não nativos do Power BI:

- **HTML Content**

- **Synoptic Panel by OkVIZ**


O Synoptic Panel foi utilizado para construção do mapa vetorial customizado do litoral pernambucano, permitindo interação direta com as cidades e o mapeamento númerico das áreas foi realizado para fins de identifiação do próprio visual.

!!! warning "Atenção"
    O visual Synoptic Panel precisa de pagamento de licença para utilização comercial, dessa forma no relatório público do Power BI não aparecerá. Portanto, foi colocado por cima um visual de imagem com uma captura de tela para ficar como exemplo e **não será possível visualizar de forma dinâmica pelos filtros**.

---

### 3. Implantação

#### 3.1 Compartilhamento
O relatório foi publicado como projeto de portfólio, permitindo navegação interativa pelos dashboards.

É possível visualizar o relatório público <a href="https://app.powerbi.com/view?r=eyJrIjoiYjgwYzgyYWUtZjZkNS00ZjI3LWFjZDQtOWRiNDNkNDUzNWIyIiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9" target="_blank" rel="noopener"> clicando aqui</a>


---

#### 3.2 Automatização
Não foi implementada atualização automática, pois a base pública não possui endpoint automatizado.

No entanto, o modelo permite atualização manual com facilidade, caso novos dados sejam disponibilizados.

---

#### 3.3 Entregáveis
- Relatório interativo em Power BI

- Modelo de dados estruturado

- Medidas DAX organizadas

- Tabela auxiliar de localidades

- Documentação técnica em Markdown


---

#### 3.4 Considerações Finais
Este projeto demonstra a aplicação prática de análise de dados em um contexto público e sensível, envolvendo segurança, turismo e meio ambiente que serve para ser utilizado em áreas de eduação e conscientazação das pessoas, além de apoio em estudos relacioandos ao tema.

O foco foi:

- Garantir consistência analítica

- Organizar visualizações claras e interativas

- Estruturar modelagem coerente

- Documentar decisões técnicas

- Explorar padrões históricos com responsabilidade


Trata-se de um projeto descritivo, orientado à exploração e comunicação de dados, com objetivo de demonstrar domínio técnico em Power BI e organização analítica.

---

## Referências
- <a href="https://semas.pe.gov.br/cemit/" target="_blank" rel="noopener">CEMIT — Comitê Estadual de Monitoramento de Incidentes com Tubarões</a>

- <a href="https://learn.microsoft.com/pt-br/power-bi/" target="_blank" rel="noopener">Documentação oficial do Power BI</a>

- <a href="https://okviz.com/synoptic-panel/" target="_blank" rel="noopener">Synoptic Panel by OkVIZ</a>

- <a href="https://squidfunk.github.io/mkdocs-material/" target="_blank" rel="noopener">Material for MkDocs</a>

---

## Anexos Técnicos
- <a href="https://app.powerbi.com/view?r=eyJrIjoiYjgwYzgyYWUtZjZkNS00ZjI3LWFjZDQtOWRiNDNkNDUzNWIyIiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9" target="_blank" rel="noopener">Link público do relatório</a>