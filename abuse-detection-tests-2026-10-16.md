# Market abuse detection — test report

Generated: 2026-10-16T20:38:01.830Z

**27 of 27 test cases passed.** Dataset: synthetic, 12,839 source documents (13,870 executions, 3,331 order streams), 208 alerts at the current thresholds.

Each case plants a known behaviour in the data and checks that the expected metric found it. The planted documents carry an internal scenario tag used only to write this report — no detection engine reads it, so a pass means the behaviour was found from trade and order data alone.

## Coverage by metric

| Metric | Threshold | Cases | Passed | Instances scored | Alerts |
|---|---:|---:|---:|---:|---:|
| Insider Trading | $50,000 | 1 | 1 | 5 | 4 |
| Front Running | $20,000 | 3 | 3 | 15 | 10 |
| Internal Front Running | $25,000 | 1 | 1 | 22 | 7 |
| Ramping | $15,000 | 3 | 3 | 231 | 61 |
| Banging the Close | $20,000 | 1 | 1 | 39 | 4 |
| Wash Trading | $15,000 | 7 | 7 | 44 | 37 |
| Cross Trading | $15,000 | 1 | 1 | 4 | 3 |
| Off-Market Trading | $10,000 | 1 | 1 | 20 | 20 |
| Spoofing | $12,000 | 3 | 3 | 60 | 32 |
| Layering | $12,000 | 3 | 3 | 15 | 14 |
| Best Execution | $15,000 | 3 | 3 | 16 | 16 |

## Test cases

### TC-RAMP-01 — Ramping — barely above threshold

**Result:** PASS

- **Metric:** Ramping
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 35
- **Alerts raised:** 3 (USD Value $20,624 to $23,535, median $20,718, threshold $15,000)

**What is being tested.** That a run of aggressive same-direction trading followed by a reversal is priced on the impact it created, across all four product families.

**Scenario.** One trader buys (or sells) the same instrument five to eight times over roughly half an hour, all in the same direction and with no offsetting flow, then reverses the whole position in two or three clips four to eighteen minutes after the last build trade. Built once per product family. Sized to land just above the default threshold, so it proves the threshold is doing real work rather than letting everything through.

**Expected.** A Ramping alert on the synthetic instrument, with the reversal matched back to the build phase by the allocation algorithm and USD Value above the Ramping threshold.

**Evidence the alert must show.** EMI as the cumulative modelled impact at the moment of reversal, PTC as the notional reversed out, the build and reversal trades listed separately, and the impact curve showing the push and the exit into it.

**What actually triggered.** 7 bought $104.1m of JPY/USD, reversed after 12.0 minutes

> Priya Dunphy bought JPY/USD 7 times in 23.9 minutes, $104,138,909 in one direction. The Market Impact Model puts the cumulative impact of that build at 2.26 bps of the quoted level — a higher level — worth 2.26 bps in value terms once duration is taken into account. $106,008,763 was then traded back the other way 12.0 minutes later, of which $104,138,909 counts as the Profit-Taking Component (capped at the build notional under the exhaustion principle). USD Value = 2.26 bps x $104,138,909 = $23,535.

Trader Priya Dunphy, JPY/USD, 2026-09-14, USD Value $23,535.

| Computed value | Amount |
|---|---:|
| Expected Market Impact (EMI) | 2.26 bps |
| Level impact | 2.26 bps |
| Profit-Taking Component (PTC) | $104,138,909 |
| Build notional | $104,138,909 |
| Reversal notional | $106,008,763 |
| Allocated against the build | $104,138,909 |
| Build executions | 7 |
| Reversal delay | 12.0 min |
| Directional purity | 100.0% |
| Product | FX.Spot |

### TC-RAMP-02 — Ramping — significant above threshold

**Result:** PASS

- **Metric:** Ramping
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 34
- **Alerts raised:** 4 (USD Value $103,296 to $139,814, median $116,850, threshold $15,000)

**What is being tested.** That a run of aggressive same-direction trading followed by a reversal is priced on the impact it created, across all four product families.

**Scenario.** One trader buys (or sells) the same instrument five to eight times over roughly half an hour, all in the same direction and with no offsetting flow, then reverses the whole position in two or three clips four to eighteen minutes after the last build trade. Built once per product family. Sized well above the threshold — a clear case an analyst would pick up first.

**Expected.** A Ramping alert on the synthetic instrument, with the reversal matched back to the build phase by the allocation algorithm and USD Value above the Ramping threshold.

**Evidence the alert must show.** EMI as the cumulative modelled impact at the moment of reversal, PTC as the notional reversed out, the build and reversal trades listed separately, and the impact curve showing the push and the exit into it.

**What actually triggered.** 5 bought protection on $168.2m of GRNTP SNRFOR 3Y CDS, reversed after 10.6 minutes

> Frances Reyes bought protection on GRNTP SNRFOR 3Y CDS 5 times in 21.9 minutes, $168,191,171 in one direction. The Market Impact Model puts the cumulative impact of that build at 393.12 bps of the quoted level — a wider spread — worth 9.06 bps in value terms once duration is taken into account. $154,320,511 was then traded back the other way 10.6 minutes later, of which $154,320,511 counts as the Profit-Taking Component. USD Value = 9.06 bps x $154,320,511 = $139,814.

Trader Frances Reyes, GRNTP SNRFOR 3Y CDS, 2026-09-16, USD Value $139,814.

| Computed value | Amount |
|---|---:|
| Expected Market Impact (EMI) | 9.06 bps |
| Level impact | 393.12 bps |
| Profit-Taking Component (PTC) | $154,320,511 |
| Build notional | $168,191,171 |
| Reversal notional | $154,320,511 |
| Allocated against the build | $154,320,511 |
| Build executions | 5 |
| Reversal delay | 10.6 min |
| Directional purity | 100.0% |
| Product | Credit.CDS |

### TC-RAMP-03 — Ramping — major above threshold

**Result:** PASS

- **Metric:** Ramping
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 36
- **Alerts raised:** 3 (USD Value $478,589 to $617,462, median $540,030, threshold $15,000)

**What is being tested.** That a run of aggressive same-direction trading followed by a reversal is priced on the impact it created, across all four product families.

**Scenario.** One trader buys (or sells) the same instrument five to eight times over roughly half an hour, all in the same direction and with no offsetting flow, then reverses the whole position in two or three clips four to eighteen minutes after the last build trade. Built once per product family. Sized far above the threshold, and should remain an alert even if the threshold is raised several times over.

**Expected.** A Ramping alert on the synthetic instrument, with the reversal matched back to the build phase by the allocation algorithm and USD Value above the Ramping threshold.

**Evidence the alert must show.** EMI as the cumulative modelled impact at the moment of reversal, PTC as the notional reversed out, the build and reversal trades listed separately, and the impact curve showing the push and the exit into it.

**What actually triggered.** 7 sold $592.4m of JPY/USD, reversed after 13.9 minutes

> Renee Dunphy sold JPY/USD 7 times in 27.6 minutes, $592,419,986 in one direction. The Market Impact Model puts the cumulative impact of that build at 12.25 bps of the quoted level — a higher level — worth 12.25 bps in value terms once duration is taken into account. $504,020,530 was then traded back the other way 13.9 minutes later, of which $504,020,530 counts as the Profit-Taking Component. USD Value = 12.25 bps x $504,020,530 = $617,462.

Trader Renee Dunphy, JPY/USD, 2026-07-20, USD Value $617,462.

| Computed value | Amount |
|---|---:|
| Expected Market Impact (EMI) | 12.25 bps |
| Level impact | 12.25 bps |
| Profit-Taking Component (PTC) | $504,020,530 |
| Build notional | $592,419,986 |
| Reversal notional | $504,020,530 |
| Allocated against the build | $504,020,530 |
| Build executions | 7 |
| Reversal delay | 13.9 min |
| Directional purity | 100.0% |
| Product | FX.Spot |

### TC-WASH-01 — Wash trading — barely above threshold

**Result:** PASS

- **Metric:** Wash Trading
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 8
- **Alerts raised:** 3 (USD Value $21,413 to $23,143, median $22,268, threshold $15,000)

**What is being tested.** That a round trip with no genuine risk transfer is caught and priced, in every product family.

**Scenario.** A trader opens a position and reverses it within one to nine minutes at near-identical size, both legs booked against the same internal account. Built once per product family. Sized to land just above the default threshold, so it proves the threshold is doing real work rather than letting everything through.

**Expected.** A Wash Trading alert where the Narrative (manufactured activity) exceeds the Counter-Narrative (genuine market risk run between the legs).

**Evidence the alert must show.** Narrative and Counter-Narrative side by side, the matched notional, the gap between the legs, the price move over that gap, and a relatedness score reflecting the internal counterparty.

**What actually triggered.** $17.2m wash on OAKLN SNRFOR 10Y CDS, offset in 9.0 minutes

> Gordon Pettigrew sold protection on $17,174,948 of OAKLN SNRFOR 10Y CDS and reversed it with 1 offsetting leg totalling $17,361,380 9.0 minutes later. Both legs were booked against Affiliate: Marlowe Securities (IE), so no risk left the firm. The manufactured activity carries a modelled impact of 14.87 bps in value terms, giving a Narrative of $25,540. Against that, the instrument moved only 2.79 bps of value while the position was open, so the genuine market risk run between the legs is worth $2,397. Narrative less Counter-Narrative = $23,143.

Trader Gordon Pettigrew, OAKLN SNRFOR 10Y CDS, 2026-09-15, USD Value $23,143.

| Computed value | Amount |
|---|---:|
| Narrative | $25,540 |
| Counter-Narrative | $2,397 |
| Matched notional | $17,174,948 |
| Modelled impact | 14.87 bps |
| Gap between legs | 9.0 min |
| Value move over the gap | 2.79 bps |
| Relatedness | 100.0% |
| Variant | wash |
| Products involved | Credit.CDS |

### TC-WASH-02 — Wash trading — significant above threshold

**Result:** PASS

- **Metric:** Wash Trading
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 8
- **Alerts raised:** 3 (USD Value $109,864 to $124,856, median $120,232, threshold $15,000)

**What is being tested.** That a round trip with no genuine risk transfer is caught and priced, in every product family.

**Scenario.** A trader opens a position and reverses it within one to nine minutes at near-identical size, both legs booked against the same internal account. Built once per product family. Sized well above the threshold — a clear case an analyst would pick up first.

**Expected.** A Wash Trading alert where the Narrative (manufactured activity) exceeds the Counter-Narrative (genuine market risk run between the legs).

**Evidence the alert must show.** Narrative and Counter-Narrative side by side, the matched notional, the gap between the legs, the price move over that gap, and a relatedness score reflecting the internal counterparty.

**What actually triggered.** $35.1m wash on DNMOR SNRFOR 3Y CDS, offset in 3.2 minutes

> Renee Dunphy bought protection on $35,135,224 of DNMOR SNRFOR 3Y CDS and reversed it with 1 offsetting leg totalling $35,544,239 3.2 minutes later. Both legs were booked against Affiliate: Marlowe Securities (IE), so no risk left the firm. The manufactured activity carries a modelled impact of 35.67 bps in value terms, giving a Narrative of $125,330. Against that, the instrument moved only 0.27 bps of value while the position was open, so the genuine market risk run between the legs is worth $474. Narrative less Counter-Narrative = $124,856.

Trader Renee Dunphy, DNMOR SNRFOR 3Y CDS, 2026-07-30, USD Value $124,856.

| Computed value | Amount |
|---|---:|
| Narrative | $125,330 |
| Counter-Narrative | $474 |
| Matched notional | $35,135,224 |
| Modelled impact | 35.67 bps |
| Gap between legs | 3.2 min |
| Value move over the gap | 0.27 bps |
| Relatedness | 100.0% |
| Variant | wash |
| Products involved | Credit.CDS |

### TC-WASH-03 — Wash trading — major above threshold

**Result:** PASS

- **Metric:** Wash Trading
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 8
- **Alerts raised:** 3 (USD Value $522,015 to $655,819, median $632,941, threshold $15,000)

**What is being tested.** That a round trip with no genuine risk transfer is caught and priced, in every product family.

**Scenario.** A trader opens a position and reverses it within one to nine minutes at near-identical size, both legs booked against the same internal account. Built once per product family. Sized far above the threshold, and should remain an alert even if the threshold is raised several times over.

**Expected.** A Wash Trading alert where the Narrative (manufactured activity) exceeds the Counter-Narrative (genuine market risk run between the legs).

**Evidence the alert must show.** Narrative and Counter-Narrative side by side, the matched notional, the gap between the legs, the price move over that gap, and a relatedness score reflecting the internal counterparty.

**What actually triggered.** $72.2m wash on MXN/USD, offset in 4.1 minutes

> Camille Pettigrew sold $73,106,001 of MXN/USD and reversed it with 1 offsetting leg totalling $72,157,256 4.1 minutes later. Both legs were booked against Affiliate: Kestrel Overseas Ltd, so no risk left the firm. The manufactured activity carries a modelled impact of 90.96 bps in value terms, giving a Narrative of $656,364. Against that, the instrument moved only 0.15 bps of value while the position was open, so the genuine market risk run between the legs is worth $545. Narrative less Counter-Narrative = $655,819.

Trader Camille Pettigrew, MXN/USD, 2026-08-27, USD Value $655,819.

| Computed value | Amount |
|---|---:|
| Narrative | $656,364 |
| Counter-Narrative | $545 |
| Matched notional | $72,157,256 |
| Modelled impact | 90.96 bps |
| Gap between legs | 4.1 min |
| Value move over the gap | 0.15 bps |
| Relatedness | 100.0% |
| Variant | wash |
| Products involved | FX.Spot |

### TC-WASH-04 — Wash trading disguised across tenors

**Result:** PASS

- **Metric:** Wash Trading
- **Products:** credit
- **Documents planted:** 8
- **Alerts raised:** 4 (USD Value $138,406 to $148,086, median $147,134, threshold $15,000)

**What is being tested.** That tenor normalisation defeats the simplest disguise: booking the two legs as different contracts on the same curve point.

**Scenario.** A credit round trip where one leg states a tenor of 58 months and the other 61 months. On any blotter they are two different contracts; both bucket onto the same five-year point.

**Expected.** A Wash Trading alert on the bucketed instrument. The legs only meet after the tenor strings have been normalised onto a standard curve point.

**Evidence the alert must show.** Both legs listed against one instrument, with the matched notional showing they fully offset.

**What actually triggered.** $24.2m wash on CALDW SNRFOR 10Y CDS, offset in 7.5 minutes

> Terrence Ferris bought protection on $24,304,664 of CALDW SNRFOR 10Y CDS and reversed it with 1 offsetting leg totalling $24,206,613 7.5 minutes later. Both legs were booked against Affiliate: Marlowe Securities (IE), so no risk left the firm. The manufactured activity carries a modelled impact of 62.49 bps in value terms, giving a Narrative of $151,263. Against that, the instrument moved only 2.62 bps of value while the position was open, so the genuine market risk run between the legs is worth $3,177. Narrative less Counter-Narrative = $148,086.

Trader Terrence Ferris, CALDW SNRFOR 10Y CDS, 2026-09-15, USD Value $148,086.

| Computed value | Amount |
|---|---:|
| Narrative | $151,263 |
| Counter-Narrative | $3,177 |
| Matched notional | $24,206,613 |
| Modelled impact | 62.49 bps |
| Gap between legs | 7.5 min |
| Value move over the gap | 2.62 bps |
| Relatedness | 100.0% |
| Variant | wash |
| Products involved | Credit.CDS |

### TC-WASH-05 — Wash trading disguised across products

**Result:** PASS

- **Metric:** Wash Trading
- **Products:** rates
- **Documents planted:** 8
- **Alerts raised:** 3 (USD Value $104,784 to $266,925, median $132,788, threshold $15,000)

**What is being tested.** That a round trip split across two different product types on the same curve point is still seen as one offset.

**Scenario.** A swaption in one direction and a cap in the other, same curve point, same size, minutes apart, both against an internal account. Neither product's own blotter shows a round trip.

**Expected.** A Wash Trading alert naming both product types, found with the default 'require same product type' setting off.

**Evidence the alert must show.** A 'cross-product wash' variant label and the two product types listed in the components.

**What actually triggered.** $779.9m cross-product wash on GBP SONIA 1Y, offset in 2.7 minutes

> Stanley Halstead received fixed on $779,921,108 of GBP SONIA 1Y and reversed it with 1 offsetting leg totalling $784,721,427 2.7 minutes later. The legs were booked as different products (IR.Swaption and IR.CapFloor), but both land on GBP SONIA 1Y, where they cancel. The manufactured activity carries a modelled impact of 3.42 bps in value terms, giving a Narrative of $267,076. Against that, the instrument moved only 0.00 bps of value while the position was open, so the genuine market risk run between the legs is worth $151. Narrative less Counter-Narrative = $266,925.

Trader Stanley Halstead, GBP SONIA 1Y, 2026-07-21, USD Value $266,925.

| Computed value | Amount |
|---|---:|
| Narrative | $267,076 |
| Counter-Narrative | $151 |
| Matched notional | $779,921,108 |
| Modelled impact | 3.42 bps |
| Gap between legs | 2.7 min |
| Value move over the gap | 0.00 bps |
| Relatedness | 100.0% |
| Variant | cross-product wash |
| Products involved | IR.Swaption, IR.CapFloor |

### TC-WASH-06 — Wash trading spread across related accounts

**Result:** PASS

- **Metric:** Wash Trading
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 12
- **Alerts raised:** 4 (USD Value $133,469 to $243,605, median $148,174, threshold $15,000)

**What is being tested.** That an offset broken into pieces across several related accounts is still assembled into one round trip.

**Scenario.** An opening leg, then two half-size opposite legs booked to two different internal accounts within minutes. No single account shows a complete round trip.

**Expected.** A Wash Trading alert that accumulates the offsetting legs until they cover the anchor, labelled as a multi-counterparty wash.

**Evidence the alert must show.** All three legs listed, the counterparties named, and the matched notional.

**What actually triggered.** $1.04bn multi-counterparty wash on GBP FRA 3M, offset in 10.8 minutes

> Tessa Prescott paid fixed on $1,043,445,352 of GBP FRA 3M and reversed it with 2 offsetting legs totalling $1,054,028,653 10.8 minutes later. The offset was spread across 3 counterparties (INT-001, INT-003, INT-002), which disguises the round trip on any single account. The manufactured activity carries a modelled impact of 2.42 bps in value terms, giving a Narrative of $252,338. Against that, the instrument moved only 0.17 bps of value while the position was open, so the genuine market risk run between the legs is worth $8,732. Narrative less Counter-Narrative = $243,605.

Trader Tessa Prescott, GBP FRA 3M, 2026-08-31, USD Value $243,605.

| Computed value | Amount |
|---|---:|
| Narrative | $252,338 |
| Counter-Narrative | $8,732 |
| Matched notional | $1,043,445,352 |
| Modelled impact | 2.42 bps |
| Gap between legs | 10.8 min |
| Value move over the gap | 0.17 bps |
| Relatedness | 100.0% |
| Variant | multi-counterparty wash |
| Products involved | IR.CapFloor |

### TC-WASH-07 — Wash trading disguised across currency pairs

**Result:** PASS

- **Metric:** Wash Trading
- **Products:** fx
- **Documents planted:** 8
- **Alerts raised:** 4 (USD Value $139,149 to $144,805, median $142,555, threshold $15,000)

**What is being tested.** That splitting an FX round trip across two different pairs does not hide it, because both legs map onto the same synthetic instrument.

**Scenario.** A currency is bought against one cross and sold against another within minutes, same size, both against an internal account.

**Expected.** A Wash Trading alert on the CCY/USD instrument, labelled as a cross-currency wash.

**Evidence the alert must show.** The two different headline pairs named, both resolving to one instrument.

**What actually triggered.** $195.0m cross-currency wash on JPY/USD, offset in 7.4 minutes

> Noel Bramwell bought $195,534,347 of JPY/USD and reversed it with 1 offsetting leg totalling $195,012,126 7.4 minutes later. The legs were booked in different currency pairs (EUR/JPY and GBP/JPY), but both map onto JPY/USD once split against USD. The manufactured activity carries a modelled impact of 7.51 bps in value terms, giving a Narrative of $146,489. Against that, the instrument moved only 0.17 bps of value while the position was open, so the genuine market risk run between the legs is worth $1,684. Narrative less Counter-Narrative = $144,805.

Trader Noel Bramwell, JPY/USD, 2026-08-05, USD Value $144,805.

| Computed value | Amount |
|---|---:|
| Narrative | $146,489 |
| Counter-Narrative | $1,684 |
| Matched notional | $195,012,126 |
| Modelled impact | 7.51 bps |
| Gap between legs | 7.4 min |
| Value move over the gap | 0.17 bps |
| Relatedness | 100.0% |
| Variant | cross-currency wash |
| Products involved | FX.Spot |

### TC-FRONT-01 — Front running — barely above threshold

**Result:** PASS

- **Metric:** Front Running
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 20
- **Alerts raised:** 3 (USD Value $27,279 to $30,253, median $29,472, threshold $20,000)
- **Also flagged by:** Ramping

**What is being tested.** That a proprietary position opened ahead of the firm's own client flow, and exited into the impact of that flow, is detected and priced.

**Scenario.** A trader opens a position on their own book, then fills two to four client orders in the same instrument and direction, then closes the position one and a half to five minutes after the last client fill. Built once per product family. Sized to land just above the default threshold, so it proves the threshold is doing real work rather than letting everything through.

**Expected.** A Front Running alert where the residual impact of the client flow at the moment of the exit drives EMI, and the exit notional drives PTC.

**Evidence the alert must show.** The position opened before the client flow, the client fills themselves, the exit, the lead time, and the portion of the exit the allocation algorithm ties back to the opening trades.

**What actually triggered.** $2.7m position exited 1.5 minutes after $2.8m of client business in DNMOR SNRFOR 10Y CDS

> Frances Alderman bought protection on $2,933,464 of DNMOR SNRFOR 10Y CDS, then filled $2,844,052 of client business in the same instrument and direction. The residual impact of that flow was 111.35 bps in value terms when the position was closed 1.5 minutes later. Applied to the $2,716,869 that exited into it, USD Value = $30,253.

Trader Frances Alderman, DNMOR SNRFOR 10Y CDS, 2026-08-17, USD Value $30,253.

| Computed value | Amount |
|---|---:|
| Expected Market Impact (EMI) | 111.35 bps |
| Level impact | 267.63 bps |
| Profit-Taking Component (PTC) | $2,716,869 |
| Impacting notional | $2,844,052 |
| Position opened | $2,933,464 |
| Exit notional | $2,716,869 |
| Allocated as position closing | $2,716,869 |
| Lead time | 90.0 s |
| Product | Credit.CDS |

### TC-FRONT-02 — Front running — significant above threshold

**Result:** PASS

- **Metric:** Front Running
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 21
- **Alerts raised:** 3 (USD Value $143,880 to $160,749, median $157,636, threshold $20,000)
- **Also flagged by:** Ramping, Wash Trading

**What is being tested.** That a proprietary position opened ahead of the firm's own client flow, and exited into the impact of that flow, is detected and priced.

**Scenario.** A trader opens a position on their own book, then fills two to four client orders in the same instrument and direction, then closes the position one and a half to five minutes after the last client fill. Built once per product family. Sized well above the threshold — a clear case an analyst would pick up first.

**Expected.** A Front Running alert where the residual impact of the client flow at the moment of the exit drives EMI, and the exit notional drives PTC.

**Evidence the alert must show.** The position opened before the client flow, the client fills themselves, the exit, the lead time, and the portion of the exit the allocation algorithm ties back to the opening trades.

**What actually triggered.** $22.6m position exited 34.0 seconds after $22.9m of client business in ZAR/USD

> Frances Alderman bought $22,595,754 of ZAR/USD, then filled $22,874,835 of client business in the same instrument and direction. The residual impact of that flow was 71.14 bps in value terms when the position was closed 34.0 seconds later. Applied to the $22,595,754 that exited into it, USD Value = $160,749.

Trader Frances Alderman, ZAR/USD, 2026-10-01, USD Value $160,749.

| Computed value | Amount |
|---|---:|
| Expected Market Impact (EMI) | 71.14 bps |
| Level impact | 71.14 bps |
| Profit-Taking Component (PTC) | $22,595,754 |
| Impacting notional | $22,874,835 |
| Position opened | $22,595,754 |
| Exit notional | $23,898,780 |
| Allocated as position closing | $23,898,780 |
| Lead time | 34.0 s |
| Product | FX.Spot |

### TC-FRONT-03 — Front running — major above threshold

**Result:** PASS

- **Metric:** Front Running
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 22
- **Alerts raised:** 3 (USD Value $428,945 to $781,970, median $727,306, threshold $20,000)
- **Also flagged by:** Ramping, Wash Trading

**What is being tested.** That a proprietary position opened ahead of the firm's own client flow, and exited into the impact of that flow, is detected and priced.

**Scenario.** A trader opens a position on their own book, then fills two to four client orders in the same instrument and direction, then closes the position one and a half to five minutes after the last client fill. Built once per product family. Sized far above the threshold, and should remain an alert even if the threshold is raised several times over.

**Expected.** A Front Running alert where the residual impact of the client flow at the moment of the exit drives EMI, and the exit notional drives PTC.

**Evidence the alert must show.** The position opened before the client flow, the client fills themselves, the exit, the lead time, and the portion of the exit the allocation algorithm ties back to the opening trades.

**What actually triggered.** $68.3m position exited 2.8 minutes after $72.6m of client business in MXN/USD

> Bridget Carrow bought $69,795,117 of MXN/USD, then filled $72,619,491 of client business in the same instrument and direction. The residual impact of that flow was 114.51 bps in value terms when the position was closed 2.8 minutes later. Applied to the $68,289,809 that exited into it, USD Value = $781,970.

Trader Bridget Carrow, MXN/USD, 2026-10-07, USD Value $781,970.

| Computed value | Amount |
|---|---:|
| Expected Market Impact (EMI) | 114.51 bps |
| Level impact | 114.51 bps |
| Profit-Taking Component (PTC) | $68,289,809 |
| Impacting notional | $72,619,491 |
| Position opened | $69,795,117 |
| Exit notional | $68,289,809 |
| Allocated as position closing | $68,289,809 |
| Lead time | 2.8 min |
| Product | FX.Spot |

### TC-FRONT-04 — Internal front running across books

**Result:** PASS

- **Metric:** Internal Front Running
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 41
- **Alerts raised:** 6 (USD Value $111,971 to $273,530, median $155,747, threshold $25,000)
- **Also flagged by:** Wash Trading

**What is being tested.** That the same mechanic is caught when the impact comes from a different book inside the firm — information leaking across an internal wall.

**Scenario.** One trader opens a position; a different trader then pushes large client flow through the same instrument in the same direction; the first trader exits into it.

**Expected.** An Internal Front Running alert attributing the impact to the other trader's flow.

**Evidence the alert must show.** The impacting traders named, with the lead time and the impacting notional.

**What actually triggered.** $105.4m exited 2.4 minutes after $110.1m of flow from Ulysses Lockhart

> Sheila Stroud bought $105,443,092 of XS2367195583, then Ulysses Lockhart pushed $110,120,793 through the same instrument in the same direction. The residual impact of that flow was 25.94 bps in value terms when the position was closed 2.4 minutes later. Applied to the $105,443,092 that exited into it, USD Value = $273,530.

Trader Sheila Stroud, DNMOR 8.0% 2031, 2026-09-01, USD Value $273,530.

| Computed value | Amount |
|---|---:|
| Expected Market Impact (EMI) | 25.94 bps |
| Level impact | 25.94 bps |
| Profit-Taking Component (PTC) | $105,443,092 |
| Impacting notional | $110,120,793 |
| Position opened | $105,443,092 |
| Exit notional | $111,018,366 |
| Allocated as position closing | $105,443,092 |
| Lead time | 2.4 min |
| Product | FixedIncome.Bond |
| Impacting traders | Ulysses Lockhart |

### TC-BESTEX-01 — Best execution — barely above threshold

**Result:** PASS

- **Metric:** Best Execution
- **Products:** fenics, fx
- **Documents planted:** 10
- **Alerts raised:** 4 (USD Value $17,781 to $18,000, median $18,000, threshold $15,000)
- **Also flagged by:** Off-Market Trading

**What is being tested.** That a client filled worse than the firm could demonstrably have achieved is identified and the detriment quantified.

**Scenario.** The firm prints a bond with a dealer at the prevailing level, then fills a client on the same ISIN seconds to minutes later at a materially worse price — worse for the client on the side they traded. An FX variant prices a client away from the market level. Sized to land just above the default threshold, so it proves the threshold is doing real work rather than letting everything through.

**Expected.** A Best Execution alert measuring the client fill against the better contemporaneous print, with USD Value equal to the detriment times the client notional.

**Evidence the alert must show.** Client price, reference price, which print the reference came from and how long before, and the gap in basis points and dollars.

**What actually triggered.** Ardent Insurance Group filled $4.1m of US91282CJL60 43.4 bps worse than the firm's own print

> Ardent Insurance Group was filled $4,148,792 of US91282CJL60 at 102.9095, having bought from the firm. Measured against the firm's own print on the same instrument of 102.4649, printed 46.0 seconds earlier with Wychwood Bank, the client was 43.4 bps worse off — 43.4 bps in value terms. Applied to the client notional, $18,000 of value moved away from the client relative to the terms the firm could demonstrably have achieved.

Trader Roland Blanchard, UST 4.0% 2034, 2026-07-30, USD Value $18,000.

| Computed value | Amount |
|---|---:|
| Client detriment (EMI) | 43.39 bps |
| Level difference | 43.39 bps |
| Client notional (PTC) | $4,148,792 |
| Client price | 102.909 |
| Reference price | 102.465 |
| Reference basis | Firm's own contemporaneous print |
| Gap to reference print | 46.0 s |
| Counterparty | Ardent Insurance Group (client) |
| Venue | Fenics UST |

### TC-BESTEX-02 — Best execution — significant above threshold

**Result:** PASS

- **Metric:** Best Execution
- **Products:** fenics, fx
- **Documents planted:** 10
- **Alerts raised:** 5 (USD Value $88,628 to $90,001, median $90,000, threshold $15,000)
- **Also flagged by:** Off-Market Trading

**What is being tested.** That a client filled worse than the firm could demonstrably have achieved is identified and the detriment quantified.

**Scenario.** The firm prints a bond with a dealer at the prevailing level, then fills a client on the same ISIN seconds to minutes later at a materially worse price — worse for the client on the side they traded. An FX variant prices a client away from the market level. Sized well above the threshold — a clear case an analyst would pick up first.

**Expected.** A Best Execution alert measuring the client fill against the better contemporaneous print, with USD Value equal to the detriment times the client notional.

**Evidence the alert must show.** Client price, reference price, which print the reference came from and how long before, and the gap in basis points and dollars.

**What actually triggered.** Tri-State Freight Systems filled $17.8m of XS2289417730 50.6 bps worse than the firm's own print

> Tri-State Freight Systems was filled $17,775,499 of XS2289417730 at 98.1937, having bought from the firm. Measured against the firm's own print on the same instrument of 97.6990, printed 32.8 seconds earlier with Fifth Harbor Securities, the client was 50.6 bps worse off — 50.6 bps in value terms. Applied to the client notional, $90,001 of value moved away from the client relative to the terms the firm could demonstrably have achieved.

Trader Tessa Thorne, GRNTP 2.75% 2028, 2026-09-03, USD Value $90,001.

| Computed value | Amount |
|---|---:|
| Client detriment (EMI) | 50.63 bps |
| Level difference | 50.63 bps |
| Client notional (PTC) | $17,775,499 |
| Client price | 98.194 |
| Reference price | 97.699 |
| Reference basis | Firm's own contemporaneous print |
| Gap to reference print | 32.8 s |
| Counterparty | Tri-State Freight Systems (client) |
| Venue | Fenics UST |

### TC-BESTEX-03 — Best execution — major above threshold

**Result:** PASS

- **Metric:** Best Execution
- **Products:** fenics, fx
- **Documents planted:** 10
- **Alerts raised:** 4 (USD Value $480,000 to $485,675, median $480,000, threshold $15,000)
- **Also flagged by:** Off-Market Trading

**What is being tested.** That a client filled worse than the firm could demonstrably have achieved is identified and the detriment quantified.

**Scenario.** The firm prints a bond with a dealer at the prevailing level, then fills a client on the same ISIN seconds to minutes later at a materially worse price — worse for the client on the side they traded. An FX variant prices a client away from the market level. Sized far above the threshold, and should remain an alert even if the threshold is raised several times over.

**Expected.** A Best Execution alert measuring the client fill against the better contemporaneous print, with USD Value equal to the detriment times the client notional.

**Evidence the alert must show.** Client price, reference price, which print the reference came from and how long before, and the gap in basis points and dollars.

**What actually triggered.** Oakline Semiconductor filled $41.6m of CAD/USD 116.9 bps worse than the market level

> Oakline Semiconductor was filled $41,562,758 of CAD/USD at 0.708351, having sold to the firm. Measured against the modelled market level of 0.716726, the client was 116.9 bps worse off — 116.9 bps in value terms. Applied to the client notional, $485,675 of value moved away from the client relative to the terms the firm could demonstrably have achieved.

Trader Laurel Bramwell, CAD/USD, 2026-09-07, USD Value $485,675.

| Computed value | Amount |
|---|---:|
| Client detriment (EMI) | 116.85 bps |
| Level difference | 116.85 bps |
| Client notional (PTC) | $41,562,758 |
| Client price | 0.70835 |
| Reference price | 0.71673 |
| Reference basis | Modelled market level |
| Counterparty | Oakline Semiconductor (client) |
| Venue | not recorded |

### TC-SPOOF-01 — Spoofing — barely above threshold

**Result:** PASS

- **Metric:** Spoofing
- **Products:** fenics
- **Documents planted:** 10
- **Alerts raised:** 5 (USD Value $19,989 to $23,931, median $22,233, threshold $12,000)

**What is being tested.** That a large visible order placed with no intention of filling, pulled quickly, while the trader executes the other way, is detected from order data.

**Scenario.** A single fully displayed order is placed at the touch and cancelled within three to twenty-five seconds without filling. While it rests, the same trader executes a third to a half of that size in the opposite direction. Sized to land just above the default threshold, so it proves the threshold is doing real work rather than letting everything through.

**Expected.** A Spoofing alert where the Narrative (competitiveness- and visibility-weighted order impact applied to the opposite-side execution) exceeds the Counter-Narrative (cost of being filled, scaled by how briefly it rested).

**Evidence the alert must show.** Order notional, displayed share, competitiveness, order life, the opposite-side execution, and nil fills on the decoy.

**What actually triggered.** $28.0m bid on XS2671104492 pulled after 6.3 seconds, $13.0m executed the other way

> Walter Castellano placed a single bid for $27,978,660 of XS2671104492 at 98.6098, with 100% of it displayed to the market — competitive enough to shift the visible balance of the book. It was cancelled after 6.3 seconds without filling. While it rested, the same trader executed $13,037,216 in the opposite direction on the same instrument. The Narrative — order impact of 18.81 bps applied to that execution — is worth $24,524. The Counter-Narrative, the cost of unwinding had the order been filled, is $594 once scaled by how briefly it was exposed. Narrative less Counter-Narrative = $23,931.

Trader Walter Castellano, XS2671104492, 2026-10-06, USD Value $23,931.

| Computed value | Amount |
|---|---:|
| Narrative | $24,524 |
| Counter-Narrative | $594 |
| Order notional | $27,978,660 |
| Displayed share | 100.0% |
| Competitiveness | 97.5% |
| Order life | 6.3 s |
| Executed the other way | $13,037,216 |
| Filled | 0 |
| Side | bid (positive amount) |

### TC-SPOOF-02 — Spoofing — significant above threshold

**Result:** PASS

- **Metric:** Spoofing
- **Products:** fenics
- **Documents planted:** 10
- **Alerts raised:** 5 (USD Value $77,221 to $132,329, median $101,632, threshold $12,000)

**What is being tested.** That a large visible order placed with no intention of filling, pulled quickly, while the trader executes the other way, is detected from order data.

**Scenario.** A single fully displayed order is placed at the touch and cancelled within three to twenty-five seconds without filling. While it rests, the same trader executes a third to a half of that size in the opposite direction. Sized well above the threshold — a clear case an analyst would pick up first.

**Expected.** A Spoofing alert where the Narrative (competitiveness- and visibility-weighted order impact applied to the opposite-side execution) exceeds the Counter-Narrative (cost of being filled, scaled by how briefly it rested).

**Evidence the alert must show.** Order notional, displayed share, competitiveness, order life, the opposite-side execution, and nil fills on the decoy.

**What actually triggered.** $185.3m bid on XS2610447215 pulled after 6.6 seconds, $95.3m executed the other way

> Ulysses Whitfield placed a single bid for $185,290,767 of XS2610447215 at 104.3136, with 100% of it displayed to the market — competitive enough to shift the visible balance of the book. It was cancelled after 6.6 seconds without filling. While it rested, the same trader executed $95,265,094 in the opposite direction on the same instrument. The Narrative — order impact of 14.22 bps applied to that execution — is worth $135,491. The Counter-Narrative, the cost of unwinding had the order been filled, is $3,162 once scaled by how briefly it was exposed. Narrative less Counter-Narrative = $132,329.

Trader Ulysses Whitfield, XS2610447215, 2026-08-27, USD Value $132,329.

| Computed value | Amount |
|---|---:|
| Narrative | $135,491 |
| Counter-Narrative | $3,162 |
| Order notional | $185,290,767 |
| Displayed share | 100.0% |
| Competitiveness | 97.6% |
| Order life | 6.6 s |
| Executed the other way | $95,265,094 |
| Filled | 0 |
| Side | bid (positive amount) |

### TC-SPOOF-03 — Spoofing — major above threshold

**Result:** PASS

- **Metric:** Spoofing
- **Products:** fenics
- **Documents planted:** 10
- **Alerts raised:** 5 (USD Value $450,660 to $657,982, median $542,408, threshold $12,000)

**What is being tested.** That a large visible order placed with no intention of filling, pulled quickly, while the trader executes the other way, is detected from order data.

**Scenario.** A single fully displayed order is placed at the touch and cancelled within three to twenty-five seconds without filling. While it rests, the same trader executes a third to a half of that size in the opposite direction. Sized far above the threshold, and should remain an alert even if the threshold is raised several times over.

**Expected.** A Spoofing alert where the Narrative (competitiveness- and visibility-weighted order impact applied to the opposite-side execution) exceeds the Counter-Narrative (cost of being filled, scaled by how briefly it rested).

**Evidence the alert must show.** Order notional, displayed share, competitiveness, order life, the opposite-side execution, and nil fills on the decoy.

**What actually triggered.** $315.4m bid on GB00BTHH2R79 pulled after 16.2 seconds, $156.7m executed the other way

> Laurel Sandoval placed a single bid for $315,424,633 of GB00BTHH2R79 at 101.7193, with 100% of it displayed to the market — competitive enough to shift the visible balance of the book. It was cancelled after 16.2 seconds without filling. While it rested, the same trader executed $156,665,046 in the opposite direction on the same instrument. The Narrative — order impact of 44.53 bps applied to that execution — is worth $697,640. The Counter-Narrative, the cost of unwinding had the order been filled, is $39,658 once scaled by how briefly it was exposed. Narrative less Counter-Narrative = $657,982.

Trader Laurel Sandoval, GB00BTHH2R79, 2026-09-23, USD Value $657,982.

| Computed value | Amount |
|---|---:|
| Narrative | $697,640 |
| Counter-Narrative | $39,658 |
| Order notional | $315,424,633 |
| Displayed share | 100.0% |
| Competitiveness | 97.5% |
| Order life | 16.2 s |
| Executed the other way | $156,665,046 |
| Filled | 0 |
| Side | bid (positive amount) |

### TC-LAYER-01 — Layering — barely above threshold

**Result:** PASS

- **Metric:** Layering
- **Products:** fenics
- **Documents planted:** 34
- **Alerts raised:** 4 (USD Value $12,159 to $17,455, median $15,838, threshold $12,000)

**What is being tested.** That a ladder of orders across several price levels — as distinct from one conspicuous order — is detected and scored separately from spoofing.

**Scenario.** Four to seven orders are placed on one side at successive price levels within seconds of each other, some amended to walk the price in, all cancelled within eight to fifty seconds without filling, with a genuine execution on the opposite side. Sized to land just above the default threshold, so it proves the threshold is doing real work rather than letting everything through.

**Expected.** A Layering alert requiring at least four distinct price levels, a high cancel ratio and a short average life, with the ladder's apparent depth weighted into the score.

**Evidence the alert must show.** Number of orders and distinct price levels, cancel ratio, amendments, average order life, and the opposite-side execution.

**What actually triggered.** 5-order bid ladder across 7 levels on XS2417295478, $52.2m executed the other way

> Frances Reyes placed 5 bids on XS2417295478 across 7 distinct price levels, $126,426,769 in total, with 2 amendments walking the price in. 100% of that notional was cancelled without filling, after an average life of 30.0 seconds. While the ladder stood, the same trader executed $52,166,196 in the opposite direction. The Narrative — ladder impact of 4.01 bps, weighted for the depth it appeared to add, applied to that execution — is worth $20,930 against a Counter-Narrative of $3,475 for the risk of being filled. Narrative less Counter-Narrative = $17,455.

Trader Frances Reyes, XS2417295478, 2026-09-22, USD Value $17,455.

| Computed value | Amount |
|---|---:|
| Narrative | $20,930 |
| Counter-Narrative | $3,475 |
| Price levels | 7 |
| Orders in ladder | 5 |
| Ladder notional | $126,426,769 |
| Cancel ratio | 100.0% |
| Amendments | 2 |
| Average order life | 30.0 s |
| Competitiveness | 62.1% |
| Executed the other way | $52,166,196 |

### TC-LAYER-02 — Layering — significant above threshold

**Result:** PASS

- **Metric:** Layering
- **Products:** fenics
- **Documents planted:** 32
- **Alerts raised:** 5 (USD Value $41,875 to $93,829, median $79,603, threshold $12,000)
- **Also flagged by:** Spoofing

**What is being tested.** That a ladder of orders across several price levels — as distinct from one conspicuous order — is detected and scored separately from spoofing.

**Scenario.** Four to seven orders are placed on one side at successive price levels within seconds of each other, some amended to walk the price in, all cancelled within eight to fifty seconds without filling, with a genuine execution on the opposite side. Sized well above the threshold — a clear case an analyst would pick up first.

**Expected.** A Layering alert requiring at least four distinct price levels, a high cancel ratio and a short average life, with the ladder's apparent depth weighted into the score.

**Evidence the alert must show.** Number of orders and distinct price levels, cancel ratio, amendments, average order life, and the opposite-side execution.

**What actually triggered.** 4-order bid ladder across 6 levels on XS2610447215, $75.0m executed the other way

> Terrence Ferris placed 4 bids on XS2610447215 across 6 distinct price levels, $151,525,072 in total, with 2 amendments walking the price in. 100% of that notional was cancelled without filling, after an average life of 33.5 seconds. While the ladder stood, the same trader executed $74,989,558 in the opposite direction. The Narrative — ladder impact of 14.39 bps, weighted for the depth it appeared to add, applied to that execution — is worth $107,882 against a Counter-Narrative of $14,053 for the risk of being filled. Narrative less Counter-Narrative = $93,829.

Trader Terrence Ferris, XS2610447215, 2026-08-13, USD Value $93,829.

| Computed value | Amount |
|---|---:|
| Narrative | $107,882 |
| Counter-Narrative | $14,053 |
| Price levels | 6 |
| Orders in ladder | 4 |
| Ladder notional | $151,525,072 |
| Cancel ratio | 100.0% |
| Amendments | 2 |
| Average order life | 33.5 s |
| Competitiveness | 69.7% |
| Executed the other way | $74,989,558 |

### TC-LAYER-03 — Layering — major above threshold

**Result:** PASS

- **Metric:** Layering
- **Products:** fenics
- **Documents planted:** 34
- **Alerts raised:** 5 (USD Value $230,943 to $447,781, median $325,778, threshold $12,000)
- **Also flagged by:** Spoofing

**What is being tested.** That a ladder of orders across several price levels — as distinct from one conspicuous order — is detected and scored separately from spoofing.

**Scenario.** Four to seven orders are placed on one side at successive price levels within seconds of each other, some amended to walk the price in, all cancelled within eight to fifty seconds without filling, with a genuine execution on the opposite side. Sized far above the threshold, and should remain an alert even if the threshold is raised several times over.

**Expected.** A Layering alert requiring at least four distinct price levels, a high cancel ratio and a short average life, with the ladder's apparent depth weighted into the score.

**Evidence the alert must show.** Number of orders and distinct price levels, cancel ratio, amendments, average order life, and the opposite-side execution.

**What actually triggered.** 4-order bid ladder across 7 levels on XS2803561194, $132.1m executed the other way

> Terrence Ferris placed 4 bids on XS2803561194 across 7 distinct price levels, $379,870,996 in total, with 3 amendments walking the price in. 100% of that notional was cancelled without filling, after an average life of 20.7 seconds. While the ladder stood, the same trader executed $132,071,712 in the opposite direction. The Narrative — ladder impact of 37.71 bps, weighted for the depth it appeared to add, applied to that execution — is worth $497,988 against a Counter-Narrative of $50,206 for the risk of being filled. Narrative less Counter-Narrative = $447,781.

Trader Terrence Ferris, XS2803561194, 2026-08-18, USD Value $447,781.

| Computed value | Amount |
|---|---:|
| Narrative | $497,988 |
| Counter-Narrative | $50,206 |
| Price levels | 7 |
| Orders in ladder | 4 |
| Ladder notional | $379,870,996 |
| Cancel ratio | 100.0% |
| Amendments | 3 |
| Average order life | 20.7 s |
| Competitiveness | 70.3% |
| Executed the other way | $132,071,712 |

### TC-INSIDER-01 — Position built ahead of a non-public event

**Result:** PASS

- **Metric:** Insider Trading
- **Products:** credit, rates, fenics, fx
- **Documents planted:** 30
- **Alerts raised:** 3 (USD Value $223,465 to $306,031, median $228,204, threshold $50,000)

**What is being tested.** That a position concentrated in the hours before an announcement is separated from ordinary book-running, and valued at the move it captured.

**Scenario.** A trader builds a position in the direction of a coming event across three to five trades in the one and a half to nine hours before it becomes public, then closes within an hour or so of the news.

**Expected.** An Insider Trading alert where the excess over the time-weighted expected position, valued at the realised move in value terms, clears the threshold.

**Evidence the alert must show.** Observed versus expected position, the excess, the event and its move, the time from the last build trade to the announcement, and the closing trade.

**What actually triggered.** $13.1m short TRIST SNRFOR 10Y CDS into "TRIST refinancing completed" (-3.651%)

> Walter Draper traded TRIST SNRFOR 10Y CDS 4 times in the 48-hour window before "TRIST refinancing completed" became public, ending $17,564,050 short. Weighting each execution by how long before the announcement it was placed, a fair trader would have been expected to hold $4,469,505, leaving an excess of $13,094,545 concentrated in the hours immediately before the news. The event moved the quoted level by -3.651%, worth -233.7 bps of value, so the excess position was worth $306,031 the moment the announcement landed. The position was closed 32.0 minutes after the news broke.

Trader Walter Draper, TRIST SNRFOR 10Y CDS, 2026-08-07, USD Value $306,031.

| Computed value | Amount |
|---|---:|
| Observed position | $-17,564,050 |
| Expected position | $-4,469,505 |
| Excess position | $-13,094,545 |
| Event level move | -3.65% |
| Value of the move | -233.71 bps |
| Event | TRIST refinancing completed (scheduled) |
| Time to announcement | 6.7 h |
| Product | Credit.CDS |

### TC-CLOSE-01 — Trading concentrated into the closing window

**Result:** PASS

- **Metric:** Banging the Close
- **Products:** fenics, fx
- **Documents planted:** 25
- **Alerts raised:** 4 (USD Value $152,951 to $425,789, median $304,926, threshold $20,000)

**What is being tested.** That volume deliberately concentrated before a closing level is distinguished from the same volume worked evenly through the session.

**Scenario.** A trader takes exposure in the morning that settles against the close, then pushes four to seven clips through the final fifteen minutes before 16:00 in the direction that helps it.

**Expected.** A Banging the Close alert where the impact standing at the close exceeds what evenly-spread trading would have left.

**Evidence the alert must show.** Actual versus modelled fair closing impact, the window notional, the concentration ratio, and the exposure carried into the close.

**What actually triggered.** $248.8m pushed through the last 15 minutes before the CHF/USD close

> Renee Ferris executed 4 times for $248,755,210 in the 15 minutes before the 16:00 close — 49% of their whole day in CHF/USD, and all of it pushing toward a lower level. Concentrated like that, the Market Impact Model leaves 18.88 bps of level impact standing at the close; the same notional worked evenly across the session would have left only 1.77 bps, because most of it would have decayed. The 17.12 bps difference is the modelled displacement, worth 17.12 bps in value terms, applied to the $248,755,210 of exposure carried into the close.

Trader Renee Ferris, CHF/USD, 2026-09-22, USD Value $425,789.

| Computed value | Amount |
|---|---:|
| Expected Market Impact (EMI) | 17.12 bps |
| Level displacement | 17.12 bps |
| Profit-Taking Component (PTC) | $248,755,210 |
| Actual closing impact | 18.88 bps |
| Modelled fair impact | 1.77 bps |
| Window notional | $248,755,210 |
| Concentration | 48.6% |
| Executions in window | 4 |

### TC-CROSS-01 — Two books crossing at an off-market level

**Result:** PASS

- **Metric:** Cross Trading
- **Products:** fenics, fx
- **Documents planted:** 8
- **Alerts raised:** 3 (USD Value $89,389 to $90,027, median $89,962, threshold $15,000)
- **Also flagged by:** Off-Market Trading

**What is being tested.** That risk moved between two internal books at a level away from the market is caught and the transferred value quantified.

**Scenario.** Two traders take opposite sides of the same instrument within a few seconds at matched size, both booked to internal accounts, at a level 40 to 200 basis points away from the market.

**Expected.** A Cross Trading alert pricing the deviation from the market level against the crossed notional.

**Evidence the alert must show.** Both sides, the agreed level, the market level, the timestamp gap and the two books.

**What actually triggered.** $5.0m of XS2803561194 crossed between Renee Dunphy and Adele Lockhart at 179.3 bps off market

> Renee Dunphy and Adele Lockhart took opposite sides of XS2803561194 within 8.7 seconds of each other, for $5,019,989 and $5,045,202. Both sides booked against an internal account, so the risk moved between books rather than to the street. The agreed level of 101.2743 sits 179.3 bps away from the modelled market level of 99.4901 — 179.3 bps in value terms. Applied to the $5,019,989 crossed, $90,027 of value moved from one book to the other.

Trader Renee Dunphy, CALDW 4.5% 2033, 2026-07-20, USD Value $90,027.

| Computed value | Amount |
|---|---:|
| Expected Market Impact (EMI) | 179.34 bps |
| Level deviation | 179.34 bps |
| Profit-Taking Component (PTC) | $5,019,989 |
| Timestamp gap | 8.7 s |
| Crossed level | 101.274 |
| Market level | 99.490 |
| Books | FI-CORP-01 / FI-CORP-01 |
| Product | FixedIncome.Bond / FixedIncome.Bond |

### TC-OFFMKT-01 — Execution booked away from the market

**Result:** PASS

- **Metric:** Off-Market Trading
- **Products:** fenics, fx
- **Documents planted:** 6
- **Alerts raised:** 6 (USD Value $60,001 to $180,120, median $135,462, threshold $10,000)
- **Also flagged by:** Best Execution

**What is being tested.** That a single execution priced well away from the prevailing level is flagged and the value transfer measured.

**Scenario.** One execution is booked 120 to 500 basis points from the modelled market level, against a client or an internal account.

**Expected.** An Off-Market Trading alert with USD Value equal to the deviation times the notional, and a statement of which side the transfer favours.

**Evidence the alert must show.** Executed level, market level, deviation in basis points, and the direction of the transfer.

**What actually triggered.** $3.7m of XS2610447215 booked 484 bps below the market

> Roland Blanchard executed $3,721,522 of XS2610447215 (FixedIncome.Bond) at 96.4534 against a modelled market level of 101.3592 — a deviation of 484 bps, well beyond any plausible spread. In value terms that is 484.0 bps, so $180,120 of value moved toward the counterparty relative to executing at the market. The counterparty was Internal Nominee Account.

Trader Roland Blanchard, WYCHW 3.25% 2027, 2026-09-17, USD Value $180,120.

| Computed value | Amount |
|---|---:|
| Expected Market Impact (EMI) | 483.99 bps |
| Level deviation | 483.99 bps |
| Profit-Taking Component (PTC) | $3,721,522 |
| Executed level | 96.453 |
| Market level | 101.359 |
| Direction of transfer | Toward the counterparty |
| Product | FixedIncome.Bond |
| Counterparty | Internal Nominee Account (internal) |

## Severity tiers

Where a behaviour is planted at three sizes, the scores must come out in order. This is what makes the threshold meaningful rather than decorative.

| Metric | Barely | Significant | Major | In order |
|---|---:|---:|---:|---|
| Ramping | $20,718 | $116,850 | $540,030 | yes |
| Wash Trading | $22,268 | $120,232 | $632,941 | yes |
| Front Running | $29,472 | $157,636 | $727,306 | yes |
| Best Execution | $18,000 | $90,000 | $480,000 | yes |
| Spoofing | $22,233 | $101,632 | $542,408 | yes |
| Layering | $15,838 | $79,603 | $325,778 | yes |

## Threshold sensitivity

Alert counts as each metric's threshold is moved, with the data held constant.

| Metric | $5,000 | $15,000 | $50,000 | $250,000 | $1,000,000 |
|---|---:|---:|---:|---:|---:|
| Ramping | 91 | 61 | 43 | 21 | 5 |
| Wash Trading | 39 | 37 | 32 | 6 | 0 |
| Front Running | 11 | 10 | 6 | 3 | 0 |
| Spoofing | 40 | 31 | 12 | 5 | 0 |
| Layering | 15 | 12 | 9 | 4 | 0 |
| Best Execution | 16 | 16 | 12 | 4 | 0 |

## Alerts from ordinary business

Alerts containing none of the planted documents. These come from the background population — activity that is structurally similar to abuse, such as a large order worked in clips and closed later. They are not failures; they are the reason scoring in dollars matters.

| Metric | From ordinary flow | Total alerts |
|---|---:|---:|
| Insider Trading | 1 | 4 |
| Front Running | 1 | 10 |
| Internal Front Running | 1 | 7 |
| Ramping | 43 | 61 |
| Banging the Close | 0 | 4 |
| Wash Trading | 1 | 37 |
| Cross Trading | 0 | 3 |
| Off-Market Trading | 0 | 20 |
| Spoofing | 0 | 32 |
| Layering | 0 | 14 |
| Best Execution | 0 | 16 |
