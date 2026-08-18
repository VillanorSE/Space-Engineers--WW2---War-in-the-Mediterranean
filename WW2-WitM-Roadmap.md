# WW2 Encounters — Expansion Roadmap & To-Do List

A staged plan for growing the mod from its current state (Naval + Air + Installations, Gray/Green factions) into a fuller PvPvE experience with Ship-Core-driven progression, borrowing proven systems from Ares at War, GV Deserts of Kharak Season 10, and MES Shared Behaviors (MSB — enenra's public MES behavior library). Ordered so small, achievable, playable milestones come first, and grid-design-heavy work comes only once the systems around it are proven. Custom C# scripting is in scope for this project where it's the right tool, but deliberately staged in after simpler XML-only versions are built and playable — see Stage 3's territory section and Stage 5 for where scripting is first introduced.

---

## Prefab naming convention (confirmed)

Two parallel prefab families per hull, distinguished by purpose rather than just faction/class:

- **`Player-WW2-[name]`** — configured for player use (includes landing gear). Used at hangars, garages, factories, and stores — the buyable/stealable grid tier described in Stage 3's expansion.
- **`NPC-WW2-[name]`** — used for NPC-controlled encounters, matching the existing convention already reflected throughout the weapon-rebuild checklist below.

---

## Prerequisite — Weapon System Rebuild

Not a stage so much as a foundation everything else sits on. The current naval weapon mod is effectively dead, so every existing ship and plane needs rebuilding onto a new weapon system before Ship Core work can be meaningfully tested — the whole premise of Stage 1 is gating access to ships you already have, and that doesn't hold if those ships don't load.

- **Naval weapons:** [Fletcher Armaments – WeaponCore Edition](https://steamcommunity.com/sharedfiles/filedetails/?id=2844434226) (Workshop 2844434226) — WeaponCore-based WW2 naval weapon pack.
- **Air weapons:** [Consty Aircraft Pack – Ordnance (WeaponCore) 1.0](https://steamcommunity.com/sharedfiles/filedetails/?id=2881339118) by Const (Workshop 2881339118), companion to Const's Tech-Focused Aircraft Pack. Confirmed via its changelog to mix genuinely period-appropriate content (.50 cal guns, 30mm aircraft cannon, unguided bombs, rocket pods) with clearly modern/anachronistic content (AIM-7/54/120 guided missiles, 5.56mm burst rifles, 9mm SMGs, .45 handguns). The lockdown plan is sound and necessary, not just thematic tidiness — gate/exclude the modern-coded weapons, keep the period-coded ones, using the same tech-item mechanism as Stage 1's unlock currency.
- **Ground vehicle weapons:** [KONTAKT Ground Systems [WeaponCore] v1.0](https://steamcommunity.com/sharedfiles/filedetails/?id=2930049835) Mostly fixed weapons. Same identification and BlockGroup-building process as Fletcher Armaments/Consty's Ordnance once you're ready to pull its block SubtypeIds.
- **CWP replacement candidates**
  - **SETB Community Tank Parts** — replace all existing armor blocks, enabling the ground vehicle rebuild off AWG CWP armor specifically.
  - **ArmourEssentials** — Mostly turret ring rotors, gunsight cameras, and rangefinders.
  - **Yakobe's Machinations** — wheels/suspension sourced from CWP, plus additional guns.
  - **SETB - Multicrew - MODPACK** — the full SETB bundle. Not intended for wholesale adoption, but worth keeping as a reference source.
- **Turret philosophy shift:** moving away from custom rotor/hinge-built turrets toward general WeaponCore turret blocks. This should make future `BlockGroup`/`BlockLimit` definitions for the Core system far more stable, since they'll reference known WeaponCore block types instead of a dead mod's custom subtypes.
- **Dependency additions:** WeaponCore itself, plus whichever specific weapon-pack mods end up used. Worth checking MES's documented WeaponCore compatibility notes once rebuild work starts, since combining MES/RivalAI-driven NPCs with WeaponCore weapons is a well-trodden combination but has its own configuration quirks (weapon targeting profiles, ammo replenishment behavior).

**To-do:**
- [x] Pull KONTAKT Ground Systems' block SubtypeIds and TypeId the same way Consty's Ordnance was handled.
- [x] Confirm whether SETB Community Tank Parts includes large grid rotors — determines if it's a full or partial AWG CWP replacement.
- [x] Cross-check Yakobe's Machinations' gun roster against KONTAKT's for overlapping/colliding SubtypeIds before using both.
- [ ] Decide final ground vehicle scale factors within the confirmed 110–120% (military) / up to 150% (civilian) ranges, and rescale the existing Fiat 626 and Renault builds if the civilian figure ends up above 100%.
- [ ] Pull the full Consty Ordnance block list and sort into "period, keep" vs. "modern, gate or exclude" — AIM-7/54/120 and the modern small arms are confirmed non-period from the changelog alone, but the full list needs a proper pass.
- [ ] Rebuild each existing hull's weapon fit onto Fletcher Armaments / Consty's Ordnance, replacing custom rotor/hinge turret rigs with general WeaponCore turrets. Broken out per-prefab below so each is a single sitting's work — check the box when that prefab's weapons are rebuilt and it loads clean with the old mod removed. Naval uses Fletcher Armaments; Air uses Consty's Ordnance (WW2-appropriate blocks only, per the lockdown scope above); Installations may need either or neither depending on what defenses they actually carry.

  **Naval:**
  - [x] NPC-WW2-Golo_Italian (Cargo Ship, Gray)
  - [x] NPC-WW2-Golo_French (Cargo Ship, Green)
  - [x] NPC-WW2-Gabbiano (Corvette, Gray)
  - [x] NPC-WW2-Spica (Torpedo Boat, Gray)
  - [x] NPC-WW2-Francesco_Crispi (Destroyer, Gray)
  - [x] NPC-WW2-Comandante_Margottini (Destroyer, Gray)
  - [x] NPC-WW2-La_Malouine (Corvette, Green)
  - [x] NPC-WW2-Bougainville (Aviso/Destroyer-behavior, Green)
  - [x] NPC-WW2-Le_Triomphant (Destroyer, Green)
  - [x] NPC-WW2-Algerie (Cruiser, Green)
  - [x] NPC-WW2-Emile_Bertin (Cruiser, Green) — **confirmed root cause found:** the AWG piston/hinge-based catapult+floatplane assembly (Loire 130) silently breaks grid loading — no exception, no error, just fails to register as an entity. Reproduced and fixed by surgical removal in a build-world test; same mechanism confirmed present on Algerie (Green Heavy Cruiser) and absent on Trento (Gray Heavy Cruiser, which has never shown the failure — clean independent confirmation). Rebuild the catapult using non-AWG piston/hinge parts (or the general WeaponCore turret approach already planned for weapons) before this hull is usable in a SpawnGroup.
  - [x] NPC-WW2-Aquila (Carrier, Gray) - Fix tower and cranes once KONTAKT mod is fixed.
  - [x] NPC-WW2-Bearn (Carrier, Green)
  - [ ] Rebuild Loire 130 on the Emile Bertin and Algier and re-export/replace.

  **Air:**
  - [x] NPC-WW2-Re2001 (Fighter, Gray) - Replace conveyors, engines, glass, and side MG covers once KONTAKT is fixed.
  - [ ] NPC-WW2-Re2000 (Fighter, Gray)
  - [x] NPC-WW2-FC20 (Attacker, Gray)
  - [ ] NPC-WW2-Loire130 (Recon, Green)
  - [ ] NPC-WW2-F4F (Fighter, Green)
  - [x] NPC-WW2-MS406 (Fighter, Green)
  - [ ] NPC-WW2-Potez630 (Attacker, Green)
  - [x] NPC-WW2-V-156-F-Bomber (Attacker, Green)
  - [x] NPC-WW2-V-156-F-Torpedo (Attacker, Green)
  - [ ] NPC-WW2-Ju52 (Cargo Plane, Gray)
  - [ ] NPC-WW2-F222 (Cargo Plane, Green)
  - [ ] NPC-WW2-C47 — **not a rebuild task.** Already orphaned/unused, superseded by F222. Decide retire-vs-rebuild first; don't spend a session rebuilding a hull you've already said you don't want.

  **Installations (architecture superseded, confirmed):** the old one-hangar/one-factory-per-plane-type model is retired. No more rebuilding a separate Hangar or Factory prefab for every plane variant that gets added — one general Hangar, one general Factory, and one general Garage per faction instead, with MES handling which vehicle variant spawns inside via SpawnGroup selection rather than the installation itself being hardcoded to one plane. This is the direct generalization of the Factory rework architecture (one factory shell per faction per class, spawning independent small-grid planes into fixed interior points, proven by the existing `OffenseSpawn-Factory-Plane`/`DeliverySpawn-Factory-Plane` mechanism) taken one step further: one shell per faction *per installation type*, not per class either.

  Prefab names below follow the naming convention above and are inferred from the existing pattern, not yet confirmed against actual file names — flag if these don't match:
  - [ ] NPC-WW2-Ammo-Depot-1
  - [ ] NPC-WW2-Hangar-Gray
  - [ ] NPC-WW2-Hangar-Green
  - [ ] NPC-WW2-Factory-Plane-Gray
  - [x] NPC-WW2-Factory-Plane-Green
  - [ ] NPC-WW2-Garage-Gray
  - [ ] NPC-WW2-Garage-Green
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

### Naval roster (confirmed)

| Tier | Gray (Italy) | Green (France) |
|---|---|---|
| **Civilian** | Golo (Italian variant) | Golo (French variant) |
| **Corvette** | Gabbiano | La Malouine |
| **Destroyer** | Francesco Crispi, Comandante Margottini, Spica (Torpedo Boat) | Bougainville (Aviso/Destroyer-behavior), Le Triomphant |
| **Cruiser** | Bartolomeo Colleoni (Giussano-class, famously lost at the Battle of Cape Spada) | Emile Bertin (France's only genuine light Cruiser under treaty classification) |
| **Heavy Cruiser** | Trento, Zara | Algérie (confirmed France's only heavy cruiser under treaty limits — no sister ship, an honest asymmetry to keep rather than fix, same shape as the Italian heavy-tank gap) |
| **Battleship** | Vittorio Veneto (Littorio-class — picked over sister ships Littorio and Roma for the broadest active-service record: present at both Taranto and Cape Matapan, rather than fame tied to a single event) | Strasbourg (Dunkerque-class; existing blueprint from an established builder, to be adapted) |
| **Carrier** | Aquila | Bearn |

**Correction on record (confirmed via research):** Trento, Zara, and Algérie are all officially classed Heavy Cruisers (Washington Treaty tonnage), not plain Cruisers — Emile Bertin alone is a genuine light Cruiser, and Bartolomeo Colleoni fills the Gray Cruiser-tier slot that opened up as a result.

**Spica placement confirmed:** Spica (Torpedo Boat, Gray) is now placed in the Destroyer tier alongside Francesco Crispi and Comandante Margottini — resolves the earlier open item about it not mapping cleanly onto the ladder.

**Known build issue affecting two hulls:** Algérie and Emile Bertin both carry the AWG piston/hinge catapult-floatplane assembly responsible for the grid-loading bug identified during the mod audit (root cause confirmed on Emile Bertin, same mechanism present on Algérie, absent on Trento). Factor the catapult rebuild into both hulls' timelines.

---

### Naval roster — Submarines (confirmed, new category outside the Civilian–Carrier ladder)

| Type | Gray (Italy) | Green (France) |
|---|---|---|
| **Standard submarine** | Adua class (17 built, 1936–38; Regia Marina's numerically most important coastal submarine class of the interwar/war period, part of the broader "600 Series" family) | *(no equivalent identified this session)* |
| **Cruiser submarine** | *(no equivalent — Italy did not build cruiser submarines)* | Surcouf (largest submarine in the world at the time; twin 203mm/8-inch cruiser-caliber guns plus torpedoes; carried its own reconnaissance floatplane, a Besson MB.411, in a hangar — same shipborne-floatplane concept as the new Air Recon tier, submarine-mounted) |

**Direct link to existing content:** several Adua-class boats — most notably *Scirè* — served as mother submarines for the Decima Flottiglia MAS "Maiale" human torpedoes (see the Alexandria raid, Naval roster context above). Not a coincidence being forced here — it's a genuine historical connection between two additions confirmed in the same research pass.

**Not yet placed in the mod's tier/progression structure.** Submarines (standard or cruiser type) don't fit the existing Civilian→Corvette→Destroyer→Cruiser→Heavy Cruiser→Battleship→Carrier ladder, and neither does Decima Flottiglia MAS as a special-forces/raider concept. Roster-confirmed here as real, verified historical content; how (or whether) they slot into Ship Core tiers, spawn behavior, or a wholly separate mechanic is a mechanics-thread decision, not addressed here.

---

### Air roster (confirmed)

| Tier | Gray (Italy) | Green (France) |
|---|---|---|
| **Civilian** | Ju52 *(open item)* | F.222 (civilian variant) |
| **Fighter** | Re2001, Re2000; Macchi C.202 Folgore | F4F, MS406; Dewoitine D.520 |
| **Attacker** | FC20; Ba.88, Ba.65 *(Stage 5 additions)* | Potez630; Bréguet 693, ANF Les Mureaux 115 *(Stage 5 additions)* |
| **Bomber** | Caproni Ca.311 *(light)*; SM.79 *(medium/torpedo)*; P.108 *(heavy)* | V-156-F *(light dive bomber, reclassified from Attacker)*; MB.210, LeO 451 *(medium)*; F.222 (military variant, heavy) |
| **Recon** | IMAM Ro.43 | Loire 130 |

**Attacker tier, three per faction (confirmed).** FC20 (Gray) and Potez630 (Green) are the existing built assets. Ba.88 and Ba.65 round out Gray's tier — Ba.65 confirmed as Italy's only dedicated ground-attack aircraft to see active service in that role, lightly armed but kept for thematic and historical accuracy. Bréguet 693 and ANF Les Mureaux 115 round out Green's tier — the 693 a genuine twin-engine ground-attack counterpart to Ba.88/Ba.65, and the Mureaux 115 an older, more obsolescent reconnaissance/light-attack hybrid (119 built, 1935–38) that fills the same "outclassed early-war plane" role Ba.65 fills for Gray, right down to both being real but underwhelming in actual combat use. The Mureaux's most notable historical distinction is being the first French aircraft shot down by the Luftwaffe, in September 1939.

**Bomber tier corrections (confirmed).** V-156-F reclassified from Attacker to Bomber — confirmed as the French export version of the Vought SB2U Vindicator dive-bomber, ordered for carrier air groups, not an attack aircraft. MB.210 confirmed as a medium bomber, not attacker — twin-engine, ~257 built, entered service 1936. Caproni Ca.311 added as Gray's light-bomber entry — 335 built, twin-engine light bomber/reconnaissance hybrid, entered service 1939; notably it also replaced Ba.65 in some ground-attack roles historically, a mild irony worth knowing but not a real conflict since the two filled different jobs (recon-bomber vs. ground-attack).

**Macchi C.202 Folgore and Dewoitine D.520 added to Fighter tier (confirmed, prior session).** Both are stronger historical picks than the existing entries in their tiers — C.202 widely regarded as Italy's best fighter of the war, D.520 the only French fighter able to meet the Bf 109E on roughly equal terms. Neither has an existing build; both are next up in the build queue.

**Recon tier (confirmed, prior session).** IMAM Ro.43 (Gray) and Loire 130 (Green), both real catapult-launched shipborne reconnaissance floatplanes. Mechanics deferred to a separate thread.

**Open item: Gray Civilian-tier air.** Ju52 is German-built, not Italian — unresolved, revisit later.

**SM.79** ("il Gobbo Maledetto," the damned hunchback) is one of the most famous Italian aircraft of the war, especially in its torpedo-bomber ("Silurante") role — a dedicated torpedo variant is planned specifically to threaten player ships. **Ba.88** fills the lighter attack role — genuinely real and well-documented, but its actual historical reputation is "notoriously one of WW2's worst operational aircraft," which fits an entry-tier plane thematically rather well.

**P.108** is Italy's only operational four-engine heavy bomber. Existing impressive model already built, needs rebuild onto the new weapon systems.

**LeO 451** is widely regarded as the best French bomber of the war — the closest French equivalent to SM.79 in national significance, even though it arrived too late and in too few numbers to matter much before the armistice. **MB.210** fills the entry-tier attack role — older, more obsolescent by 1940, the same "weaker starting" role Ba.88 fills for Gray.

**F.222** is confirmed via research to be far more significant than expected: explicitly classified as a heavy bomber, "the only four-engined bomber in front line service with any Allied air force" at the start of the Battle of France, flew 63 night-bombing sorties over Germany in May–June 1940, and a related variant flew the first-ever Allied bombing raid on Berlin. Civilian use was the historical *secondary* role, not primary — the reverse of how the existing asset is currently used (Civilian tier). **Build approach confirmed: the existing F.222 build will be modified into the military-spec Bomber-tier version rather than built from scratch.**

**Civilian tier scope, confirmed as open-ended:** the Civilian tier isn't locked to historical trucks alone. Non-historical utility vehicles players will want — mining, welding, grinding rigs, possibly articulated-arm designs — are anticipated future additions to this tier, not ruled out by the historical framing used for the rest of the roster. Not scoped yet; revisit when utility-vehicle design work actually starts.

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

## Stage 2 — Squadron, Reputation, and Morale Depth

No new grids required. Design sourced from a three-way comparison of Ares at War (AaW), MES Shared Behaviors (MSB — enenra's public MES behavior library, github.com/enenra/mes-shared-behaviors), and GVK Deserts of Kharak S10, reconciled below. Confirmed decisions, not open questions.

**Squadron/escort tracking: MSB's `CommandChain` system, adopted wholesale.** AaW's own squadron pattern was considered first (each plane in a fixed numbered flight — Squadron1/2/3 — broadcasts its own death via a command code, survivors watch boolean flags for their specific wingmen and break off once all others are down) and works, but it's hand-authored per fixed slot count, not generic. MSB's `CommandChain` does the same job — leader/escort join-up, renaming, and death notification via `CommandCode` broadcast (`MSB_LeaderDead`) and boolean state (`LeaderInactive`) — but uses RivalAI's native `AssignEscortFromCommand` for slot assignment, so it scales to however many escorts actually spawn rather than needing a separately-authored trigger/condition pair per numbered slot. **Decision: use MSB's mechanism as the underlying system; flight size of 2–3 planes remains a content choice (how many fighters get spawned into a given encounter), not a constraint baked into the tracking logic itself.**
- Add `MSB_System_CommandChain_Leader_TriggerGroup` to flight leaders, `MSB_System_CommandChain_Escort_TriggerGroup` to wingmen, per MSB's usage notes.
- Both require one of `MSB_DynamicCommon_TriggerGroup`/`MSB_StaticCommon_TriggerGroup` present on the encounter first, per MSB's base requirements.

**Layered reputation: binary contribution-threshold gate, not tiered credit.** Sourced from AaW's `_FAC` REPSystem (`FAC-Context-REPAHE.sbc` and per-faction siblings): a hostile unit's death (`Type:Compromised`) checks `CheckCustomCounters:CountPlayerDamage >= 15` before awarding reputation with the beneficiary faction (i.e., killing something a faction also considers hostile improves standing with them — "enemy of my enemy"), radius-shared so nearby faction-mates of the killer get credit too. MSB's `_DynamicCommon.sbc` separately tracks three finer-grained kill-credit outcomes (`Compromised_PlayerHelped` / `Compromised_PlayerMadeFinalShot` / `Compromised_PlayerNotHelped`) which could in principle tier the reward. **Decision: don't tier it.** Two players who both clear the damage-contribution minimum should get equal full credit — a partial-credit tier risks one contributor feeling shortchanged relative to the other for what reads, in the moment, as equivalent effort. Use AaW's binary threshold shape; MSB's finer states aren't needed for this.
- Replace the flat ±1 `ReputationDamage` trigger with `CheckCustomCounters:CountPlayerDamage` gating a `ChangeReputationWithPlayers` action on relevant hostile-to-Gray/hostile-to-Green kills.
- Radius-share the reputation gain to nearby faction members of the credited player, matching AaW's `ReputationChangesForAllRadiusPlayerFactionMembers`.

**Morale: adopted, in scope for this stage.** Sourced from MSB's `System_Morale.sbc` (marked WIP/unfinished upstream, but functional): a decaying `CustomCounter` (starts ~100, ticks down under sustained combat, drops sharply on low health / weapons lost / leader death / nearby losses) that swaps `TargetProfile` at low-morale thresholds — a shaken unit doesn't hard-retreat, it just stops proactively hunting at range (detection range effectively shrinks). Entirely pure-XML, no scripting, self-contained — doesn't gate or get gated by anything else in this stage. Pairs conceptually with the already-built (separately tracked, not part of this roadmap item) plane disable/spiral-crash mechanic — low morale plus a real chance of visibly losing control, rather than either alone, is the intended combined effect; that mechanic will be refined and expanded on its own timeline, noted here only for context.

**To-do:**
- [ ] Add MSB's `_Common` TriggerGroup (Dynamic or Static as appropriate) as a base requirement to fighter behaviors before layering CommandChain on top.
- [ ] Wire `MSB_System_CommandChain_Leader_TriggerGroup` / `_Escort_TriggerGroup` into existing fighter flights; confirm flights still read as 2–3 plane groups in practice even though the mechanism itself is N-agnostic.
- [ ] Build the `CountPlayerDamage`-gated reputation actions for Gray/Green "enemy of my enemy" kills, radius-shared to nearby faction members.
- [ ] Add MSB's `MoraleSystem_TriggerGroup` to fighter (and eventually other) behaviors; tune decay/threshold values against actual playtests rather than assuming MSB's defaults fit WW2 Encounters' pacing.
- [ ] Playtest: confirm squadrons visibly thin out/disengage as CommandChain reports losses, confirm reputation responds to *who* you fight, confirm morale-driven target-profile shrinkage is noticeable without being confusing.

---

## Stage 3 — Capturable Territory

No new grids required, and no Custom Planet dependency for this base version (see the corrected territory-growth mechanism below — the earlier assumption that true radius-based expansion needed hand-placed regions on mod-controlled terrain turned out to be wrong; see next section). Adapt AaW's Capturable/CapturableController pattern to one installation type (Ammo Depot is simplest) — on capture: recolor grid, flip block ownership, disable old owner's supply triggers, enable new owner's. Hooks into existing `WW2-Manipulation-AmmoDepots` and faction-override plumbing. Confirmed via AaW's actual files: ownership state is a per-installation `SandboxBoolean` (`{Faction}{SpawnGroupName}`) re-checked at world load (`Type:Session` trigger) and re-applied via `RecolorGrid` + `ChangeBlockOwnership` — this is the pattern to replicate.

**General "stay out" zones: MSB's `AreaRestriction` system, adopted broadly — not scoped to capturable installations only.** Sourced from MSB's `System_AreaRestriction.sbc`: pre-built radius tiers (100/1000/2500/5000m) that warn a neutral/hostile player on entry, then apply periodic reputation loss (`-25` every 10s in MSB's default) for as long as they linger, via a `PlayerNeutral`-gated `Manual` trigger pair (in-range/out-of-range) plus a repeating `StillInRange` timer. Functionally similar to AaW's separate `CapturableController` loiter-penalty layer, but general-purpose rather than capture-specific. **Decision: use this generically** — around capturable installations (discouraging enemy loitering pre-capture), around non-capturable but sensitive locations (a permanent naval base that should never be taken but should still be respected), and specifically to discourage players from building bases too close to non-friendly NPC installations. Treat it as a reusable tool to reach for anywhere "don't hang around here uninvited" is the desired signal, not a one-off built just for capture.

**To-do:**
- [ ] Build capture trigger chain for Ammo Depot, using AaW's session-load boolean-check pattern.
- [ ] Add MSB's `AreaRestriction` TriggerGroup (radius tier TBD per installation type) to the Ammo Depot and to at least one non-capturable installation, to prove the generic use case alongside the capture-specific one.
- [ ] Wire captured territory in as a third feed into the Stage 1 unlock currency (confirmed design — see Stage 1).
- [ ] Playtest: confirm a captured depot actively works for its new owner, and confirm the AreaRestriction warning/reputation-loss cadence feels like a nudge rather than a punishment.

### Stage 3 expansion — territory growth, ownership model, and War Level (confirmed design, corrected and expanded from a prior version)

**Correction to a prior assumption in this roadmap:** an earlier version of this section assumed true radius-based territory expansion "would need MES to compute which installations currently fall inside a changing radius — not confirmed to exist as a feature," and designed around that limitation using hand-placed regions sequenced after the Custom Planet track. Reviewing GVK Deserts of Kharak S10's actual files disproves that assumption. GVK implements exactly this, in pure MES/RivalAI XML, no scripting or custom terrain required:

- Each faction has one real zone (`{Faction}_Zone`), pre-defined at several nested radii (GVK uses 10000/20000/30000/40000/50000/55000m as an example scale — WW2 Encounters' own radii TBD, and now additionally constrained by the confirmed 60km-diameter (30km-radius) planet — see the Custom Planet track), only one `Active:true` at a time.
- A `CustomSandboxCounter` (GVK: `{Faction}_Points`) tracks standing.
- A `Timer`-type trigger checks that counter every ~10s against per-radius-tier thresholds and fires a `ChangeZoneAtPosition` action with `ZoneRadiusChangeType:Set` and `ZoneToggleActiveMode` to resize/toggle the live zone directly, broadcasting a status chat line. Both growth and shrinkage are native — each radius tier has a paired Enable and Disable condition (`GreaterOrEqual` / `Less` against the same threshold), so territory can contract as well as expand as the counter moves.

**Decision: adopt GVK's mechanism directly as the core territory-growth engine, no Custom Planet dependency.** This removes the sequencing block the prior version of this roadmap placed on this whole section — it can be built as soon as Stage 3's basic capture mechanics are proven, independent of terrain work.

**Points/counter sourcing:** reuse GVK's pattern of encounter-type-weighted point values (their scale: Base 50, Outpost 10, Cargo Ship 10, Elite Drone 15, Large/Medium/Small Drone 4/2/1 — WW2 Encounters' own values TBD) rather than the flat cargo-delivery-only counter this section previously proposed. Damaging one faction's asset should be able to simultaneously credit the opposing faction, matching GVK's paired increase/decrease actions.

**Ownership model, now explicitly split into two categories (new decision, not previously specified):**
1. **Dynamically-spawning encounters** (regular MES ship/plane/vehicle spawns) always belong to their design-time faction — a Gray spawn stays Gray. What territory controls for these is *where* they're allowed to spawn: gated to only spawn inside that faction's current territory zone via `ZoneConditions`, same mechanism GVK uses for its salvage-quality-by-distance zones.
2. **Fixed-location installations** (domain anchors, maintenance yards, other pinned-coordinate structures — see the buyable/stealable section below) are the ones that actually change hands. Ownership flips based on which faction currently controls the territory that location sits in, using the AaW-style session-load boolean-check/recolor/reflip pattern already confirmed for single-installation capture above.

**Neutral installations:** the prior version of this section justified unthemed neutral installations (refineries, etc.) by citing "the 'Nobody' faction pattern already confirmed to exist in Ares at War's own structure." That citation doesn't hold up — checking AaW's actual files, `Nobody` is just a folder name organizing wreck/salvage prefabs, not a real in-game faction or a neutral-installation-with-reputation-gated-purchases pattern. The underlying design idea (unthemed refineries, reputation-gated buying) is still sound and buildable — MES/RivalAI factions and reputation don't require AaW precedent to work this way — it just isn't precedented by AaW the way previously claimed. Reputation-gated purchases at neutral installations reuse the same lever already confirmed for Advanced Military Components in Stage 1.

**War Level — a separate tracked value from territory radius, not the same counter reused.** Rises over time based on ongoing faction activity (exact feed TBD — likely a slower-accumulating derivative of the same kinds of events that feed territory Points, but tracked independently so radius and War Level don't move in lockstep). Two confirmed effects, both about spawn *composition* scaling, not just frequency:
1. **Fixed-installation functionality/activity level** — a domain anchor or maintenance yard's stock, services, or active defenses can scale with War Level, layered on top of whichever faction currently owns it via the territory-ownership flip above.
2. **Dynamic spawn escalation** — composition, not just difficulty, scales with War Level. Confirmed example: air cargo convoys spawn as a single unescorted plane at low War Level; at higher War Level the same convoy type can include fighter escorts, additional defensive call-ins, and/or more cargo planes per spawn. This generalizes to other dynamic spawn types (naval convoys, patrol composition) as a design pattern, not just the cargo-plane case — layers onto the existing `ThreatScoreMinimum` spawn-gating pattern already used for the carrier's threat threshold, rather than requiring new spawn-gating infrastructure.

**Future upgrade path, deliberately deferred (Stage 5+): Ares at War's scripted faction-strength model.** AaW's actual implementation (`Factions.cs`) is real custom C# — a per-faction `Strength_Counter` and `Aggression_Counter`, with a `Holdings` list that sums captured installations' production into strength and gates a `ReadyForExpansion` flag. This is a genuinely richer aggregate ("how tough is this faction right now, based on everything it currently holds") than a flat points counter, but it requires scripting to sum live state across many grids — not something static XML triggers do well. **Decision: stay on the GVK-style flat counter for now (fully within Stage 3's XML-only scope); treat AaW's scripted Strength/Holdings model as a defined Stage 5+ upgrade path** once the simpler version is built and proven, at which point scripting becomes a reasonable investment rather than a Stage 3 blocker. This keeps Stage 3's playable milestone reachable without any C#, while leaving a clear, named next step for when more depth is wanted.

**Players fighting both factions stop being an edge case,** same reasoning as before: since territory ownership is driven by relative faction standing rather than by direct player capture, a player hostile to both sides just slows both factions' growth rather than creating a logical contradiction about who's capturing what.

**To-do:**
- [ ] Decide WW2 Encounters' own radius tiers and point thresholds (GVK's specific numbers are a starting reference, not a requirement; must also fit within the confirmed 60km-diameter/30km-radius planet — GVK's own outer tier (55km radius) will not fit as-is and needs scaling down).
- [ ] Build the nested `{Faction}_Zone` radius definitions and their paired Enable/Disable timer-trigger-condition sets, per GVK's pattern.
- [ ] Build the `CustomSandboxCounter` point-award/deduction actions per encounter type (adapt GVK's weighted scale to WW2 Encounters' own roster).
- [ ] Add `ZoneConditions` gating to existing dynamic SpawnConditions so Gray/Green spawns are restricted to their own current territory.
- [ ] Build the fixed-installation ownership-flip logic (reuses the single-installation capture mechanics above, applied per-location based on which territory zone currently contains it).
- [ ] Design the War Level counter's feed and decide whether it's derived from the same events as territory Points or tracked from a distinct set.
- [ ] Build the convoy-escalation proof-of-concept: single-plane air cargo convoy at low War Level, escorted/multi-plane version unlocked at a higher tier — smallest concrete test of the spawn-composition-scaling idea before generalizing it further.
- [ ] Decide neutral-installation scope: which installation types (refineries likely, others TBD) stay unthemed and reputation-gated rather than faction-owned from the start.
- [ ] Playtest: confirm territory visibly grows and shrinks with faction fortunes, confirm dynamic spawns respect territory boundaries, confirm the War Level convoy example reads as a meaningful escalation rather than a stat tweak.

### Stage 3 expansion — buyable/stealable grids and three location tiers (confirmed design)

Built on the territory and War Level system defined above — no separate tracking system, no Custom Planet dependency (that dependency is removed along with the region-based approach it was tied to).

**Three location tiers, each a different point on the permanent/capturable/dynamic spectrum — and each maps directly onto the two-category ownership model above:**

- **Domain anchors** — one Airport and one Port per faction (four total: Gray Airport, Gray Port, Green Airport, Green Port). Permanent, faction-fixed forever, never capturable — these are fixed-location installations whose *ownership* never changes, though their functionality/stock still scales with War Level per the mechanism above. Airport and Port stay deliberately separate, not combined into one mixed hub. Ground vehicles are available at both, since Ground doesn't get its own dedicated anchor (not ruled out forever, just not yet — the Ground roster is the newest and thinnest of the three domains right now). Each anchor sells its faction's complete current catalog, gated by War Level rather than by regional stock.

  **Port anchor design, confirmed:** not intended as historically accurate reconstructions of real places — real installations are used as loose inspiration only, for layout logic and proportion, not as a build target to replicate. **Gray Port is loosely inspired by La Spezia** (Italy's largest naval dockyard, construction + fitting-out role); **Green Port is loosely inspired by Toulon** (France's main Mediterranean naval arsenal, zoned basin layout — repair basin / submarine basin / fleet mooring / stores). **Design philosophy, confirmed: full (1:1) scale buildings and mooring/construction structures, but far fewer of them than the historical installations had.** No excavated dry dock — each Port anchor gets one **docking spot** (a pier/bulkhead for mooring alongside, surface-level, not a caisson-gated graving dock) and one **construction slip**, rather than the five-plus dry docks a real yard like La Spezia or Toulon's multi-basin arsenal had. **Confirmed footprint targets**, sized to comfortably fit the largest current hull (Vittorio Veneto, 237.76m × 32.82m) with realistic clearance: **~275m × ~45m for the docking pier/bulkhead, ~285m × ~50m for the construction slip.** The goal is an installation that reads as genuinely full-scale next to a 1:1 Aquila or Vittorio Veneto, not a compressed miniature — the compression happens in *count* of repeated facilities, not in the scale of any individual structure.

  **Terrain fitting, confirmed technique:** MES's voxel-spawning capability will be used to carve/scoop the correct berth/slip shapes and level the surrounding ground so the Port anchor's pier, construction slip, and quay structures sit correctly into planet terrain, rather than requiring hand-sculpted terrain to already exist at the anchor's fixed coordinates before the installation is placed.

- **Maintenance yards** — new tier, sitting between the anchors and the hangars/garages: a fixed-location installation, permanent position, but capturable via the territory-ownership-flip mechanism above, with inventory reflecting whoever currently holds it.
- **Hangars/garages/factories** — one general prefab per faction per installation type (not per plane/vehicle variant, see the Prerequisite section's Installations note), functioning as regular dynamic MES spawns: these follow the *dynamic-encounter* half of the ownership model — they stay with their design-time faction and are gated to spawn only within that faction's current territory, rather than changing hands themselves. Which specific vehicle variant appears inside is an MES SpawnGroup selection made at spawn time, not baked into the installation prefab itself.

  **Factory production-line build states (confirmed, Fighter tier):** a Factory's interior spawn points fill with sub-spawns representing a production line rather than uniformly finished hulls. Fighter-tier factories use 6 slots: 2 fully built (100%, the untouched blueprint, no manipulation needed), 2 at a randomized 50–75% build state, 2 at a randomized 25–50% build state, using MES's `ReduceBlockBuildStates` manipulation (confirmed to only affect non-essential blocks like armor/glass, not functional blocks) via two reusable Manipulation Profiles (`WW2-Manipulation-BuildState-Stage2`, `WW2-Manipulation-BuildState-Stage1`) applied uniformly across each affected hull rather than to a random subset of its blocks. Applied identically across every Fighter variant rather than authored per-plane. Attacker tier (3–4 slots, exact count and percentage scheme TBD pending checking previous builds for what physically fits) needs its own profile(s) once confirmed.

  Each Factory instance randomly commits to one class per spawn cycle — a 6-slot Fighter production line (all 6 the same randomly-selected Fighter variant) or a 3–4-slot Attacker line (all slots the same randomly-selected Attacker variant) — never mixed within one factory instance.

**Steal-or-buy, same location, two paths to the same grid.** Anywhere a player can steal a vehicle, they can also buy that same vehicle — friendly/reputable players take the peaceful route, everyone else takes the risk-it route, both ending at the same actual hull. Applies at the hangar/garage tier specifically, where stealing already lives.

**War Level effects specific to this section** (the general War Level mechanism and its two confirmed effect categories are defined in the territory-growth section above; this is the domain-anchor-specific application of effect #1, fixed-installation functionality/activity scaling):
- **Domain anchor stock** — higher War Level unlocks better hulls/materials in that faction's complete catalog. This is the more legible of War Level's signals to a player — not "spawn tables quietly shifted," but "the shelf at the anchor visibly got better."

**Restocking has real, working precedent already shipped in this mod — it's not a new problem.** The general per-faction Factory installations already use a persistent structure periodically spawning new grids nearby (`OffenseSpawn-Factory-Plane`, `DeliverySpawn-Factory-Plane`), now generalized to spawn whichever plane variant a SpawnGroup selects rather than one hardcoded plane per Factory prefab. Maintenance yards need the same capability aimed at ground vehicles instead of aircraft. Ownership-change restocking (when a yard flips faction) doesn't need separate infrastructure either — it's one more action added to the same capture trigger chain already designed above (recolor, flip ownership, swap supply triggers, **now also: reroll stock**).

**One real, unverified gap in the restocking plan: precise spawning inside a tight interior bay.** Factory-Plane's precedent is proven for aircraft, which tolerate minor spawn-point imprecision by simply flying away; a ground vehicle spawned slightly off inside a garage bay can clip through walls or get stuck. Recommended sequencing: prove the restocking mechanic on **open platforms in an outdoor supply yard first** (no tight geometry to clip through), and only attempt precise interior-bay spawning as a stretch goal once the basic mechanic is confirmed reliable — building the harder version first risks discovering it doesn't work after already committing to it.

**One open question, deliberately not chased down yet, held per your own instinct:** whether hostility toward a specific player can be dynamically gated by that player's own threat score or prior aggression, independent of whether bigger NPCs simply exist in the world at a given War Level. This is what would let a slow/non-combat player see the war escalate around them without personally being targeted by it. Threat score is confirmed to exist and gate spawning; whether it (or reputation/counter logic) can additionally gate an NPC's *initial aggression state toward a specific player* dynamically, versus only being checked at spawn time, is unverified — worth an empirical check before this becomes load-bearing, same treatment as the P.108 gun question.

**SDX2's mission system — no longer pursuable as a reference.** SDX2's GitHub isn't public and no MES-related files could be located on the Steam Workshop (likely unlisted). Flagged as interesting in an earlier session but there's nothing concrete to evaluate against MES's actual Event system, and no further avenue to get one — dropped as a reference source rather than left open.

**To-do (blocked on the territory-growth to-dos above; Custom Planet dependency removed):**
- [ ] Build/place the four domain anchors (Gray/Green × Airport/Port).
- [ ] Design the "complete catalog" concept — which hulls are sellable at all, and how War Level gates the list.
- [ ] Build cored/sellable versions of the general per-faction hangar/garage prefabs (a cored variant alongside the stealable one).
- [ ] Wire buy-capability into existing hangar/garage locations alongside the existing steal mechanic.
- [ ] Design and place maintenance yards as a new prefab tier, distinct from both anchors and hangars.
- [ ] Prototype restocking on an open-air supply yard before attempting interior-bay spawning.
- [ ] Add "reroll stock" as an action in the capture trigger chain for maintenance yards.
- [ ] Verify whether per-player dynamic hostility gating (threat score/reputation-based) is actually achievable in MES, before committing the "big stuff exists but isn't aggressive unless provoked" design to it.
- [x] Docking pier/bulkhead and construction slip footprint confirmed for the Gray and Green Port anchors — no excavated dry dock; ~275m × ~45m for the docking pier/bulkhead, ~285m × ~50m for the construction slip, sized to comfortably fit the largest hull in the current Naval roster (Vittorio Veneto, 237.76m × 32.82m) with realistic clearance.

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
- **Fleet/Escort formalization:** generalize the existing `CarrierSpawn` escort pattern into something reusable across ship classes. Now that Stage 2 adopts MSB's `CommandChain` for squadron/escort tracking (see Stage 2 — this superseded the originally-planned AaW `EscortSystem` port, since MSB's version is already N-agnostic rather than needing generalization from a fixed-slot pattern), this item is really "apply the existing CommandChain mechanism to carriers and their escorts," not a new system.
- **Faction Strength/Holdings scripting** (from AaW, deferred from Stage 3): the scripted upgrade path already named in Stage 3's territory section — aggregate captured-installation production into a real per-faction Strength value once the flat GVK-style counter has been proven. This is the natural point to introduce custom scripting into the project, staged deliberately after Stage 3's simpler XML-only version is playable.
- **Narrative flavor:** ScenarioTools for MES Events (Workshop 2998575759) for NewsFeed broadcasts and dynamic GPS markers, no custom C# required.
- **Economy stations** (AaW + GVK both): superseded by the fuller domain-anchor/maintenance-yard/hangar design confirmed in Stage 3's expansion — see there for the current version rather than this placeholder.

---

## Parallel track — Custom Planet

Doesn't block or get blocked by the numbered stages themselves — new terrain doesn't need Stage 4's new hulls to exist, and Stage 4's hulls don't need new terrain either. Pick this up whenever, including alongside the weapon rebuild, since it's a genuinely different skill (terrain/image work, not XML/grid design) and switching between them may be a welcome break rather than a distraction.

**Correction to a prior version of this roadmap:** this section previously claimed Stage 3's territory expansion had a hard dependency on the Custom Planet track, since the design at the time relied on hand-placed capture regions needing reliable spacing and terrain under the mod's own control. That design has since been replaced — GVK Deserts of Kharak S10's actual files show true radius-based territory growth is achievable in pure MES/RivalAI XML via `ChangeZoneAtPosition`, with no terrain requirements at all (see Stage 3). The Custom Planet track is once again fully non-blocking, same as every other parallel track — pick it up purely on its own merits (a Mediterranean coastline is still a strong fit for this mod's setting), not because anything else is waiting on it.

**Planet size: confirmed at 60km diameter (30km radius) — the same size as GVK's own Kharak planet, not just using it as a size reference.** Verified via two independent sources: GVK's own community guide states Kharak's terrain is "based on the vanilla planet Pertam, which has been highly modified with smooth roads, higher peaks, traversable canyons, and fun slopes" — no resizing mentioned, so it inherits Pertam's native diameter — and the official Space Engineers wiki's planet-size table lists Pertam at 60.00km diameter (vs. 120km for EarthLike/Mars/Alien, 19km for moons). GVK's own Zone 3 boundary ("52km+" from their Crossroads hub, per their server guide) is consistent with this: that's a great-circle surface distance, and the maximum possible surface distance on a 30km-radius sphere is roughly 94km (half the circumference), so a 52km zone edge fits comfortably without implying a larger planet. This directly constrains Stage 3's territory-zone radius tiers (GVK's own example radii go out to a 55km-radius outer zone, which does not fit a 30km-radius planet — see the to-do flagged there) and gives a fixed frame for placing the four domain anchors and any maintenance yards along the coastline.

**What a custom planet actually is:** a `PlanetGeneratorDefinition` (.sbc) plus a heightmap (6 tiled grayscale textures forming the terrain) plus a biome map (6 more tiled textures, RGB-channel-encoded: RED = ground material, GREEN = foliage, BLUE = ore placement). Ports and convoy routes aren't part of the planet file itself — those are just where you place your existing installation prefabs and patrol GPS points once terrain exists, which is work you already know how to do. The new skill is specifically coastline/terrain shaping.

**Critical constraint, not optional:** heightmap/biome map resolution must stay at or below 2048x2048. Going higher breaks raycasts — confirmed to cause exactly the failure modes that would hit this mod specifically: WeaponCore weapons shooting through voxels, and MES-driven NPCs crashing more often. Don't chase extra detail past 2k.

**Recommended path, validated by GVK doing exactly this:** don't build from scratch. Either start from `EarthLikeModExample` (Keen's own learning sandbox) or copy an existing planet — GVK's own Kharak planet is literally "vanilla Pertam, highly modified" with smooth roads and traversable terrain, not an original heightmap. A real-world coastline pulled from **terrain.party** (real Earth heightmaps, up to 60km) is also a strong option specifically for a Mediterranean-theater WW2 mod — solves natural coastlines and smooth convoy routes in one step without needing terrain-generation skill. GVK's planet is 60km (Pertam's own native diameter, not a scaled-down choice GVK made — see above), not the full 120km EarthLike/Mars/Alien default — and now this project's own confirmed size too, matching GVK exactly rather than just referencing it.

**To-do:**
- [ ] Install World Machine (or plan to hand-paint in GIMP/Photoshop) and a text editor for `.sbc` work (Notepad++ or similar).
- [ ] Decide: modify an existing planet (Pertam or similar) vs. pull a real coastline from terrain.party vs. fully custom heightmap. Pick the lowest-effort option that gets a usable coastline — this isn't a place to prove terrain-art skills.
- [ ] Load `EarthLikeModExample` and read through its `PlanetGeneratorDefinitions.sbc` entry to understand the system hands-on before editing anything real.
- [ ] Source or build the heightmap (≤2048x2048, all 6 faces, seams matching, sized for the confirmed 60km-diameter planet).
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

The two parallel tracks (Custom Planet, Map Tool) sit fully outside this sequence — they don't block the stages and the stages don't block them. An earlier version of this roadmap had Stage 3's fuller territory expansion waiting on the Custom Planet track; that dependency is gone now that GVK's radius-growth mechanism (see Stage 3) proved terrain control was never actually required. Pick up the planet work whenever the XML/grid work needs a break; leave the map tool alone until there's live territory worth mapping.
