# 🔥 Detecção de Cicatrizes de Fogo no Cerrado
### Recriação e Modernização Metodológica — Landsat 9 + Random Forest

Recriação modernizada do artigo **"Detecção de Cicatrizes do Fogo na Vegetação do Cerrado
no Distrito Federal entre 1999 e 2011"** (Silva, Costa & Matricardi, 2017), aplicada ao
PARNA da Chapada dos Veadeiros com dados de 2025.

![Mapa de classificação do GEE](/files/mapa-classificado.png)

## Sobre o Projeto

O objetivo é recriar a metodologia original (Landsat 5 + ACP + Árvore de Decisão) com
ferramentas modernas, avaliando o potencial de evolução da abordagem acadêmica.

| Artigo Original (2017) | Esta Recriação (2025) |
|---|---|
| Landsat 5 TM, TOA | Landsat 9 OLI-2, reflectância L2 |
| Árvore de Decisão, limiares manuais | Random Forest supervisionado |
| ROIs manuais como ground truth | Polígonos oficiais ICMBio |
| Sem separação treino/teste | Divisão por polígono, 80/20 |
| Acurácia 96.17% (inflada) | Acurácia 87.5% (dados não vistos) |
| Kappa 0.596 | Kappa 0.741 |
| Recall queimado 0.50 | Recall queimado 0.82 |


## Resultados

- **Acurácia:** 87.5%
- **Kappa:** 0.741 (Bom — Landis & Koch, 1977)
- **Recall queimado:** 0.82
- **Principal descoberta:** o alinhamento temporal entre imagem e polígonos de referência
  foi o fator mais crítico de desempenho, elevando o Kappa de 0.37 para 0.741


## Pipeline

```
Etapa 1 — Coleta de dados (ICMBio + Landsat 9 via GEE)
Etapa 2 — Pré-processamento (QA_PIXEL + índices espectrais + imagem única)
Etapa 3 — Geração de amostras (stratifiedSample por polígono)
Etapa 4 — Análise exploratória (Índice M + ACP)
Etapa 5 — Classificação (Random Forest, divisão por polígono)
Etapa 6 — Validação espacial (GEE + imagem independente)
```

## Especificações Técnicas

| Item | Detalhe |
|---|---|
| Linguagem | Python 3.12 |
| Plataforma | Google Earth Engine |
| Satélite | Landsat 9 OLI-2, Collection 2, Level-2 |
| Resolução | 30m |
| Período | Janeiro–Setembro 2025 |
| Área de estudo | PARNA Chapada dos Veadeiros, Goiás |
| Classificador | RandomForestClassifier (scikit-learn) + smileRandomForest (GEE) |

### Principais bibliotecas

```
earthengine-api
geemap
geopandas
scikit-learn
pandas
numpy
matplotlib
seaborn
geocanvas
```

## Como Usar

### 1. Clone o repositório

```bash
git clone https://github.com/AndersonAlvesCoelho/GAICQ
cd GAICQ
```

### 2. Crie o ambiente

```bash
conda create -n geo python=3.12
conda activate geo
pip install -r requirements.txt
```

### 3. Configure o Google Earth Engine

```bash
earthengine authenticate
```

### 4. Baixe os shapefiles do ICMBio

Os dados vetoriais não estão incluídos no repositório por questões de tamanho.
Baixe manualmente e organize na pasta `files/` conforme a estrutura abaixo.

**Limites das UCs (ICMBio):**
https://www.gov.br/icmbio/pt-br/servicos/geoprocessamento/malha-digital/dados-abertos

**Áreas Atingidas por Fogo — AAF (ICMBio):**
https://experience.arcgis.com/experience/2dfd350a50db4726bc9b964546cd645b/page/AAF---DOWNLOAD

Após o download, organize assim:

```
files/
├── limites_ucs_2025/
│   ├── limites_ucs_2025.shp
│   ├── limites_ucs_2025.dbf
│   ├── limites_ucs_2025.prj
│   └── limites_ucs_2025.shx
└── aaf_2025/
    ├── aaf_2025.shp
    ├── aaf_2025.dbf
    ├── aaf_2025.prj
    └── aaf_2025.shx
```

### 5. Execute o notebook

```bash
jupyter notebook acp_random_forest.ipynb
```

Execute as células em ordem. Cada etapa tem seu markdown explicativo.


## Como Contribuir

1. Fork o repositório
2. Crie uma branch para sua contribuição (`git checkout -b feature/minha-contribuicao`)
3. Faça as alterações e documente no markdown da etapa correspondente

4. Abra um Pull Request descrevendo o que foi alterado e por que

Sugestões de contribuição prioritárias:

- Implementar Leave-One-Polygon-Out completo na Etapa 5
- Estratificar amostras não queimadas por fisionomia via MapBiomas
- Adicionar índice BAI como feature adicional
- Testar com outras UCs do Cerrado
- Implementar validação cruzada espacial

---

## Licença

MIT License — sinta-se livre para usar, adaptar e distribuir com atribuição.
