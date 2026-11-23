# 📘 Guia de estudo — Logistics Analytics para iniciantes

Este guia foi pensado para quem **já conhece o básico de Python** e está começando com Ciência de Dados.

## 1. Primeiro: entenda o problema

Você não está tentando construir um aplicativo de entregas. Você é um(a) **analista de dados** que recebeu uma planilha com pedidos e precisa descobrir padrões.

A principal pergunta é: **"Nossas entregas estão chegando dentro do prazo?"**

Antes de analisar qualquer coisa, olhe os campos do arquivo `dados/entregas.csv`. Cada linha representa uma entrega. Cada coluna representa uma informação daquele pedido.

## 2. Conheça o DataFrame

O Pandas trabalha com objetos chamados `DataFrame`, que funcionam como tabelas.

```python
import pandas as pd

df = pd.read_csv("dados/entregas.csv")
print(df.head())       # mostra as primeiras cinco linhas
print(df.shape)        # mostra (quantidade de linhas, quantidade de colunas)
print(df.columns)      # nomes das colunas
print(df.isna().sum()) # quantidade de valores vazios por coluna
```

**Prática:** crie um arquivo `exploracao.py` na raiz do projeto, cole o exemplo acima e execute `python exploracao.py`. Esse arquivo é apenas um exercício, não precisa ser publicado.

## 3. Como saber se uma entrega atrasou?

O arquivo tem duas datas fundamentais:

- `data_prevista`: dia em que a empresa prometeu entregar;
- `data_entrega`: dia em que a entrega ocorreu.

Uma entrega atrasou se a data real for **posterior** à prevista.

No script principal, a linha é:

```python
df["atrasada"] = df["data_entrega"] > df["data_prevista"]
```

O Pandas compara as datas linha por linha e cria uma coluna de valores `True` ou `False`.

## 4. Por que a média de True/False vira percentual?

Em operações com Pandas, `True` equivale a 1 e `False` equivale a 0.

Se 3 de 10 pedidos atrasaram, a média será `0.3` e a taxa será `30%`.

```python
taxa_atraso = df["atrasada"].mean() * 100
print(f"Taxa de atraso: {taxa_atraso:.2f}%")
```

Na base incluída, são **298 pedidos atrasados dentre 1.200**, ou **24,83%**.

## 5. O que é groupby()?

Imagine que você quer descobrir a taxa de atraso de **cada região separadamente**. O `groupby()` divide a tabela em grupos e permite fazer cálculos dentro de cada grupo.

```python
atraso_regioes = df.groupby("regiao")["atrasada"].mean() * 100
print(atraso_regioes)
```

Esse trecho funciona depois de criar a coluna `atrasada` do passo 3. O arquivo principal já executa toda essa preparação de maneira automática.

## 6. Para que serve cada arquivo Python?

### `src/gerar_dados.py`

Cria dados de treinamento. Usamos `NumPy` para sortear regiões, datas, distâncias, modalidades e custos a partir de regras fictícias.

- `rng = np.random.default_rng(42)`: cria um gerador de números pseudoaleatórios com resultado reproduzível;
- `rng.choice(...)`: escolhe opções de uma lista de acordo com probabilidades;
- `pd.DataFrame(...)`: cria uma tabela;
- `df.to_csv(...)`: grava a tabela em um arquivo CSV.

**Importante:** quem está aprendendo pode usar o CSV pronto e deixar o gerador para estudar depois.

### `src/analisar_entregas.py`

É o arquivo central do seu projeto. Ele:

1. Lê os dados;
2. Confere erros básicos;
3. Cria colunas calculadas;
4. Resume os resultados;
5. Salva tabelas;
6. Gera quatro gráficos.

Os blocos aparecem como funções (`def ...`), uma forma de organizar etapas e evitar repetir código.

## 7. Entenda os quatro gráficos

**Gráfico 1 — Taxa de atraso por região:** ajuda a comparar proporções, não apenas quantidade absoluta de atrasos. Uma região com mais pedidos pode ter mais atrasos em números absolutos, mas uma taxa menor.

**Gráfico 2 — Tempo médio por região:** mostra quantos dias se passam entre a data de compra e a data da entrega.

**Gráfico 3 — Custo médio por modalidade:** compara o custo operacional médio fictício das entregas econômicas, padrão e expressas.

**Gráfico 4 — Pedidos por mês:** usa uma linha para demonstrar como a quantidade de pedidos variou durante 2025.

## 8. Exercícios para fazer sozinho

Experimente estes exercícios em ordem:

- [ ] Executar `src/analisar_entregas.py` e conferir os resultados.
- [ ] Abrir `dados/entregas.csv` e localizar o pedido `PED-00001`.
- [ ] No terminal ou em `exploracao.py`, mostrar apenas `df[["regiao", "custo_entrega_reais"]].head()`.
- [ ] Descobrir a entrega mais cara usando `df.nlargest(1, "custo_entrega_reais")`.
- [ ] Filtrar pedidos da região Sul: `df[df["regiao"] == "Sul"]`.
- [ ] Criar um novo gráfico de custo médio por região, aproveitando os exemplos do script.
- [ ] Escrever com suas palavras duas conclusões sobre os gráficos.

## 9. Como explicar o projeto em uma entrevista de estágio?

**Pergunta: "Qual problema você resolveu?"**

> Simulei a análise de operações de uma empresa de entregas. Meu objetivo foi medir atrasos, tempos de entrega e custos e comparar indicadores entre regiões e modalidades.

**Pergunta: "O que você fez com Python?"**

> Usei Pandas para ler e validar um CSV, trabalhar com datas, calcular taxas e médias, e agrupar dados. Depois gerei gráficos com Matplotlib e salvei relatórios para apresentar os resultados.

**Pergunta: "Qual foi sua principal descoberta?"**

> Na base sintética, a taxa geral de atrasos foi de 24,83%. O Norte apresentou a maior taxa, 44,44%, o que levanta uma hipótese para investigar distâncias e prazos — mas não prova a causa dos atrasos.

**Pergunta: "De onde vêm os dados?"**

> São dados fictícios e reproduzíveis gerados com Python e NumPy para fins de aprendizagem. Deixei isso explícito na documentação.

## 10. Como publicar no GitHub

1. Crie uma conta no GitHub, se ainda não tiver.
2. Crie um repositório **público** com o nome `logistics-analytics` (sem marcar a opção de README inicial, porque o projeto já tem um).
3. Abra um terminal na pasta do projeto.
4. Execute os comandos abaixo, trocando `SEU_USUARIO` pelo seu usuário do GitHub.

```bash
git init
git add .
git commit -m "Primeiro projeto: Logistics Analytics"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/logistics-analytics.git
git push -u origin main
```

> Esses comandos pressupõem que o Git já esteja instalado e configurado com sua identidade. O GitHub pode pedir autenticação via navegador ou método de credenciais configurado.

**Antes de publicar:** atualize a seção "Autor" do README, verifique se os gráficos abriram corretamente e leia as conclusões para saber explicá-las sem decorar frases.

## 11. Erros comuns

- **`ModuleNotFoundError: No module named 'pandas'`:** você ainda não instalou as bibliotecas no mesmo Python usado para executar o projeto. Rode `python -m pip install -r requirements.txt` ou use diretamente o Python do ambiente `.venv`.
- **`python: command not found` ou `'python' não é reconhecido`:** instale o Python e confirme que o terminal consegue encontrá-lo. No Windows, teste `py`.
- **`FileNotFoundError` para `entregas.csv`:** execute `python src/gerar_dados.py` para recriar o dataset.
- **Gráficos não apareceram na tela:** isso é esperado. O script salva arquivos PNG na pasta `resultados/graficos/` em vez de abrir janelas.

---

**Desafio final:** depois de conseguir executar tudo, tente alterar uma parte pequena do script por conta própria e explique como o resultado mudou. Esse processo faz você aprender muito mais do que simplesmente copiar o código.
