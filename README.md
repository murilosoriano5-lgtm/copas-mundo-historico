# Copas do Mundo — Histórico (1990–2026)

Base de dados histórica das Copas do Mundo FIFA masculinas, cobrindo todas as edições de **1990 até 2026**: seleções, edições e jogos (fase de grupos, mata-mata, resultados, artilheiros).

## Estrutura do projeto

```
copas-mundo-historico/
├── README.md                     # este arquivo
├── CLAUDE.md                     # documentação técnica do projeto (schema, convenções, status)
├── docs/
│   └── ESTRUTURA_DE_DADOS.md     # dicionário de dados: o que é cada coluna
└── data/
    ├── edicoes.csv               # 1 linha por Copa (ano, sede, campeão, vice...)
    ├── selecoes.csv              # seleções e confederações (tabela de referência)
    └── jogos.csv                 # todos os jogos, edição por edição
```

## Status atual

- `edicoes.csv`: preenchido de 1990 a 2022 (dados históricos consolidados). 2026 aguardando confirmação.
- `jogos.csv`: template criado, aguardando os dados dos jogos.
- `selecoes.csv`: template criado, aguardando lista de seleções.

## Como contribuir com dados

Veja `docs/ESTRUTURA_DE_DADOS.md` para o formato exato de cada arquivo antes de adicionar linhas.

## Edições cobertas

1990 · 1994 · 1998 · 2002 · 2006 · 2010 · 2014 · 2018 · 2022 · 2026
