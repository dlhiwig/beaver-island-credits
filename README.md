# Beaver Island Credits (BIC) — Off-Grid Community Token Currency

**Created:** March 14, 2026
**Setting:** Beaver Island, Michigan — 20–50 veteran co-op community
**Acronym:** BIC

---

## Concept

Beaver Island Credits is a **closed-loop, private, off-grid digital token system** for a small, trust-based veterans' community. Not a cryptocurrency — no blockchain, no mining, no global value, no speculation. Think **digitized community IOUs** or **co-op credits** that track who owes what for real-world goods and services.

**Value is backed by:** Mutual agreement + real assets within the group (not external markets or fiat).

---

## Key Characteristics

| Property | Description |
|----------|-------------|
| **Scope** | Local & closed — only usable within Beaver Island community |
| **Ledger** | Centralized — one trusted "bank" node (or few synced) holds master ledger |
| **Issuance** | Central — core crew creates tokens when someone contributes value |
| **Transfers** | P2P via Meshtastic LoRa mesh (long-range, low-power radio) |
| **Infrastructure** | No internet needed — fully off-grid |
| **Crypto primitives** | None required — admin PIN/channel encryption + group trust |
| **Purpose** | Mutual aid with accounting — bridge gaps the mainland economy ignores |

---

## How It Works

1. **Someone contributes value** (supplies propane, catches fish, repairs a roof, provides medical supplies)
2. **Core crew issues credits** — e.g., "issue 100 credits to Vet X for supplying winter propane"
3. **Credits transfer P2P** via LoRa mesh messages — `pay !nodeid 50` or `balance`
4. **Central ledger tracks** all balances and transactions
5. **Credits spent** on goods/services within the community (firewood, fish shares, boat fuel, meds, labor)

**No cash-out to USD/crypto** — manual bridging possible but rare and risky (in-story).

---

## Real-World Inspiration

### Meshtbank (Primary)
- **Creator:** Roni Bandini (~late 2025)
- **Hardware:** Seeed XIAO nRF52840 + SX1262 + FireBeetle ESP32 (ledger/display)
- **Function:** Lightweight ledger of user balances and transfers over Meshtastic
- **Commands:** `pay !nodeid 50`, `balance`
- **Coverage:** Hackaday, Hackster.io, Reddit /r/meshtastic
- **Tagline:** "Meshtastic banking system for the apocalypse"
- **Key insight:** No blockchain bloat — just a central node everyone trusts

### Other Parallels
- **BerkShares** (Massachusetts) — local paper currency backed by local banks
- **Ithaca Hours** — community currency based on labor time
- **Bristol Pound** (UK) — local digital currency for Bristol businesses
- **LETS Systems** (Local Exchange Trading Systems) — mutual credit networks
- **Time-banking apps** — track labor exchanges without money
- **Burning Man scrip** — event-based token systems

---

## Technical Stack (In-Story)

| Component | Hardware/Software |
|-----------|------------------|
| **Mesh network** | Meshtastic on LoRa radios |
| **Bank node** | FireBeetle ESP32 + display |
| **User nodes** | Seeed XIAO nRF52840 + SX1262 |
| **Ledger** | Lightweight flat-file or SQLite on bank node |
| **Security** | Channel encryption + admin PIN |
| **Range** | LoRa — miles of range, low power |
| **Power** | Solar/battery (off-grid compatible) |

---

## Use Cases (In-Story Economy)

| Good/Service | Example Transaction |
|--------------|---------------------|
| Firewood | 50 credits per cord |
| Fish shares | 20 credits per catch share |
| Roof repairs | 200 credits for labor |
| Medical supplies | 100 credits per kit |
| Boat fuel | 30 credits per gallon allocation |
| Propane | 100 credits per winter supply run |
| Tool lending | 10 credits per day |
| Guard/watch duty | 15 credits per shift |

---

## Plot Tension Opportunities

- **Issuance dispute** — Who decides how many credits a contribution is worth? Power dynamics.
- **Node failure during storm** — Bank node goes offline, no one can transact. Emergency protocol?
- **Counterfeit attempt** — Someone fakes a `pay` command. How is it detected?
- **Inflation pressure** — Too many credits issued, goods become scarce. Who pulls the lever?
- **Trust breakdown** — What happens when the "banker" is accused of favoritism?
- **External discovery** — Mainland authorities find out about the parallel economy.
- **Redemption/exit** — A vet wants to leave the island. Can credits convert to anything?
- **Mesh range limits** — A distant homestead can't reach the bank node. Relay trust?

---

## Design Principles

1. **Pragmatic, not revolutionary** — Adaptation of existing open-source tools, not invention
2. **Trust-based** — Small group where reputation matters more than cryptography
3. **Off-grid first** — Must work when internet, power grid, and supply chains fail
4. **Tied to real value** — Every credit represents actual goods/labor contributed
5. **Low-profile** — Not designed to attract attention from outside
6. **Simple** — Any veteran can understand and use it, no tech background needed

---

## Research & References

- [ ] Meshtbank GitHub repo (Roni Bandini)
- [ ] Meshtastic documentation — message types, encryption, mesh topology
- [ ] LoRa hardware specs — range, power consumption, solar compatibility
- [ ] BerkShares / Ithaca Hours case studies — what worked, what failed
- [ ] LETS system design documents
- [ ] Community currency economics — inflation control in closed systems
- [ ] Beaver Island geography — mesh coverage feasibility, terrain, population

---

## Next Steps

- [ ] Research Meshtbank source code and hardware BOM
- [ ] Map Beaver Island terrain for LoRa mesh coverage modeling
- [ ] Define the Beaver Island economy — what goods/services, what's scarce, what's abundant
- [ ] Design the credit issuance governance (who decides, voting, disputes)
- [ ] Draft the "founding charter" of the Beaver Island Credits system (in-story document)
- [ ] Identify plot-critical technical failure modes
- [ ] Create character roles: banker, auditor, merchant, skeptic

---

*"The real magic is the off-grid mesh keeping it functional when everything else fails."*
