# 🔥 Monitoramento de Focos de Queimadas — Estado da Paraíba (2021–2024)

[![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://acadcruzeirodosul-my.sharepoint.com/:u:/g/personal/giovana_ribeiro_cs_unipe_edu_br/IQBrMCapNIgHSLIBaq4tbMC8AbQ2Ga9pVakjd3xkxrnxFEo?e=DodPDu)
[![Dados Públicos](https://img.shields.io/badge/Dados.gov.br-Monitoramento_de_Queimadas-005CA9?style=for-the-badge)](https://dados.gov.br/dados/conjuntos-dados/monitoramento-de-queimadas)
[![Status](https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge)]()

Painel de Business Intelligence para mapeamento, exploração espacial e análise temporal dos focos de incêndios florestais e queimadas no estado da Paraíba. O objetivo é fornecer subsídios para fiscalização ambiental, preservação ecológica e tomada de decisão no setor público.

---

## 📌 Sumário
- [Visão Geral e Contexto](#-visão-geral-e-contexto)
- [Principais Insights dos Dados Reais](#-principais-insights-dos-dados-reais)
- [Arquitetura e Modelagem de Dados](#-arquitetura-e-modelagem-de-dados)
- [Pipeline de Tratamento (ETL)](#-pipeline-de-tratamento-etl)
- [Estrutura do Painel e KPIs](#-estrutura-do-painel-e-kpis)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Autoria e Créditos](#-autoria-e-créditos)

---

## 🌲 Visão Geral e Contexto

As queimadas no semiárido e na faixa litorânea afetam diretamente a biodiversidade, a fertilidade do solo e a qualidade do ar. Este projeto analisa ocorrências registradas por satélite de referência entre 2021 e 2024 no estado da Paraíba, identificando sazonalidades críticas e zonas prioritárias para alocação de brigadas e fiscalização preventiva.

* **Fonte:** Programa Queimadas (INPE) via [Portal Brasileiro de Dados Abertos](https://dados.gov.br/dados/conjuntos-dados/monitoramento-de-queimadas).
* **Granularidade:** Detecção pontual individual (data/hora, coordenadas geográficas, município e bioma).

---

## 📊 Principais Insights dos Dados Reais (Paraíba)

* **Volume Total:** 3.732 focos registrados entre 2021 e 2024.
* **Predomínio por Bioma:** **Caatinga** concentra **92,3%** dos focos (3.445 ocorrências), enquanto a **Mata Atlântica** soma **7,7%** (287 ocorrências).
* **Sazonalidade Crítica:** Mais de **65% dos focos** ocorrem no último trimestre do ano, com pico expressivo em **Novembro** (1.327 focos) e **Outubro** (765 focos), coincidindo com o auge da estiagem no sertão paraibano.
* **Municípios Mais Atingidos:** Destaque para o Sertão e Alto Sertão paraibano:
  1. Conceição (202 focos)
  2. Cajazeiras (162 focos)
  3. Princesa Isabel (100 focos)
  4. São José da Lagoa Tapada (98 focos)
  5. Juru (96 focos)

---

## 📐 Arquitetura e Modelagem de Dados

O modelo relacional foi projetado em **Esquema Estrela (*Star Schema*)** para otimização de performance e simplificação das métricas DAX:

```
        ┌──────────────┐
        │   Dim_Data   │
        └──────┬───────┘
               │ 1:N
┌──────────────┤
│              │
│       ┌──────┴───────┐       1:N ┌──────────────┐
│       │  Fato_Focos  ├───────────┤   Dim_Bioma  │
│       └──────┬───────┘           └──────────────┘
│              │ 1:N
└──────────────┤
        ┌──────┴───────┐
        │Dim_Município │
        └──────────────┘
```

* **`Fato_Focos`:** Tabela central com chaves substitutas (*surrogate keys*), identificadores de detecção, data/hora e coordenadas (`lat`/`lon`).
* **`Dim_Data`:** Dimensão calendário com hierarquia temporal (Ano, Mês, Dia, Ano/Mês, Dia da Semana).
* **`Dim_Município`:** Informações normalizadas de municípios e estado.
* **`Dim_Bioma`:** Classificação dos biomas atingidos.

---

## ⚙️ Pipeline de Tratamento (ETL)

Desenvolvido no **Power Query (M)**:
1. **Importação e Padronização:** Leitura dos arquivos anuais com codificação `Latin-1` e delimitador `;`.
2. **Tipagem e Limpeza:** Conversão de coordenadas para tipos numéricos decimais e campo temporal para `DateTime`.
3. **Consolidação (Append):** União das bases anuais em uma única entidade de fatos com filtragem exclusiva para o estado da Paraíba.
4. **Criação de Chaves:** Geração das chaves relacionais para conexão com as tabelas de dimensão.

---

## 🖥️ Estrutura do Painel

O painel foi estruturado seguindo as melhores práticas de hierarquia visual e experiência do usuário (UX):

* **Top Bar / KPIs:** Cartões com Total de Focos, Município Crítico, Ano Mais Crítico, Mês de Pico e Bioma Predominante.
* **Visão Geoespacial:** Mapa coroplético e de pontos com a densidade das ocorrências nas microrregiões da PB.
* **Análise Temporal:** Evolução anual em linha e distribuição mensal em colunas para leitura de sazonalidade.
* **Rankings e Participação:** Barras horizontais dos Top 10 municípios e gráfico de rosca com a proporção dos biomas.
* **Painel de Segmentação Lateral:** Filtros combinados por Ano, Mês, Município e Bioma.

---

## 🛠️ Tecnologias Utilizadas

* **Power BI Desktop** (Modelagem dimensional, DAX e visualizações)
* **Power Query** (ETL e transformação de dados)
* **HTML5 / CSS3 / Chart.js** (Prototipação e wireframing interativo)

---

## 👥 Autoria e Documentação

* **Autor:** Kauan Albuquerque
* **Colaboração:** Giovana Yngrid e Letícia Bento *(Pesquisa, concepção e documentação técnica)*
