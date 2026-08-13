# WW2 Encounters — Expansion Roadmap & To-Do List

A staged plan for growing the mod from its current state (Naval + Air + Installations, Gray/Green factions) into a fuller PvPvE experience with Ship-Core-driven progression, borrowing proven systems from Ares at War and GV Deserts of Kharak Season 10. Ordered so small, achievable, playable milestones come first, and grid-design-heavy work comes only once the systems around it are proven.

---

## Prerequisite — Weapon System Rebuild

Not a stage so much as a foundation everything else sits on. The current naval weapon mod is effectively dead, so every existing ship and plane needs rebuilding onto a new weapon system before Ship Core work can be meaningfully tested — the whole premise of Stage 1 is gating access to ships you already have, and that doesn't hold if those ships don't load.

- **Naval weapons:** [Fletcher Armaments – WeaponCore Edition](https://steamcommunity.com/sharedfiles/filedetails/?id=2844434226) (Workshop 2844434226) — WeaponCore-based WW2 naval weapon pack.
- **Air weapons:** [Consty Aircraft Pack – Ordnance (WeaponCore) 1.0](https://steamcommunity.com/sharedfiles/filedetails/?id=2881339118) by Const (Workshop 2881339118), companion to Const's Tech-Focused Aircraft Pack. Confirmed via its changelog to mix genuinely period-appropriate content (.50 cal guns, 30mm aircraft cannon, unguided bombs, rocket pods) with clearly modern/anachronistic content (AIM-7/54/120 guided missiles, 5.56mm burst rifles, 9mm SMGs, .45 handguns). The lockdown plan is sound and necessary, not just thematic tidiness — gate/exclude the modern-coded weapons, keep the period-coded ones, using the same tech-item mechanism as Stage 1's unlock currency.
- **Ground vehicle weapons: confirmed — KONTAKT Ground Systems [WeaponCore] v1.0.** Mostly fixed weapons. Same identification and BlockGroup-building process as Fletcher Armaments/Consty's Ordnance once you're ready to pull its block SubtypeIds.
- **CWP replacement candidates**
  - **SETB Community Tank Parts** — replace all existing armor blocks, enabling the ground vehicle rebuild off AWG CWP armor specifically.
  - **ArmourEssentials** — Mostly turret ring rotors, gunsight cameras, and rangefinders.
  - **Yakobe's Machinations** — wheels/suspension sourced from CWP, plus additional guns.
  - **SETB - Multicrew - MODPACK** — the full SETB bundle. Not intended for wholesale adoption, but worth keeping as a reference source.
- **Turret philosophy shift:** moving away from custom rotor/hinge-built turrets toward general WeaponCore turret blocks. This should make future `BlockGroup`/`BlockLimit` definitions for the Core system far more stable, since they'll reference known WeaponCore block types instead of a dead mod's custom subtypes.
- **Dependency additions:** WeaponCore itself, plus whichever specific weapon-pack mods end up used. Worth checking MES's documented WeaponCore compatibility notes once rebuild work starts, since combining MES/RivalAI-driven NPCs with WeaponCore weapons is a well-trodden combination but has its own configuration quirks (weapon targeting profiles, ammo replenishment behavior).

**To-do:**
- [ ] Pull KONTAKT Ground Systems' block SubtypeIds and TypeId the same way Consty's Ordnance was handled.
- [ ] Confirm whether SETB Community Tank Parts includes large grid rotors — determines if it's a full or partial AWG CWP replacement.
- [ ] Cross-check Yakobe's Machinations' gun roster against KONTAKT's for overlapping/colliding SubtypeIds before using both.
- [ ] Decide final ground vehicle scale factors within the confirmed 110–120% (military) / up to 150% (civilian) ranges, and rescale the existing Fiat 626 and Renault builds if the civilian figure ends up above 100%.
- [ ] Pull the full Consty Ordnance block list and sort into "period, keep" vs. "modern, gate or exclude" — AIM-7/54/120 and the modern small arms are confirmed non-period from the changelog alone, but the full list needs a proper pass.
- [ ] Rebuild each existing hull's weapon fit onto Fletcher Armaments / Consty's Ordnance, replacing custom rotor/hinge turret rigs with general WeaponCore turrets. Broken out per-prefab below so each is a single sitting's work — check the box when that prefab's weapons are rebuilt and it loads clean with the old mod removed. Naval uses Fletcher Armaments; Air uses Consty's Ordnance (WW2-appropriate blocks only, per the lockdown scope above); Installations may need either or neither depending on what defenses they actually carry.

  **Naval:**
  - [ ] NPC-WW2-Golo_Italian (Cargo Ship, Gray)
  - [ ] NPC-WW2-Golo_French (Cargo Ship, Green)
  - [ ] NPC-WW2-Gabbiano (Corvette, Gray)
  - [ ] NPC-WW2-Spica (Torpedo Boat, Gray)
  - [ ] NPC-WW2-Francesco_Crispi (Destroyer, Gray)
  - [ ] NPC-WW2-Comandante_Margottini (Destroyer, Gray)
  - [ ] NPC-WW2-La_Malouine (Corvette, Green)
  - [ ] NPC-WW2-Bougainville (Aviso/Destroyer-behavior, Green)
  - [ ] NPC-WW2-Le_Triomphant (Destroyer, Green)
  - [ ] NPC-WW2-Emile_Bertin (Cruiser, Green) — **confirmed root cause found:** the AWG piston/hinge-based catapult+floatplane assembly (Loire 130) silently breaks grid loading — no exception, no error, just fails to register as an entity. Reproduced and fixed by surgical removal in a build-world test; same mechanism confirmed present on Algerie (Green Heavy Cruiser) and absent on Trento (Gray Heavy Cruiser, which has never shown the failure — clean independent confirmation). Rebuild the catapult using non-AWG piston/hinge parts (or the general WeaponCore turret approach already planned for weapons) before this hull is usable in a SpawnGroup.
  - [ ] NPC-WW2-Aquila (Carrier, Gray)
  - [ ] NPC-WW2-Bearn (Carrier, Green)

  **Air:**
  - [ ] NPC-WW2-Re2001 (Fighter, Gray)
  - [ ] NPC-WW2-Re2000 (Fighter, Gray)
  - [ ] NPC-WW2-FC20 (Attacker, Gray)
  - [ ] NPC-WW2-F4F (Fighter, Green)
  - [ ] NPC-WW2-MS406 (Fighter, Green)
  - [ ] NPC-WW2-Potez630 (Attacker, Green)
  - [ ] NPC-WW2-V-156-F (Attacker, Green)
  - [ ] NPC-WW2-Ju52 (Cargo Plane, Gray)
  - [ ] NPC-WW2-F222 (Cargo Plane, Green)
  - [ ] NPC-WW2-C47 — **not a rebuild task.** Already orphaned/unused, superseded by F222. Decide retire-vs-rebuild first; don't spend a session rebuilding a hull you've already said you don't want.

  **Installations:**
  - [ ] NPC-WW2-Ammo-Depot-1
  - [ ] NPC-WW2-Hangar-FC20
  - [ ] NPC-WW2-Hangar-MS406
  - [ ] NPC-WW2-Hangar-Potez630
  - [ ] NPC-WW2-Hangar-Re2001
  - [ ] NPC-WW2-Factory-Plane-FC20
  - [ ] NPC-WW2-Factory-Plane-MS406
  - [ ] NPC-WW2-Factory-Plane-Potez630
  - [ ] NPC-WW2-Factory-Plane-Re2000
  - [ ] NPC-WW2-Factory-Plane-Re2001
- [ ] Re-verify SpawnGroups/Behaviors/Loot still reference correct ammo/weapon subtype IDs after rebuild (loot container definitions currently reference old ammo names like `FiddyShellWC`, `HispanoDrumAP` — these will need updating).
- [ ] Once rebuilt, re-run the same cross-reference audit process used earlier in this project (defined-vs-referenced SubtypeId sweep) to catch anything broken by the swap.

---

## Stage 1 — Core Progression: Vehicles and Bases

**Goal:** everything a player builds — ships, planes, tanks, and bases — is capped by a placeable Core block (via Ship Core Framework, Workshop 3552595651), starting small and unlocking bigger/better through play. No new grids required beyond what the weapon rebuild already touches.

### Confirmed tier ladders

| Category | Civilian track | Military track |
|---|---|---|
| Naval | **Civilian** (Golo-scale) | **Corvette** → Destroyer → Cruiser → Heavy Cruiser → Battleship → Carrier |
| Air | **Cargo Plane** (Ju52/F222-scale) | **Fighter** → Attacker/Bomber → Heavy Bomber |
| Wheeled | **Transport** (new) | **Armored Car** (new) → Light Tank → Medium Tank → Heavy Tank |
| Base | *(no civilian/military split)* | **Base** (few per faction, HQ-scale) and **Outpost** (more per faction, smaller) — both available from the start, not sequential |

**Stage 1 scope specifically:** only the starting rung of each track, plus Base and Outpost (both needed since they're parallel, not sequential). That's Civilian Naval, Corvette Naval, Cargo Plane, Fighter, Transport, Armored Car, Base, Outpost — eight core definitions — plus the unlock-gate system working end-to-end for exactly one proof-of-concept upgrade (Corvette → Destroyer). Everything past that first rung on each ladder belongs to Stage 5.

### Ground vehicle scale and weapon dependency (confirmed)

**No large grid wheels — the entire Wheeled category is small grid only.** This is a firm constraint, not a default: no large-grid wheeled vehicles exist anywhere in the ladder.

**Scale factor: two-tier, driven by functional-block real estate, not wheel proportions.** Correctly-sized wheels exist, so 1:1 is viable on that front — the actual driver is fitting functional blocks (cockpit, respawn kit, cargo, weapon mount) inside a genuinely small real-world hull at small-grid's 0.5m block size. This hits Armored Cars hardest — historically some of the smallest vehicles in the whole roster, and the tightest fit for the same reason. Same two-tier structure still applies, just for the corrected reason: weapon blocks are fixed dimensions that don't rescale with the hull, so military vehicles stay conservative; civilian vehicles have no such anchor and can flex further if it helps fit functional blocks comfortably.
- **Military track (Armored Car → Heavy Tank):** capped around **110–120%** of real-world scale.
- **Civilian track (Transport):** allowed up to **150%**.

**Note for your own builds, not a mod-content concern:** vanilla SE's AI Flight block doesn't support wheeled vehicle propulsion at all — confirmed via Keen's own support forum, a longstanding unaddressed gap, not a bug on your end. Doesn't affect your NPC Armored Cars/tanks, which run on RivalAI/MES (already proven for ground units), but rules out the vanilla Automatons toolkit specifically if you ever want a player-built AI escort — that'd need a dedicated third-party rover-AI script instead.

**Existing assets affected:** the Fiat 626 (Italian) and Renault AHR-or-similar (French) are both built at 1:1 and are the natural Transport-tier hulls. If civilian scale-up is adopted, both need rescaling before use — worth deciding before further detail work goes into them at the wrong scale.

### Wheeled roster (confirmed)

Researched and cross-checked against real WW2 deployment history — including a look at whether either faction fielded another nation's equipment, since neither historically had a proper heavy tank of its own by international classification. Resolved without needing to borrow across the Axis/Allied divide: both factions have strong native tank-destroyer/SPG lineages that fill the Heavy tier honestly.

| Tier | Gray (Italy) | Green (France) |
|---|---|---|
| **Transport** | Fiat 626 *(built)* | Renault AHR-or-similar *(built)* |
| **Armored Car** | AB 41; **L3/35** (reclassified down from Light Tank — a 3.2-ton MG-only tankette reads closer to a light armored car by international standards than to a contemporary light tank) | Panhard 178 (AMD 35) |
| **Light Tank** | **L6/40** (now Italy's sole Light Tank entry following L3/35's move) | Renault R35, Hotchkiss H35/H39 |
| **Medium Tank** | M13/40, M14/41; **Semovente da 75/18** (Italy's most-produced SPG, same chassis family, arguably a better vehicle than the tank it's based on) | SOMUA S35, Char D2 |
| **Heavy Tank** | **P26/40** (kept — Italy's own "pesante" classification, even though internationally comparable to a Panzer IV/early Sherman); **Semovente da 90/53 or da 105/25** (native tank-hunter answer to the international-heavy-tank gap, no borrowed equipment needed — final pick between the two still open) | **Char B1 (bis)** (holds up as genuinely heavy under international classification too); **M10 Wolverine** (227–443 delivered to Free French forces 1943–45, real documented combat history including the Battle of Dompaire) |

**Ruled out during research, worth remembering so it doesn't resurface as an assumption:**
- Tiger/Panther for Italy — Germany never transferred either to Italian units; confirmed dead end, not a style choice.
- M7 Priest for France — real, well-documented Lend-Lease option, but declined on purpose (indirect fire, out of scope for now).
- Churchill for France — searched, found no confirmation either way; treat as unverified, not assumed real.
- Captured R35 for Italy — genuinely real (German-supplied, 1941, two real battalions), but set aside over faction-color confusion risk (an R35 in Gray sitting next to the actual Green-faction R35).

### Mechanical design (confirmed against the actual Ship Core Framework v3 schema)

- `MobilityType`: `Mobile` for vehicle cores, `Static` for Base/Outpost. One enum field, not two booleans.
- **Civilian vs. Military differentiation:** two levers —
  1. `Modifiers` — Military cores get `RefineSpeed`/`RefineEfficiency`/`AssemblerSpeed` reduced (~0.5) even where a few production blocks are allowed; Civilian cores stay at baseline or above.
  2. `BlockLimits` — cap weapon block counts low on Civilian cores, cap production/cargo block counts low on Military cores, via `BlockGroup` definitions. Blocked on the weapon rebuild above for the weapon-side groups; production/cargo/tool-side groups can be written now since they're vanilla block types.
- **Speed/boost:** `MaxSpeed`/`MaxBoost` are fractions of the world's `MaxPossibleSpeedMetersPerSecond`, not absolute values — changing the world setting rescales every core's absolute speed automatically without touching individual core files. Currently set to 150 m/s; **raising to 250 m/s under consideration, blocked on a physics test.** A known subgrid pitch-down bug (planes tipping toward the ground) has previously appeared around 150 m/s+ in testing — general subgrid pitch/tilt bugs are well-documented in the wider SE community (dampeners not accounting for subgrid mass, phantom forces from unshared inertia tensors), but the specific 150 m/s threshold isn't something I could independently confirm as a documented trigger, so it's this project's own prior finding, not a verified external one. Test incrementally on an actual subgrid-bearing plane before committing to 250. If adopted, revisit the tuning (not just the mechanics) of every already-written `MaxSpeed`/`MaxBoost` fraction — they'll still function unchanged, but the resulting absolute speeds may no longer match the intended feel. Boost itself is duration/cooldown-limited only — the framework has no native fuel-drain boost mode. Naval "flank speed" will use the same duration/cooldown mechanism as air/wheeled for now (tuned with a longer duration to read as "sustained" rather than "burst"); a true fuel-cost mechanic is a possible custom-trigger addition later if the simple version doesn't feel right.
- **Upgrade path (confirmed, deferred to later):** the "upgrade a Corvette core to allow more guns/propellers" idea maps directly onto `UpgradeModule.BlockLimitModifiers` — a real, first-class feature of the framework, not something to build custom. Stage 1 doesn't need this yet; Stage 1 is core-swap only (place a better core block outright). Converting to upgrade-modules-on-top-of-one-core is a clean later refinement once tier numbers are proven, not a Stage 1 requirement.

### Unlock economy

**Confirmed: territory/salvage-based, mostly salvage to start.** Fed by what a spawn *is* — military or civilian-themed — not by whether it was killed or found already wrecked. Every spawn type should exist in both an active (fight it) and wrecked (salvage it) form where that makes sense, but the loot-sourcing split runs along the military/civilian axis, not the alive/dead one:

1. **Military spawns** (active or wrecked) — combat ships, planes, weapon emplacements. Primary source of Military and Advanced Military Components.
2. **Civilian spawns** (active or wrecked) — cargo ships, transports, civilian installations. Primary source of Industrial Components.

Both routes reward exploring and engaging with the world rather than a pure kill-counter. Territory control (Stage 3) becomes a further feed once captured installations exist — not part of Stage 1, since territory doesn't exist yet.

**Confirmed component names, structure, and sourcing rules.** Three different components, not one generic currency:

- **Industrial Components** — gates Advanced Civilian production blocks. Sourced from: advanced production blocks (a player can *manufacture* these, not just find them), trade-location purchases, and loot — weighted heavily toward civilian-themed spawns, present in smaller amounts on military-themed spawns too.
- **Military Components** — gates first-tier combat hulls at low quantity. Sourced from: non-basic weapon blocks, advanced production output, trade-location purchases, and loot — weighted heavily toward military-themed spawns, present in smaller amounts on civilian-themed spawns too.
- **Advanced Military Components** — gates top-tier combat hulls. Sourced from: advanced weapons, advanced production output, trade-location purchases (**gated behind good faction standing**, not just currency), and loot — at very low quantity on spawn classes that wouldn't themselves need Advanced Military Components to build as a player core, and at meaningfully higher quantity on spawn classes that would. This ties loot value directly to the same tier scale as build cost, rather than being a separate system that happens to coexist with it.

**Basic/starting-tier everything uses plain vanilla components, no specialized item at all** — the unlock materials only enter the picture once a player is unlocking *beyond* the starting rung. This applies to weapons too: **basic weapons cost vanilla components, non-basic/advanced weapons cost Military Components** (or Advanced Military Components at the top end) — the same tiering logic already applied to hulls, applied consistently to weapon blocks as well. Practical implication for the weapon rebuild: the `WeaponBlocks` BlockGroup splits already done (Guns/Bombs/Torpedoes/Smoke) will need a further basic/advanced sub-classification once real costing starts — flagged as a to-do, not resolved yet.

**Trade locations** (Stage 5's planned economy stations) will eventually sell Industrial, Military Components, and Advanced Military Components all locked behind good faction standing — the first concrete gameplay payoff for Stage 2's reputation system, which otherwise is purely behavioral/flavor. Reputation stops being just "how NPCs act toward you" and becomes something that gates real economic access.

**Fleet-size limiting: fixed cost + finite income, not escalating price.** A core costs a flat amount of a genuinely scarce component; since the component is earned at a tuned (slow) rate, a player's standing fleet is naturally capped by how much they've banked and haven't spent — no special mechanic needed for this, it's just a normal crafting cost against a limited income rate. An escalating-cost variant (each *additional* core of a type costing more, based on how many are currently built and standing — not lifetime built, resets on loss) is a genuinely different and harder feature, since it requires live-tracking currently-placed grids and dynamically adjusting a cost, which needs real scripting rather than static XML. Interesting for later, but it's a Stage 5+ idea, not Stage 1.

**Production-block spam is already a solved problem, not a new one.** `BlockLimits` on the core itself already caps "how many Assembler blocks total" independent of what gates the upgrade — no separate anti-spam mechanic needed if Advanced Civilian production blocks end up costing Industrial Components.

### Two smaller open items

- **Static defensive-only grids** (a "gun outpost" concept) — already covered by the existing Base/Outpost `WeaponCap` BlockLimits (4 and 2 respectively), no new core type needed. Revisit only if that turns out to feel insufficient in practice.
- **Small utility grids with no functional blocks** (ramps, bridges, similar) — most core-limiting mods of this style allow a small default block count on any grid with no core placed at all, specifically so minor builds aren't blocked. Confirmed for Ship Core Framework specifically. Non core grids will also have a production block limit of 0.

### Core costs by tier (confirmed)

| Category | Starting tier (vanilla components) | Mid tier (Military Components) | Top tier (Advanced Military Components) |
|---|---|---|---|
| **Naval** | Civilian, Corvette | Destroyer (low), Cruiser (many) | Heavy Cruiser (low), Battleship (very many), Carrier (medium) |
| **Air** | Cargo Plane, Fighter | Attacker/Bomber (low–medium) | Heavy Bomber (low) |
| **Wheeled** | Transport, Armored Car | Light Tank (very few), Medium Tank (low) | Heavy Tank (low) |
| **Base** | Base, Outpost — hard `MaxPerFaction`/`MaxPerPlayer` limit instead of component gating (Outpost intentionally allowed higher than Base) | — | — |

Air and Wheeled costs should sit **cheaper overall** than the equivalent Naval tier at the same component quantity — both are small-grid-only and far more likely to be lost/destroyed in normal play than a ship, so the same nominal "low amount" of Military Components should represent a smaller real cost for a plane or tank than for a destroyer.

**Production/tool blocks:** Basic tier uses vanilla components; Advanced tier uses Industrial Components. Upgrade modules for assemblers/refineries potentially also cost Industrial Components — spam risk here is already covered by `BlockLimits`, not something to solve separately.

**Practical Stage 1 scope:** only the vanilla-cost starting tier plus the Destroyer proof-of-concept (low Military Components) actually need to exist for Stage 1's playable milestone. Everything else in the table above — Cruiser and up, Attacker and up, Light Tank and up — is real, confirmed design, but belongs with Stage 5's deeper roster.

### Salvage loop

Non-hostile, pre-damaged versions of existing hulls (a hulked Francesco Crispi, a stripped Golo) as static, explorable salvage sites — existing prefabs with `IsPirate:false` and some blocks pre-removed/damaged. Should cover both military-themed wrecks (Francesco Crispi, Le Triomphant) and civilian-themed wrecks (Golo) to match the military/civilian loot-sourcing split above, not just one or the other. Towed or ground down at a Base/Outpost, feeding the unlock economy. No new grids required, so this belongs in Stage 1 rather than waiting on Stage 4.

### Do NPCs need Ship Cores?

**Working answer: no.** Ship Core Framework's `MaxPerFaction`/`MaxPerPlayer` design is oriented around limiting player-built grids — an MES-spawned NPC ship isn't "progressing," it's pre-built encounter content, same as today. This matches the broader ecosystem convention (Block Restrictions explicitly differentiates `AllowedForNPC`/`AllowedForPlayer`/`AllowedForUnowned`) of exempting NPC ownership from player-progression systems by default. Not fully settled, though — found a real, unresolved forum comment from someone hitting this exact ambiguity in practice with a different but related core mod ("tried spawning ships with the core[s] on them there was no level set to them"). Treat as a real to-do, not an assumption. (Note: your own correction — SDX2 runs MES underneath, with AI Enabled/Crew Enabled as supplementary systems rather than a replacement — makes SDX2 more relevant evidence here than originally credited, not less, since it's a real MES-based server coexisting with a core-progression mod.)

**To-do:**
- [ ] Once Ship Core Framework is actually integrated, test spawning an NPC ship with no core at all — confirm it isn't rejected, capped, or otherwise affected before assuming NPCs can stay core-free..
- [ ] Add type-appropriate ammo to NPC cargo loot — naval gun ammo on ships, aircraft gun ammo on planes — extending the existing `WW2-Loot-*` profiles per ship class. Blocked on confirmed ammo magazine SubtypeIds from Fletcher Armaments/Consty's Ordnance (separate from the weapon *block* IDs already gathered — ammo magazine IDs are typically distinct strings, need their own lookup pass once the weapon rebuild settles).

**To-do:**
- [ ] Add Industrial/Military/Advanced Military Components to existing `WW2-Loot-*` container profiles at tuned drop frequencies, per the military/civilian sourcing split confirmed above.
- [ ] Sub-classify the existing `WeaponBlocks` `BlockGroup` split (Guns/Bombs/Torpedoes/Smoke) into basic vs. advanced, to support vanilla-vs-Military-Component weapon costing.
- [ ] Test subgrid pitch-down behavior on an actual plane incrementally from 150 up toward 250 m/s before committing to the world speed increase.
- [ ] Write `ShipCoreConfig_World.xml` (confirm `MaxPossibleSpeedMetersPerSecond` — 150 or 250 pending the physics test above — plus `MassTypeMode`, `FrictionSpeedValueMode`).
- [ ] Write `BlockGroup` definitions: `ProductionBlocks`, `CargoBlocks`, `ToolBlocks` (can start now, vanilla types) and `WeaponBlocks` (blocked on weapon rebuild).
- [ ] Add Ship Core Framework as a mod dependency to the world/mod list.
- [ ] Smoke test before committing further: write one minimal `ShipCore` definition (no component cost, just a block limit or two) and place it on a throwaway test grid. Confirm the block limit actually enforces, `/core` commands respond as documented, and the correct `MaxPossibleSpeedMetersPerSecond` value is taking effect — before writing any of the real definitions below on top of an unverified foundation.
- [ ] Write the eight Stage 1 `ShipCore` XML definitions (Civilian Naval, Corvette Naval, Cargo Plane, Fighter, Transport, Armored Car, Base, Outpost) — vanilla-component cost.
- [ ] Write the Destroyer `ShipCore` definition — low Military Components cost — as the Stage 1 proof-of-concept unlock tier.
- [ ] Design and build the Corvette → Destroyer unlock trigger chain as the proof-of-concept for the whole gating system.
- [ ] Spawn-condition a small number of salvageable wreck variants of existing hulls, covering both military and civilian spawn themes.
- [ ] Playtest: start on vanilla-cost everything, fight/salvage using existing assets, confirm the Destroyer unlock actually fires once enough Military Components are banked.

---

## Stage 2 — Squadron & Reputation Depth

No new grids required. Two patterns adapted from Ares at War's `_FAC` shared framework:

- **Squadron wingman tracking:** each plane in a flight broadcasts its own death via a command code; survivors track losses via boolean flags and break off once their flight is gone. Slots into existing `Behaviors-Fighter.sbc`.
- **Layered reputation:** replace the flat ±1 `ReputationDamage` trigger with conditional logic (e.g., destroying something a faction also considers hostile improves standing with them, via a counter threshold) — richer Condition Profiles on existing triggers, no new content.

**To-do:**
- [ ] Group existing fighters into 2–3 plane flights; add squadron death-tracking triggers.
- [ ] Design conditional reputation rules per faction (what counts as "an enemy of my enemy" for Gray vs. Green).
- [ ] Playtest: confirm squadrons visibly thin out/disengage, confirm reputation responds to *who* you fight.

---

## Stage 3 — Capturable Territory

No new grids required. Adapt AaW's Capturable/CapturableController pattern to one installation type (Ammo Depot is simplest) — on capture: recolor grid, flip block ownership, disable old owner's supply triggers, enable new owner's. Hooks into existing `WW2-Manipulation-AmmoDepots` and faction-override plumbing. This first-pass version doesn't need the custom planet — one installation, no spacing or terrain requirements.

**To-do:**
- [ ] Build capture trigger chain for Ammo Depot.
- [ ] Wire captured territory in as a third feed into the Stage 1 unlock currency (confirmed design — see Stage 1).
- [ ] Playtest: confirm a captured depot actively works for its new owner.

### Stage 3 expansion — region-based territory growth (confirmed design, sequenced after the Custom Planet track)

A fuller version, worth real design attention now even though it builds later. Confirmed pieces:

- **Neutral installations exist.** Refineries and similar can be unthemed — owned by neither faction by default, matching the "Nobody" faction pattern already confirmed to exist in Ares at War's own structure. Reputation-gated purchases at these (buying ice/hydrogen with sufficient faction standing) is a direct reuse of the same mechanism already confirmed for Advanced Military Components in Stage 1 — same lever, applied to a new resource.
- **Cargo delivery tracking is fully buildable now, no scripting needed.** The exact hook already exists: `WW2-Command-CargoPlane-DestinationReached` and `WW2-Command-CargoShip-DestinationReached` (the same destination-arrival commands fixed for a SubtypeId collision earlier in this project) fire exactly when a cargo vehicle reaches its drop-off point. Attach a CustomCounter-increment Action to that command, weighted differently for ships vs. planes (e.g. +20 per cargo ship delivery, less per plane), and the tracking side is just config on an existing trigger.
- **Region-based ownership, not true geometric expansion.** A genuinely growing circular zone would need MES to compute which installations currently fall inside a changing radius — not confirmed to exist as a feature. The buildable equivalent: group installations into a small number of hand-placed regions ahead of time, give each region its own per-faction CustomCounter pair, and flip every installation in a region together once one faction's counter crosses a threshold (e.g. Gray reaches 100 in a region, that region's installations flip to Gray). Overlapping-territory "shrinks the other side" becomes two counters racing per region, not literal geometry.
- **Players fighting both factions stop being an edge case.** Since ownership is driven by relative delivery volume between factions rather than by direct player capture, a player hostile to both sides just slows both factions' growth rather than creating a logical contradiction about who's capturing what.
- **Confirmed dependency: sequence after the Custom Planet track.** Hand-placed regions need reliable spacing, flat buildable terrain, and sensible connectivity between locations — none of which is guaranteed on a planet that isn't under the mod's own control. This is the first case in this roadmap where a parallel track becomes a real prerequisite rather than staying non-blocking; worth remembering if this pattern comes up again.
- **Static, invulnerable installations fit MES's existing tools.** Hand-placed static encounters with invulnerability set are a normal MES capability, not something new to build.

**To-do (blocked on Custom Planet completing):**
- [ ] Design the region layout against the finished planet — count, spacing, terrain suitability.
- [ ] Build the CustomCounter-increment Actions on the existing cargo destination-reached commands.
- [ ] Build the per-region threshold-check and ownership-flip logic, reusing the single-installation capture mechanics above.
- [ ] Decide neutral-installation scope: which installation types (refineries confirmed, others TBD) stay unthemed and reputation-gated rather than faction-owned from the start.

### Stage 3 expansion — buyable/stealable grids, three location tiers, and War Level (confirmed design)

Built on the same cargo-delivery counter as the region-growth system above — one tracked value, two consumers, not two systems.

**Three location tiers, each a different point on the permanent/capturable/dynamic spectrum:**

- **Domain anchors** — one Airport and one Port per faction (four total: Gray Airport, Gray Port, Green Airport, Green Port). Permanent, faction-fixed forever, never capturable, matching the routing-endpoint anchor point from the original territory design — these aren't a new location type, they're that same anchor given a second job. Airport and Port stay deliberately separate, not combined into one mixed hub. Ground vehicles are available at both, since Ground doesn't get its own dedicated anchor (not ruled out forever, just not yet — the Ground roster is the newest and thinnest of the three domains right now). Each anchor sells its faction's complete current catalog, gated by War Level (below) rather than by regional stock.
- **Maintenance yards** — new tier, sitting between the anchors and the hangars/garages: permanent position (doesn't relocate or despawn the way a normal dynamic encounter would), but capturable via the territory system above, with inventory reflecting whoever currently holds it.
- **Hangars/garages** — the existing prefab tier, functioning as regular dynamic MES spawns, outside the territory system entirely.

**Steal-or-buy, same location, two paths to the same grid.** Anywhere a player can steal a vehicle, they can also buy that same vehicle — friendly/reputable players take the peaceful route, everyone else takes the risk-it route, both ending at the same actual hull. Applies at the hangar/garage tier specifically, where stealing already lives.

**War Level — a second consumer of the cargo-delivery counter, not a second tracking system.** The same CustomCounter mechanism built for territory-region thresholds also drives a per-faction War Level value. Two confirmed effects:
1. **Domain anchor stock** — higher War Level unlocks better hulls/materials in that faction's complete catalog. This is the more legible of the two signals to a player — not "spawn tables quietly shifted," but "the shelf at the anchor visibly got better."
2. **Spawn escalation** — bigger vessels or larger squadrons become available to spawn at higher War Level, layering onto the existing `ThreatScoreMinimum` spawn-gating pattern (the same mechanism already tuned once before, for the carrier's threat threshold) rather than requiring new spawn-gating infrastructure.

**Restocking has real, working precedent already shipped in this mod — it's not a new problem.** The existing Factory-Plane installations already do a persistent structure periodically spawning new grids nearby (`OffenseSpawn-Factory-Plane`, `DeliverySpawn-Factory-Plane`). Maintenance yards need the same capability aimed at ground vehicles instead of aircraft. Ownership-change restocking (when a yard flips faction) doesn't need separate infrastructure either — it's one more action added to the same capture trigger chain already designed above (recolor, flip ownership, swap supply triggers, **now also: reroll stock**).

**One real, unverified gap in the restocking plan: precise spawning inside a tight interior bay.** Factory-Plane's precedent is proven for aircraft, which tolerate minor spawn-point imprecision by simply flying away; a ground vehicle spawned slightly off inside a garage bay can clip through walls or get stuck. Recommended sequencing: prove the restocking mechanic on **open platforms in an outdoor supply yard first** (no tight geometry to clip through), and only attempt precise interior-bay spawning as a stretch goal once the basic mechanic is confirmed reliable — building the harder version first risks discovering it doesn't work after already committing to it.

**One open question, deliberately not chased down yet, held per your own instinct:** whether hostility toward a specific player can be dynamically gated by that player's own threat score or prior aggression, independent of whether bigger NPCs simply exist in the world at a given War Level. This is what would let a slow/non-combat player see the war escalate around them without personally being targeted by it. Threat score is confirmed to exist and gate spawning; whether it (or reputation/counter logic) can additionally gate an NPC's *initial aggression state toward a specific player* dynamically, versus only being checked at spawn time, is unverified — worth an empirical check before this becomes load-bearing, same treatment as the P.108 gun question.

**SDX2's mission system — flagged as interesting, not yet enough information to design against.** No specifics have been described yet beyond "really cool"; revisit once there's something concrete to evaluate against MES's actual Event system rather than designing around a description that isn't there yet.

**To-do (blocked on the region-growth to-dos above, plus Custom Planet):**
- [ ] Build/place the four domain anchors (Gray/Green × Airport/Port).
- [ ] Design the "complete catalog" concept — which hulls are sellable at all, and how War Level gates the list.
- [ ] Build cored/sellable versions of existing hangar/garage prefabs (a cored variant alongside the stealable one).
- [ ] Wire buy-capability into existing hangar/garage locations alongside the existing steal mechanic.
- [ ] Design and place maintenance yards as a new prefab tier, distinct from both anchors and hangars.
- [ ] Prototype restocking on an open-air supply yard before attempting interior-bay spawning.
- [ ] Add "reroll stock" as an action in the capture trigger chain for maintenance yards.
- [ ] Verify whether per-player dynamic hostility gating (threat score/reputation-based) is actually achievable in MES, before committing the "big stuff exists but isn't aggressive unless provoked" design to it.

---

## Stage 4 — First New Grids: Basic-Tier Starters

New grid design finally becomes necessary. Deliberately small:
- Naval: one small patrol boat per faction, sized to Corvette Core.
- Wheeled: one Armored Car per faction — this is also where land combat gets its first real content instead of just installations.

Air can wait — the existing Fighter roster already covers the starting tier.

**To-do:**
- [ ] Design Gray Armored Car.
- [ ] Design Green Armored Car.
- [ ] Design Gray naval patrol boat (Corvette-tier).
- [ ] Design Green naval patrol boat (Corvette-tier).
- [ ] Wire all four into SpawnGroups/Behaviors/Loot per existing pipeline.

---

## Stage 5 — Elite Tier, Escalation, and Polish

By now the loop is proven end to end. Roughly in order of effort:

- **Rest of each ladder:** Destroyer → Cruiser → Heavy Cruiser → Battleship → Carrier (Naval); Attacker/Bomber → Heavy Bomber (Air); Light Tank → Medium Tank → Heavy Tank (Wheeled). **Correction, confirmed via research:** Trento, Zara, and Algérie are all officially classed Heavy Cruisers (Washington Treaty tonnage), not plain Cruisers — Emile Bertin alone is a genuine light Cruiser. Corrected roster: **Cruiser tier — Bartolomeo Colleoni** (Gray, Giussano-class, famously lost at the Battle of Cape Spada) and **Emile Bertin** (Green). **Heavy Cruiser tier — Trento and Zara** (Gray) and **Algérie** (Green, confirmed France's only heavy cruiser under treaty limits — no sister ship, an honest asymmetry to keep rather than fix, same shape as the Italian heavy-tank gap). **Battleship — Strasbourg** (Green, Dunkerque-class; existing blueprint from an established builder, to be adapted) and **Vittorio Veneto** (Gray, Littorio-class; a real, purchasable 1/1800 reference model exists on MyMiniFactory, and the class is well-represented across Sketchfab/CGTrader/physical scale kits too — picked over sister ships Littorio and Roma for having the broadest active-service record: present at both Taranto and Cape Matapan, rather than fame tied to a single event). Note: Algerie and Emile Bertin both carry the AWG piston/hinge catapult-floatplane assembly responsible for the grid-loading bug (see Stage prefab checklist) — factor the rebuild into their timelines, not just Emile Bertin's rebuild-list entry.

### Air roster — Attacker/Bomber and Heavy Bomber tiers (confirmed)

**Gray (Italy):**
- **Attacker/Bomber tier — Breda Ba.88 and Savoia-Marchetti SM.79.** SM.79 ("il Gobbo Maledetto," the damned hunchback) is one of the most famous Italian aircraft of the war, especially in its torpedo-bomber ("Silurante") role — a dedicated torpedo variant is planned specifically to threaten player ships. Ba.88 fills the lighter/entry role — genuinely real and well-documented, but its actual historical reputation is "notoriously one of WW2's worst operational aircraft," which fits an entry-tier plane thematically rather well rather than being a downside.
- **Heavy Bomber tier — Piaggio P.108.** Italy's only operational four-engine heavy bomber. Existing impressive model already built, needs rebuild onto the new weapon systems.

**Green (France):**
- **Attacker/Bomber tier — Lioré et Olivier LeO 451.** Widely regarded as the best French bomber of the war — the closest French equivalent to SM.79 in national significance, even though it arrived too late and in too few numbers to matter much before the armistice. Optional third pick for full symmetry with Gray's entry-tier: **Bloch MB.210** (older, more obsolescent by 1940, same "weaker starting bomber" role Ba.88 fills for Gray) — not decided, just flagged as available if wanted later.
- **Heavy Bomber tier — Farman F.222.** Confirmed via research to be far more significant than expected: explicitly classified as a heavy bomber, "the only four-engined bomber in front line service with any Allied air force" at the start of the Battle of France, flew 63 night-bombing sorties over Germany in May–June 1940, and a related variant flew the first-ever Allied bombing raid on Berlin. Civilian use was the historical *secondary* role, not primary — the reverse of how the existing asset is currently used (Cargo Plane tier). Strong case for reusing/varianting the existing F.222 build into a military-spec version rather than starting from scratch — build approach (shared base hull vs. two separate builds) still an open call.

**Reference material found for new builds** (BA.88, SM.79, LeO 451 all appear to come from the same modeler's consistent "Historic Aircraft (1914–1974)" series on CGTrader — worth building from one consistent source rather than mixing styles):
- SM.79: multiple free/paid 3D models on Sketchfab and CGTrader, including several explicitly modeled as the torpedo-bomber variant.
- Ba.88: 3D model on CGTrader (same series as above), plus a dedicated multi-view blueprint on drawingdatabase.com.
- LeO 451: 3D model on CGTrader (same series), three-view drawings also findable via general image search.
- P.108: no new search needed — existing asset already covers this.


- **Upgrade Modules:** convert core-swap progression to modules-on-top-of-one-core where it improves the feel (per the confirmed schema support above).
- **Retaliation/escalation** (from AaW): tiered "strike back" raids against captured territory, cooldown-limited. Reuses Stage 3's capture plumbing.
- **Fleet/Escort formalization** (from AaW's EscortSystem): generalize the existing `CarrierSpawn` escort pattern into something reusable across ship classes.
- **Dynamic territory framing** (from GVK S10): even a coarse "who holds more captured installations" counter feeding spawn frequency/difficulty gets most of the value of GVK's full alliance-strength system.
- **Narrative flavor:** ScenarioTools for MES Events (Workshop 2998575759) for NewsFeed broadcasts and dynamic GPS markers, no custom C# required.
- **Economy stations** (AaW + GVK both): superseded by the fuller domain-anchor/maintenance-yard/hangar design confirmed in Stage 3's expansion — see there for the current version rather than this placeholder.

---

## Parallel track — Custom Planet

Doesn't block or get blocked by the numbered stages themselves — new terrain doesn't need Stage 4's new hulls to exist, and Stage 4's hulls don't need new terrain either. Pick this up whenever, including alongside the weapon rebuild, since it's a genuinely different skill (terrain/image work, not XML/grid design) and switching between them may be a welcome break rather than a distraction. **One real dependency exists, though:** Stage 3's region-based territory expansion (see Stage 3) needs this track finished first — hand-placed capture regions need reliable spacing and terrain that only a planet under the mod's own control can guarantee. First case in this roadmap where a parallel track became a genuine prerequisite; the original Stage 3 single-installation capture doesn't share this dependency.

**What a custom planet actually is:** a `PlanetGeneratorDefinition` (.sbc) plus a heightmap (6 tiled grayscale textures forming the terrain) plus a biome map (6 more tiled textures, RGB-channel-encoded: RED = ground material, GREEN = foliage, BLUE = ore placement). Ports and convoy routes aren't part of the planet file itself — those are just where you place your existing installation prefabs and patrol GPS points once terrain exists, which is work you already know how to do. The new skill is specifically coastline/terrain shaping.

**Critical constraint, not optional:** heightmap/biome map resolution must stay at or below 2048x2048. Going higher breaks raycasts — confirmed to cause exactly the failure modes that would hit this mod specifically: WeaponCore weapons shooting through voxels, and MES-driven NPCs crashing more often. Don't chase extra detail past 2k.

**Recommended path, validated by GVK doing exactly this:** don't build from scratch. Either start from `EarthLikeModExample` (Keen's own learning sandbox) or copy an existing planet — GVK's own Kharak planet is literally "vanilla Pertam, highly modified" with smooth roads and traversable terrain, not an original heightmap. A real-world coastline pulled from **terrain.party** (real Earth heightmaps, up to 60km) is also a strong option specifically for a Mediterranean-theater WW2 mod — solves natural coastlines and smooth convoy routes in one step without needing terrain-generation skill. GVK's planet is 60km, not the full 120km — a reasonable size target rather than defaulting to the largest option.

**To-do:**
- [ ] Install World Machine (or plan to hand-paint in GIMP/Photoshop) and a text editor for `.sbc` work (Notepad++ or similar).
- [ ] Decide: modify an existing planet (Pertam or similar) vs. pull a real coastline from terrain.party vs. fully custom heightmap. Pick the lowest-effort option that gets a usable coastline — this isn't a place to prove terrain-art skills.
- [ ] Load `EarthLikeModExample` and read through its `PlanetGeneratorDefinitions.sbc` entry to understand the system hands-on before editing anything real.
- [ ] Source or build the heightmap (≤2048x2048, all 6 faces, seams matching).
- [ ] Build the biome map: RED channel (ground materials — sand/grass/rock placement for a coastal Mediterranean feel), GREEN channel (foliage), BLUE channel (ore placement, tied to `OreMappings`).
- [ ] Write the `PlanetGeneratorDefinition` — `ComplexMaterials`, `OreMappings`, `EnvironmentItems`, atmosphere/gravity settings.
- [ ] Spawn the planet in a test world (Shift+F10 admin spawn) and sanity-check coastlines, ore placement, and — importantly — that WeaponCore/MES don't exhibit raycast issues.
- [ ] Once terrain is solid: place port locations (existing Base/Outpost/Hangar prefabs) and plan convoy GPS routes using the existing SpawnGroup/patrol pipeline.

---

## Parallel track — Map Tool (deferred decision)

Not scoped yet on purpose — see the ROI discussion above. Revisit once Stage 3/5's capturable territory is live and actually contested; a map has nothing dynamic to show before then. When that point arrives, work through the three tiers (existing Workshop map script → custom LCD/GPS script → bespoke external tool) in that order, stopping at whichever tier actually earns its cost.

---

## Long-term vision

**Sub-factions.** Not scoped yet, named so early architecture doesn't box it out. Splitting Gray/Green into historical sub-factions (Germans/Italians under Gray; French/UK/US/Soviets under Green, or some other split) matches AaW's structure of many factions under fewer broad alliances. Practical implication for everything above: avoid hardcoding "GRAY"/"GREEN" any more than necessary, prefer patterns that generalize to a new faction being copy-and-retarget rather than a rewrite.

**First wave, confirmed: Germany (Gray) and UK (Green).** UK over US for the first wave — Britain was Italy's primary Mediterranean opponent for nearly the entire period the existing roster already centers on (Taranto, Cape Matapan, Malta convoys, the early North African campaign all predate US Mediterranean involvement, which didn't begin until Operation Torch in November 1942). UK also brings more genuinely new content to players, since it's less commonly built in the existing SE Workshop ecosystem than US equipment — the tradeoff being fewer existing builds to reference/borrow from, which is a real cost but a manageable one given the mod's existing pattern of building from scratch (Fiat 626, Renault). US remains a strong later addition, natural to pair with a Sicily/Italy-invasion-era expansion rather than this first wave.

**More ore/ingot variety, SDX2-inspired.** SDX2 doesn't add a resource layer beneath their components — it just adds more raw ores and ingots alongside vanilla's existing set (lead, copper, etc., sitting next to iron, silicon, cobalt). So the actual tradeoff here is simpler than "add a new economic tier": it's whether refining/component-building recipes get more varied and specific, or stay on vanilla's existing ore set. Still a real accessibility-vs-depth call — more ore types means more mining/refining variety but also more for a new player to track — but it's additive breadth, not structural complexity. Parked for the same reason as the custom planet and map tool: not needed to prove Stage 1's core loop, easy to add later by defining new ore/ingot pairs and wiring them into whichever recipes should use them, without needing to touch the three-component system's own structure at all.

---

## Why this order

The weapon rebuild has to happen first, full stop — nothing else is testable on top of a dead weapon mod. After that, Stages 1–3 are entirely trigger/XML work on the (now-rebuilt) existing roster and get a genuinely different-feeling mod — full core progression across ships/planes/tanks/bases, squadron behavior, reputation, capturable territory, salvage economy — before a single new hull needs designing. Stage 4 is deliberately small so the payoff of "new grids" gets tested cheaply before Stage 5 asks for more of them. Each stage's playable milestone stands on its own.

The two parallel tracks (Custom Planet, Map Tool) mostly sit outside this sequence — they don't block the stages and the stages don't block them, with one exception: Stage 3's fuller region-based territory expansion now waits on the Custom Planet track, since it needs terrain the mod actually controls. Pick up the planet work whenever the XML/grid work needs a break; leave the map tool alone until there's live territory worth mapping.
