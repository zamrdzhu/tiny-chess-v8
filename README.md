# tiny-chess-v8

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/zamrdzhu/tiny-chess-v8/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/zamrdzhu/tiny-chess-v8/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in a single `main.py` under the 4 KiB cap, built for make.fun's Tiny Chess bounty ($500 in USDC). It plays through the competition's HTTP environment protocol — no UCI, fully deterministic, fixed node budget per move (no wall clock in the search).

## Design

- **Search:** negamax with MTD-bi null-window iterative deepening over a fail-soft bound function, transposition table with lower/upper bounds, best-move table, killer moves, and null-move pruning (R=2).
- **Quiescence:** captures and promotions, with all evasions when in check; stand-pat otherwise.
- **Evaluation:** hand-authored piece-square tables (3 cp quantisation, file-symmetric), material, bishop-pair bonus, tapered king table (middlegame shelter → endgame centralisation by material phase), and dead-material damping for unwinnable endings.
- **Rules:** full move generation on a 120-square board — castling with attack-tested transit squares, double pushes, en passant (including pin cases), and queen promotions.
- **Root safety:** the driver keeps the best move of the last *completed* iteration, so an interrupted deepening step can never leak a half-searched move.

## Packaging

To fit the 4,096-byte limit, `main.py` ships as a self-extracting archive: the engine source is lzma-compressed and base85-encoded behind a two-line unpacker (the same packing style the house baseline uses). Locally it grades 1,364 / 1,451 on the kit's practice trials 0 and 1.

## Files

- `main.py` — the engine (Python 3, standard library only)
- `LICENSE` — MIT

---

<sub>Entered on [make.fun](https://make.fun/?ref=ta9jpaag&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
