# DELIVERY — The Train of Seven Doors (v1.0.0)

Delivered 31 October 2026 (evening) → 1 November 2026 by **Z Code**, from the
specification written by Cédric Bernadac for a one-shot mission
(tracking: `MISSION.json`).

## What is delivered

A complete, self-contained browser adventure (~104 KiB `index.html`, zero
dependencies, zero network):

- 7 carriages + the midnight locomotive; 3 fork wagons offering
  **fight / encounter / supplies**; hand-written events in all three languages;
- turn-based combat with **announced intents** (exact damage, telegraphed
  charged blows), three base actions (strike / guard / lantern flash),
  multi-enemy targeting, shields, energy economy;
- **6 items** that change decisions (tart, tea, punched ticket, golden lantern,
  copper star, locomotive key) — every reward is a deliberate choice, no random
  drops;
- **3 pairs of upgrades** (one of two after wagons 2, 4, 6);
- 3 regular enemy types + a two-phase final adversary;
- **two endings** depending on the final choice, with an epilogue line that
  reacts to whether you helped the lantern child;
- defeat is forgiving: retry the current wagon at 70% HP, keeping items and
  upgrades; autosave + Continue; endings remembered on the title screen;
- EN / FR / ES complete (default EN), keyboard shortcuts, touch-friendly
  layout, reduced-motion support, synthesized audio with a mute toggle.

## Verification (honest report)

**Headless tests — 16/16 pass** (Node, run against the real `index.html` core;
suite kept in the workshop dev folder, outside this repo):
bundle self-containment, i18n key+placeholder parity EN/FR/ES, combat base
numbers (strike 5-7, flash 10/4 energy, guard shield 7/+2), trunk telegraph →
chomp, guard timing, ticket −4 vs inspectors only, lantern +1/turn, star on
every third strike, tart/tea in-combat turn cost, key −8 / first-strike −6 on
the finale, all six upgrades, both endings reachable + epilogue variance,
**winnability without any item use across 25 seeds**, full greedy path across
15 seeds, defeat → retry at 70% HP, determinism at equal seed, save/load
round-trip byte-identical, stage graph integrity.

**Balance simulations** (seeded, policy bots: guard when a ≥9 blow or ≥10
combined phase is shown, otherwise strike/flash the weakest):
- careful, minimal path, **no items at all: 60/60 wins, ~7 HP left** (the
  mission's winnability requirement, proven strictly);
- careful, greedy path (all optional fights + earned items): **60/60, ~4 HP** —
  greed is the self-selected hard mode;
- reckless (never guards, never uses items): **43/60 on the minimal path**,
  dying at the final phase — announced intents genuinely matter;
- current numbers: elite 34 HP (whistle / charge 18 / 18 / stamp 13),
  Stoker 28 (8 / charge 18 / 18 / hiss), Engineer 25 (charge 20 / 20 / 10 /
  whistle 5+1), chomp 13, stamp 14.

**Kimi K3.1 Pro consult** (31/10, session `session_ffa9460c`): verdict "readable
but too lenient"; proposal: +4 damage on the five telegraphed blows, targets
50-55/60 playing intents and ≤25/60 without guarding. **Tested and not
adopted**: in simulation +4 made careful play unwinnable on both paths
(0/60, deaths at the final phase) and +2 collapsed the greedy path (0/60),
while rushing the final boss while ignoring intents stayed at 60/60 — the
proposal punishes exactly the wrong players. The shipped numbers keep the
mission's winnability guarantee; difficulty is chosen by route (greedy path
ends at ~4 HP average).

**Real browser play** (IAB, local HTTP): two complete playthroughs —
(1) FR, greedy-ish: kitchen trunk fight, archivist's ticket, lantern child
(helped), cat trade (tart → key), Inspector of the Sixth Door, sleep, finale
both phases → **ending "Take the fire"** (7/7 doors, 5 fights, child epilogue
line present); (2) FR, quiet path: cook, archivist, watched the child, refused
the cat, sleep → **ending "Daybreak"** at 7 HP. Endings memory lit both stars;
EN/ES switches verified; 390×740 mobile combat layout verified; 11 screenshots
in the workshop `evidence/train-sept-portes/`. Playtesting found and fixed one
real bug (story page index not passed to the view — first page rendered
"undefined") plus a cosmetic fix ("Wagon 8" label on the locomotive).

## Known limits

- Balance was validated with three bot profiles × 60 seeds and two human-played
  runs; it was not tuned on a large human sample.
- The final boss can be rushed down by a max-DPS build before its biggest blow
  lands (a deliberate speed strategy — guarding still halves damage).
- Audio was designed synthetically; no formal listening pass.
- The tea item is only usable mid-fight by design; the tooltip says so.
