# Comparação entre k-NN e SVM na Classificação de Desempenho Acadêmico e de Entregas Urbanas

Trabalho da disciplina de **Inteligência Artificial** do curso de **Ciência da Computação (5º bloco)** da **Universidade Estadual do Piauí (UESPI)**.

O projeto compara dois algoritmos de classificação supervisionada, **k-vizinhos mais próximos (k-NN)** e **máquina de vetores de suporte (SVM)**, em dois problemas de classificação binária com dados tabulares fornecidos pelo professor. Os experimentos estão em notebooks Jupyter e os resultados estão consolidados em um artigo científico.

## Autores

- Hilário Oliveira Marques Filho
- Ryan Charles Lima Sousa
- Luca Aureo Teixeira

## Problemas e dados

Cada conjunto tem 2.000 registros, 10 atributos de entrada e sem valores ausentes. Os dados foram fornecidos pelo professor.

| Conjunto | Arquivo | Alvo | Classes |
|---|---|---|---|
| Desempenho acadêmico | `dataset_educacao_desempenho_2000.csv` | `aprovado` | 1.338 não aprovados (66,9%) e 662 aprovados (33,1%) |
| Entregas urbanas | `dataset_entregas_urbanas_2000.csv` | `entrega_no_prazo` | 960 atrasadas (48,0%) e 1.040 no prazo (52,0%) |

**Atributos do desempenho acadêmico:** idade, horas de estudo por semana, frequência (%), atividades entregues (%), horas de sono, trabalha, deslocamento (min), internet estável, nota anterior e participação em aula.

**Atributos das entregas:** distância (km), nível de trânsito, intensidade de chuva, horário de pico, entregador fixo, tempo de preparação do pedido (min), prazo prometido (min), paradas adicionais, experiência do entregador (anos) e tipo de veículo (categórico).

## Estrutura do repositório

```
.
├── notebooks/
│   ├── Desempenho_Academico_KNN.ipynb
│   ├── Desempenho_Academico_SVM.ipynb
│   └── Classificacao_Entregas_Urbanas_KNN_SVM.ipynb
├── dados/                      # CSVs fornecidos pelo professor
├── artigo/
│   ├── Artigo_KNN_SVM.pdf
└── README.md
```


## O que cada notebook faz

**`Desempenho_Academico_KNN.ipynb`**
- Divisão 80/20 estratificada e padronização (`StandardScaler`).
- Variação de k (1, 3, 5, 7, 11, 15, 25 e 51).
- Comparação das distâncias Euclidiana, Manhattan, Minkowski (p=3) e Chebyshev.
- Efeito da padronização e do PCA (80% da variância).

**`Desempenho_Academico_SVM.ipynb`**
- SVM linear com C de 0,001 a 100.
- SVM com kernel RBF em uma grade de 25 combinações de C e gamma.
- Matriz de confusão, curva ROC (AUC), curva Precision-Recall e número de vetores de suporte.

**`Classificacao_Entregas_Urbanas_KNN_SVM.ipynb`**
- Pré-processamento com `ColumnTransformer` (padronização, atributos binários e *one-hot* para o tipo de veículo).
- k-NN: 8 valores de k × 2 distâncias (Euclidiana e Manhattan).
- SVM RBF: grade de 25 combinações de C e gamma.
- Seleção de hiperparâmetros por **validação cruzada estratificada de 5 partições, somente no treino**, e avaliação final única no teste com matrizes de confusão.

## Principais resultados

Os valores abaixo são do conjunto de teste (400 registros).

**Entregas urbanas** (k-NN com k=51 e distância de Manhattan; SVM RBF com C=10 e gamma=0,01):

| Modelo | Acurácia | F1 macro | Atrasos identificados | Entregas no prazo identificadas |
|---|---|---|---|---|
| k-NN | 82,25% | 82,17% | 151 de 192 (78,7%) | 178 de 208 (85,6%) |
| SVM RBF | 82,50% | 82,50% | 164 de 192 (85,4%) | 166 de 208 (79,8%) |

A diferença de acurácia é de um único acerto e não indica superioridade de nenhum dos algoritmos. O que muda é o tipo de erro: o SVM deixa passar menos atrasos e gera mais alertas falsos de atraso.

**Desempenho acadêmico** (referência: um classificador que sempre prevê a classe majoritária tem 67% de acurácia):

| Modelo | Acurácia | F1 macro | Recall de "aprovado" |
|---|---|---|---|
| k-NN (k=3, Euclidiana) | 67,5% | 0,617 | 0,43 |
| SVM linear (C=0,1) | 71,0% | 0,623 | 0,35 |
| SVM RBF (C=10, gamma=0,1) | 68,5% | 0,630 | 0,45 |

Todos os modelos ficaram pouco acima da referência e tiveram dificuldade em identificar os alunos aprovados.

## Limitações

- **Os dois problemas usam protocolos diferentes de escolha de parâmetros.** Em entregas, os parâmetros foram escolhidos por validação cruzada no treino. Em desempenho acadêmico, foram escolhidos olhando o próprio conjunto de teste, então as métricas desse problema podem estar otimistas e não são diretamente comparáveis às de entregas.
- Cada problema usa uma única divisão treino/teste de 400 registros, e diferenças de poucos pontos percentuais estão dentro da variação amostral.
- O SVM linear não foi avaliado em entregas, e k=51 é o limite superior da grade de k-NN.
- Os dados foram fornecidos pelo professor, e não se verificou aqui se refletem dados reais.

Os detalhes completos estão no artigo.

## Como executar

Requisitos: Python 3.9 ou superior.

```bash
git clone <URL-DO-REPOSITORIO>
cd <NOME-DO-REPOSITORIO>

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install numpy pandas matplotlib seaborn scipy scikit-learn jupyter

jupyter notebook notebooks/
```

Os notebooks usam `random_state=42` nas divisões de dados.

## Artigo

O artigo completo, com metodologia, resultados e discussão, está em [`artigo/Artigo_KNN_SVM.pdf`](artigo/Artigo_KNN_SVM.pdf). 

## Referências

- AYRES, L. D. G.; BRITO, I. V. S.; SOUZA, R. R. G. e. *Utilizando aprendizado de máquina para detecção automática de URLs maliciosas brasileiras*. Universidade Federal da Bahia.
- WESSEL, L. H. Aprendizado de máquina na predição de doença cardíaca: uma análise comparativa de algoritmos de classificação. *Revista Eletrônica Multidisciplinar de Investigação Científica*, v. 5, n. 23, 2026.
- SILVA, L. A.; PERES, S. M.; BOSCARIOLI, C. *Introdução à mineração de dados: com aplicações em R*. Rio de Janeiro: Elsevier, 2016.
- PEDREGOSA, F. et al. Scikit-learn: machine learning in Python. *Journal of Machine Learning Research*, v. 12, p. 2825-2830, 2011.
