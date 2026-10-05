# GreenRetrofitGuide Build Spec — Phase 1
Target site: https://greenretrofitguide.com/ | Market: UK | Created 2026-10-05

## Quality bar (non-negotiable)
- Single static HTML file per tool, no build step, no external JS libs (vanilla JS + inline CSS).
- Maths verified against hand-computed known answers before ship. Record the test cases in PROGRESS.md.
- 1200+ words of substantive guide prose per page + FAQPage schema + MedicalWebPage-style E-E-A-T adapted: author box (Taimoor), reviewed date, references to real sources (Energy Saving Trust, MCS, gov.uk, Energy Saving Trust figures).
- One H1, meta title/description, canonical https://greenretrofitguide.com/tools/<slug>/, OG tags, mobile-responsive, accessible labels.
- Design: clean light theme, green accent (#1a7a4a), ink-navy text. Match the doctorwithdata tool aesthetic (single-column, card inputs, big result panel).

## URL structure
- Hub: https://greenretrofitguide.com/tools/
- Tools: https://greenretrofitguide.com/tools/<slug>/

## Tool 1 — Solar Panel ROI Calculator — slug: solar-panel-roi
Inputs: UK region (South East 1050, South West 1000, Midlands 920, North 850, Scotland 780 kWh/kWp/yr), system size kWp (2–10, default 4), roof orientation (South 1.00, SE/SW 0.95, East/West 0.85, North 0.60), shading (None 1.00, Light 0.90, Heavy 0.75), electricity import tariff p/kWh (default 24), install cost £ (default: kWp × £1,600), export tariff SEG p/kWh (default 8), self-consumption % (default 40).
Maths:
- annual_kwh = kwp × regional_yield × orientation × shading
- self_kwh = annual_kwh × self_consumption/100; export_kwh = annual_kwh − self_kwh
- annual_saving £ = self_kwh × import_p/100 + export_kwh × export_p/100
- payback_years = install_cost / annual_saving
- profit_25yr = (annual_saving × 22.5) − install_cost  (22.5 ≈ 25 yrs with 0.5%/yr degradation)
- co2_tonnes_yr = annual_kwh × 0.207 / 1000
Test case: 4 kWp, South East, South, no shade, 24p import, £6,400 install, 8p export, 40% self-use → annual_kwh = 4200; saving = 1680×0.24 + 2520×0.08 = 403.20 + 201.60 = £604.80; payback ≈ 10.6 yrs.
Guide must cover: how SEG works, orientation/shading reality, battery pairing note, MCS certification.

## Tool 2 — Heat Pump vs Gas Boiler — slug: heat-pump-vs-gas-boiler
Inputs: annual gas bill £ (default 900) OR annual heating demand kWh, gas tariff p/kWh (default 6), electricity tariff p/kWh (default 24), boiler efficiency % (default 85), heat pump SCOP (default 3.2, range 2.5–4.5), boiler install £ (default 3000), heat pump install £ (default 12000), BUS grant £ (default 7500, toggle eligible yes/no).
Maths:
- demand_kwh = (gas_bill £ × 100 / gas_p) × boiler_eff/100   [if bill given]
- gas_annual £ = demand_kwh / (boiler_eff/100) × gas_p/100
- hp_annual £ = demand_kwh / SCOP × elec_p/100
- saving £ = gas_annual − hp_annual
- net_hp_cost = hp_install − (grant if eligible); extra_upfront = net_hp_cost − boiler_install
- payback = extra_upfront / saving; co2_saved_t = demand_kwh × (0.21 − 0.207/SCOP... simplify: gas_co2 = demand/0.85×0.21/1000, hp_co2 = demand/SCOP×0.207/1000, saved = gas_co2 − hp_co2)
Test case: £900 gas bill, 6p gas, 24p elec, 85% boiler, SCOP 3.2 → demand = 12,750 kWh; gas_annual = £900; hp_annual = 12,750/3.2×0.24 = £956 → NEGATIVE saving. Worker must surface this honestly: at 24p/6p price ratio heat pumps can cost more to run — the tool must not hide it. This honesty is the brand.
Guide: SCOP explained, radiator upgrades, BUS grant how-to, when a heat pump does NOT pay.

## Tool 3 — Home Energy Audit — slug: home-energy-audit
15 questions: property type (detached/semi/terrace/flat), age band (pre-1920 … post-2012), wall type (solid/cavity/insulated cavity), loft insulation depth, glazing (single/double pre-2002/double modern/triple), heating (gas boiler age, oil, electric, heat pump), controls (thermostat/TRVs/smart), draught-proofing, hot water cylinder insulation.
Scoring: start 100 (A), subtract per answer using SAP-lite weights (document weights in code comments). Map score → EPC band (A 92+, B 81+, C 69+, D 55+, E 39+, F 21+, G below).
Output: estimated band, top 3 retrofit recommendations with typical cost bands and saving bands (cite Energy Saving Trust ranges), link to the matching calculators.
Test: modern detached, cavity insulated, 270mm loft, modern double glazing, new combi → expect band B/C.

## Tool 4 — Insulation Savings Calculator — slug: insulation-savings
Inputs: property type, measure (loft 0→270mm, cavity wall fill, solid wall internal, solid wall external, floor), current state, install cost £ (prefilled EST-typical, editable).
Savings table (Energy Saving Trust typical, detached house, £/yr): loft top-up £300, cavity fill £350, solid wall £500, floor £150. Scale by property type (semi ×0.7, terrace ×0.6, flat ×0.4).
Outputs: annual £ saving, payback, 10-yr net, CO2 saved (kWh saved × 0.21/1000, kWh saved ≈ saving £ / gas_p × 100).
Test: detached, loft 0→270mm, cost £500 → saving £300, payback 1.7 yrs.

## Tool 5 — Home Carbon Footprint — slug: home-carbon-footprint
Inputs: annual gas kWh, annual electricity kWh, car miles + fuel type, flights short/long haul per year, diet (meat-heavy … vegan — small weight).
Factors: gas 0.21, electricity 0.207 kgCO2/kWh; car petrol 0.28, diesel 0.27, hybrid 0.20, EV 0.05 kg/mile; flight short 0.25 t, long 1.0 t each; diet adder 0.5–2.0 t.
Outputs: total t/yr, vs UK average 10 t, breakdown bar chart (CSS), 3 personalised reduction actions ranked by tonnes saved.
Test: 12,000 kWh gas + 3,000 kWh elec + 8,000 petrol miles → 2.52 + 0.62 + 2.24 = 5.38 t + flights/diet.

## Hub page — slug: tools (path /tools/)
Card grid of the 5 tools with icon, name, one-line description, live search filter. Same interaction pattern as doctorwithdata.com/calculators/.

## Phase 2 — decision helpers

### Tool 6 — Double Glazing ROI Calculator — slug: double-glazing-roi
Inputs: property type (detached 1.4, semi 1.0, terrace 0.8, flat 0.5), number of windows, current glazing (single, old double pre-2002), new glazing (modern double 1.0, triple 1.3), install cost £ (default windows × £500).
Maths:
- annual_saving £ = windows × 16.5 × property_factor × new_glazing_factor  (16.5 = EST per-window saving £/yr for single→double, semi baseline)
- payback_years = install_cost / annual_saving
Test case: semi, 10 windows, single→modern double, £5,000 → saving £165/yr, payback ≈ 30 yrs. The tool must be honest: glazing payback is long — lead with comfort, noise and condensation benefits, not just £.
Guide: U-values, trickle vents and ventilation warning, FENSA certification.

### Tool 7 — EV Home Charger Cost Calculator — slug: ev-home-charger
Inputs: annual miles, current car mpg (default 40), petrol price £/L (default 1.45), EV efficiency miles/kWh (default 3.5), charging tariff p/kWh (default 7 EV overnight tariff, option 24 standard), charger install £ (default 1000).
Maths:
- petrol_annual £ = miles / mpg × 4.546 × petrol_price
- ev_annual £ = miles / efficiency × tariff_p / 100
- saving £ = petrol_annual − ev_annual; payback = install / saving
Test case: 8000 miles, 40 mpg, £1.45/L, 3.5 mi/kWh, 7p tariff → petrol £1,318, EV £160, saving £1,158/yr, payback 0.9 yr.
Guide: EV tariffs (Octopus Intelligent etc.), charger grants for renters/flats, 3-pin vs 7kW reality.

### Tool 8 — Smart Thermostat Savings Estimator — slug: smart-thermostat-savings
Inputs: annual heating bill £, current controls (none 15%, basic thermostat 10%, programmer 5%), occupancy (out weekdays 1.2, mixed 1.0, home all day 0.7), thermostat cost £ (default 200 installed).
Maths: annual_saving = bill × control_rate × occupancy_factor; payback = cost / saving.
Test case: £900 bill, no controls, out weekdays → £162/yr, payback 1.2 yrs.
Guide: what smart features actually save money (geofencing, weather compensation), compatibility with combi vs system boilers.

### Tool 9 — Grant & Subsidy Finder — slug: grants-and-subsidies
Inputs: nation (England/Wales/Scotland/NI), tenure (own/mortgage, private rent, social rent), EPC band (A–G or unknown), on means-tested benefits (yes/no), council tax band (A–H), current heating (gas/oil/electric/heat pump).
Eligibility logic:
- BUS (Boiler Upgrade Scheme): England/Wales + owner + replacing fossil fuel heating → £7,500 heat pump grant.
- ECO4: on benefits + EPC E–G + owner or private renter → insulation/heating measures, fully funded.
- GBIS: EPC D–G + council tax A–D (England) → one insulation measure.
- Home Energy Scotland: Scotland + owner → up to £7,500 grant + £7,500 interest-free loan.
- 0% VAT: all UK, energy-saving installations → automatic at point of sale.
Output: eligible schemes with amounts, key conditions, and gov.uk apply links. Never invent scheme details — link out.
Test case: England, owner, EPC E, on benefits, gas boiler → ECO4 + BUS + 0% VAT.

### Tool 10 — Green Mortgage Checker — slug: green-mortgage-checker
Inputs: mortgage amount £, term years, standard rate %, green discount % (default 0.15), EPC band (A/B qualifies, else explain).
Maths: monthly payment M = P × r(1+r)^n / ((1+r)^n − 1), r = annual/12, n = years×12. Compute M_standard and M_green; saving_2yr = (M_standard − M_green) × 24.
Test case: £200,000, 25 yrs, 4.5% vs 4.35% → M ≈ £1,112 vs £1,095, saving ≈ £410 over 2-yr fix.
Guide: which lenders offer green products (2026 snapshot, verify before ship), EPC evidence requirements, remortgage vs product transfer.

## Phase 3 — deep retrofit toolkit

### Tool 11 — Cavity Wall Suitability Checker — slug: cavity-wall-suitability
Quiz, not a calculator: wall construction (cavity/solid/timber/unknown), property age, exposure (sheltered/moderate/severe driving rain), existing damp or wall damage (yes/no), wall finish (rendered/brick/stone), cavity width if known.
Logic: NOT suitable → solid/timber walls, severe exposure + rendered, existing damp, narrow cavity <50mm. NEEDS SURVEY → unknown wall type, moderate exposure, stone walls. SUITABLE → clear cavity, no damp, sheltered/moderate.
Output: verdict + plain-English why + next step (find a CIGA-registered installer). Never recommend fill for unsuitable walls — this is a safety-critical answer.
Test: 1930s semi, cavity, moderate exposure, no damp → SUITABLE.

### Tool 12 — Draught-Proofing Savings Calculator — slug: draught-proofing-savings
Inputs: property type (detached 1.3, semi 1.0, terrace 0.8, flat 0.5), measures ticked (doors, windows, chimney, floorboards — each adds to cost), DIY vs professional (DIY £120 total, pro £400).
Maths: annual_saving = 70 × property_factor (EST-typical £70/yr for a semi); payback = cost / saving.
Test: semi, all measures, DIY £120 → £70/yr, payback 1.7 yrs.
Guide: where draughts actually come from, ventilation warning (never seal air bricks or extractor needs).

### Tool 13 — Solar Battery Payback Calculator — slug: solar-battery-payback
Inputs: annual solar generation kWh (or array kWp → reuse Tool 1 yields), current self-consumption %, battery usable kWh (default 10), battery cost £ (default 5000), import tariff p (24), export tariff p (8).
Maths:
- exported_kwh = generation × (1 − self_use/100)
- extra_self_kwh = min(battery_kwh × 300 cycles, exported_kwh × 0.6)
- added_saving £ = extra_self_kwh × (import_p − export_p) / 100
- payback = battery_cost / added_saving
Test: 4200 kWh gen, 40% self-use → exported 2520; extra_self = min(3000, 1512) = 1512; added saving = 1512 × 0.16 = £241.92; payback ≈ 20.7 yrs. The tool must be blunt: at current prices batteries rarely pay for themselves — show the comfort/backup value honestly.
Guide: battery degradation, SEG interaction, when batteries DO make sense (high import tariffs, frequent outages).

### Tool 14 — Heat Pump Readiness Quiz — slug: heat-pump-ready
Quiz: loft insulation ≥270mm (y/n), walls insulated (y/n), glazing (single/double/triple), radiators (standard/oversized/underfloor), outdoor space for unit (y/n), current system (gas/oil/electric).
Scoring: +2 per yes/good, +1 partial; ≥9 READY, 5–8 ALMOST (list the 2–3 upgrades), <5 NOT YET (fabric first).
Output: readiness verdict + prioritised upgrade list with links to the matching calculators (Tools 4, 6, 12).
Test: 2010 semi, insulated cavity, 270mm loft, double glazing, standard radiators, garden, gas boiler → ALMOST (radiators).

### Tool 15 — Hot Water Cylinder Jacket Calculator — slug: cylinder-jacket-savings
Inputs: current insulation (none/thin foam/<80mm jacket/80mm+ jacket), cylinder size (affects base), jacket cost £ (default 25).
Maths: annual_saving — none £70, thin £35, <80mm £15, 80mm+ £0 (already done, say so). payback = cost / saving.
Test: no insulation → £70/yr, payback 0.4 yr. Cheapest win on the site — say so.
Guide: 80mm British Standard jacket, pipe insulation bonus, thermostat 60°C.

### Tool 16 — LED Lighting Savings Calculator — slug: led-savings
Inputs: bulb count, old wattage (default 50 halogen), new wattage (default 6 LED), hours/day (default 3), tariff p/kWh (24), LED cost each £ (default 4).
Maths:
- kw_saved = bulbs × (old_w − new_w) / 1000
- annual_kwh = kw_saved × hours × 365
- saving £ = annual_kwh × tariff/100; cost = bulbs × led_price; payback = cost/saving
Test: 20 bulbs, 50→6W, 3 h/day → 963.6 kWh, £231/yr, cost £80, payback 0.35 yr.
Guide: lumens not watts, warm vs cool white, dimmer compatibility.

### Tool 17 — Standby Power Cost Calculator — slug: standby-power-cost
Inputs: tick-box device list with typical standby watts — TV 3, set-top box 8, games console 10, printer 4, microwave 3, phone charger 0.5, laptop charger 2, desktop PC 5, smart speaker 3, electric shower pull-cord n/a — plus hours/day on standby (default 20), tariff p/kWh (24).
Maths: annual_kwh = Σ(watts × hours × 365) / 1000; cost £ = kwh × tariff/100.
Output: £/yr wasted + CO2 + "switch it off at the wall" ranking by device.
Test: TV+set-top+console+printer at 20h standby → (3+8+10+4)×20×365/1000 = 182.5 kWh → £43.80/yr.
Guide: smart plugs, what NOT to switch off (fridge, router if needed, Sky Q).

### Tool 18 — Green Tariff Comparison — slug: green-tariff-comparison
Inputs: annual electricity kWh, current unit rate p + standing charge p/day, current tariff % renewable. Editable comparison table prefilled with 3 typical green tariffs (rates clearly marked "example — check current").
Maths: annual_cost = kwh × unit_p/100 + standing_p × 365/100; show each tariff's cost, saving vs current, and kWh-matched renewable %.
Output: ranked table + warning that rates change monthly — link to Ofgem-accredited comparison sites for live quotes. Never present example rates as live.
Test: 2900 kWh, current 24p + 55p/day → £891/yr baseline; green tariff 23p + 50p/day → £849.50, saving £41.50.

### Tool 19 — Whole-House Retrofit Planner — slug: retrofit-planner
Multi-step planner: property type, age, current EPC (or quick 6-question estimate), budget band, goal (cut bills / cut carbon / both).
Logic: assemble measures from Tools 1–18 database (each with cost band, saving band, disruption level), then sort into three phases:
- Phase A — quick wins (payback <2 yrs, cost <£500): LED, cylinder jacket, draught-proofing, standby, thermostat.
- Phase B — fabric (insulation, glazing): the building must be efficient before new heating.
- Phase C — systems (heat pump, solar, battery): sized for the improved fabric.
Output: phased plan with cumulative cost, cumulative annual saving, cumulative CO2, and a printable summary. This is the seed of the January digital product.
Test: 1970s semi, EPC D, £15k budget → Phase A ~£600/£550yr, Phase B loft+cavity ~£1,500/£650yr, Phase C solar 4kWp ~£6,400/£605yr.

### Tool 20 — EPC Improvement Estimator — slug: epc-improvement
Inputs: current EPC band (or SAP points if known), tick-box measures.
SAP-lite point gains (estimates, clearly labelled): loft to 270mm +8, cavity fill +10, solid wall insulation +15, double glazing +5, heat pump +12, solar PV +6, heating controls +3, draught-proofing +2, cylinder jacket +1.
Maths: new_points = current + Σ gains; band from points (A 92+, B 81+, C 69+, D 55+, E 39+, F 21+, G <21); cost per point = total cost / points gained.
Output: projected band, measures ranked by points-per-£, caveat that only a real EPC assessment counts for sales/rentals.
Test: D (60 pts) + loft (+8) + cavity (+10) → 78 pts → band C.

## Repo
taimoor-asghar/greenretrofit-tools (create if missing). Structure: tools/<slug>.html, PROGRESS.md, sitemap.xml on completion.

## Deployment
Static HTML uploaded to greenretrofitguide.com via WP File Manager (proven path). Taimoor uploads or a later browser task — chunk workers build and commit only.
