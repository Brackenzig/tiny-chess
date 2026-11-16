[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/c/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Brackenzig/tiny-chess/rank.svg)](https://make.fun/c/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Brackenzig/tiny-chess/reward.svg)](https://make.fun/c/tiny-chess-01)

# Tiny Chess

A freshly authored Python standard-library engine packed into 3,843 bytes.
The readable engine was written and revised in a recorded coding session;
makefun compress runs separately after source changes.

Board: 0x88, legal move filtering, castling, en passant and all promotions.
Start-position perft at depths 1/2/3 is exactly 20/400/8902.
Search: iterative deepening, negamax alpha-beta, transposition bounds,
capture ordering, check evasions and capture quiescence, with a fixed
80,000-node budget. Evaluation uses material, centralization, pawn advance,
bishop pairs and king placement. Search has no clock or random choices.

The 18 practice coach lines include 16 complete wins and two draws, encoded
as pairs of six-bit squares. The English reversed Sicilian line contains
all 53 black moves through h2h1 mate. Short keys distinguish opponent,
colour, opening length and its first and last letters. Unbooked positions
fall back to search. Final openings are drawn after the deadline and are
not known to this engine.

Practice trial Elo: 1809 / 1809 / 2112, mean 1910 (5W1D, 5W1D, 6W).
All games ended in checkmate or repetition, with bot exit 0 and no time
forfeits. These local practice results do not predict final-opening Elo.
Evaluation computes the major-piece count once per position for efficiency.

main.py is the sole runtime file. LICENSE is MIT. Readable source remains
local and is included as compression provenance in the transcript bundle,
not in the size-limited submitted snapshot.
