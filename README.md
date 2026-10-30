# Checkpoint 3 — Clustering aplicado à energia

Atividade prática em Python com **K-Means**, **método Elbow** e **Silhouette Score** para identificar perfis de consumo de energia em unidades consumidoras.

## Integrantes

| Nome | RM |
| --- | --- |
| Júlia Konishi | 569506 |
| Alexandre Rizzi | 569621 |
| Miguel Putini | 571624 |
| João Vitor Giadans | 571608 |
| João Victor Scheren | 568883 |

**Turma:** 1CCPO

## Objetivo da atividade

Uma equipe de gestão de energia quer conhecer os diferentes perfis de uso de suas unidades consumidoras. O objetivo é:

- explorar os dados e padronizar as variáveis;
- comparar diferentes quantidades de clusters (k) com o método Elbow e o Silhouette Score;
- justificar a escolha de k;
- visualizar e interpretar os perfis encontrados;
- propor ações de gestão de energia para cada perfil e discutir as limitações da análise.

## Base de dados

A base contém **120 unidades consumidoras fictícias**, geradas com NumPy (semente 303) para fins didáticos. Ela está **incorporada ao notebook** como texto CSV, então não é necessário baixar nenhum arquivo. Os dados não foram coletados de medidores, empresas ou bases públicas.

| Coluna | Significado | Unidade |
| --- | --- | --- |
| `id_unidade` | Identificador da unidade (excluído do modelo) | — |
| `consumo_mensal_kwh` | Energia total consumida no mês | kWh |
| `demanda_maxima_kw` | Maior demanda de potência média em janela de 15 minutos no mês | kW |
| `consumo_noturno_pct` | Parcela da energia mensal consumida entre 18h e 6h | % |

Não há valores ausentes nem identificadores duplicados.

## Estrutura do repositório

```
├── Checkpoint_3_Clustering_Energia_Entrega.ipynb   # notebook preenchido e executado
├── README.md
└── requirements.txt
```

## Como executar o notebook

1. Tenha o Python 3.9 ou superior instalado.
2. Instale as dependências:
   ```bash
   pip install -r requirements.txt
   ```
3. Abra o notebook no Jupyter Notebook, JupyterLab, VS Code ou Google Colab:
   ```bash
   jupyter notebook Checkpoint_3_Clustering_Energia_Entrega.ipynb
   ```
4. Execute todas as células na ordem (*Restart & Run All*).

Todos os ajustes do K-Means usam `random_state=42` e `n_init=10`, para que os resultados sejam reproduzíveis.

## Metodologia

1. **Exploração:** formato, tipos, valores ausentes, estatísticas descritivas e duplicidades.
2. **Padronização:** `StandardScaler` aplicado às três variáveis numéricas (`id_unidade` e os rótulos de cluster ficam fora do modelo).
3. **Elbow:** K-Means com k de 1 a 8 e análise da inércia.
4. **Silhouette Score:** cálculo para k de 2 a 8 sobre a mesma matriz padronizada.
5. **Modelo final:** ajuste com o k escolhido, contagem por grupo e score final.
6. **Interpretação:** gráficos de dispersão, tabela de perfis nas unidades originais e ações propostas.

## Principais resultados

**Elbow (inércia):**

| k | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Inércia | 360,00 | 184,70 | 74,25 | 61,10 | 52,06 | 46,36 | 40,90 | 37,25 |

A queda é grande até k=3 (de 184,70 para 74,25) e pequena depois (de 74,25 para 61,10).

**Silhouette Score:**

| k | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Score | 0,4835 | **0,6031** | 0,5190 | 0,4282 | 0,4343 | 0,3240 | 0,3106 |

**k escolhido: 3.** O Elbow e o Silhouette Score apontam para o mesmo valor. Os três grupos têm 40 unidades cada.

**Perfis encontrados (médias nas unidades originais):**

| Cluster | Unidades | Consumo médio | Demanda média | Consumo noturno médio | Perfil |
| ---: | ---: | ---: | ---: | ---: | --- |
| 0 | 40 | 876,50 kWh | 6,87 kW | 25,49% | Consumo intermediário, maior demanda, uso predominantemente diurno |
| 1 | 40 | 245,09 kWh | 2,58 kW | 60,82% | Baixo consumo e baixa demanda |
| 2 | 40 | 1433,93 kWh | 6,15 kW | 67,37% | Alto consumo, demanda moderada, maior parcela noturna |

**Ações propostas (resumo):**

- **Cluster 0:** investigar os picos de demanda e avaliar o escalonamento de equipamentos de maior potência.
- **Cluster 1:** manter monitoramento simples e orientar o uso consciente.
- **Cluster 2:** priorizar análise de eficiência energética e verificar quais usos ocorrem no período noturno, sem presumir desperdício.

## Limitações

- A base é simulada e pequena (120 unidades), portanto não representa consumidores reais.
- Há apenas três variáveis agregadas de um único mês; faltam curvas de carga, histórico, tipo de atividade, equipamentos e tarifas.
- O K-Means depende da escolha de k, é sensível a valores extremos e assume grupos aproximadamente esféricos.
- Os clusters indicam semelhança entre unidades, não provam desperdício nem substituem uma auditoria energética.

## Tecnologias

Python, pandas, NumPy, matplotlib e scikit-learn.
