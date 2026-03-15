# Beaver Island Credits — MVP Development Plan

*Comprehensive plan for the fictional book's ultra-realistic framing.*

**Setting:** Beaver Island, Michigan — pop ~600-750, with 20-50 active veteran participants.

**Goal:** Build trust-based mutual aid for essentials (firewood, fish, repairs, meds, fuel) without external traces. Emphasize community vet bonds, redundancy, and simplicity.

---

## Phase 1: Planning & Research (1-2 months in-story)

### Core Crew Assembly
- **3-5 trusted vets:**
  - Lead dev/planner (protagonist)
  - Mechanic (hardware builds)
  - Medic/logistics (supply chain, community needs)
  - Security/opsec specialist

### Scope Definition
- **Max users:** Start with 10-15, cap at 30-40 (gossip/leak risk)
- **Token backing:** Issued manually by core crew against real assets
  - 1 credit = 1 cord firewood, 1 fish haul share, or $ equivalent in community fund cash
- **Use cases:** Barter for services/goods; small emergency fund payouts

### Tech Stack Decisions

| Component | Choice | Notes |
|-----------|--------|-------|
| **Primary comms** | Meshtastic LoRa nodes (LilyGo/Heltec, $20-40 each) | 1-5 km wooded, repeaters on high points |
| **User devices** | Cheap Android burners ($50-100) + Meshtastic app + custom Python/Flutter wallet UI | QR generation, sign/verify |
| **Alternative** | Meshtastic device + Bluetooth to phone | App wakes on button press, vibrate-only |
| **Ledger** | Encrypted shared file (JSON/SQLite) synced via mesh | Last-timestamp-wins + core manual resolve |
| **AI server** | Old PC/GPU running offline LLM | Balance queries via voice/text over mesh, weather, med reminders — nice-to-have |

### Risk Brainstorm (Plot Tension)

| Risk | Solution | Story Potential |
|------|----------|-----------------|
| Forest/tree interference → dead zones | 3-5 solar repeater nodes on tower/high spots | Storm knocks out repeaters |
| Battery drain | Solar chargers mandatory | Winter, short days, charger fails |
| Forgotten passphrases, lost devices | Weekly USB ledger backups by core crew | Who holds the backups? Trust. |
| Legal gray area | Keep tokens "boring" (no drugs/weapons), frame as co-op barter | DEA/ATF discovers system |
| Gossip/leaks | Strict "need-to-know", silent/vibrate-only transactions | A vet talks to a mainlander |

---

## Phase 2: Build the MVP (2-3 months in-story)

### Hardware Procurement (Discreet Mainland Buys)
- 20-30 Meshtastic nodes + antennas
- Burners or dedicated LoRa + screen modules (ESP32 + OLED wallets)
- Solar panels/batteries for repeaters
- Raspberry Pi / old laptop for ledger sync + AI box

### Software Development (Dead Simple)
1. Fork Meshtastic firmware if needed for custom channels/encryption
2. **Custom app/script:**
   - Generate unique QR/private key pair on setup (one-time passphrase from core)
   - Send: Scan QR → enter amount → sign with private key → broadcast over mesh
   - Receive: Auto-verify signature → update local balance
   - Sync ledger file opportunistically when nodes in range
3. Test offline signing/verification first (ECDSA)
4. **AI integration:** Mesh text/voice → central node → LLM responds locally → broadcast back

### Infrastructure

| System | Details |
|--------|---------|
| **Mesh channel** | "Beaver IslandNet" on US 915 MHz band |
| **Tower Wi-Fi** | Optional high-speed local sync (payments stay LoRa-only) |
| **Fiber line** | Air-gapped except emergency mainland VPN for "news" (tax/DEA chatter alerts) — never for transactions |
| **Redundancy** | Two ledger master nodes (one hidden); solar backups |

---

## Phase 3: Testing & Iteration (1-2 months in-story)

### Dry Run #1 (Week 1)
- Core crew only
- 10-20 fake transfers ("I owe you 5 credits for roof fix")
- Test: range, battery, sync conflicts

### Dry Run #2 (Weeks 2-3)
- Add 5 more trusted vets
- Real-ish trades: barter firewood/fish using paper proxies first, then switch to tokens

### Stress Tests

| Test | What It Proves |
|------|----------------|
| Simulate dead zones | Repeater failover works |
| Power outage | Solar failover works |
| Lost device | Passphrase + USB recovery works |
| Double-spend attempt | Core crew resolution works |
| Snitch scenario | Security audit, opsec review |

### Success Metrics
- 90%+ delivery rate island-wide
- No major conflicts after 50+ test transactions
- Users report easy/intuitive (QR scan + amount)

### Bug Fixes
- Tweak LoRa settings (longer range modem preset, lower data rate for reliability in woods)
- Security audit: core crew reviews code/logs

---

## Phase 4: Soft Launch & Monitoring (Ongoing)

### Operations
- Roll out to initial 15-20 users
- Weekly core meet (disguised as poker/fishing) for:
  - Ledger review
  - New credit issuance
  - Dispute resolution
- Monitor: gossip levels, battery complaints, range issues

### Scaling
- Expand slowly: add districts if >30 users (separate mesh channels, monthly bridge tx)

### Plot Hooks

| Event | Drama |
|-------|-------|
| Newcomer wants in | Vetting process — who vouches? |
| Storm knocks out repeaters | Manual fallbacks, paper IOUs, trust test |
| Someone "loses" a big balance | Accusation or real? Core crew mediates |
| Mainland authority discovers system | Evacuation protocol for hardware |
| Vet wants to leave island | Can credits convert? Exit drama |
| Two bank nodes disagree | Poker night becomes a trial |

---

## Character Roles (Suggested)

| Role | Function | Story Arc |
|------|----------|-----------|
| **The Architect** | Lead dev, designed the system | Burden of being the single point of knowledge |
| **The Banker** | Runs the bank node, issues credits | Power corrupts? Accused of favoritism? |
| **The Mechanic** | Builds/repairs hardware | Keeps it running in storms, MacGyver moments |
| **The Skeptic** | Doesn't trust digital anything | Converts slowly, then becomes the biggest advocate |
| **The Medic** | Tracks medical supply credits | Ethical dilemmas — who gets meds when supply is short? |
| **The Outsider** | New arrival, wants in | Trust test, vetting, potential leak risk |

---

---

## Field Realism Notes (Practical Concerns)

*Things that sound good on paper but bite you on the island.*

### 1. Range Sucks in Trees
- Beaver Island has dense pines and hills — LoRa drops packets in woods
- **Fix:** Repeaters on tower, old water tanks, church steeples
- Still expect dead zones — plan for them, don't pretend they won't happen

### 2. Burner + Bluetooth = Battery Hell
- Phones die fast when Bluetooth always scanning
- **Fix:** App wakes only on "send" tap — no constant pings
- Or: carry a separate $10 LoRa keychain, pair once, forget it

### 3. Interference
- Fishermen, tourists, Wi-Fi tower — everyone blasting 2.4 GHz
- Meshtastic can hop channels, but a drone parked nearby? Toast
- **Test it now**, not after the first trade flops

### 4. Noisy Neighbors
- Vets talk. If a burner beeps every transaction, someone asks "what's that?"
- Cover blown.
- **Fix:** Mute everything. Silent mode, vibrate-only at most.

### 5. Power
- Island grid is flaky. Nodes sip juice, but if the tower dies mid-sync → ledger splits
- **Fix:** Solar panel per node — cheap, quiet, no one notices

### 6. Human Error
- Vet forgets passphrase, loses phone in the lake, or worse — sells it to a tourist
- **Fix:** "Freeze codes" — core crew sends kill phrase, bricks the wallet remotely

### 7. Trust Is the Hardest Part
- Not the code, not the burners — it's **trust**
- Vets who've seen worse than a tax audit will buy in if it feels like **mutual aid, not a hustle**
- Start tiny: one boat, one mechanic, one medic. Prove it works. Then grow.
- The system has to feel boring — "Island Credits" or "Beaver Island Tokens" — sounds dull enough to fly under radar

### 8. Enforce Scarcity
- Don't let tokens multiply like rabbits
- Every credit must trace back to a real contribution
- Core crew controls issuance — no one else can mint

### 9. Legal Gray Zone
- You're not laundering, you're bartering. But if someone uses Beaver Island Credits for weed or guns → "conspiracy to distribute"
- **Rule:** Keep it boring — fish, firewood, meds. Nothing sexy.

---

## AI Server Concept — "Beaver Island Echo"

**Hardware:** One beefy box (old gaming rig with GPU) running Llama or Mistral locally.
**Purpose:** Community utility, not core to payments.

| Feature | How It Works |
|---------|-------------|
| Balance queries | "What's my balance?" via voice/text over mesh |
| Weather forecasts | Local inference, broadcast to mesh |
| Med reminders | Scheduled alerts to specific nodes |
| Trade approval | Auto-approve if both parties say "yes" (optional) |
| News alerts | Emergency mainland VPN → "IRS is sniffing around" |

**Naming options:** Beaver Island Echo, or something deliberately mundane.

**Rule:** AI is a nice-to-have community tool. Payments work without it. Never a single point of failure.

---

## Dry Run Proposal (In-Story)

**"Five vets, one week, fake tokens — just to see who forgets their phone at home."**

- 5 core crew members
- 10-20 fake transactions over 7 days
- Real scenarios: "I owe you 5 credits for the roof fix"
- Track: who forgets their phone, who can't remember the passphrase, who panics when a tx doesn't go through
- Debrief at poker night — what worked, what sucked, who needs more training

**This is the scene that sells the reader on the system.** The first real test. The awkward fumbling. The moment it actually works and someone says "holy shit, that's it?"

---

## Scaling Tension — The Cash-Out Problem

**"Once you hit fifty, someone's gonna want to cash out for real dollars. That's where it gets messy."**

### Keep It Small (20 people) vs. Scale to Island (~600)
- At 20: trust-based, everyone knows everyone, disputes settle at poker night
- At 50+: someone inevitably wants USD. That creates a **bridge problem**:
  - Who converts? At what rate? Is that money transmission?
  - Legal exposure jumps from "quirky barter club" to "unlicensed financial service"

### The Real Purpose (Not Tax Evasion)
- Shadow economy isn't about dodging taxes forever
- It's about **bridging gaps the mainland ignores**:
  - Groceries, meds, heat — real needs
  - Barter rings for firewood, fish, repairs — no paper trail
  - "Community fund" that looks like a poker night pot but funds meds or boat fuel

### Built-In Cover
- Remote island, insular community, vets who know how to keep secrets
- Vet bond = people won't rat if they think it's for the group
- **But**: island gossip is the #1 operational risk

### Opsec Layers
- Rotate who handles cash
- Use dead drops for physical exchanges
- Never name names in digital comms
- Transactions are silent/vibrate-only
- Keep the tokens boring — fish, firewood, meds. Nothing that attracts attention

---

*"The 'currency' aspect is just mutual credit tracking; the real magic is the off-grid mesh keeping it functional when everything else fails."*
