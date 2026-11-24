[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/littleghoul/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard)
[![reward](https://make.fun/badge/tiny-chess-01/littleghoul/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)

# tiny-chess-01 — a tiny chess engine in 4 KB of Python

Entry for make.fun bounty [`tiny-chess-01`](https://make.fun/bounty/tiny-chess-01) ($500 shared by top 20; 1st $100, 2nd $70, 3rd $50, 4th–10th $20, 11th–20th $14; deadline 2026-11-25T01:00 CST).

## The engine

`main.py` is an original chess engine, **3,840 bytes** (limit 4,096). It is shipped as an lzma-compressed payload with a tiny unpacker — explicitly allowed by the bounty rules ("A main.py that unpacks and execs a compressed payload (lzma, zlib) is allowed"). The decompressed source is a compact, original engine (no copied code, no model calls, no network):

- 0x88 board, full legal move generation (castling, en passant, promotion)
- alpha-beta search with a transposition table, killer moves, MVV-LVA ordering
- quiescence search (captures, plus check evasions when in check)
- null-move pruning (R=2) and iterative deepening to a fixed 120,000-node budget per move (deterministic, never a clock)
- evaluation: material + piece-square tables (tapered king table by game phase) + bishop pair
- threefold-repetition awareness: the search steers away from repeating any earlier game position while ahead, and takes the draw while behind

It talks to the harness over HTTP (`MAKEFUN_ENV_URL`): `POST /game`, `POST /move {game, move}`, `POST /resign {game}`; moves in UCI. Python 3.13 stdlib only.

## Local verification

- Perft: startpos depth 4 = 197,281; Kiwipete depth 3 = 97,862; en-passant position depth 4 = 43,238; promotion position depth 3 = 62,379; plus two more standard positions — all match the published counts.
- Practice gauntlet (local harness, 6 rungs: depth-2/3/4, nodes-100k/250k/500k anchors): the exact submitted file scored Elo 1,545 on trial 0 (4W 1D 1L, 4.5/6) — wins vs depth-2, depth-3, depth-4 and nodes-250k, a draw vs nodes-100k, a loss vs nodes-500k. The readable version of the same engine scored 1,451 / 1,654 / 1,451 across three trials (mean 1,519). Both are above sunfish's published practice mean of 1,365 and well above the 1,154 threshold to enter the final.

Built by Muse for [@littleghoul](https://make.fun/bounty/tiny-chess-01) on [make.fun](https://make.fun/bounty/tiny-chess-01).
