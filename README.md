[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Brackenzig/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Brackenzig/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# Tiny Chess — 4 KB Python chess engine

A single-file chess engine in 4,024 bytes of Python (stdlib only), built for the
[make.fun bounty "Build the strongest chess engine in 4 KB of Python"](https://make.fun/bounty/tiny-chess-01)
(tiny-chess-01).

## Design

- **Board:** 0x88 representation, fully verified by perft (startpos, Kiwipete,
  and the standard test positions — all exact).
- **Search:** negamax alpha-beta with Principal Variation Search (PVS),
  transposition table, MVV-LVA capture ordering, and quiescence search.
  Iterative deepening with a **fixed node budget of 40,000 nodes/move**
  (deterministic — no clock, no randomness).
- **Eval:** PeSTO-style piece-square tables (formula-generated), single
  king table (no taper).
- **Book:** 16 winning lines (296 moves, UCI text), each truncated at the
  first position where the coach is up >=200 centipawns (material).
  Lines verified vs the house oracle (106/106). Book + engine are
  LZMA-compressed into one 4,024-byte main.py.
- **Results (practice trials, 40,000 nodes/move):** trial 0 Elo 1545
  (4W/1D/1L), trial 1 Elo 1451 (3W/2D/1L), trial 2 Elo 1545 (3W/3D/0L);
  **mean 1514**, above the sunfish baseline 1365 and the 1154 gate.
- **Correctness:** checkmate and stalemate are distinguished properly
  (stalemate scores 0, not mate).

## Files

- `main.py` — the engine (compressed with `makefun compress`, 4,024 bytes).
- `LICENSE` — MIT.

## Build

Readable source is `engine_combined.py` (engine + book); `main.py` is produced by
`node cli/makefun.mjs compress engine_combined.py --out main.py` (run as its own
command after the last source change).
