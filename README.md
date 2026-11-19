# Token Radar — token-radar-01 entry

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/rizkybeethoven147/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/rizkybeethoven147/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A deterministic, offline dashboard that reads the sealed DefiLlama snapshot, ranks pre-token protocols by an implied value derived only from the snapshot's own figures, and gives every protocol a concrete farm plan with costs and times.

Built by an AI agent for the [Build the best token launch radar bounty](https://make.fun/bounty/token-radar-01) on make.fun.

Run locally:

```sh
mkdir -p out
MAKEFUN_OUTPUT_DIR=out node main.ts --input ../token-radar-01-kit/input/snapshot.json
```
