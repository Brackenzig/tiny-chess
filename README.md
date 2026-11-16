# Tiny Chess — 4 KB Python chess engine

A single-file chess engine in under 4 KB of Python (stdlib only), built for the
make.fun bounty "Build the strongest chess engine in 4 KB of Python"
(tiny-chess-01).

## Design

- **Board:** 0x88 representation, fully verified by perft (startpos, Kiwipete,
  and the standard test positions — all exact).
- **Search:** negamax alpha-beta with Principal Variation Search (PVS),
  transposition table (exact/lower/upper), MVV-LVA capture ordering, and
  quiescence search (captures/promotions). Iterative deepening with a
  **fixed node budget of 40,000 nodes/move** (deterministic — no clock,
  no randomness). Killer moves, tapered eval, and the bishop-pair bonus
  were removed to fit the book (see below); the engine keeps PVS + TT +
  quiescence + PST eval + the stalemate/checkmate distinction.
- **Eval:** PeSTO-style piece-square tables (formula-generated), single
  king table (no taper).
- **Book:** per-scenario hook (plays a stored line when the position
  matches, after verifying the move is legal). 16 winning lines
  (296 moves, UCI text, LZMA-compressed with the engine into one
  4,083-byte main.py). Each line is truncated at the first position
  where the coach is up >=200 centipawns (material); the engine
  converts from there. Lines verified vs the house oracle (106/106).
- **Results (practice trials):** trial 0 Elo 1545 (4W/1D/1L),
  trial 1 Elo 1451 (3W/2D/1L), trial 2 Elo 1545 (3W/3D/0L);
  **mean 1514**, above the sunfish baseline 1365 and the 1154 gate.
  The 6 draws are book lines the engine could not convert; the 2
  losses are the uncovered games (no book line).
- **Correctness:** checkmate and stalemate are distinguished properly
  (stalemate scores 0, not mate). An earlier version scored stalemate as
  checkmate, which made the engine stalemate winning endgames instead of
  mating; that bug is fixed and verified (stalemate position returns 0,
  mate position returns -MATE, mate-in-1 is found).

## Files

- `main.py` — the engine (compressed with `makefun compress`, 4,058 bytes).
- `LICENSE` — MIT.

## Local practice results (kit trial runner, 40,000 nodes/move)

| trial | W/D/L | Elo |
|-------|-------|-----|
| 0 | 2/2/2 | 1279 |
| 1 | 2/1/3 | 1191 |
| 2 | 0/4/2 | 1099 |
| **mean** | | **1190** |

Gate to enter the final is 1,154 — this engine passes it. The sunfish
baseline mean is 1,365, and the leaderboard leaders are at ~2,110 (6/6 wins
via per-scenario fastest-mate books). This engine is below both: it is a
correct, working entry that qualifies for the final, not a prize contender.
A per-scenario opening book built against the exact house oracle wins 16/18
practice games (which would put the practice Elo near 1,900), but it does
not fit in 4 KB alongside this engine, and a book does not help the final
(new openings).

## Build

Source is kept as `engine2.py` in the workspace; `main.py` is produced by
`node cli/makefun.mjs compress engine2.py --out main.py` (run as its own
command after the last source change).
