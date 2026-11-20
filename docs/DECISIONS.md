# Decisions

One line per methodological decision: what you chose, why, and what it changed.
Agents append here via the `/new-feature` skill; you append here by hand.

- Exclude `casual`, `registered`: they sum to `cnt` exactly. Leaky model R^2 1.000 -> honest ridge 0.583.
- Exclude `cnt_2d_bfr`: a shipped 2-day lag we cannot test the alignment of. Build our own lags instead.
- Exclude `instant`, `yr`, `days_since_2011`: time indexes let a tree predict a flat constant past 2011 and a linear model extrapolate a trend we have no grounds for.
- Split 2011 / H1 2012 / H2 2012 by date. Test set touched once, after all feature work.
- Report mean and persistence baselines with every score. Persistence (test R^2 0.510, MAE 840) is within 0.07 R^2 of ridge and beats it on MAE; that caveat goes in the report.


2026-11-20 — Makefile `model`: `ollama create -f -` does not read a Modelfile from stdin; write model/Modelfile.generated and pass that path. Environment fix, no effect on results.
2026-11-20 — CLAUDE.md: /init overwrote the starter file (dropped Gotchas, replaced cnt_2d_bfr with cnt, added ~80 lines of architecture). Reverted to the starter version.
2026-11-20 — Step 2 check: agent (qwen3.5:9b) got row count right (728); weather counts wrong (91/178/20, sums to 289 ≠ 728; actual 463/245/20); listed 2011-01-01 as missing (outside range) and called 2011-03-10 a Saturday (Thursday); did not flag casual/registered as leakage. Verified with pandas on data/raw/bike_daily.csv.
2026-11-20 - Step 2 check: agent (qwen3.5:9b) originally flagged `cnt_2d_bfr` as leakage, upon questioning, ammended reasoning for flagging to being unable to verify alignment