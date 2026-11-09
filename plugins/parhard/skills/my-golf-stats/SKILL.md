---
name: my-golf-stats
description: Use when the signed-in player asks about their own rounds, scores or scoring trend ("how did I play last week", "what is my scoring average", "my best round").
---
1. Call `my_rounds` for recent rounds or `my_stats` for averages, mix and trend. Use `round_detail` with a roundId for a hole-by-hole card.
2. These tools need the player to switch on rounds or stats access when connecting, and ParHard Plus. If the result says `plan_required` or the tool is not listed, say so plainly and point to parhard.com/plans or to reconnecting. Do not retry.
3. Report only what came back: the player's own strokes, gross and score to par. Never calculate or estimate a handicap or rating. Putts, fairways and greens appear only where the player recorded them.
4. Never invent a round or a number.
