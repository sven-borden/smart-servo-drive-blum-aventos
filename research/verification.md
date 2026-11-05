# Verification of the key claims

Each key claim was checked by one Opus agent acting as skeptic (open the cited sources, look for contradictions) and independent fact-checker (find a primary source not in the cited list). Verdicts: confirmed / refuted / uncertain. 'Corrected claim' is the most accurate wording after verification.

## C01 — CONFIRMED

**Claim:** SERVO-DRIVE for AVENTOS opens electrically when the front is pressed (touch-to-open, using about 2 mm of distance-bumper travel) or when a paired radio switch is pressed, and the flap can be moved by hand at any time; front-press and manual openings involve no switch transmission, so a device that only decodes switch frames cannot detect most openings.

**Why it matters:** Changes the requirement from 'catch the OPEN command' to 'detect that the flap opened'. The design needs a flap-state sensor (reed or Hall sensor on the carcass, magnet on the flap) or radio notifications sent by the drive itself (C04). The 5-minute timer must start on any open transition and be cancelled by any close.

**Corrected claim:** SERVO-DRIVE for AVENTOS opens the flap automatically in two ways. The first is a brief press on the front (touch-to-open). Blum distance bumpers on the front keep the gap that gives the required trigger path of about 2 mm (Blum 2010 and 2016 brochures; the current BA-139 leaflet says only "required distance"). Leaning on the flap does not trigger it. The second is pressing a paired, battery-powered (CR2032) SERVO-DRIVE switch. Blum says the switch "triggers the automatic opening and closing action", but its pictograms and marketing show the intended pattern: open by touching the front, close with the switch. The flap can also be opened or closed by hand at any time, including with the power off, without damaging the drive. A front-press opening is sensed by the drive unit itself, and an opening by hand involves no electronics at all, so neither produces a switch transmission. A device that decodes only switch frames therefore misses front-press and manual openings, which in Blum's intended usage are the normal way to open the flap; it would mostly see CLOSE commands. Drive units do exchange radio frames with sync and collision-avoidance partners (community code handles a drive-originated 'sync move' frame carrying the new state), so motor-driven openings may be visible as frames sent by the drive (C04). No source shows that an opening by hand produces any frame. A flap-state sensor (reed or Hall sensor on the carcass, magnet on the flap) is the only method that is reliably known to catch every open transition. The 5-minute timer should start on any open transition and be cancelled by any close.

**Evidence:** SKEPTIC CHECK OF CITED SOURCES (local copies):
(1) Blum KLA0031 BA-139 v7 EN instruction leaflet (local copy of the cited d2.blum.com ..._$sen_$aof_$v7.pdf, extract blum_docs/ba139_en.txt), section "SERVO-DRIVE for AVENTOS", pp. 10-13, says the following. "The SERVO-DRIVE switch triggers the automatic opening and closing action of SERVO-DRIVE for AVENTOS." The pictograms read "Press on the front - The lift system opens automatically" and "Press on the SERVO-DRIVE switch - The lift system closes automatically". Under "Manual operation": "The lift system can be opened or closed manually at any time without damaging SERVO-DRIVE for AVENTOS ... Manual opening of the lift system by moving it upwards". Also: "SERVO-DRIVE for AVENTOS is only triggered by a brief touch. Leaning on the lift system will not open it." And: "The SERVO-DRIVE switch is battery-operated (CR2032)." Finally: "Blum distance bumpers must be attached to the front to maintain the required distance between the lift system and the cabinet."
Caveat: this leaflet states "2 mm" only in the SERVO-DRIVE flex and drawer sections, not in the AVENTOS section. Do not cite BA-139 for the 2 mm figure.
(2) The German edition (nonrf/blumdocs/sch_utf8.txt) is identical in substance. It says "Über den SERVO-DRIVE-Schalter wird das automatische Öffnen oder Schließen ... ausgelöst" and "Druck auf die Front -> die Klappe öffnet automatisch". It also says "Die Klappe kann jederzeit uneingeschränkt manuell geöffnet oder geschlossen werden" and "nur durch kurzes Drücken ausgelöst. Wenn Sie sich an der Klappe anlehnen, erfolgt kein Öffnen."
(3) MA-502 (ldlonline copy, ldl_utf8.txt), section "AVENTOS function", has the same pictograms and says "can be opened or closed manually without restrictions".
(4) Patent US8066341 (Blum) describes, in general terms, an electric flap drive that can be activated by manual pressure on the flap (touch-latch) or by a sensor. It is generic support only, not a description of the product.

INDEPENDENT PRIMARY SOURCES NOT IN THE CITED LIST:
(5) Blum's 2010 technical brochure "Aventos Servo-Drive range technical" (Blum Australia edition), hosted at furlat.lv. It says:
- "Lift systems open with just a light touch - and then close again with the press of a button."
- "3e Blum distance bumper - The Blum distance bumper creates and maintains the required trigger path of 2mm"
- "lift systems with SERVO-DRIVE for AVENTOS can also be easily opened and closed manually at any time, e.g. when there is a power failure"
- In the FAQ: "Pulling the handle does not trigger the motion support system because SERVO-DRIVE for AVENTOS is only activated by a light touch on the front. In any event, the lift system can be opened manually at any time."
- "The SERVO-DRIVE switches are attached to the cabinet sides at the bottom."
- "While deactivated, the lift systems can continue to be opened and closed manually."
(6) Blum, Inc.'s 2016 US brochure "SERVO-DRIVE for AVENTOS" (local wwx_brochure). It says:
- "A light touch on the front using your hand or elbow is all that's required to open doors with SERVO-DRIVE for AVENTOS."
- "Comfortable and easy closing provided by the easy to reach switch on the cabinet side"
- "Distance bumpers ensure the required trigger path of 2 mm"
- "Up to three drive units can be set for synchronized motion"
- Collision avoidance means "only one front opens at a time".

TRAPS AND CONTRADICTIONS:
- No contradiction was found.
- Trap: blum.com/ch/.../servo-drive-aventos/verarbeitung-montage/ says "Auslöseweg von mindestens 2 mm", but the page is titled "SERVO-DRIVE für LEGRABOX, TANDEMBOX, MOVENTO und TANDEM". That is the drawer product, so do not use it for AVENTOS.
- The blum.com/gb page returned HTTP 403 and was not retried by other means.

THE INFERENCE PART:
- No Blum document says in so many words that a front-press opening sends no switch frame. It does follow from the architecture. The switch is a separate CR2032 push-button. The drive unit senses the front press itself. A hand opening involves no electronics at all.
- Caveat for the design: the drive units do talk to each other over radio for sync and collision avoidance. The Port-Net/BlumServoDrive code (c03_verify/raw/src_blum_servo_drive.cpp, BLUM_ACTIVE state, around lines 583-606) handles a frame sent by the drive itself (data[5]==0x26), logs "Sync Move requested from <peer> to <state>" and raises OPEN/CLOSE events. So motor-driven openings may be visible as drive-originated frames to a sync peer (see C04).
- Whether an opening by hand produces any frame is not documented anywhere.
- "Most openings" rests on Blum's intended usage pattern (open by touch, close by switch). The actual share depends on the household.

**Verifier sources:**
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/ba139_en.txt (local copy of https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/sch_utf8.txt (German Blum SERVO-DRIVE Bedienungsanleitung, SERVO-DRIVE fuer AVENTOS section)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/ldl_utf8.txt (local copy of https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf)
- https://www.furlat.lv/wp-content/uploads/2014/10/Aventos-Servo-Drive-range-technical.pdf (opened via local copy C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c01_check/furlat_technical.pdf, text extracted to C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c01_v3/furlat.txt)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/wwx_utf8.txt (Blum, Inc. 2016 brochure 'SERVO-DRIVE for AVENTOS', local copy of wwx_brochure.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c01_verify/pat/US8066341.txt (local copy of https://patentimages.storage.googleapis.com/6b/97/c5/c411ba178ec4b2/US8066341.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/raw/src_blum_servo_drive.cpp and src_blum_servo_drive.h (Port-Net/BlumServoDrive library)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/bluhm/page.html (bluhm-de 'Blum ServoDrive wireless protocol deciphered' page)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c04_check/sync_findings.txt
- https://www.blum.com/ch/de/produkte/bewegungstechnologien/servo-drive/servo-drive-aventos/verarbeitung-montage/ (fetched; turned out to be the drawer SERVO-DRIVE page, so not usable for AVENTOS)
- https://www.blum.com/gb/en/products/motion-technologies/servo-drive/servo-drive-aventos/ (HTTP 403, not accessible)

**Sources cited by research:**
- https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sde_$aof_$v7.pdf
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf
- https://eggersmannusa.com/wp-content/uploads/2017/12/Blum_ServoDrive_EN.pdf
- https://www.blum.com/at/de/produkte/bewegungstechnologien/servo-drive/servo-drive-aventos/programm/
- https://www.blum.com/us/en/products/motion-technologies/servo-drive/overview/
- https://www.manualslib.de/manual/389920/Blum-Aventos-Hf.html?page=10
- https://www.kuechen-forum.de/forum/themen/aventos-hf-mit-servodrive-manuell-oeffnen-aber-elektrisch-schliessen.25256/
- https://patentimages.storage.googleapis.com/6b/97/c5/c411ba178ec4b2/US8066341.pdf

## C02 — CONFIRMED

**Claim:** Pressing the SERVO-DRIVE radio switch toggles the flap relative to its current position (a closed flap opens, an open flap closes), and neither the switch nor its frame carries the open/closed state.

**Why it matters:** A blind press five minutes later would re-open a flap that someone already closed. Every automatic press must therefore be gated by a sensed 'open and stationary' state and confirmed afterwards. This favours any route that can send an explicit close instead (C03).

**Corrected claim:** A press on the SERVO-DRIVE radio switch (23P5020; older 21P5020) sends only a trigger. Blum's patent DE202009010203U1 describes this trigger as a signal that encodes the switch's identity. bluhm-de decoded the frame as 'ba MM NN ff 01 20 XX YY dd': switch ID, a single fixed command 0x20 and a changing counter. It has no target-position field and no open/closed field. The drive unit, not the switch, chooses the direction from its own position: a closed flap opens and an open flap closes. Blum documents that the switch triggers 'opening and closing' (BA-139). Its patent AT11904U1 says the same switch triggers the drive whether the flap is closed or open. A first-hand report confirms that a switch press opens a closed lift. In standard mounting, though, the closed front covers the switch, so users normally press it only to close and open the flap by pressing the front. The switch has no position sensing and no state display (only battery and connect LEDs), and no state comes back to the user. So an automatic press must be gated by a sensed 'open and stationary' state and confirmed afterwards. Otherwise a press five minutes later could re-open a flap someone has already closed. Not documented: a press after the flap was moved by hand (position-based behaviour is inferred), during motion, after an interrupted move, or when collision avoidance blocks opening; also, synced drives move together. The drive itself does track its state: drive-to-drive sync frames (Port-Net type 0x26) carry an explicit target byte, which is the basis for the explicit-close route (C03).

**Evidence:** No contradicting evidence found. Two independent primary sources outside the cited list support the claim.

(1) Blum BA-139 v7 instruction leaflet (cited; local EN and DE copies read).
- EN: "The SERVO-DRIVE switch triggers the automatic opening and closing action of SERVO-DRIVE for AVENTOS."
- DE: "Über den SERVO-DRIVE-Schalter wird das automatische Öffnen oder Schließen ... ausgelöst."
- The pictogram labels "Press on the front – The lift system opens automatically" and "Press on the SERVO-DRIVE switch – The lift system closes automatically".
- The switch has only "1 Battery display (LED)" and "2 Connect (LED)". It shows no position.
- On power failure: "simply touch the switch to re-activate SERVO-DRIVE function". The direction is not stated.
- The MA-502 copy uses the same "opening and closing" sentence.
- Blum brochures market the switch for closing: "Comfortable and easy closing Provided by the easy to reach switch on the cabinet side". The 2010 technical brochure says "The switch signals the drive unit via a wireless connection."

(2) AT11904U1, Julius Blum (cited; pages 1-4 read).
- [0007]: "Dasselbe Schaltelement kann daher zur Auslösung der Antriebsvorrichtung dienen, unabhängig davon, ob sich das Möbelteil in der Schließlage oder einer von der Schließlage unterschiedlichen Lage befindet."
- [0009]: closing works when the user "das nun nicht mehr durch das Möbelteil verdeckte Schaltelement unmittelbar betätigt".
- [0018]: "Das Schaltelement kann auch als Funkschalter realisiert werden."
- Caveat: the embodiment is a wired concept, not product documentation.

(3) Independent primary source, not cited: DE202009010203U1 (Julius Blum GmbH, AT priority 29.08.2008, wireless trigger switches).
- [0037]: on actuation the trigger switch "sendet ... ein entsprechendes Signal, welches (auch) seine Identität kodiert". If the ID is on the stored list, "Das Motherboard löst daraufhin den Elektromotor des Möbelantriebs A11 aus".
- So the switch signal carries its identity, and the drive alone decides what to do. No direction or state is sent.

(4) Independent primary source code, not cited: Port-Net/BlumServoDrive (local copy).
- Drive-to-drive sync frame 0x26 carries an explicit target byte (data[12] = 0x01 or 0x10).
- The library's toggleState() picks the direction from a remembered peer state. So open/closed state lives in the drive and sync layer, not in a switch frame.
- Caveat: the library maps 0x01 inconsistently between its send path and its receive path.

(5) bluhm-de reverse engineering (cited): "The switch sends the following 9 byte sequence ... ba MM NN ff 01 20 XX YY dd. MM NN is the switch's ID. XX YY is a sequence number which changes for every submission."
- That is one fixed command (0x20) with no state field.
- Caveats: single source, not replicated, and channels and addresses are withheld.

(6) First-hand reports (cited).
- Shelly forum, Andreas Wobig, 4 Jan 2025: "Die Taster lösen auf/zu mit je einem Tastendruck aus (leider ohne den Status auf oder zu bereitzustellen)". The replies recommend reed contacts or a Shelly Blu Door/Window sensor to get the state.
- woodworker.de, ascona, 3 Feb 2023: "Ohne Front öffnet der Aventos ohne Probleme beim druck auf den Schalter". So a switch press opens a closed lift.
- daumemo: shorting the 23P5020 button pins gives an "open/close command", so "the cabinet door can be opened or closed from an automation". The author uses a separate magnetic sensor for the state.
- c4forums: HTTP 403, so not checked.

Gaps and caveats:
- No source tests a press after the flap was closed by hand. The position-based direction is inferred from the drive's own position sensing.
- A MA-400/2EN line quoted by an earlier agent ("does not open when pressing on the front, but it does close when pressing the SERVO-DRIVE switch") is not in the local text extract, so I could not verify it.
- Undocumented cases: a press during motion, after an interrupted move, and in collision-avoidance or sync groups.
- In standard mounting the closed front covers the switch.

**Verifier sources:**
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/ba139_en_v7.txt and ba139_de_clean.txt)
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/ldl_utf8.txt)
- https://patentimages.storage.googleapis.com/36/28/13/dbfe09a61f9083/AT11904U1.pdf (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c02_verify/patent/AT11904U1.pdf, pages 1-6)
- DE202009010203U1, Julius Blum GmbH (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c02check/DE202009010203U1.utf8.txt) - independent, not in cited list
- https://github.com/Port-Net/BlumServoDrive (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/portnet/blum_servo_drive.h and blum_servo_drive.cpp) - independent, not in cited list
- https://www.bluhm-de.com/content/os-tools/en/projects/home-information-system/blum-servodrive-wireless-protocol-deciphered.html (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/bluhm/page.html)
- https://shelly-forum.com/thread/27978-blum-servodrive-mit-shelly-uni-steuern/
- https://www.woodworker.de/forum/threads/aventos-hl-servo-drive.125962/
- https://www.c4forums.com/forums/topic/45942-blum-servo-drive/ (HTTP 403, not accessible)
- https://daumemo.com/how-do-you-connect-a-blum-servo-drive-to-z-wave-home-assistant-system/ (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/daumemo/page.html)
- https://www.furlat.lv/wp-content/uploads/2014/10/Aventos-Servo-Drive-range-technical.pdf (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc3/furlat_technical.txt)
- https://www.woodworkerexpress.com/skin/common_files/files/specs/blum/blum-servo-drive-brochure.pdf (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc3/wwe_brochure.txt)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc4/hktop.txt (Blum HK top US technical brochure 2021)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/netsuite_troubleshooting.txt (Blum Inc. SDR for AVT FAQ 10/31/2013)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c04_check/sync_findings.txt (earlier agent notes, cross-check only)

**Sources cited by research:**
- https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf
- https://www.woodworker.de/forum/threads/aventos-hl-servo-drive.125962/
- https://daumemo.com/how-do-you-connect-a-blum-servo-drive-to-z-wave-home-assistant-system/
- https://shelly-forum.com/thread/27978-blum-servodrive-mit-shelly-uni-steuern/
- https://www.c4forums.com/forums/topic/45942-blum-servo-drive/
- https://www.bluhm-de.com/content/os-tools/en/projects/home-information-system/blum-servodrive-wireless-protocol-deciphered.html
- https://www.blum.com/us/en/products/motion-technologies/servo-drive/servo-drive-aventos/assembly/
- https://patentimages.storage.googleapis.com/36/28/13/dbfe09a61f9083/AT11904U1.pdf

## C03 — CONFIRMED

**Claim:** The open-source library Port-Net/BlumServoDrive (v0.3.0; ESP32 with nRF24L01+ through RF24; last commit 2025-01-10) pairs with a drive through the drive's own SYNC procedure as a virtual synchronised drive, reports drive movements as OPEN/CLOSE events and sends explicit (not toggle) open or close commands. It takes one of at most three sync slots, its open/close parameter encoding contradicts itself between the send and receive paths, and the drive generation it was tested on is not documented.

**Why it matters:** It is the only known working route that both observes drive movements and sends a close that is safe to repeat, without modifying Blum hardware. If it fails on the owner's drive, the design falls back to a genuine switch as actuator (C10, C11). On the classic generation, sync cannot be active together with collision avoidance, and a close sent to a synchronised group moves every front in it.

**Corrected claim:** Port-Net/BlumServoDrive by Christian Port is an ESP32-only Arduino library (espressif32) that drives an nRF24L01(+) through nRF24/RF24 ^1.4.9.
- Version 0.3.0 appears only in library.json and library.properties; there are no tags or releases.
- The LICENSE file is GPL-2.0, but library.json says LGPL-3.0.
- 15 commits, from 2024-12-27 to the last one on 2025-01-10 (3af57f625f). As of 2026-11-05 it has 0 stars, forks and issues.

What it does, judged from the code alone (the README is one line):
- It pairs with a drive through the drive's own two-sided SYNC procedure, as a synchronisation partner rather than as a switch. Discovery runs on RF channel 10 at the sync-config address 'BLUMS'; a switch address 'BLUMT' is commented out. Normal operation is on channel 4.
- Its sync-identify frame (0x27), wireless-reset frame (0x42) and address scheme (0xDD + reversed ID + pipe) match an independent capture on bluhm-de.com.
- It raises OPEN/CLOSE callbacks when a peer drive sends a 0x26 'sync move' frame, and after its own successful moves.
- sendMove(idx, open) sends a 0x25 wake frame and then a 0x26 frame with an explicit target byte: 0x10 for open, 0x01 for close. toggleState() and the bundled example toggle from the stored state instead.
- The receive path reports 0x01 as OPEN and stores the raw byte as the peer's state. The send path and toggleState() treat 0x01 as close/closed. The two mappings contradict each other, and have since the first commit. Which one is right, and whether the drive treats the target as absolute so that a repeated CLOSE does nothing, is untested.

The three-unit limit:
- Blum documents up to three drive units per synchronisation group, for the classic generation (MA-502, MA-400) and for the 2021 AVENTOS HK top generation.
- The virtual peer therefore takes one of those three places. The library itself sets no limit.

What is not documented:
- Neither the Blum product family (the README says 'shelf'; drawer and AVENTOS SERVO-DRIVE share the SYNC procedure) nor the generation it was developed on is documented.
- No independent reports of anyone using it exist.

**Evidence:** SKEPTIC CHECK (four local copies of the library source have the same md5, and a live GitHub API check today agrees with them):
- library.json has "version": "0.3.0", "platforms": ["espressif32"] and the dependency nrf24/RF24 "^1.4.9". The code uses xTaskCreatePinnedToCore and Preferences, so it is ESP32-only.
- Live API on 2026-11-05: last commit 3af57f625f, 2025-01-10T13:54:41Z, "changed pers to ptr". 15 commits since 2024-12-27, only branch main, tags [], 0 issues, forks and stars. A GitHub code search for BlumServoDrive finds only an unrelated portfolio site.
- Side findings: library.json says "LGPL-3.0" but the LICENSE file is GPL-2.0. The whole README is "Blum ServoDrive Library / allow control of shelf equipped with blum servo drive." It names no product family and no generation, and there are no issues or test reports.
- It acts as a sync peer, not a switch. In the header: "uint64_t sync_cfg_addr = 0x534D554C42LL;" ('BLUMS'), and the switch address is commented out: "//uint64_t switch_cfg_addr = 0x544D554C42LL;" ('BLUMT'). startSync() does setChannel(10) and openReadingPipe(0, sync_cfg_addr), sends a DISCOVER every 500 ms and takes a 0x82 ack. After 5 tries without an ack it waits for a discover instead. It then sends SYNC_ACK on channel 4.
- This matches Blum's two-sided procedure in MA-502: "Press the <SYNC> button for the first drive unit until the LED flashes green ... Press the <SYNC> button for the second drive unit ..."
- Events: "enum event_t {SYNC_END, SYNC_IDENTIFY, OPEN, CLOSE, NEWPEER, UNLINK};". In the receive path, "recPayload.data[5] == 0x26" ("Sync Move requested from") leads to "findPeer(peerID)->setState(recPayload.data[12]); ... _event_callback(recPayload.data[12] == 01 ? OPEN : CLOSE, ...)".
- Explicit send: "bool sendMove(uint idx, bool open);" sends a 0x25 wake frame, then a 0x26 frame built from the template "ch4_position_cmd ... 0x01}; // template set to close", with "if(_requested_open) { sendPayload.data[12] = 0x10; }".
- toggleState() treats 0x01 as closed (it opens) and 0x10 as open (it closes). The receive path treats 0x01 as OPEN. So the send and receive mappings contradict each other.
- The same lines are already in the first commit (first.cpp lines 354 and 677).
- Other bugs: "if((recPayload.len = 13) && ...)" is an assignment, not a comparison, and the member _pipe is never set.
- The library itself sets no limit on peers (std::vector).
- The three-unit limit comes from Blum. MA-502 and MA-400/2EN: "Up to three drive units can be synchronised allowing them to move simultaneously." MA-400 troubleshooting #11/#12: sync fails while "Collision avoidance is activated", and the reverse. Troubleshooting #9: "Two or three fronts open or close in unison: Synchronisation was activated in error".

INDEPENDENT PRIMARY SOURCES (not in the cited list):
1. bluhm-de.com, a first-hand protocol write-up:
   - "Inside the buttons are nrf24L01+ transceivers", running on "at least three different frequencies" for buttons, programming and syncing.
   - Sync button: "BA MM NN 00 01 27 00 YY DD BA UU VV 01 is sent to the sync address of the stored partner(s)".
   - Wireless reset: "BA MM NN 00 01 42 00 YY DD BA UU VV 01 ... removes the sync programming on the synced drives".
   - Address: "dd NNMM ba01".
   - These match the library's ch4_sync_button_cmd and ch4_reset templates and addressFromID() byte for byte. The one difference is byte 12 of the 0x27 frame: devID 0x94 in the library, 01 in the capture. This supports the virtual sync-partner design.
   - The page's Sync section is empty and it shows no 0x25 or 0x26 frames, so it cannot settle the 0x01/0x10 question. It names no model.
   - It also offers a paid, closed "virtual switch/cabinet sketch". So "only known route" holds only for open-source code.
2. Blum, Inc. 2021 AVENTOS HK top technical brochure (newer generation): "Up to three drive units can be synchronized allowing them to move simultaneously." Pairing there uses link and OK buttons instead of <SYNC>. The 2014/2016 US brochures and the 2010 technical brochure also say three.

No contradicting evidence found. What remains unverified on hardware:
- which 0x26 byte value means close;
- whether a repeated CLOSE does nothing;
- which product family and generation the library works with.

**Verifier sources:**
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/raw/src_blum_servo_drive.cpp
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/raw/src_blum_servo_drive.h
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/raw/library.json
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/raw/library.properties
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/raw/README.md
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/raw/examples_basic_basic_controller.cpp
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/raw/LICENSE
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/first.cpp
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/first.h
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/commits.json
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/org_repos.json
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_check/repo_live.json
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_check/commits_live.json
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_skeptic/commits_now.json
- https://api.github.com/repos/Port-Net/BlumServoDrive/commits?per_page=3
- https://api.github.com/repos/Port-Net/BlumServoDrive
- https://api.github.com/repos/Port-Net/BlumServoDrive/branches
- https://api.github.com/repos/Port-Net/BlumServoDrive/issues?state=all
- https://api.github.com/search/code?q=BlumServoDrive
- https://www.bluhm-de.com/content/os-tools/en/projects/home-information-system/blum-servodrive-wireless-protocol-deciphered.html
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/bluhm/page.html
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/ldl_ma502.txt (https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/eggersmann_servo_en.txt (https://eggersmannusa.com/wp-content/uploads/2017/12/Blum_ServoDrive_EN.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc4/hktop.txt (Blum Inc. 2021 AVENTOS HK top technical brochure, https://d2.blum.com/services/BEC003/aventoshktoptech_ep_dok_bus_$sen-us_$aof_$v5.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc3/wwe_brochure.txt (Blum Inc. 2016 SERVO-DRIVE for AVENTOS brochure)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc3/furlat_technical.txt (https://www.furlat.lv/wp-content/uploads/2014/10/Aventos-Servo-Drive-range-technical.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c04_check/sync_findings.txt (earlier-agent notes, secondary)

**Sources cited by research:**
- https://github.com/Port-Net/BlumServoDrive
- https://github.com/Port-Net/BlumServoDrive/blob/main/src/blum_servo_drive.cpp
- https://github.com/Port-Net/BlumServoDrive/blob/main/src/blum_servo_drive.h
- https://github.com/Port-Net/BlumServoDrive/blob/main/library.json
- https://github.com/Port-Net/BlumServoDrive/blob/main/examples/basic/basic_controller.cpp
- https://github.com/Port-Net/BlumServoDrive/commits/main
- https://raw.githubusercontent.com/Port-Net/BlumServoDrive/HEAD/LICENSE
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf
- https://eggersmannusa.com/wp-content/uploads/2017/12/Blum_ServoDrive_EN.pdf

## C04 — UNCERTAIN

**Claim:** Once the ESP32 is paired as a sync partner, the drive sends it a move notification for every electric movement, including openings triggered by a front press, so OPEN can be detected over the radio without a separate flap sensor; movements made by hand are expected not to be reported.

**Why it matters:** Decides whether the reed sensor is the primary OPEN detector or only a cross-check, and whether front-press openings can be timed without extra hardware.

**Corrected claim:** This is an untested assumption. Suppose the owner's drive accepts the ESP32 as a SYNC partner. Port-Net emulates the old SYNC-button pairing and does not say which drive generation it supports. A synced drive can then send the ESP32 a 0x25 wake frame followed by a 0x26 move-to-position request, and the Port-Net library raises an OPEN/CLOSE event when a 0x26 arrives.

Blum documents only that synchronised drives "move simultaneously" (several cabinets sharing one wide front) and that the flap can be moved by hand at any time. No Blum manual, patent, the Port-Net code or the bluhm-de write-up says the drive sends such a frame for front-press openings, for every electric movement, or never for hand movements. Blum's patents describe sync and collision status as shared or queried when a drive is triggered. The library also reads 0x01 as OPEN on receive, while its send side uses 0x10 for open, so the direction label may be inverted.

Until a bench test on the owner's drive shows which events produce frames and how byte 12 encodes the target, the reed sensor should stay the primary OPEN detector. The test should cover a front press, a switch press, opening and closing by hand, a collision stop and power-on. The sync frames can then be used as an optional cross-check.

**Evidence:** SKEPTIC CHECK OF CITED SOURCES
(1) Port-Net blum_servo_drive.cpp (local copy c06_dl/portnet_live/live.cpp, which matches the GitHub main file). Only the RECEIVE path bears on C04. In state BLUM_ACTIVE every received frame is ACKed:
- data[5]==0x25 prints "Sync Move wake received."
- 0x26 prints "Sync Move requested from: <peer> to: <data[12]>", calls setState(data[12]) and fires `_event_callback(recPayload.data[12] == 01 ? OPEN : CLOSE, ...)`.
- 0x27 raises SYNC_IDENTIFY and 0x42 unlinks the peer.
So a synced drive can send its partner a wake frame plus a move REQUEST (move to a target), not a status report. Neither the code nor the one-line README ("allow control of shelf equipped with blum servo drive.") says which events make the drive send it: front press, switch, every electric move, or never on a manual move. The repo has 0 issues, 0 forks and no test report. The receive mapping (0x01 = OPEN) also contradicts the library's own send path: sendMovePos() sets `if(_requested_open) data[12] = 0x10;`, and its template value 0x01 means close. The OPEN/CLOSE label may therefore be inverted.

(2) MA-502/3 (ldlonline) and BA-139 v7 (EN/DE) say:
- "Press on the front: The lift system opens automatically. Press on the SERVO-DRIVE switch: The lift system closes automatically"
- "The lift system can be opened or closed manually without restrictions"
- "Up to three drive units can be synchronised allowing them to move simultaneously. This function is required for several cabinets with a uniform front."
Nothing on radio traffic, on which triggers reach synced units, or on manual moves within a sync group.

(3) DE202009010203U1 [0043]-[0050]: synced drives are "nur gemeinsam (synchron) auslösbar" (triggered only together), taught with the connect key in "synchro" mode. Collision avoidance works by QUERY at trigger time: the first drive "fragt ... bei der Verifikationseinrichtung 1 des zweiten Möbelantriebs A21 nach, in welcher Schließlage sich das bewegbare Möbelteil ... befindet" (asks the second drive what closed position its part is in), using a position sensor. Nothing is broadcast per movement, and the patent covers trigger switches, not front presses.

(4) US12499751B2 (USPTO PDF, cols 3 and 7, read as page images): "status data describe the position and/or movement status of at least one furniture drive. In particular, this can be used for avoiding collisions and/or for synchronising two driven furniture parts, when two furniture drives share data among each other". Also: "When the command for opening is sent to a first furniture element ... the status of a second furniture element 1 can be conveyed to the first". The wording is permissive ("can") and describes a generic ESB+Bluetooth design. Nothing on triggers, front presses or hand movement, and its example again sends status in response to a command.

INDEPENDENT SOURCES (not cited)
- bluhm-de.com "Blum ServoDrive Wireless Protocol Deciphered" (separate decoding effort): documents only the switch toggle 0x20, pairing, sync-identify 0x27 and reset 0x42. Its "Collision" section is empty and it shows no drive-to-drive move frames, so it neither confirms nor refutes.
- Blum US brochures (2014/2016): "Up to three drive units can be set for synchronized motion. Synchronization is ideal for multiple cabinets that share one wide door." Also: "can also be easily opened and closed manually at anytime, e.g. when there is a power outage."
- Blum DE manual: "Bis zu drei Antriebseinheiten können synchronisiert werden und bewegen sich dadurch gleichzeitig. Diese Funktion wird bei mehreren Korpussen mit durchgängiger Front benötigt." (up to three drives move together, for several cabinets with one continuous front). Because synced units share one front, a press may reach several drives mechanically. Blum's sync use case therefore does not prove a front-press trigger is passed on by radio.
- Blum US troubleshooting page: "collision avoidance is active if another front is open in the same collision group", remedy "Close all fronts manually". Drives learn a linked front is open however it was opened, which fits the query model. "Hand movements are not reported" is therefore also unverified.
- Searches: GitHub shows no other radio implementation or test report. One German web search returned only Blum manuals and videos, no first-hand reports.

CONCLUSION: No primary source confirms which events are covered (front press yes, every electric move, manual no). Nothing concretely refutes it either. It must be tested on the owner's drive.

**Verifier sources:**
- https://github.com/Port-Net/BlumServoDrive/blob/main/src/blum_servo_drive.cpp (local copy C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c06_dl/portnet_live/live.cpp)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c06_dl/portnet_live/live.h
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/gh_portnet/README.md
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf (local text C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/ldl_utf8.txt)
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf (local texts C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/ba139_en.txt and C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/fcc_dl/kla0031_ba139_u.txt)
- https://patentimages.storage.googleapis.com/a3/d4/8e/5db82b4f715ae0/DE202009010203U1.pdf (local text C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c02check/DE202009010203U1.utf8.txt)
- https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12499751 (downloaded to C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c04_v2_pat/US12499751.pdf, pages 11-13 read)
- https://www.bluhm-de.com/content/os-tools/en/projects/home-information-system/blum-servodrive-wireless-protocol-deciphered.html (local copy C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/bluhm/page.html)
- https://cdnimages.opentip.com/Docs/HDL/ServoDriveAventos_hl.pdf (local text C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/opentip_utf8.txt)
- https://www.woodworkerexpress.com/skin/common_files/files/specs/blum/blum-servo-drive-brochure.pdf (local text C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/wwx_utf8.txt)
- https://www.ballerina.de/media/config/media/content/downloads/montageanleitungen/montage_blum5.pdf (local text C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/bal_utf8.txt)
- https://www.blum.com/us/en/landingpages/products/servo-drive/troubleshooting/
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c04_check/sync_findings.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c02_verify/gh/ (GitHub search results)

**Sources cited by research:**
- https://github.com/Port-Net/BlumServoDrive/blob/main/src/blum_servo_drive.cpp
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf
- https://patentimages.storage.googleapis.com/a3/d4/8e/5db82b4f715ae0/DE202009010203U1.pdf
- https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12499751

## C05 — UNCERTAIN

**Claim:** There are three current drive families: 21FA00x for HF/HS/HL, 23KA00x for HK top, and 23.A00x for HF/HS/HL top (since 2023 in the EU, 2024 in the US); legacy units are 21KA/21LA/21SA. They use different radio modules (FCC W95-21A00L33, W95-21FA00Y, W95-23A00L83, W95-23A001) and different pairing buttons (<SWITCH>/<SYNC>/<COLL>/<Reset Wireless> on classic drives versus <Connect/Link>/<OK>/<Play>/collision/reset on top drives). The published protocol work has not been confirmed on the top generation or on drives with the electronics update shipped from September 2025.

**Why it matters:** All protocol evidence comes from unidentified hardware dating from 2023–2025. The owner's drive generation decides whether the nRF24 route is likely to work and which pairing steps apply, so photos of the label and the button panel are needed before buying radio hardware.

**Corrected claim:** Blum's current SERVO-DRIVE for AVENTOS drive units (BA-139/7, 11.25) come in three families:
- 21FA00x for classic AVENTOS HF/HS/HL. The current revision is 21FA000/001/005 with suffix .21 or .22; its EU declaration of conformity is dated 2025-06-26.
- 23KA00x for AVENTOS HK top.
- 23.A00x for AVENTOS HF/HS/HL top.

The two top drives have EU declarations dated 2023-09-06. US marketing for the top generation dates from 2024. Legacy drives are 21LA/21SA (HL/HS), 21KA (classic HK) and the older 21FA without a suffix.

The FCC/IC IDs on the drive label differ by generation:
- Legacy 21xA00y drives contain module W95-21A00L33 (IC 8352A-21A00L33).
- Revised 21FA00x.22 drives carry W95-21FA00Y (IC 8352A-21FA00Y).
- HK top 23KA001 contains radio module 23.A00L83 (W95-23A00L83).
- HF/HS/HL top 23.A001 carries W95-23A001 (IC 8352A-23A001).

W95-21FA00Y and W95-23A001 are grants for whole drive units, not modules.

Pairing controls also differ:
- Classic 21xA drives have <Reset Motion>, <SWITCH>, <SYNC>, <COLL> and <Reset Wireless>.
- HK top has <Connect> (called <Link> in US documents), <Ok>, <Play>, <Collision> and <Reset>.
- The US SERVO-DRIVE AVENTOS pictograms show the same panel for the top drive.

No published protocol work (2023-2025) names the drive generation it was tested on. So it is unconfirmed on the top generation and on the revised 21FA00x.21/.22 drives.

Blum's May 2025 'new optimised version', with upgraded electronics in all drive units and available from September 2025, covers only SERVO-DRIVE for box and runner systems, SERVO-DRIVE uno and SERVO-DRIVE flex. It does not name SERVO-DRIVE for AVENTOS. To identify the generation, use the article-number suffix and the FCC ID on the label, not the purchase date.

**Evidence:** Most of the claim is confirmed by primary Blum documents. One sub-clause is contradicted: the September 2025 electronics update is not documented for AVENTOS drives. The US year is only partly supported.

(1) BA-139/7 (EN, 11.25)
- Technical data: 'SERVO-DRIVE drive unit for AVENTOS 21FA00x' and 'SERVO-DRIVE drive unit for AVENTOS top 23.A00x, 23KA00x'.
- EU declaration: 'machine types 23.A001, 23KA001, 21FA000.22, 21FA001.22, 21FA005.22'.
- Marking pages (rendered pp. 25-28): 'The drive unit "23KA001" contains the radio module "23.A00L83". Contains IC: 8352A-23A00L83 ... Contains FCC ID: W95-23A00L83'. 23.A001: 'IC: 8352A-23A001 ... FCC ID: W95-23A001'. 21FA000.22/21FA001.22/21FA005.22: 'IC: 8352A-21FA00Y ... FCC ID: W95-21FA00Y'.

(2) Older BA-139/3 (Schachermayer copy)
- Marking page '21xA00y, Z10C5007': 'Constains IC: 8352A-21A00L33 ... Contains FCC ID: W95-21A00L33'.
- Its declaration lists the radio types 21FA001, 21LA001, 21SA001 and 21KA001.

(3) MA-502/3 (LDL copy)
- Declaration lists 23KA001/000, 21FA001/000, 21LA001/000, 21SA001/000 and 21KA001/000.
- US supplier declaration (SDoC): '23KA00y ..., 23.A00L83' and '21xA00y | x in {F, L, S}'.
- Classic button layout: '<Reset Motion>, <SWITCH>, <SYNC>, <COLL>, <Reset Wireless>'.
- 'SERVO-DRIVE for AVENTOS HK top' layout: '<Reset>, <Collision>, <Play>, <Ok>, <Connect>'.
- The German copy says the same.

(4) US HK top technical document v5: '<Play> button', '<Ok> button', '<Link> button', 'Press collision button', 'Press reset button'.

(5) US MA-1098/0ML (09.23), pp. 7-8: the pictograms show a top-style panel (link, ok, 'test 2x' play, collision, reset). There are no text labels.

(6) EU declarations of conformity
- 23.A000/23.A001 'fuer AVENTOS HF-/HL-/HS- top', signed 2023-09-06 (cme205108).
- 23KA000/23KA001 'fuer AVENTOS HK top', signed 2023-09-06 (me199043).
- 21FA000/001/005 .21 and .22 'fuer AVENTOS', citing Regulation (EU) 2023/826, signed 2025-06-26 (cme185362).

(7) Independent sources not in the cited list
- Blum US SERVO-DRIVE brochure: 'SERVO-DRIVE set 21FA000' with cover sets 21F8000.NA, 21S8000.NA and 21L8000.NA (HF/HS/HL); 'SERVO-DRIVE set 21KA000 Cover set 21K8000.NA' (HK).
- Blum US AVENTOS top brochure, LIT.AVT3001.06.24, (c) 2024: 'our newest generation of AVENTOS lift systems'.

(8) Contradiction: Blum press release 'SERVO-DRIVE - new optimised version' (May 2025).
- It says 'electronics in all drive units being upgraded' and 'available from September'.
- It continues: 'This relates to all three product variants: SERVO-DRIVE for box and runner systems, SERVO-DRIVE uno ..., SERVO-DRIVE flex'.
- AVENTOS is not among them. The change that does matter for AVENTOS is the 21FA00x.21/.22 revision with new FCC and IC IDs.

(9) Nuances and access limits
- W95-23A001 and W95-21FA00Y are grants for whole drive units, not modules.
- The FCC database returned 'Access Denied'. fccid.io and electric.garden returned 403, and fcc.io returned 404. These were not circumvented.
- A search snippet dates the W95-23A001 grants to 2022-10-23. This is unverified.

**Verifier sources:**
- https://www.blum.com/su/en/company/press/press-releases-detail_94208.html
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf (local C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/ba139_en.pdf pp.24-29 and ba139_en.txt)
- https://www.blum.com/file/cme185362_zf_dok_bau?country=de&language=de (local C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme185362_zf.pdf pp.1-3)
- https://www.blum.com/file/me199043_zf_dok_bau?country=de&language=de (local C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/fcc_dl/doc_me199043.pdf pp.2-3)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme205108_zf.txt and me8343120_zf.txt (Blum EU DoC 23.A000/23.A001)
- https://webshop.schachermayer.com/cdn/medias/docus/175/webd_Blum_Servodrive_Montageanleitung.pdf (local C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/sch_utf8.txt)
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf (local C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/ldl_utf8.txt)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/prodid/dl3/servo-drive-montageanleitung.txt (German MA-502 copy)
- https://d2.blum.com/services/BEC003/aventoshktoptech_ep_dok_bus_$sen-us_$aof_$v5.pdf (local C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/rfimpl/txt/hktop_tech_us.txt)
- https://www.blum.com/file/cme90761_ma_dok_bau?country=us&language=en (local C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme90761_ma.pdf pp.7-8)
- https://www.blum.com/file/cme105764_ep_dok_bus?country=us&language=en (local C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/prodid/dl4/cme105764_ep_dok_bus_us_en.txt) - not in cited list
- https://www.woodworkerexpress.com/skin/common_files/files/specs/blum/blum-servo-drive-brochure.pdf (local C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c02_verify/brochure/wwe_brochure.txt) - not in cited list
- https://fccid.io/W95 (HTTP 403, inaccessible)
- https://electric.garden/julius-blum-gmbh-w95/servo-drive-for-aventos-hf-hs-hl-top-23a001 (HTTP 403, inaccessible)
- https://apps.fcc.gov/oetcf/eas/reports/GenericSearchResult.cfm (Access Denied, inaccessible)

**Sources cited by research:**
- https://www.blum.com/de/de/produkte/bewegungstechnologien/servo-drive/servo-drive-aventos/programm/
- https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de
- https://webshop.schachermayer.com/cdn/medias/docus/175/webd_Blum_Servodrive_Montageanleitung.pdf
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf
- https://www.blum.com/file/cme90761_ma_dok_bau?country=us&language=en
- https://d2.blum.com/services/BEC003/aventoshktoptech_ep_dok_bus_$sen-us_$aof_$v5.pdf
- https://www.blum.com/file/cme106574_fl_dok_bus?country=us&language=en
- https://www.blum.com/file/me8343120_zf_dok_bau?country=de&language=de
- https://www.blum.com/file/me199043_zf_dok_bau?country=de&language=de
- https://www.blum.com/file/cme185362_zf_dok_bau?country=de&language=de
- https://fccid.io/W95-23A00L83
- https://electric.garden/julius-blum-gmbh-w95/servo-drive-for-aventos-hf-hs-hl-top-23a001
- https://www.blum.com/su/en/company/press/press-releases-detail_94208.html

## C06 — CONFIRMED

**Claim:** The radio link is compatible with Nordic Enhanced ShockBurst (nRF24L01+ style): GFSK at 1 Mbps on fixed channels (2404 and 2410 MHz used by the library for operation and sync pairing; 2452 MHz listed in an FCC record), 5-byte addresses derived from 3-byte device IDs, dynamic payload length, auto-acknowledge with ACK payloads, and 16-bit CRC. The RF chip is most likely an nRF24-family part, but no photo confirms it.

**Why it matters:** Fixes the radio parameters (1 Mbps, ESB, dynamic payloads, ACK payloads, CRC-16, the channel set) and rules out sub-GHz modules, BLE and 802.15.4 stacks.

**Corrected claim:** The current-generation Blum SERVO-DRIVE radio link (switch 23P5020; drive units 21FA00x, 23.A00x, 23KA00x and flex Z10C500x) works with an nRF24L01+ running Nordic Enhanced ShockBurst. It uses GFSK at 1 Mbps on fixed channels with no hopping. The Port-Net library uses RF_CH 4 (2404 MHz) for normal operation and RF_CH 10 (2410 MHz) for drive-to-drive sync discovery and pairing. Addresses are 5 bytes. The operating address is 0xDD + the 3-byte device ID in reverse byte order + a pipe byte (0x01 for commands, 0x02 for sync), matching Bluhm's 'dd NNMM ba01'. Sync discovery instead uses the fixed address 0x534D554C42 ('BLUMS', LSByte first). The link uses dynamic payload length, auto-acknowledge with ACK payloads, and a 2-byte CRC (CRC-16, X^16+X^12+X^5+1). The CRC-16 is inferred: the library calls disableCRC(), but auto-ack forces EN_CRC on and RF24's init leaves CRCO=1. Blum declares a 2.404-2.452 GHz band at up to +4 dBm / 2.5 mW, and Bluhm reports at least three frequencies (button, programming, sync). So a third channel exists but is unidentified, probably at the 2452 MHz upper band edge and possibly for switch programming. FCC listings, seen only in search snippets because the FCC pages returned HTTP 403, give 2404-2452 MHz for the 2019 radio switch W95-23P502L00 but 2403-2477 MHz for the 2009-era W95-21P5020/W95-21A00L33, so first-generation hardware may use a different channel plan. The RF IC is unconfirmed. Bluhm says 'nrf24L01+' without a photo, and the only PCB photo shows one QFN whose marking is unreadable. Blum's +4 dBm rating is above the nRF24L01+'s 0 dBm maximum, so the part may be an ESB-compatible Nordic system-on-chip (e.g. nRF51/nRF52) or a clone rather than a discrete nRF24L01+. Operation at 2.4 GHz rules out sub-GHz modules. ESB framing (5-byte address, CRC-16, ACK payloads) rules out BLE and 802.15.4.

**Evidence:** SKEPTIC CHECK OF CITED SOURCES
(1) Port-Net library. The three local copies of src/blum_servo_drive.cpp are identical (663 lines), and the first commit has the same settings. begin(): `setDataRate(RF24_1MBPS); enableDynamicPayloads(); disableCRC(); enableAckPayload(); setChannel(4); closeReadingPipe(0); openReadingPipe(1, addressFromID(_id, 1)); openReadingPipe(2, 0x02);`. addressFromID is commented "// 0xDD LL NN MM 0X" and returns 0xdd00000000 | id bytes in reverse order | pipe index, giving a 40-bit (5-byte) address from the 3-byte ID. Sync discovery calls `setChannel(10); openReadingPipe(0, sync_cfg_addr);`. The header sets `uint64_t sync_cfg_addr = 0x534D554C42LL;`. This is a FIXED address ("BLUMS", LSByte first), not one derived from a device ID. The header also has a commented-out `switch_cfg_addr = 0x544D554C42LL`. Send path: `openWritingPipe(address); write(...)`, then `getDynamicPayloadSize()`/`read()` read the ACK payload. Receive path: `writeAckPayload(pipe, ...)`. The library never calls setAutoAck(false). library.json depends on nrf24/RF24.
(2) CRC caveat. The library calls disableCRC(), and in RF24.cpp that clears only EN_CRC. But RF24 _init_radio first writes CONFIG with EN_CRC|CRCO ("16-bit CRC (CRC required by auto-ack)"), and also sets `EN_AA, 0x3F` and `setAddressWidth(5)`. The nRF24L01+ datasheet I downloaded says: "EN_CRC ... Enable CRC. Forced high if one of the bits in the EN_AA is high". It also says: "The CRC is the mandatory error detection mechanism in the packet. It is either 1 or 2 bytes"; "The polynomial for 2 byte CRC is X16+ X12 + X5 + 1. Initial value 0xFFFF." So a 2-byte CRC is in effect, but this is inferred, not captured on air. RF24's own getCRCLength() returns CRC_16 when AA is set and CRCO=1.
(3) Datasheet: "F0= 2400 + RF_CH [MHz]", so channel 4 = 2404 MHz and channel 10 = 2410 MHz. "The radio front end uses GFSK modulation". "Programmable output power: 0, -6, -12 or -18dBm".
(4) Bluhm page (first-hand, no photo): "Inside the buttons are nrf24L01+ transceivers. They are working on at least three different frequencies: 1. Button communication 2. Programming 3. Syncing". "The cupboard receiver sends the following sequence as Ack Payload". "The receiver's ID is reversed and stored in the switch to be used as transmission address 01 (i.e. dd NNMM ba01)". The author withholds the channel numbers.
(5) Cited patents: the local text extracts of US12698670B2, US12499751B2, US20230165384A1 and DE202009010203U1 (plus WO2021243390A1 and US20230213904A1) never mention 2.4 GHz, Nordic/nRF, ShockBurst, Bluetooth, ZigBee, 868 MHz or CRC. They neither support nor contradict C06.
(6) daumemo PCB photo (switch board, silkscreen "23P502L00.01 S1a"): the visible side shows one QFN IC with an unreadable marking, a 2x3 pad programming header and a wire antenna. The chip cannot be identified from it.

INDEPENDENT PRIMARY SOURCE (not cited)
Blum SERVO-DRIVE instruction leaflet kla0031 BA 139, EN and DE versions. It covers "drive unit for AVENTOS 21FA00x", "AVENTOS top 23.A00x, 23KA00x", "SERVO-DRIVE flex drive unit Z10C500x" (with wireless receiver) and "SERVO-DRIVE switch 23P5020", each with: "Frequency band: 2.404 to 2.452 GHz", "Maximum radio-frequency power: +4 dBm / 2.5 mW", "Range: typically 10 m". The band edges are exactly nRF24 channels 4 and 52. This confirms 2.4 GHz operation (ruling out sub-GHz) and shows that 2452 MHz is the declared upper band edge. An older Blum leaflet (Schachermayer copy) gives the 21P5020 switch as "FCC ID: W95-21P5020", "IC: 8352A-21P5020", "Frequenz: 2.4 GHz", "Bidirektional, über codierte Telegramme".

CAVEATS
- +4 dBm is above the nRF24L01+'s 0 dBm maximum. Either the figure includes antenna gain (EIRP), or the chip is a different ESB-compatible part. My inference: Nordic nRF51/nRF52 system-on-chip parts reach +4 dBm.
- The FCC pages returned HTTP 403: apps.fcc.gov GenericSearch for W95, fccid.io/W95-23P502L00, device.report/julius-blum/23p502l00, and electric.garden. I did not try to get around this. Search snippets I could not verify list W95-23P502L00 "Radio Switch" (DTS, 2019-09-18) and W95-23P502L00-01 at 2404-2452 MHz. They list the 2009-era W95-21P5020 (switch) and W95-21A00L33 (module) at 2403-2477 MHz. So "2452 MHz" in the FCC record is the top of a range, and older hardware may use a different channel plan.
- A WebSearch found no other independent protocol implementation.

No primary evidence contradicts ESB framing, 1 Mbps GFSK, the 2404/2410 MHz channels, ID-derived 5-byte addresses, dynamic payloads, ACK payloads or the 2-byte CRC.

**Verifier sources:**
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c06_dl/portnet_live/live.cpp (local copy of https://github.com/Port-Net/BlumServoDrive/blob/main/src/blum_servo_drive.cpp)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c06_dl/portnet_live/live.h
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c03_verify/first.cpp
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/gh_portnet/library.json
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c06_dl/rf24/RF24.cpp (local copy of https://raw.githubusercontent.com/nRF24/RF24/master/RF24.cpp)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/bluhm/page.html (local copy of https://www.bluhm-de.com/content/os-tools/en/projects/home-information-system/blum-servodrive-wireless-protocol-deciphered.html)
- https://www.sparkfun.com/datasheets/Components/SMD/nRF24L01Pluss_Preliminary_Product_Specification_v1_0.pdf (downloaded to C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c06_verify2/ds/nrf24l01p_ds.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/ba139_en.txt (Blum SERVO-DRIVE instruction leaflet kla0031 BA 139 EN; origin per blum_docs/urls.txt, e.g. https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/fcc_dl/kla0031_ba139.txt (same leaflet, German)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/sch_utf8.txt (older Blum SERVO-DRIVE leaflet, Schachermayer copy, FCC ID W95-21P5020)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c06_txt/ (text extracts of US12698670B2, US12499751B2, US20230165384A1, DE202009010203U1, WO2021243390A1, US20230213904A1)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/daumemo/crop_pcb_components.png (crop of https://daumemo.com/wp-content/uploads/2026/02/Blum-HK-Top-Aventos-disassembled-servo-drive-trigger-button.jpg)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/crops/pcb_component_side.png

**Sources cited by research:**
- https://github.com/Port-Net/BlumServoDrive/blob/main/src/blum_servo_drive.cpp
- https://www.bluhm-de.com/content/os-tools/en/projects/home-information-system/blum-servodrive-wireless-protocol-deciphered.html
- https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12698670
- https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12499751
- https://patentimages.storage.googleapis.com/f3/1e/63/fd2f1289140f01/US20230165384A1.pdf
- https://patentimages.storage.googleapis.com/a3/d4/8e/5db82b4f715ae0/DE202009010203U1.pdf
- https://device.report/julius-blum/23p502l00
- https://www.sparkfun.com/datasheets/Components/SMD/nRF24L01Pluss_Preliminary_Product_Specification_v1_0.pdf
- https://raw.githubusercontent.com/nRF24/RF24/master/RF24.cpp
- https://daumemo.com/wp-content/uploads/2026/02/Blum-HK-Top-Aventos-disassembled-servo-drive-trigger-button.jpg

## C07 — UNCERTAIN

**Claim:** The ESP32's built-in radio cannot exchange these ESB frames, so the device needs either an external genuine Nordic nRF24L01+ on SPI or an nRF52840 running Nordic ESB in nRF24 legacy mode; Si24R1, BK242x/RFM7x, SE8R01 and XN297 clones reportedly cannot do the required dynamic-payload/ACK-payload exchange.

**Why it matters:** Sets the parts list. Option 1: an ESP32 board (e.g. XIAO ESP32-C6, which matches the library's example pins) plus a plain PCB-antenna nRF24L01+ with 10 µF and 100 nF capacitors at its pins. Option 2: a single-chip XIAO nRF52840, which has no Wi-Fi and needs the library ported. Buy modules from two sources and test them before fixing the enclosure design.

**Corrected claim:** The ESP32's own radio cannot exchange the nRF24-style Enhanced ShockBurst frames that the BlumServoDrive library uses. On the ESP32-C6 that radio covers Wi-Fi 6, BLE 5 (125k, 500k, 1M and 2M) and 802.15.4 O-QPSK, and no proprietary GFSK/ESB mode is documented.

The frames are: 1 Mbps, channel 4 for operation and channel 10 for pairing, 5-byte addresses, dynamic payload length, and auto-ACK with ACK payloads. Because auto-ACK forces the nRF24L01+ to keep its CRC on, the library's disableCRC() call has no effect and the frames carry a 16-bit CRC.

So the device needs one of two radios:
- an external transceiver on SPI that behaves like a genuine Nordic nRF24L01+ (the library is written for the RF24 driver), or
- a Nordic nRF5x chip such as the nRF52840 running ESB, configured as ESB_PROTOCOL_ESB_DPL at 1 Mbps with a 16-bit CRC and ACK payloads. Do not use the NCS ESB_LEGACY_CONFIG preset: it is fixed-length, 2 Mbps and 8-bit CRC. Nordic's driver already copies the nRF24L01's inverted ACK bit.

Clones:
- Si24R1 sends the NO_ACK/ACK bit with the opposite polarity to genuine parts in dynamic-payload mode, so auto-ACK and ACK-payload exchange with genuine radios fails. Only a workaround on the sending side is known.
- XN297/XN297L uses a different on-air format (3-byte fixed preamble, 10-bit packet control field, optional scrambling).
- SE8R01 is reported as not compatible.
- BK242x (RFM70/73/75, and many epoxy-blob 'nRF24L01+' fakes) can be switched to genuine-compatible NO_ACK polarity with an undocumented bank-1 'compatible mode' bit. That only works after a special, undocumented startup sequence that RF24 does not perform, so it is not a drop-in part for this library.

All of this assumes the Blum radios behave like genuine Nordic parts; their RF chip has not been verified.

Option 1: a XIAO ESP32-C6, whose pins match the library example (CSN on GPIO0/D0, CE on GPIO1/D1, LED on GPIO15, default SPI pins SCK 19, MISO 20, MOSI 18), plus a PCB-antenna nRF24L01+. Fit a low-ESR 10 µF capacitor (RF24 recommends electrolytic or tantalum) and a 100 nF capacitor across VCC and GND at the module.

Option 2: a single-chip XIAO nRF52840. It has BLE but no Wi-Fi, and the library has to be ported away from the RF24 API, the ESP32 task function xTaskCreatePinnedToCore and ESP32 Preferences storage. If you use TMRh20's nrf_to_nrf, check the ACK behaviour on the bench first, because v1.6.0 never sets the ACK bit that Nordic's driver sets.

Buy modules from two sources and test them against the Blum units before fixing the enclosure design.

**Evidence:** VERDICT NOTE: The core of the claim is backed by primary sources. The ESP32's radio cannot be used. The device needs either an nRF24L01+ that behaves like a genuine Nordic part, on SPI, or a Nordic nRF5x chip running ESB. Si24R1, XN297 and SE8R01 are unsuitable. Two details are wrong (the BK242x grouping and the 'legacy mode' wording), so I cannot mark it 'confirmed'. Use corrected_claim.

(1) ESP32 radio. Espressif's ESP32-C6 datasheet (local copy; a primary source not in the cited list) lists only '2.4 GHz Wi-Fi 6 (802.11ax), Bluetooth 5 (LE), Zigbee and Thread (802.15.4)'. For BLE it gives 'Speed: 125 Kbps, 500 Kbps, 1 Mbps, 2 Mbps'; for 802.15.4 it gives 'OQPSK PHY ... 250 Kbps'. It documents no proprietary GFSK or ESB mode. Arduino forum, Whandall, 2022-07-10: 'You can not emulate a NRF24L01+ with only on chip resources.' I found nothing that says otherwise.

(2) What the library needs. In the Port-Net source, begin() calls setDataRate(RF24_1MBPS), enableDynamicPayloads(), disableCRC(), enableAckPayload() and setChannel(4); pairing uses channel 10. ACK payloads are set with writeAckPayload() and read with getDynamicPayloadSize(). The nRF24L01+ datasheet says of EN_CRC: 'Forced high if one of the bits in the EN_AA is high'. RF24's own getCRCLength() checks 'if (config_reg & _BV(EN_CRC) || AA)'. So with auto-ACK on, disableCRC() has no effect, and the CRC stays 16-bit because RF24 sets CRCO at startup.

(3) nRF52840 option. Nordic's nRF Connect SDK (NCS) ESB docs say the module 'enables an nRF5 Series device to communicate with an nRF5 or nRF24L Series device'. They also list '1 to 32 bytes dynamic payload length in legacy mode' and 'Backward compatible with legacy nRF24Lxx Enhanced ShockBurst'. However, NCS esb.h defines ESB_LEGACY_CONFIG as '.protocol = ESB_PROTOCOL_ESB' (commented 'Fixed payload length'), with ESB_BITRATE_2MBPS and ESB_CRC_8BIT. That preset does not match these dynamic-length frames with ACK payloads. What is needed is ESB_PROTOCOL_ESB_DPL at 1 Mbps with a 16-bit CRC. NCS esb.c contains '/* nRF24L01 chip inverts ACK bit */ pdu->type.dpl_pdu.ack = !current_payload->noack;'. This is a primary source, independent of the cited list, confirming the bit-polarity quirk behind the clone problems.

(4) Si24R1.
- MySensors thread, Zeph, 2015-03-25: 'the Si24R1 got the ACK bit inverted (following an error in the datasheet), so it's incompatible with the real nRF24L01+ (and good clones) in ESB mode.'
- Kegetys (simagic README): the clones 'have the message NO_ACK bit inverted from the genuine ones and this means they are not directly capable of communicating the ACK packets with the genuine chips'. He also gives a workaround on the sending side that 'works at least with the Si24R1', so 'cannot' is slightly too strong.

(5) BK242x and SE8R01. The cited RFM7x-lib README contradicts the claim for BK242x. It says these chips need a 'special (undocumented of course) initialization sequence'. It also says: 'Inversion of NO_ACK bit in the air payload can be controlled by the undocumented compatible mode bit in one of the bank1 registers ... Otherwise dynamic compatible mode is recommended (genuine nrf?)'. So BK242x can be set to match genuine parts, but RF24 does not perform that startup sequence, so it is not a drop-in replacement. The same README calls SE8R01 'not compatible'. The MySensors thread does not mention SE8R01, XN297 or BK242x at all (0 hits).

(6) XN297. The Panchip XN297L datasheet (primary, not in the cited list; text extracted locally) gives the Enhanced BURST packet as 'Preamble (3 byte) Address (3~5 byte) Package control field (10bit): Payload length (7bit) PID (2bit) NO_ACK (1bit)'. It says the preamble 'can't be configured by the register', and scrambling can be switched on or off. The nRF24L01+ uses a 1-byte preamble and a 9-bit control field, so the two cannot exchange frames in ACK mode. goebish's XN297_emu.ino has to emulate the preamble, scrambling and CRC in software. A DeviationTx forum report on XN297L auto-ACK came up in search, but the site refused the connection, so I could not open it.

(7) Parts list.
- The arduino-esp32 pin file pins_XIAO_ESP32C6.h gives LED_BUILTIN=15, D0=0, D1=1, SCK=19, MISO=20 and MOSI=18. This matches the example: CSN=GPIO_NUM_0, CE=GPIO_NUM_1, LED on pin 15 switched on with LOW, and the default SPI pins.
- RF24 COMMON_ISSUES: 'Add capacitor(s) close to the VCC and GND pins of the radio. Typically, 10uF is enough ... low ESR', and 'please use Electrolytic or Tantalum capacitors. Ceramic capacitors may not be good enough'. The 100 nF capacitor is standard practice but does not come from the RF24 docs. COMMON_ISSUES says nothing about clones, so it does not support that part of the claim.

(8) Caution for the nRF52840 port. TMRh20's nrf_to_nrf v1.6.0 has an RF24-like API with ACK payloads, 1 Mbps Nordic mode and a 16-bit CRC. But when sending dynamic-length frames it writes the control bits as PID<<1, so the lowest bit is always 0. NCS puts its 'ack' bit in that position and sets it to 1 for frames that request an ACK. Either test this on the bench or use NCS ESB directly.

(9) Earlier research in this project did not identify the RF chip inside the Blum units. The 'genuine' requirement therefore assumes the Blum radios behave like genuine Nordic parts.

**Verifier sources:**
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/gh_portnet/src_blum_servo_drive.cpp
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/gh_portnet/src_blum_servo_drive.h
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/gh_portnet/examples_basic_basic_controller.cpp (local copy of https://github.com/Port-Net/BlumServoDrive/blob/main/examples/basic/basic_controller.cpp)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c07_verify/rf24_common_issues.md (https://raw.githubusercontent.com/nRF24/RF24/master/COMMON_ISSUES.md)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c07_verify/rfm7x_readme.md (https://raw.githubusercontent.com/jnk0le/RFM7x-lib/master/README.md)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c07_verify/simagic_readme.md (https://github.com/Kegetys/simagic-arduino-poc)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c07_verify/mysensors_1153_p1.txt (https://forum.mysensors.org/topic/1153/we-are-mostly-using-fake-nrf24l01-s-but-worse-fakes-are-emerging)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c07_verify/ncs_esb_index.rst (https://nrfconnectdocs.nordicsemi.com/ncs/latest/nrf/protocols/esb/index.html)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c07_check/ncs_esb.h (nRF Connect SDK include/esb.h)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c07_check/ncs_esb.c (nRF Connect SDK subsys/esb/esb.c)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c07_verify/nrf_to_nrf_readme.md (https://github.com/TMRh20/nrf_to_nrf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/rfimpl/nrf52/nrf_to_nrf.cpp (nrf_to_nrf v1.6.0)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c07_check/esp/esp32c6_ds.txt (Espressif ESP32-C6 datasheet)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/rfimpl/core/pins_XIAO_ESP32C6.h (arduino-esp32 XIAO_ESP32C6 variant)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/rfimpl/rf24/RF24.cpp
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nordic/ps.txt (Nordic nRF24L01+ Product Specification v1.0)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/rfimpl/clones/XN297_emu.ino (goebish/nrf24_multipro)
- https://forum.arduino.cc/t/can-i-use-an-esp32-as-an-nrfl01/1011253
- https://www.panchip.com/static/upload/file/20221014/1665726592628185.pdf (text: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c07_xn297/xn297l_ds.txt)

**Sources cited by research:**
- https://github.com/Port-Net/BlumServoDrive/blob/main/examples/basic/basic_controller.cpp
- https://nrfconnectdocs.nordicsemi.com/ncs/latest/nrf/protocols/esb/index.html
- https://github.com/TMRh20/nrf_to_nrf
- https://github.com/Kegetys/simagic-arduino-poc
- https://forum.mysensors.org/topic/1153/we-are-mostly-using-fake-nrf24l01-s-but-worse-fakes-are-emerging
- https://raw.githubusercontent.com/jnk0le/RFM7x-lib/master/README.md
- https://raw.githubusercontent.com/nRF24/RF24/master/COMMON_ISSUES.md
- https://wiki.seeedstudio.com/xiao_esp32c6_getting_started/
- https://wiki.seeedstudio.com/XIAO_BLE/

## C08 — NOT VERIFIED (AGENT FAILED)

**Claim:** On the generation(s) analysed by bluhm-de and Port-Net, devices are identified by fixed 3-byte IDs stored during pairing, the receiver does not check the per-frame sequence counter, and frames contain no authentication field or rolling code, so a peer paired through the drive's documented SYNC or SWITCH procedure is accepted. Blum's 2020 patent family describes optional encryption and time windows, which newer hardware may implement.

**Why it matters:** If this holds on the owner's drive, a firmware-only transmitter paired through SYNC is feasible and no counter needs to be stored. If the drive implements the encryption or time-window variants, the RF route is closed and only the genuine-switch actuator remains.

**Sources cited by research:**
- https://www.bluhm-de.com/content/os-tools/en/projects/home-information-system/blum-servodrive-wireless-protocol-deciphered.html
- https://github.com/Port-Net/BlumServoDrive/blob/main/src/blum_servo_drive.cpp
- https://patentimages.storage.googleapis.com/a3/d4/8e/5db82b4f715ae0/DE202009010203U1.pdf
- https://patentimages.storage.googleapis.com/99/2c/15/57a397ea95d027/US20230236580A1.pdf
- https://patentimages.storage.googleapis.com/f3/1e/63/fd2f1289140f01/US20230165384A1.pdf
- https://www.blum.com/su/en/company/press/press-releases-detail_94208.html

## C09 — CONFIRMED

**Claim:** All SERVO-DRIVE radios (switches 21P5020/23P5020, drives 21FA/23KA/23.A, the flex receiver) are specified for 2.404–2.452 GHz, at most +4 dBm (2.5 mW), about 10 m typical range and bidirectional coded telegrams. Blum declares them under EN 300 440 (short-range devices) rather than EN 300 328 or any sub-GHz standard, while the 2009 first-generation FCC filings list 2403–2477 MHz.

**Why it matters:** Confirms a narrowband 2.4 GHz link on a few fixed channels with no frequency hopping, so one retunable nRF24-class radio is enough. The 10 m typical range lets the device sit anywhere in or next to the cabinet. First-generation parts may use a different channel plan.

**Corrected claim:** Blum's current SERVO-DRIVE manual (BA 139 v7) gives every radio part it lists the same radio data: 2.404-2.452 GHz, at most +4 dBm (2.5 mW), typically 10 m range depending on material and contents, and "bidirektional, über codierte Telegramme" (bidirectional, via coded telegrams; the English edition mistranslates this as "via a coded wire"). The parts covered are drive units 21FA00x, 23.A00x and 23KA00x, the wireless receiver of the flex drive Z10C500x (radio type Z10C5007), and switch 23P5020. Switch 21P5020 is not in BA 139 v7, but a US-market Blum leaflet gives the same switch data. It lists 21P5020 as a radio equipment type and says it exists with the current PCB 23P502L00 and with an older PCB 21P502L00. The EU DoCs for 23P5020, 21FA000/001/005 (.21/.22), 23KA000/001 and 23.A000/001 cite EN 300 440 v2.1.1 (short-range devices, 1-40 GHz) as their only radio-spectrum standard, plus EN 301 489-1/-3 for EMC. None cites EN 300 328 or a sub-GHz standard such as EN 300 220, and older R&TTE-era manuals cite EN 300 440-2. No full DoC was checked for Z10C5007 or 21P5020. The first-generation FCC grants, W95-21P5020 (switch, PCB 21P502L00) and W95-21A00L33 (drive radio module), were granted 2009-12-23 and list 2403-2477 MHz; this comes from search snippets because the FCC pages blocked access. The 2.404-2.452 GHz figure therefore applies to current hardware, and first-generation parts may use a wider or different channel plan. EN 300 440 by itself does not rule out frequency hopping.

**Evidence:** (1) RADIO SPECS (Blum BA 139 v7, cited source). The German original gives the same four lines for "Antriebseinheit für AVENTOS 21FA00x", "AVENTOS top 23.A00x, 23KA00x", "flex Antriebseinheit Z10C500x – Funk (mit Funkempfänger)" and "SERVO-DRIVE Schalter 23P5020":
- "Frequenzband: 2,404 bis 2,452 GHz"
- "Maximale Sendeleistung: +4 dBm / 2,5 mW"
- "Reichweite: typisch 10 m (abhängig von Material und Füllgut)"
- "Übertragungsart: bidirektional, über codierte Telegramme"
The English v7 mistranslates the last line as "bidirectional, via a coded wire". The claim's "coded telegrams" matches the German. BA 139 v7 lists only switch 23P5020, not 21P5020. Its simplified DoC says: "radio equipment types 23P5020 and Z10C5007 conform to Directive 2014/53/EU".

(2) 21P5020 (US-market leaflet, local file, not in the cited list). It gives the same switch data: "SERVO-DRIVE switch ... Frequency band: 2.404 to 2.452 GHz ... Range: typically 10 m". It also says: "radio equipment types 23.A00L83, 23P5020, 21P5020 and Z10C5007 conform to the Directive 2014/53/EU". Its identifier table reads "23P5020, 21P5020 both with PCB 23P502L00 / 21P5020 with PCB 21P502L00". So the 21P5020 name covers both the current PCB and an older PCB.

(3) STANDARDS (EU DoCs, rendered and read):
- 23P5020 radio switch (cme213761, signed 2026-03-18): under 2014/53/EU the only listed standard is "EN 300 440 v2.1.1 Funkanlagen mit geringer Reichweite (SRD) — ... 1 GHz bis 40 GHz". Other standards listed: EN 301 489-1/-3, EN 62368-1 and EN 62479.
- 21FA000.21 to 21FA005.22 (cme185362, 2025-06-26): EN 300 440 v2.1.1.
- 23KA000/001 (me199043, 2023-09-06): EN 300 440 v2.1.1.
- 23.A000/001 (me8343120, cited; cme205108, not cited): "EN 300 440 v2.1.1 Short Range Devices (SRD); Radio equipment to be used in the 1 GHz to 40 GHz".
None of these lists EN 300 328 or EN 300 220. Older (R&TTE-era) manuals also cite EN 300 440: the Ballerina copy has "ETSI EN 300 440-2 V1.4.1 ... 2010-12-29" and the Eggersmann English manual has "EN 300440-2, EN 301489-1, EN 301489-3". Gap: I found no full DoC with a standards list for Z10C5007 or 21P5020. cme91302 turned out to be the Z10ZE000 vibration sensor, which has no radio (EMC only).

(4) FCC. Every FCC page and mirror refused automated access (403 or Access Denied); I did not work around this. An extended web search returned snippets of the fccid.io / fcc.report entries:
- W95-21P5020, "Remote Control to open and close cupboard doors": "Application Frequency Range 2403.0-2477.0", grant date 2009-12-23.
- W95-21A00L33 (first-gen drive radio module): "Frequency Range: 2403-2477 MHz".
Both were certified by TÜV Rheinland LGA. Earlier-agent notes, which I did not re-check, say the newer W95-23A00L83 grant lists 2.404-2.452 GHz at 0.44 mW (15C/DTS), and W95-23P502L00-01 lists test channels 2404/2410/2452 MHz at 0 dBm, GFSK.

No contradicting primary evidence was found. One caveat for "why it matters": declaring under EN 300 440 does not by itself rule out frequency hopping. The fixed-channel conclusion rests on the narrow band and the channel evidence, not on the standard.

**Verifier sources:**
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/ba139_en_v7.txt (from https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/ba139_de_clean.txt (from https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sde_$aof_$v7.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme213761_zf.pdf (https://www.blum.com/file/cme213761_zf_dok_bau?country=de&language=de, 23P5020 EU DoC)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme185362_zf.pdf (https://www.blum.com/file/cme185362_zf_dok_bau?country=de&language=de, 21FA EU DoC)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf_blumdocs/blum_me199043_zf_dok_bau.pdf (https://www.blum.com/file/me199043_zf_dok_bau?country=de&language=de, 23KA EU DoC)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/me8343120_zf.txt (https://www.blum.com/file/me8343120_zf_dok_bau?country=de&language=de, 23.A EU DoC)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme205108_zf.txt (Blum EU DoC cme205108 for 23.A000/23.A001, not in cited list)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme91302_zf.pdf (Z10ZE000 vibration sensor DoC, non-radio)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/fccid_leaflet_clean.txt (US-market SERVO-DRIVE leaflet listing 21P5020 and PCBs 21P502L00/23P502L00, not in cited list)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/eggersmann_servo_en.txt (older SERVO-DRIVE manual citing EN 300440-2, not in cited list)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/ballerina_blum38.txt (https://www.ballerina.de/media/config/media/content/downloads/montageanleitungen/montage_blum38.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c04_check/sync_findings.txt (earlier-agent notes on FCC snippets, not re-verified)
- WebSearch snippets: https://fccid.io/W95-21P5020/User-Manual/Users-Manual-English-1218574 | https://fccid.io/document/1223763 (W95-21A00L33) | https://fcc.report/FCC-ID/W95-21P5020
- Attempted, HTTP 403/Access Denied (not circumvented): https://fccid.io/W95 | https://fccid.io/W95-21P5020 | https://apps.fcc.gov/oetcf/eas/reports/GenericSearchResult.cfm (grantee W95) | https://electric.garden/julius-blum-gmbh-w95 | https://device.report/fccid/W9523P502L00 | https://device.report/fccid/W9521P5020; https://fcc.io/W95-21P5020 returned 404

**Sources cited by research:**
- https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sde_$aof_$v7.pdf
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf
- https://webshop.schachermayer.com/cdn/medias/docus/175/webd_Blum_Servodrive_Montageanleitung.pdf
- https://www.blum.com/file/cme213761_zf_dok_bau?country=de&language=de
- https://www.blum.com/file/me8343120_zf_dok_bau?country=de&language=de
- https://www.blum.com/file/me199043_zf_dok_bau?country=de&language=de
- https://www.blum.com/file/cme185362_zf_dok_bau?country=de&language=de
- https://www.ballerina.de/media/config/media/content/downloads/montageanleitungen/montage_blum38.pdf
- https://fcc.report/FCC-ID/W95-21P5020
- https://fccid.io/W95-23A00L83
- https://electric.garden/julius-blum-gmbh-w95/servo-drive-for-aventos-hf-hs-hl-top-23a001

## C10 — REFUTED

**Claim:** A drive accepts additional genuine 23P5020 switches without removing existing ones: up to six per classic-generation drive, each switch bound to one drive, with no documented limit for the top generation. Pairing is <SWITCH> then the switch (classic) or <Connect>, the switch, then <OK> (top). <Reset Wireless> or reset erases all paired switches plus the sync and collision settings.

**Why it matters:** A dedicated second switch becomes a fallback actuator that does not depend on the protocol and leaves the owner's switch untouched (about €30–55 / US$48–54). A Reset Wireless during experiments wipes every pairing and the sync/collision configuration.

**Corrected claim:** A SERVO-DRIVE for AVENTOS drive unit accepts more genuine 23P5020 switches without removing the existing ones, and each switch can be assigned to only one drive unit. Blum's US documents give the same limit of up to six switches per drive unit for both the classic HF/HS/HL drive (2014) and the AVENTOS HK top drive (2021: "Up to 6 switches can be programmed to a single drive unit"). EU manuals give no number; their "max. 6" means drive units linked for collision avoidance, and up to three drive units can be synchronised.

Pairing on the classic drive: press and hold <SWITCH> until the LED flashes, then press the switch until the LED stays lit, and repeat for each extra switch. Pairing on HK top: press the flashing <Connect> button (called "link" in US documents) until it stays lit, press each switch, press the flashing <OK>, then run the reference run.

On the classic drive, <Reset Wireless> (pen, at least 3 s) deactivates all functions and deletes all active switches, synchronisation and collision-avoidance settings for that drive; the reference run is reset separately with <Reset Motion>. On HK top, <Reset> returns the drive to factory settings, which by implication also clears the links, sync and collision settings, so switches must be re-linked and the reference run repeated.

In the US, 23P5020 is a set of two switches (2/box). In the EU it is a single switch, about €29–31 including VAT (lignoshop). So a US$48–54 price is for a pair.

**Evidence:** Most of C10 holds up. One part is wrong: Blum does document a limit for the top generation. Blum's own AVENTOS HK top document, which is in the cited list, gives the same six-switch limit.

1) Wrong part, the top-generation limit. The Blum, Inc. AVENTOS HK top technical document (2021) was checked as rendered PDF pages, not just as a text extract. The local copy is ops_elec/blum_doc4/hktop_tech_us.pdf, which matches the cited aventoshktoptech US document.
- Page 28, "A Linking the SERVO-DRIVE switch": "Each switch can only be assigned to one SERVO-DRIVE unit." and "NOTE: Up to 6 switches can be programmed to a single drive unit". The steps are: "1 Press flashing link button. 2 Link button lights continuously. 3 Press each switch to link. 4 Press flashing "ok" button to complete linking. 5 Proceed to start reference run."
- Page 30 repeats the note. Page 23 says: "SERVO-DRIVE switch – For adding additional activation switches to a SERVO-DRIVE for AVENTOS application. Up to six switches can be added per drive unit … NOTE: A switch can only be synchronized to one drive unit … SERVO-DRIVE switch (set of 2) Part no. 23P5020".
- So the limit is six for HK top as well. The US documents call the button "link"; the EU manual calls it <Connect>.

2) Classic generation, confirmed.
- Blum, Inc. 2014 document (opentip ServoDriveAventos_hl.pdf): "Set includes two switches / Up to six switches can be added per drive unit / NOTE: A switch can only be synchronized to one drive unit". The steps are: "1 Press and hold the <SWITCH> button until the LED flashes 2 Press and hold the SERVO-DRIVE switch until the LED lights up continuously … Repeat steps 1 and 2 for additional SERVO-DRIVE switches in the cabinet." The woodworkerexpress brochure has the same wording.
- The EU manual (KLA0001 MA 502 v3, LDL copy) says: "A switch can only be assigned to one drive unit … Repeat procedure A 1–2 for additional SERVO-DRIVE switches in the cabinet." It gives no number of switches. Its only "max. 6" is "drive units (max. 6) can be linked" for collision avoidance, and separately "Up to three drive units can be synchronised". These are easy to misread as switch limits.

3) Reset, confirmed.
- Opentip document: "F Reset wireless – Deactivates all functions. All active SERVO-DRIVE switches, synchronization and collision avoidance settings for the respective drive unit are deleted." You press it with a pen for at least 3 s.
- The EU manual v3 says only "Deactivates all functions: All active SERVO-DRIVE switches … are deleted". The reference run is reset separately with <Reset Motion>.
- HK top: "E Reset – Resets the drive unit to the factory setting" (LDL). The US document adds: "Press reset button … Link button will flash continuously when reset complete." It does not list what gets erased. That pairings, sync and collision settings are wiped is an inference from "factory setting".

4) Independent primary sources not in the cited list.
- Blum EN manual hosted by Eggersmann (local eggersmann_servo_en.txt): "F Reset Wireless Deactivates all functions. All active SERVO-DRIVE switches, synchronisations and collision avoidance settings for the respective drive unit are deleted."
- Blum DE Montageanleitung hosted by Ballerina (local ballerina_blum5.txt): "Ein SERVO-DRIVE-Schalter kann jeweils nur mit einer Antriebseinheit aktiviert werden!" and "Reset Wireless Deaktiviert alle Funktionen: alle aktivierten SERVO-DRIVE-Schalter der jeweiligen Antriebseinheit werden gelöscht." Neither copy gives a switch count.
- The cited patent DE202009010203U1 is consistent: it describes a learn mode started with a "connect" key and several release switches (S11, S12) assigned to one drive.
- The official Blum US programme page states no limit. It does show that "top" drive units now also exist for HF, HS and HL. The six-switch figure is primary-documented only for classic HF/HS/HL and for HK top. A retailer (E.B. Bradley) also lists it for HF/HS/HL top.

5) Part number and price.
- US: 23P5020 is a set of two switches (Blum page 23; E.B. Bradley: "(2) SERVO-DRIVE for AVENTOS Switches", "2/Box", "Up to (6) switches can be added per drive unit", manufacturer SKU "23P5020 SCHALT V2 HGR").
- EU: lignoshop sells it as "Lieferumfang: 1 Stück" for €29.38–30.56 including VAT.
- So the US$48–54 figure is for a pair. I did not verify US prices.

The Richelieu-hosted Blum PDF could not be read (connection reset). The c4forums and toolz4you pages were not opened.

**Verifier sources:**
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/ldl_utf8.txt (https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/opentip_utf8.txt (https://cdnimages.opentip.com/Docs/HDL/ServoDriveAventos_hl.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc3/wwe_brochure.txt (https://www.woodworkerexpress.com/skin/common_files/files/specs/blum/blum-servo-drive-brochure.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc4/hktop_tech_us.pdf pages 23 and 28 rendered, plus hktop.txt (https://d2.blum.com/services/BEC003/aventoshktoptech_ep_dok_bus_$sen-us_$aof_$v5.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme90761_ma.txt (https://www.blum.com/file/cme90761_ma_dok_bau?country=de&language=de; pictograms only, no relevant text)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c06_txt/DE202009010203U1.txt (https://patentimages.storage.googleapis.com/a3/d4/8e/5db82b4f715ae0/DE202009010203U1.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/eggersmann_servo_en.txt (https://eggersmannusa.com/wp-content/uploads/2017/12/Blum_ServoDrive_EN.pdf) [not in cited list]
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/ballerina_blum5.txt (https://www.ballerina.de/media/config/media/content/downloads/montageanleitungen/montage_blum5.pdf) [not in cited list]
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/ballerina_blum38.txt [not in cited list]
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc4/sch_clean.txt [Schachermayer-hosted Blum operating manual]
- https://www.blum.com/us/en/products/motion-technologies/servo-drive/servo-drive-aventos/programme/ [not in cited list]
- https://www.lignoshop.de/servo-drive-schalter-symmetrisch-blum23p5020.html
- https://www.ebbradley.com/b23p5020-lg.html [not in cited list; retailer]
- https://www.richelieu.com/documents/docsGr/125/013/6/1250136/1621595.pdf [attempted; inaccessible, ECONNRESET]

**Sources cited by research:**
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf
- https://cdnimages.opentip.com/Docs/HDL/ServoDriveAventos_hl.pdf
- https://www.woodworkerexpress.com/skin/common_files/files/specs/blum/blum-servo-drive-brochure.pdf
- https://d2.blum.com/services/BEC003/aventoshktoptech_ep_dok_bus_$sen-us_$aof_$v5.pdf
- https://www.blum.com/file/cme90761_ma_dok_bau?country=de&language=de
- https://www.toolz4you.com/blum-servo-drive-schalter-fur-aventos-symmetrisch-tiefgrau-23p5020-628101.html
- https://www.lignoshop.de/servo-drive-schalter-symmetrisch-blum23p5020.html
- https://www.lignoshop.de/23p5020-servo-drive-schalter-weiss.html
- https://www.c4forums.com/forums/topic/45942-blum-servo-drive/
- https://patentimages.storage.googleapis.com/a3/d4/8e/5db82b4f715ae0/DE202009010203U1.pdf

## C11 — UNCERTAIN

**Claim:** The 23P5020 runs on a 3.0 V CR2032 (rated 10 mA), and its PCB 23P502L00.01 has test pads labelled +3 V, GND, 'Button to MCU' and 'Button to Vcc (maybe 1.8 V)'. One documented build powered it at 3.34 V and triggered the drive by bridging the button pads with a potential-free contact for 0.8 s; Blum's lean protection ignores presses held too long, so the actuator must be an isolated momentary contact with an adjustable pulse length.

**Why it matters:** Defines the fallback actuator circuit: power the donor switch from 3.3 V (never together with the coin cell) and bridge the pads through a PhotoMOS such as the AQY212EH rather than a GPIO. Make the pulse length adjustable, starting at 0.8 s, and keep the output off while the ESP32 boots.

**Corrected claim:** Blum specifies the SERVO-DRIVE switch 23P5020 for a 3.0 V lithium button cell (CR2032) with a rated current of 10 mA. Blum's FCC SDoC lists the switch with PCB 23P502L00, and the component-side silkscreen reads 23P502L00.01.

The PCB has no pad labels. The names "+3 V", "GND", "Button to MCU" and "Button to Vcc (maybe 1.8 V)" are annotations the daumemo author drew on a photo after identifying the test pads with a multimeter. The 1.8 V figure is his unconfirmed guess.

One documented build (daumemo, Feb 2026) works like this:
- The coin cell is removed.
- The switch is powered from the Fibaro Smart Implant's 3.3 V sensor supply, measured at 3.34 V.
- The two button pads are wired to the Smart Implant's potential-free OUT1, with auto-off parameter 156 = 8 (0.8 s).
- This triggers the drive.

A Shelly forum thread proposes the same approach (about 3.3 V from a Shelly Uni, potential-free output, 1 s auto-off) but reports no result.

Blum's documented lean protection is not a switch feature. It applies to pressing or leaning on the front, where the drive's touch-to-open ignores anything but a brief touch or pull. Blum documents no rule for how the switch handles long presses; during pairing, the switch is pressed until the drive's LED lights steadily.

So bridge the pads through an isolated contact such as a PhotoMOS (the AQY212EH is rated 60 V / 550 mA). Use a short, adjustable pulse: 0.8 s is proven and about 1 s has been suggested. Keep the output off while the ESP32 boots. Power the switch from 3.3 V, never together with the coin cell. These choices copy a normal key press and are precautions; lean protection does not require them.

**Evidence:** The hardware facts hold up. Two parts of the claim are misread.

CONFIRMED:
(1) Blum BA-139 (kla0031) EN v7, technical data: "SERVO-DRIVE switch 23P5020 / Supply voltage: 3.0 V Lithium button cell CR2032 / Rated current: 10 mA". The DE copy says the same: "3,0 V Lithium Knopfzelle CR2032 / Bemessungsstrom: 10 mA".
(2) Independent primary source: Blum MA-502/3 10.20 (LDL copy). Its FCC SDoC (47 CFR 2.1077) says: "23P5020, 21P5020 both with PCB 23P502L00 / 21P5020 with PCB 21P502L00". It also says "Insert the new battery (type CR2032)". The ".01" suffix shows only in a component-side photo crop in the scratchpad (silkscreen "23P502L00.01"; where the photo came from is not recorded). daumemo's photos show only the battery side: "JX02 94V-0 1422" and two "+" marks.
(3) daumemo article (published 2026-02-11, updated 2026-02-24): "By measuring them with a multimeter, I have found which ones can be used ... Two of those pins (red and blue in the photo) are power and two other (green and orange) are used to short the trigger button". Further quotes: "the output of the brown wire is 3.34 V (measured with a multi-meter) ... successfully used connecting it to the BLUM wireless switch"; "no coin cell left inside"; "Parameter 156 – set to 8 (0.8 seconds) ... to imitate the push button press". Its one-PSU diagram labels the wiring "Button direct connection", with two wires running to the Smart Implant's output terminals.
(4) Independent primary source: Fibaro FGBS-222 manual (local copy; the online manual returned HTTP 403). Quotes: "2 potential-free outputs"; "Maximum current on outputs 150mA"; "30V DC / 20V AC"; "SP (brown) - power supply conductor for DS18B20 or DHT22 sensor (3.3V)"; parameter 156 "Output 1 - auto off ... 1-27000 (0.1s-45min, 0.1s step)". So 8 means 0.8 s, and the contact is potential-free.
(5) Farnell 1795425 is the Panasonic AQY21xEH PhotoMOS datasheet. The AQY212EH is rated 60 V / 550 mA with 5,000 V reinforced I/O isolation.

PROBLEMS:
(a) The PCB carries no pad labels. "+3 V", "GND", "Button to MCU" and "Button to Vcc (maybe 1.8 V)" are coloured arrows the blogger drew on photo DSC00074. The unannotated photo shows no such silkscreen, and "maybe 1.8 V" is his own guess.
(b) Blum's lean protection concerns the front, not the switch (a mix-up between the switch and the drive unit). BA-139 EN: "SERVO-DRIVE for AVENTOS is only triggered by a brief touch. Leaning on the lift system will not open it." and "The electromechanical opening function is only triggered by a brief touch or pull. In other cases (for example, if the front is leaned on), unintentional activation is deliberately ignored." DE: "Wenn Sie sich an der Klappe anlehnen, erfolgt kein Öffnen." No Blum document found says how the switch handles a long button press. In MA-502 pairing, the switch is held: "Press the <SWITCH> button until the LED flashes green ... Press the SERVO-DRIVE switch until the LED lights up green continuously."
(c) The Shelly forum thread (Jan 2025) is only a plan, with no result reported. It says the 23P5020 runs on "3V CR2032", plans to use the "potentialfreien Shelly Uni Kontakten" and about 3.3 V from the Uni's sensor VCC, and suggests "Auto off (1s)".
Minor: daumemo calls the cell "Li-Ion". Blum says it is a primary lithium CR2032.

**Verifier sources:**
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/daumemo/page.html (local copy of https://daumemo.com/how-do-you-connect-a-blum-servo-drive-to-z-wave-home-assistant-system/)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/daumemo/DSC00074_Blum_view1_schematic0_wm.jpg (https://daumemo.com/wp-content/uploads/2026/02/DSC00074_Blum_view1_schematic0_wm.jpg)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/daumemo/Blum-HK-Aventos-Top-servo-drive-wireless-trigger-PCB.jpg
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/daumemo/Blum-Aventos-Trigger-connection-to-Fibaro-Smart-Implant-using-one-PSU.png
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/ba139_en.txt (https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/fcc_dl/kla0031_ba139_u.txt (German BA-139, https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/ldl_utf8.txt (Blum MA-502/3 10.20 with FCC SDoC, https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf) [independent primary]
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/fibaro/fgbs222_pl.pdf (Fibaro FGBS-222 Smart Implant manual; text at C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c11_txt/fgbs222_pl.txt) [independent primary]
- https://manuals.fibaro.com/smart-implant/ (HTTP 403, not accessible)
- https://shelly-forum.com/thread/27978-blum-servodrive-mit-shelly-uni-steuern/
- https://www.farnell.com/datasheets/1795425.pdf (saved to C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c11_dl/farnell_1795425.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/crops/pcb_component_side.png
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/crops/housing_label.png

**Sources cited by research:**
- https://daumemo.com/how-do-you-connect-a-blum-servo-drive-to-z-wave-home-assistant-system/
- https://daumemo.com/wp-content/uploads/2026/02/DSC00074_Blum_view1_schematic0_wm.jpg
- https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf
- https://eggersmannusa.com/wp-content/uploads/2017/12/Blum_ServoDrive_EN.pdf
- https://shelly-forum.com/thread/27978-blum-servodrive-mit-shelly-uni-steuern/
- https://www.farnell.com/datasheets/1795425.pdf

## C12 — CONFIRMED

**Claim:** No SERVO-DRIVE for AVENTOS drive has a documented wired trigger or control input. Its only cable is the 2-core 24 V DC SELV distribution cable, which carries power only and is fed by a Blum supply (Z10NE040 72 W/3 A; Z10NA40x 24 W/1 A for one drive; Z10NE030 24 W/1 A; Z10NA300 12 W/0.5 A uno). Blum forbids connecting any other device to that supply, so the ESP32 needs its own USB supply, and a design that listens continuously (about 13 mA nRF24 RX) cannot run on a battery.

**Why it matters:** Rules out a wired trigger at the drive, so actuation must go through the radio. It also sets the supply: a separate 5 V USB adapter by default. Tapping the 24 V line would break Blum's rule, may void the warranty, and needs a buck converter rated for at least 36 V (not a Mini360/MP2307). Battery power is realistic only for a design that sleeps and wakes to act.

**Corrected claim:** Blum documents no wired trigger or control input on any SERVO-DRIVE for AVENTOS drive (21FA00x, 23.A00x/23KA00x, uno). The documented triggers are the battery-powered 2.4 GHz SERVO-DRIVE switch (CR2032) and pressing the front. The English BA-139 phrase "bidirectional, via a coded wire" is a mistranslation of "über codierte Telegramme", which describes the radio link, not a wire.

The drive is wired for power only. It runs on 24 V DC SELV through the 2-core cut-to-length distribution cable (rated 24 V max; on HF/HS/HL it reaches the drive via the bracket-profile cable). The supply must be a Blum unit:
- Z10NE040: 72 W, 24 V/3 A (US set Z10NE04UG22)
- Z10NA40x plug-in: 24 W, 24 V/1 A, total cable at most 2 m
- earlier Z10NE030: 24 W/1 A
- Z10NA300/Z10NA30x plug-in: 12 W, 24 V/0.5 A, used for SERVO-DRIVE uno with only one drive per plug-in supply
- first-generation Z10NE050/Z10NE010 and Z10NE020/Z10NA200
No document reviewed limits the Z10NA40x to one drive.

Wired control exists only outside this scope:
- the COMBOX (Z10ZC00A) uses a communication cable but works only with the drawer drive Z10A3000.02;
- the non-radio earthquake (vibration) sensor Z10ZE000 can only switch drives off, and the documents reviewed do not say how it is wired.

Blum says the SERVO-DRIVE supply may feed only the SERVO-DRIVE components intended for it ("keine anderen Geräte ... anschließen"), and it disclaims liability for any other use. So the ESP32 should have its own 5 V USB adapter. Tapping the 24 V rail goes against Blum's rule and may affect warranty or liability. It would also need a buck converter rated well above 24 V (for example at least 36 V); a Mini-360/MP2307 (4.75-23 V input) is unsuitable.

An nRF24L01+ that listens continuously draws 12.6-13.5 mA in RX. That alone would empty a 2500 mAh battery in about 8 days, before counting the ESP32. Battery power is therefore impractical rather than impossible, and realistic only for a design that sleeps and wakes to act.

**Evidence:** CORE CLAIM CONFIRMED BY BLUM PRIMARY DOCUMENTS. Three details need correcting.

(1) BA-139/7, the current Blum leaflet. I read the local EN copy (= cited d2.blum.com v7) and the DE copy.
- Trigger paths: "The SERVO-DRIVE switch triggers the automatic opening and closing action". Pressing the front opens the lift. Pressing the switch closes it. "The SERVO-DRIVE switch is battery-operated (CR2032)."
- Drive units 21FA00x, 23.A00x and 23KA00x are listed with "Radio equipment: 2.404 to 2.452 GHz". The English line "Transmission method: bidirectional, via a coded wire" is a MISTRANSLATION. The German original reads "bidirektional, über codierte Telegramme" (via coded messages). It does not describe a wired interface.
- Supply rule: "All SERVO-DRIVE drive units, the COMBOX and the vibration sensor are of protection class III and must only be supplied with safety extra-low voltage (SELV). They are only to be used with the power supply units provided by Blum."
- Z10NE040: 24.0 V DC, 3.0 A, 72.0 W. Its OEM CE DoC (cme108516) is from FSP Group: "Switching Power Adapter, Model Number: Z10NE040".
- Z10NA40x*: 24.0 V DC, 1.0 A, 24.0 W. "The overall length of the cable for SERVO-DRIVE plug-in power supply units Z10NA40x* must not exceed two metres." No document I read says the Z10NA40x is "for one drive".
- Connection rule, under "Structural changes ... are not allowed": EN "SERVO-DRIVE components are the only devices that should be connected to the SERVO-DRIVE power supply unit." DE "Schließen Sie am SERVO-DRIVE-Netzteil keine anderen Geräte als die dafür vorgesehenen SERVO-DRIVE-Komponenten an." The leaflet also says "Julius Blum GmbH is not liable for any other use."

(2) Cable.
- 2010 Blum brochure (furlat, cited): "6b Distribution cable - This cable is used to supply power to the drive unit. The maximum permitted operating power is 24V." It also says "The switch signals the drive unit via a wireless connection." The transformer at that time was Z10NE050.01.
- so-tech dealer page (fetched): "Leitungen: 2-adrig"; "Über dieses Kabel werden die ... Antriebseinheiten mit der benötigten Energie versorgt"; "max. zulässige Betriebsspannung beträgt 24V".

(3) Independent primary sources not in the cited list.
- Older Blum SERVO-DRIVE leaflet (local fccid_leaflet): "All SERVO-DRIVE appliances are of protection class III ... SELV ... only to be used with the transformers provided by Blum." "24-Watt Blum transformer Z10NE030 ... 24.0 W / 24.0 V d.c. / 1.0 A". "12-Watt Blum transformer Z10NA300 ... 12.0 W / 24.0 V d.c. / 0.5 A". "Blum components are the only devices that should be connected to the Blum transformer."
- Blum MA-707/1EN, SERVO-DRIVE uno (02.17): "Only one drive unit may be used per Blum plug-in transformer." "Blum components designed for this purpose are the only devices that should be connected to the Blum plug-in transformer." "The cable from the plug-in transformer to the drive unit may not be extended."
- SERVO-DRIVE uno for AVENTOS folder: "Z10NA30xyF ... Blum wall power supply (12 W) for single applications". So the one-drive rule is documented for the uno 12 W supply, not for the Z10NA40x.
- 2009 Blum AVENTOS manual with EC DoC (eggersmann copy): the family is Z10NE050, Z10NE010 (EN 60950 applied, so these are supplies), 21xA0x1, 21P5020, 21P0020 (unidentified), 21.A00L33 and Z10ZE000. The manual says "Blum SERVO-DRIVE components are the only devices that should be connected".
- Blum Inc. US Power Supply Set sheet (cme118480, 2024): "72 watt power supply", "Supplies power to the drive units", set Z10NE04UG22.

(4) Search for contradicting evidence.
- Blum DoC cme91302 (cited) covers Z10ZE000 "SERVO-DRIVE Erschütterungs-Sensor" (vibration sensor), described as "elektronische Steuerung zur Abschaltung von Blum SERVO-DRIVE Antriebseinheiten bei Erdbeben" (switches the drives off in an earthquake). It lists EMC and RoHS only, no radio directive. So it is a wired, non-radio device that can only shut drives off, not trigger them. How it is wired is not documented in anything I could open.
- COMBOX Z10ZC00A uses a "communication cable". The search results quote Blum's flyer as saying it "can only be used with the optimised SERVO-DRIVE drive unit: Z10A3000.02", a drawer drive, not AVENTOS. The flyer itself returned 404, so I could not read it directly.
- KNX forum (cited): there is only one post and no replies. It says "früher hatte man wohl einen Eingang am Servo-Drive ... Jetzt geht das wohl nur noch via Funk" (there apparently used to be an input; now it is apparently radio only). That is hearsay, and no Blum document supports it.
- industrialmonitordirect (cited) says "The SERVO-DRIVE expects a momentary dry contact closure". It has no author and no sources, gives no pinout, and its supply sizing is inconsistent. It is not credible and Blum's documents contradict it.

(5) Electrical figures.
- goughlui (cited): Mini-360 "4.75V-23V input"; MP2307 is a "3A, 23V" part. It is therefore unsuitable on a 24 V rail.
- nRF24L01+ datasheet: RX current is 13.5 mA at 2 Mbps, 13.1 mA at 1 Mbps and 12.6 mA at 250 kbps, so "about 13 mA" is right. At that draw a 2500 mAh battery lasts about 8 days for the radio alone. That makes always-listening battery operation impractical, not impossible.

Not resolved: the "Z10NA40x for one drive" wording; what part 21P0020 is; how Z10ZE000 is wired.

**Verifier sources:**
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/ba139_en.txt)
- https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de (local German copy: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/fcc_dl/kla0031_ba139_u.txt)
- https://www.furlat.lv/wp-content/uploads/2014/10/Aventos-Servo-Drive-range-technical.pdf (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc3/furlat_technical.txt)
- https://www.so-tech.de/Schubkasten----Zargensysteme/Schubladensysteme/Servo-Drive/Blum-Servo-Drive-Verteilerkabel-Ablaengbar.html
- https://www.blum.com/file/cme91302_zf_dok_bau?country=de&language=de (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme91302_zf.pdf)
- https://www.blum.com/file/cme108516_zf_dok_bau?country=de&language=de (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme108516_zf.pdf)
- https://knx-user-forum.de/forum/%C3%B6ffentlicher-bereich/geb%C3%A4udetechnik-ohne-knx-eib/2053886-blum-servo-drive-servo-drive-schalter-23p5020-an-knx-anbinden
- https://industrialmonitordirect.com/blogs/knowledgebase/blum-servo-drive-knock-to-open-integration-solenoid-trigger
- https://goughlui.com/2018/07/04/tested-mini-360-mp2307-based-3a-buck-converter-module/
- nRF24L01+ Product Specification v1.0 (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nordic/ps.txt)
- NEW: https://d2.blum.com/services/BEC003/cme118480_bz_dok_bus_$sen-us_$aof_$v1.pdf (downloaded: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c12_dl/cme118480.pdf)
- NEW: older Blum SERVO-DRIVE instruction leaflet with Z10NE030/Z10NA300 data (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/fccid_leaflet.txt)
- NEW: Blum MA-707/1EN SERVO-DRIVE uno (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc2/uno_aventos_ma.pdf)
- NEW: Blum SERVO-DRIVE uno for AVENTOS folder (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc2/uno_aventos_folder.pdf)
- NEW: https://eggersmannusa.com/wp-content/uploads/2017/12/Blum_ServoDrive_EN.pdf (2009 Blum manual + EC DoC; local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/eggersmann_servo_en.txt)
- NEW: https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf (local: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/ldl_utf8.txt)
- https://www.blum.com/ch/de/produkte/bewegungstechnologien/servo-drive/servo-drive-uno-aventos/programm/ (fetched; lists no part numbers)
- https://d2.blum.com/services/BEC003/box0046-fl-166_fl_dok_bau_$sen-in_$aof_$v1.pdf (COMBOX flyer; HTTP 404, content known only from search snippet)
- Earlier-agent notes: C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c04_check/sync_findings.txt

**Sources cited by research:**
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf
- https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de
- https://www.furlat.lv/wp-content/uploads/2014/10/Aventos-Servo-Drive-range-technical.pdf
- https://www.so-tech.de/Schubkasten----Zargensysteme/Schubladensysteme/Servo-Drive/Blum-Servo-Drive-Verteilerkabel-Ablaengbar.html
- https://www.blum.com/file/cme91302_zf_dok_bau?country=de&language=de
- https://www.lignoshop.de/out/media/aventos-top-info.pdf
- https://www.blum.com/file/cme108516_zf_dok_bau?country=de&language=de
- https://www.manualslib.com/manual/3612384/Blum-Servo-Drive.html
- https://knx-user-forum.de/forum/%C3%B6ffentlicher-bereich/geb%C3%A4udetechnik-ohne-knx-eib/2053886-blum-servo-drive-servo-drive-schalter-23p5020-an-knx-anbinden
- https://industrialmonitordirect.com/blogs/knowledgebase/blum-servo-drive-knock-to-open-integration-solenoid-trigger
- https://goughlui.com/2018/07/04/tested-mini-360-mp2307-based-3a-buck-converter-module/
- https://www.sparkfun.com/datasheets/Components/SMD/nRF24L01Pluss_Preliminary_Product_Specification_v1_0.pdf

## C13 — CONFIRMED

**Claim:** The drive's built-in collision detection stops opening and closing when the front meets a body part or object, including just after a close command. The flap may be moved by hand at any time. A mains interruption during motion leaves the flap where it stopped, and the next switch press re-activates the drive. The duty cycle is S3 5 % (max. 10 movements per 10 min), and the drive's EU DoC cites EN ISO 14118 (prevention of unexpected start-up). What a switch press does during motion is not documented.

**Why it matters:** Blum's stop-on-contact is the last line of defence, and an unattended close counts as an unexpected start-up. The firmware therefore needs: a warning before closing, an optional presence veto, a cancel control, at most one retry, a rate limit below 10 movements per 10 min, a fresh countdown after power-up, and no commands during reference runs. Test what a press during motion does before ever allowing a second command.

**Corrected claim:** Blum's current instruction leaflet BA-139/7 (EN and DE, with the same text in the FCC-filed leaflet) says the drive's integrated collision detection automatically interrupts opening and closing when the front hits a body part or other object. Older Blum brochures (Blum Inc. 2014 and Blum AU) add that this applies even just after the switch has been pressed for closing. Blum still warns that reaching into the lever area during motion can cause injury, and that people sitting or standing on work surfaces can be pushed or hurt. Collision detection is therefore a last-resort safeguard, not a guarantee.

The lift can be opened or closed by hand at any time without damaging the drive. However, Blum says not to press or pull with force against the direction of a powered movement, because this causes premature wear.

If power is interrupted during motion, the flap stays where it is and can be closed by hand. When power returns, pressing the SERVO-DRIVE switch puts the drive back into operation, with no reset needed. Blum does not say whether that press also moves the flap.

The drive units 21FA00x, 23.A00x and 23KA00x are rated S3 5 % (max. 10 motions per 10 min). The EU declarations of conformity for both the 21FA000/001/005 drive (signed 2025-06-26) and the 23.A000/23.A001 HF/HL/HS top drive list EN ISO 14118:2018 (prevention of unexpected start-up) under the Machinery Directive 2006/42/EC. They list it alongside EN ISO 12100, EN ISO 13849-1/-2 and EN 60335-1. The HK top (23KA) declaration was not checked.

No Blum document says what a switch press does during motion. Blum brochures say only that the motion "can be interrupted at any time", without saying how. The assembly instructions say the automatic reference run (the flap opens and closes twice) must not be interrupted by reaching in. If it is interrupted, it must be reset (Reset Motion E1) and started again.

**Evidence:** Skeptic check (cited sources, local copies):
(1) BA-139/7 EN p.11: "The lift system can be opened or closed manually at any time without damaging SERVO-DRIVE for AVENTOS." Collision detection / lean protection: "The opening and closing action is interrupted automatically via the integrated collision detection feature when the front hits a body part or other object." DE: "Durch die integrierte Kollisionserkennung wird der Öffnungs- bzw. Schließvorgang automatisch unterbrochen, wenn die Front auf einen Körperteil oder Gegenstand trifft."
(2) Power failure, EN p.13: "If the power supply is interrupted during the opening or closing process, the SERVO-DRIVE for AVENTOS lift system will remain in that position. The front can be closed manually, or when power is restored; simply touch the switch to re-activate SERVO-DRIVE function." DE: "...bleibt die Klappe stehen. Um SERVO-DRIVE für AVENTOS wieder in Betrieb zu nehmen, betätigen Sie den SERVO-DRIVE-Schalter."
(3) Technical data for drive units 21FA00x and 23.A00x/23KA00x: "Operating mode: S3 5 % (max. 10 motions/10 min.)".
(4) "Just after a close command" comes only from older Blum brochures, not from BA-139/7. Opentip (Blum Inc. 2014) and Furlat (AU): "Even when the switch has just been pressed for closing - the closing procedure is halted immediately if the user again reaches into the cabinet and/or an object is placed between the cabinet and the door."
(5) EU DoC me8343120, covering 23.A000/23.A001 (HF/HL/HS top): under 2006/42/EC it lists "EN ISO 14118: 2018 Safety of machinery - Prevention of unexpected start-up". No contradictions found.
Caveats found in the same manual: the NOTE says "Do not press or pull with force against the movement direction when opening or closing the lift system" (premature wear). The safety section says "Reaching into the area around the lift system lever during the opening and closing motion can cause injury" and "Persons, especially children, who are sitting or standing on work surfaces may be pushed from work surfaces or injured when opening and closing a lift system." So collision detection does not remove every hazard.

Independent primary sources not in the cited list:
(a) EU DoC cme185362 (scanned PDF read as images) for 21FA000/001/005 .21/.22, "SERVO-DRIVE Antriebseinheit für AVENTOS", signed Höchst 2025-06-26. Under 2006/42/EG it lists "EN ISO 14118: 2018 Sicherheit von Maschinen - Vermeidung von unerwartetem Anlauf", alongside EN ISO 12100:2010, EN ISO 13849-1:2023, EN ISO 13849-2:2012 and EN 60335-1. So both the 21FA and the 23.A top drives cite EN ISO 14118. The 23KA (HK top) DoC was not checked.
(b) EU DoC cme205108 (Doc. Id BAU0207504961 Idx 01, 23.A000/23.A001) also lists EN ISO 14118:2018.
(c) The SERVO-DRIVE instruction leaflet filed with the FCC (an older revision) repeats the collision, manual-operation and power-failure text word for word, and gives "Operating mode: S3 5% max. 10 motions/10 min."
(d) Blum assembly instructions re-hosted by Ballerina (montage_blum5), on the reference run: "Front öffnet und schließt 2 Mal automatisch: Den Vorgang keinesfalls durch Eingreifen unterbrechen bzw. stoppen. HINWEIS: Im Falle einer Unterbrechung die Referenzfahrt zurücksetzen - siehe Reset Motion E 1. Die Referenzfahrt erneut starten." In English: the front opens and closes twice automatically and must not be interrupted by reaching in; if it is interrupted, reset with Reset Motion E1 and restart.

Switch press during motion: none of BA-139/7 (EN/DE), the FCC leaflet or the assembly instructions documents it. Blum brochures say only "the motion can be interrupted at any time" without naming how. So "not documented" holds in the narrow sense.

**Verifier sources:**
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/ba139_en_v7.txt (https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/ba139_de_clean.txt (https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/me8343120_zf.txt (https://www.blum.com/file/me8343120_zf_dok_bau?country=de&language=de)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme205108_zf.txt (Blum EU DoC cme205108, 23.A000/23.A001, Doc. Id BAU0207504961)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme185362_zf.pdf pages 1-3 (https://www.blum.com/file/cme185362_zf_dok_bau?country=de&language=de, 21FA drive EU DoC)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/fccid_leaflet_clean.txt (FCC-filed SERVO-DRIVE leaflet; the research index maps it to https://fccid.io/m/d1b5fcb86d3083a1873b97676a0f2a009907166fb423cf3f44e77145eb2c5ac7.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc3/opentip_servodrive_aventos_hl.txt (https://cdnimages.opentip.com/Docs/HDL/ServoDriveAventos_hl.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc3/furlat_technical.txt (https://www.furlat.lv/wp-content/uploads/2014/10/Aventos-Servo-Drive-range-technical.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/ballerina_blum5.txt (https://www.ballerina.de/media/config/media/content/downloads/montageanleitungen/montage_blum5.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/ldl_ma502.txt (https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf)
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/wwx_brochure.txt (re-hosted Blum brochure: 'the motion can be interrupted at any time')
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/research/all_research.json (index used only to map local files to URLs)

**Sources cited by research:**
- https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de
- https://d2.blum.com/services/BEC003/kla0031-ba-139_ba_dok_bau_$sen_$aof_$v7.pdf
- https://cdnimages.opentip.com/Docs/HDL/ServoDriveAventos_hl.pdf
- https://www.furlat.lv/wp-content/uploads/2014/10/Aventos-Servo-Drive-range-technical.pdf
- https://www.blum.com/ch/de/produkte/bewegungstechnologien/servo-drive/servo-drive-uno-aventos/programm/
- https://www.blum.com/file/kla0015-fo-030_fo_dok_bau?country=eu&language=en
- https://3409204.app.netsuite.com/core/media/media.nl?id=39128&c=3409204&h=I7hFZKVLl7M8rCEdbdyTx-rTtlr8LS-2ySoJUI6AM-XP9YhH&_xt=.pdf
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf
- https://www.blum.com/file/me8343120_zf_dok_bau?country=de&language=de
- https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/8692497

## C14 — CONFIRMED

**Claim:** No Blum product, setting, app or API provides automatic or timed closing for SERVO-DRIVE for AVENTOS. The only timed behaviour in the range is the 2.5/4/8 s hold-open time of SERVO-DRIVE flex, and 'SERVO-DRIVE smart' is an unreleased concept, so the 5-minute close must run entirely on the ESP32.

**Why it matters:** The whole timing function is new and runs on the ESP32; no supported integration can replace the device.

**Corrected claim:** No Blum product, setting, app or API offers a time-delayed or unattended close for SERVO-DRIVE for AVENTOS. A touch on the front opens the flap and a press on the SERVO-DRIVE radio switch closes it; "closes automatically" in Blum's manuals means motor-driven on that switch press. The flap can also be moved by hand at any time. The only other automatic movement is the reference run at installation, when the front opens and closes twice.

The only timed automatic close in the SERVO-DRIVE range belongs to SERVO-DRIVE flex for appliance doors, which pulls the door shut again if it is not gripped. On the version announced in May 2025 and sold from September 2025, the hold-open time is 2.5 s (factory setting), 4 s or 8 s; earlier flex units used a fixed time of about 3 s.

Blum's apps (the EASY ASSEMBLY assembly app) and AMPEROS AC (a furniture power outlet) do not control SERVO-DRIVE. "SERVO-DRIVE smart", a gateway that links SERVO-DRIVE to smart-home platforms and voice assistants, was described as a concept study with no market-launch date (Chancenland, undated, about 2021), and no evidence of a launch was found up to November 2026.

The 5-minute close therefore has to be built outside Blum's system; in this project that is the ESP32. Third-party DIY wiring exists, such as daumemo's Z-Wave/Fibaro relay on the switch, but it is not a Blum product.

**Evidence:** I found no contradicting evidence. The claim holds, but three wordings need tightening.

SKEPTIC PASS (cited sources, local copies first):
(1) BA-139 instruction leaflet, EN v7 (local ba139_en_v7.txt, the same document as kla0031-ba-139). "The SERVO-DRIVE switch triggers the automatic opening and closing action of SERVO-DRIVE for AVENTOS. Press on the front - The lift system opens automatically. Press on the SERVO-DRIVE switch - The lift system closes automatically." It adds: "The lift system can be opened or closed manually at any time." On power failure the lift "will remain in that position ... simply touch the switch to re-activate". The AVENTOS section has no timer or hold-open setting. In the SERVO-DRIVE flex section: "2b) User does not touch door - Door closes again ... You can choose between three different hold-open times. 2.5 s (factory setting) / 4 s / 8 s". So "closes automatically" for AVENTOS means the motor closes the flap when the switch is pressed. It is not an unattended close.
(2) Schachermayer DE Bedienungsanleitung (an older edition). It has the same AVENTOS wording: "Druck auf den SERVO-DRIVE-Schalter - die Klappe schliesst automatisch". Its flex section shows a fixed "> 3 s ... Benutzer greift nicht ein - Tuer wird wieder geschlossen". So the earlier flex had a fixed hold-open of about 3 s, not 2.5/4/8 s.
(3) LDL MA-502 installation instructions, EN. There is no timer. The only automatic movement without a switch press is the commissioning reference run: "Press on front: The reference run starts automatically / Front opens and closes 2x automatically". The other timings are button holds (Reset Motion and Reset Wireless at least 3 s, and > 1.5 s).
(4) cme90761 US installation instructions. The only timings are "3s" button holds.
(5) Blum US SERVO-DRIVE overview page: "Lift systems in wall cabinets can be easily opened with a light touch and closed again with the press of a button." It does not mention an app, smart home, a timer or an API.
(6) Chancenland article: "a concept study was created that links the SERVO-DRIVE opening support system with common smart home platforms and voice assistants ... a specially developed gateway ... Blum's development team has been working since 2019 on the production-ready implementation ... A date for the market launch has not yet been set." The page shows no date; its image was uploaded in 2021/06. It mentions voice-commanded open and close, but no timed close.

INDEPENDENT PRIMARY SOURCES (not in the cited list):
(a) Blum press release "SERVO-DRIVE", Hoechst, May 2025 (press-releases-detail_94208). It describes upgraded electronics, adjustable trigger sensitivity and an installation-depth LED. For flex it says "three different hold-open times ... 2.5, 4 and 8 seconds", available "from September" 2025. It does not mention an app, smart home, gateway, voice control or timed closing.
(b) Blum CH programme page for SERVO-DRIVE fuer AVENTOS: "Leichtes Antippen der Front und schon oeffnen sich Klappen"; "Komfortables und einfaches Schliessen: Dafuer sorgt der gut erreichbare Schalter an der Korpusseite"; "Der SERVO-DRIVE-Schalter kommuniziert mittels Funk mit der Antriebseinheit". It mentions no timer, smart home or voice control. The only app on the page is the EASY ASSEMBLY assembly app.
(c) Blum AMPEROS AC overview page. AMPEROS AC is a furniture power outlet that is "the perfect match for BLUMOTION, TIP-ON BLUMOTION and SERVO-DRIVE". The page shows no automation, timer or API. A search snippet said an app controls USB ports and lights, but the page did not show this. In either case nothing controls SERVO-DRIVE.
(d) The FCC-filed SERVO-DRIVE leaflet and the Eggersmann-hosted Blum installation instructions (local copies) repeat the BA-139 wording.

Two extended web searches (EN, and DE "Smart Home App Sprachsteuerung automatisch schliessen 2026") found no Blum launch of SERVO-DRIVE smart and no timed close for AVENTOS. The only smart-home result was daumemo's third-party Z-Wave/Fibaro wiring.

I did not re-open the cited EU downloads page.

CAVEATS:
- "Unreleased" rests on finding no launch evidence. No Blum statement says so.
- "The only timed behaviour" needs narrowing to "the only timed automatic close". Configuration button holds and the reference run also exist, and the earlier flex used a fixed ~3 s.
- "Entirely on the ESP32" is a project design choice. The timer could also run on another non-Blum controller.

**Verifier sources:**
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/ba139_en_v7.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/schachermayer_servodrive.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf/blumdocs/ldl_utf8.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/prodid/dl4/cme90761_ma_dok_bau_us_en.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/prodid/dl7/info-servo-drive-flex.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/fccid_leaflet_clean.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/eggersmann_servo_en.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc4/hktop.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/prodid/dl4/sdrunoavt_fl_dok_bus_us_en.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/rfimpl/txt/market_approvals_2026.txt
- C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c14_verify/chancenland.html
- https://www.chancenland.at/en/news/servo-drive-smart-by-julius-blum/
- https://www.blum.com/eu/en/company/press/press-releases-detail_94208.html
- https://www.blum.com/ch/de/produkte/bewegungstechnologien/servo-drive/servo-drive-aventos/programm/
- https://www.blum.com/eu/en/products/electronic-systems/amperos-ac/overview/
- https://www.blum.com/us/en/products/motion-technologies/servo-drive/overview/

**Sources cited by research:**
- https://www.blum.com/file/kla0031-ba-139_ba_dok_bau?country=de&language=de
- https://webshop.schachermayer.com/cdn/medias/docus/175/webd_Blum_Servodrive_Montageanleitung.pdf
- https://www.ldlonline.co.uk/media/wysiwyg/others/media/kla0001-ma-502_ma_dok_bau__sen__aof__v3.pdf
- https://www.blum.com/file/cme90761_ma_dok_bau?country=us&language=en
- https://www.blum.com/eu/en/products/motion-technologies/servo-drive/servo-drive-aventos/downloads-videos/
- https://www.blum.com/us/en/products/motion-technologies/servo-drive/overview/
- https://www.chancenland.at/en/news/servo-drive-smart-by-julius-blum/

## Completeness critic

Overall the research phase is broad and mostly primary-sourced. Agents actually opened BA-139/7, MA-502/3, MA-400/2EN, the HK top v5 document, the EU DoCs, five Blum patent families, the full Port-Net and RF24 source, and the free bluhm-de text. That already supports the core design facts:
- A flap-state sensor is mandatory, because openings by front press or by hand send no switch frame.
- The switch is a toggle.
- There is no wired input and no Blum timer.
- The ESP32 needs its own USB supply and an external ESB radio.
- An extra 23P5020 switch can be paired.

Most of the remaining unknowns can only be settled with the owner's label and button-panel photos plus a bench test:
- which events produce 0x26 frames (C04);
- what the 0x01/0x10 values in byte 12 mean;
- whether CLOSE is idempotent;
- how the toggle behaves after a hand move;
- what a switch press does during motion, and what press length is accepted;
- the 23P5020 pad voltages and whether the Connect LED shows feedback;
- Wi-Fi coexistence;
- what happens when the S3 duty cycle is exceeded.
More desk research will not close these. The design phase should plan a bench-test matrix and branch on the results.

LOCAL EVIDENCE NOT YET CARRIED INTO THE CLAIMS
(1) Classic and HK top drives can be collision-linked. The HK top US technical document v5, p.32 'Collision Avoidance with Other Drive Units' (C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_doc4/hktop.txt, around lines 1011-1022), shows a classic COLL drive (held >1.5 s) linking with an HK top drive. An earlier agent read the MA-502/3 p.35 pictograms the same way. So the two generations share at least the drive-to-drive collision protocol, which weakens C05's 'unconfirmed on top generation'.
(2) HK top sync is two-sided, like classic SYNC. The same file (around lines 976-991) gives the sequence: link on drive 1, link on drive 2, then OK, then Play. Play starts a reference run that completes the sync. This matters for Port-Net's startSync().
(3) Option A may depend on the ESP32 being reachable:
- DE202009010203U1 [0044]/[0049] (C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/c02check/DE202009010203U1.utf8.txt) describes collision avoidance as a query to the partner before triggering, and synced drives as 'nur gemeinsam ausloesbar' (only triggered together).
- Port-Net sends an application-level BLUM_C4_ACK for every frame a drive sends (C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/gh_portnet/src_blum_servo_drive.cpp, BLUM_ACTIVE branch).
- No troubleshooting table opened so far covers a missing sync partner (C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/ops_elec/blum_ma/eggersmann_servo_en.txt, rows 1-12).
(4) The newer DoCs look like regulatory reissues, not new hardware. The 23P5020 DoC of 2026-03-18 only adds Battery Regulation (EU) 2023/1542 (C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/blum_docs/cme213761_zf.pdf, p.2). The 21FA DoC of 2025-06-26 adds Regulation (EU) 2023/826. The grant date and radio data for W95-21FA00Y were never found.
(5) C08 (no authentication) still has no verdict.

FOUR MATERIAL GAPS DESK RESEARCH CAN CLOSE
(a) How the drive behaves when its sync partner is offline. If it refuses to move or shows an error, option A disables normal touch-to-open whenever the ESP32 is down, and option B wins.
(b) Protocol continuity across 21FA.22, 23KA and 23.A, cross-generation sync, and the exact scope of the 2025 update.
(c) A source of genuine nRF24L01+ modules and a test on arrival. Every EU listing found is unverified. An Si24R1 clone would give a false negative on the bench. The nRF52840 fallback changes the BOM and needs a port.
(d) The normative baseline for unattended closing. The only part of IEC 60335-2-116:2025 that was read is the preview (C:/Users/glagl/AppData/Local/Temp/claude/245be834f3f85c48/d2b78163-db28-4d75-a608-30bae3174e3b/scratchpad/nonrf_iec/prev.txt). Its time-force entrapment limits in 20.2.105, the IEC 60335-1 remote-operation clauses and the auto-close rules for door and window drives are still unread. They decide whether a warning, a presence sensor and a local enable switch go on the BOM.

DELIBERATELY NOT PROMPTED
- FCC internal photos and the chip identity: the sites are blocked, and any genuine Nordic ESB radio works, so it is not BOM-critical.
- The GPL-2.0 vs LGPL-3.0 licence mismatch: settle it by reading the LICENSE file and headers. It affects publishing, not the design.
- Part 21P0020 and the Z10ZE000 wiring: low impact.

- **Gap:** Option A dependency: drive behaviour when its synchronised partner is off or unreachable — Option A makes the ESP32 a permanent sync partner of the real drive. The Port-Net code answers every drive frame with an application-level ACK, and Blum utility model DE202009010203U1 describes synced drives as 'nur gemeinsam ausloesbar' and collision avoidance as a query to the partner before triggering. If the drive refuses to move, delays or shows an error when its partner is silent, every ESP32 outage or reboot would break normal touch-to-open and switch operation of the owner's flap, which would decide the choice in favour of option B (second genuine switch plus reed). It is listed among the open questions, but nobody researched it. No Blum troubleshooting table opened so far covers it: MA-400/2EN rows 1-12, the 2013 Blum Inc. FAQ and HK top v5. The same sources may also settle C04, whether a front press is passed to partners by radio.
- **Gap:** Protocol continuity across 21FA.22, HK top and HF/HS/HL top, and the real scope of the 2025 changes — All protocol evidence (Port-Net 2024/25, bluhm-de 2023) comes from hardware of unknown generation. The revised classic drive 21FA00x.22 has its own whole-unit FCC grant, W95-21FA00Y, whose date and radio data were never found. HF/HS/HL top (W95-23A001) is a separate 2022 Part 15.249 design. Local Blum documents show collision linking between classic and HK top drives, but nothing covers cross-generation sync or the 23.A drives. The 2025/2026 DoC reissues look regulatory, but that is not confirmed. This decides how likely option A is to work on the owner's unit before any radio hardware is bought. It also completes the September 2025 and generation checks (C05, C08).
- **Gap:** BOM risk for option A: obtaining and verifying a genuine Nordic nRF24L01+ (or switching to an nRF52840) — The Blum link depends on dynamic payloads with auto-ACK and ACK payloads. Si24R1 clones break that exchange, BK242x parts need a special start-up sequence, and SE8R01 and XN297 are incompatible. The platform research found that none of the EU listings it saw (AZ-Delivery 3-pack, OpenELAB, hobbyelectronica) guarantees a genuine chip. A clone module would give a false negative on the bench and could wrongly rule out option A. If genuine parts cannot be had, the BOM moves to an nRF52840 (genuine Nordic radio, but no Wi-Fi) and the ESP32-only library must be ported. Nobody researched Nordic's lifecycle status, a known-genuine EU module, or a test to run when modules arrive.
- **Gap:** Safety baseline for unattended, timer-initiated closing (warning, presence check, force limits) — The device starts a powered closing of an overhead flap with nobody necessarily present. The research covered Blum's collision detection, Blum's warnings and EN ISO 14118, but never read the rules for movements not started by a hold-to-run control (remote or timer). IEC 60335-2-116:2025 explicitly covers kitchen storage units and powered doors and drawers. Its revised clause 20.2.105 adds a time-force entrapment test; only the preview was read. IEC 60335-1 has clauses on remote operation, and the standards for door and window drives have rules for automatic closing. The results decide whether the BOM and firmware need a pre-close buzzer/LED, a presence or obstruction sensor, a local enable switch with an indicator, and retry limits.

## Merged open questions

- Does a drive send move notifications to its sync partners for openings by front press, by switch and by hand, and at power-up? Are the notifications repeated or batched?
- What do the open/close parameter values in the library's move frames (0x01 vs 0x10) really mean? Its send and receive paths disagree; calibrate on the owner's unit.
- Do top-generation drives (23.A00x, 23KA00x with Connect/OK/Play) use the same sync pairing and frame layout as the classic 21FA drives?
- Did the September 2025 electronics update add encryption, counter checks or time windows, as described in the EP4162xxx/US2023 patent family?
- Which RF IC is in the 23P5020 and the drive modules (nRF24L01+ with an MCU, nRF24LE1, or an nRF5x running ESB)? Do first-generation parts (PCB 21P502L00, module 21.A00L33, FCC range 2403–2477 MHz) use the same channels and frames?
- Which channel(s) does the original switch use for button frames and for pairing? Is 2452 MHz (nRF channel 52) the third channel?
- What does a switch press or sync command do while the flap is moving (stop, reverse, ignore)? Does the first press after a power cut move the flap, or only re-activate the drive?
- What press duration does the drive accept (lean protection): the minimum to register and the maximum before it is ignored?
- How many switches can a top-generation drive hold? Does an old 21P5020 pair with top drives?
- How does the drive behave when its virtual sync partner is unpowered, out of range or slow to acknowledge (delay, error LED, refusal to move)?
- Is a close command sent to an already closed drive through the sync path harmless? Does the drive check the partner's device-type byte or ID prefix?
- What voltage and topology do the 23P5020 button pads have (high side to a ~1.8 V rail or to 3 V)? Does the Connect LED indicate a drive acknowledgement?
- What does the drive do when the S3 duty cycle (10 movements per 10 min) is exceeded?
- Does the drive's standby current differ between open and closed, which would allow state sensing on the 24 V line?
- How much does the ESP32's own 2.4 GHz Wi-Fi/BLE transmission degrade nRF24 reception on 2404/2410 MHz inside a small enclosure?
- Can an nRF52840 (NCS ESB or nrf_to_nrf with its single receive buffer) reproduce the library's ACK-payload and multi-pipe behaviour?
- Is the Port-Net code GPL-2.0-or-later, GPL-2.0-only or LGPL-3.0? This decides the licence of published firmware.
- Does the earthquake sensor Z10ZE000 cut drive power inline, or use a control line, which would hint at an undocumented wired interface?

## Merged questions for the owner

- Please photograph the white label on the drive unit, clipped to the left lift mechanism: article number (21FA…, 21KA/LA/SA…, 23KA… or 23.A…), BAU serial and FCC ID. Which AVENTOS type is it (HF, HS, HL or HK; top or not)?
- Which buttons does the drive have: <SWITCH>/<SYNC>/<COLL>/<Reset Motion>/<Reset Wireless>, or <Connect/Link>/<OK>/<Play 'test 2x'> with collision/reset? A photo would help.
- What part number is on the back of your radio switch (21P5020 or 23P5020), and which FCC ID? Does it have one LED (battery) or two (battery and Connect)? How many switches are paired, and if you open one, what does the PCB marking say?
- When was the cabinet bought or installed (before or after September 2025), and in which country or region (EU, UK, US/CA)?
- How do you normally open the flap: by pressing the front, with the switch, or by hand? Should the 5-minute timer start for all of them, and should closing by hand cancel it?
- Is this a single drive, or is it synchronised or collision-linked with other drives? How many?
- Which Blum power supply is installed (label Z10NE0…/Z10NA…)? Is there a free mains socket or USB port near the cabinet for a separate ESP32 supply?
- Which actuation route do you prefer? (a) Pair the ESP32 once as a sync partner using the drive's SYNC/Link button; this uses a sync slot, and a later Reset Wireless also unpairs your switches. (b) Buy a second 23P5020 and wire it in; opening it voids Blum's warranty on that part. (c) A mechanical presser.
- Quick tests on your unit: (1) with the flap closed, press the switch; does it open? (2) Press the switch during opening and during closing; does the flap stop, reverse, or ignore it? (3) Hold the switch 2–3 s with the flap open; is the press ignored? (4) Open the flap by hand, then press the switch; does it close?
- May a reed sensor and magnet be fitted on the carcass and flap? What are the materials near the bottom edge of the flap? Is there already an interior-light door contact?
- Do you want Home Assistant / Wi-Fi integration, or a standalone device? If Wi-Fi: is 5 GHz available, and which 2.4 GHz channel does your router use?
- Are children or pets around, or do items stick out past the cabinet edge? Do you want a warning beep/light before closing and a presence sensor veto? If closing is interrupted, should the device retry once, notify you, or give up?
- Which hardware and tools do you already have (nRF24L01+ modules, nRF52840 dongle, SDR, logic analyser, XIAO or other ESP32 boards, Arduino IDE or PlatformIO, 3D printer and filament, soldering iron for heat-set inserts)?
- Where in the cabinet could the enclosure go (free space, surface material), and may you drive two small screws there?

## Minor facts (not verified)

- Suffixes: 23.A000 / 23KA000 = set (drive plus distribution cable, node and end protectors); 23.A001 / 23KA001 = drive in multipack; 21FA000.2x / 001.2x / 005.2x = packaging and customer variants; drive serials have the form BAUxxxxxxx; drive colour TGR (RAL 7037).
- Drive and switch part numbers are the same worldwide. Only power supplies and cords are market-coded (Z10NE04xy, Z10NA40xy, where x is the market and y the language). US cover sets carry a '.NA' suffix, and the US sold 21P5020 as a 2-pack.
- 23P5020 replaces 21P5020 and works with all SERVO-DRIVE radio modules, according to dealer text copied from Blum. It comes in SW, HGR and TGR, is 42 mm long, symmetrical, and presses into a Ø35 × 11 mm bore whose centre is 12 mm from the front edge and 30–90 mm above the bottom panel (template M31.2000). A surface-mount adapter 23P5216 exists, and two switches ship in each cover-cap set.
- Switch battery LED: flashes red when the battery is low, lights red if the cell is reversed (MA-502/3 says it flashes). The switch's '1 W standby' figure in BA-139/7 is evidently a documentation error.
- Drive standby power: 0.22 W (21FA00x), 0.15 W (23.A001), 0.40 W (23KA001); rated current 2.0 A at 24 V DC; protection class III.
- Z10NG120 is a wall holder, not a power supply. Accessories: distribution cable Z10K800AE (8 m), connecting node Z10V100E.01, cable holder Z10K0009. Only one supply per distribution cable; plug-in supply cable at most 2 m; old US Z10NE020G was quoted for both 8 and 16 drives.
- SERVO-DRIVE uno uses the same drive and switch on a single-drive plug-in supply. SERVO-DRIVE flex uses the 23P5020 through receiver Z10C5007 (same module W95-21A00L33) or, from 2025, a built-in receiver. The drawer SERVO-DRIVE has no radio switch, and COMBOX is for drawers only.
- There is no SERVO-DRIVE for AVENTOS HK-S, HK-XS or HKi. '21HA000' appears in no Blum document; what 21LA/21SA were exactly is unresolved.
- Blum patents: the 2008 block diagram labels the radio 'nRF' and the drive MCU 'NEC 500 Series'. In the 2009 design the radio sits on a separate board linked over SPI. The drive learns end positions in a reference run, measures position with a potentiometer, and detects collisions from deviations against the learned speed profile. Typical travel: HF about 120°, HS about 130°, HL about 140°.
- The Port-Net example pins match a Seeed XIAO ESP32-C6 (CSN GPIO0, CE GPIO1, BOOT GPIO9, LED GPIO15). On a XIAO ESP32-C3, MISO is GPIO9 (the BOOT strapping pin), so CSN needs a pull-up and the pins must be remapped.
- Toolchain as of Nov 2026: RF24 v1.6.2 (IRQ masked by default since 1.6.0); arduino-esp32 3.3.12; PlatformIO platform-espressif32 v8.0.0 supports it. The library needs core 3.x (C++20 erase_if).
- Prices: XIAO ESP32-C3 $4.90 (about €7.90), C6 $5.20 (€8.50), nRF52840 $9.90 (€11.90); nRF52840 Dongle €9.78 excl. VAT; nRF24L01 3-pack about €7.99; 23P5020 about €29–38 in DE/AT, about $48–54 in the US.
- Enclosure: PLA/PETG walls of about 2 mm lose under 0.01 dB at 2.4 GHz; avoid conductive or filled filaments; keep at least 15 mm antenna clearance (Espressif guidance). Use YAPP_Box (MIT) or BOSL2 (BSD-2); NopSCADlib is GPL-3.0. ruthex M2.5 insert hole 4.0 mm; FDM snap-fit clearance about 0.5 mm.
- Transmit limits: EU ERC/REC 70-03 Annex 1 band i allows 10 mW e.i.r.p.; US 15.249 allows 50 mV/m at 3 m (about 0.75 mW). An nRF24 at RF24_PA_LOW (-12 dBm) is well within both; avoid PA+LNA modules at full power.
- An RTL-SDR cannot tune 2.4 GHz. For bench checks on the owner's own system, the RF24 scanner example, a second nRF24, an nRF52840 dongle in ESB monitor mode, or a HackRF with URH are enough.
- Wi-Fi channel 1 overlaps 2404/2410 MHz and channel 6 avoids them (inference); the XIAO ESP32-C5 can move its own Wi-Fi to 5 GHz.
- ESPHome is feasible through an external component (loucks1/nrf24_espidf supports dynamic and ACK payloads; kuralabs/esphome-nrf24 uses fixed 32-byte frames and does not fit). ESPHome now defaults to esp-idf; set the api and wifi reboot_timeout to 0s for a local safety device.
- Sensor notes: reed switches act at 13–25 mm, far more than the roughly 2 mm bumper gap, and should be wired so a broken wire reads as not closed. Tilt sensors fail on HL (parallel lift). A ToF sensor or an INA226 on the 24 V line could serve as an optional second signal.
- PhotoMOS AQY212EH: LED current 5–10 mA, about 330 Ω from a 3.3 V GPIO, 1 µA off-state leakage. Use a boot-quiet pin with a pull-down and treat the output as a self-ending pulse, never a latching switch.
- Garage-door practice (LiftMaster, Konnected): warn audibly and visually before an unattended close, make one retry, then stay open. IEC 60335-2-116:2025 covers motorised kitchen units; the EU Machinery Regulation 2023/1230 applies from 20 Jan 2027.
- Licences: RF24 GPL-2.0; nrf_to_nrf GPL-3.0; Port-Net's LICENSE file is GPL-2.0 while library.json says LGPL-3.0; the owner's repo is MIT.
- bluhm-de.com sells its channel and address data (donation of at least €15) and an Arduino sketch (€25); its free part matches Port-Net's frame layout. Other precedents: HA forum Sonoff SV (2019), Control4 forum (2024), Shelly Uni plan (2025), an unanswered KNX question (2025). pierreallier/Pilotage_ServoDrive_Blum drives the motor directly and is not relevant.