---
type: project
status: active
area: "[[Devices]]"
tags: [project, todo]
updated: 2026-11-14
---
# Acer TravelMate Warranty Claim

The personal laptop (see [[About Me]]) switches off instantly on battery power. Windows logs show sudden power loss, not a Windows crash. The likely cause is the battery, battery connector or motherboard power circuit. It is under Acer warranty until 26 Aug 2027.

## Open items
- [ ] TODO: Log the warranty case with Acer South Africa (Contact Acer on acer.com/za-en) using the draft below. Record the case/RMA number here.
- [ ] TODO: Download the BobShop invoice and save the listing page (screenshots of the description, seller name and date) to `Attachments/`.
- [ ] TODO: Send the seller the heads-up message below through BobShop messages.
- [ ] TODO: Check BobShop's buyer-protection or dispute terms and how long there is to raise an issue.
- [ ] TODO: Check whether the BobShop seller is a registered business (Consumer Protection Act 6-month implied warranty applies only to business sellers).
- [ ] TODO: Ask Acer whether the battery has a shorter warranty than the laptop, and whether they honour a Europe-region ("EA") unit in South Africa.
- [ ] TODO: Back up the laptop (push the vault to a remote) before sending it in.
- [ ] TODO: Decide on the login for the technician (temporary local account or share the password in person).

## Device details

| Field | Value |
|---|---|
| Model | TravelMate P414RN-53-TCO (TMP414RN-53-TCO) |
| Part number | NX.B26EA.003 |
| Serial number | NXB26EA0034300E7F77600 |
| Warranty expiry | 26 Aug 2027 (per Acer's website) |
| Processor / RAM | Intel Core i5-1335U / 32 GB |
| BIOS | V1.60 (latest on Acer's site) |
| Battery | AP22ABN |
| 4G modem IMEI | 867228050409688 (Quectel EM060K-GL; sticker worn, visible digits 409688 match) |
| Bought | Second-hand on BobShop, received 20 or 21 Oct 2026 |

## Evidence from the Windows logs
- 14 unexpected shutdowns from 21 Oct to 14 Nov 2026. Every one: Kernel-Power 41, bugcheck 0, power button not pressed.
- No blue screen and no crash dump, although crash dumps are enabled.
- No WHEA (CPU/memory), disk, thermal or battery-driver errors.
- Charger state at the moment of shutdown:
  - 25 Oct: died at the moment the charger was unplugged. Strongest single clue.
  - 14 Nov 21:20: unplugged, died about 11 minutes later.
  - 14 Nov 21:57: unplugged after the first battery reset, died about 5 minutes later.
  - 7–8 Nov: battery report shows battery use shortly before each.
  - 13 Nov (3 in a row): on battery, by Matthew's account. Windows then went into Automatic Repair; Startup Repair found nothing.
- It has never cut out on the charger (about 3 hours of logged active use plugged in, plus normal use).
- Shutdowns started on 21 Oct, about 2 hours after Windows Update installed BIOS V1.60 (5.44.1.60). V1.60 only adds Secure Boot certificates; the EC firmware last changed in V1.27. The previous BIOS version is unknown.

## Already tried
- BIOS: already on the latest version (V1.60).
- Intel graphics driver updated to 32.0.101.7079 (14 Nov 2026).
- Battery reset with the pinhole on the underside, done twice. It still cut out on battery about 5 minutes after the first reset.
- BitLocker is off on drive C, so a battery reset won't trigger a recovery-key prompt.

## Draft message to Acer

**Subject:** Warranty repair – TravelMate P414RN-53-TCO shuts off instantly on battery power (S/N NXB26EA0034300E7F77600)

Good day,

I'd like to log a warranty repair for my Acer laptop.

**Device details**
- Model: TravelMate P414RN-53-TCO (TMP414RN-53-TCO)
- Part number: NX.B26EA.003
- Serial number: NXB26EA0034300E7F77600
- Warranty expiry, per Acer's website: 26 Aug 2027
- BIOS: V1.60 (latest on Acer's support site)
- OS: Windows 11 Pro

**Fault**
- When running on battery, the laptop switches off instantly, with no warning, at any charge level from about 67% to 100%.
- It has done this 14 times since 21 October 2026, sometimes within seconds of starting and sometimes after up to about an hour.
- It has never switched off while connected to the charger.
- On one occasion (25 October) it switched off at the exact moment the charger was unplugged.
- After 3 cut-outs in a row, Windows went into Automatic Repair. Startup Repair found no problems.

**Diagnostics already done**
- Windows logs show Kernel-Power event 41 for every cut-out, with bugcheck code 0 and no power-button press. There was no blue screen and no crash dump, even though crash dumps are enabled. This indicates a sudden loss of power, not a Windows crash.
- No hardware, disk, thermal or battery-driver errors are logged.
- BIOS is on the latest version (V1.60).
- Intel graphics driver updated to Acer's latest (32.0.101.7079).
- Battery reset done twice with the pinhole on the underside. It switched off again on battery about 5 minutes after the first reset.

I believe the fault is in the battery (AP22ABN), the battery connector, or the motherboard power circuit. Please advise on the repair process and the nearest service option.

Regards,
[Your full name]
[Contact number]
[Physical address, if collection is needed]

## Message to the seller (BobShop)

> Hi there,
>
> Thanks again for the TravelMate. I'm really enjoying it. Just a quick heads-up: it's developed a quirk where it switches off when running on battery, though it's perfectly fine on the charger. It's still under Acer's warranty until 2027, so I'm going to have Acer look at it.
>
> To help with the warranty claim, would you happen to have any of the original purchase paperwork, or know where it was originally bought? No worries if not.
>
> Thanks, and have a great day!

## Expected warranty process

| Step | What to expect |
|---|---|
| Log the case | Online or by phone. Keep the case/RMA number. |
| First-line troubleshooting | They may ask for a battery reset or BIOS update. Both are done. |
| Proof of purchase | Usually by serial number. They may ask for an invoice; use the BobShop invoice. |
| Getting it to them | Acer-authorised service centre or courier collection. Ask which applies. |
| Diagnosis and repair | Test and replace the faulty part. Roughly 1–3 weeks depending on parts. |
| Return | Ask whether they will wipe or reinstall Windows. |

## Until it's repaired
- Use it only on the charger.
- Only run Windows or BIOS updates while plugged in.
- If it has to run on battery, save often and expect it to die at any moment.

## Fallbacks
- If Acer won't cover the battery: argue that the 25 Oct cut-out at the moment of unplugging may be a connector or motherboard fault, which should still be covered.
- If that fails: Consumer Protection Act rights against the seller (business sellers only), or BobShop's dispute process.
- Free IMEI blacklist check (optional): put a SIM in the laptop; if it connects to a South African network, it isn't blocked there.

Back to [[Devices]]
