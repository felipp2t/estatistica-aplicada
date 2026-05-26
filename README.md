# 📊 Projeto de Extensão — Estatística Aplicada

Este repositório contém o projeto de extensão para a disciplina de **Estatística Aplicada**, focado na análise de dados públicos municipais, modelagem estatística e regressões lineares múltiplas.

---

## 🎯 Pergunta Alvo (Problema de Pesquisa)

> **"De que maneira a flutuação mensal na proporção de cargos comissionados e o volume de adiantamentos emergenciais explicam a variabilidade dos gastos com pessoal entre as secretarias?"**

### 💡 Em Português Claro (A Hipótese)
O objetivo do projeto é testar e validar estatisticamente a seguinte hipótese:
* **As secretarias municipais que possuem maior rotatividade/proporção de cargos comissionados e que utilizam maior volume de adiantamentos financeiros de emergência são exatamente aquelas onde a folha salarial de pessoal se mostra mais instável e imprevisível de um mês para o outro?**

---

## ⚙️ Tecnologias e Configuração do Ambiente

O projeto utiliza o **`uv`**, um gerenciador de pacotes e ambientes Python de última geração, extremamente veloz.

### 🚀 Como Rodar o Projeto Localmente

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/felipp2t/estatistica-aplicada.git
   cd estatistica-aplicada
   ```

2. **Crie e sincronize o ambiente virtual:**
   ```bash
   # Cria o ambiente virtual e instala todas as dependências do pyproject.toml
   uv sync
   ```

3. **Ative o ambiente virtual:**
   *No Git Bash (Windows/Linux):*
   ```bash
   source .venv/Scripts/activate
   ```
   *No PowerShell:*
   ```powershell
   .venv\Scripts\Activate.ps1
   ```

---

## 📂 Organização dos Arquivos e Dados

Toda a base de dados pesada em formato `.csv` e `.json` é gerenciada na pasta `/bases` e protegida pelo `.gitignore` para manter o repositório leve.

> [!TIP]
> Para baixar as bases originais direto do portal de transparência da prefeitura municipal com as nomenclaturas exatas, consulte o arquivo detalhado de instruções: **[bases/BASES.md](file:///c:/Users/Felipe/Downloads/grafico_dispersao_XY/bases/BASES.md)**.

---

## 📓 Linha de Execução do Pipeline (Notebooks)

O projeto está dividido em uma sequência organizada de **4 Jupyter Notebooks** na pasta `src/`:

### 1. 🧹 [01_etl_base_unificada.ipynb](file:///c:/Users/Felipe/Downloads/grafico_dispersao_XY/src/01_etl_base_unificada.ipynb)
* **Objetivo:** Processo de ETL (Extração, Transformação e Carga). Carrega os 7 JSONs brutos, faz o mapeamento e a padronização dos nomes das secretarias, expande a série de funcionários por mês/ano e exporta a base de dados integrada de mais de 20 mil registros em `bases/base_unificada.csv`.

### 2. 📈 [02_analise_correlacoes.ipynb](file:///c:/Users/Felipe/Downloads/grafico_dispersao_XY/src/02_analise_correlacoes.ipynb)
* **Objetivo:** Realiza a fatorização das variáveis categóricas e calcula a Correlação de Pearson em relação à variável de instabilidade de gastos. Gera o relatório `CORRELACOES.md` e renderiza gráficos visuais (gráfico de correlações relevantes e plano cartesiano de dispersão X-Y com linha de tendência).

### 3. 📐 [03_calculo_amostra.ipynb](file:///c:/Users/Felipe/Downloads/grafico_dispersao_XY/src/03_calculo_amostra.ipynb)
* **Objetivo:** Aplica as fórmulas de cálculo de tamanho amostral (amostra para populações infinitas e correção de fator de população finita usando o total da base unificada) para definir o tamanho amostral estatisticamente representativo para responder à pergunta problema.

### 4. 🔬 [04_regressao_ols.ipynb](file:///c:/Users/Felipe/Downloads/grafico_dispersao_XY/src/04_regressao_ols.ipynb)
* **Objetivo:** Regressão Linear Múltipla usando Mínimos Quadrados Ordinários (OLS) via `statsmodels`. Aplica o diferencial exigido de conversão matemática das variáveis categóricas qualitativas selecionadas em **bits binários** (Binary Encoding) em vez de Dummies tradicionais.

---

## 📋 Variáveis Analisadas (Arquivos de Origem)

As análises cruzam informações de 4 tipos de dados municipais principais:

### 1. Estrutura do Quadro (`Quadro de Cargos-2025.json` e `2026.json`)
* **`classificacaoCargo`:** Se o cargo é Efetivo ou Comissionado (Variável chave para a pergunta).
* **`quantidadeVagasCriadas`:** O limite máximo de expansão de gastos daquela secretaria.
* **`quantidadeVagasPreenchidas`:** A taxa de ocupação real da estrutura.
* **`nivelEscolaridade`:** Requisito do cargo, que costuma ditar o patamar salarial inicial.
* **`situacaoCargo`:** Indica se o cargo está Ativo (gerando custo) ou em extinção.
* **`competencia`:** Mês de referência do quadro.

### 2. Perfil dos Agentes (`Agentes Públicos-Trabalhando.json`)
* **`valorRemuneracaoContratual`:** O custo nominal bruto de cada servidor.
* **`vinculoEmpregaticio`:** Estatutário, CLT, Temporário (vínculos possuem encargos e estabilidades diferentes).
* **`dataAdmissao`:** Utilizado para calcular o "Tempo de Casa" do servidor (anuênios/triênios).
* **`cargaHorariaSemanal`:** Permite normalizar o custo entre jornadas de 20h e 40h.
* **`orgao`:** Chave de ligação para cruzar com as despesas financeiras.

### 3. Execução Financeira (`Despesas com Pessoal-2025.json` e `2026.json`)
* **`descricaoElemento`:** Identifica se o gasto é com vencimentos, obrigações patronais ou indenizações extraordinárias (exonerações).
* **`valorEmpenhado`:** Planejado vs. realizado.
* **`tipoEmpenho`:** Ordinário, Estimativo, Global (indica a previsibilidade do pagamento).
* **`saldoAPagar`:** Restos a pagar acumulados.
* **`valorLiquidadoEmpenho`:** Valor real efetivado e conferido (fidelidade do gasto).
* **`dataEmpenho`:** Variável temporal para a série histórica.

### 4. Adiantamentos (`Adiantamentos-2025.json` e `2026.json`)
* **`valorPagamento`:** Custo direto de cada adiantamento emergencial (gastos imprevistos).
* **`funcao` e `fonteRecurso`:** Destino e flexibilidade da verba emergencial.
* **`acao`:** Motivo que gerou a liberação emergencial de caixa.