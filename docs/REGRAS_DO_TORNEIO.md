# Regulamento do torneio (Copa fictícia)

> Universo fictício, Copa anual (1990–2026, 37 edições). Ver `README.md` e `CLAUDE.md` para o contexto geral do projeto.

## Participantes

- **32 seleções** por edição.
- **8 grupos** (A a H) de **4 seleções** cada.
- Sem limite de seleções do mesmo continente no mesmo grupo.

## Formação dos potes

4 potes de 8 seleções cada, definidos pela **pontuação no ranking histórico** (ver seção abaixo).

- **Pote 1**: sede da edição + campeão da edição anterior (únicos 2 "travados" — todo o resto é por pontuação, sem outras travas manuais) + seleções de maior pontuação no ranking histórico acumulado até completar 8.
  - **Exceção — Copa de 1990**: é a primeira edição, não há "campeão anterior" nem histórico acumulado. Os potes dessa edição são definidos externamente (fornecidos pelo usuário), não calculados.
- **Potes 2, 3 e 4**: demais seleções, em ordem decrescente de pontuação histórica, 8 por pote.

## Fluxo de trabalho por edição

1. Usuário informa a sede da edição e a lista de seleções participantes.
2. Sistema identifica o campeão da edição anterior (de `data/edicoes.csv`).
3. Sistema calcula o ranking histórico acumulado até aquele momento (ver fórmula abaixo, baseado em `data/jogos.csv`).
4. Pote 1 é montado (sede + campeão anterior + maiores pontuações). Potes 2–4 completam por pontuação.
5. Sorteio dos grupos: 1 seleção de cada pote por grupo, sem restrição continental. **A sede cai sempre no Grupo A** (regra fixa); os demais, aleatório.
6. Jogos da fase de grupos são disputados e registrados.
7. Mata-mata segue o chaveamento cruzado (ver abaixo).

## Fase de grupos → mata-mata (16 avançam)

Os 2 primeiros colocados de cada grupo avançam.

### Chaveamento cruzado

| Colocação | Grupos | Lado do chaveamento |
|---|---|---|
| 1º lugar | A, C, E, G | **Lado A** |
| 1º lugar | B, D, F, H | **Lado B** |
| 2º lugar | A, C, E, G | **Lado B** (cruza) |
| 2º lugar | B, D, F, H | **Lado A** (cruza) |

Ou seja: o 1º colocado de um grupo nunca cai no mesmo lado do chaveamento que o 2º colocado do mesmo grupo — evita reencontro precoce e mantém a lógica de cruzamento como na Copa real.

### Confrontos das oitavas de final

Dentro de cada lado, os confrontos seguem o padrão clássico da Copa real (1º de um grupo contra 2º de outro grupo do mesmo lado):

**Lado A:** 1A x 2B · 1C x 2D · 1E x 2F · 1G x 2H
**Lado B:** 1B x 2A · 1D x 2C · 1F x 2E · 1H x 2G

Vencedores desses confrontos avançam para as quartas dentro do mesmo lado (1A/2B vs 1C/2D, 1E/2F vs 1G/2H — e o espelho no lado B), depois semifinal por lado, e os dois finalistas (um de cada lado) se enfrentam na final.

## Critérios de desempate (fase de grupos)

Aplicados nesta ordem até resolver o empate:

1. Saldo de gols
2. Mais gols marcados
3. Confronto direto
4. Sorteio (último critério, se tudo empatar)

## Ranking histórico (usado para formar os potes)

Pontuação por resultado, acumulada em todas as edições anteriores de uma seleção:

| Resultado | Pontos |
|---|---|
| Vitória | 3 |
| Empate | 1 |
| Derrota | 0 |

**Regra especial — pênaltis:** se a partida termina empatada (tempo normal/prorrogação) e é decidida nos pênaltis (típico do mata-mata), **conta como empate (1 ponto) para as duas seleções** no ranking histórico — o resultado dos pênaltis define quem avança no torneio, mas não altera a pontuação no ranking. Isto é: quem "perde" nos pênaltis também ganha 1 ponto de ranking, não 0.

O ranking é recalculado a partir de `data/jogos.csv` sempre que uma nova edição for montada (ver `data/ranking_historico.csv` para os snapshots já calculados).

**Critério de desempate no ranking (para formar potes):** quando duas ou mais seleções têm a mesma pontuação, usar **ordem alfabética** até o usuário definir outro critério. Primeira vez que isso foi necessário: potes de 1991 (ex: Chile e México empatados em 6 pontos).

**Seleções sem histórico (nunca disputaram a Copa fictícia):** entram no ranking com 0 pontos, concorrendo normalmente pelos potes 3/4.
