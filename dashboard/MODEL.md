# radar-2 model

The implied value is the entry's own estimate of what a protocol's not-yet-trading token might be worth. It is not a price, and it is not a promise. Every input comes from the sealed snapshot or from the listed protocols inside that same snapshot. No protocol name is special-cased.

## Formula

```
categoryRatio = median( winsorize(mcap_usd / tvl_usd, 0.02, 2) )
                over listed protocols in the same category
fallback      = median over all listed protocols (0.169x in this snapshot)

growthMult    = 1 + 0.5 * tanh(change_7d / 50)
launchProb    = 1 / (1 + exp(-(age_no_token_days - 365) / 180))
chainBreadth  = 1 + 0.02 * min(max(chain_count - 1, 0), 4)
confidence    = 0.9 if change_7d is missing, times 0.9 if listing date is missing

implied = max(1, round(tvl_usd * categoryRatio * growthMult * launchProb * chainBreadth * confidence))
```

## Why these terms

- **TVL** sets the scale. A protocol holding more value has more to launch against.
- **categoryRatio** is learned from the snapshot's own listed protocols. A category whose tokens already trade at a high market cap per dollar of TVL gets a higher anchor. Ratios are winsorized to `[0.02, 2]` and a category needs at least four comparables; otherwise the global median is used.
- **tanh growth** shrinks a noisy seven-day move. A single -28% week cannot dominate the ranking the way a linear clamp can.
- **launchProb** is logistic in the time the protocol has run with no token. Long-running no-token protocols are overdue rather than merely mature; an unknown listing date uses 365 days and the row is flagged with lower confidence.
- **chainBreadth** and **confidence** are small, bounded adjustments for multi-chain deployment and missing data.

## Worked example

Steakhouse Financial, TVL $2.75B, 7d -2.19%, listed 543 days before the snapshot, Risk Curators on Base and Ethereum:

- categoryRatio: global median 0.169x (fewer than four listed Risk Curators comparables)
- growthMult: `1 + 0.5 * tanh(-2.19 / 50) = 0.978`
- launchProb: `1 / (1 + exp(-(543 - 365) / 180)) = 0.731`
- chainBreadth: `1.02`
- confidence: `1.0`
- implied: `2.75e9 * 0.169 * 0.978 * 0.731 * 1.02 ≈ $0.30B`

The rank-1 row in the screenshot shows the same factors, and the page recomputes the number from the same arithmetic before it renders.

## Coverage

Every protocol on the snapshot's watchlist is forced into the board. Every other rankable pre-token protocol is scored too, and the 300 strongest early finds are added. Protocols whose token already trades are kept in a separate Live token tab with their real market caps; they never enter the ranking.
