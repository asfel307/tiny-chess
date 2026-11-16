[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/asfel307/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/asfel307/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# Tiny Chess

A strong chess engine in 4 KB of Python — built for make.fun's
[tiny-chess-01](https://make.fun/bounty/tiny-chess-01) bounty ($500).

## Engine

`main.py` is a single-file engine (0x88 board) that plays the gauntlet
protocol over HTTP: it reads `MAKEFUN_ENV_URL`, plays each game the env
serves, and exits when the trial is done.

- negamax + alpha-beta pruning, iterative deepening to a fixed node budget
- quiescence search (captures + promotions, all evasions when in check)
- null-move pruning, late-move reductions, check extensions
- transposition table with incremental Zobrist hashing
- move ordering: TT move first, MVV-LVA captures, history heuristic
- evaluation: material + piece-square tables + king endgame table
- deterministic: fixed node budget, no wall clock, no randomness

The readable engine source is lzma-compressed inside `main.py` (allowed by
the competition rules) so the whole entry fits the 4,096-byte cap for
`main.py`. Decompressed it is ~6.8 KB of plain Python 3 stdlib only.

## Run a practice trial locally

```sh
# start the env server (from the competition kit)
MAKEFUN_ENV_DATA_DIR=<kit>/practice/trial-0/input/env \
MAKEFUN_STATE_DIR=/tmp/tiny-env-state \
MAKEFUN_ENV_PORT=8080 node program/env/server.mjs

# run the engine
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

## Test results (practice trials, local)

- Move generation verified: perft(1..4) from start = 20 / 400 / 8902 / 197281;
  Kiwipete perft(3) = 97862 (exact).
- Tactics: Scholar's mate Qxf7#, back-rank Rd8, promotion axb8=Q — all found.
- Speed: ~35,000 nodes/sec on this VM; node budget 150,000/move (~2.6–7.8 s/move).
- Practice Elo (local, 6 games/trial): trial-0 1279, trial-1 1191,
  trial-2 876 — mean 1115 (floor 1154; trials 0–1 clear it).

## License

MIT — see [LICENSE](LICENSE).

---
Entered on [make.fun](https://make.fun/bounty/tiny-chess-01) — ref=tvfbjxfd
