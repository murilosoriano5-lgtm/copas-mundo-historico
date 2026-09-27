# CLAUDE.md — Documentação técnica do projeto

Este arquivo existe para que qualquer sessão do Claude (ou qualquer pessoa) entenda o projeto rapidamente, mesmo sem histórico de conversa anterior. É a "memória" persistente do projeto — atualize sempre que a estrutura ou o status mudar.

## Objetivo do projeto

Consolidar o histórico de jogos das Copas do Mundo FIFA masculinas de **1990 a 2026** em arquivos de dados estruturados (CSV), documentados e versionados no git.

## Por que CSV e não banco de dados

Simplicidade: fácil de editar, revisar em diff no git, importar em Excel/Python/Pandas sem dependências. Se o volume ou as consultas exigirem mais no futuro, migrar para SQLite é a evolução natural — mas não antes de haver necessidade real.

## Arquivos de dados (`data/`)

| Arquivo | Conteúdo | Status |
|---|---|---|
| `edicoes.csv` | 1 linha por edição da Copa (ano, sede, campeão, vice, artilheiro...) | Preenchido 1990–2022, 2026 pendente |
| `selecoes.csv` | Seleções nacionais e confederação | Template, pendente |
| `jogos.csv` | Todos os jogos de todas as edições | Template, pendente |

Schema completo de cada coluna: ver `docs/ESTRUTURA_DE_DADOS.md`.

## Convenções

- Datas no formato `AAAA-MM-DD`.
- Nomes de seleções em português, sempre iguais aos usados em `selecoes.csv` (ex: sempre "Alemanha", nunca alternar com "Alemanha Ocidental" sem justificar — ver nota abaixo).
- Alemanha Ocidental (1990) é tratada como "Alemanha" por continuidade histórica da FIFA. Se o usuário preferir distinguir, ajustar aqui e nos dados.
- `fase` em `jogos.csv` usa valores fixos: `grupos`, `oitavas`, `quartas`, `semifinal`, `terceiro_lugar`, `final`.
- Placar de pênaltis só é preenchido quando o jogo foi decidido nessa disputa.

## Status conhecido / pendências

- 2026: sede é Estados Unidos, Canadá e México. Resultado (campeão etc.) **não preenchido** — não deve ser inventado; só adicionar quando o usuário confirmar os dados oficiais.
- `jogos.csv` está vazio (só cabeçalho + 1 linha de exemplo). Os dados serão enviados pelo usuário aos poucos, por edição.

## Como adicionar dados

1. Confirmar o formato das colunas em `docs/ESTRUTURA_DE_DADOS.md`.
2. Adicionar as linhas no CSV correspondente.
3. Commitar com mensagem descritiva (ex: `Adiciona jogos da fase de grupos - Copa 1990`).
4. Atualizar a tabela de status neste arquivo e no README.
