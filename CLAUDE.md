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
  - **Copa de 1991 completa (64/64 jogos).** Oitavas: Itália 1x0 Bélgica, Holanda 5x2 Dinamarca, México 0x1 Argentina, Inglaterra 2x1 Tchecoslováquia (Lado A) · Brasil 2x1 Polônia, Uruguai 2x0 Suécia, Alemanha 2(4)x2(5) França (pênaltis), Espanha 2x0 Camarões (Lado B). Quartas: Itália 2x0 Holanda, Argentina 1x2 Inglaterra (Lado A) · Brasil 1x0 Uruguai (prorrogação — mesma pendência de precisão: split normal/prorrogação não informado), França 2x0 Espanha (Lado B). Semifinal: Itália 1(4)x1(3) Inglaterra (pênaltis), Brasil 1x3 França. Terceiro lugar: Inglaterra 1x0 Brasil. **Final: Itália 3x1 França — Itália é campeã jogando em casa.** `edicoes.csv` atualizado: campeão Itália, vice França, 3º Inglaterra, 4º Brasil.
- `ranking_historico.csv` recalculado para `ano_referencia=1992`, acumulando 1990+1991 (128 jogos) — confirmado pelo usuário que o critério é pontuação TOTAL acumulada, não só da edição anterior. Topo: Itália (36 pts), Brasil (32), Inglaterra (30), França (28), Alemanha (23), Espanha/Holanda (22), Uruguai (18).
- **Copa de 1992: potes e grupos definidos.** Sede Suíça (seleção nova, sem histórico), campeão anterior Itália. Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990+1991) entre as 30 seleções da lista.
  - Pote 1: Suíça, Itália, Brasil (32), Inglaterra (30), França (28), Alemanha (23), Espanha (22), Uruguai (18).
  - Pote 2: México (13), Argentina (12), Polônia (11), Camarões (10), Egito (7), Austrália (5), Dinamarca (5), Suécia (5).
  - Pote 3: Irã (3), Nigéria (3), Nova Zelândia (1), Paraguai (1), África do Sul (0), Áustria (0), Bolívia (0), Canadá (0).
  - Pote 4: Emirados Árabes (0), Guatemala (0), Hungria (0), Irlanda (0), Japão (0), Líbia (0), Omã (0), Quênia (0).
  - **Bug corrigido:** a ordenação alfabética (desempate de pontuação igual) estava usando comparação padrão de string, que põe "África do Sul" e "Áustria" depois de Z por causa do acento. Corrigido com normalização de acentos antes de ordenar; documentado em `docs/REGRAS_DO_TORNEIO.md` para não repetir em edições futuras.
  - Grupos sorteados com `random.seed(1992)`, Suíça travada no Grupo A (regra fixa). Resultado em `data/grupos.csv`.
  - 10 seleções novas adicionadas a `selecoes.csv`: Suíça, Irlanda, Áustria, Hungria, Bolívia, Guatemala, Líbia, Quênia, Omã, Emirados Árabes.
  - **Fase de grupos de 1992 completa** (48 jogos em `jogos.csv`). Todos os grupos resolvidos por pontos + saldo de gols, sem necessidade de confronto direto.
    Classificação: A: Suíça/Polônia · B: Itália/Áustria · C: França/Suécia · D: Brasil/Hungria · E: Uruguai/Camarões · F: Inglaterra/Nigéria · G: Alemanha/Dinamarca · H: Espanha/Argentina.
  - **Copa de 1992 completa (64/64 jogos).** Oitavas: Suíça 4(3)x4(2) Áustria (pênaltis), França 3x0 Hungria, Uruguai 2x1 Nigéria (prorrogação), Alemanha 3x1 Argentina (Lado A) · Itália 2x0 Polônia, Brasil 1(4)x1(3) Suécia (pênaltis), Inglaterra 1x0 Camarões, Espanha 2x1 Dinamarca (Lado B). Quartas: Suíça 1(3)x1(5) França (pênaltis), Uruguai 0x1 Alemanha (Lado A) · Itália 2x0 Brasil, Inglaterra 2x3 Espanha (Lado B). Semifinal: França 2(12)x2(11) Alemanha (pênaltis — disputa longa), Itália 1x0 Espanha (prorrogação). Terceiro lugar: Alemanha 2x1 Espanha (prorrogação). **Final: França 2x0 Itália — França é campeã.** `edicoes.csv` atualizado: campeão França, vice Itália, 3º Alemanha, 4º Espanha.
  - Pendência de precisão (mesmo padrão já registrado): Uruguai x Nigéria, Itália x Espanha e Alemanha x Espanha (3º lugar) foram decididos na prorrogação, mas só o placar final foi informado — split normal/prorrogação não registrado.
- `ranking_historico.csv` recalculado para `ano_referencia=1993`, acumulando 1990+1991+1992 (192 jogos). Topo: Itália (54 pts), França (45), Alemanha/Inglaterra (42), Brasil (40), Espanha (35), Uruguai (28), Holanda (22).
- **Copa de 1993: potes e grupos definidos.** Sede México, campeão anterior Alemanha. Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990-1992) entre as 30 seleções da lista.
  - Pote 1: México, Alemanha, Itália (54), França (45), Inglaterra (42), Brasil (40), Espanha (35), Uruguai (28).
  - Pote 2: Holanda (22), Argentina (18), Egito (9), Nigéria (9), Suíça (9), Austrália (8), Colômbia (5), Paraguai (5).
  - Pote 3: Japão (4), Portugal (4), Argélia (3), Hungria (3), Marrocos (3), Nova Zelândia (3), Estados Unidos (1), Irlanda (1).
  - Pote 4: Cuba (0), Escócia (0), Jamaica (0), Líbano (0), Síria (0), Sudão (0), Uganda (0), Vietnã (0) — todas sem histórico na Copa fictícia.
  - Grupos sorteados com `random.seed(1993)`, México travado no Grupo A (regra fixa). Resultado em `data/grupos.csv`.
  - 8 seleções novas adicionadas a `selecoes.csv`: Escócia, Cuba, Jamaica, Sudão, Uganda, Líbano, Síria, Vietnã.
  - **Fase de grupos de 1993 completa** (48 jogos em `jogos.csv`).
    Classificação: A: México/Holanda · B: Suíça/Itália · C: Espanha/Austrália · D: Alemanha/Escócia · E: Uruguai/Argentina · F: França/Colômbia · G: Brasil/Paraguai · H: Inglaterra/Líbano.
  - **Novo critério de desempate usado pela 1ª vez (Grupo E):** Uruguai e Argentina empataram em pontos (7), saldo de gols (+2), gols marcados (6) e até no confronto direto (2x2) — usuário definiu critério novo: maior pontuação no ranking histórico acumulado decide. Uruguai (28 pts) ficou em 1º, Argentina (18 pts) em 2º. Regra adicionada permanentemente em `docs/REGRAS_DO_TORNEIO.md` como critério 4 (antes do sorteio, que agora é só o último recurso).
  - **Copa de 1993 completa (64/64 jogos).** Oitavas: México 1(14)x1(15) Itália (pênaltis), Espanha 1x0 Escócia, Uruguai 2x0 Colômbia, Brasil 3x1 Líbano (Lado A) · Suíça 1x2 Holanda (prorrogação), Alemanha 2x0 Austrália, França 4x2 Argentina (prorrogação), Inglaterra 1(5)x1(3) Paraguai (pênaltis) (Lado B). Quartas: Itália 2x0 Espanha, Uruguai 3x1 Brasil (Lado A) · Holanda 2x3 Alemanha, França 2x1 Inglaterra (Lado B). Semifinal: Itália 1x0 Uruguai (prorrogação), Alemanha 3x0 França. Terceiro lugar: Uruguai 2x1 França. **Final: Itália 0x2 Alemanha — Alemanha é bicampeã.** `edicoes.csv` atualizado: campeão Alemanha, vice Itália, 3º Uruguai, 4º França.
  - Pendência de precisão (mesmo padrão): Suíça x Holanda e Itália x Uruguai (semifinal) foram na prorrogação, só placar final informado.
- `ranking_historico.csv` recalculado para `ano_referencia=1994`, acumulando 1990-1993 (256 jogos). Topo: Itália (66 pts), Alemanha (63), França (58), Brasil (52), Inglaterra (50), Espanha (45), Uruguai (44), Holanda (31).
- **Copa de 1994: potes e grupos definidos.** Sede Suécia (sem histórico), campeão anterior França. Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990-1993) entre as 30 seleções da lista.
  - Pote 1: Suécia, França, Itália (66), Alemanha (63), Brasil (52), Inglaterra (50), Espanha (45), Uruguai (44).
  - Pote 2: Holanda (31), México (27), Argentina (25), Suíça (15), Austrália (14), Paraguai (12), Egito (11), Dinamarca (9).
  - Pote 3: Irã (7), Japão (7), Marrocos (7), Portugal (7), Chile (6), Nova Zelândia (6), Líbano (4), África do Sul (3).
  - Pote 4: Coreia do Sul (3), Peru (1), Angola (0), Bolívia (0), Bulgária (0), Líbia (0), Noruega (0), Sudão (0).
  - Grupos sorteados com `random.seed(1994)`, Suécia travada no Grupo A. Resultado em `data/grupos.csv`.
  - 3 seleções novas adicionadas a `selecoes.csv`: Bulgária, Noruega, Angola.
  - **Sedes/cidades desta edição informadas pelo usuário (8):** Estocolmo I, Estocolmo II, Uppsala, Örebro, Malmö, Helsingborg, Gotemburgo I, Gotemburgo II. Ainda não associadas a jogos específicos — usar quando o usuário mandar os resultados informando a cidade de cada partida.
  - **Copa de 1994 completa (64/64 jogos), com cidades preenchidas em todas as partidas** (primeira edição com esse detalhe completo). Grupos: A: Suécia/Suíça · B: Inglaterra/Dinamarca · C: França/México · D: Itália/Noruega · E: Espanha/África do Sul · F: Alemanha/Egito · G: Uruguai/Argentina · H: Brasil/Holanda. Oitavas, quartas e semifinal com várias disputas de pênaltis (Espanha 5x4 Egito, Itália 4x3 México, Brasil 6x5 Argentina, Suécia 4x3 França, Suécia 4x5 Espanha, Itália 6x7 Brasil) e prorrogações (Inglaterra 1x2 Itália, Alemanha 0x1 Brasil, Suécia 1x0 Itália no 3º lugar — splits normal/prorrogação não informados, mesma pendência já registrada). **Final: Espanha 1x2 Brasil — Brasil conquista o 2º título (o 1º foi em 1990).** `edicoes.csv`: campeão Brasil, vice Espanha, 3º Suécia, 4º Itália.
- `ranking_historico.csv` recalculado para `ano_referencia=1995`, acumulando 1990-1994 (320 jogos). Topo: Itália (80 pts), Alemanha (75), Brasil/França (69), Inglaterra (60), Espanha (59), Uruguai (51), Holanda (40).
- **Copa de 1995: potes e grupos definidos.** Sede Uruguai (sem histórico), campeão anterior Brasil. Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990-1994) entre as 30 seleções da lista.
  - Pote 1: Uruguai, Brasil, Itália (80), Alemanha (75), França (69), Inglaterra (60), Espanha (59), Holanda (40).
  - Pote 2: México (33), Argentina (32), Austrália (17), Dinamarca (16), Camarões (15), Paraguai (15), Colômbia (11), Nigéria (11).
  - Pote 3: Portugal (10), Nova Zelândia (9), Japão (8), Tunísia (7), Argélia (6), El Salvador (3), Bulgária (2), Costa Rica (1).
  - Pote 4: Angola (0), Chade (0), China (0), Coreia do Norte (0), Cuba (0), Haiti (0), Romênia (0), Vietnã (0).
  - Grupos sorteados com `random.seed(1995)`, Uruguai travado no Grupo A. Resultado em `data/grupos.csv`.
  - 4 seleções novas adicionadas a `selecoes.csv`: Romênia, Haiti, Chade, Coreia do Norte.
  - **Sedes/cidades desta edição (8, corrigidas com o usuário — "Montevidéu III" duplicado era erro de digitação, corrigido para Montevidéu II):** Montevidéu I, Montevidéu II, Montevidéu III, Montevidéu IV, Colônia, Maldonado, Sacramento, Punta del Este.
  - **Copa de 1995 completa (64/64 jogos).** Grupos: A: Uruguai/Dinamarca · B: Holanda/México · C: Argentina/Inglaterra · D: Alemanha/Colômbia · E: Brasil/Austrália · F: Itália/Paraguai · G: França/Tunísia · H: Espanha/Nigéria. Oitavas com destaque para Espanha 1(4)x1(5) Tunísia nos pênaltis (Tunísia elimina a Espanha). Quartas, semifinal (Alemanha 4x3 Itália nos pênaltis) e final: **Uruguai 0x1 Alemanha (prorrogação) — Alemanha conquista o 2º título** (após 1993; em 1992 o campeão foi a França, não a Alemanha — correção de um erro anterior). `edicoes.csv`: campeão Alemanha, vice Uruguai, 3º França, 4º Itália.
  - Pendência de precisão (mesmo padrão): Uruguai x México, Brasil x Paraguai (oitavas), Brasil x França (quartas) e Uruguai x Alemanha (final) foram na prorrogação, só placar final informado.
- `ranking_historico.csv` recalculado para `ano_referencia=1996`, acumulando 1990-1995 (384 jogos). Topo: Itália (94 pts), Alemanha (92), França (87), Brasil (79), Espanha/Uruguai (69), Inglaterra (67), Holanda (50).
- **Copa de 1996: potes e grupos definidos.** Sede Estados Unidos (3 pts de histórico, mas sede/campeão sempre travados independente de pontuação), campeão anterior Alemanha. Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990-1995) entre as 30 seleções da lista.
  - Pote 1: Estados Unidos, Alemanha, Itália (94), França (87), Brasil (79), Espanha (69), Uruguai (69), Inglaterra (67).
  - Pote 2: Holanda (50), México (38), Suécia (29), Austrália (23), Dinamarca (22), Camarões (18), Egito (18), Paraguai (18).
  - Pote 3: Colômbia (16), Nigéria (15), Tunísia (14), Nova Zelândia (13), Portugal (13), Japão (11), Chile (7), Marrocos (7).
  - Pote 4: Coreia do Sul (6), Áustria (5), Líbano (4), Bolívia (3), Arábia Saudita (1), Equador (0), Gana (0), Rússia (0).
  - Grupos sorteados com `random.seed(1996)`, Estados Unidos travados no Grupo A. Resultado em `data/grupos.csv`.
  - 3 seleções novas adicionadas a `selecoes.csv`: Rússia, Equador, Gana.
  - **Sedes/cidades desta edição (12, mais que o normal de 8):** Nova York, Los Angeles, Dallas, San Francisco, Seattle, Chicago, Washington D.C., Houston, Miami, Philadelphia, Kansas City, Atlanta. Ainda não associadas a jogos específicos.
  - **Copa de 1996 completa (64/64 jogos).** Grupos: A: Estados Unidos/Egito · B: Alemanha/Camarões · C: México/Itália (zebra: México passou na frente da Itália) · D: França/Tunísia · E: Holanda/Uruguai · F: Brasil/Áustria · G: Inglaterra/Dinamarca · H: Espanha/Austrália. Oitavas e quartas com Itália eliminando a França nos pênaltis e depois os EUA (sede) também nos pênaltis, antes de cair pra Holanda na semi. **Final: Holanda 2x3 Brasil (prorrogação) — Brasil conquista o 3º título** (1990, 1994, 1996). `edicoes.csv`: campeão Brasil, vice Holanda, 3º México, 4º Itália.
    Nota: nas oitavas, duas partidas (França x Itália e México x Tunísia) vieram com os rótulos "Lado A"/"Lado B" trocados na mensagem do usuário — os jogos em si batem exatamente com o padrão de chaveamento 1X/2Y, só a legenda que se confundiu. Registrado sem problema, não afeta os dados.
  - Pendência de precisão (mesmo padrão): Holanda x Áustria, Alemanha x Egito, Brasil x Uruguai (oitavas) e Holanda x Brasil (final) foram na prorrogação, só placar final informado.
- `ranking_historico.csv` recalculado para `ano_referencia=1997`, acumulando 1990-1996 (448 jogos). **Alemanha assume a liderança do ranking histórico** (104 pts) pela primeira vez, superando a Itália (102). Brasil (98) e França (95) seguem colados.
- **Copa de 1997: potes e grupos definidos.** Sede Argentina (sem histórico), campeão anterior Brasil. Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990-1996) entre as 30 seleções da lista.
  - Pote 1: Argentina, Brasil, Alemanha (104), Itália (102), França (95), Espanha (81), Inglaterra (79), Uruguai (76).
  - Pote 2: Holanda (66), México (53), Austrália (27), Tunísia (19), Nova Zelândia (17), Portugal (16), Estados Unidos (14), Japão (14).
  - Pote 3: África do Sul (9), Argélia (8), Coreia do Sul (6), Costa Rica (4), Peru (4), Arábia Saudita (3), Canadá (3), Gana (2).
  - Pote 4: Cuba (1), Rússia (1), Croácia (0), Equador (0), Libéria (0), Papua Nova Guiné (0), Senegal (0), Turquia (0).
  - Grupos sorteados com `random.seed(1997)`, Argentina travada no Grupo A. Resultado em `data/grupos.csv`.
  - 5 seleções novas adicionadas a `selecoes.csv`: Croácia, Turquia, Libéria, Senegal, Papua Nova Guiné.
  - **Sedes/cidades desta edição (10) informadas, aguardando associação aos jogos** (usuário disse que manda depois): Buenos Aires I, Buenos Aires II, Córdoba, La Plata, Rosário, Mar del Plata, Avellaneda, Mendoza, Santa Fé, Quilmes.
  - **Copa de 1997 completa (64/64 jogos).** Grupos: A: Argentina/Nova Zelândia · B: França/Portugal · C: Holanda/Itália · D: Espanha/México · E: Inglaterra/Tunísia · F: Brasil/Austrália · G: Japão/Alemanha · H: Uruguai/Rússia.
    **2º caso de empate "total" no Grupo F:** Brasil e Austrália empatados em pontos (5), saldo (+2), gols marcados (4) e confronto direto (1x1) — resolvido pelo critério 4 (ranking histórico): Brasil (98 pts) na época vs Austrália (27 pts) → Brasil 1º, Austrália 2º. Consistente com o chaveamento das oitavas que o usuário mandou.
    Oitavas/quartas/semis com 3 disputas de pênaltis (Inglaterra 4x3 Austrália, Brasil 4x3 Tunísia, Alemanha 5x4 Uruguai). **Final: Argentina 0x1 Alemanha — Alemanha conquista o 3º título** (1993, 1995, 1997), ampliando a liderança no ranking histórico. `edicoes.csv`: campeão Alemanha, vice Argentina, 3º Inglaterra, 4º Espanha.
  - Sedes/cidades de 1997 (10, informadas mas não associadas aos jogos — usuário optou por não detalhar cidade por partida nesta edição).
- `ranking_historico.csv` recalculado para `ano_referencia=1998`, acumulando 1990-1997 (512 jogos). Topo: **Alemanha (119 pts, folga confortável)**, Itália (108), França (105), Brasil (104), Espanha (96), Inglaterra (95), Uruguai (84), Holanda (78).
- **Copa de 1998: potes e grupos definidos.** Sede Brasil, campeão anterior Alemanha. Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990-1997) entre as 30 seleções da lista.
  - Pote 1: Brasil, Alemanha, Itália (108), França (105), Espanha (96), Inglaterra (95), Uruguai (84), Holanda (78).
  - Pote 2: Argentina (60), México (59), Austrália (33), Dinamarca (28), Egito (24), Japão (23), Portugal (22), Nova Zelândia (21).
  - Pote 3: Suíça (21), Colômbia (17), Estados Unidos (17), África do Sul (12), Bélgica (9), Coreia do Sul (7), Costa Rica (7), Irã (7).
  - Pote 4: Marrocos (7), Gana (6), Rússia (5), Cuba (2), Senegal (1), Angola (0), Palestina (0), Porto Rico (0).
  - Grupos sorteados com `random.seed(1998)`, Brasil travado no Grupo A. Resultado em `data/grupos.csv`.
  - 2 seleções novas adicionadas a `selecoes.csv`: Palestina, Porto Rico.
  - **Sedes/cidades desta edição (12):** São Paulo, Rio de Janeiro, Belo Horizonte, Porto Alegre, Fortaleza, Salvador, Recife, Belém, Goiânia, Curitiba, Manaus, São Luís. Ainda não associadas a jogos específicos.
  - **Copa de 1998 completa (64/64 jogos).** Grupos: A: Brasil/Argentina (decidido por gols marcados, mesmos pontos e saldo) · B: Holanda/Suíça · C: Dinamarca/Alemanha (zebra: campeã defensora só 2ª) · D: Espanha/Nova Zelândia · E: França/Colômbia · F: Uruguai/Austrália · G: Itália/Portugal · H: Inglaterra/México. Oitavas com zebras fortes: Nova Zelândia elimina a Dinamarca, Austrália elimina a França, Portugal elimina a Inglaterra (prorrogação). **Final: Brasil 4x2 Alemanha — Brasil conquista o 4º título** (1990, 1994, 1996, 1998), isolado como maior campeão. `edicoes.csv`: campeão Brasil, vice Alemanha, 3º Uruguai, 4º Austrália.
    Correção aplicada: nas quartas, "Austrália 2(4)x1(2) Itália" como enviado era inconsistente (placar direto não combina com disputa de pênaltis); confirmado com o usuário — foi 1x1 no tempo normal, Austrália venceu nos pênaltis por 4x2.
  - Sedes/cidades de 1998 (12) informadas mas não associadas aos jogos.
- `ranking_historico.csv` recalculado para `ano_referencia=1999`, acumulando 1990-1998 (576 jogos). Topo: **Alemanha (134 pts)**, Brasil/Itália (121, empatados), França (114), Inglaterra (104), Espanha (103), Uruguai (100), Holanda (85).
- **Copa de 1999: potes e grupos definidos.** Sede Alemanha, campeão anterior Brasil. Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990-1998) entre as 30 seleções da lista.
  - Pote 1: Alemanha, Brasil, Itália (121), França (114), Inglaterra (104), Espanha (103), Uruguai (100), Holanda (85).
  - Pote 2: Argentina (70), México (65), Austrália (43), Dinamarca (35), Portugal (31), Suécia (31), Nova Zelândia (30), Egito (26).
  - Pote 3: Suíça (25), Colômbia (24), Japão (24), Tunísia (24), Estados Unidos (21), Argélia (11), Marrocos (10), Áustria (9).
  - Pote 4: Gana (9), Coreia do Sul (7), Arábia Saudita (6), Canadá (6), Jamaica (1), Bahrein (0), Líbia (0), Trinidad e Tobago (0).
  - Grupos sorteados com `random.seed(1999)`, Alemanha travada no Grupo A. Resultado em `data/grupos.csv`.
  - 2 seleções novas adicionadas a `selecoes.csv`: Trinidad e Tobago, Bahrein.
  - **Sedes/cidades desta edição (10):** Berlim, Munique, Dortmund, Gelsenkirchen, Colônia, Nuremberg, Hamburgo, Stuttgart, Düsseldorf, Kaiserslautern. Ainda não associadas a jogos específicos.
  - **Copa de 1999 completa (64/64 jogos).** Grupos: A: Alemanha/Marrocos · B: Uruguai/Nova Zelândia · C: Holanda/Suécia · D: Espanha/Áustria · E: Inglaterra/Argentina · F: Itália/México · G: França/Portugal · H: Brasil/Austrália.
    **Correção de dados no Grupo D:** Espanha e Áustria ficaram empatados em pontos e saldo de gols na versão original enviada (o que, pelo critério "mais gols marcados", favoreceria a Áustria — contradizendo o chaveamento das oitavas). Usuário corrigiu o placar Espanha x Dinamarca de 1x0 para **2x0**, o que resolve o empate direto pelo saldo de gols (Espanha +2 x Áustria +1), sem precisar alterar a ordem dos critérios de desempate.
    **Final: Alemanha 4x2 Brasil — Alemanha conquista o 4º título** (1993, 1995, 1997, 1999), empatando com o Brasil como maior campeão (4 títulos cada). `edicoes.csv`: campeão Alemanha, vice Brasil, 3º Inglaterra, 4º Uruguai.
  - Sedes/cidades de 1999 (10) informadas mas não associadas aos jogos.
- `ranking_historico.csv` recalculado para `ano_referencia=2000`, acumulando 1990-1999 (640 jogos — 10 edições completas!). Topo: **Alemanha (155 pts)**, Brasil (137), Itália (131), França (123), Inglaterra (122), Uruguai (113), Espanha (111), Holanda (95).
- **Copa de 2000: potes e grupos definidos.** Sede Rússia (sem histórico — é a entidade "Rússia" distinta da antiga URSS), campeão anterior Alemanha. Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990-1999) entre as 30 seleções da lista.
  - Pote 1: Rússia, Alemanha, Brasil (137), Itália (131), França (123), Inglaterra (122), Uruguai (113), Espanha (111).
  - Pote 2: Holanda (95), Argentina (77), México (71), Austrália (51), Portugal (37), Suécia (37), Nova Zelândia (35), Egito (28).
  - Pote 3: Japão (27), Tunísia (26), Camarões (24), Estados Unidos (24), Nigéria (18), Polônia (16), Áustria (15), África do Sul (12).
  - Pote 4: Gana (11), Costa Rica (10), Arábia Saudita (7), Coreia do Sul (7), Equador (0), Fiji (0), Porto Rico (0), República Tcheca (0, seleção nova, distinta da antiga Tchecoslováquia).
  - Grupos sorteados com `random.seed(2000)`, Rússia travada no Grupo A. Resultado em `data/grupos.csv`.
  - 2 seleções novas adicionadas a `selecoes.csv`: República Tcheca, Fiji.
  - **Sedes/cidades desta edição (12):** Moscou I, Moscou II, São Petersburgo, Sochi, Samara, Volgogrado, Kazan, Kaliningrado, Ecaterimburgo, Nizhny Novgorod, Rostov, Krasnodar. Ainda não associadas a jogos específicos.
  - **Copa de 2000 completa (64/64 jogos).** Grupos: A: Holanda/Rússia (sede só 2º) · B: Brasil/Suécia · C: Uruguai/Arábia Saudita · D: Inglaterra/Portugal · E: Itália/Argentina · F: Austrália/Espanha · G: Alemanha/Nova Zelândia · H: França/Polônia.
    **Zebra histórica:** a Rússia (sede) eliminou o Brasil nos pênaltis ainda nas oitavas. **Final: Uruguai 0x1 Austrália — Austrália conquista o 1º título de uma seleção da confederação Ásia/Oceania**, depois de eliminar Espanha, Argentina (PR), Nova Zelândia (pên.) e Inglaterra (PR) no caminho — campanha de zebra do início ao fim. `edicoes.csv`: campeão Austrália, vice Uruguai, 3º Itália, 4º Inglaterra.
  - Sedes/cidades de 2000 (12) informadas mas não associadas aos jogos.
- `ranking_historico.csv` recalculado para `ano_referencia=2001`, acumulando 1990-2000 (704 jogos). Topo: **Alemanha (167 pts)**, Itália (149), Brasil (147), Inglaterra (135), Uruguai (131), França (130), Espanha (117), Holanda (107). Austrália (campeã) sobe para 70 pts, ainda fora do top 10 do acumulado histórico — título não implica ranking alto ainda.
- **Copa de 2001: potes e grupos definidos.** Sede Inglaterra, campeão anterior Austrália.
  - **Correção aplicada:** a lista original da América vinha com o cabeçalho "8 Vagas" mas 10 seleções listadas (Jamaica e Trinidad e Tobago sobrando no fim). Confirmado com o usuário: são 8 vagas mesmo, Jamaica e Trinidad e Tobago removidas da lista de 2001 (não participam desta edição).
  - Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990-2000) entre as 30 seleções da lista.
  - Pote 1: Inglaterra, Austrália, Alemanha (167), Itália (149), Brasil (147), Uruguai (131), França (130), Espanha (117).
  - Pote 2: Holanda (107), Argentina (83), México (75), Nova Zelândia (45), Portugal (43), Japão (30), Estados Unidos (24), Nigéria (21).
  - Pote 3: Rússia (16), Gana (14), Bélgica (12), Chile (8), Coreia do Sul (8), Peru (8), Líbano (7), Equador (3).
  - Pote 4: Senegal (3), Croácia (2), Costa do Marfim (0), Guiné (0), Iraque (0), Jordânia (0), República Democrática do Congo (0), República Tcheca (0).
  - Grupos sorteados com `random.seed(2001)`, Inglaterra travada no Grupo A. Usuário pediu para trocar os grupos B e C de letra (sem novo sorteio, só relabeling): B passou a ser Espanha/Japão/Coreia do Sul/República Tcheca, C passou a ser Alemanha/Estados Unidos/Peru/Costa do Marfim. Resultado final em `data/grupos.csv`.
  - 5 seleções novas adicionadas a `selecoes.csv`: Costa do Marfim, Guiné, República Democrática do Congo, Jordânia, Iraque.
  - **Sedes/cidades desta edição (10):** Londres I, Londres II, Manchester, Liverpool, Birmingham, Newcastle, Southampton, Sunderland, Leeds, Sheffield. Ainda não associadas a jogos específicos.
  - **Copa de 2001 completa (64/64 jogos).** Grupos: A: Inglaterra/Argentina · B: Espanha/República Tcheca · C: Alemanha/Estados Unidos · D: Nova Zelândia/Brasil (zebra: Brasil só 2º) · E: França/Croácia · F: Holanda/Itália (zebra: Itália só 2º) · G: Austrália/Portugal · H: Uruguai/México.
    **Final: Inglaterra 2x1 Argentina (prorrogação) — Inglaterra conquista o 1º título, jogando em casa.** `edicoes.csv`: campeão Inglaterra, vice Argentina, 3º Austrália, 4º Holanda.
  - Sedes/cidades de 2001 (10) informadas mas não associadas aos jogos.
- `ranking_historico.csv` recalculado para `ano_referencia=2002`, acumulando 1990-2001 (768 jogos). Topo: **Alemanha (179 pts)**, Inglaterra (156, subiu com o título), Itália (154), Brasil (153), França (140), Uruguai (138), Espanha (127), Holanda (120).
- **Copa de 2002: potes e grupos definidos.** Sede Canadá, campeão anterior Inglaterra. Pote 1 completado pelas 6 maiores pontuações do ranking acumulado (1990-2001) entre as 30 seleções da lista.
  - Pote 1: Canadá, Inglaterra, Alemanha (179), Itália (154), Brasil (153), França (140), Uruguai (138), Espanha (127).
  - Pote 2: Holanda (120), Argentina (94), Austrália (86), México (80), Nova Zelândia (58), Portugal (51), Suécia (43), Dinamarca (37).
  - Pote 3: Japão (30), Tunísia (29), Estados Unidos (28), Paraguai (21), Gana (18), Argélia (14), Líbano (8), Senegal (6).
  - Pote 4: Irlanda (5), Hungria (4), Iraque (3), Cazaquistão (0), Guatemala (0), Guiné Equatorial (0), Honduras (0), Togo (0).
  - Grupos sorteados com `random.seed(2002)`, Canadá travado no Grupo A. Resultado em `data/grupos.csv`.
  - 4 seleções novas adicionadas a `selecoes.csv`: Honduras, Guiné Equatorial, Togo, Cazaquistão.
  - **Sedes/cidades desta edição (8):** Montreal, Toronto I, Toronto II, Vancouver, Ottawa, Calgary, Edmonton, North York. Ainda não associadas a jogos específicos.
  - **Copa de 2002 completa (64/64 jogos).** Grupos: A: México/Canadá · B: Alemanha/Suécia · C: Argentina/Espanha · D: Brasil/Portugal · E: Itália/Holanda · F: França/Estados Unidos · G: Austrália/Inglaterra · H: Uruguai/Nova Zelândia.
    **Maior zebra do projeto até aqui:** Nova Zelândia (Pote 2, 2ª colocada do Grupo H) eliminou Austrália, Itália e Argentina no caminho e **goleou a Alemanha por 3x0 na final**, conquistando o 1º título. `edicoes.csv`: campeão Nova Zelândia, vice Alemanha, 3º França, 4º Argentina.
  - Sedes/cidades de 2002 (8) informadas mas não associadas aos jogos.
- `ranking_historico.csv` recalculado para `ano_referencia=2003`, acumulando 1990-2002 (832 jogos). Topo: **Alemanha (195 pts)**, Itália (166), Brasil (165), Inglaterra (162), França (156), Uruguai (148), Espanha (133), Holanda (126). Nova Zelândia (campeã) sobe para 73 pts, 12º lugar.
- `potes.csv` e `grupos.csv` já têm a Copa de 1990 completa: potes fornecidos pelo usuário, grupos sorteados por script Python com `random.seed(1990)`. Restrição aplicada no sorteio: França forçada no Grupo A e Brasil no Grupo B (lados opostos da chave), a pedido do usuário, para que só se encontrem na final se ambas vencerem seus grupos. Demais posições sorteadas aleatoriamente respeitando 1 seleção por pote por grupo.
- `selecoes.csv` tem as 32 seleções de 1990.
- Sedes, campeões e demais resultados são inventados pelo usuário — não usar dados reais da FIFA como referência ou preenchimento padrão.
- Regulamento completo (potes, sorteio, chaveamento, desempate, ranking) já fechado e documentado em `docs/REGRAS_DO_TORNEIO.md`.

## Como adicionar dados

1. Confirmar o formato das colunas em `docs/ESTRUTURA_DE_DADOS.md`.
2. Adicionar as linhas no CSV correspondente.
3. Commitar com mensagem descritiva (ex: `Adiciona jogos da fase de grupos - Copa 1990`).
4. Atualizar a tabela de status neste arquivo e no README.
