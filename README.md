```python
import pandas as pd
excel_path = 'Base_Consultorio_Nutricionista_2024_2025_SUJA.xlsx'
xls = pd.ExcelFile(excel_path)
print("Sheet names:", xls.sheet_names)
df_raw = pd.read_excel(excel_path, sheet_name=xls.sheet_names[0])
print("Shape:", df_raw.shape)
print("Columns:", df_raw.columns.tolist())
print(df_raw.head(2))


```

```text
Sheet names: ['Sheet1']
Shape: (5100, 17)
Columns: ['id_consulta', 'id_paciente', 'nome_paciente', 'cpf', 'telefone', 'email', 'genero', 'bairro', 'data_consulta', 'nutricionista', 'tipo_consulta', 'objetivo', 'peso', 'altura', 'valor_consulta', 'forma_pagamento', 'status_consulta']
   id_consulta  id_paciente  nome_paciente             cpf telefone                             email    genero    bairro data_consulta      nutricionista     tipo_consulta                objetivo  peso altura valor_consulta forma_pagamento status_consulta
0         4040          454  Matteo Aragão  962.531.840-24      NaN           aylacostela@example.org         F  Perdizes    2025-12-05      Dra Ana Paula  Consulta Inicial   Performance Esportiva   NaN  1.73m            250             pix       REALIZADA
1         3816         1083  Diogo Ribeiro             NaN      NaN  luiz-fernandoda-rosa@example.org  Feminino   Santana    2025-02-15  Dra Mariana Alves  Consulta Inicial  Controle de Colesterol  71.6   1.91            350          Debito       cancelado


```

# Documentação do Projeto: Inteligência de Dados para Consultório de Nutrição

Este documento apresenta a especificação técnica completa e a documentação para o repositório do GitHub do projeto de tratamento, modelagem e visualização de dados de um consultório de nutrição (período 2024–2025).

---

## 📌 1. Visão Geral do Projeto

O objetivo deste projeto é estruturar um pipeline completo de engenharia e análise de dados para um consultório de nutrição. Partindo de uma base de dados transacional bruta e inconsistente (`Base_Consultorio_Nutricionista_2024_2025_SUJA.xlsx`), o processo engloba:

1. **Limpeza e Tratamento de Dados** utilizando **Python (Pandas e RegEx)**.


2. **Modelagem Dimensional (Star Schema)** com criação de tabelas fato e dimensões padronizadas.


3. **Desenvolvimento de Dashboard em Power BI**, contemplando indicadores de desempenho (KPIs), recursos visuais avançados e design focado em experiência do usuário.



---

## 🐍 2. Limpeza e Tratamento de Dados (Python / Jupyter Notebook)

O script em Python executou as seguintes etapas de saneamento e modelagem:

* **Remoção de Duplicadas:** Eliminação de registros totalmente duplicados para mitigar ruídos nas análises.


* **Tratamento de Textos e Pontuação:** Padronização de nomes de pacientes e nutricionistas com remoção automatizada de títulos acadêmicos/médicos (*Dr.*, *Dra.*, *Nutri*) via expressões regulares (`re`), além da normalização de status de consultas, formas de pagamento e bairros para o formato *Title Case*.


* **Tratamento de Datas:** Conversão robusta da coluna de datas com tratamento de erros (`errors='coerce'`) para lidar com inconsistências de calendário (ex: *31/02/2025*).


* **Limpeza de Valores Monetários:** Extração de caracteres alfanuméricos espúrios, padronização de separadores decimais e conversão correta para o tipo numérico flutuante (`float`).


* **Normalização de Peso e Altura:** Correção de *outliers* extremos de digitação (ex: valores de peso mal formatados ou alturas em centímetros convertidas para metros).



---

## 🗄️ 3. Modelagem Dimensional (Star Schema)

A partir da base tratada, estruturou-se um modelo estrela (*Star Schema*) exportado no arquivo `Base_Tratada_Modelada_Nutricao.xlsx`, composto por:

| Tabela / Objeto | Descrição da Entidade | Chaves Principais / Relacionamentos |
| --- | --- | --- |
| **`f_Atendimento`** | Tabela Fato contendo os registros transacionais de consultas. | `id_consulta` (PK), `id_paciente`, `id_bairro`, `id_forma_pagamento`, `id_status_consulta`, `Id_nutricionista` |
| **`dim_paciente`** | Cadastro consolidado de pacientes, dados de contato e biometria. | `id_paciente` (PK) |
| **`dim_localidade`** | Informações geográficas de bairros dos atendimentos. | `id_bairro` (PK) |
| **`dim_forma_pagamento`** | Formas e modalidades de pagamento aceitas. | `id_forma_pagamento` (PK) |
| **`dim_status`** | Estados operacionais das consultas (*Realizada*, *Cancelada*, etc.). | `id_status_consulta` (PK) |
| **`dim_nutricionistas`** | Relação dos profissionais de nutrição do consultório. | `Id_nutricionista` (PK) |

---

## 📊 4. Indicadores, Gráficos e Design no Power BI

O relatório desenvolvido no Power BI (`.pbix`) consome a camada modelada e apresenta uma interface visual altamente profissional:

* **Indicadores (KPIs Principais):**
* **Faturamento Total** e Ticket Médio por consulta.
* **Volume Total de Atendimentos** e taxa de conversão/realização de consultas em relação aos cancelamentos.
* **Evolução Biométrica Média** (acompanhamento de peso e IMC dos pacientes).


* **Gráficos e Recursos Visuais:**
* **Gráficos de Linha e Tendência:** Análise temporal do faturamento e número de consultas por mês/ano.
* **Gráficos de Barras / Colunas:** Distribuição de atendimentos por nutricionista, objetivos dos pacientes (ex: *Performance Esportiva*, *Controle de Colesterol*) e bairros com maior demanda.
* **Matrizes e Segmentações (Slicers):** Filtros dinâmicos por período, profissional e forma de pagamento.


* **Design e Experiência do Usuário (UI/UX):**
* Estruturação em páginas temáticas com navegação fluida por botões.
* Paleta de cores alinhada ao segmento de saúde e bem-estar, com cartões limpos, fundos contrastantes e uso eficiente de espaço visual (sem poluição visual).



---

## 🚀 5. Estrutura sugerida para o Repositório GitHub (`README.md`)

```text
📁 repositorio-consultorio-nutricao/
│
├── 📂 data/
│   ├── Base_Consultorio_Nutricionista_2024_2025_SUJA.xlsx
│   └── Base_Tratada_Modelada_Nutricao.xlsx
│
├── 📂 notebooks/
│   └── limpeza_e_modelagem.ipynb
│
├── 📂 dashboard/
│   └── Consultorio_Nutricao_Analytics.pbix
│
└── README.md

```
