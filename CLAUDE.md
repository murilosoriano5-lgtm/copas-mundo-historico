# CLAUDE.md — Documentação técnica do projeto

Este arquivo existe para que qualquer sessão do Claude (ou qualquer pessoa) entenda o projeto rapidamente, mesmo sem histórico de conversa anterior. É a "memória" persistente do projeto — atualize sempre que a estrutura ou o status mudar.

## Objetivo do projeto

Consolidar o histórico de jogos de uma **Copa do Mundo FICTÍCIA** (universo próprio do usuário, não é o histórico real da FIFA) em arquivos de dados estruturados (CSV), documentados e versionados no git.

**Regra importante para qualquer sessão do Claude:** NUNCA preencher `edicoes.csv` ou `jogos.csv` com resultados reais da Copa do Mundo de verdade (ex: Brasil campeão em 1994, França em 2018 etc.). Todo campeão, resultado e artilheiro aqui é inventado pelo usuário ou combinado com ele. Em caso de dúvida sobre um dado, perguntar antes de preencher.

**Diferença chave para o mundo real:** aqui a Copa é **anual** (1990 a 2026 = 37 edições, uma por ano), não a cada 4 anos.

**Regulamento completo do torneio** (potes, sorteio de grupos, chaveamento do mata-mata, critérios de desempate, ranking histórico): ver `docs/REGRAS_DO_TORNEIO.md`. Sempre consultar antes de montar potes, grupos ou calcular classificação.

## Por que CSV e não banco de dados

Simplicidade: fácil de editar, revisar em diff no git, importar em Excel/Python/Pandas sem dependências. Se o volume ou as consultas exigirem mais no futuro, migrar para SQLite é a evolução natural — mas não antes de haver necessidade real.

## Arquivos de dados (`data/`)

| Arquivo | Conteúdo | Status |
|---|---|---|
| `edicoes.csv` | 1 linha por edição da Copa (37 linhas: 1990–2026, uma por ano) | Template com anos preenchidos, resto em branco |
| `selecoes.csv` | Seleções nacionais e confederação | Template com exemplos, pendente |
| `jogos.csv` | Todos os jogos de todas as edições | Template vazio, pendente |
| `potes.csv` | Composição dos 4 potes de cada edição | Template vazio, pendente |
| `grupos.csv` | Resultado do sorteio dos grupos de cada edição | Template vazio, pendente |
| `ranking_historico.csv` | Snapshot do ranking usado para montar os potes | Template vazio, pendente |

Schema completo de cada coluna: ver `docs/ESTRUTURA_DE_DADOS.md`.

## Convenções

- Datas no formato `AAAA-MM-DD`.
- Nomes de seleções em português, sempre iguais aos usados em `selecoes.csv`.
- `fase` em `jogos.csv` usa valores fixos: `grupos`, `oitavas`, `quartas`, `semifinal`, `terceiro_lugar`, `final`.
- Placar de pênaltis só é preenchido quando o jogo foi decidido nessa disputa.
- Em jogo decidido nos pênaltis, o ranking histórico conta 1 ponto (empate) para as duas seleções — pênaltis definem quem avança, não a pontuação.
- Montagem de potes e sorteio de grupos: sempre seguir `docs/REGRAS_DO_TORNEIO.md`. Exceção: Copa de 1990 tem os potes fornecidos diretamente pelo usuário, não calculados.

## Status conhecido / pendências

- `edicoes.csv` tem as 37 linhas (1990–2026) com o ano preenchido; 1990 já tem sede (França) e número de seleções/jogos. Resto (campeão, vice, artilheiro...) em branco até os jogos serem disputados.
- `jogos.csv` e `ranking_historico.csv` estão vazios (só cabeçalho) — aguardando os jogos de 1990 serem enviados/registrados.
- `potes.csv` e `grupos.csv` já têm a Copa de 1990 completa: potes fornecidos pelo usuário, grupos sorteados por script Python com `random.seed(1990)`. Restrição aplicada no sorteio: França forçada no Grupo A e Brasil no Grupo B (lados opostos da chave), a pedido do usuário, para que só se encontrem na final se ambas vencerem seus grupos. Demais posições sorteadas aleatoriamente respeitando 1 seleção por pote por grupo.
- `selecoes.csv` tem as 32 seleções de 1990.
- Sedes, campeões e demais resultados são inventados pelo usuário — não usar dados reais da FIFA como referência ou preenchimento padrão.
- Regulamento completo (potes, sorteio, chaveamento, desempate, ranking) já fechado e documentado em `docs/REGRAS_DO_TORNEIO.md`.

## Como adicionar dados

1. Confirmar o formato das colunas em `docs/ESTRUTURA_DE_DADOS.md`.
2. Adicionar as linhas no CSV correspondente.
3. Commitar com mensagem descritiva (ex: `Adiciona jogos da fase de grupos - Copa 1990`).
4. Atualizar a tabela de status neste arquivo e no README.
