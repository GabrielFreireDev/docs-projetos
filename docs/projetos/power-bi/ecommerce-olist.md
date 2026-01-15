# Análise de E-commerce — Dataset Olist

<div class="video-container">
  <video class="video-doc" autoplay loop muted playsinline>
    <source src="../assets/navegacao-relatorio-olist.mp4" type="video/mp4">
  </video>
</div>


!!! info "Importante"
    A documentação deste projeto foi desenvolvida com base na metodologia **ESI (Entendimento, Solução e Implantação)**  criada e propagada pelo Mestre em Power BI <a href="https://www.linkedin.com/in/leokarpa/" target="_blank" rel="noopener">Leonardo Karpinski</a> em sua Formação em Análise de Dados na plataforma de ensino <a href="https://xperiun.com/ed/plataforma/" target="_blank" rel="noopener">Xperiun.</a>


## Visão Geral
Este projeto utiliza o **Brazilian E-Commerce Public Dataset by Olist** com o objetivo de realizar uma **análise descritiva** do comportamento de vendas, clientes, vendedores, entregas e avaliações em um marketplace brasileiro.

O projeto foi desenvolvido com foco em **portfólio profissional**, aplicando boas práticas de análise de dados, modelagem analítica, métricas e visualização.  

Apesar de utilizar dados públicos, todas as decisões técnicas e analíticas foram tratadas como se o projeto estivesse inserido em um ambiente real de negócio.

---

## 1. Entendimento

### 1.1 Contexto do Negócio
A Olist atua como um **marketplace**, conectando vendedores e clientes por meio de uma plataforma de e-commerce.

Nesse modelo de negócio, o acompanhamento de indicadores como faturamento, pedidos, entregas e satisfação do cliente é fundamental para entender o desempenho da operação.

Como se trata de um dataset público, não há acesso às regras internas da empresa. Portanto, o entendimento do negócio foi construído a partir:

- Da estrutura dos dados

- Dos nomes dos campos

- Do comportamento observado nas tabelas

- De premissas documentadas ao longo do projeto


---

### 1.2 Objetivo do Projeto
Os principais objetivos deste projeto são:

- Realizar uma **análise descritiva** do marketplace

- Explorar o comportamento de vendas ao longo do tempo

- Analisar clientes, vendedores e categorias de produtos

- Avaliar entregas e experiência do cliente por meio das avaliações

- Construir dashboards claros, organizados e interativos

- Demonstrar domínio técnico para fins de portfólio profissional


---

### 1.3 Escopo e Requisitos
O escopo do projeto contempla:

- Análises históricas (descritivas)

- Métricas consolidadas e segmentadas

- Comparações temporais (year over year)

- Análise por Região do país

- Análise de Pareto de algumas métricas

- Dashboards organizados por visões do negócio

- Documentação das decisões técnicas e analíticas


Fora do escopo:

- Modelos preditivos

- Inferências causais

- Recomendações automatizadas

---

### 1.4 Dados Utilizados
Os dados utilizados são públicos e disponibilizados no Kaggle:

- Fonte: <a href="https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce" target="_blank" rel="noopener">**Brazilian E-Commerce Public Dataset by Olist**</a>

- Granularidade principal: item do pedido

- Entidades analisadas:

    - Pedidos

    - Pagamentos

    - Clientes

    - Vendedores

    - Avaliações

    - Produtos

- Período: 2017 a 2018 (pedidos de 2016 não foram considerados).


!!! warning "Premissas Importantes"
    Como se trata de um dataset público, algumas regras de negócio precisaram ser inferidas a partir do comportamento dos dados.

As regras que foram consideradas:

- Durante a exploração dos dados, foram identificadas **inconsistências temporais e de sincronização entre tabelas**, possivelmente decorrentes de extrações realizadas em momentos distintos do ERP. As tabelas *itens_pedidos*, *pedidos*, *pagamentos* e *avaliações* não possui exatamente os mesmos pedidos. Portanto, a tabela *pedidos* foi utilizada como fonte de dados principal e foi feita a mesclagem com as outras tabelas (exceto Avaliações) para formar a tabela fato *Vendas* utilizando apenas os pedidos correspondentes entre si. Isso faz total diferença nos resultados dos cálculos

- Foram considerados apenas pedidos que geram receitas: status "aprovado", "enviado" ou "entregue"

- A informação da forma de pagamento no arquivo original *olist_order_payments_dataset.csv* possui a informação "voucher" na coluna *payment_type* que foi interpretada como *desconto* nos casos dos pedidos que também tinham algum tipo de pagamento (boleto, cartão de crédito e cartão de débito) para o mesmo pedido e status "entregue". E foi considerado como *devolução* os pedidos que tinham somente valores marcados com "voucher" na coluna *payment_type* e status "entregue", sem nenhum outro tipo de pagamento atrelado. 

- Alguns pedidos com o *tempo para entrega do pedido* e *tempo para avaliar pedido* estão com um tempo considerado absurdo se for considerar as operações de e-commerce que temos atualmente. Todavia, eles foram mantidos. 


Demais informações são extremamente técnicas e podem ser verificadas no arquivo *pibx* do projeto disponibilizados no repositório do <a href="https://github.com/GabrielFreireDev/ecommerce-olist-power-bi/" target="_blank" rel="noopener">**Github**</a>

---

## 2. Solução

### 2.1 Extração dos Dados
Os arquivos foram importados diretamente para o Power BI a partir dos arquivos CSV disponibilizados no Kaggle.

Nesta etapa, foi realizada uma análise inicial da estrutura dos dados, tipos de campos e volume de registros. Além disso, os arquivos *csv* da base de dados original tiverem algumas informações traduzidas de inglês para português antes de serem importados no Power BI, objetivando um melhor entendimento geral das informações.

---

### 2.2 Transformação dos Dados (ETL)
A etapa de transformação foi essencial para garantir consistência analítica.

Principais ações realizadas:

- Padronização de tipos de dados

- Correção de problemas de separador decimal

- Tratamento de valores ausentes

- Criação de colunas auxiliares para regras de negócio e simplificação de exibição dos dados nos visuais

- Alinhamento entre tabelas com base nos pedidos que efetivamente possuem itens e pagamentos

- Criação de classificações analíticas (ex: pedidos que geram receita)

Essas transformações foram realizadas no **Power Query** e algumas delas no **Power BI Desktop** ou por **Linguagem DAX**, priorizando clareza, rastreabilidade e facilidade de manutenção.


---


### 2.3 Modelagem dos Dados
Foi adotada uma **modelagem em estrela**, separando fatos e dimensões para facilitar análises e cálculos.

- Tabela fato principal: **Vendas**

- Tabelas fato auxiliares: Avaliações

- Dimensões: Clientes, Produtos, Vendedores, Calendário, Hora

- Relacionamentos definidos de forma controlada

- Evitada a relação direta entre tabelas fato

Essa abordagem garante melhor desempenho e controle de contexto nas análises.

---

### 2.4 Cálculos e Métricas
As métricas foram desenvolvidas em **DAX**, respeitando as regras de negócio inferidas e as decisões de modelagem. Para fins de organização e manutenção, as medidas DAX foram organizadas conforme o dashboard onde ela está sendo utilizada (ou foi utilizada pela primeira vez) primando a agilidade para localização e manuentação do relatório.

Exemplos de métricas:

- Faturamento

- Receita líquida

- Total de descontos

- Total de devoluções

- Total de Pedidos

- Clientes e Vendedores ativos

- Indicadores de entrega

- Métricas de avaliação do cliente


---

### 2.5 Visualização e Dashboards

A criação do *background* do relatório foi desenvolvida no **Figma**.

Os dashboards, assim como as meddias, foram organizados por visões do negócio:

- Visão Geral

- Análise de Pareto

- Detalhamento Mensal

- Clientes

- Vendedores

- Produtos

- Avaliações

- Entregas


!!! note "Observação"
    Alguns visuais estão com um filtro de "valores maior que 1" nas medidas quantitativas. Isto foi feito para melhor experiência visual das informações.


O design do relatório como um todo prioriza:

- Hierarquia visual clara

- Paleta de cores consistente

- Storytelling orientado ao negócio

- Facilidade de navegação

- Interatividade sem excesso de ruído visual

---

## 3. Implantação

### 3.1 Compartilhamento
Como projeto de portfólio, o relatório e a documentação são disponibilizados publicamente (<a href="https://app.powerbi.com/view?r=eyJrIjoiMTM3YTUzODctMWI0My00Y2IwLWIwZTktNDg4MWE5MjRiZmYwIiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9" target="_blank" rel="noopener">clique aqui</a>), permitindo que qualquer pessoa explore cada dashboard e compreenda as decisões adotadas.

---

### 3.2 Automatização
Não foi implementada automação de atualização dos dados, pois o dataset é estático.

No entanto, o modelo foi construído de forma que suporte facilmente futuras automatizações, caso novas extrações sejam disponibilizadas.

---

### 3.3 Entrega
Os principais entregáveis do projeto são:

- Relatório interativo no Power BI

- Modelo de dados estruturado

- Métricas documentadas

- Etapas de ETL documentaadas

- Documentação técnica em Markdown

---

### 3.4 Considerações Finais
Este projeto demonstra a aplicação prática de análise de dados em um contexto de marketplace, mesmo utilizando dados públicos.

O foco foi garantir:

- Consistência analítica

- Clareza nas decisões

- Boa comunicação visual

- Documentação transparente

Trata-se de um projeto descritivo, desenvolvido para demonstrar competências técnicas e analíticas em um cenário próximo ao real.

---

## Referências

- <a href="https://www.kaggle.com/" target="_blank" rel="noopener">Brazilian E-Commerce Public Dataset by Olist</a>

- <a href="https://learn.microsoft.com/pt-br/power-bi/ " target="_blank" rel="noopener">Documentação oficial do Power BI </a>

- <a href="https://squidfunk.github.io/mkdocs-material/" target="_blank" rel="noopener">Material for MkDocs</a>

## Anexos Técnicos

- <a href="https://github.com/GabrielFreireDev/ecommerce-olist-power-bi" target="_blank" rel="noopener">Repositório Github com arquivos do projeto</a>

- <a href="https://app.powerbi.com/view?r=eyJrIjoiMTM3YTUzODctMWI0My00Y2IwLWIwZTktNDg4MWE5MjRiZmYwIiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9" target="_blank" rel="noopener">Link público do relatório</a>

