# Research outcome (phase 1), 2026-11-05

Question: how can an ESP32-class device notice that the Blum AVENTOS lift (SERVO-DRIVE) was opened and make it close five minutes later? The owner's lift is an **AVENTOS HF top**, i.e. the top-generation drive unit 23.A000 / 23.A001.

## Files

| File | What it is |
|---|---|
| `README.md` | This index plus the condensed findings. |
| `verification.md` / `verification.json` | The 14 key claims with the verifier's verdict, corrected wording, evidence and sources; completeness-critic output; merged open questions and questions for the owner. |
| `raw/research_*.json` | One file per research angle (product, regulatory, prior art, patents, behaviour, alternatives, platform, RF implementation): every finding with confidence, quoted evidence, source URLs, design impact, negative results, open questions. |
| `raw/merge-claims.json` | The merged key claims as handed to verification. |
| `raw/gapfill_*.json` | Targeted follow-ups requested by the completeness critic (present once the background run finished them). |
| `research-report.md` | Full synthesis with status tags and sources, written by the background workflow (arrives when that run completes). |
| `aventos-auto-close-plan.html` | Standalone copy of the plan page published as an artifact (draft 1.1). Diagrams render via mermaid from a CDN. |

Plan derived from this research: `../docs/plan.md` and https://claude.ai/artifact/RrvkGMAwSHqTvnYmhZUmpy

## How it was produced

Multi-agent workflow: eight research agents (Opus, max effort), each with one angle, searching in EN/DE/FR/NL/IT and reading primary sources (Blum manuals and declarations of conformity, FCC/ISED records, patents, library source code, datasheets, forums). A merge agent distilled 14 design-relevant claims; one verifier per claim acted as skeptic and independent fact-checker; a completeness critic requested gap-fill research; a synthesis agent writes the full report. Downloaded sources were kept as local copies for the verifiers. Two agents were blocked by model safeguards on the first run and re-run with neutral wording; a usage limit interrupted the first verification pass. Status at the time of writing: 13 of 14 claims verified (C08, the "no rolling code / fixed IDs" claim, failed twice and is still unverified), critic done, gap-fill and synthesis pending.

## Condensed findings

Status tags: [confirmed] primary source quoted and independently checked; [uncertain] plausible, not proven; [corrected] partly wrong in the first pass; [inferred] engineering inference.

### The owner's system
- AVENTOS HF top → SERVO-DRIVE drive unit 23.A000 (set with 1500 mm distribution cable, connecting node, end protectors) or 23.A001 (unit). FCC ID W95-23A001 (grants 2022-10-23: Part 15 Class B digital device + Part 15.249 transmitter, 2404–2452 MHz). EU since 2023, US 2024. Buttons: Link, Ok, Play ("test 2x"), Collision, Reset. [confirmed]
- Switch 23P5020 (round button, battery LED + Connect LED, FCC W95-23P502L00-01, CR2032, 10 mA rated); older 21P5020. Press-fit in a Ø35 mm bore on each carcass side. Up to 6 switches per drive, each switch bound to one drive. Linking: Link → press each switch → Ok. Synchronising two drives: Link on both → Ok → Play (reference run, max 3 drives). Collision avoidance links up to 6 drives. Reset restores factory settings. [confirmed; the "no limit documented for top" statement was corrected]
- Other families for reference: 21FA00x classic HF/HS/HL (buttons SWITCH/SYNC/COLL/Reset Motion/Reset Wireless), 23KA00x HK top. Drive label on the left lift mechanism: article number, BAU serial, FCC ID. Blum updated SERVO-DRIVE electronics in September 2025 (source: dealer/press mentions; no official change note found). [confirmed / uncertain for the 2025 change]

### Radio link
- All current radio parts share one spec: 2.404–2.452 GHz, max +4 dBm (2.5 mW), ~10 m, "bidirektional, über codierte Telegramme". EU DoCs cite EN 300 440 (generic SRD), never EN 300 328 or a sub-GHz standard. [confirmed]
- Protocol: Nordic Enhanced ShockBurst (nRF24L01+ link layer), GFSK 1 Mbps, no hopping; channel 4 (2404 MHz) operation, channel 10 (2410 MHz) sync discovery, 2452 MHz listed in an FCC record; 5-byte addresses 0xDD + reversed 3-byte device ID + pipe byte (01 switch pipe, 02 sync pipe); dynamic payloads; auto-ACK with ACK payloads; CRC-16. Confirmed at code level (Port-Net library) and independently by bluhm-de's write-up. RF chip not photographed (FCC internal photos inaccessible). [confirmed]
- Not Bluetooth: no Blum document mentions it; the band does not cover 2402–2480 MHz; DoC under EN 300 440; the switch is filed as "2.4 GHz proprietary, GFSK". The only Bluetooth mention is a US retailer bullet. [confirmed]
- Switch frame: 9 bytes "ba MM NN ff 01 20 XX YY dd" to receiver address "dd NN MM ba 01": switch ID, one fixed command (0x20), a counter; no open/close field. The drive picks the direction. Counter reportedly not checked (no rolling code) on the generation analysed. [confirmed for the frame; the "not checked" part is claim C08, unverified]
- Blum patents: 2008 family (switch ID + function code, teach-in via a connect key; block diagram labels the radio "nRF"); 2009 family (radio on a separate SPI board); 2020 family (US12698670B2 etc.) names ShockBurst/ESB for the switch link and Bluetooth/Wi-Fi for phones and a bridge; some embodiments mention encryption and time windows. No patent gives bit-level formats. [confirmed]

### The open-source library (Port-Net/BlumServoDrive)
- ESP32 + nRF24L01+ via RF24; 15 commits 2024-12-27 to 2025-01-10, v0.3.0, 0 stars; LICENSE GPL-2.0 vs library.json LGPL-3.0. Pairs through the drive's sync discovery (channel 10, address "BLUMS"), learns the drive ID, then runs on channel 4. Sends absolute moves (0x25 wake, 0x26 move with byte 12 = 0x01/0x10), receives the drive's 0x25/0x26 frames as OPEN/CLOSE callbacks. Frame types 0x27 sync identify, 0x42 wireless reset; response = request | 0x80; 8-bit sequence counter never checked. [confirmed]
- Caveats: open/close byte mapping contradicts itself between send and receive; four bugs (assignment in a condition, unassigned `_pipe`, off-by-one bounds check, NVS namespace cleared); not thread-safe; no failure callback; retries 5 × 500 ms per frame type; pairing aborts after 20 s. Author's hardware undocumented; example pins match a Seeed XIAO ESP32-C6 exactly. Drive generation tested is unknown; naming matches the classic SYNC buttons. The top generation (the owner's) is untested. [confirmed]
- Whether a sync partner is notified of every movement, including front-press openings, is an untested assumption. [uncertain]

### Operational behaviour
- Normal opening is a brief push on the front (about 2 mm bumper travel); the switch "triggers opening or closing" and in practice toggles; the flap can be moved by hand at any time; none of these produce a switch frame except the switch press. [confirmed]
- Collision detection stops opening and closing on contact, even right after a close command. Lean protection ignores presses held too long. After a mains cut the flap stays put and a switch press re-activates the drive. Duty S3 5 %, max 10 movements per 10 min. LEDs only, no beeper. No timer, auto-close, app or API exists (SERVO-DRIVE flex has a 2.5/4/8 s hold-open for appliance doors; "SERVO-DRIVE smart" is an unreleased concept). What a switch press during motion does is undocumented. [confirmed]

### Electrical and wired options
- No AVENTOS drive has a wired control input; the only cable is the two-core 24 V DC SELV distribution cable. Power supplies: Z10NE040 family 72 W/3 A, Z10NA40x 24 W/1 A plug-in (one drive), earlier Z10NE030 24 W, Z10NA300 12 W, US Z10NE020G 72 W. Blum forbids connecting other devices to its supply and recommends a switched outlet. [confirmed]
- Switch internals (daumemo teardown, HK top): PCB 23P502L00.01, test pads for +3 V, GND and the two button contacts (the pad names and the "maybe 1.8 V" rail are the blogger's annotations). Proven: 3.3 V feed plus a potential-free contact across the button pads for 0.8 s toggles the flap. Recommended actuator: PhotoMOS (e.g. AQY212EH) or opto, never a GPIO/MOSFET to ground. [uncertain for the pad details, confirmed for the method]

### Prior art
- daumemo.com (2026-02): modified 23P5020 + reed sensor + Fibaro Smart Implant (Z-Wave) on an HK top; 3D-printed housing (Thingiverse 7293224). bluhm-de.com (2023): protocol write-up, channels and addresses paywalled (€15), Arduino sketch €25. Shelly forum (2025-01): Shelly Plus Uni plan, not built. Older: Sonoff SV (HA forum 2019), Control4 forum (2024). Nothing in ESPHome, Home Assistant core, rtl_433, Flipper Zero, URH; no Reddit/Hackaday write-ups. [confirmed]

### RF implementation options
- ESP32 + genuine Nordic nRF24L01+ over SPI (clones Si24R1, BK2425/RFM7x, XN297 reportedly fail ACK-payload/dynamic-payload exchange). nRF52840 (XIAO nRF52840 with nrf_to_nrf, or Nordic ESB in nRF Connect SDK with legacy nRF24 config) is a single-chip alternative without Wi-Fi. XIAO ESP32-C3 needs a CSN pull-up or pin remap (MISO = GPIO9 strapping pin); XIAO ESP32-C6 matches the library's pins. Toolchain: arduino-esp32 3.x, RF24 1.4.10 for a faithful first test then 1.6.2 (IRQ masked by default since 1.6.0). ESPHome possible via an external component on an esp-idf nRF24 driver, not plug-and-play. [confirmed / uncertain for clone behaviour]
- Verification tools: RF24 scanner, a second nRF24 on the computed drive address, Nordic nRF52840 dongle in ESB monitor mode (~€12). An RTL-SDR cannot tune 2.4 GHz; a 2.4 GHz SDR with Universal Radio Hacker is optional. [confirmed]
- Legal: EU 10 mW e.i.r.p. (ERC/REC 70-03 band i, EN 300 440); US 47 CFR 15.249. Plain modules at low power are fine; PA+LNA modules would exceed limits at full power. Wi-Fi channel 1 overlaps 2404/2410 MHz. [confirmed / inferred]

### Non-RF building blocks
- Flap sensing: reed + magnet at the closing edge (works for HF/HS/HL/HK; actuates at 15–25 mm); ToF (VL53L0X) as a non-contact alternative; tilt/accelerometer fails on HL; current sensing gives motion but not position and modifies Blum cabling. Wire reed so a broken cable reads "not closed". [confirmed]
- Actuation: isolated contact (PhotoMOS/opto) across the button of a spare 23P5020; mechanical pusher as last resort. Firmware patterns in ESPHome or Arduino: closed → opened → wait → warn → close → confirm → retry once → fault; disable ESPHome's 15-min reboot-on-no-API for standalone use; boot-quiet outputs. [confirmed]
- Safety: keep Blum collision detection; warn 5 s audibly and visually before an unattended close (garage-door practice, 16 CFR 1211.14 pattern); presence veto (LD2410) optional; one retry then latch; respect the duty rating; IEC 60335-2-116:2025 covers motorised kitchen units; Blum's DoC for 23.A000/23.A001 cites EN 60335-1 and EN ISO 14118. [confirmed]

### Platform and enclosure
- Board: Seeed XIAO ESP32-C6 (~€8.50) recommended; XIAO ESP32-C3 (~€7.90), XIAO nRF52840 (~€11.90). Power: separate USB-C 5 V; if a 24 V source were ever used, MP1584 has little margin and Mini360 is out of spec. Battery only realistic for a deep-sleep, reed-woken, spare-switch variant; a continuous nRF24 receiver (13 mA) needs USB. [confirmed]
- Enclosure: YAPP_Box (MIT) parametric generator; BOSL2; NopSCADlib XIAO model (21 × 18 × 1.2 mm); PLA/PETG ~0.01 dB at 2.4 GHz; ≥ 15 mm antenna clearance from metal, ≥ 30 mm from the Blum PSU, cables clear of moving parts; M2 inserts 3.2 mm holes, M2.5 4.0 mm; VHB tape or screws. [confirmed]

### Still unknown
- Does the top-generation drive accept the library's sync pairing, and does it relay front-press moves to a sync partner?
- Real meaning of byte 12 in 0x26 frames (0x01 vs 0x10).
- Which channel(s) the switch itself uses for commands and programming (bluhm-de paywalls them; 2452 MHz is a guess from the FCC record).
- What changed in the September 2025 electronics update; whether post-2025 drives check counters or encrypt.
- Exact RF chip in switch and drive.
- Behaviour of a switch press during motion; collision-detection force threshold.
