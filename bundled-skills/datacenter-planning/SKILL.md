---
name: datacenter-planning
description: "Critical infrastructure planning for data centers — space layout, power path design (utility to rack), cooling systems (air/liquid/immersion), tier classification, equipment installation, grounding/bonding, structured cabling, PUE optimization, and commissioning. Use when installing equipment or designing facility infrastructure."
risk: safe
source: community
date_added: "2026-05-25"
---

## When to Use

Use this skill when planning, designing, or installing equipment in a data center — greenfield builds, colocation fit-outs, capacity expansions, technology refresh cycles, or individual equipment deployments. This covers physical critical infrastructure decisions: space allocation and layout, power distribution from utility feed to rack PDU, cooling capacity and delivery method, structured cabling topology, grounding and bonding, tier/Uptime Institute classification, efficiency targets and PUE measurement, and commissioning procedures.

## Do Not Use This Skill When

- Designing cloud-native architectures or selecting cloud services (use `cloud-architect`)
- Procuring electricity contracts, PPAs, or utility tariff analysis (use `energy-procurement`)
- Writing software deployment pipelines or CI/CD configuration
- Selecting server hardware models or comparing CPU/GPU specifications
- Network protocol design or routing configuration

## Role and Context

You are a senior critical infrastructure engineer with 15+ years designing, building, and commissioning mission-critical data center facilities. You have held roles across the full stack: power systems engineer (utility coordination, switchgear, UPS, generators), mechanical engineer (cooling plant, containment, liquid cooling), and operations (capacity management, efficiency optimization, incident response). You work at the intersection of electrical engineering (NEC, NFPA 70E), mechanical engineering (ASHRAE, SMACNA), telecommunications (TIA-942, BICSI), and industry frameworks (Uptime Institute Tier Standard).

Your stakeholders include: facilities directors who own the physical plant, electrical contractors who build the infrastructure, IT operations who install and maintain equipment, network architects who need predictable cable pathways, colo provider engineers who enforce their standards, and finance who approve every watt of capacity and want to see PUE improve year over year. You balance availability (tier level) against capital efficiency — because a Tier IV design costs 2.5–4x Tier III but may not deliver proportional business value for every workload.

You understand that every decision has a trade-off: higher redundancy means higher capex and typically lower efficiency. Higher power density means less floor space but more complex cooling. Liquid cooling enables GPU clusters but adds leak risk and maintenance complexity. Your job is to present clear options with quantified trade-offs, not to prescribe a single answer.

## Core Knowledge

### Global Standards and Regulatory Framework

**Electrical Standards:**
- IEC (International Electrotechnical Commission) standards govern electrical installations globally, though adoption varies by region
  - IEC 60364: Low-voltage electrical installations (analogous to NEC in North America)
  - IEC 60909: Short-circuit current calculation
  - IEC 61439: Switchgear and control gear assemblies
  - IEC 62310: Transfer systems (ATS)
- North America (US/Canada): NEC (NFPA 70), CSA C22.1. Voltages: 480V/277V wye, 208V/120V wye
- EMEA (Europe, Middle East, Africa): IEC standards, EN standards mandatory in EU. Voltages: 400V/230V TN-S system, 230V/400V across the EU as harmonized (IEC 60038)
- APAC: Varies by country. Australia (AS/NZS 3000), Japan (JIS C 0364), China (GB 50054). Voltages: 380V/220V in mainland China, 230V in Australia/India
- IEC 60038 defines standard voltages: 230V (tolerance +10/-6%) for nominal 230V systems, harmonized to replace 220V/240V legacy systems
- Three-phase systems: 400V (Europe/Asia) = 480V (North America) for the same power delivery capacity at 50Hz vs 60Hz

**Building and Structural Standards:**
- North America: IBC (International Building Code), ASCE 7 (minimum design loads including seismic)
- Europe: Eurocode EN 1991 (actions on structures), EN 1998 (seismic design)
- Seismic zones: ASCE 7 categories (A–F) in US; IBC assigns Seismic Design Categories (SDC) A–D+. ASCE 7-22 requires seismic design for Data Centers in SDC B and above
- Eurocode seismic: EC8 classifies facilities into importance classes. Critical infrastructure typically importance factor γI = 1.5
- Australia: AS 1170 (minimum design actions)
- IBC and Eurocode both require anchor bolt design for equipment racks in seismic zones

**Cooling and Mechanical Standards:**
- ASHRAE TC 9.9: Thermal guidelines (globally applicable, used as reference in most markets)
- Europe: FprEN 50600 series (data center facilities infrastructure) — European adoption of TIA-942
- VDI (Germany): VDI 6022 (ventilation and air conditioning), VDI 3803 (air-conditioning systems)
- ISO 11084: Rack systems (European standard for 19-inch rack dimensions)

**Telecommunications Cabling:**
- TIA-942: North American data center cabling standard (widely adopted globally as de facto)
- ISO/IEC 11801: International generic cabling for customer premises
- EN 50173: European datacentre cabling standard
- BICSI 002: Data Center Design and Implementation Best Practices (global)
- TIA-607-B / IEC 60304: Grounding and bonding for telecom

**Seismic Bracing Specifics:**
- ASCE 7-22 Section 13.2.1: Equipment in Data Centers must be anchored per manufacturer's anchorage design or prescriptive requirements Table 13.5-1
- IBC Table 1705.13: Special inspection requirements for anchorage and bracing
- Rack anchoring: either floor-mounted via approved anchor bolts (min 3/4" galvanized steel) with认定为 rating, or overhead restrained ( seismically rated_un拘束)
- Seismic Restraint Company: earthquake restraints certified to OSHPD (California) or pre-qualified to ASCE 7
- Post-installation: verify anchor bolt torque and restraint engagement per ICC-ES AC336

**Environmental and Sustainability:**
- ISO 14001: Environmental management systems
- ISO 50001: Energy management systems
- BREEAM (Europe): Building research establishment environmental assessment method — data center specific credit categories
- LEED (North America): Leadership in Energy and Environmental Design — available for data centers
- EU Energy Efficiency Directive (EED): mandates large enterprises to undergo energy audits every 4 years

**Metrication:**
- This skill uses both metric (SI) and imperial (US customary) units — this is intentional because data centers operate in both systems depending on region
- Always state units explicitly. Do not assume the reader uses either system
- Temperature: Celsius primary, Fahrenheit in parentheses for North American contexts
- Power: kilowatts (kW) universally
- Area: square meters (m²) or square feet (sq ft) — use both for cross-regional clarity
- Pressure: Pascals (Pa) for airflow, inches of water column (in-wg) for US practice (1 in-wg = 249 Pa)

### Space Planning and Layout

**White Space Dimensions and Rack Layout:**

- Standard 19-inch rack footprint: 600mm wide × 1070mm deep (typical) up to 1200mm deep for GPU servers with rear-door heat exchangers. Height: 42U–48U (standard), 52U for some high-density configurations
- Hot aisle / cold aisle (HACA) is the non-negotiable standard. Racks face each other into cold aisles; backs face into hot aisles. The pattern alternates: cold / hot / cold / hot across the floor. Never arrange all racks facing the same direction — this creates hot spots that no amount of cooling can fix
- Cold aisle width: minimum 1200mm (4 ft). Wider aisles (1500–1800mm / 5–6 ft) improve airflow uniformity but consume floor space. Hot aisle width: 900–1200mm (3–4 ft). If contained, hot aisles can be narrower because personnel access is restricted
- Row length: 10–20 racks per row is the practical maximum. Beyond 20 racks, end-of-row airflow velocity drops, and the middle racks receive significantly less cooling airflow than end racks. CFD modeling is recommended for any row longer than 15 racks
- End-of-row clearances: minimum 900mm (3 ft) for equipment movement and service access. Main corridors: minimum 1200mm (4 ft), preferably 1800mm (6 ft) for equipment delivery pathways

**Containment:**

- Cold aisle containment (CAC) is strongly preferred for most deployments. It isolates the cold supply from the hot return, allowing higher supply temperatures (18–22°C instead of 12–15°C) and improving chiller efficiency. CAC requires: aisle doors at both ends (sliding or swing), overhead panels between racks (brush grommets or solid panels), and sealing all gaps under racks and around cable penetrations
- Hot aisle containment (HAC) contains the exhaust heat and returns it directly to the cooling unit. Allows even higher supply temperatures (22–27°C) because the cold aisle is the general room ambient. However, HAC complicates fire suppression — smoke detection and clean-agent release must account for the contained volume, and personnel safety during egress from contained aisles must be addressed
- Containment leak testing: after installation, use a smoke pencil or thermal camera during commissioning to identify and seal all unintended air paths. A 5% containment leak can increase cooling energy by 15–25% because the cooling unit sees mixed return temperatures and cannot operate efficiently

**Floor Loading and Structural:**

- Raised floor: standard tile size 600×600mm. Typical loading: 7.5–12 kN/m² (150–250 lb/ft²) for general-purpose. High-density GPU zones: 15+ kN/m² (300+ lb/ft²). Verify floor loading capacity before installing any equipment exceeding 500 kg — many raised floors are rated for uniform load only, not point loads from heavy servers
- Slab-on-grade (no raised floor): increasingly common for liquid-cooled and high-density deployments. Eliminates the 400–600mm raised floor height, reducing total building height and improving seismic stability. Cable routing is entirely overhead in basket trays
- Ceiling height: minimum 4.2m (14 ft) from finished floor to bottom of overhead obstructions for raised-floor designs. Minimum 4.5m (15 ft) for overhead busway and cable tray routing. For liquid cooling with overhead piping: minimum 5.0m (16.5 ft) to accommodate piping, insulation, and condensate drains
- Seismic bracing: required in seismic zones (IBC categories C, D, E, F). Racks must be braced to overhead structure or bolted to floor. Unbraced racks in a seismic event become projectiles. Anchor bolts must be torqued to manufacturer specification and periodically re-torqued (every 12 months)

**Physical Security:**

- Perimeter security layers:
  - Outer perimeter: fence line or building facade with controlled entry points. Minimum 8 ft (2.4m) anti-climb fence with intrusion detection (strain-sensitive cable or fiber optic fence sensors)
  - Vehicle barrier: crash-rated bollards or planters at vehicle access points (loading dock, entry). Must stop a 15,000 lb vehicle at 50 mph (K12 or K8 rating per ASTM F2656)
  - Exterior lighting: full coverage of perimeter and parking areas. 0.5 foot-candles minimum at grade. LED with motion sensors and backhaul connectivity for remote monitoring
- Access control:
  - Mantrap or sally port: interlocking vestibule that prevents tailgating. First door must close and lock before second door opens. Required at main entrance to white space zone
  - Badge or proximity card (125 kHz or 15.56 MHz NFC) for standard access. Biometric (fingerprint or iris) for high-security areas
  - Visitor management: temporary badges with escort requirement. Visitor access logged and time-limited
  - Turnstile or man-trap at white space entry: anti-tailgate sensors detect if more than one person attempts to pass on a single credential
  - Server room/cage access: biometric + PIN or dual-badge requirement for Tier III/IV white space
- CCTV coverage:
  - All exterior entry points, loading dock, mantrap, white space aisles, and mechanical rooms must have camera coverage
  - Camera type: IP-based, minimum 4MP resolution, IR illumination for low-light areas
  - Retention: minimum 90 days of continuous recording for white space, 30 days for perimeter
  - Monitoring: NVR with motion detection and analytics (object left, line crossing). Real-time monitoring by security operations center or third-party monitoring service
- Cage and zone security:
  - Cage walls: floor-to-ceiling or roof-deck (minimum 9 ft / 2.7m), welded wire mesh or gypsum board on steel studs. Cage walls should not interfere with sprinkler coverage
  - Perimeter of each cage: card reader + biometric. Cage door: electromagnetic lock with request-to-exit (REX) button
  - Multi-tenant colocation: each customer cage is a separate security zone with independent access control and billing
- Loading dock security:
  - Minimum 3m (10 ft) wide, 4.5m (15 ft) tall, with leveling dock plate capable of supporting 1500+ kg pallet jacks and rack delivery skates
  - Overhead clearance for vertical rack rotation during unboxing
  - Dock door: motorized roll-up door with card reader and camera. Interlock with security desk to prevent unauthorized access
  - Receiving area: secure staging room where all deliveries are inspected before entry to white space. Package scanning and x-ray for loose equipment
- Compliance considerations:
  - PCI-DSS v4.0 Section 9: physical security controls for data centers handling cardholder data. Requires annual penetration testing, 24/7 monitoring, and proper escort procedures
  - SOC 2 Type II: physical access logs must demonstrate effective access control over the assessment period
  - HIPAA Security Rule: physical safeguards for electronic protected health information (ePHI). Access to server rooms must be restricted to workforce members with proper authorization
  - Data sovereignty regulations (GDPR Art. 30, various national laws): may require data to remain within specific geographic boundaries. Physical security controls must prevent unauthorized cross-border data access

### Edge and Micro Data Centers

**Use Cases and Definition:**
- Edge data centers: typically 100 kW–2 MW IT load, serving latency-sensitive applications (CDN, autonomous vehicles, IoT, mobile edge computing)
- Micro data centers: <100 kW, often pre-fabricated container or box deployment, used for cell tower backhaul, retail edge, or temporary construction site facilities
- Key characteristics: limited or no on-site staff, remote management, may be in uncontrolled environments (industrial, outdoor, remote locations)

**Design Considerations for Edge/Micro:**

- Prefabricated vs. stick-built: prefabricated units (Schneider EcoStruxure Micro Data Center, Vertiv SmartRow, Dell Micro边缘数据中心) arrive pre-tested with power, cooling, and security integrated. Lead time: 8–16 weeks vs. 12–24 months for custom build. Preferred for edge deployments where construction quality is variable
- Environmental rating (IEC 60529): outdoor edge cabinets must meet IP54 or higher for dust and water protection. NEMA 3R minimum for outdoor, NEMA 4 or 4X for corrosive environments
- Power redundancy: most edge sites are Tier I–II. Single utility feed + UPS + generator is standard. 2N is rarely justified at edge due to cost
- Cooling: DX (direct expansion) air conditioning is most common at edge — no chilled water loop required. Condenser can be remote. For outdoor cabinets, thermoelectric cooling or packaged DX with redundant compressors may be used
- Remote monitoring: mandatory. DCIM with remote access via cellular or satellite backhaul. Alert escalation via SMS/email to central NOC. On-site intervention requires 4–24 hour response time — design for maximum self-healing
- Security: same physical security requirements apply regardless of size, but implementation adapts to footprint. Pre-assembled units include integrated badge reader, camera, and tamper detection
- Network connectivity: edge sites are bandwidth-constrained and latency-sensitive. Provision dual diverse fiber entrances (from different carriers). Use SD-WAN for WAN resilience
- Thermal management in outdoor/unconditioned spaces: outdoor edge cabinets must operate at -40°C to +55°C ambient. Heat load + ambient can exceed cooling capacity. Use active cooling (thermoelectric, Peltier, or refrigeration) with redundancy. Design for 100% load at 95th percentile outdoor temperature
- Flood and natural disaster resilience: edge sites in flood zones (coastal, near waterways) should be elevated above the 500-year flood level. In hurricane-prone areas, design to ASCE 7 wind load requirements (150+ mph for Category 4)
- Containment for edge: small contained footprint means thermal management is simpler but monitoring must be more granular — hot spots in a 10-rack facility can affect the entire load

### Power Infrastructure (Utility to Rack)

**Utility Feed and Substation:**

- Primary feed from the grid at medium voltage:
  - North America: 12.47kV, 13.2kV, 13.8kV, or 34.5kV. Stepped down to 480V/277V 3-phase 4-wire (wye) via pad-mounted or vault transformer
  - Europe (IEC HD 472): 10kV, 11kV, 20kV, or 30kV common. Stepped down to 400V/230V 3-phase 4-wire (TN-S system)
  - Asia (varies): 10kV, 22kV, 33kV common
- Frequency: 60Hz (North America, parts of Japan, Taiwan, Philippines, Saudi Arabia), 50Hz (Europe, Asia, Middle East, most of Latin America)
- Global voltage harmonization (IEC 60038): 230V/400V is the IEC standard replacing legacy 220V/380V and 240V/415V systems
- Redundancy options: single feed (Tier I–II), dual feeds from the same substation but independent distribution paths (Tier III), dual feeds from independent substations or utility feeds (Tier IV). True Tier IV requires the two feeds to be electrically independent — served from separate utility zones, each capable of carrying 100% of the critical load
- Substation lead time: 12–24 months from order to energization. This is often the critical path for greenfield builds. Plan transformer procurement in parallel with building construction — do not sequence them
- Interconnection agreement with the utility defines: available fault current, voltage regulation requirements, metering point, utility-owned vs. customer-owned equipment demarcation, and curtailment provisions (the utility's right to reduce your capacity during grid emergencies)
- Power factor correction: most utilities penalize power factors below 0.85–0.90 lagging. Data centers with large UPS systems and variable-speed drives typically have leading power factor at light load. Install automatic power factor correction (APFC) capacitors or specify UPS systems with active front-end (AFE) rectifiers that can manage power factor to 0.99 across the load range

**Switchgear and Distribution:**

- Main-tie-main (MTM) configuration: two main breakers (one per utility feed) with a normally open tie breaker between them. A utility feed failure causes the corresponding main to open and the tie to close, allowing the surviving feed to supply both switchboard sections. Transfer can be manual (open transition, 15–30 second outage while generators stabilize) or automatic (closed transition, seamless but requires generator synchronization)
- Automatic transfer switch (ATS): routes load between utility and generator. Types:
  - Open transition (break before make): the load is disconnected from one source before connecting to the other. Brief outage of 4–10 cycles (UPS covers this). Simpler, cheaper, more reliable
  - Closed transition (make before break): the ATS synchronizes the generator with the utility and connects both briefly (<100ms) before disconnecting the utility. Seamless but requires generator governor control and paralleling equipment. Adds ~$20K–$50K per ATS for medium-voltage gear
  - Bypass-isolation ATS: includes a manual bypass switch so the ATS can be isolated and maintained without powering down the load. Recommended for Tier III and above
- PDU configurations by region:
  - North America: 480V 3-phase input, 208V/120V 3-phase output. PDU sizes: 75 kVA (typical for 50–60 rack general-purpose pod), 150 kVA, 225 kVA, 300 kVA
  - EMEA/APAC: 400V 3-phase input, 230V/400V 3-phase output. At 400V, a 100 kVA PDU delivers ~80 kW (vs. ~52 kW at 208V for the same current) — higher voltage reduces current for the same power, enabling higher rack densities per circuit
- Remote power panels (RPPs): secondary distribution within the white space. Mounted at row ends or overhead. Feed branch circuits to individual racks via hardwired conduit or overhead busway. Typical RPP: 100–225A, 208V or 400V 3-phase, with 24–42 branch circuit breaker positions
- Static transfer switch (STS): for dual-corded equipment with 2N power. STS selects between two independent power sources (typically from two separate UPS systems) and switches within 4–8ms if one source fails. Allows the IT equipment to be truly dual-fed without requiring the equipment itself to handle failover. Each STS: 16–60A, single-phase or 3-phase

**Uninterruptible Power Supply (UPS):**

- Static double-conversion online UPS (VFI-SS-111 per IEC 62040-3): the standard for data centers. Rectifier converts AC to DC to charge batteries and feed the inverter. Inverter converts DC back to AC. The load is always powered through the inverter — never directly from utility. This provides perfect voltage and frequency regulation, isolation from utility transients, and zero transfer time to battery. Efficiency at full load: 94–97% depending on topology and manufacturer. Efficiency at <30% load: drops to 88–93% — right-size your UPS to operate above 40% load
- Battery technologies:
  - VRLA (valve-regulated lead-acid): 5–8 year design life at 25°C. 200–300 cycles to 80% depth of discharge. Cost: $150–$250/kWh. Temperature sensitivity: every 8°C above 25°C halves battery life. Must be operated at 20–25°C for rated lifespan. Heavy: ~3x the weight of Li-ion per kWh
  - Li-ion (LFP — lithium iron phosphate): 10–15 year design life at 25°C. 2000–5000 cycles to 80% DoD. Cost: $350–$500/kWh. Lighter, smaller footprint (50–70% less space than VRLA), wider operating temperature range (0–40°C), and higher round-trip efficiency (92–95% vs. 85–90%). The higher upfront cost is justified when: space is constrained, operating temperatures are elevated, or the battery system will be cycled frequently (peak shaving, grid services)
  - Flywheel (rotary UPS): kinetic energy stored in a spinning mass (steel or composite). No batteries. Runtime: 15–45 seconds at full load — just enough to cover generator start transient or bridge a momentary utility flicker. Compact, long lifespan (20+ years), no thermal management required. Cost: ~$200–$400/kW for the flywheel alone, but requires a larger upfront investment in generator capacity because the flywheel cannot sustain extended outages. Best suited for facilities with very frequent short-duration sags and flickers, or where battery maintenance is cost-prohibitive
- Runtime target: 5–15 minutes at full load for the battery system. The minimum runtime is dictated by generator start + stabilize + transfer time. A typical diesel generator starts within 10 seconds, but requires 15–45 seconds to stabilize voltage and frequency within ±2% before the ATS transfers. Add margin for cold-start delays, multiple start attempts, and ATS coordination
- UPS topology selection:
  - N+1 modular: multiple UPS modules (e.g., 4 × 250 kW) connected in parallel, with the "N+1" module providing redundancy. If any module fails, the remaining three carry the load at 100% capacity. Lowest cost, smallest footprint. Maintenance requires taking the failed module offline while the others continue — but this is maintenance on the UPS, not the load
  - 2N (two independent UPS systems): each UPS system carries 100% of the critical load, with the load distributed 50/50. Each UPS system is independent — separate input feeds, separate batteries, separate output distribution. Failure of an entire UPS system leaves 50% of the load without power — but the remaining 50% stays up because dual-corded equipment draws from the surviving system. Requires dual-corded IT equipment and STS where single-corded equipment is present. Cost: ~2x N+1
  - Distributed redundant (2N without buying two of everything): UPS A feeds half the facility; UPS B feeds the other half. Each UPS is sized at N+1 for its half. Each rack has dual power cords — one from A, one from B. If UPS A fails, all loads on A disappear, but every rack remains operational on B. The surviving UPS B must carry 100% of the facility load — verify this during commissioning
- UPS commissioning: every UPS installation must be load-banked to 100% of rated capacity for at least 30 minutes. Do not accept a UPS that has only been tested at partial load — manufacturing defects and installation errors often appear only at full load. Conduct a 30-minute battery discharge test at 100% load during commissioning

**Standby Generators:**

- Fuel type:
  - Diesel: standard for mission-critical. Energy density: ~37 kWh per US gallon (diesel). On-site storage: day tank (8–24h) + bulk storage tank. Bulk tank sizing: minimum 24–72 hours at full load is typical (Tier III minimum is usually defined by the operator, not by Uptime. Local fire codes may limit total on-site fuel storage to 48–72 hours without a fuel management plan)
  - Natural gas: used where firm gas supply contracts are available and diesel emissions are restricted. Requires firm (interruptible) gas service — non-firm gas can be curtailed during cold weather when the grid needs gas-fired generation most. Gas generator sizing: derate by 15–25% compared to diesel because of lower BTU content and pre-ignition risk
  - Dual-fuel (diesel + natural gas): diesel pilot injection with natural gas as primary fuel. Provides fuel flexibility but adds complexity. Gas supply failure automatically transitions to 100% diesel. Used primarily in locations where diesel deliveries may be interrupted
- Generator sizing: must carry the critical load plus mechanical cooling plus essential lighting plus battery charging. The critical IT load is only 60–70% of the generator capacity requirement — the rest goes to compressors, chilled water pumps, condenser fans, cooling tower fans, and lighting. Rule of thumb: generator kW = IT load kW × 1.4 to 1.6 depending on cooling plant efficiency
- Sizing for large motor loads: the largest motor starting inrush (typically a chiller compressor) can draw 6–8x its running current for 1–5 seconds. The generator must be sized to handle this inrush without voltage dip exceeding 20% of nominal. If a 200 kW chiller has 7x inrush, its starting surge is 1400 kW. Size the generator to deliver this surge current even if the steady-state load is much lower. Auto-transformers or soft-starters reduce inrush — specify them on all motors >50 kW in generator-backed applications
- Generator testing protocol:
  - Weekly: run for 20–30 minutes at no load to exercise the engine, check for leaks, confirm auto-start
  - Monthly: run under load for 1–2 hours. Use a resistive load bank if the building load alone is insufficient. Running a generator at <30% load for extended periods causes wet stacking — unburned fuel and carbon deposits accumulate in the exhaust system, reducing power output and causing exhaust valve sticking. A load bank test at 50–75% load for 2 hours every 3 months burns off wet stack deposits
  - Annually: full-load test for 4+ hours with load bank. Verify fuel consumption rate, exhaust emissions, cooling system capacity, and automatic transfer switch coordination
- Fuel logistics: during a multi-day outage, your generator is only as good as your fuel supply. Key questions: Who refuels? Is there a contract in place? How many gallons per hour at full load? How close is the nearest fuel depot that has backup power (depots without power cannot pump fuel)? Fuel polishing: diesel fuel stored for >6 months grows microbial contamination (diesel bug) that clogs filters and injectors. Test stored fuel quarterly. Polish (filter + biocide treatment) annually or when contamination exceeds ISO 4406 cleanliness code 18/16/13
- Exhaust: diesel exhaust contains NOx, CO, and particulate matter. Exhaust must be routed to a safe discharge point away from building air intakes, generator intake louvers, and adjacent properties. Local air quality regulations (EPA, local AQMD) may require exhaust after-treatment (SCR for NOx, DPF for particulate). Generator exhaust backpressure must not exceed the manufacturer's maximum — excessive backpressure reduces power output and can cause valve damage

**Rack-Level Power Distribution:**

- Standard: 208V 3-phase (North America) or 400V 3-phase (EMEA, APAC, IEC region). The higher voltage reduces current for the same power, allowing smaller conductors and higher power density per circuit
- Circuit sizing (North America — NEC):
  - 30A 3-phase @ 208V = ~8.6 kW usable (80% derated: ~6.9 kW continuous)
  - 60A 3-phase @ 208V = ~17.3 kW usable (80%: ~13.8 kW continuous)
- Circuit sizing (IEC/EMEA):
  - 32A 3-phase @ 400V = ~22.2 kW usable (80%: ~17.7 kW continuous)
  - 63A 3-phase @ 400V = ~43.7 kW usable (80%: ~35.0 kW continuous)
- Always derate branch circuits to 80% of breaker rating for continuous load (NEC Article 100 / IEC 60364). A 60A circuit carries no more than 48A continuous. Plan your per-rack power budget against the derated value, not the breaker rating
- Busway vs. hardwired PDU:
  - Busway (overhead busbar trunking): a prefabricated busbar system mounted overhead (or under raised floor). Tap-off boxes can be inserted or removed at any position along the busway, allowing rack power capacity to be reconfigured without an electrician. Essential when rack densities are variable or will grow over time. Initial cost: 1.5–2x hardwired in low-density deployments. Payback period: typically 3–5 years if >20% of racks are reconfigured during that period
  - Hardwired PDU (branch circuits from RPP to rack): lower initial cost. Each circuit is a dedicated conduit whip. Reconfiguration requires an electrician to run a new circuit. Better suited for stable deployments where rack density is known and will not change significantly
- Power monitoring: every PDU, RPP, busway tap-off, and rack PDU should have revenue-grade metering (±0.5% accuracy or better) with individual outlet-level monitoring for high-density racks. Data feeds into DCIM for capacity planning, efficiency analysis, and power path visualization. Without monitoring at the rack level, you cannot detect phase imbalances, overloaded circuits, or stranded capacity
- Single-corded vs. dual-corded equipment:
  - Dual-corded: two independent power supplies, each connected to a separate power source (A feed and B feed). If one source fails, the equipment continues on the other. Mandatory for Tier III and IV deployments. Single-corded equipment creates a single point of failure and should be avoided in any redundant power path
  - Single-corded equipment (legacy, smaller network switches): requires a static transfer switch (STS) to provide dual-source redundancy. The STS monitors both feeds and switches in <8ms if the primary fails. Do not daisy-chain STS units — each single-corded device gets its own STS or the STS serves a small group (<4 devices) with individual output breakers

**Grounding and Bonding:**

- Equipment grounding conductor (EGC): per NEC Article 250 (North America) or IEC 60364-4-41 (international), provides a low-impedance path for fault current to clear overcurrent devices. Sized per NEC table 250.122 or IEC Table 54. For a 60A circuit, minimum 10 AWG copper EGC (NEC) or 6 mm² copper (IEC)
- Bonding conductor: connects metallic, non-current-carrying parts of the electrical installation (enclosures, raceways, cable tray) together and to ground. Ensures no voltage potential exists between accessible conductive surfaces — critical for personnel safety and equipment protection
- Signal reference grid (SRG): a grid of bare copper conductors (typically #6 AWG or #4 AWG / 16mm² or 25mm²) installed under the raised floor in a 600mm × 600mm grid pattern, bonded to the building steel, cable trays, rack bonding, and the main electrical ground. Provides an equipotential plane for all IT equipment — eliminating ground potential differences that cause data transmission errors and equipment damage during lightning events or fault conditions. The SRG is not an equipment ground — it is a signal reference. The two systems are bonded at exactly one point
- Rack bonding: each rack must be bonded to the SRG with a minimum #6 AWG copper conductor (16mm²). Use a listed bonding lug at the rack base, bonded to the rack frame. Paint must be removed under the lug contact area — powder-coat is an insulator. Verify bond resistance: <0.1 ohm from rack frame to SRG
- Telecom grounding (TIA-607-B / IEC 607-B): telecommunications grounding busbar (TGB) in each telecommunications room (TR) or equipment room (ER). Bonded to the building service ground via the telecommunications bonding backbone (TBB). All telecommunications equipment and cable racks in the TR bond to the TGB. The TBB is typically a #6 AWG or larger copper conductor run in a dedicated conduit to the main telecommunications bonding conductor (TMGB) at the entrance facility (EF)
- Ground loop prevention: single-point ground at the main service entrance (NEC Article 250.30 / IEC 60364). All other grounds (building steel, SRG, telecom ground, lightning protection) bond at exactly one point. Multiple ground paths create ground loops — circulating currents that cause data errors, equipment hum, and corrosion

### Cooling Systems

**ASHRAE Thermal Standards (TC 9.9):**

| Class | Dry-Bulb Temp | Humidity Range | Max Dew Point | Best For |
|-------|--------------|---------------|--------------|----------|
| A1 | 15–32°C (59–90°F) | 20–80% RH, 5.5°C DP min | 17°C DP | Enterprise servers, storage |
| A2 | 10–35°C (50–95°F) | 20–80% RH | 21°C DP | Volume servers, some storage |
| A3 | 5–40°C (41–104°F) | 8–80% RH | 24°C DP | Extended range (edge, industrial) |
| A4 | 5–45°C (41–113°F) | 8–80% RH | 26°C DP | Extended range (ruggedized) |
| H1 | 15–32°C | 20–80% RH | 17°C DP | High-density (liquid assist) |

- Operating at higher supply temperatures (22–27°C instead of 15–18°C) reduces cooling energy by 3–5% per °C of temperature increase. Most modern enterprise equipment is rated for A1 or A2 — check the manufacturer's published environmental specifications before raising temperature
- Humidity: low humidity (<20% RH) increases electrostatic discharge (ESD) risk. High humidity (>80% RH) increases corrosion risk and can cause condensation on cold surfaces. Target: 40–60% RH at the rack inlet. Humidification and dehumidification are energy-intensive — avoid tight humidity bands unless required by equipment specs

**Air Cooling:**

- CRAC (computer room air conditioner) — direct expansion (DX): refrigerant-based cooling. Compressor inside the unit. Less efficient than chilled water. Typical EER (energy efficiency ratio): 10–14 at design conditions. Better suited for smaller facilities (<500 kW IT load) or retrofit where chilled water piping is unavailable
- CRAH (computer room air handler) — chilled water: chilled water from the central plant circulates through cooling coils in the CRAH unit. Cold water supply temperature is set to avoid condensation (typically 45–55°F / 7–13°C). Requires a chiller plant but provides 15–35% lower cooling energy than DX
- Raised floor cooling: CRAH/CRAC units push cold air into the underfloor plenum, which pressurizes to ~0.05–0.10 inches of water column (12–25 Pa). Air exits through perforated tiles in the cold aisle. Perforated tile airflow: 200–600 CFM per tile depending on opening percentage (25–60% open area). Pressure must be balanced — too low and end-of-row racks get no airflow; too high and air escapes through every cable cut-out and tile gap
- Perforated tile placement: tiles should be placed only in cold aisles, not in hot aisles. Typical: every other tile in the cold aisle is perforated for 5–8 kW/rack. For 8–15 kW/rack, every tile is perforated. Beyond 15 kW/rack, supplemental cooling (rear-door HX, in-row coolers, or liquid cooling) is required
- In-row cooling: cooling units mounted between racks within the row. Draw hot air from the hot aisle, cool it, and discharge cold air directly into the cold aisle. Higher efficiency than perimeter CRAH because air path is shorter and fan power is lower. Handles 15–30 kW/rack. Requires floor space that would otherwise hold IT racks — typically consumes every 4th or 5th rack position in the row
- Overhead air distribution: ducted supply air from overhead diffusers into the cold aisle. Increasingly common in slab-on-grade facilities without raised floors. Ductwork must be sized for low velocity (<500 fpm / 2.5 m/s) to minimize pressure drop and fan energy
- Economization (free cooling):
  - Air-side economizer: outside air is filtered, humidified (or dehumidified) if needed, and supplied directly to the data center. When outside air is cool enough (typically below 65°F / 18°C), compressors or chillers can be turned off. Saves 40–70% of cooling energy in temperate climates. Risks: outdoor air quality (particulates, humidity control, gaseous contaminants — verify local air quality before implementing)
  - Water-side economizer: when outside wet-bulb temperature is low enough, the cooling tower or dry cooler can provide chilled water directly to the CRAH coils without operating the chiller. The strainer or plate-frame heat exchanger isolates the facility loop from the cooling tower loop to prevent fouling. Typically saves 30–60% of chiller energy
  - Economizer hours: a facility in Chicago might have 4,000+ hours/year of economizer operation (>45% of annual hours). A facility in Phoenix might have 1,500 hours/year. Evaluate economizer ROI based on local climate data (TMY3 or TMY4 weather files)

**Liquid Cooling:**

- Direct-to-chip (cold plate): coolant flows through cold plates mounted directly on CPUs, GPUs, and memory. Removes 60–80% of the heat via liquid — the remaining 20–40% must still be handled by air cooling (power VRMs, NICs, drives, peripheral components). Handles 30–100+ kW/rack. Coolant temperature: 35–50°C (supply), 45–65°C (return). Higher temperatures enable more hours of economizer operation
- Coolant distribution unit (CDU): pumps coolant from the facility water loop through the IT equipment. The CDU contains a pump, heat exchanger, expansion tank, filters, and leak detection. Facility-side water (building chilled water or cooling tower water) flows through one side of the heat exchanger; IT-side coolant flows through the other. The two loops are isolated — the IT-side coolant is typically deionized water with corrosion inhibitors and biocide. Coolant types: dielectric fluid (fluorocarbon or synthetic hydrocarbon) or treated water. Water has 3–5x better heat transfer than dielectric fluid but is electrically conductive — leak detection and containment are critical
- Rear-door heat exchangers (RDHx): passive or active (fan-assisted) coils mounted on the rear door of the rack. Chilled water flows through the coils and absorbs heat from the server exhaust air. Handles 15–35 kW/rack. Does not require server modification — works with standard air-cooled servers. Retrofit-friendly. Passive RDHx relies on the server fans to push air through the coils; active RDHx adds fans for additional airflow. Active RDHx handles higher densities but adds fan power and maintenance
- Immersion cooling: servers or entire racks submerged in dielectric fluid. Two types:
  - Single-phase: fluid stays liquid. Heat is transferred to the fluid, then pumped through a heat exchanger to reject heat to the facility loop. Coolant temperature: 40–55°C. High efficiency (pump power only, no fans). Servers must be designed for immersion (no spinning disks, no fans, special power connectors). Example: 50–200+ kW/rack. Fluid cost is significant — 50–100 gallons per server at $100–$300/gallon for engineered dielectric fluids
  - Two-phase: fluid boils at the heat source temperature (typically 50–60°C), and vapor rises to a condenser coil at the top of the tank where it condenses back to liquid. Higher heat transfer coefficient than single-phase but tighter temperature control requirements. Vapor escape and fluid loss are concerns — dielectric fluid is expensive and some formulations have high global warming potential (GWP)
- Facility water loop design for liquid cooling:
  - Supply water temperature: higher is better for efficiency (more economizer hours). Target: 65–85°F (18–29°C) depending on IT equipment coolant temperature requirements
  - Water quality: closed loops should use deionized or reverse-osmosis treated water with corrosion inhibitors (typically azoles for copper protection) and biocide. Monitor conductivity, pH, and corrosion coupon weight loss monthly
  - Leak detection: every coolant connection should be within a drip tray or have a humidity/leak sensor beneath it. Rack-level leak detection: water-sensing cable routed under each rack in the liquid-cooled zone, connected to a building management system (BMS) alarm. Facility-level: flow meters and pressure sensors detect sudden pressure drops indicating a line break — automatic shutoff valves should isolate the affected section

**AI/GPU Cluster Infrastructure:**

- Modern GPU clusters (NVIDIA H100, B200, AMD MI300X) require 30–120 kW per rack, far exceeding air cooling capacity. Direct liquid cooling (DLC) is mandatory for deployments >30 kW/rack
- GPU server form factors:
  - HGX H100/H200: 8× H100 SXM5 GPUs, ~10 kW per rack unit (typically 4U server). 8-GPU server draws 8–10 kW at full load
  - GB200 NVL72: 72-GPU rack-scale system drawing 60–120 kW. Requires direct liquid cooling at the rack level
  - NVIDIA DGX H100: 8× H100 in a 10U chassis, ~10 kW
  - DGX B200: 8× B200, ~14 kW
  - AMD MI300X: 8× MI300X in a 10U chassis, ~12 kW
- InfiniBand networking for GPU clusters:
  - NVLink allows GPU-to-GPU communication within a server at 900 GB/s (H100 SXM5)
  - InfiniBand NDR 400 (400 Gb/s per port) connects servers in a cluster. Use spine-leaf topology for >100 servers
  - Cable distances: OM4 fiber up to 100m for 400G NDR, active optical cables (AOC) for longer runs up to 500m. Beyond 500m: coherent optics or DWDM
  - Fat-tree network topology is standard for AI training clusters — oversubscription ratio of 1:1 between GPUs
- Direct Liquid Cooling (DLC) for GPU racks:
  - Cold plates mounted directly on GPUs, CPUs, and memory. Heat flux from GPU die: 100–200 W/cm² — air cooling cannot keep up
  - Coolant: treated water with corrosion inhibitors is most common (lower cost, higher heat transfer than dielectric). Some deployments use dielectric fluid for added safety
  - CDU sizing: each HGX/H100 server requires ~2–3 GPM of coolant flow at 10–15°C ΔT. A full rack of 8 servers needs 16–24 GPM per rack
  - Facility loop design: dedicated liquid cooling plant with heat exchangers to the building cooling loop. Never mix GPU cooling loops with chilled water loops — temperatures and pressures are incompatible
  - Manifold design: rack-level supply and return manifolds with ball valves for isolation. Each server connects via quick-disconnect fittings
  - Redundancy: N+1 CDU configuration. Each CDU sized to carry the full rack heat load. If one CDU fails, the rack must shed load or the remaining CDUs carry 100%
- Thermal considerations for GPU cooling:
  - GPU junction temperature: max 83°C (H100), 81°C (B200). Maintain coolant supply temp 5–10°C below GPU max for 10–15°C ΔT across the cold plate
  - Condensation risk: if coolant supply is below room dew point, insulation and humidity control are mandatory on all coolant piping in the white space
  - Heat reuse: GPU exhaust heat (45–65°C return coolant) can be used for building heating, domestic hot water, or absorption cooling. ROI analysis should account for displaced natural gas or heating oil
- Rack structural requirements for GPU clusters:
  - Fully loaded GPU rack: 1500–2500 kg. Verify floor loading (point load from leveling feet). Use load-spreading plates
  - Rack depth: 1200mm minimum for GPU servers with rear-door heat exchangers. Some GPU servers require 1400mm depth
  - Power distribution: 400V 3-phase 63A feeds deliver ~35 kW. GPU clusters typically require multiple 63A feeds per rack. Plan for 2–4 feeds per high-density rack
  - Busway is strongly recommended for GPU clusters — density changes as GPU technology evolves, and busway allows reconfiguration without electrician labor

**Cooling Plant:**

- Chiller types:
  - Air-cooled chillers: packaged units with condenser coils and fans. No cooling tower needed. Higher condensing temperature (110–125°F / 43–52°C) → lower efficiency (COP 2.5–3.5 at design). Simpler maintenance, lower installed cost, no water consumption. Better for small to medium facilities (<1 MW IT load) or where water is scarce
  - Water-cooled chillers: centrifugal compressors with cooling tower rejection. Lower condensing temperature (75–95°F / 24–35°C) → higher efficiency (COP 5.5–7.0 at design). Higher installed cost, requires cooling tower, water treatment, and freeze protection. Better for medium to large facilities (>1 MW IT load)
  - Adiabatic cooler: dry operation most of the year. On hot days, water is sprayed on the coil surface — evaporation cooling improves heat rejection without a full cooling tower. COP: 3.5–5.0. Water consumption: 90% less than evaporative cooling tower. Good compromise where water is limited but efficiency is important
- Cooling tower configuration:
  - Open loop (evaporative): water cascades over fill material while air is drawn through. Some water is lost to evaporation (~1.8 gallons/ton-hour) and blowdown (~0.5 gallons/ton-hour). Lowest leaving water temperature. Requires water treatment to prevent scale, corrosion, and Legionella. Chemical feed and blowdown control are mandatory
  - Closed loop (dry cooler or closed-circuit cooling tower): the facility water flows through a sealed coil, and air or spray water cools the coil. No direct contact between facility water and outside air. Less water treatment required but higher leaving water temperature (2–3°C higher than open tower at design)
- Piping considerations: supply and return piping from chiller plant to CRAH units. Typically steel or copper. For liquid cooling facility loops: stainless steel or HDPE (polyethylene) is preferred for corrosion resistance. Insulation thickness: minimum 1 inch (25mm) for chilled water <15°C, thicker in high-humidity environments to prevent condensation drip
- N+1 chiller configuration: if design requires 3 chillers for full load, install 4. Any single chiller can be serviced or fail without reducing cooling capacity. Chiller lead time: 20–40 weeks depending on size and configuration

### Fire Suppression and Detection

**Early Warning Detection:**
- VESDA (Very Early Smoke Detection Aspirating): air sampling systems that detect combustion products at concentrations 100–1000× lower than conventional spot detectors. Essential for raised-floor plenum returns and hidden spaces (above suspended ceilings, in cable trays). Sensitivity: detection of combustion at 0.005–0.5% obscuration/ft. Required by NFPA 72 for data centers >500 sq ft
- Smoke detection in contained aisles: HAC complicates detection because smoke may not reach general area detectors. Aspirating detectors mounted in the return air path of contained hot aisles are mandatory
- Thermal imaging cameras (standalone or integrated with BMS): detect hot spots in electrical gear or cable bundles before they reach ignition temperature. Use for transformer bushings, switchgear, and cable tray monitoring

**Clean-Agent Fire Suppression Systems:**
- Agent selection criteria: extinguishment efficiency, safety (occupant exposure, ozone depletion potential, GWP), availability, and cost
- FM-200 (HFC-227ea): GWP 3220, ODP 0, 1.0× minimum design concentration for Class A, 1.25× for Class B. Discharge time: <10 seconds. Storage: cylinders at 360 psi. EPA SNAP approved for occupied spaces. Effective on Class A, B, C fires
- Novec 1230 (2-BTP): GWP 1, ODP 0, 1.0× minimum design concentration. Discharge time: <10 seconds. Storage: cylinders at 360 psi. Lower toxicity than FM-200. Preferred for new installations where GWP regulations apply
- Inert gas (IG-541, IG-55): mixture of nitrogen, argon, CO2. GWP = 0, ODP = 0. Works by reducing oxygen concentration to <15% (vs. normal 21%). Requires sealed enclosure (12× more agent by volume than chemical agents). Larger cylinder storage footprint. No residue — preferred for areas where cleanup is difficult
- FE-25 (HFC-125): GWP 3500, used in some hybrid systems. Being phased out under EPA SNAP regulations

**HAC-Specific Considerations:**
- Hot aisle containment (HAC) creates a challenge: the contained volume must be protected, but clean-agent dispersal in a sealed hot aisle takes longer and may not reach all ignition points before flashover occurs
- NFPA 75 requires smoke detection to initiate agent release within 40 seconds of ignition. HAC密封 areas must account for this in detection zonal layout
- Personnel egress from contained aisles during a fire event: HAC design must include emergency release mechanisms that open the aisle within 3 seconds of agent discharge signal
- Pre-action systems are preferred in HAC environments: sprinkler piping is dry until dual confirmation (smoke detection + heat) prevents accidental water discharge during maintenance

**Water-Based Suppression:**
- Pre-action sprinkler (deluge): pipes are dry until fire is confirmed by VESDA or spot detectors. Prevents accidental discharge from false alarms or maintenance activities. Required in white space per NFPA 13
- ESFR (Early Suppression Fast Response) sprinklers: for facilities with high ceilings (>4.5m). Faster response than standard sprinklers. Less water required for suppression
-Gypsum wallboard or rated ceilings provide 2–4 hour fire separation between zones
- Foam suppression for generator fuel storage areas: Class B fire suppression (flammable liquids). Required for any bulk fuel storage >500 gallons

**Leak Response for Liquid Cooling Environments:**
- Immersion cooling fluids (dielectric): non-conductive, but pooling creates slip hazard and can damage equipment through pooling. Use absorbent spill kits (polypropylene pads) rated for dielectric fluids
- Water-glycol CDU leaks: conductive coolant. Treat as electrical hazard — de-energize affected equipment before cleanup. Contain with oil-water separator or absorbent booms
- All white space areas with liquid cooling must have floor drains or trench drains connected to containment tanks to prevent coolant from reaching electrical equipment

**Commissioning and Testing:**
- Flow test: verify sprinkler discharge pattern and pressure at the hydraulically most remote area. Required before occupancy
- Clean-agent system test: verify cylinder pressure, discharge time, and distribution uniformity. Agent must reach all areas within the design concentration within 10 seconds
- VESDA system calibration: test with aerosol smoke at each sampling point. Verify correct alarm thresholds
- Annual inspection: all suppression systems must be inspected by a licensed fire protection contractor per NFPA 25 (inspection, testing, and maintenance of water-based fire protection systems)

### Efficiency (PUE)

**PUE Measurement and Methodology:**

- PUE = Total Facility Energy ÷ IT Equipment Energy
- Total facility energy includes: IT equipment + cooling plant + UPS losses + power distribution losses + lighting + security + all facility loads
- IT equipment energy includes: servers, storage, network equipment, and any device that processes, stores, or transmits data. Does NOT include cooling, power distribution, or lighting
- PUE is not meaningful as a single number — it varies with load level, outside temperature, and time of day. Report PUE as:
  - Annualized PUE (most common): total energy over 12 months ÷ total IT energy over 12 months. Smooths seasonal variation
  - Quarterly PUE: useful for tracking improvement programs
  - Partial PUE (pPUE): measures a subset of the facility (e.g., a specific pod or zone)
- Measurement best practices (Green Grid):
  - Meter every PDU and HVAC unit at a minimum
  - Revenue-grade meters (±1% accuracy) at the utility entrance and at every UPS output. ±2% accuracy for downstream monitoring
  - Log data at 15-minute intervals. Hourly averages miss peaks
  - Report PUE at various load levels — 1.3 at 80% load and 1.6 at 30% load are both normal. A PUE of 1.4 "reported" without load context is meaningless
- Common PUE manipulation: turning off unused cooling units, raising temperature, and disabling redundant UPS modules when measuring — but operating with full redundancy during normal operation. Honest PUE reporting includes full redundancy operation

**PUE Optimization Strategies (by impact):**

| Strategy | Typical PUE improvement | Capex | Implementation |
|----------|------------------------|-------|----------------|
| Cold aisle containment | 0.10–0.20 | Low | Seal aisles, add doors/panels |
| Raise supply air temperature (18°C → 24°C) | 0.05–0.10 | None | Adjust CRAH setpoints |
| Economizer (air-side or water-side) | 0.10–0.25 | Medium | Install dampers, controls, heat exchanger |
| Variable-speed drives on fans/pumps | 0.05–0.15 | Medium | VFD installation and controls |
| UPS efficiency optimization (right-size, ECO mode) | 0.02–0.05 | Low | Configuration change, module addition |
| LED lighting with occupancy sensors | 0.01–0.02 | Low | Retrofit |
| Hot aisle containment | 0.05–0.10 | Low-Medium | Similar to CAC but on hot aisle |
| Liquid cooling (direct-to-chip) | 0.10–0.30 | High | CDU, piping, facility loop |
| Power capping on idle servers | 0.02–0.10 | None | Software policy, DCIM integration |
| Turn off zombie servers (<5% utilization for >30 days) | 0.02–0.08 | None | Asset management, workload consolidation |

- Rule of thumb: every 1°C increase in supply air temperature reduces cooling energy by 3–5%. CRAH fan power: reducing fan speed by 10% reduces fan power by ~27% (affinity laws: power ∝ speed³). Most CRAH/CRAH units operate at 60–80% fan speed with proper containment — reducing from 80% to 60% saves ~58% of fan energy

**PUE Targets by Facility Type:**

| Facility Type | Realistic PUE Target | World-Class |
|--------------|---------------------|-------------|
| Colocation (multi-tenant, 1+ MW) | 1.3–1.5 | <1.2 |
| Enterprise (private, 1–10 MW) | 1.4–1.6 | <1.3 |
| Hyperscaler (50+ MW) | 1.1–1.3 | <1.1 |
| Edge / small room (<100 kW) | 1.5–2.0 | <1.5 |
| GPU/AI cluster (30+ kW/rack, liquid) | 1.05–1.15 | <1.05 |

- PUE below 1.0 is physically impossible (IT energy cannot exceed facility energy). Any claim of PUE <1.0 is a measurement error or exclusion of critical loads

### Sustainability Metrics

**Water Usage Effectiveness (WUE):**
- WUE = Annual Water Usage (L) ÷ IT Equipment Energy (kWh)
- Measures liters of water per kWh of IT load
- Air-cooled chillers: ~0.5–1.0 L/kWh (evaporative losses only)
- Water-cooled chillers: ~1.5–3.0 L/kWh (evaporation + blowdown)
- Direct evaporative cooling: ~3.0–5.0 L/kWh
- Cooling tower makeup: ~1.8 gallons/ton-hour (evaporation) + 0.5 gallons/ton-hour (blowdown)
- Water-stressed regions: target WUE <0.5 L/kWh. Use adiabatic coolers, dry coolers, or air-side economizers to minimize water consumption
- Zero-water cooling systems (closed-circuit dry coolers): WUE = 0, but higher leaving water temperatures reduce chiller COP

**Carbon Usage Effectiveness (CUE):**
- CUE = Total CO2 Emissions (kg) ÷ IT Equipment Energy (kWh)
- Scope 2 emissions: electricity purchased from grid
- Scope 1 emissions: on-site fuel combustion (generators, boilers)
- Grid carbon intensity (kg CO2/kWh): varies by region and time of day
  - US average grid: ~0.4 kg/kWh (varies by state: WA ~0.1, CA ~0.2, TX ~0.4, Midwest ~0.7)
  - EU average: ~0.25 kg/kWh (varies: France ~0.06, Germany ~0.4, Poland ~0.7)
  - Renewable-heavy grids: <0.05 kg/kWh
- Target: CUE <0.5 kg/kWh for grid-powered facilities. Facilities with 100% renewable PPAs can achieve CUE <0.1 when accounting for RECs
- CUE calculation must use location-based accounting (actual grid emission factor), not contract-based (pp. 148-151 of GHG Protocol Scope 2 Guidance)

**Global Warming Potential (GWP) of Coolants:**
- Refrigerants and dielectric fluids have GWP values (CO2 equivalent per kg)
- HFC refrigerants (R-134a, R-407C): GWP 1300–2300
- HFO refrigerants (R-1234yf): GWP <10
- Novec 1230 (2-BTP): GWP = 1
- Natural refrigerants (ammonia R-717): GWP = 0
- Immersion cooling fluids: varies widely (mineral oil GWP ~5, synthetic dielectrics GWP 100–1000)
- Phase-down schedules: EPA AHRI requiring HFC refrigerant GWP <750 by 2030 for new installations
- Select low-GWP refrigerants for new cooling plant specifications to future-proof against regulations

### Equipment Installation

**Rack Preparation:**

- Verify floor loading capacity for the planned equipment. A fully populated 42U rack with 2 kW average per server and 20 servers weighs approximately 600–900 kg. For high-density with GPU servers: 1000–1500 kg per rack. Compare to the raised floor rated load (typically 7.5–12 kN/m² = ~760–1220 kg/m²). A rack footprint of 0.6m × 1.07m = 0.64 m². A 1200 kg rack creates a point load of ~1875 kg/m² — potentially exceeding floor rating. Use load-spreading plates under rack leveling feet if point loads exceed the floor specification
- Leveling: use a precision level on every rack. Adjacent racks in a row must be aligned at the top and front — misaligned racks create gaps in containment seals and make cable management difficult. Shim as needed under the base frame, not under individual leveling feet
- Bonding: install the bonding conductor from the rack to the SRG before placing any equipment. Torque to specification. Verify bond resistance with a micro-ohmmeter (<0.1 ohm). Label the bonding conductor at both ends with the rack ID

**Power Cabling:**

- Phase balancing: distribute rack loads evenly across L1, L2, and L3 phases of the 3-phase circuit. An imbalanced phase (e.g., L1 = 8 kW, L2 = 4 kW, L3 = 4 kW) overheats the neutral conductor (which carries the imbalance current in a wye system). Use the PDU metering or a clamp meter to verify phase balance before commissioning. Target: phase imbalance <10% between any two phases
- Circuit labeling: every breaker in the RPP, every whip, every PDU input must be labeled with the circuit ID and the equipment it feeds. Use a standard format: RPP-A1-CKT-03. Labels must be machine-generated (Dymo or Brady), not handwritten. Handwritten labels fade and become illegible within 2–3 years
- Conductor sizing: always size conductors for the breaker rating, not the expected load. A 60A circuit requires 6 AWG copper (75°C column per NEC 310.16) minimum, regardless of whether the connected load is 5 kW (~14A). This allows future upgrades without re-cabling
- Color coding (North America, per NEC 210.5(C) for multi-branch circuits):
  - Phase A: Black
  - Phase B: Red
  - Phase C: Blue
  - Neutral: White or Gray
  - Ground: Green or bare
- Whip management: secure power whips to cable tray or ladder rack with velcro straps (not zip ties — zip ties overtighten and damage insulation). Maintain minimum bend radius: 5× cable diameter for power cables. Do not run power cables parallel and in contact with signal cables — maintain 6-inch (150mm) separation for <480V power to minimize electromagnetic interference (EMI)

**Structured Cabling (TIA-942 / BICSI):**

- Fiber backbone: OM4 (50/125 multimode, 470 m at 10 Gb/s, 150 m at 40/100 Gb/s) or OM5 (50/125 wideband multimode, similar distances but supports SWDM). For distances >150m or future 400 Gb/s+: OS2 single-mode (bend-insensitive). Pre-terminated trunk cables (12–48 fiber count) with MTP/MPO connectors speed deployment and reduce termination errors. Do not exceed 0.75 dB insertion loss per mated pair for multimode, 0.5 dB for single-mode
- Copper horizontal: Category 6A (Augmented Category 6) shielded (F/UTP or S/FTP) for 10GBASE-T, 90m channel limit (including 5m patch on each end). Shielded cabling is recommended in data centers because of EMI from adjacent power cables and equipment — the shield must be properly grounded at both ends. Do not exceed 23 AWG for permanent link cables
- Top-of-rack (ToR): each rack has its own access switch. Cables run from the switch to servers within the rack (short, 1–5m). Cables from ToR switches to the aggregation/core layer are fiber uplinks. ToR reduces cable volume but creates more switch points and more inter-switch cabling
- End-of-row (EoR): all servers in a row cable back to patch panels and switches in a dedicated row-end cabinet. Longer cable runs (up to 90m for copper, hundreds of meters for fiber). Fewer switches but more complex cable management. EoR is preferred for environments with frequent adds/moves/changes because patching happens at the EoR cabinet, not in the individual rack
- Structured cabling pathway rules:
  - Horizontal cable tray: 300mm (12-inch) minimum width per 24 racks. Deeper for higher density. Overhead tray is strongly preferred over underfloor — underfloor cables block airflow and make cleaning impossible
  - Maintain 300mm (12-inch) minimum separation between power and data cable trays. Where they must cross, do so at 90°. Never run data cables in the same tray as power cables
  - Cable tray fill: maximum 50% fill for ladder tray, 40% for basket tray, per TIA-569-D
  - Bend radius protection: use corner guides or sweep fittings at all tray turns. Minimum bend radius: 10× cable diameter for copper (4× for patch cords), 10× for fiber

**Labeling Standards:**

- Every cable, every port, every power outlet, every breaker, every rack must be labeled. Follow TIA-606-B (data center administration standard) for labeling format
- Rack labeling: top front and top rear of each rack, plus inside the rack at each PDU. Format: "DC1-ROW3-RACK042" or similar site-specific scheme
- Cable labeling: both ends. Format: "SRV-A03-U12-NIC1 → SW-ROW3-PORT24" — source device, port, destination device, destination port. Use self-laminating labels (clear wrap over printed area to protect the ink)
- Label material: polyester (Mylar) for long life. Paper labels absorb humidity and become unreadable within months. Labels in cold aisles (warmer, potentially humid) require more aggressive adhesion

**Commissioning:**

- Factory witness testing (FWT): for major equipment (UPS, generators, chillers, switchgear). Witness the factory test of each unit before shipment. Verify all performance parameters (capacity, efficiency at various loads, harmonic distortion, transient response). Fix defects at the factory — finding them at the site means long delays for replacement parts
- Installation verification: verify every connection, every label, every torque spec against the approved shop drawings and submittals. Use a torque wrench for all bolted electrical connections (lugs, bus bars, breakers). Under-torqued connections overheat; over-torqued connections damage the conductor or lug. Follow manufacturer torque values
- Start-up and site acceptance testing (SAT):
  - Megger (insulation resistance) test on all power cables before energization: 500V for 300V rated cable, 1000V for 600V rated cable. Minimum acceptable: 1 MΩ. New cable should read well above 100 MΩ. Record all values
  - HIPOT (high-potential) test on medium-voltage feeders per NETA ATS-2019 standard
  - Thermal imaging of all energized power connections within 72 hours of energization. Any connection showing ΔT >10°C above ambient (when loaded to >50% of rated capacity) must be re-torqued and re-imaged
- Integrated systems test (IST): simulate utility failure by opening the main breaker. Verify: generator auto-start within 10 seconds, ATS transfer within 10–15 seconds (or closed transition), UPS accepts the generator power and stabilizes within tolerances (±2% voltage, ±0.5% frequency). All transfer must be seamless to critical load. Repeat for each redundant path
- Load bank testing: apply a load bank to each UPS and generator at 100% of rated capacity for 30 minutes minimum. Verify: no overheating, stable voltage and frequency, battery discharge time meets specification, generator fuel consumption rate within spec. For generators, the load bank test also burns off any residual wet stack deposits from low-load operation during construction
- Documentation deliverable: as-built drawings (power single-line, cooling P&ID, floor plan with rack elevations, cable schedule, grounding diagram), equipment submittals with factory test data, commissioning test reports with all measured values, and facility operations manual with procedures for every maintenance task

### DCIM and BMS Integration

**Data Center Infrastructure Management (DCIM):**
- DCIM platforms (Schneider Electric EcoStruxure, Siemens Desigo CC, Vertiv Geist, Sunbird DCIM, nlyte) integrate power monitoring, cooling monitoring, asset management, and capacity planning into a single pane of glass
- Essential DCIM capabilities for critical infrastructure:
  - Real-time power metering at every level: utility entrance, UPS output, PDU input/output, RPP, rack PDU, and individual outlets
  - Rack temperature monitoring: at minimum 3 sensors per rack (top, middle, bottom of front intake). For contained aisles, sensors at rack inlets are mandatory
  - Cooling capacity monitoring: supply/return water temperature, chiller ton-hours, CRAH/CRAH fan speed, humidifier status
  - Asset management: rack elevation tracking (which server in which U), warranty and maintenance contract dates, firmware versions
  - Capacity planning dashboards: rack space, power, and cooling headroom by row, zone, and facility
- Integration with BMS: DCIM should read BMS data (chiller status, AHU status, generator status, fire alarm) via BACnet, Modbus, or SNMP. BMS should receive alarms from DCIM (over-temperature, overload, power path failure)
- Threshold alerting:
  - Rack inlet temperature >30°C: immediate alert
  - UPS load >85%: warning alert
  - UPS load >95%: critical alert (bypass risk)
  - Phase imbalance >15%: warning; >20%: critical
  - Cooling capacity <90% of available (N-1 violation): immediate alert
- Trend logging: store all sensor data at 5-minute intervals for minimum 13 months (to capture annual peaks). Use for PUE calculation, capacity planning, and anomaly detection
- IT equipment integration: DCIM agents on servers (via IPMI, Redfish, or vendor-specific APIs) report server-level power draw, inlet temperatures, and fan speeds. This data is essential for accurate IT load measurement and thermal analysis

**Building Management System (BMS):**
- BMS platforms (Johnson Controls Metasys, Honeywell Niagara, Siemens Desigo) control and monitor mechanical equipment: chillers, cooling towers, pumps, CRAH units, humidifiers, exhaust fans, and economizers
- BMS communication protocols:
  - BACnet/IP: preferred for modern systems (ISO 16484-5)
  - Modbus TCP/IP: legacy equipment
  - LonWorks: legacy HVAC systems
  - SNMP: UPS, generators, ATS
  -BACnet MS/TP for field-level devices
- Setpoints and control loops:
  - Chilled water supply temperature setpoint: typically 45–55°F (7–13°C) — lower setpoint improves chiller efficiency but increases energy consumption
  - CRAH fan speed: controlled by pressure sensor in the supply plenum. Maintain 0.05–0.10 in-wg (12–25 Pa) positive pressure in the cold aisle
  - Economizer dampers: modulate based on outdoor air temperature and humidity. Enable economizer when outside air is <65°F (18°C) dry-bulb and <60% RH
  - Humidification: maintain 40–60% RH at rack inlets. Over-humidification wastes energy; under-humidification increases ESD risk
- Alarm management:
  - BMS alarms must route to on-call facilities staff within 5 minutes
  - Critical alarms (cooling failure, fire, security breach) must escalate automatically to management if not acknowledged within 15 minutes
  - BMS alarm history must be retained for minimum 12 months
- Trend data: log all BMS points at 1-minute intervals. Essential for identifying equipment degradation (e.g., chiller efficiency declining over 3 years, cooling tower blowdown increasing)

**Integration Architecture:**
```
IT Equipment (Redfish/IPMI) → DCIM → BMS (read-only)
BMS → Controllers → Chillers, Towers, CRAH, AHU, Economizers
DCIM → Alerts → NOC, Facilities, Management
BMS → Alerts → Facilities, Fire System, Security
BMS → Fire Alarm Panel → Suppression System
BMS → Security System → Access Control, CCTV
```
- Network architecture: DCIM and BMS should be on isolated control networks, not on the same VLAN as IT equipment. Physical separation is preferred for security. If IP-based, use 802.1X port authentication and VLAN isolation
- Cybersecurity: BMS and DCIM are OT systems and are increasingly targeted by threat actors. Follow IEC 62443 (industrial automation and control systems security). Disable unused services, enforce strong passwords, use encryption (TLS 1.2+), and log all access

## Decision Frameworks

### Power Path Design

When selecting between N+1 and 2N for a new deployment:

1. **What is the cost of downtime?** If a power outage costs >$100K per hour, 2N is likely justified. If <$10K per hour, N+1 is sufficient. Between $10K and $100K, evaluate the probability of a UPS failure vs. maintenance windows
2. **Is the existing facility designed for dual path?** Retrofitting 2N into a single-path facility requires dual utility feeds, dual UPS, dual PDU, and dual-strip racks — typically 60–80% of the cost of a new build. Consider colocation or new construction instead
3. **What is the equipment's power supply configuration?** If all equipment is single-corded, 2N provides no benefit unless STS units are added at each rack (costly and introduces a new failure point). 2N only works when the IT equipment is dual-corded and configured to draw power from both feeds
4. **What does the maintenance schedule look like?** With N+1, taking a UPS module offline for maintenance requires the remaining modules to carry full load — acceptable only during scheduled periods. With 2N, taking an entire UPS system offline still leaves the facility operational at 100%

### Cooling Method Selection

When choosing between air, rear-door HX, direct-to-chip, and immersion:

1. **What is the maximum per-rack power density?** <15 kW/rack: air cooling with containment is sufficient. 15–30 kW/rack: air + rear-door HX or in-row coolers. 30–60 kW/rack: direct-to-chip liquid cooling + air assist. >60 kW/rack: direct-to-chip or immersion
2. **How many racks at high density?** For <10 racks at >30 kW/rack, consider a dedicated liquid cooling zone with its own CDU and facility water loop. For >20 racks at >30 kW/rack, liquid cooling should be the primary cooling method, not supplemental
3. **What is the existing facility's cooling architecture?** Retrofitting liquid cooling into a facility designed for air requires significant piping, CDU installation, and leak detection. If the facility is less than 5 years old, verify floor loading and overhead space for coolant piping. Facilities older than 10 years may need structural evaluation before adding liquid cooling
4. **What are the maintenance capabilities?** Liquid cooling requires staff trained in coolant handling, leak response, and CDU maintenance. If the facility is run with minimal on-site staff (typical for colocation), air cooling or rear-door HX is more practical

### Equipment Installation Sequencing

When installing new equipment in an existing operational space:

1. **Verify power path capacity** before any installation. Confirm that the breaker feeding the target rack has sufficient spare capacity. Use DCIM data or a clamp meter — never rely on breaker labeling alone. A breaker labeled "60A" may already be feeding 48A continuous to existing equipment. Spare capacity must be at minimum 20% of the breaker rating to accommodate inrush without nuisance tripping
2. **Verify cooling capacity** for the planned heat load. If you are adding 10 kW to a row that already averages 8 kW/rack in an air-cooled environment, verify that the perforated tile airflow in the target cold aisle can deliver ~2500 CFM of 22°C air (approximately 200 CFM per kW for a 10°C ΔT). If the row is already at air cooling limits, add rear-door HX or relocate the equipment to a lower-density row
3. **Cable pathway capacity** — check overhead tray fill. TIA-569-D limits: 40% fill for basket tray, 50% for ladder tray. If the tray is above 50% fill, install additional tray before adding cables
4. **Floor loading** — if the equipment exceeds 500 kg, verify floor point-load capacity at the installation location. Raised floor tiles near support stringers can carry more load. Tiles in the center of a 600×600mm grid carry the least

## Cost Modeling Framework

### Total Cost of Ownership (TCO) by Tier Level

| Cost Category | Tier I | Tier II | Tier III | Tier IV |
|--------------|--------|---------|----------|---------|
| Electrical capex multiplier | 1.0x | 1.2–1.5x | 1.8–2.5x | 2.5–4.0x |
| Mechanical capex multiplier | 1.0x | 1.1–1.3x | 1.5–2.0x | 2.0–3.0x |
| Annual opex (% of capex) | 5–8% | 6–9% | 8–12% | 12–18% |
| PUE (typical) | 1.8–2.0 | 1.6–1.8 | 1.4–1.6 | 1.3–1.5 |
| Generator fuel storage | 8h | 24h | 72h | 96h+ |

### Cost Breakdown by Subsystem (Typical 1 MW IT Load, Tier III)

| Subsystem | % of Total Capex | Typical Cost ($M) |
|-----------|-----------------|-------------------|
| Building shell and structural | 15–20% | $2.0–3.0 |
| Electrical distribution (utility to rack) | 20–25% | $2.5–4.0 |
| UPS and batteries | 10–15% | $1.5–2.5 |
| Generators and ATS | 8–12% | $1.0–2.0 |
| Cooling plant (chillers, towers, CRAH) | 15–20% | $2.0–3.0 |
| Raised floor/structure | 5–8% | $0.7–1.2 |
| Fire suppression | 3–5% | $0.4–0.8 |
| Cabling (power + fiber) | 3–5% | $0.4–0.8 |
| DCIM and controls | 2–3% | $0.3–0.5 |
| Security systems | 2–3% | $0.3–0.5 |
| **Total** | | **$10–18M** |

### Operating Cost Model

| Cost Element | Formula | Example (1 MW IT, PUE 1.4) |
|--------------|---------|----------------------------|
| IT load energy | IT kW × 8,760 hours | 8,760 MWh/year |
| Total facility energy | IT energy × PUE | 12,264 MWh/year |
| Utility cost | Total MWh × $/kWh | $0.12/kWh → $1.47M/year |
| Cooling energy cost | Facility energy × (PUE-1)/PUE × utility rate | ~$380K/year |
| UPS loss energy cost | Facility energy × UPS loss % × utility rate | ~$147K/year |
| Generator fuel (backup) | Fuel gal/hr × hours × $/gallon | ~$50K/year (exercising) |
| Water cost (cooling towers) | Gallons/year × $/gallon | ~$30K/year |
| Maintenance contracts | % of capex | ~$400K/year |
| Staff (facilities) | FTE count × fully-loaded cost | 2–4 × $150K = $300–600K/year |

### PUE Impact on Operating Cost

- PUE 1.8 vs. 1.4 at 1 MW IT and $0.12/kWh:
  - Annual energy cost difference: (1.8 - 1.4) × 1,000 kW × 8,760 hr × $0.12 = $4.2M/year
  - At 10 MW IT: $42M/year difference
- Every 0.1 improvement in PUE saves ~$87K/year per MW of IT load at $0.10/kWh

### Right-Sizing and Deferral Analysis

- Over-sizing UPS by 30% adds ~$150K–$300K in upfront cost and reduces efficiency by 2–4% at typical loads
- Over-sizing cooling plant by 20% adds ~$200K–$500K and reduces chiller efficiency at part load
- Deferring a second generator for 2 years: saves ~$500K now but requires careful load management during that period. Compare the cost of delay against the risk of a single-generator failure
- N+1 vs. 2N: 2N doubles electrical infrastructure cost but enables concurrent maintenance without downtime. For Tier III, concurrent maintenance of distribution path is required — 2N is the only architecture that satisfies this without business interruption

## Key Edge Cases

1. **Generator black start failure during multi-day outage:** A generator that starts reliably for weekly 20-minute no-load tests may fail when it needs to run for 72 consecutive hours. Fuel filter clogging (from microbial growth in stored diesel), coolant leaks that don't appear during short runs, and exhaust system condensation are common failure modes. Never rely on weekly no-load tests as proof of generator reliability — annual full-load 4+ hour load bank tests are non-negotiable
2. **UPS battery failure during discharge:** A battery string may test fine at float voltage and impedance, but a single weak cell goes to zero voltage under load and causes the entire string to fail at 60% discharge. The UPS switches to bypass (raw utility power) to protect the batteries — but if the utility is already down, everything drops. Solution: annual full-load battery discharge tests, and replace strings with any cell showing >20% capacity degradation from the string average
3. **Cold aisle containment pressure collapse:** A contained cold aisle is only effective if it is sealed. If a rack is removed or a tile is lifted for maintenance and not resealed, the cold aisle pressure drops by 20–50%, and the last 3–4 racks in the row (furthest from the CRAH) lose all cooling airflow. Server intake temperatures in those racks can rise from 22°C to 35°C+ within 15 minutes. Trigger a walk-through containment check after any maintenance that opens the cold aisle
4. **Phase imbalance after equipment addition:** Adding a new server to a 208V 3-phase rack PDU, plugged into L1-N (120V), changes the phase balance. If L1 was already carrying 45A and L3 was at 30A, the addition pushes L1 to 50A while L3 stays at 30A. The neutral conductor now carries 20A imbalance current. At 60A breaker rating, the circuit is at 50A (83% of rating, still within 80% derated continuous limit) — but any future addition must go to L3 to rebalance
5. **Water-cooled chiller freeze protection failure:** A winter cold snap causes the cooling tower basin or condenser piping to freeze if the freezestat (low-temperature cutout) fails or if heat trace is de-energized. A burst pipe in the cooling loop can leak thousands of gallons into the mechanical room. Freeze protection for any water-cooled facility in a climate with freezing temperatures must include: heat trace on all outdoor piping, freeze protection thermostats at multiple locations, low-temperature alarm to the BMS with auto-dial notification, and manual drain-down procedures for prolonged outages
6. **Hot air recirculation in an uncongested cold aisle:** A server with a failed fan runs hotter and its exhaust spills into the cold aisle. Adjacent servers ingest this heated air, causing their fans to speed up, increasing their exhaust temperature, and creating a thermal cascade. A single failed fan in a 42U rack can cause 8–12 adjacent servers to exceed the ASHRAE allowable temperature within 2 hours. CFD modeling could have predicted the recirculation path. Thermal monitoring at the rack inlet (top, middle, bottom of each rack) detects this quickly — deploy at least 3 temperature sensors per rack
7. **Cable tray overload during a capacity expansion:** Adding 48 new patch cables (Cat6A, ~0.25 inches diameter each) in an already congested tray pushes fill from 45% to 60%. The cables cannot shed heat adequately — the center of the bundle operates at 50°C+, exceeding the 60°C cable rating and reducing service life. An overhead tray loaded to >40% fill in a GPU cluster where ambient temperature is 28°C creates a hotspot in the cable bundle. Use fiber for dense interconnections (smaller diameter, no heat generation) or install additional tray before the expansion
8. **UPS ECO mode (economy mode) transfer failure:** ECO mode runs the load on utility through a static bypass for efficiency improvement (99% vs. 96%). If utility power quality degrades (sag, spike, frequency shift), the UPS must transfer to double-conversion mode in <4ms. Some UPS systems fail this transfer if the bypass static switch is defective or if the control logic misidentifies the fault. ECO mode is safe only if the UPS can transition to double-conversion faster than the IT equipment's power supply hold-up time (typically 8–20ms). Test ECO mode during commissioning for every UPS before putting it into production

## Communication Patterns

### Internal Stakeholders

- **Facilities/Operations:** talk in terms of specific procedures and limits. "The cooling capacity for row 4 is 12 kW/rack with current tile configuration. Adding a 20 kW GPU rack requires either adjusting to liquid cooling or installing rear-door heat exchangers before deployment."
- **IT Operations:** quantify impact on deployment schedules and operational risk. "Installing these 24 servers requires 6 hours of rack preparation, 4 hours of structured cabling, and 2 hours of power verification. The phase balance on RPP-A1 needs adjustment before installation — that requires a 30-minute power outage."
- **Finance/Management:** talk in cost of downtime, capex required, and PUE impact. "Adding liquid cooling to the GPU zone costs $450K in CDU, piping, and facility loop modifications, but enables 40 kW/rack density and reduces PUE by 0.15. Without it, deployment is limited to 15 kW/rack air-cooled and requires 2.5× more floor space for the same compute capacity."

### Contractor/Vendor Communication

- **Electrical contractors:** be explicit about torque specs, testing requirements, and labeling. "All bolted connections require a calibrated torque wrench at manufacturer-specified torque values. Megger test every branch circuit before energization and record values. Label every breaker at both ends with machine-printed labels following the labeling schedule in Appendix B."
- **Cooling contractors:** specify water quality and commissioning tests. "Chilled water loops must be filled with treated water meeting the water quality spec in Section 15900. Flow balance every coil to within ±5% of design CFM. Provide a balancing report with measured vs. design values."
- **Cabling contractors:** require test results for every link. "Every Cat6A channel must pass a full channel test (permanent link + patch cords) per TIA-1152-A for Class EA. Provide a test results file (Fluke LinkWare or equivalent) for every drop. Any channel failing NEXT or RL must be re-terminated and re-tested."

## Escalation Protocols

| Trigger | Action | Timeline |
|---------|--------|----------|
| UPS bypass (load running on utility bypass, not inverter) | Notify facility manager, investigate cause (overload, battery failure, rectifier fault). Determine if return to normal is safe | Immediate — load is unprotected from utility disturbances |
| Generator failure during utility outage | Manually start backup generator if available. If single generator: call fuel supplier, dispatch mechanic, prepare for extended utility bypass | Immediate — UPS runtime is 5–15 minutes |
| High rack inlet temperature (>30°C / >86°F) | Verify cold aisle containment integrity. Check CRAH setpoints and operation. Increase CRAH fan speed if available. If >32°C: begin emergency shutdown procedures for affected racks | Within 5 minutes — IT equipment may trip at 35–40°C inlet |
| Cooling plant failure (chiller or tower lost) | Verify N+1 redundancy holds. If all chillers offline: load-shed non-critical IT loads, reduce power draw, and call chiller service | Within 15 minutes — cooling capacity decays as water temperature rises |
| Water leak in white space | Shutoff valve at zone immediately. Sump pump or wet vacuum. Call restoration contractor if water reached IT equipment | Immediate — electricity + water = electrocution and equipment damage |
| Phase imbalance >20% between any two phases | Rebalance loads by distributing new equipment connections. If rebalancing requires circuit re-wiring: schedule outage window | Within 1 week — severe imbalance overheats neutral and reduces transformer life |
| >50% cable tray fill | Install additional tray before any new cables are added | Before next cable deployment |
| Raised floor quarter-turn fastener (QTF) not reinstalled after maintenance | Walk entire cold aisle to verify all QTF are present and sealed | Immediate — cooling performance degrades with every missing tile |
| Fuel delivery >48h in advance | Confirm fuel supplier contract, test fuel quality, verify tank capacity and containment | Weekly — order when tank drops below 50% |

### Escalation Chain

Facility Technician → Facility Manager (immediate for any life-safety or critical infrastructure failure) → Data Center Operations Director (within 1 hour for failures affecting IT load) → VP Infrastructure / CTO (>$100K equipment loss or extended downtime)

## Performance Indicators

Track monthly, review quarterly with facilities and operations:

| Metric | Target | Red Flag |
|--------|--------|----------|
| PUE (quarterly rolling average) | <1.4 | >1.6 for two consecutive quarters |
| IT load vs. UPS capacity | <80% of UPS rated capacity | >90% — no room for failure or expansion |
| Cooling capacity vs. IT load | <70% of available cooling capacity | >85% — approach cooling limits |
| Phase imbalance (any two phases) | <10% difference | >20% |
| Generator monthly load test | Full-load or load bank test >50% capacity for 2h minimum | Missed monthly test or <30% load (wet stacking risk) |
| UPS battery impedance | Within ±25% of baseline for all cells | Any cell >50% deviation or VRLA >25% deviation |
| Rack inlet temperature range within row | <3°C variation between any two racks | >5°C variation — containment leak or poor airflow |
| Floor loading (white space average) | <70% of rated capacity | >85% — structural risk in any zone |
| Cable tray fill (average) | <40% fill | >50% fill in any tray segment |
| Power path availability (12-month rolling) | 99.99%+ (excluding scheduled maintenance) | <99.9% (unplanned outage in power path) |
| Emergency generator fuel on-site | >72h at full load | <24h |
| Commissioning test completion (new deployments) | 100% of checklist items passed before production | Any uncorrected failed test |

## Instructions

1. Clarify the scope: greenfield vs. retrofit, tier target, total IT load estimate, timeline, and budget constraints before providing any design
2. Identify critical constraints first — utility capacity at site, floor loading limits, ceiling height, physical security requirements, and existing infrastructure that cannot be modified
3. Present options with quantified trade-offs — always compare at least 2 viable designs at different tier or power density points, with capex, opex, and PUE estimates for each
4. Validate all calculations with worked examples — power (kW → kVA → breaker sizing → conductor sizing), cooling (IT kW → cooling tons → chiller capacity), and space (racks → rows → white space area → total facility area)
5. Document every assumption explicitly — every capacity plan is only as good as its assumptions about workload growth rate, power density per rack, efficiency improvement trajectory, and cost escalation
6. Include failure scenarios in every recommendation — "If this UPS fails, what happens?" — and verify the answer is acceptable
7. Reference specific codes and standards (NEC, ASHRAE TC 9.9, TIA-942, Uptime Institute) in every recommendation where they apply

## Related Skills

| Skill | Use For |
|-------|---------|
| `cloud-architect` | Cloud vs. on-premises architecture decisions |
| `energy-procurement` | Utility tariff analysis and electricity procurement |
| `architecture` | Big-picture system design and trade-off analysis |
| `network-101` | Network topology and structured cabling design |

## Regulatory and Compliance Framework

### Data Center Standards and Certifications

**Uptime Institute Tier Certification:**
- Tier III (Concurrently Maintainable): design review + physical site tour. Does not certify operations
- Tier IV (Fault Tolerant): design review + physical site tour + third-party engineer involvement. Most rigorous
- Tier Certification is valid for 3 years, after which the facility must be re-certified
- Tier certification does not guarantee uptime — it certifies design and operational capability

**SOC 2 (Service Organization Control 2):**
- Trust Services Criteria: Security, Availability, Processing Integrity, Confidentiality, Privacy
- Type I: point-in-time assessment of design
- Type II: assessment of operational effectiveness over time (typically 12-month period)
- Physical security controls are audited as part of the Security and Availability criteria
- Common Criteria (ISO/IEC 15408): international standard for security evaluation of IT products. May be required for government or defense contracts

**ISO 27001 (Information Security Management):**
- Annex A controls include physical and environmental controls (A.7)
- A.7.1: Physical security perimeter — barriers, locked doors, logging of entry
- A.7.2: Physical entry — access control systems, visitor management, badge policies
- A.7.3: Securing offices, rooms, and facilities — white space requirements
- Certification requires an accredited registrar (ANAB, UKAS, etc.)

**PCI-DSS v4.0 (Payment Card Industry Data Security Standard):**
- Requirement 9: restricts physical access to cardholder data environment (CDE)
- 9.3: controls at physical entry points (badge, biometric, mantrap)
- 9.5: visitor logs maintained with arrival/departure times, escorted access required
- 9.9: attack vectors from removable media and paper — requires secure storage and destruction
- Annual penetration testing required (Req 11.3)

**HIPAA (Health Insurance Portability and Accountability Act):**
- Security Rule: physical safeguards for ePHI (electronic protected health information)
- § 164.310(a)(1): facility access controls — contingency operations, locked facilities
- § 164.310(a)(2): workstation use and security — physical access to workstations displaying ePHI
- § 164.310(d): device and media controls — disposal of ePHI on physical media

**EU GDPR (General Data Protection Regulation):**
- Art. 30: each controller/processor must maintain a record of processing activities, including physical security measures
- Art. 32: security of processing — includes physical security as technical and organizational measure
- Data breach notification: 72 hours to supervisory authority after becoming aware of a breach
- Data localization: some EU member states have specific requirements for data residency within national borders

**FISMA (Federal Information Security Management Act) and FedRAMP (US):**
- Moderate impact level: requires perimeter security, access logging, intrusion detection
- High impact level: additional controls for media protection, personnel security, and contingency planning
- FedRAMP Authorization: required for cloud services handling US federal data

**Environmental and Energy Regulations:**
- EPA Clean Air Act: stationary diesel generators >3000 HP require permits in non-attainment areas. NOx, CO, and particulate matter limits apply
- EU Energy Efficiency Directive (EED): large enterprises (>250 employees or >€50M turnover) must undergo energy audits every 4 years. Data centers >100 kW are explicitly covered
- EU Corporate Sustainability Reporting Directive (CSRD): large data center operators must disclose energy consumption, efficiency metrics (PUE, WUE), and renewable energy share
- UK Energy Security Act / Data Centre Strategy: mandatory reporting of PUE for data centers >100 kW from 2024
- California Title 24 / Energy Code: indoor lighting power density limits, commissioning requirements for mechanical systems
- EU Ecodesign Directive / UK Energy-related Products (ErP): minimum efficiency standards for UPS (>500kVA require efficiency >95% at 75% load)

**Permits and Inspections:**
- Building permit: required for new construction and significant modifications (electrical, mechanical, structural). Issued by local authority having jurisdiction (AHJ)
- Electrical inspection: rough-in and final inspection by licensed electrical inspector. Must pass before energization
- Mechanical inspection: HVAC installation inspected for proper refrigerant handling, ventilation, and fire suppression integration
- Fire permit: suppression system installation requires permit and hydrostatic test of piping
- Occupancy permit: required before building is occupied. Fire marshal sign-off required for raised floor and assembly areas
- Environmental permits: stormwater (during construction), air emissions (generators, cooling towers in non-attainment zones), water discharge (cooling tower blowdown)

## Limitations

- Use this skill only when the task clearly matches the scope described above. It provides critical infrastructure guidance but does not replace stamped engineering drawings or local code compliance review.
- All recommendations must be verified against local codes (NEC, NFPA, local amendments), manufacturer specifications, and site-specific conditions before implementation.
- Stop and ask for clarification if required inputs (power budget, tier target, site constraints, timeline) are missing or if the requested design conflicts with safety codes.
- Electrical work must be performed by licensed electricians. Cooling system modifications must be performed by certified HVAC technicians. This skill provides the engineering logic — it does not replace professional licensing.
