# 🏥 Agente Orquestrador de IA - Análise de Dados SRAG (Indicium HealthCare)

## 📖 Sobre o Projeto
Este projeto é uma Prova de Conceito (PoC) desenvolvida para a Indicium HealthCare Inc. O objetivo é fornecer uma solução baseada em Inteligência Artificial Generativa capaz de auxiliar profissionais de saúde na compreensão em tempo real da severidade de surtos de Síndrome Respiratória Aguda Grave (SRAG) no Brasil.

A solução utiliza um **Agente Orquestrador** que cruza dados estruturados (banco de dados do Open DATASUS) com dados não estruturados (notícias em tempo real) para gerar relatórios epidemiológicos automatizados, precisos e embasados.

## 🏗️ Arquitetura da Solução
A arquitetura foi construída utilizando os mais modernos conceitos de *Agentic AI*:

* **Cérebro (LLM):** Google Gemini configurado com temperatura 0 para garantir respostas determinísticas e factuais.
* **Orquestrador:** LangGraph (`create_react_agent`), responsável por raciocinar sobre o pedido do usuário e decidir qual ferramenta usar.
* **Ferramentas (Tools):**
  1. `extrair_metricas_srag`: Consulta um banco de dados SQLite local utilizando SQL para extrair taxas de mortalidade, ocupação de UTI, vacinação e crescimento de casos.
  2. `buscar_noticias_recentes`: Utiliza a API do DuckDuckGo para buscar o contexto qualitativo atualizado em tempo real na internet.
  3. **Visualizer:** Um pipeline de dados em Python (`matplotlib` e `pandas`) que gera gráficos analíticos mensais e diários salvos localmente.

## 🛡️ Critérios de Avaliação Atendidos

Neste projeto, foram aplicadas as melhores práticas de Engenharia de Software e IA Segura:

1. **Tratamento de Dados Sensíveis:** O pipeline inicial aplica uma técnica de *allowlist*, ignorando mais de 90 colunas do DATASUS e carregando para a memória apenas as 4 colunas estritamente necessárias (EVOLUCAO, UTI, VACINA, DT_NOTIFIC), garantindo a anonimização total dos pacientes (Compliance com LGPD).
2. **Guardrails:** O prompt de sistema do Agente possui regras restritas que proíbem alucinações matemáticas (obrigando o uso da ferramenta SQL) e limitam o escopo de resposta exclusivamente ao tema da saúde/SRAG.
3. **Governança e Transparência:** Foi implementado um sistema de *streaming* de eventos que gera logs em tempo real (marcados como `[AUDITORIA]`), permitindo rastrear exatamente quando o Agente consultou o banco de dados ou a internet.
4. **Clean Code:** Funções altamente modulares, com *docstrings* claras, tratamento de exceções (blocos `try/except`) e consultas SQL otimizadas.

## 🚀 Como Executar o Projeto

### Pré-requisitos
* Python 3.10+
* Uma chave de API do Google Gemini (`GOOGLE_API_KEY`)

### Passo a Passo (Instalação Local ou Google Colab)

1. **Clone o repositório:**
   ```bash
   git clone [https://github.com/victorlochindicium/poc-srag-agente.git](https://github.com/victorlochindicium/poc-srag-agente.git)
   cd poc-srag-agente
