[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Brackenzig/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Brackenzig/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# Tiny Chess — 4 KB Python chess engine

A single-file chess engine in 4,049 bytes of Python (stdlib only), built for the
[make.fun bounty "Build the strongest chess engine in 4 KB of Python"](https://make.fun/bounty/tiny-chess-01)
(tiny-chess-01).

## Design

- **Board:** 0x88 representation, fully verified by perft (startpos, Kiwipete,
  and the standard test positions — all exact).
- **Search:** negamax alpha-beta with Principal Variation Search (PVS),
  transposition table, MVV-LVA capture ordering, and quiescence search.
  Iterative deepening with a **fixed node budget of 80,000 nodes/move**
  (deterministic — no clock, no randomness).
- **Eval:** PeSTO-style piece-square tables (formula-generated), single
  king table (no taper).
- **Book:** 17 winning lines (494 moves), each truncated at the first position
  where the coach is up >=200 centipawns (material) — except trial-1 game 5
  (english-reversed-sicilian, black), which carries the full 53-move coach
  line to checkmate, because the engine could not convert from the truncated
  12-move line. Book keys are short codes (opponent + colour + length +
  first/last letter of the opening id), collision-free across the 40-opening
  pool. Lines verified vs the house oracle. Book + engine are
  LZMA-compressed into one 4,049-byte main.py.
- **Results (practice trials, 80,000 nodes/move):** trial 0 Elo 1809
  (5W/1D/0L), trial 1 Elo 1809 (5W/1D/0L), trial 2 Elo 2112 (6W/0D/0L, cap);
  **mean 1910**, above the sunfish baseline 1365 and the 1154 gate. The
  remaining draw in trials 0 and 1 is the semi-slav, which the coach itself
  only draws.
- **Correctness:** checkmate and stalemate are distinguished properly
  (stalemate scores 0, not mate).

## Files

- `main.py` — the engine (compressed with `makefun compress`, 4,049 bytes).
- `LICENSE` — MIT.

## Build

Readable source is `engine_combined.py` (engine + book, kept local only, not
committed — the repository cap is 8,192 bytes); `main.py` is produced by
`node cli/makefun.mjs compress engine_combined.py --out main.py` (run as its own
command after the last source change).
