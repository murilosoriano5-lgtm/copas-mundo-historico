# Estrutura de dados

Dicionário de dados dos arquivos em `data/`. Sempre consulte antes de adicionar ou editar linhas.

## `edicoes.csv`

Uma linha por edição da Copa do Mundo.

| Coluna | Tipo | Descrição |
|---|---|---|
| `ano` | inteiro | Ano da edição (ex: 1990) |
| `sede` | texto | País(es)-sede |
| `campeao` | texto | Seleção campeã (vazio se ainda não disputada) |
| `vice_campeao` | texto | Seleção vice-campeã |
| `terceiro_lugar` | texto | Seleção em 3º lugar |
| `quarto_lugar` | texto | Seleção em 4º lugar |
| `numero_selecoes` | inteiro | Quantidade de seleções participantes |
| `numero_jogos` | inteiro | Total de jogos da edição |
| `artilheiro` | texto | Nome do artilheiro (pode ter mais de um, separados por `;`) |
| `gols_artilheiro` | inteiro | Quantidade de gols do artilheiro |
| `melhor_jogador` | texto | Vencedor da Bola de Ouro / prêmio equivalente da edição |

## `selecoes.csv`

Tabela de referência das seleções citadas em `jogos.csv` e `edicoes.csv`.

| Coluna | Tipo | Descrição |
|---|---|---|
| `nome` | texto | Nome padronizado da seleção (usado em todos os outros arquivos) |
| `confederacao` | texto | `UEFA`, `CONMEBOL`, `CONCACAF`, `CAF`, `AFC`, `OFC` |
| `codigo_fifa` | texto | Código de 3 letras da FIFA (ex: `BRA`, `GER`, `ARG`) — opcional |

## `jogos.csv`

Uma linha por jogo, de todas as edições.

| Coluna | Tipo | Descrição |
|---|---|---|
| `ano_edicao` | inteiro | Ano da Copa a que o jogo pertence (chave para `edicoes.csv`) |
| `fase` | texto | Um de: `grupos`, `oitavas`, `quartas`, `semifinal`, `terceiro_lugar`, `final` |
| `grupo` | texto | Letra do grupo (ex: `A`) — só preenchido quando `fase = grupos`, senão vazio |
| `data` | data | Formato `AAAA-MM-DD` |
| `cidade` | texto | Cidade onde o jogo foi disputado |
| `estadio` | texto | Nome do estádio |
| `selecao_casa` | texto | Nome da seleção mandante (usar nome padronizado de `selecoes.csv`) |
| `selecao_visitante` | texto | Nome da seleção visitante |
| `gols_casa` | inteiro | Gols da seleção mandante no tempo normal |
| `gols_visitante` | inteiro | Gols da seleção visitante no tempo normal |
| `gols_prorrogacao_casa` | inteiro | Gols adicionais na prorrogação (vazio se não houve prorrogação) |
| `gols_prorrogacao_visitante` | inteiro | Idem, visitante |
| `penaltis_casa` | inteiro | Pênaltis convertidos pela casa (vazio se não houve disputa) |
| `penaltis_visitante` | inteiro | Idem, visitante |
| `publico` | inteiro | Público presente no estádio (opcional, vazio se desconhecido) |

### Regras importantes

- Se o jogo terminou no tempo normal, deixar `gols_prorrogacao_*` e `penaltis_*` vazios.
- Se foi decidido na prorrogação sem pênaltis, preencher `gols_prorrogacao_*` e deixar `penaltis_*` vazio.
- Se foi para pênaltis, preencher `penaltis_casa` e `penaltis_visitante` com o placar da disputa.
- O resultado final do jogo é sempre `gols_casa + gols_prorrogacao_casa` (idem visitante) somado a quem venceu nos pênaltis, se houver.
