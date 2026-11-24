# Tiny Chess

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/c/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/taniarestu/tiny-chess-01/rank.svg)](https://make.fun/c/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/taniarestu/tiny-chess-01/reward.svg)](https://make.fun/c/tiny-chess-01)

Entry for the [make.fun bounty "Tiny Chess"](https://make.fun/c/tiny-chess-01): a
chess engine in one Python file under 4 KiB, stdlib only, no network, no clock,
no unseeded randomness.

## The engine

0x88 board, negamax alpha-beta with iterative deepening to a fixed node
budget, transposition table (Zobrist, deterministic seed), MVV-LVA capture
ordering, two killer moves per ply, history heuristic, quiescence search with
stand-pat and delta pruning, null-move pruning, tapered material + piece-square
evaluation with bishop-pair bonus.

- Node budget: 100,000 nodes per move (measured ~40,000-65,000 nodes/sec on
  this machine: average ~2 s/move, worst observed ~7 s, hard limit 10 s/move).
- Practice Elo (local trials, kit scorer): 1,451 at 150,000 nodes
  (trial-0: 4W 0D 2L; trial-1 paired: 3W 2D 1L). The submitted 100,000-node
  budget trades a little Elo for a safety margin against the 10 s/move
  limit (a 150,000-node trial lost one game on time in a sharp endgame).
  Floor 1,154 passed.

## How it runs

The environment serves the game over HTTP; the engine answers each move:

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8931 python3 -P -s main.py
```

`main.py` is the packed entrypoint (see `engine.py`, the readable source it
was packed from with `makefun compress`).
