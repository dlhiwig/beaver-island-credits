# Beaver Island Credits — Technical Bible

*Every detail below is pulled from real, working projects. Not conceptual or sci-fi.*

---

## Meshtbank Core Architecture

### Hardware Bill of Materials (~$80-120 per bank node)

| Component | Part | Purpose | Cost |
|-----------|------|---------|------|
| Radio/mesh node | Seeed XIAO nRF52840 + WIO-SX1262 LoRa shield | Meshtastic firmware + 915 MHz LoRa | ~$25 |
| Processing/display | DFRobot FireBeetle 2 ESP32-C6 | Dual-core, runs ledger logic (Arduino/C++) | ~$15 |
| Screen (optional) | DFRobot Fermion 2.0" TFT 320×240 (GDI cable) | Display — can remove to save power | ~$15 |
| Power | 3.7V LiPo battery + 5V solar panel | Runs indefinitely with small panel | ~$15-25 |
| Case | 3D-printed (STLs on Cults3D/GitHub) | Pocketable or shed-mountable | ~$5 |

**Total size:** Pocketable or mountable in a shed.

### Physical Build (Verbatim from Meshtbank)

1. Mount SX1262 shield directly on XIAO nRF52840
2. Run two jumper wires:
   - FireBeetle GPIO 17 → XIAO pin 7 (TX)
   - FireBeetle GPIO 16 → XIAO pin 6 (RX)
3. Share 3.3V and GND
4. TFT connects via FireBeetle's GDI header

### Firmware & Software Setup

1. **Flash Meshtastic** to XIAO nRF52840 via official flasher (https://flasher.meshtastic.org/). Hold BOOT button while plugging USB.
2. **Configure via web client** (https://client.meshtastic.org/) in Serial mode:
   - Region = US (915 MHz)
   - Channel name = "Beaver IslandBank"
   - Module = Serial (Proto mode)
   - TX pin = 7, RX pin = 6
   - Hops = 1 during testing
   - Disable MQTT
3. **Arduino IDE on FireBeetle ESP32-C6:**
   - Install DFRobot_GDL library (TFT)
   - Install Meshtastic Arduino library
4. **Load Meshtbank sketch** (`meshtbank1.ino` from GitHub). Customize admin password.

---

## Ledger & Transaction System

### How It Works (Zero Blockchain)

- FireBeetle maintains **single source of truth** ledger in RAM + optional flash/EEPROM
- Lightweight JSON-style arrays: users, balances, timestamped history
- All communication via **Meshtastic direct messages** (DMs) — never broadcast
- Security: channel encryption (PSK) + admin PIN

### User Commands (from any Meshtastic node/phone)

```
balance                    → returns current credits
pay !27e52039 50           → transfers 50 credits to node ID
history                    → shows all transactions
history 5                  → shows last 5 transactions
help                       → command list
```

### Admin Commands (bank device only, PIN-protected)

```
setup <pass> !nodeid 100   → create/reset user with starting balance
delete <pass> !nodeid      → remove user
reset <pass>               → wipe everything
listusers <pass>           → list all users
checkbal <pass> !nodeid    → check any user's balance
checkhist <pass> !nodeid   → check any user's history
```

---

## Client Hardware — What Each Vet Carries

### Best Wallet Nodes ($25-60)

| Board | Specs | Price | Notes |
|-------|-------|-------|-------|
| **Heltec WiFi LoRa 32 V3** | ESP32-S3 + SX1262, OLED, 240 MHz, 4 MB flash | ~$35 | Native Meshtastic, balance cache + QR |
| **LilyGo T-Echo** | nRF52840, e-ink display, battery holder, GPS | ~$45 | E-ink = sunlight readable, GPS for dead-zone mapping |
| **LilyGo T-Beam** | ESP32 + SX1262, OLED, GPS, battery holder | ~$50 | Popular Meshtastic board |
| **Seeed XIAO ESP32S3 + LoRa** | Thumb-sized, Bluetooth to phone | ~$25 | Pairs with cheap phone for UI |

### Bank/Repeater Nodes

- Same XIAO nRF52840 + SX1262 as Meshtbank
- Or: **RAK Wireless WisBlock** modules (modular, even lower power)
- SX1262 LoRa: 915 MHz, 100-200 mW TX
  - Wooded terrain: 1-5 km range
  - Line-of-sight from tower: 10+ km
  - Add 5-8 dBi antenna for extra reach

### Island Deployment Tweaks (Proven)

- **Trees/hills kill range** → 2-3 solar-powered repeaters on high points
- **Battery life** → Force sleep mode; screen wakes on button press only
- **Interference** → Meshtastic frequency hopping + private channel = invisible to scanners
- **Sync** → One hidden bank node (vet's shed) + repeater nodes on tower = full coverage
- **Redundancy** → Multiple bank nodes synced manually at weekly poker nights (USB ledger handoff)

---

## Power & Off-Grid Realism

- Sleep current: **micro-amps** — months on 3000 mAh cell with screen off
- Add $10 solar panel to FireBeetle → **never needs recharging**
- Designed explicitly for "no grid, no internet" scenarios

---

## Transaction Flow Pseudocode

### 1. Wallet Setup (One-Time)

```
function setup_wallet(passphrase_from_core):
    seed = SHA256(passphrase_from_core + device_salt)
    private_key = ECDSA_generate_private_from_seed(seed)
    public_key = ECDSA_derive_public(private_key)
    node_id = meshtastic_get_node_id()    // e.g., !abcd1234

    wallet = {
        "node_id": node_id,
        "pubkey": hex(public_key),
        "privkey": encrypted_store(hex(private_key), passphrase),
        "balance": 0,
        "ledger": []
    }

    save_wallet_to_local_storage(wallet)
    display("Wallet ready. Your ID: " + node_id)
```

### 2. Credit Issuance (Core Crew Only)

```
function issue_credits(recipient_node_id, amount, memo):
    if not is_core_crew():
        return error("Only core can issue credits")

    tx = {
        "type": "issue",
        "from": "core_issuer",
        "to": recipient_node_id,
        "amount": amount,
        "memo": memo or "monthly stipend",
        "timestamp": current_unix_time(),
        "tx_id": SHA256(tx without signature)
    }

    signature = ECDSA_sign(SHA256(serialize(tx)), core_private_key)
    tx["sig"] = hex(signature)

    broadcast_over_mesh(encode(tx))
    append_to_local_ledger(tx)
    update_balance_cache(recipient_node_id, +amount)
```

### 3. P2P Transfer (Sending)

```
function send_transfer(to_node_id, amount, memo):
    if get_local_balance() < amount:
        return error("Insufficient balance")

    tx = {
        "type": "transfer",
        "from": my_node_id,
        "to": to_node_id,
        "amount": amount,
        "memo": memo or "",
        "timestamp": current_unix_time(),
        "nonce": random_32bit(),
        "tx_id": SHA256(serialize above fields)
    }

    signature = ECDSA_sign(SHA256(serialize(tx)), my_private_key)
    tx["sig"] = hex(signature)

    display_qr(encode_tx_for_qr(tx))    // optional in-person scan
    broadcast_over_mesh(encode(tx))

    append_to_local_ledger(tx)
    update_balance_cache(my_node_id, -amount)
    display("Sent " + amount + " to " + to_node_id)
```

### 4. Receiving & Verification

```
on_mesh_receive(raw_packet):
    tx = decode(raw_packet)
    if not tx or tx.timestamp < oldest_allowed:
        discard

    message_hash = SHA256(serialize(tx without sig))

    if tx.type == "issue":
        signer_pub = core_public_key
    else:
        signer_pub = lookup_pubkey(tx.from)

    if signer_pub is null:
        queue_for_later()
        return

    if not ECDSA_verify(message_hash, tx.sig, signer_pub):
        log("Invalid signature from " + tx.from)
        return

    if tx.nonce in seen_nonces_from(tx.from):
        discard    // replay attack

    if has_conflicting_pending(tx):
        core_crew_resolve()
        return

    append_to_local_ledger(tx)
    mark_nonce_seen(tx.from, tx.nonce)

    if tx.to == my_node_id:
        update_balance_cache(tx.to, +tx.amount)
        vibrate_and_notify("Received " + tx.amount + " from " + tx.from)

    if should_rebroadcast():
        rebroadcast(raw_packet)
```

### 5. Balance Query

```
function get_balance():
    balance = 0
    for tx in local_ledger:
        if tx.type == "issue" or tx.to == my_node_id:
            balance += tx.amount
        if tx.from == my_node_id:
            balance -= tx.amount
    return balance    // cross-check with cache; alert if drift
```

---

## Ledger Conflict Resolution

### Automatic Merge

```
function merge_ledgers(incoming_ledger_packets, my_local_ledger):
    combined = copy(my_local_ledger)
    pending_conflicts = []

    for each tx in incoming_ledger_packets:
        if tx.tx_id already in combined:
            continue    // duplicate

        if not verify_signature(tx) or is_stale_or_replay(tx):
            discard
            continue

        if causes_double_spend_or_balance_error(tx, combined):
            pending_conflicts.append(tx)
            continue

        combined.append(tx)

    sort_combined_by_timestamp(combined)

    if pending_conflicts is not empty:
        broadcast_conflict_alert_to_core_crew()
        flag_for_manual_review(pending_conflicts)

    update_my_balance_cache(combined)
    save_local_ledger(combined)
    rebroadcast_any_new_tx()
```

### Double-Spend Detection

```
function causes_double_spend_or_balance_error(new_tx, ledger_so_far):
    if new_tx.type != "transfer": return false

    running_balance = calculate_balance_up_to(new_tx.from, new_tx.timestamp, ledger_so_far)
    if new_tx.amount > running_balance:
        return true

    if nonce_already_used_by_sender(new_tx.from, new_tx.nonce, ledger_so_far):
        return true    // replay attempt

    return false
```

### Core Crew Manual Resolution

```
function core_resolve_conflicts(conflict_list, core_private_key):
    // Happens at weekly in-person meet (poker night / fishing trip)
    sort_by_timestamp(conflict_list)

    for each disputed_tx in conflict_list:
        decision = core_vote_or_consensus("approve", "reject", "reverse")

        if decision == "reject":
            mark_tx_invalid(disputed_tx)
        elif decision == "reverse":
            reversal = create_signed_reversal_tx(disputed_tx, core_private_key)
            append_to_ledger(reversal)
            broadcast_reversal()

    re_merge_all_ledgers_across_island()
    notify_affected_users("Dispute settled – your new balance is X")
```

---

## Real-World Inspiration Projects

| Project | Description | Relevance |
|---------|-------------|-----------|
| **Meshtbank** (Roni Bandini, 2024-2025) | Off-grid "apocalypse banking" on Meshtastic | Direct template for Beaver Island |
| **Tiny Off-Grid Payment Node** (Reddit community) | Lightweight ledger on Meshtastic boards | Client wallet reference |
| **BTC Mesh Relay** (eddieoz/btcmesh) | Bitcoin raw tx chunked over LoRa mesh | Mainland bridging model |
| **Crankk + Meshtastic** (2024) | Compose blockchain tx offline, relay when connected | Air-gapped gateway model |
| **MeshChain** (community experiment) | Privacy-focused blockchain on LoRa | Full on-chain on mesh |
| **BerkShares** (Massachusetts) | Local paper currency backed by local banks | Community currency model |
| **Ithaca Hours** | Labor-time community currency | Time-banking parallel |
| **Bristol Pound** (UK) | Local digital currency | Digital community currency |

---

## Technical Constraints & Realism Notes

- **Tx size:** Keep < 200-240 bytes to fit Meshtastic text payloads; binary protobuf for efficiency
- **Pubkey distribution:** Bootstrap list on USB from core, or include in first tx from each user
- **Scale limit:** 30+ users → mesh congestion → split channels or districts
- **Lost device:** Recover from passphrase + latest USB ledger copy held by core
- **Crypto primitives:** ECDSA (secp256k1) via micro-ecc or Python's ecdsa library
- **Ledger format:** Append-only signed tx list + balance cache (JSON or SQLite)
- **No full blockchain** — just ordered, signed transactions with manual conflict resolution

---

*"Not revolutionary tech — pragmatic adaptation of existing open-source tools to a veterans' co-op that needs to stay self-reliant and low-profile."*
