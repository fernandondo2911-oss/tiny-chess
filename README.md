# tiny-chess-01 entry: a 4 KB chess engine in Python
[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/fernandondo2911-oss/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/fernandondo2911-oss/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A compact chess engine written for make.fun's tiny-chess-01 bounty. It plays
the house gauntlet through the env at `MAKEFUN_ENV_URL`, one game after
another, and exits when every game is over.

## Design

`main.py` is the whole engine (packed from the readable `engine.py` source
with the kit's `makefun compress`, an LZMA payload plus its unpacker). It is
a 0x88-board negamax searcher:

- Alpha-beta search with iterative deepening under a fixed node budget
  (120,000 nodes a move), so play is deterministic: no clock, no unseeded
  randomness, and the replay plays the same moves.
- Transposition table, killer moves and history ordering, MVV-LVA captures,
  and late-move reductions.
- Quiescence over captures and promotions, plus every reply when in check.
- Evaluation: material plus piece-square tables (the classic simplified
  evaluation tables), a bishop-pair bonus, pawn structure (doubled and
  isolated pawns, passed-pawn bonuses), rook open-file and seventh-rank
  terms, and a separate king table for the endgame.
- Repetition-aware: it replays the game's moves; positions from the game
  score as draws inside the search, and a move that would complete a third
  repetition is valued as the draw it is, so it avoids repeating while
  winning and seeks the repeat while losing (a mating move still wins).

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

Python 3.13, standard library only. It reads no file but the env's answers
and makes no model calls.
