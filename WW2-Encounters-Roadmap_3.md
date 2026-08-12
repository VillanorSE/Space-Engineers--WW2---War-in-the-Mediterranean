# WW2 Encounters — Expansion Roadmap & To-Do List

A staged plan for growing the mod from its current state (Naval + Air + Installations, Gray/Green factions) into a fuller PvPvE experience with Ship-Core-driven progression, borrowing proven systems from Ares at War and GV Deserts of Kharak Season 10. Ordered so small, achievable, playable milestones come first, and grid-design-heavy work comes only once the systems around it are proven.

---

## Prerequisite — Weapon System Rebuild

Not a stage so much as a foundation everything else sits on. The current naval weapon mod is effectively dead, so every existing ship and plane needs rebuilding onto a new weapon system before Ship Core work can be meaningfully tested — the whole premise of Stage 1 is gating access to ships you already have, and that doesn't hold if those ships don't load.

- **Naval weapons:** [Fletcher Armaments – WeaponCore Edition](https://steamcommunity.com/sharedfiles/filedetails/?id=2844434226) (Workshop 2844434226) — WeaponCore-based WW2 naval weapon pack.
- **Air weapons:** [Consty Aircraft Pack – Ordnance (WeaponCore) 1.0](https://steamcommunity.com/sharedfiles/filedetails/?id=2881339118) by Const (Workshop 2881339118), companion to Const's Tech-Focused Aircraft Pack. Confirmed via its changelog to mix genuinely period-appropriate content (.50 cal guns, 30mm aircraft cannon, unguided bombs, rocket pods) with clearly modern/anachronistic content (AIM-7/54/120 guided missiles, 5.56mm burst rifles, 9mm SMGs, .45 handguns). The lockdown plan is sound and necessary, not just thematic tidiness — gate/exclude the modern-coded weapons, keep the period-coded ones, using the same tech-item mechanism as Stage 1's unlock currency.
- **Turret philosophy shift:** moving away from custom rotor/hinge-built turrets toward general WeaponCore turret blocks. This should make future `BlockGroup`/`BlockLimit` definitions for the Core system far more stable, since they'll reference known WeaponCore block types instead of a dead mod's custom subtypes.
- **Dependency additions:** WeaponCore itself, plus whichever specific weapon-pack mods end up used. Worth checking MES's documented WeaponCore compatibility notes once rebuild work starts, since combining MES/RivalAI-driven NPCs with WeaponCore weapons is a well-trodden combination but has its own configuration quirks (weapon targeting profiles, ammo replenishment behavior).

**To-do:**
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
  - [ ] NPC-WW2-Emile_Bertin (Cruiser, Green) — **confirmed root cause found:** the AWG piston/hinge-based catapult+floatplane assembly (Loire 130) silently breaks grid loading — no exception, no error, just fails to register as an entity. Reproduced and fixed by surgical removal in a build-world test; same mechanism confirmed present on Algerie (Green Cruiser) and absent on Trento (Gray Cruiser, which has never shown the failure — clean independent confirmation). Rebuild the catapult using non-AWG piston/hinge parts (or the general WeaponCore turret approach already planned for weapons) before this hull is usable in a SpawnGroup.
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

### Mechanical design (confirmed against the actual Ship Core Framework v3 schema)

- `MobilityType`: `Mobile` for vehicle cores, `Static` for Base/Outpost. One enum field, not two booleans.
- **Civilian vs. Military differentiation:** two levers —
  1. `Modifiers` — Military cores get `RefineSpeed`/`RefineEfficiency`/`AssemblerSpeed` reduced (~0.5) even where a few production blocks are allowed; Civilian cores stay at baseline or above.
  2. `BlockLimits` — cap weapon block counts low on Civilian cores, cap production/cargo block counts low on Military cores, via `BlockGroup` definitions. Blocked on the weapon rebuild above for the weapon-side groups; production/cargo/tool-side groups can be written now since they're vanilla block types.
- **Speed/boost:** `MaxSpeed`/`MaxBoost` are fractions of the world's `MaxPossibleSpeedMetersPerSecond` (confirmed set to 150 m/s). Boost is duration/cooldown-limited only — the framework has no native fuel-drain boost mode. Naval "flank speed" will use the same duration/cooldown mechanism as air/wheeled for now (tuned with a longer duration to read as "sustained" rather than "burst"); a true fuel-cost mechanic is a possible custom-trigger addition later if the simple version doesn't feel right.
- **Upgrade path (confirmed, deferred to later):** the "upgrade a Corvette core to allow more guns/propellers" idea maps directly onto `UpgradeModule.BlockLimitModifiers` — a real, first-class feature of the framework, not something to build custom. Stage 1 doesn't need this yet; Stage 1 is core-swap only (place a better core block outright). Converting to upgrade-modules-on-top-of-one-core is a clean later refinement once tier numbers are proven, not a Stage 1 requirement.

### Unlock economy

**Confirmed: territory/salvage-based, mostly salvage to start.** Fed by what a spawn *is* — military or civilian-themed — not by whether it was killed or found already wrecked. Every spawn type should exist in both an active (fight it) and wrecked (salvage it) form where that makes sense, but the loot-sourcing split runs along the military/civilian axis, not the alive/dead one:

1. **Military spawns** (active or wrecked) — combat ships, planes, weapon emplacements. Primary source of Military and Advanced Military Components.
2. **Civilian spawns** (active or wrecked) — cargo ships, transports, civilian installations. Primary source of Industrial Components.

Both routes reward exploring and engaging with the world rather than a pure kill-counter. Territory control (Stage 3) becomes a further feed once captured installations exist — not part of Stage 1, since territory doesn't exist yet.

**Confirmed component names, structure, and sourcing rules.** Three materials, not one generic currency:

- **Industrial Components** — gates Advanced Civilian production blocks. Sourced from: advanced production blocks (a player can *manufacture* these, not just find them), trade-location purchases, and loot — weighted heavily toward civilian-themed spawns, present in smaller amounts on military-themed spawns too.
- **Military Components** — gates first-tier combat hulls at low quantity. Sourced from: non-basic weapon blocks (advanced production output), trade-location purchases, and loot — weighted heavily toward military-themed spawns, present in smaller amounts on civilian-themed spawns too.
- **Advanced Military Components** — gates top-tier combat hulls. Sourced from: advanced weapons (advanced production output), trade-location purchases (**gated behind good faction standing**, not just currency), and loot — at very low quantity on spawn classes that wouldn't themselves need Advanced Military Components to build as a player core, and at meaningfully higher quantity on spawn classes that would. This ties loot value directly to the same tier scale as build cost, rather than being a separate system that happens to coexist with it.

**Basic/starting-tier everything uses plain vanilla components, no specialized item at all** — the unlock materials only enter the picture once a player is unlocking *beyond* the starting rung. This applies to weapons too: **basic weapons cost vanilla components, non-basic/advanced weapons cost Military Components** (or Advanced Military Components at the top end) — the same tiering logic already applied to hulls, applied consistently to weapon blocks as well. Practical implication for the weapon rebuild: the `WeaponBlocks` BlockGroup splits already done (Guns/Bombs/Torpedoes/Smoke) will need a further basic/advanced sub-classification once real costing starts — flagged as a to-do, not resolved yet.

**Trade locations** (Stage 5's planned economy stations) will eventually sell Industrial and Military Components directly, with Advanced Military Components specifically locked behind good faction standing — the first concrete gameplay payoff for Stage 2's reputation system, which otherwise is purely behavioral/flavor. Reputation stops being just "how NPCs act toward you" and becomes something that gates real economic access.

**Fleet-size limiting: fixed cost + finite income, not escalating price.** A core costs a flat amount of a genuinely scarce component; since the component is earned at a tuned (slow) rate, a player's standing fleet is naturally capped by how much they've banked and haven't spent — no special mechanic needed for this, it's just a normal crafting cost against a limited income rate. An escalating-cost variant (each *additional* core of a type costing more, based on how many are currently built and standing — not lifetime built, resets on loss) is a genuinely different and harder feature, since it requires live-tracking currently-placed grids and dynamically adjusting a cost, which needs real scripting rather than static XML. Interesting for later, but it's a Stage 5+ idea, not Stage 1.

**Production-block spam is already a solved problem, not a new one.** `BlockLimits` on the core itself already caps "how many Assembler blocks total" independent of what gates the upgrade — no separate anti-spam mechanic needed if Advanced Civilian production blocks end up costing Industrial Components.

### Two smaller open items

- **Static defensive-only grids** (a "gun outpost" concept) — already covered by the existing Base/Outpost `WeaponCap` BlockLimits (4 and 2 respectively), no new core type needed. Revisit only if that turns out to feel insufficient in practice.
- **Small utility grids with no functional blocks** (ramps, bridges, similar) — most core-limiting mods of this style allow a small default block count on any grid with no core placed at all, specifically so minor builds aren't blocked. Not confirmed for Ship Core Framework specifically — needs an actual check against the framework's real behavior, not an assumption either way.

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
- [ ] Once Ship Core Framework is actually integrated, test spawning an NPC ship with no core at all — confirm it isn't rejected, capped, or otherwise affected before assuming NPCs can stay core-free.
- [ ] Check Ship Core Framework's actual default behavior for coreless/small grids — confirm whether a minor build (ramp, bridge, similar) below some threshold is naturally exempt, per the "Two smaller open items" note above.
- [ ] Add type-appropriate ammo to NPC cargo loot — naval gun ammo on ships, aircraft gun ammo on planes — extending the existing `WW2-Loot-*` profiles per ship class. Blocked on confirmed ammo magazine SubtypeIds from Fletcher Armaments/Consty's Ordnance (separate from the weapon *block* IDs already gathered — ammo magazine IDs are typically distinct strings, need their own lookup pass once the weapon rebuild settles).

**To-do:**
- [ ] Add Industrial/Military/Advanced Military Components to existing `WW2-Loot-*` container profiles at tuned drop frequencies, per the military/civilian sourcing split confirmed above.
- [ ] Sub-classify the existing `WeaponBlocks` `BlockGroup` split (Guns/Bombs/Torpedoes/Smoke) into basic vs. advanced, to support vanilla-vs-Military-Component weapon costing.
- [ ] Write `ShipCoreConfig_World.xml` (confirm `MaxPossibleSpeedMetersPerSecond:150`, `MassTypeMode`, `FrictionSpeedValueMode`).
- [ ] Write `BlockGroup` definitions: `ProductionBlocks`, `CargoBlocks`, `ToolBlocks` (can start now, vanilla types) and `WeaponBlocks` (blocked on weapon rebuild).
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

No new grids required. Adapt AaW's Capturable/CapturableController pattern to one installation type (Ammo Depot is simplest) — on capture: recolor grid, flip block ownership, disable old owner's supply triggers, enable new owner's. Hooks into existing `WW2-Manipulation-AmmoDepots` and faction-override plumbing.

**To-do:**
- [ ] Build capture trigger chain for Ammo Depot.
- [ ] Wire captured territory in as a third feed into the Stage 1 unlock currency (confirmed design — see Stage 1).
- [ ] Playtest: confirm a captured depot actively works for its new owner.

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

- **Rest of each ladder:** Destroyer → Cruiser → Heavy Cruiser → Battleship → Carrier (Naval); Attacker/Bomber → Heavy Bomber (Air); Light Tank → Medium Tank → Heavy Tank (Wheeled). Cruiser-tier roster is now named: **Trento** and **Zara** (Gray) alongside **Algerie** (Green) — this also fixes the old Gray/Green Cruiser imbalance, since Gray previously had none at all. Emile Bertin and a completed Comandante Margottini remain in the mix as well, so Green ends up with two Cruiser-tier hulls (Emile Bertin, Algerie) against Gray's two (Trento, Zara) — nicely balanced once all four are built. **Battleship** now has real intent behind it too — one hull per faction, not yet named/started. Note: Algerie and Emile Bertin both carry the AWG piston/hinge catapult-floatplane assembly responsible for the grid-loading bug (see Stage prefab checklist) — factor the rebuild into their timelines, not just Emile Bertin's rebuild-list entry.
- **Upgrade Modules:** convert core-swap progression to modules-on-top-of-one-core where it improves the feel (per the confirmed schema support above).
- **Retaliation/escalation** (from AaW): tiered "strike back" raids against captured territory, cooldown-limited. Reuses Stage 3's capture plumbing.
- **Fleet/Escort formalization** (from AaW's EscortSystem): generalize the existing `CarrierSpawn` escort pattern into something reusable across ship classes.
- **Dynamic territory framing** (from GVK S10): even a coarse "who holds more captured installations" counter feeding spawn frequency/difficulty gets most of the value of GVK's full alliance-strength system.
- **Narrative flavor:** ScenarioTools for MES Events (Workshop 2998575759) for NewsFeed broadcasts and dynamic GPS markers, no custom C# required.
- **Economy stations** (AaW + GVK both): friendly ports buying/selling fuel, ammo, repairs, on top of the Stage 1 player-base economy.

---

## Parallel track — Custom Planet

Doesn't block or get blocked by the staged sequence above — new terrain doesn't need Stage 4's new hulls to exist, and Stage 4's hulls don't need new terrain either. Pick this up whenever, including alongside the weapon rebuild, since it's a genuinely different skill (terrain/image work, not XML/grid design) and switching between them may be a welcome break rather than a distraction.

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

**Sub-factions.** Not scoped yet, named so early architecture doesn't box it out. Splitting Gray/Green into historical sub-factions (Germans/Italians under Gray; French/US/UK/Soviets under Green, or some other split) matches AaW's structure of many factions under fewer broad alliances. Practical implication for everything above: avoid hardcoding "GRAY"/"GREEN" any more than necessary, prefer patterns that generalize to a new faction being copy-and-retarget rather than a rewrite.

**More ore/ingot variety, SDX2-inspired.** SDX2 doesn't add a resource layer beneath their components — it just adds more raw ores and ingots alongside vanilla's existing set (lead, copper, etc., sitting next to iron, silicon, cobalt). So the actual tradeoff here is simpler than "add a new economic tier": it's whether refining/component-building recipes get more varied and specific, or stay on vanilla's existing ore set. Still a real accessibility-vs-depth call — more ore types means more mining/refining variety but also more for a new player to track — but it's additive breadth, not structural complexity. Parked for the same reason as the custom planet and map tool: not needed to prove Stage 1's core loop, easy to add later by defining new ore/ingot pairs and wiring them into whichever recipes should use them, without needing to touch the three-component system's own structure at all.

---

## Why this order

The weapon rebuild has to happen first, full stop — nothing else is testable on top of a dead weapon mod. After that, Stages 1–3 are entirely trigger/XML work on the (now-rebuilt) existing roster and get a genuinely different-feeling mod — full core progression across ships/planes/tanks/bases, squadron behavior, reputation, capturable territory, salvage economy — before a single new hull needs designing. Stage 4 is deliberately small so the payoff of "new grids" gets tested cheaply before Stage 5 asks for more of them. Each stage's playable milestone stands on its own.

The two parallel tracks (Custom Planet, Map Tool) sit outside this sequence deliberately — they don't block the stages and the stages don't block them. Pick up the planet work whenever the XML/grid work needs a break; leave the map tool alone until there's live territory worth mapping.
