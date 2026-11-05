# smart-servo-drive-blum-aventos

Auto-close add-on for a Blum AVENTOS HF top lift with SERVO-DRIVE: an ESP32 device that notices when the flap has been opened and sends the drive a close command five minutes later. Later: a 3D-printed OpenSCAD enclosure.

## Status

Phase 1 (research) done, phase 2 (design) planned and awaiting validation (draft 1.1, 2026-11-05).

- `docs/plan.md` – recommended architecture, staged plan with go/no-go gates, bill of materials, safety measures, decisions D1–D10 and home checks. Published page: https://claude.ai/artifact/RrvkGMAwSHqTvnYmhZUmpy
- `research/README.md` – research index and condensed findings with status tags.
- `research/verification.md` – the 14 key claims with verdicts, corrected wording, evidence and sources; critic output; open questions.
- `research/raw/` – raw findings per research angle (evidence quotes, negative results, open questions) and the merged key claims.
- `research/research-report.md` – full synthesis (written by the background workflow when it completes).
- `research/aventos-auto-close-plan.html` – standalone copy of the plan page.

## Key facts in one breath

Blum's switch and drives talk Nordic Enhanced ShockBurst (nRF24L01+ class) on 2404/2410 MHz, not Bluetooth; an open-source ESP32 + nRF24L01+ library can join the drive as a virtual synchronised drive and send an absolute close, but it is untested on the HF top generation; the switch itself only sends a toggle trigger; a push on the front opens the flap with no radio frame at all, so a reed sensor on the flap is the reliable trigger; no Blum drive has a wired input and Blum offers no timer.
