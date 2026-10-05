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
- `jogos.csv`: fase de grupos de 1990 completa (48 jogos). Datas e nomes de estádio não foram informados pelo usuário — ficaram em branco de propósito (não inventar). Cidades preenchidas quando informadas.
- Classificação final dos grupos de 1990 (nenhum empate precisou de critério de desempate — pontos já definiram 1º/2º em todos os grupos):
  A: 1º França, 2º URSS · B: 1º Brasil, 2º Polônia · C: 1º Alemanha, 2º Chile · D: 1º Holanda, 2º México · E: 1º Espanha, 2º Tchecoslováquia · F: 1º Uruguai, 2º Argentina · G: 1º Inglaterra, 2º Iugoslávia · H: 1º Itália, 2º Colômbia.
- Oitavas de final de 1990 registradas em `jogos.csv` (8 jogos): França 1x0 Polônia, Alemanha 3x1 México, Espanha 1x0 Argentina, Inglaterra 4x2 Colômbia (Lado A) · Brasil 2x1 URSS, Holanda 2x0 Chile, Uruguai 1(4)x1(3) Tchecoslováquia (pênaltis), Itália 1x0 Iugoslávia (Lado B).
  - **Pendência de precisão:** Brasil 2x1 URSS foi decidido na prorrogação, mas o usuário só informou o placar final (2x1), não o placar no tempo normal antes da prorrogação. Ficou registrado como gols_casa=2/gols_visitante=1 com `gols_prorrogacao_*` em branco — não inventar o split; perguntar ao usuário se precisar do detalhe exato.
- Quartas de final de 1990 registradas em `jogos.csv` (4 jogos): França 1(4)x1(2) Alemanha (pênaltis), Espanha 0x3 Inglaterra (Lado A) · Brasil 2x1 Holanda, Uruguai 0x2 Itália (Lado B).
- **Copa de 1990 completa (64/64 jogos).** Semifinal: França 1x0 Inglaterra · Brasil 0x0 Itália (pên. 9x8). Terceiro lugar: Inglaterra 1x3 Itália. **Final: França 1x2 Brasil (prorrogação) — Brasil é o 1º campeão da Copa fictícia.** `edicoes.csv` atualizado: campeão Brasil, vice França, 3º Itália, 4º Inglaterra. Artilheiro/melhor jogador de 1990 ainda não informados (em branco).
  - Mesma pendência de precisão da final: placar final 1x2 informado, split normal/prorrogação não.
- `ranking_historico.csv` calculado para `ano_referencia=1991` a partir dos 64 jogos de 1990 (vitória=3, empate=1, derrota=0). Topo do ranking: Brasil e Itália (17 pts), França (16 pts), Alemanha e Inglaterra (13 pts).
- **Copa de 1991: potes e grupos já definidos — 100% por pontuação do ranking histórico, sem travas manuais.** Sede Itália, campeão anterior Brasil, pote 1 completado pelas 6 maiores pontuações entre as 30 seleções da lista do usuário. Potes 2–4 pela mesma lógica, ordem alfabética em caso de empate (ver `docs/REGRAS_DO_TORNEIO.md`).
  - Pote 1: Itália, Brasil, França (16 pts), Alemanha (13), Inglaterra (13), Espanha (12), Holanda (12), Uruguai (8).
  - Pote 2: Tchecoslováquia (7), Chile (6), México (6), Argentina (5), Polônia (5), Egito (4), Bélgica (3), Camarões (3).
  - Pote 3: Tunísia (3), Austrália (1), Argélia (0), Canadá (0), Coreia do Sul (0), Dinamarca (0), El Salvador (0), Etiópia (0).
  - Pote 4: Irã (0), Japão (0), Kuwait (0), Nova Zelândia (0), Paraguai (0), Peru (0), Suécia (0), Zimbábue (0).
  - **Correção aplicada:** a Tunísia estava grafada "Túnisia" em `selecoes.csv`/`potes.csv`/`grupos.csv` mas "Tunísia" em `jogos.csv`/`ranking_historico.csv` — isso fez o cálculo do pote ignorar os 3 pontos da Tunísia em 1990 e colocá-la no pote 4 por engano. Padronizado para "Tunísia" em todos os arquivos; pote 3/4 e sorteio de grupos refeitos.
  - **Regra permanente nova:** a sede cai sempre no Grupo A (adicionada em `docs/REGRAS_DO_TORNEIO.md`). Itália travada no Grupo A; demais posições sorteadas com `random.seed(1991)`. Resultado em `data/grupos.csv`.
  - 11 seleções novas (nunca jogaram em 1990) adicionadas a `selecoes.csv`: Suécia, Dinamarca, Canadá, Paraguai, El Salvador, Peru, Zimbábue, Argélia, Etiópia, Kuwait, Irã.
  - **Fase de grupos de 1991 completa** (48 jogos registrados em `jogos.csv`). Nenhum grupo precisou de confronto direto — todos resolvidos por pontos + saldo de gols (Grupo F: Argentina 2º sobre Tunísia por saldo; Grupo G: Inglaterra 1º sobre Camarões por saldo, ambos com mesma pontuação).
    Classificação: A: Itália/Polônia · B: Brasil/Bélgica · C: Holanda/Suécia · D: Uruguai/Dinamarca · E: México/França · F: Alemanha/Argentina · G: Inglaterra/Camarões · H: Espanha/Tchecoslováquia.
  - **Oitavas de 1991 (ainda não disputadas):** Lado A: Itália x Bélgica, Holanda x Dinamarca, México x Argentina, Inglaterra x Tchecoslováquia · Lado B: Brasil x Polônia, Uruguai x Suécia, Alemanha x França, Espanha x Camarões.
- `ranking_historico.csv` continua vazio — só passa a ser usado a partir da Copa de 1991 (1990 teve potes definidos manualmente pelo usuário).
- `potes.csv` e `grupos.csv` já têm a Copa de 1990 completa: potes fornecidos pelo usuário, grupos sorteados por script Python com `random.seed(1990)`. Restrição aplicada no sorteio: França forçada no Grupo A e Brasil no Grupo B (lados opostos da chave), a pedido do usuário, para que só se encontrem na final se ambas vencerem seus grupos. Demais posições sorteadas aleatoriamente respeitando 1 seleção por pote por grupo.
- `selecoes.csv` tem as 32 seleções de 1990.
- Sedes, campeões e demais resultados são inventados pelo usuário — não usar dados reais da FIFA como referência ou preenchimento padrão.
- Regulamento completo (potes, sorteio, chaveamento, desempate, ranking) já fechado e documentado em `docs/REGRAS_DO_TORNEIO.md`.

## Como adicionar dados

1. Confirmar o formato das colunas em `docs/ESTRUTURA_DE_DADOS.md`.
2. Adicionar as linhas no CSV correspondente.
3. Commitar com mensagem descritiva (ex: `Adiciona jogos da fase de grupos - Copa 1990`).
4. Atualizar a tabela de status neste arquivo e no README.
