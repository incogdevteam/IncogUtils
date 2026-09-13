# IncogUtils 1.1.55

IncogUtils is the unified Paper/Purpur 26.2 suite for the Incog server family.
It combines RPG progression, custom enchants/items, reforges, relics, pets,
achievements, IncogEcon/Hex, claims, anti-macro, ItemForge, homes, vaults,
player shops, and the Shopping District in one Maven-built plugin JAR.

## Latest update

Version 1.1.55 includes the latest menu, claims, market, and PVP action-bar updates.
The optional PVP action-bar status is a short periodic
reminder. It appears for five seconds, clears during inactivity, and returns
every three minutes by default without changing the actual `/pvp` state. Both
timings are configurable. A full server restart is required when installing
the JAR.

This release also includes the 1.1.51 and 1.1.46 changes:

- `/settings` controls personal skill-progress, numeric-health, level-up, and achievement notifications.
- `/adminsettings` controls global and per-feature player access. Defensive listeners and data storage remain loaded.
- Skill progress and numeric health share the action bar; root inventory menus have a Back button to `/menus`.
- Bundled feature prefixes use the shared `IncogUtils • Feature` format, with custom prefixes preserved.
- Peaceful and PvP-disabled players receive environmental and ordinary mob damage; protection blocks damage attributed to another player, including projectiles, tamed mobs, and player-placed TNT.

See [1.1.55 update notes](INCOGUTILS_1.1.55_UPDATE_NOTES.md),
[1.1.51 update notes](INCOGUTILS_1.1.51_UPDATE_NOTES.md),
[1.1.50 update notes](INCOGUTILS_1.1.50_UPDATE_NOTES.md),
[1.1.49 update notes](INCOGUTILS_1.1.49_UPDATE_NOTES.md),
[1.1.48 update notes](INCOGUTILS_1.1.48_UPDATE_NOTES.md),
[1.1.47 update notes](INCOGUTILS_1.1.47_UPDATE_NOTES.md),
[1.1.46 update notes](INCOGUTILS_1.1.46_UPDATE_NOTES.md), and the
[changelog](CHANGELOG.md) for details. The 1.1.55 source targets Java 25;
live server behavior still needs verification on your installation.

This release includes the custom enchantment system and its complete reference
in [enchantments.md](enchantments.md). That guide lists every enchantment's
effect, maximum level, compatible item types, and current acquisition method.
Acquisition routes include enchanting-table rolls, generated loot tables, rare
mob drops with escalating per-player chances, milestone items, recipes, and
achievement-linked rewards.

| System | Included |
|---|---|
| Progression | 17 configurable skills, level trees, milestones, global/per-skill caps, stat sources, numeric high-health HUD, and SQLite saves |
| RPG catalog | 100 non-overlapping custom enchants, 52 reforges, 77 custom items, 68 milestone relic rewards, 3 accessory slots, and 34 achievements |
| Pets | 175 configurable pets, skill filtering, skull visuals, XP/levels, five rank pets, and a disabled-by-default master switch |
| Economy | Vault wallet, rank-limited weekly infinite-stock events, market, `/sell`, auctions, orders, trades, player shops, Sell Wands, XP Vault, stash, price history, and The Hex |
| Protection | Claims, Peaceful/Aggressive PVP, configurable TNT raids, anti-macro verification, staff freeze, and Shopping District protection |
| Utilities | ItemForge, homes, vaults, `/menus`, dedicated plugin reload, PlaceholderAPI, LuckPerms, and optional WorldEdit/FAWE templates |

## Requirements

- Paper or Purpur 26.2
- Java 25
- ExcellentEnchants is supported and recommended, but not required
- Vault plus an economy provider are required when the Economy module is enabled

The production JAR includes its SQLite JDBC runtime. Player data is stored in
`plugins/IncogUtils/player-data.db` using WAL-mode SQLite and is cached in
memory while a player is online.

## Maven build

1. Install a Java 25 JDK and Maven 3.9+.
2. In the project folder, run `mvn --batch-mode clean verify`.
3. Stop the server and remove every old IncogRPG, IncogEcon, Incog-Claims, Incog-AntiMacro, and ItemForge JAR. Keep only the new IncogUtils JAR.
4. Copy `target/IncogUtils-1.1.55.jar` into the server's `plugins` directory.
5. Start the server. Keep existing configuration and player-data files. For later
   configuration changes, use `/iureload`, `/incogreload`, or `/rpgreload`.

`pom.xml` is the authoritative build file. Use Maven for this project and its
GitHub CI.

## ItemForge catalog

Vanilla Creative tabs cannot be extended by a server-only plugin. Give creative
builders `itemforge.catalog`, then they can run `/itemforge catalog` to browse
and take one copy of every giveable custom item from IncogUtils without
receiving editor or item-management access. This includes ItemForge, IncogRPG
items/relics, custom enchant books, Claims items, Vault Regen Keys, and the
Sell Wand.

## PlaceholderAPI

With PlaceholderAPI installed, IncogUtils automatically exposes player-facing
claims, homes, economy, market, daily-reward, XP vault, Shopping District, and
RPG values. See [Placeholder reference](docs/PLACEHOLDERS.md).

Peaceful/Aggressive markers are reconciled directly with NEZNAMY TAB when it
is installed, without replacing the rank suffix already configured in TAB.
The check repeats every 40 ticks by default (`playstyle-icons.tab-refresh-ticks`
in `modules/claims/config.yml`) to recover from delayed player loading and
formatter refreshes. Restart after changing the refresh interval. Proxy TAB setups may use
`%incogutils_claims_playstyle_suffix%` through TAB-Bridge instead.

## GitHub Actions CI

`.github/workflows/maven.yml` builds and tests the project on Java 25 whenever
you push to `main` or `develop`, open a pull request to `main`, or run it
manually. It restores the Maven cache, runs `mvn --batch-mode
--no-transfer-progress verify`, uploads the shaded JAR, and uploads Surefire
reports on failure. It is CI-only and requires no repository secrets.

See GitHub's [Maven build guide](https://docs.github.com/en/actions/tutorials/build-and-test-code/java-with-maven) if you later want to add release publishing.

## Configuration and data

All live configuration lives under `plugins/IncogUtils/`. Files in
`src/main/resources/` are bundled defaults; edit the generated live copy, not
the JAR.

- `config.yml`: caps, global XP multiplier, stat caps, anti-exploit behavior, persistence, menus, and compatibility.
- `skills.yml`: every skill, XP curve, sources, per-level stats, milestones, commands, and per-skill cap.
- `enchants.yml`: all 100 IncogRPG-only enchants, item groups, conflicts, proc chance, cooldown, and per-level effect scaling.
- `enchantments.md`: the player-facing enchantment catalog with effects and acquisition instructions.
- `reforges.yml`: station costs, weighted rolls, compatible item groups, rarity, and every stat on every reforge.
- `items.yml`: names, lore, models, stats, preloaded enchants/reforges, recipes, mob drops, and obtainment text.
- `relics.yml`: 51 additional accessory-ready relic definitions (the other 17 signature rewards remain in `items.yml`).
- `pets.yml`: 175 pets, linked-skill bonuses, pet XP curves, unlock levels, visuals, five rank-exclusive pets, and the disabled-by-default master switch.
- `achievements.yml`: advancement-tree nodes, metrics, targets, thresholds, parents, visibility, frames, and rewards.
- `messages.yml`: all player-facing text in MiniMessage format.

| Module path | What it controls |
|---|---|
| `antimacro/config.yml` | anti-macro checks, thresholds, challenges, alerts, and actions |
| `claims/config.yml` | claim sizes, flags, earning tiers, Peaceful/Aggressive behavior, raid TNT, and cooldowns |
| `itemforge/config.yml` | ItemForge logging and passive ability settings |
| `itemforge/items.yml` | ItemForge custom-item definitions, including custom durability |
| `economy/config.yml` | Vault mode, market, weekly rank events, rank perks, player shops, auctions, trade, Hex, and Shopping District |
| `economy/shopping-district.yml` | saved plot owners, names, grid positions, layouts, sizes, and expansions |
| `homes/config.yml` | home limits, warmup, danger and combat checks |
| `vaults/config.yml` | vault keys, cooldowns, resets, and rewards |

Vault administrators can distribute the real, PDC-tagged keys with
`/regenkey <player> <normal|ominous> [amount]` (aliases: `/vaultregenkey`,
`/vrkey`). It only targets online players and requires
`incogvaults.admin.keys`; `incogvaults.admin` grants it to operators. The
amount is 1–64 and any inventory overflow is dropped safely at the recipient's
feet.

### ItemForge custom durability

In the ItemForge item editor, click **Maximum durability** and enter a positive
whole number. The item will receive genuine vanilla durability beginning at
full health; normal attacks, mining, and other vanilla durability use reduce it
as expected. Use `none` to remove the custom value. The same setting is
available in `plugins/IncogUtils/itemforge/items.yml` as `max-durability`:

```yaml
items:
  ember_blade:
    material: DIAMOND_SWORD
    max-durability: 2400
```

Minecraft does not allow a max-durability item to stack above one, so ItemForge
enforces a stack size of one while this setting is enabled. `unbreakable: true`
still takes precedence and prevents the item from losing durability.

Back up `player-data.db` together with `player-data.db-wal` and
`player-data.db-shm` after stopping the server. Back up the whole plugin folder
before upgrades so the module data files are covered too.

### Shopping District

`plugins/IncogUtils/economy/config.yml` contains the `shopping-district`
section (its source default is `src/main/resources/modules/economy/config.yml`).
It controls the separate void world (including environment and difficulty), island materials, starter and
maximum plot sizes, expansion steps, escalating plot prices, first-join
allocation, teleport cooldowns, PvP/mob rules, and protection behavior.
PvP is disabled by default and is enforced by both the world PvP flag and the
district damage listener.

When Multiverse-Inventories is installed, IncogUtils automatically finds the
existing MVI group containing the configured Overworld (`world` by default),
adds `incog_shopping_district`, and enables the `inventory` share. That share
keeps inventory contents, armor, offhand, and Ender Chest consistent between
the two worlds. Configure this under `shopping-district.inventory-sharing`;
set `group` explicitly if the Overworld belongs to more than one group.
Players receive a starter plot automatically and use `/shopdistrict home` to
visit it. They can place a chest or barrel inside their plot, hold the item to
sell, and run `/pshop create <price>`. Additional district commands are:

- `/shopdistrict spawn`
- `/shopdistrict visit <player> [plot-name|number]`
- `/shopdistrict list [player]`
- `/shopdistrict info [plot-name|number]`
- `/shopdistrict expand [target-size] [plot-name|number]`
- `/shopdistrict buy [template-id]`
- `/shopdistrict rename [current|plot-number] <new-name...>`
- `/shopdistrict sell [plot-name|number]`

Only the public hub and allocated plots are materialized; all unallocated grid
space beyond the outer plot boundary remains void. IncogUtils creates one
continuous paved district surface containing the hub and allocated plots. One
expanding world border and configurable outer-perimeter barriers prevent void
falls without putting walls between shops. A movement safeguard remains as a
final void rescue.
New plots start at 40×40, everyone can purchase up to
50×50, and higher paid size ceilings are unlocked by rank: Secret 60×60,
Unknown 70×70, Cryptic 80×80, Ominous 90×90, and Incog/Incog+ 100×100.
The rank grants only the purchase limit—never a free expansion. Other players'
plots, containers, hoppers, pistons, fluids, explosions, hostile entities, and
dropped items are protected by default. Staff can use the
`incogshop.shoppingdistrict.admin`, `.bypass`, and `.free` permission nodes.

Templates are configured beneath `shopping-district.template.templates`.
Place a WorldEdit/FAWE `.schem` file below `plugins/IncogUtils/templates/`, then
reference it with `schematic-file: templates/<name>.schem`. The loader centers
the real non-air content, ignoring empty WorldEdit selection padding.

**Schematics now paste one block lower than the old behavior.** With the
default `placement.y-offset: 0`, the lowest non-air block rests at `floor-y`.
This applies to existing configuration files too. Set `y-offset: 1` for a
specific template only if you intentionally want the previous height.

Plot teleports no longer use the raw center block, which could be inside a
centered template. `/shopdistrict buy`, `/shopdistrict home`, visit/search
teleports, and staff plot teleports find the nearest two-block-high passable
space with a solid non-hazardous floor. The district hub spawn also sits one
block above its platform. If an entire plot has no valid landing space, the
teleport is cancelled instead of suffocating the player.

Registered `/pshop` containers in an owner's allocated plot receive a private
`Your Shop` floating label by default. It is a non-interactive hologram that is
hidden from every other player, including staff, rather than a public marker.
Configure it at `shopping-district.owner-shop-holograms` (`enabled`, `text`,
and `y-offset`), then run `/shopdistrict admin reload` or `/reload`.

WorldEdit/FAWE templates are pasted one horizontal layer at a time. The default
`shopping-district.template.schematic-layers-per-tick: 1` spreads tall builds
over multiple ticks to avoid one large block edit; raise it cautiously only for
small, pre-tested templates.

### Market rank perks

The default server-market buy discounts and sell bonuses use these LuckPerms nodes: Secret (`incogshop.discount.secret` / `incogshop.sellbonus.secret`) at 5%, Unknown at 10%, Ominous at 15%, Cryptic at 20%, Incog at 25%, and Incog+ (`incogshop.discount.incogplus` / `incogshop.sellbonus.incogplus`) at 30%. The highest matching tier wins; tiers do not stack. These apply only to instant server-market transactions, `/sell`, and Sell Wands. All twelve rank-perk permissions are formally declared by IncogUtils, so they appear as registered plugin permissions after installing this version.

### Daily Market Flux

Every Minecraft day (24,000 full-time ticks; normally 20 real minutes), the
market chooses a random number of tradable items and gives each an independently
rolled Buy and/or Sell multiplier. By default 4–12 items are selected, Buy and
Sell each have a 50% chance to be affected, and any selected item that misses
both rolls is given one modifier anyway. The default range is `0.80x` to
`1.75x`: a lower Buy multiplier is a discount, while a higher Buy multiplier is
a surcharge; a higher Sell multiplier is a bonus payout. An item may receive
both effects on the same Minecraft day.

Configure it at `market.daily-flux` in `plugins/IncogUtils/economy/config.yml`.
Set `world` to the world whose day/night cycle should drive the reset; leave it
blank to use the first loaded normal world. Active modifiers are persisted in
`economy/daily-market-flux.yml` and appear in each affected market item's lore.

### Daily Rewards

`/daily` claims the player’s configured reward; `/daily status` shows the
current streak and next reward. `/daily admin` (permission
`incogshop.daily.admin`, OP by default) opens the in-game editor. A player can
claim once per configured calendar interval, with the reset time controlled by
`daily-rewards.zone-id` in
`plugins/IncogUtils/economy/config.yml`. Missing a claim interval restarts the
cycle at Day 1; claiming the final configured streak day wraps to Day 1 on the next consecutive claim.

Each entry under `daily-rewards.rewards` can include any combination of:

- `money: 500.0` — deposited through the server’s Vault economy provider.
- `items: ["DIAMOND 4"]` — vanilla material plus amount; full inventories drop
  the overflow at the player’s feet.
- `commands: ["crate key {player} daily 1"]` — console commands. Supported
  placeholders are `{player}`, `{uuid}`, and `{day}`.

Set `cycle-days` to any streak length from 1 through 30. The default is a
30-day cycle. `/daily admin` now includes a **Streak length** button, so the
cycle can be changed in-game without editing YAML. `claim-interval-days: 1`
means daily; set it to `7` for weekly, or any number of days up to 365.

The admin editor also supports rank-specific rewards. Click **Add LuckPerms
Rank**, type the group name exactly (for example `secret`, `incog`, or
`incog+`), then click that rank. The editor displays a separate button for every active
streak day, up to Day 30. Each Day button supports:

- Left-click: set the reward item to the stack held in your main hand.
- Right-click: set that day’s money reward through chat.
- Shift-left-click: set a console command through chat.
- Middle-click: clear that rank’s override for the day, reverting to Base
  Rewards for that day.

Rank rewards are stored under `daily-rewards.rank-rewards`; the root `rewards`
section is always the fallback. The highest matching configured rank priority
wins. Your standard ranks are automatically prioritized Secret → Unknown →
Ominous → Cryptic → Incog → Incog+. Permission:
`incogshop.daily` is granted to everyone by default.

### UltraCosmetics menu shortcut

When UltraCosmetics is installed, the first `/menus` page includes an
**UltraCosmetics** shortcut. It runs `/uc menu` as the player and respects
UltraCosmetics’ own `ultracosmetics.openmenu` permission. If UltraCosmetics is
missing or the player lacks that permission, the button clearly displays as
locked; IncogUtils still loads normally.

### Weekly Infinite Stock

`market.weekly-infinite-stock` in `plugins/IncogUtils/economy/config.yml`
creates one configurable weekly, rank-only instant-buy event. The default event
starts each Sunday at 20:00 in `America/New_York`, lasts **90 minutes (1.5
hours)**, and selects its materials just 10 seconds before it begins. The draw
is persisted in `economy/weekly-infinite-stock.yml`, so a restart cannot reroll
an active event.

One shared random draw is made from enabled, buyable market materials. By
default Cryptic may buy the first selected item without consuming stock, Incog
may buy the first two, and Incog+ may buy all three. A player qualifies through
either the matching LuckPerms rank group (`cryptic`, `incog`, or `incogplus` /
`incog+`) or the existing discount permission (`incogshop.discount.cryptic`,
`.incog`, or `.incogplus`). Both the group names, permissions, and item counts
are configurable.
The highest matching item count wins; ranks never stack. The event bypasses
only stored stock for eligible instant purchases—prices and transaction fees
still apply normally.

Eligible players see a private `Weekly Infinite Stock` card in `/market` and
the instant-buy page. Before the draw it shows the exact draw countdown; after
selection it reveals only that player's entitled item(s), never higher-tier
items. Use `excluded-materials` to remove any Bukkit material from future draws.
`/weeklystock` is an always-available status check that reports the next event,
your entitlement, and (when revealed) only your selected items.

`/globalrestock` restores every market entry to `market.initial-stock` without
changing prices, demand pressure, modes, or auto-restock settings. It requires
`incogshop.admin.globalrestock` and has aliases `/marketrestock` and
`/restockall`.

### Aggressive-claim TNT raids

Any Aggressive player can place and ignite TNT inside another Aggressive claim, regardless of that claim's block-place or interact flags. Peaceful claims remain protected. By default, a raider can have up to 10 active raid TNT blocks/primed TNT at once; adjust `tnt-raid.max-active-per-player` in `plugins/IncogUtils/claims/config.yml` to change this limit.

Reloading is transactional: a malformed file is rejected and the previous valid configuration remains active.

Vanilla attribute writes use Bukkit's stable attribute registry rather than a development-build-specific Paper registry return type. If a future server build is still binary-incompatible, `stats.attribute-application.failure-policy: DISABLE` stops attribute writes after the first failure and suppresses all scheduler stack traces. Set `log-first-failure: false` for completely silent fallback behavior. Skill/custom-stat calculations and `/stats` remain available while vanilla attribute writes are disabled.

On a version upgrade, IncogRPG adds only missing bundled paths and catalog entries, preserves every existing configured value, and creates `*.pre-<version>.bak` backups for changed YAML files. This can be disabled with `general.merge-missing-defaults-on-upgrade`.

## Player commands

- `/menus` — open the central IncogUtils navigator for RPG, claims, economy, Hex, ItemForge, and utility menus.
- `/freeze <player> [reason]` — moderator freeze toggle; use `/freeze <player> off` to explicitly unfreeze and `/freeze list` to review active freezes.
- `/skills [player]` — open the skill and combined-stat menu.
- `/skill <skill> [player]` — open the paginated per-level reward tree.
- `/stats [player] [page]` — inspect every active stat and its exact source distribution.
- `/items [page]` — browse custom items, recipes, drops, and obtainment instructions.
- `/enchants [page]` — browse IncogRPG's non-overlapping enchant catalog, including the configured bonus increase per enchant level.
- `/achievements [player] [page]` — browse progress, parent branches, requirements, and rewards.
- `/pets [skill] [page]` and `/pet` — browse/summon pets when enabled.
- `/accessories` — equip up to three persistent skill relics.
- `/reforge` — open the weighted reforge anvil.
- `/shopdistrict` — manage your protected plot, visit shops, expand, or buy a
  new plot. Aliases: `/sdistrict`, `/shoppingdistrict`, `/shopplot`.

## Administration

- `/incogrpg reload`
- `/incogrpg save`
- `/incogrpg skill get <player> [skill|all]`
- `/incogrpg skill setlevel <player> <skill> <level>`
- `/incogrpg skill setxp <player> <skill> <current-xp>`
- `/incogrpg skill settotalxp <player> <skill> <total-xp>`
- `/incogrpg skill addxp <player> <skill> <amount>`
- `/incogrpg skill removexp <player> <skill> <amount>`
- `/incogrpg skill addlevels <player> <skill> <amount>`
- `/incogrpg skill max <player> [skill|all]`
- `/incogrpg skill reset <player> [skill|all]`
- `/incogrpg enchant apply <id> [level]`
- `/incogrpg enchant remove <id>`
- `/incogrpg enchant clear`
- `/incogrpg enchant book <id> [level] [player]`
- `/incogrpg reforge set <id> [force]` — set the held item's exact reforge.
- `/incogrpg reforge remove|get` — remove or inspect the held item's reforge.
- `/incogrpg reforge setfor <player> <mainhand|offhand|helmet|chestplate|leggings|boots> <id> [force]`
- `/incogrpg reforge removefrom|getfrom <player> <slot>`
- `/incogrpg reforge stone <player> <reforge> [amount]`
- `/incogrpg reforge list [page]`
- `/incogrpg item give <player> <id> [amount]`
- `/incogrpg item refresh [player]`
- `/incogrpg achievement get <player> [achievement|all]`
- `/incogrpg achievement grant <player> <id> [give-rewards]`
- `/incogrpg achievement revoke <player> <id>`
- `/incogrpg achievement reset <player>`
- `/incogrpg achievement setprogress <player> <metric> <target> <value>`
- `/incogrpg achievement addprogress <player> <metric> <target> <value>`
- `/incogrpg achievement check <player>`
- `/incogrpg pet get <player> <pet>`
- `/incogrpg pet unlock|lock <player> <pet|all>`
- `/incogrpg pet summon|dismiss <player> [pet]`
- `/incogrpg pet setlevel|setxp <player> <pet> <value>`
- `/incogrpg stats [player]`
- `/incogrpg debug`

All administrator command branches have separate permission nodes under `incogrpg.admin.*`. LuckPerms can grant them normally.

When a safe enchant application targets a configured counterpart (for example, Afterimage and Echo Strike), the new enchant replaces the old one automatically. This also applies to anvil book application. `/incogrpg enchant apply ... force` still intentionally bypasses conflict rules for administrators.

## PlaceholderAPI

- `%incogrpg_total_level%`
- `%incogrpg_skill_mining_level%`
- `%incogrpg_skill_mining_xp%`
- `%incogrpg_skill_mining_required%`
- `%incogrpg_skill_mining_percent%`
- `%incogrpg_stat_custom_crit_chance%`
- `%incogrpg_reforge%`
- `%incogrpg_achievements_completed%`
- `%incogrpg_achievement_first_steps_completed%`
- `%incogrpg_achievement_first_steps_percent%`
- `%incogrpg_metric_blocks_broken_all%`

Replace `mining` or the stat suffix with any configured ID.

## Developer API

Retrieve `IncogRpgApi` from Bukkit's `ServicesManager`. It exposes cached profiles, skill and achievement definitions, progression changes, achievement metrics, calculated stats and their source breakdown, custom-item construction/identification, item enchants, and reforges. `SkillXpGainEvent` is cancellable; `SkillLevelUpEvent`, `CustomEnchantProcEvent`, and `AchievementUnlockEvent` provide integration points.

## ExcellentEnchants compatibility

IncogRPG does not reimplement ExcellentEnchants effects such as Vampire, Venom, Temper, Smelter, Veinminer, Treefeller, Haste, Telekinesis, Replanter, Sniper, Stopping Force, Explosive Arrows, Regrowth, or Double Catch. Its PDC keys and lore markers use the `incogrpg` namespace, and its custom book handler only touches books containing IncogRPG data.

Start with `docs/PLAYER_GUIDE.md` or `docs/ADMIN_GUIDE.md`. See
`docs/SHOPPING_DISTRICT.md` for the protected procedural shop world,
`SKILLS_AND_STATS.md` for the progression matrix, `docs/RELICS.md` for all 68
relics and stats, `docs/PETS.md` for the pet system, `docs/ACHIEVEMENTS.md` for
all 34 advancement nodes, `docs/CUSTOM_ITEMS.md` for item acquisition, and
`docs/REFORGES.md` for all 52 reforge outcomes.
For the duplicate-JAR and attribute-modifier failures shown in the supplied server log, see `docs/TROUBLESHOOTING.md`.

## Player-system quick reference

### Skills, stats, and health

- `/skills` opens the full skill profile; `/skill <id>` opens its paged level
  tree, including configured XP, stat, item, command, and message rewards.
- `/stats` shows every active stat and the exact distribution from skills,
  equipment, enchants, reforges, relics, and pets.
- `progression.global-skill-cap` controls the default cap. Each `skills.yml`
  definition can override it, and `progression.total-level-cap` can limit the
  combined total. `incogrpg.bypass.skillcap` bypasses the combined cap.
- Real health is never reduced by the HUD. `hud.health.limit-vanilla-hearts`
  only limits the visible vanilla row to ten hearts; the independent numeric
  action bar shows the true health when maximum health is non-vanilla.
- Skills, achievement progress, pets, and accessory contents save in SQLite.
  They survive normal restarts; configure `persistence.autosave-seconds` and
  `persistence.save-on-level-up` in `config.yml`.

See [SKILLS_AND_STATS.md](SKILLS_AND_STATS.md) for every default skill and stat
gain, and [PLAYER_GUIDE.md](docs/PLAYER_GUIDE.md) for the player workflow.

### Enchants, reforges, items, and relics

- `/enchants` shows each custom enchant's configured effect, acquisition method,
  and level scaling. The `/menus` Enchant Codex displays the same information
  as a `How to Obtain` subsection on every enchant entry. The catalog does not
  duplicate the overlapping ExcellentEnchants effects.
- Add enchant books through `/incogrpg enchant book <id> [level] [player]`.
  Compatible items and books combine in a normal anvil.
- Counterpart enchantments replace their configured opposite rather than being
  rejected. Administrator `force` commands remain the intentional bypass.
- `/reforge` uses loss-safe escrow. The held item is returned on menu close,
  quit, or plugin disable, and a reroll replaces the old prefix/stat package
  instead of stacking it.
- Reforge sources can be station rolls, custom catalyst items, crafts, mobs,
  chest finds, or source-exclusive natural rolls. `reforges.yml` controls all
  weights, groups, and availability.
- `/items` explains configured recipes/drops. Milestone-exclusive relic rewards
  cannot be obtained through random drop sources.
- `/accessories` opens the three-slot relic bag; equipped relics are included
  in `/stats` and persist in the player database.

The exhaustive catalogs are [REFORGES.md](docs/REFORGES.md),
[RELICS.md](docs/RELICS.md), and [CUSTOM_ITEMS.md](docs/CUSTOM_ITEMS.md).

### Pets and achievements

- Pets are disabled by default: set `settings.enabled: true` in `pets.yml` and
  run the IncogUtils reload command to enable them.
- `/pets [skill] [page]` filters pets by skill; `/pet summon <id>`,
  `/pet dismiss`, and `/pet info` control the active pet.
- Pet skull textures, entity visuals, XP curves, levels, skill XP shares,
  stats, crafting restrictions, rank gates, and modifiers are all configurable.
- Rank-exclusive pets remain visible in the normal menu but display their
  requirement until the player has the matching permission.
- `/achievements` shows live requirements, parent branches, unlock rewards, and
  progress. Achievement chat names are hoverable and explain exactly how the
  node is obtained.

Read [PETS.md](docs/PETS.md), [PET_RANKS.md](PET_RANKS.md), and
[ACHIEVEMENTS.md](docs/ACHIEVEMENTS.md) for the complete default lists.

## Unified command registry

Every command below is declared in `plugin.yml` and registered by the single
IncogUtils host plugin.

| Module | Commands |
|---|---|
| RPG | `/skills`, `/skill`, `/stats`, `/items`, `/enchants`, `/achievements`, `/pets`, `/pet`, `/accessories`, `/reforge` |
| General | `/menus`, `/freeze`, `/incogutils`, `/reload` (`/iureload`, `/incogreload`, `/rpgreload`) |
| Anti-macro | `/antimacro`, `/verify` |
| Claims | `/claims` (`/claim`, `/incogclaims`), `/pvp` |
| ItemForge | `/itemforge` (`/iforge`) |
| Market and storage | `/market` (`/shop`), `/sell`, `/stash`, `/xpvault` (`/xpbank`) |
| Trading | `/ah`, `/pshop`, `/shopdistrict`, `/sellwand`, `/trade` |
| Economy admin | `/hex` (`/thehex`), `/marketadmin`, `/globalrestock` (`/marketrestock`, `/restockall`) |
| Homes | `/sethome`, `/forcesethome`, `/home`, `/delhome`, `/homes`, `/homeadmin` |
| Vaults | `/incogvaults reload` |

### Shopping District commands

| Command | Purpose |
|---|---|
| `/shopdistrict home [plot-name\|number]` | Allocate a starter plot if needed and safely teleport to a selected owned plot |
| `/shopdistrict spawn` | Go to the public district hub |
| `/shopdistrict visit <player> [plot-name\|number]` | Safely visit another player's plot |
| `/shopdistrict list [player]` | List plots, including their names and numbers |
| `/shopdistrict info [plot-name\|number]` | Show a plot's size, expansion, and price information |
| `/shopdistrict buy [template-id]` | Buy an escalating-price plot and choose a configured template |
| `/shopdistrict expand [target-size] [plot-name\|number]` | Expand one plot up to the configured maximum |
| `/shopdistrict rename [current\|plot-number] <name...>` | Save a unique name (1–32 plain-text characters) for a plot |
| `/shopdistrict sell [plot-name\|number]` | Sell a plot, refund its configured amount, and clear it back to void |
| `/shopdistrict find <material> [result]` | Visit a matching registered player shop by the item it sells |
| `/shopdistrict admin reload\|save\|allocate\|tp\|forcesell …` | Staff reload, allocation, teleport, persistence, and force-sale controls |

Aliases are `/sdistrict`, `/shoppingdistrict`, and `/shopplot`. Plot names may
contain letters, numbers, spaces, apostrophes, underscores, and hyphens; a name
cannot be only a number because list numbers are also selectors.

### Economy, homes, claims, and moderation commands

| Command | Purpose |
|---|---|
| `/market`, `/sell`, `/stash` | Dynamic server market, bulk selling, and overflow storage |
| `/xpvault deposit\|withdraw <amount\|all>` | Deposit/withdraw experience |
| `/ah sell\|bid\|buy\|my\|cancel\|claim` | Auction House workflow |
| `/trade <player\|accept\|deny\|cancel>` | Secure player-to-player trade |
| `/pshop create\|remove\|price\|item\|stock\|info\|list` | Physical chest/barrel player shops |
| `/hex` | The Hex item-upgrade GUI |
| `/globalrestock` | Restore all market entries to the configured `market.initial-stock` value without changing price/demand tuning |
| `/sethome`, `/home`, `/delhome`, `/homes` | Named homes with configured warmup/safety checks |
| `/claims` | Claim core, trust, flags, type, merge, delete, and staff controls |
| `/pvp` | Player PVP-protection toggle |
| `/freeze <player> [reason\|off]`, `/freeze list` | Moderator freeze management |
| `/antimacro status\|reset\|reload\|alerts` | Anti-macro monitoring controls |
| `/itemforge create\|edit\|recipe\|give\|duplicate\|delete\|list\|abilities\|inspect\|logging\|reload` | ItemForge management |

`COMMANDS.md` remains a compact registry, while in-game tab completion shows
the exact branches available to a sender.

## Permissions

Use LuckPerms or any compatible Bukkit permission manager. `plugin.yml` has the
full registry; these are the important operational nodes.

| Permission | Grants |
|---|---|
| `incogrpg.use` | Player RPG menus/commands |
| `incogrpg.inspect` | Inspect another player's RPG profile |
| `incogrpg.reforge`, `incogrpg.accessories`, `incogrpg.pets` | Reforge station, relic bag, and pets |
| `incogrpg.admin` | All `incogrpg.admin.*` RPG staff commands |
| `incogutils.menus` | `/menus` |
| `incogutils.freeze`, `incogutils.freeze.bypass` | Freeze powers and immunity |
| `incogantimacro.admin`, `.alerts`, `.bypass` | Anti-macro control, alerts, bypass |
| `incogclaims.use`, `incogclaims.admin` | Claim/player and claim/staff access |
| `itemforge.use`, `itemforge.admin` | ItemForge player/staff access |
| `incogshop.market`, `.sell`, `.stash`, `.xpvault`, `.auction`, `.trade`, `.playershop`, `.hex` | Market and economy features |
| `incogshop.admin.globalrestock` | Run `/globalrestock` to restore default market stock |
| `incogshop.shoppingdistrict`, `.visit`, `.admin`, `.bypass`, `.free` | District use, visiting, staff controls, protection/cooldown bypass, free plots |
| `incoghomes.sethome`, `.home`, `.delhome`, `.admin` | Homes player/staff access |
| `incogvaults.admin` | Vault module administration |

Rank pet nodes are `incogutils.pets.rank.vip`, `.mvp`, `.elite`, `.legend`, and
`.sovereign`.

## Rank discounts and sell bonuses

The highest matching tier wins; tiers never stack. The defaults affect instant
server-market purchases, `/sell`, and Sell Wands only—not player shops,
auctions, trades, or player-created market orders.

| Rank | Discount | Sell bonus | Permission nodes |
|---|---:|---:|---|
| Secret | 5% | 5% | `incogshop.discount.secret`, `incogshop.sellbonus.secret` |
| Unknown | 10% | 10% | `incogshop.discount.unknown`, `incogshop.sellbonus.unknown` |
| Ominous | 15% | 15% | `incogshop.discount.ominous`, `incogshop.sellbonus.ominous` |
| Cryptic | 20% | 20% | `incogshop.discount.cryptic`, `incogshop.sellbonus.cryptic` |
| Incog | 25% | 25% | `incogshop.discount.incog`, `incogshop.sellbonus.incog` |
| Incog+ | 30% | 30% | `incogshop.discount.incogplus`, `incogshop.sellbonus.incogplus` |

Change values or permission nodes under `rank-perks` in
`plugins/IncogUtils/economy/config.yml`.

## Operations and troubleshooting

- Use `/reload`, not Bukkit/Paper's global `/reload`.
- If the economy module is disabled, verify Vault and its economy provider load
  before IncogUtils.
- If a schematic uses the fallback shack, install WorldEdit/FAWE, verify the
  `.schem` lives below `plugins/IncogUtils/`, verify the configured path, then
  buy a fresh test plot. Existing player plots are intentionally never repasted
  during reload.
- A plot teleport that fails has no safe two-block-high landing location; leave
  a solid floor with two passable blocks above it somewhere on the plot.
- To restore legacy template height for one schematic, set that template's
  `placement.y-offset: 1`.
- Attribute compatibility failures use the configurable
  `stats.attribute-application.failure-policy` circuit breaker. With its
  default `DISABLE` mode, custom stat calculation and `/stats` remain usable
  while repeated server log spam is suppressed.

Read [ADMIN_GUIDE.md](docs/ADMIN_GUIDE.md) for startup/backups/verification,
[CONFIG_REFERENCE.md](docs/CONFIG_REFERENCE.md) for the YAML schema, and
[TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) for known runtime issues.
