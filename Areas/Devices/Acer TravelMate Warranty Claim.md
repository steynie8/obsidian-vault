---
type: project
status: active
area: "[[Devices]]"
tags: [project, todo]
updated: 2026-11-19
---
# Acer TravelMate Warranty Claim

The personal laptop (see [[About Me]]) switches off instantly on battery power. Windows logs show sudden power loss, not a Windows crash. The likely cause is the battery, battery connector or motherboard power circuit. It is under Acer warranty until 26 Aug 2027.

## Open items
- [ ] TODO: Log the warranty case with Acer South Africa (Contact Acer on acer.com/za-en) using the draft below. Record the case/RMA number here.
- [ ] TODO: Download the BobShop invoice and save the listing page (screenshots of the description, seller name and date) to `Attachments/`.
- [x] Send the seller the heads-up message below through [[BobShop]] messages. Seller replied by 19 Nov 2026 (see below).
- [ ] TODO: Send the seller the reply below (decline collection for now, keep their offer as a fallback).
- [ ] TODO: Check BobShop's buyer-protection or dispute terms and how long there is to raise an issue.
- [ ] TODO: Check whether the BobShop seller is a registered business (Consumer Protection Act 6-month implied warranty applies only to business sellers).
- [ ] TODO: Ask Acer whether the battery has a shorter warranty than the laptop, and whether they honour a Europe-region ("EA") unit in South Africa.
- [ ] TODO: Back up the laptop (push the vault to a remote) before sending it in.
- [ ] TODO: Decide on the login for the technician (temporary local account or share the password in person).
- [ ] TODO: Before handing the laptop to Acer, undo the battery CPU cap so they can see the fault: `powercfg /setdcvalueindex SCHEME_CURRENT SUB_PROCESSOR PROCTHROTTLEMAX 100` then `powercfg /setactive SCHEME_CURRENT`.
- [ ] TODO: Record whether the cut-outs stop on battery with the CPU cap on (test started 19 Nov 2026 23:43).

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
- 17 unexpected shutdowns from 21 Oct to 19 Nov 2026. Every one: Kernel-Power 41, bugcheck 0, power button not pressed, followed by a cold boot (BootType 0).
- No blue screen and no crash dump, although crash dumps are enabled.
- No WHEA (CPU/memory), disk, thermal or battery-driver errors.
- Charger state at the moment of shutdown:
  - 25 Oct: died at the moment the charger was unplugged. Strongest single clue.
  - 14 Nov 21:20: unplugged, died about 11 minutes later.
  - 14 Nov 21:57: unplugged after the first battery reset, died about 5 minutes later.
  - 7–8 Nov: battery report shows battery use shortly before each.
  - 13 Nov (3 in a row): on battery, by Matthew's account. Windows then went into Automatic Repair; Startup Repair found nothing.
  - 14 Nov ~23:55: **on the charger**, but during a sleep or shutdown transition (Kernel-Power 41 SleepInProgress=6), not during normal use. Windows Update was installing at 23:51 (an Intel extension update failed with 0x8024200B). Matthew may have closed the lid. Exact trigger unknown.
  - 17 Nov 01:15: entered hibernation on battery, then failed to resume (found 19 Nov 21:34, Kernel-Power 41 SleepInProgress=5). Power may have died while the session was being saved.
  - 19 Nov ~21:34: on battery, cut out about 1 minute after starting.
- It has never cut out on the charger during normal use (about 3 hours of logged active use plugged in, plus normal use). The only plugged-in failure was the 14 Nov sleep/shutdown transition above.
- Shutdowns started on 21 Oct, about 2 hours after Windows Update installed BIOS V1.60 (5.44.1.60). V1.60 only adds Secure Boot certificates; the EC firmware last changed in V1.27. The previous BIOS version is unknown.

## Already tried
- BIOS: already on the latest version (V1.60).
- Intel graphics driver updated to 32.0.101.7079 (14 Nov 2026).
- Battery reset with the pinhole on the underside, done twice. It still cut out on battery about 5 minutes after the first reset.
- BitLocker is off on drive C, so a battery reset won't trigger a recovery-key prompt.
- Load trigger (19 Nov 2026): 5 cut-outs on battery that evening (21:34, 22:43, 23:18, 23:29, 23:32), twice the moment Chrome was opened. 21 unexpected shutdowns in total. Fits a battery that can't deliver peak current: the voltage sags under a sudden load and the power cuts out.
- Workaround and test (19 Nov 2026 23:43): "Maximum processor state" on battery capped at 70% in the Balanced plan (was 100%). The charger setting is unchanged at 100%. If the cut-outs stop, that confirms the load theory.

## Seller's hibernation theory (checked 19 Nov 2026)
The seller suggested it is hibernating because of a BIOS or Windows setting, and offered to collect it and test it. The logs don't support this:

| If it were hibernation | What the logs show |
|---|---|
| "System is entering sleep" event first (Kernel-Power 42) | None before any cut-out during use |
| Resumes with open apps | Cold boot every time (BootType 0), nothing restored |
| No "unexpected shutdown" error | Kernel-Power 41 and event 6008 every time |
| No Automatic Repair | Automatic Repair triggered on 13 Nov |

| Setting (Balanced plan) | Value |
|---|---|
| Critical battery action | Hibernate, at 2% only |
| Low battery action | None, at 6% |
| Sleep after (battery / charger) | 10 min idle / 15 min idle |

- Cut-outs happen at 67–100% charge, during active use, 15 seconds to about an hour after starting. No setting triggers at those levels.
- The battery reports a healthy capacity (65.6 Wh full against 65.0 Wh design). Capacity says how much energy it holds, not whether it can deliver power under load, so a "battery above 70%" test doesn't point to a setting.
- Decision: decline collection for now and go to Acer first. Keep the seller's offer as a fallback.

## Draft message to Acer
Log it through Fix My Acer (Acer South Africa's online repair booking): https://customercare.acer-euro.com/customerselfservice/CaseBooking.aspx?CID=ZA&LID=ENG&OP=1. Phone 087 151 3400 (Mon–Fri 08:00–17:00). Track the case at https://za.answers.acer.com/app/case_status.

### Short version (for the form's fault-description box)
> Laptop switches off instantly on battery power, at any charge level (42–100%), with no warning. 21 times since 21 Oct 2026. Triggered by sudden load: it switches off the moment Chrome is opened or within seconds of starting up. Never switches off during normal use on the charger. Once it died at the exact moment the charger was unplugged. Windows logs show Kernel-Power 41 with bugcheck 0 (sudden power loss, no blue screen, no crash dump). BIOS V1.60 (latest), battery reset done twice: no change. Suspect battery (AP22ABN), connector or motherboard power circuit.

### Full version

**Subject:** Warranty repair – TravelMate P414RN-53-TCO shuts off instantly on battery power (S/N NXB26EA0034300E7F77600)

Good day,

I'd like to log a warranty repair for my Acer laptop.

**Device details**
- Model: TravelMate P414RN-53-TCO (TMP414RN-53-TCO)
- Part number: NX.B26EA.003
- Serial number: NXB26EA0034300E7F77600
- Warranty expiry, per Acer's website: 26 Aug 2027
- BIOS: V1.60 (latest on Acer's support site)
- Battery: AP22ABN
- OS: Windows 11 Pro
- Purchased second-hand in October 2026. The invoice is available on request.

**Fault**
- When running on battery, the laptop switches off instantly, with no warning, at any charge level from about 42% to 100%.
- It has done this 21 times since 21 October 2026, including 5 times on the evening of 19 November.
- The shutdowns are triggered by sudden load: twice on 19 November it switched off the moment Chrome was opened, and several times within seconds of starting up. This suggests the battery cannot sustain peak current.
- It has never switched off during normal use while connected to the charger. Once (14 November) it failed during a sleep or shutdown while plugged in.
- Once (17 November) it failed to resume from hibernation on battery.
- On 25 October it switched off at the exact moment the charger was unplugged.
- On 13 November, after three cut-outs in a row, Windows went into Automatic Repair. Startup Repair found no problems.

**Diagnostics already done**
- Windows logs show Kernel-Power event 41 for every cut-out, with bugcheck code 0 and no power-button press. There was no blue screen and no crash dump, even though crash dumps are enabled. This indicates a sudden loss of power, not a Windows crash.
- No hardware (WHEA), disk, thermal or battery-driver errors are logged.
- Power settings checked: hibernation only triggers at 2% battery, so it isn't a sleep or hibernation setting.
- BIOS is on the latest version (V1.60).
- Intel graphics driver updated to Acer's latest (32.0.101.7079).
- Battery reset done twice with the pinhole on the underside. It switched off again on battery about 5 minutes after the first reset.

I believe the fault is in the battery (AP22ABN), the battery connector, or the motherboard power circuit. A load test of the battery should show it. Could you please also confirm:
- whether the battery is covered for the full warranty period, and
- that the warranty is honoured in South Africa, as the part number indicates a Europe-region unit.

Please advise on the next steps and the nearest service option.

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

### Seller's reply (received by 19 Nov 2026, summarised)
- Thinks it is going into hibernation because of BIOS or Windows settings, common on some Acer models.
- Suggested an online battery test: if the battery is above 70%, it's a setting.
- Offered to collect it, test it, and book it with their repair agent if it's a bigger problem.

### Reply to the seller (draft)

> Morning, thanks so much for getting back to me and for the offer to help, I really appreciate it.
>
> I did check the hibernation and sleep settings. Windows logs these shutdowns as a sudden loss of power rather than hibernation, and it restarts fresh each time instead of resuming. Since it's still under Acer's warranty until 2027, I'll let Acer test the battery and board first.
>
> If Acer can't help, I'll gladly take you up on your offer. I'll keep you posted. Thanks again!

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
- If that fails: take up the seller's offer to collect and book it with their repair agent, or use Consumer Protection Act rights against the seller (business sellers only), or BobShop's dispute process.
- Free IMEI blacklist check (optional): put a SIM in the laptop; if it connects to a South African network, it isn't blocked there.

Back to [[Devices]]
