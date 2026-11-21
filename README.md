# Jensen Huang × NVIDIA: notícias, YouTube e ações

Projeto de ciência de dados (UFPB) que junta três bases para observar como a cobertura de notícias e a repercussão no YouTube sobre o caso Jensen Huang se relacionam com os movimentos das ações da NVIDIA (NVDA).

**Período:** 18/10/2026 a 18/11/2026, no fuso de Nova York. Pregões da NYSE de 19/10 a 18/11 (18/10 é domingo).
Os resultados descrevem uma relação observada neste período, não causa.

## Como rodar

1. Crie e ative um ambiente virtual e instale as dependências: `pip install -r requirements.txt`.
2. Copie `.env.example` para `.env` na raiz e coloque suas chaves da YouTube Data API em `YOUTUBE_API_KEYS`.
3. Abra os notebooks de dentro da pasta `notebooks/` (o código acha a raiz pela pasta `.git`) e rode, em qualquer ordem:
   - `coleta_youtube.ipynb`
   - `coleta_acoes.ipynb`
   - `coleta_noticias.ipynb`

   Cada um termina com um teste de junção que usa as tabelas dos outros, se já existirem.

## Estrutura

```
data/raw/        dados brutos das fontes (versionados) e caches (não versionados)
data/processed/  tabelas limpas e agregadas, prontas para a análise
notebooks/       coleta, limpeza e validação de cada fonte
```

| Fonte | Bruto (`data/raw`) | Processado (`data/processed`) |
|---|---|---|
| YouTube | `youtube_videos_bruto.csv` | `youtube_videos.csv`, `youtube_diario*.csv` |
| Ações | `acoes_bruto_nvda.csv`, `acoes_bruto_qqq.csv` | `acoes_nvidia.csv`, `acoes_modelo_mercado.json` |
| Notícias | `noticias_rss_bruto.csv` | `noticias_limpas.csv`, `noticias_diario*.csv` |

Os arquivos `*_pregao*.csv` usam `data_pregao` como chave comum. Os arquivos `*_caso.csv` têm só o conteúdo marcado como `sobre_o_caso`.

## Regras comuns às três bases

- **`data_pregao`:** publicação em fim de semana ou a partir das 16h (Nova York) vai para o próximo pregão; antes das 16h em dia útil, para o mesmo dia.
- **`sobre_o_caso`:** o título tem um termo exclusivo do caso, ou fala da pessoa/CEO junto com uma palavra de escândalo. É a mesma regra no YouTube e nas notícias.
- **Ações:** `retorno_anormal_beta_pp` (modelo de mercado contra o QQQ, estimado antes do estudo) é a medida principal. `retorno_anormal_pp` (diferença simples) fica para comparação.

## Limitações

Cada notebook tem uma seção de documentação com as limitações da sua fonte. As principais: amostra limitada por relevância no YouTube e no Google News, views cumulativas que dependem da data da coleta, e eventos concorrentes na janela (por exemplo, o balanço de 17/11). A janela termina em 18/11: a queda de 20/11 teve outro gatilho (investigação da SEC sobre a contabilidade), sem relação com o caso.

## Segredos

O `.env` com as chaves nunca deve ser commitado. Se uma chave vazar, apague-a no Google Cloud e crie outra.
