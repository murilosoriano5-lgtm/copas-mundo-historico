# Copas do Mundo — Histórico Fictício (1990–2026)

Base de dados de uma **Copa do Mundo fictícia** — universo próprio, não é o histórico real da FIFA. Aqui a Copa acontece **todo ano**, de **1990 a 2026** (37 edições), em vez de a cada 4 anos como no mundo real.

Cobre seleções, edições e jogos (fase de grupos, mata-mata, resultados, artilheiros) desse universo.

## Estrutura do projeto

```
copas-mundo-historico/
├── README.md                     # este arquivo
├── CLAUDE.md                     # documentação técnica do projeto (schema, convenções, status)
├── docs/
│   ├── ESTRUTURA_DE_DADOS.md     # dicionário de dados: o que é cada coluna
│   └── REGRAS_DO_TORNEIO.md      # regulamento: potes, sorteio, chaveamento, desempate, ranking
└── data/
    ├── edicoes.csv               # 1 linha por Copa (ano, sede, campeão, vice...)
    ├── selecoes.csv              # seleções e confederações (tabela de referência)
    ├── jogos.csv                 # todos os jogos, edição por edição
    ├── potes.csv                 # composição dos 4 potes por edição
    ├── grupos.csv                # resultado do sorteio dos grupos por edição
    └── ranking_historico.csv     # snapshot do ranking usado pra montar os potes
```

## Formato do torneio (resumo)

32 seleções, 8 grupos de 4, 4 potes por pontuação no ranking histórico (sede + campeão anterior sempre no pote 1). Classificam os 2 primeiros de cada grupo, com chaveamento cruzado no mata-mata. Regulamento completo em `docs/REGRAS_DO_TORNEIO.md`.

## Status atual

- `edicoes.csv`: template com as 37 linhas (1990 a 2026), todas em branco aguardando os dados fictícios (sede, campeão, artilheiro...).
- `jogos.csv`, `potes.csv`, `grupos.csv`, `ranking_historico.csv`: templates vazios, aguardando os dados.
- `selecoes.csv`: template com seleções de exemplo (nações reais podem participar deste universo fictício, mas os resultados/campeões são inventados).

## Como contribuir com dados

Veja `docs/ESTRUTURA_DE_DADOS.md` para o formato exato de cada arquivo antes de adicionar linhas.

## Edições cobertas

37 edições, uma por ano: 1990, 1991, 1992 ... 2025, 2026 (Copa anual, não a cada 4 anos).
