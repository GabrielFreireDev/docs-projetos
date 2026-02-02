
# Análise de Xadrez em Números

<div class="video-container">
  <video class="video-doc" autoplay loop muted playsinline>
    <source src="../assets/navegacao-relatorio-xadrez.mp4" type="video/mp4">
  </video>
</div>


!!! info "Importante"
    A documentação deste projeto foi desenvolvida com base na metodologia **ESI (Entendimento, Solução e Implantação)**  criada e propagada pelo Mestre em Power BI <a href="https://www.linkedin.com/in/leokarpa/" target="_blank" rel="noopener">Leonardo Karpinski</a> em sua Formação em Análise de Dados na plataforma de ensino <a href="https://xperiun.com/ed/plataforma/" target="_blank" rel="noopener">Xperiun.</a>

# Visão Geral

Este projeto utiliza dados públicos da **API do Chess.com** com o objetivo de realizar uma **análise descritiva** do **Top 50 do ranking global** de enxadrez nas modalidades **Blitz, Bullet e Rapid**, além de uma análise detalhada de partidas.

O projeto foi desenvolvido com foco em **portfólio profissional**, aplicando boas práticas de engenharia de dados, modelagem analítica, métricas, visualização e storytelling em Power BI.

Apesar de utilizar dados públicos, todas as decisões técnicas e analíticas foram tratadas como se o projeto estivesse inserido em um **ambiente real de negócio**, considerando limitações de fonte, regras de modelagem e consistência analítica.

---

## 1. Entendimento

### 1.1 Contexto do Negócio

O Chess.com é uma plataforma global de enxadrez online que disponibiliza rankings, estatísticas e partidas de jogadores do mundo inteiro, organizados por diferentes modalidades e controles de tempo.

Nesse contexto, métricas como **rating**, **classificação**, **vitórias**, **derrotas**, **empates**, **precisão das partidas** e **aberturas jogadas** são fundamentais para entender:

- O nível competitivo dos jogadores

- A distribuição geográfica do alto rendimento

- O desempenho individual por modalidade

- Padrões de jogo e comportamento estratégico


Como se trata de uma API pública, não há acesso às regras internas da plataforma. Portanto, o entendimento do negócio foi construído a partir:

- Da estrutura dos endpoints da API

- Dos nomes e tipos dos campos retornados

- Do comportamento observado nos dados

- De premissas documentadas ao longo do projeto


---

### 1.2 Objetivo do Projeto

Os principais objetivos deste projeto são:

- Realizar uma **análise descritiva** do Top 50 global do Chess.com

- Comparar o desempenho entre as modalidades Blitz, Bullet e Rapid

- Analisar estatísticas individuais de enxadristas

- Avaliar partidas mensais sob a ótica de resultado, precisão e abertura

- Explorar a distribuição geográfica dos jogadores

- Criar dashboards claros, organizados e interativos

- Demonstrar domínio técnico para fins de **portfólio profissional**


Além disso, o projeto inclui um **showcase analítico dedicado ao Grande Mestre Luís Paulo Supi**, com análise das partidas disputadas ao longo de 2025.

---

### 1.3 Escopo e Requisitos

O escopo do projeto contempla:

- Análises descritivas (históricas e consolidadas)

- Métricas de desempenho por jogador e modalidade

- Comparações entre modalidades

- Análise geográfica por país e continente

- Análise de partidas mensais

- Análise de aberturas (ECO)

- Dashboards organizados por visões analíticas

- Documentação das decisões técnicas e analíticas


Fora do escopo:

- Modelos preditivos

- Inferências causais

- Recomendações automatizadas

- Simulações de rating futuro


---

### 1.4 Dados Utilizados

Os dados utilizados são públicos e obtidos por meio da **API do Chess.com**.

- Fonte: <a href="https://www.chess.com/news/view/published-data-api" target="_blank" rel="noopener">Chess.com Public Data API</a>

- Granularidade principal:

  - Ranking por modalidade

  - Estatísticas por jogador

  - Partidas (nível jogador-partida)

- Entidades analisadas:

  - Enxadristas

  - Classificações

  - Estatísticas

  - Partidas

  - Países

  - Aberturas (ECO)

- Período:

  - Rankings: snapshot da última data de coleta dos dados

  - Partidas: ano de 2025 (showcase)


!!! warning "Premissas Importantes"
    Como se trata de uma API pública, algumas regras de negócio precisaram ser inferidas a partir do comportamento dos dados. O chess.com foi utilizado como base para crião do relatório e sempre será a melhor platorma para esse devido fim.

As premissas consideradas foram:

- O Chess.com fornece apenas o **Top 50 por modalidade**, podendo haver interseção de jogadores entre modalidades

- Um mesmo enxadrista pode participar de uma, duas ou três modalidades

- Algumas siglas de país retornadas pela API não possuem correspondência oficial

- Partidas abandonadas podem apresentar precisão de 100% e foram tratadas separadamente, já que uma queda de conexão da internet pode resultar em em abandono

- Ratings retornados em partidas representam o rating do jogador **no momento da partida**, não o rating consolidado da modalidade

Para fins de portifólio, os dados foram armazenados em arquivos *csv*, porém, seria completamente possível armanazenar em um bando de dados no modelo data warehouse. 

Demais informações técnicas podem ser verificadas diretamente no arquivo `.pbix` e nos scripts Python disponibilizados no repositório do projeto no GitHub.

---

## 2. Solução

### 2.1 Extração dos Dados

A extração foi realizada via **scripts Python**, consumindo diretamente os endpoints da API do Chess.com.

Nesta etapa foram realizadas:

- Requisições HTTP controladas

- Persistência dos dados brutos em formato JSON

- Separação por entidade e período

- Registro de logs para controle de execução


A extração foi organizada em camadas, permitindo rastreabilidade e reprocessamento completo quando necessário.

---

### 2.2 Transformação dos Dados (ETL)

A etapa de transformação foi essencial para garantir consistência analítica.

Principais ações realizadas:

- Conversão de timestamps Unix para data

- Padronização de tipos de dados

- Tradução de resultados de partidas para PT-BR

- Tratamento de valores ausentes

- Criação de colunas auxiliares

- Normalização de nomes de aberturas (ECO)

- Padronização de usernames

- Criação de chaves surrogate para dimensões


As transformações foram realizadas majoritariamente em **Python (Pandas)**, com ajustes complementares no **Power Query**, priorizando clareza, rastreabilidade e facilidade de manutenção.

---

### 2.3 Modelagem dos Dados

Foi adotada uma **modelagem dimensional em estrela**, separando fatos e dimensões.

- Tabelas fato:

  - Classificação

  - Estatísticas

  - Partidas Mensais

- Dimensões:

  - Enxadrista

  - País

  - Modalidade

  - Calendário

  - ECO (Aberturas)


Uma decisão importante de modelagem foi a criação da tabela de partidas no nível **jogador-partida**, evitando relacionamentos ambíguos entre brancas e pretas e simplificando as análises por jogador, cor e resultado.

Essa abordagem garante melhor desempenho, clareza analítica e facilidade na escrita de medidas DAX.

---

### 2.4 Cálculos e Métricas

As métricas foram desenvolvidas em **DAX**, respeitando as premissas do negócio e as decisões de modelagem.


Exemplos de métricas:

- Total de enxadristas

- Rating médio por modalidade

- Taxa de vitória

- Precisão média

- Distribuição por cor

- Total de partidas analisadas

- Quantidade de enxadritas por modalidade


---

### 2.5 Visualização e Dashboards

O design do relatório foi desenvolvido diretamente no **Power BI**, com inspiração visual em tabuleiros de xadrez. Os ícones do relatório foram importados do Figma.

Os dashboards foram organizados por visões analíticas:

- Visão Geral

- Classificação Global

- Estatística Individual

- Partidas Mensais

Foram utilizados:

- Parâmetros de campo para alternância de temas

- Segmentações estratégicas

- Ícones temáticos

- Hierarquia visual clara

- Storytelling orientado à análise esportiva


!!! note "Observação"
    Alguns visuais utilizam filtros específicos para melhorar a legibilidade e evitar interpretações equivocadas dos dados.

---

## 3. Implantação

### 3.1 Compartilhamento

Como projeto de portfólio, o relatório e a documentação são disponibilizados publicamente por meio de link do Power BI Service e repositório no GitHub, permitindo livre exploração do conteúdo e validação das decisões técnicas adotadas.

---

### 3.2 Automatização

A extração foi desenvolvida de forma automatizável via Python.

Embora o projeto não esteja conectado a uma rotina de atualização automática no Power BI Service, toda a arquitetura foi pensada para suportar facilmente futuras automações e atualizações periódicas.

---

### 3.3 Entrega

Os principais entregáveis do projeto são:

- Relatório interativo no Power BI

- Modelo dimensional estruturado

- Scripts de extração e transformação em Python

- Métricas documentadas

- Documentação técnica em Markdown


---

### 3.4 Considerações Finais

Este projeto demonstra a aplicação prática de análise de dados em um contexto esportivo e competitivo, utilizando dados públicos, mas com abordagem profissional.

O foco foi garantir:

- Consistência analítica

- Boas decisões de modelagem

- Clareza visual

- Storytelling orientado a dados

- Documentação transparente


Trata-se de um projeto descritivo, desenvolvido para demonstrar competências técnicas e analíticas em um cenário próximo ao real.

---

## Referências

- <a href="https://www.chess.com/news/view/published-data-api" target="_blank" rel="noopener">Chess.com Public Data API</a>

- <a href="https://learn.microsoft.com/pt-br/power-bi/" target="_blank" rel="noopener">Documentação oficial do Power BI</a>

- <a href="https://squidfunk.github.io/mkdocs-material/" target="_blank" rel="noopener">Material for MkDocs</a>


## Anexos Técnicos

- <a href="https://github.com/GabrielFreireDev/xadrez-em-numeros" target="_blank" rel="noopener">Repositório GitHub com scripts e documentação do projeto</a>

- <a href="https://app.powerbi.com/view?r=eyJrIjoiZWMwZjk1NjgtYmFmMy00NzZlLTkzOTYtZTdkZjAyZmFiZmMzIiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9" target="_blank" rel="noopener">Link público do relatório no Power BI</a>
