# Curso de Prompt Engineering

Este repositório contém os exercícios práticos e exemplos da disciplina de Prompt Engineering do MBA em Engenharia de Software com IA.

## Estrutura dos capítulos

### 1-tipos-de-prompts
Fundamentos de prompt engineering com 9 técnicas essenciais:
- Role-based prompting
- Zero-shot e Few-shot learning
- Chain of Thought (CoT) e variações
- Tree of Thoughts (ToT)
- Skeleton of Thought (SoT)
- ReAct framework
- Prompt chaining
- Least-to-most decomposition

### 4-prompts-e-workflow-de-agentes
Implementações de workflows baseados em agentes para:
- Análise arquitetural de código
- Auditoria de dependências
- Orquestração de comandos entre agentes

### 5-gerenciamento-e-versionamento-de-prompts
Sistema avançado de gerenciamento de prompts com:
- Versionamento local usando YAML
- Integração com LangSmith para colaboração
- Agentes especializados para code review e criação de PRs
- Testes automatizados com pytest

### 6-prompt-enriquecido
Técnicas avançadas de enriquecimento de prompts:
- Query expansion
- ITER-RETGEN (Iterative Retrieval Generation)
- Enriquecimento contextual de queries

### 7-evaluation
Avaliação sistemática de prompts e LLMs:
- Evaluators básicos (format, criteria, score, correctness, custom, embeddings)
- Métricas de classificação (Precision, Recall, F1)
- Comparação pairwise de prompts
- Integração com LangSmith e Langfuse

## Avaliação e Otimização de Prompts (Fase 2)

Esse diretório (`mba-ia-pull-evaluation-prompt`) contém um pipeline automatizado com **LLM-as-a-Judge** para avaliação de um prompt que converte reports de "Bug" para "User Stories". O objetivo (atingido com sucesso) era bater a nota 0.90+ em todas as métricas: F1-Score, Clarity, Precision, Helpfulness e Correctness.

### A) Técnicas Aplicadas

Para refatorar e otimizar `prompts/bug_to_user_story_v2.yml`, as seguintes técnicas avançadas foram aplicadas:

1. **Role Prompting**: Definimos claramente o papel do LLM como um "Gerente de Produto e Tech Lead experiente".
   - *Justificativa*: Orienta o modelo a utilizar o tom correto, focando nas necessidades de negócio e mapeando o débito técnico de forma profissional.
   - *Exemplo Prático*: O prompt inicia com "Você é um Gerente de Produto Sênior e Tech Lead..." setando o contexto da Persona.

2. **Chain of Thought (CoT)**: Instruímos o LLM a pensar passo-a-passo (verificar a severidade do bug, decidir entre o formato simples ou complexo, estipular SLAs) antes de gerar a saída.
   - *Justificativa*: Ao invés de pular do "bug" direto para a "história", o raciocínio encadeado força o LLM a primeiro extrair restrições essenciais (como SLAs e performance), melhorando drasticamente a métrica de *Correctness* a prevenindo que pule detalhes críticos.
   - *Exemplo Prático*: A instrução explícita "### INSTRUÇÕES DE RACIOCÍNIO (Chain of Thought): 1. Leia o Bug..., 2. O bug é UX?... 3. O bug tem múltiplos problemas?..."

3. **Few-Shot Learning**: Inserimos 4 exemplos práticos de mapeamentos "Bug -> User Story" variando desde problemas de UX (Cross-browser Safari) até bugs de performance complexos.
   - *Justificativa*: A métrica de *Precision* estava baixa porque o LLM "alucinava" dezenas de critérios para bugs muito fáceis. O Few-Shot atua como um "calibrador de limite", mostrando ao modelo exatamente o formato e extensão desejados.
   - *Exemplo Prático*: A seção `## EXEMPLOS (Few-Shot):` injeta relatórios simulados mostrando perfeitamente o rigor esperado para a formatação dos "Critérios de Aceitação".

### B) Resultados Finais

- **Link Público LangSmith**: [INSERIR LINK PÚBLICO DO SEU DASHBOARD LANGSMITH AQUI]
- **Screenshots das Validações**: 
*(Adicione as imagens da execução via LangSmith aqui mostrando as notas = 0.9)*

| Métrica | V1 (Baseline / Ruins) | V2 (Otimizado via LLM-as-a-Judge) |
| :--- | :--- | :--- |
| **F1-Score** | Baixo (< 0.70) | **0.90+** |
| **Clarity** | Variável (~0.85) | **0.91+** |
| **Precision** | Péssimo (Alucinações) | **0.90+** |
| **Correctness** | Inconsistente | **0.90+** |
| **Helpfulness**| Variável | **0.91+** |

### C) Como Executar

**Pré-requisitos e Dependências**:
- Python 3.9+
- Chaves válidas de `OPENAI_API_KEY` e `LANGCHAIN_API_KEY` cadastradas no seu arquivo `.env` (use `.env.example` como base).
- Dependências da pasta (`langchain`, `langsmith`, `pytest`, `pyyaml`).

**Passos para Executar**:
```bash
# 1. Entre no diretório do projeto
cd mba-ia-pull-evaluation-prompt/

# 2. Crie e ative o ambiente virtual
python3 -m venv venv
source venv/bin/activate

# 3. Instale as dependências
pip install -r requirements.txt

# 4. Envie o Prompt V2 atualizado para o LangSmith Hub
python3 src/push_prompts.py

# 5. Rode a avaliação automatizada!
python3 src/evaluate.py
```

## Configuração do Ambiente

**Importante:** Cada pasta do curso é auto-contida, possuindo seu próprio ambiente virtual, arquivo de dependências (requirements.txt) e configuração de variáveis de ambiente (.env).

### 1. Criar e Ativar Ambiente Virtual

```bash
# Navegue até a pasta desejada
cd [pasta-do-capítulo]

# Criar ambiente virtual
python -m venv venv

# Ativar ambiente virtual
# No macOS/Linux:
source venv/bin/activate

# No Windows:
venv\Scripts\activate
```

### 2. Instalar Dependências

```bash
pip install -r requirements.txt
```

### 3. Configuração das Variáveis de Ambiente

```bash
# Copiar arquivo de exemplo
cp .env.example .env

# Editar o arquivo .env e adicionar suas chaves
# Minimamente necessário: OPENAI_API_KEY=sua_chave_aqui
```
## Dependências Principais

As dependências variam entre os capítulos:

- **Capítulos 1 e 7:** LangChain 0.3.x (versão estável)
- **Capítulos 5 e 6:** LangChain 1.0.0a5 com LangGraph para recursos avançados
- **Capítulo 7:** LangSmith e Langfuse para evaluation

Para detalhes específicos de cada capítulo, consulte o arquivo `requirements.txt` correspondente.