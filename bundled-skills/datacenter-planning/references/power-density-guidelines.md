# Power Density Guidelines

## Per-Rack Power by Workload Type

| Workload | kW/rack (typical) | kW/rack (peak) | Cooling Method | Circuit Required |
|----------|------------------|----------------|----------------|-----------------|
| General-purpose CPU | 4–8 | 10–12 | Air (CAC) | 30A 3-phase |
| Storage-heavy | 6–10 | 12–15 | Air (CAC) | 30A 3-phase |
| Compute-intensive CPU | 8–15 | 18–22 | Air + rear-door HX | 60A 3-phase |
| GPU training (H100/B200) | 30–50 | 60+ | Direct-to-chip liquid | 60A 3-phase 400V |
| GPU inference | 15–25 | 30–40 | Direct-to-chip or RDHx | 60A 3-phase |
| High-density colocation | 10–20 | 25–35 | Liquid or hybrid | 60A 3-phase |
| Edge / micro DC | 2–5 | 8 | Air (DX) | 30A single-phase |

## Facility-Level Power Metrics

At various IT load scales:

| Metric | 100 kW IT | 500 kW IT | 1 MW IT | 5 MW IT | 20 MW IT |
|--------|-----------|-----------|---------|---------|----------|
| Racks (15 kW/rack avg) | 7 | 34 | 67 | 334 | 1334 |
| White space area (sq ft) | ~850 | ~4,000 | ~8,000 | ~40,000 | ~160,000 |
| Total facility area (sq ft) | ~3,000 | ~12,000 | ~25,000 | ~120,000 | ~450,000 |
| Cooling plant (tons) | ~31 | ~156 | ~313 | ~1,565 | ~6,260 |
| Generator capacity (kW) | ~160 | ~800 | ~1,600 | ~8,000 | ~32,000 |
| UPS capacity (kW) | ~150 | ~750 | ~1,500 | ~7,500 | ~30,000 |
| Utility service | 300A @ 480V | 1,200A @ 480V | 2,400A @ 480V | 12,500A @ 480V or MV | MV service |
| PUE (air-cooled, contained) | 1.5 | 1.4 | 1.35 | 1.3 | 1.25 |
| Annual electric bill (@$0.10/kWh) | $87K | $438K | $876K | $4.4M | $17.5M |

## Circuit Sizing Quick Reference (208V 3-phase, North America)

| Breaker | Derated Continuous (80%) | Usable kW | Typical Use |
|---------|------------------------|-----------|-------------|
| 15A 1-pole | 12A | 1.4 kW | Single server, network switch |
| 20A 1-pole | 16A | 1.9 kW | Storage shelf, monitor |
| 30A 3-phase | 24A | 8.6 kW | Low-density rack, 4–6 servers |
| 50A 3-phase | 40A | 14.4 kW | Medium-density rack |
| 60A 3-phase | 48A | 17.3 kW | High-density rack |
| 100A 3-phase | 80A | 28.8 kW | Very high-density or RDHx |

At 400V 3-phase (EMEA/APAC), multiply usable kW by 400/208 = 1.92x. A 60A circuit at 400V delivers ~33 kW usable.

## Power Path Loss Budget

| Component | Typical Loss | % of Total |
|-----------|-------------|------------|
| Utility transformer | 1–2% | 1.5% |
| Switchgear + distribution | 0.5–1% | 0.8% |
| UPS (double-conversion, 96% eff) | 4% | 4.0% |
| PDU transformer (480→208V) | 2–3% | 2.5% |
| RPP + branch conductors | 0.5–1% | 0.8% |
| Rack PDU + whip | 0.5–1% | 0.5% |
| IT equipment PSU | 5–15% | 10.0% |
| **Total distribution loss** | | **~7.6% (excl. IT PSU)** |

Every point of efficiency improvement in the power path translates directly to PUE improvement. Replacing a 93% efficient UPS with a 97% efficient unit saves 4% of total facility energy.
