# eleicoes_rj

Coleta as eleições de 2018 a 2026 no RJ (Base dos Dados) e bases de contexto (ISP, RAIS, 1746, bairros) e monta um banco relacional DuckDB com os mesmos nomes de conjunto, tabela e coluna da origem.

## Como rodar

```
uv sync                                    # instala as dependências do pyproject.toml
gcloud auth application-default login      # credencial do Google Cloud para o BigQuery
BQ_PROJETO=<seu-projeto> uv run quarto render caderno_coleta.qmd
```

Saídas:

- `dfs/brutos/<recorte>/<conjunto>/<tabela>/*.parquet`: dado bruto, um arquivo por consulta. Nunca é sobrescrito.
- `dfs/eleicoes_rj_<recorte>.duckdb`: o banco, refeito a cada execução a partir dos Parquets.
- `caderno_coleta.html`: relatório com as checagens.

## Como ajustar

Tudo está no bloco `parametros` e na lista `EXTRACTIONS` do caderno.

- Estado inteiro em vez da capital: `MUNICIPIOS_TSE = []`.
- Mais uma tabela: acrescente uma linha em `EXTRACTIONS`.
- Atualizar 2026 depois do segundo turno: apague os arquivos `*_2026.parquet` e rode de novo.
- Guardar título eleitoral e CPF em claro: `HASH_PII = False`.
