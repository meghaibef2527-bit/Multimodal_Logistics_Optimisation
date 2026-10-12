# Interview Preparation

## 60-second answer
I built a synthetic multimodal logistics optimisation model to identify the lowest-cost feasible route for each shipment while respecting SLA and leg-level capacity. I used Python to reconstruct route economics from individual route legs rather than simply copying the supplied recommendation table. I scaled tonne-kilometre costs to actual shipment weight, tested each candidate for capacity and SLA, selected the minimum-cost feasible route, independently validated the result, compared it with a logically valid direct-road baseline, and ran sensitivity scenarios. I then prepared Excel and Power BI-ready outputs for business communication.

## 2-minute technical answer
The data is relational: shipments, hubs, network connections, candidate routes, route legs and reference recommendations. The key modelling choice is that cost per kilometre is a USD-per-tonne-km rate. I therefore calculate shipment-specific variable cost as tonnes × distance × tonne-km rate and add fixed and handling costs. I sum transit time, use minimum leg capacity as the route capacity constraint, calculate reliability multiplicatively across legs, and calculate emissions from tonne-km factors. A candidate is feasible only when every leg can carry the shipment and total transit time meets the SLA. I select the lowest-cost feasible candidate and retain `No feasible route` when none exists.

## Likely questions
1. **Why multimodal?** Different modes trade off cost, speed, capacity, reliability and network access.
2. **Why Python?** Reproducible and scalable route calculations across 5,000 shipments.
3. **Why Power BI?** Interactive management reporting after the analytical engine is complete.
4. **Why not the cheapest route?** The cheapest overall route may violate SLA or capacity.
5. **How did you define SLA?** Route transit time <= shipment required delivery hours.
6. **How did you handle capacity?** Every route leg must have capacity >= shipment weight.
7. **How did you calculate cost?** Tonnes × distance × USD/tonne-km + fixed + handling.
8. **How did you validate?** Independent recalculation against the supplied recommendation output.
9. **Why no-feasible routes?** All candidates can fail SLA and/or capacity; forcing a route would hide network gaps.
10. **What would you add to real data?** Live congestion, real rates, fuel, port dwell, dynamic capacity, directional rates and lane reliability.

## Challenge questions
**“The data is synthetic. Why does the project matter?”** — The portfolio value is the transparent optimisation methodology, validation discipline and reproducibility, not a claim of real carrier performance.

**“Why did your result differ from the supplied recommendations?”** — I independently reconstructed the leg-level economics and did not force agreement. The discrepancies are retained for model governance.

**“Why a road baseline?”** — Road is the direct flexible baseline. Savings are only counted when road itself meets SLA and capacity.

## Numbers to remember
- 5,000 shipments
- 60 hubs
- 393 network connections
- 516 candidate routes
- 804 route legs
- 4,875 independently feasible shipments
- 99.64% route agreement with reference
- 18 route discrepancies
- 4,401 valid road-baseline comparisons
- $198,968 modelled savings vs valid road baseline

Never call the data “Maersk data”; call it a synthetic modelling dataset.
