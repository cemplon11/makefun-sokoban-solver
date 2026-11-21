# makefun-sokoban-solver

A push-optimal Sokoban solver for make.fun's `sokoban-solver-01` bounty
(`sokoban-solver-01-run-20261121`).

## How it works

A generated level is a one-wide hallway with 1–4 rooms off it, each room
holding 3–4 boxes and as many goals. A box pushed out of a room's door can
never come back, so every box stays in its room and the level's push optimum
is the sum of the rooms' optima. The solver therefore:

1. cuts the level into hallway + rooms (dead squares of the player's row),
2. solves each room push-optimally with a breadth-first search over pushes
   (player-reachability regions, dead-square and frozen 2x2 pruning),
3. stitches the room solutions together with shortest walks between them.

Pure TypeScript, Node built-ins only. Fully deterministic: the search is
budgeted by states expanded, never by the clock — no `Date`, no
`Math.random`, no environment reads.

## Results (practice set)

| instance | pushes | P* | points |
|----------|--------|----|--------|
| i001 | 13 | 13 | 1,000 |
| i002 | 16 | 16 | 1,000 |
| i003 | 29 | 29 | 1,000 |
| i004 | 47 | 47 | 1,000 |
| i005 | 60 | 60 | 1,000 |

Grade-local score: **1,000,000** (house baseline plain-astar scores 200,000 on
the same set). Also verified at P* on three freshly drawn generator sets
(15/15 instances optimal, ~0.5 s per set).

## Run

```sh
MAKEFUN_INPUT_DIR=practice/input MAKEFUN_OUTPUT_DIR=out node main.ts
```

Reads `/input/<id>/level.txt` for `i001`–`i005`, writes
`/output/<id>/solution.txt` (LURD moves).
