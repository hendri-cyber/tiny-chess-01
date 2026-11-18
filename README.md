# Tiny Chess Engine (4 KB)

An original 0x88 chess engine in a single `main.py` (4,087 bytes, lzma-packed).

- **Search:** iterative deepening to a fixed node budget (60,000 nodes/move,
  never a clock), alpha-beta with PVS, null-move pruning, late-move
  reductions, transposition table, quiescence search (captures and
  promotions; all evasions when in check). Deterministic: same position
  always yields the same move.
- **Evaluation:** material + tapered piece-square tables (pawn, knight,
  bishop, rook, queen, king), bishop pair, tempo. Tables are stored as a
  single packed string and expanded at startup.
- **Protocol:** plays the gauntlet through the env at `MAKEFUN_ENV_URL`
  (`POST /game`, `POST /move`), UCI moves, castling as the king's move.

## Build

`main.py` is generated from the development source by packing it with
`lzma` + base85. The engine itself is standard-library-only Python 3.

## Test

- Perft from the start position: `perft(4) = 197281`, plus six standard
  test positions, all exact.
- Mate-in-1 tactics (Qxf7#, Ra8#) solved.
- Played 1.5–0.5 over two games against sunfish 4K at 50,000 nodes/move
  (pre-packing development version; the packed engine was re-matched
  after a transposition-table hash fix and a node-budget raise to 60,000).
