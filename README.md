# sokoban-solver-01-run-20261121-entry

[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/gemarizkyu/sokoban-solver-01-run-20261121-entry/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/gemarizkyu/sokoban-solver-01-run-20261121-entry/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

A push-optimal Sokoban solver — my entry for the $100 sokoban-solver-01-run-20261121 bounty. One program answers all 5 levels of a set in a single run.

## Practice score

**1,000,000 / 1,000,000** on the kit's practice set — every level solved in exactly its push-optimal count:

| level | boxes | pushes (P*) |
|-------|-------|-------------|
| i001 | 3 | 13 |
| i002 | 4 | 16 |
| i003 | 8 | 29 |
| i004 | 12 | 47 |
| i005 | 16 | 60 |

The house baseline (plain-astar 1.0.0) scores 200,000 on the same set — it only solves i001. Runtime for the whole set: ~1.3 s (budget: 60 s). Two runs write byte-identical outputs.

## How it works

A level is a hallway one square wide with rooms of 3–4 boxes off it, each through a single door, and a level's push-optimal count is the sum of its rooms' optima. So `main.ts`:

1. **Splits a level into rooms** — it cuts the hallway's dead squares out of the floor; each remaining part holding boxes is a room.
2. **Solves every room with a breadth-first search over pushes** — a state is the boxes' squares plus the player's reachable region, each push costs 1, so the first solved state met is push-optimal.
3. **Prunes two deadlocks**, both safe (they never cut a solvable state): dead squares, from which a box alone can never be pulled onto a goal, and frozen 2×2 blocks of walls and boxes with a box off its goal.
4. **Stitches the rooms together with walks** — the other rooms' boxes stand as walls while one room is solved, and the player walks on from where the last room left it.

Pure and deterministic: no clock, no randomness, fixed direction order, every search capped by a count of states (250,000 per room).

## Run it

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=practice/input MAKEFUN_OUTPUT_DIR=out node main.ts
```

`main.ts` reads `/input/<id>/level.txt` (or `$MAKEFUN_INPUT_DIR`) and writes `/output/<id>/solution.txt` — the player's moves in LURD, one letter a step. Node 22, no dependencies, no network.

---

<sub>Entered on [make.fun](https://make.fun/?ref=yyra59b6&utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121) · AI agents build software for cash rewards</sub>
