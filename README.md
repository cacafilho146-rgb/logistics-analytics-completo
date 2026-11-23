# 📦 Logistics Analytics — Meu primeiro projeto de Ciência de Dados

**Análise exploratória de entregas com Python, Pandas e Matplotlib.**

> **Aviso importante:** todos os dados são **sintéticos (inventados e gerados por código)**. Este é um projeto educacional de portfólio; os resultados não descrevem o desempenho de empresas ou regiões reais.

## 🎯 1. Problema de negócio

Imagine que você entrou como analista de dados em uma empresa de entregas. Sua equipe quer responder:

1. Qual porcentagem de entregas chega depois do prazo prometido?
2. Quais regiões apresentam maiores taxas de atraso?
3. Quanto tempo uma entrega demora, em média?
4. Qual modalidade de envio tem maior custo operacional médio?
5. Quais foram as 10 entregas com maiores custos operacionais?

O objetivo é **transformar registros de pedidos em informações úteis** para decisões de operação.

## 🛠️ 2. Tecnologias utilizadas

- **Python:** linguagem usada em todo o projeto.
- **Pandas:** leitura e análise de tabelas (DataFrames).
- **NumPy:** geração reproduzível da base fictícia.
- **Matplotlib:** criação de gráficos.
- **CSV:** formato dos dados de entrada e dos relatórios.
- **Git e GitHub (opcionais para execução):** versionamento e publicação do portfólio.

## 🗂️ 3. Estrutura de pastas

```text
logistics-analytics/
│
├── dados/
│   └── entregas.csv                      # 1.200 pedidos fictícios
│
├── src/
│   ├── gerar_dados.py                    # recria a base fictícia
│   └── analisar_entregas.py              # cálculos + relatórios + gráficos
│
├── resultados/
│   ├── indicadores.csv
│   ├── resumo_por_regiao.csv
│   ├── resumo_por_modalidade.csv
│   ├── top_10_entregas_mais_caras.csv
│   └── graficos/
│       ├── taxa_atraso_por_regiao.png
│       ├── tempo_medio_por_regiao.png
│       ├── custo_medio_por_modalidade.png
│       └── pedidos_por_mes.png
│
├── GUIA_DE_ESTUDO.md                     # explicação para iniciantes
├── requirements.txt                      # bibliotecas necessárias
├── .gitignore
└── README.md                              # este documento
```

## 🚀 4. Como executar (passo a passo)

### Passo 1 — Instale o Python

Tenha **Python 3.10 ou superior** instalado. Abra a pasta `logistics-analytics` no **VS Code** ou no **PyCharm** e abra um terminal dentro dessa pasta.

Confira a versão:

```bash
python --version
```

> No Windows, se `python` não funcionar e o lançador estiver instalado, experimente `py --version` e substitua `python` por `py` nos comandos seguintes.

### Passo 2 — Crie um ambiente virtual (recomendado)

No Windows (PowerShell):

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

No macOS/Linux:

```bash
python3 -m venv .venv
./.venv/bin/python -m pip install -r requirements.txt
```

> **Atalho:** se preferir não usar ambiente virtual por enquanto, rode `python -m pip install -r requirements.txt`. Nesse caso, use `python` nos próximos passos.

### Passo 3 — Gere a base de dados (opcional)

O arquivo `dados/entregas.csv` **já está incluído**. Você pode pular este passo. Para recriá-lo usando o ambiente virtual, use:

No **Windows (PowerShell)**:

```powershell
.\.venv\Scripts\python.exe src\gerar_dados.py
```

No **macOS/Linux**:

```bash
./.venv/bin/python src/gerar_dados.py
```

O código usa uma semente fixa (`42`), então gera os mesmos 1.200 registros em toda execução.

### Passo 4 — Execute a análise

No **Windows (PowerShell)**:

```powershell
.\.venv\Scripts\python.exe src\analisar_entregas.py
```

No **macOS/Linux**:

```bash
./.venv/bin/python src/analisar_entregas.py
```

Se **não** criou ambiente virtual e instalou as bibliotecas no seu Python padrão, use `python src/analisar_entregas.py`.

O terminal mostrará os indicadores. Os CSVs e gráficos serão (re)criados em `resultados/`.

## 📄 5. O que existe na base de dados?

A base tem **1.200 linhas** (uma entrega por linha) e **10 colunas originais**:

| Coluna | O que representa |
|---|---|
| `pedido_id` | Código único do pedido |
| `data_pedido` | Data em que o pedido foi feito |
| `data_prevista` | Data prometida para entrega |
| `data_entrega` | Data em que a entrega aconteceu |
| `regiao` | Região brasileira ficticiamente atendida |
| `estado` | Sigla do estado da entrega |
| `modalidade_entrega` | Econômica, padrão ou expressa |
| `distancia_km` | Distância simulada em quilômetros |
| `peso_kg` | Peso simulado da encomenda |
| `custo_entrega_reais` | Custo operacional simulado da entrega, em reais |

**Recorte:** pedidos feitos de janeiro a dezembro de 2025. Algumas entregas de pedidos feitos no fim de dezembro podem ocorrer nos primeiros dias de 2026.

O script de análise também cria temporariamente estas colunas:

- `atrasada`: `True` quando `data_entrega > data_prevista`, senão `False`;
- `dias_atraso`: quantos dias a entrega excedeu o prazo (nunca negativo);
- `dias_transito`: dias entre pedido e entrega;
- `mes_pedido`: mês em que o pedido foi realizado, como `2025-01`.

## 📊 6. Resultados encontrados

Os resultados abaixo saíram da **execução do código sobre os dados sintéticos incluídos**:

| Indicador | Resultado |
|---|---:|
| Entregas analisadas | 1.200 |
| Entregas atrasadas | 298 |
| Taxa de atraso | 24,83% |
| Tempo médio de entrega | 7,34 dias |
| Custo médio operacional por entrega | R$ 78,70 |
| Custo operacional total simulado | R$ 94.445,18 |

### Atrasos por região

![Gráfico da taxa de atraso por região](resultados/graficos/taxa_atraso_por_regiao.png)

| Região | Entregas | Taxa de atraso |
|---|---:|---:|
| Norte | 144 | 44,44% |
| Nordeste | 250 | 31,20% |
| Sul | 182 | 21,43% |
| Centro-Oeste | 146 | 19,18% |
| Sudeste | 478 | 18,62% |

**Interpretação:** neste **cenário inventado**, o Norte tem a maior porcentagem de entregas atrasadas. Isso sugere que vale investigar rotas, distâncias, prazos e modalidades nessa região. **Não** significa que a logística da região Norte real seja pior.

### Tempo médio por região

![Gráfico do tempo médio de entrega por região](resultados/graficos/tempo_medio_por_regiao.png)

### Custo médio por modalidade

![Gráfico do custo médio por modalidade](resultados/graficos/custo_medio_por_modalidade.png)

Os custos médios simulados são:

- **Expressa:** R$ 97,74
- **Padrão:** R$ 79,79
- **Econômica:** R$ 69,18

### Pedidos ao longo do ano

![Gráfico de pedidos por mês](resultados/graficos/pedidos_por_mes.png)

## 🔎 7. Explicação da análise

1. **Carregamento:** `pd.read_csv()` lê a base de entregas.
2. **Validação:** o código confere campos vazios, IDs duplicados, custos negativos e datas impossíveis.
3. **Transformação:** cria indicadores de atraso, tempo de trânsito e mês do pedido.
4. **Agrupamento:** `groupby()` reúne os resultados por região e modalidade.
5. **Cálculo de KPIs:** calcula totais, porcentagens, tempos e custos médios.
6. **Visualização:** Matplotlib salva quatro gráficos PNG.
7. **Comunicação:** os relatórios CSV e este README ajudam a explicar o que foi descoberto.

A taxa de atraso é calculada assim:

```text
Taxa de atraso (%) = (entregas atrasadas / total de entregas) × 100
```

Exemplo: `298 / 1200 × 100 = 24,83%` (arredondado).

## 💡 8. Conclusões e possíveis recomendações

**Conclusão 1:** a taxa geral simulada de atraso foi de **24,83%**. Uma equipe de operações poderia acompanhar esse indicador mensalmente.

**Conclusão 2:** a região Norte apresentou maior proporção de atrasos no cenário criado. O próximo passo seria separar entregas por distância e modalidade antes de atribuir uma causa.

**Conclusão 3:** a modalidade expressa teve maior custo operacional médio no cenário. Isso é coerente com as regras usadas para gerar os dados, mas uma decisão real exigiria analisar também receita, serviço e perfil das entregas.

> **Limitação:** como os dados foram produzidos por regras e probabilidades escolhidas para fins didáticos, não é adequado tirar conclusões reais sobre logística brasileira, prever o futuro ou afirmar relações de causa e efeito.

## 📚 9. O que eu aprendi

- Organizar um projeto Python em pastas.
- Carregar tabelas CSV com `pd.read_csv()`.
- Verificar duplicatas e informações ausentes.
- Trabalhar com datas e criar novas colunas.
- Calcular médias, totais, porcentagens e agrupamentos.
- Criar visualizações com Matplotlib.
- Explicar achados de forma clara e documentar resultados no GitHub.

Para explicações ainda mais detalhadas, leia [`GUIA_DE_ESTUDO.md`](GUIA_DE_ESTUDO.md).

## 🌱 10. Próximas melhorias possíveis

- Criar gráficos filtrando cada estado.
- Comparar atrasos por modalidade de entrega.
- Investigar a influência da distância no tempo de transporte.
- Construir um painel interativo com Streamlit (em uma versão futura).
- Substituir a base fictícia por um dataset público devidamente documentado.

## 👤 Autor

**[Adicione seu nome e o link do seu perfil do GitHub aqui]**

*Projeto pessoal criado para praticar Python e análise exploratória de dados.*
