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

## Repo
taimoor-asghar/greenretrofit-tools (create if missing). Structure: tools/<slug>.html, PROGRESS.md, sitemap.xml on completion.

## Deployment
Static HTML uploaded to greenretrofitguide.com via WP File Manager (proven path). Taimoor uploads or a later browser task — chunk workers build and commit only.
