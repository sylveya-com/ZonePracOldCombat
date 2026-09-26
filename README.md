# ZonePracOldCombat

1.8 PvP mode enforcement for [ZonePractice Pro](https://modrinth.com/plugin/zonepractice-pro) via [OldCombatMechanics](https://github.com/kernitus/BukkitOldCombatMechanics/releases)

## » About

This addon bridges ZonePractice Pro and OldCombatMechanics. Maps arena ladders to OCM combat modes (`old` / `new`) and applies module overrides per-player at match start. No world restriction headaches, no manual `/ocm mode` commands — players get the right combat mode automatically.

## » Dependencies

- **ZonePractice Pro** — required
- **OldCombatMechanics** — required

## » Installation

1. Install ZonePractice Pro and OldCombatMechanics
2. Drop `ZonePracOldCombat-1.0.0.jar` into your `plugins/` folder
3. Restart the server
4. Edit `plugins/ZonePracOldCombat/config.yml` to map ladders to modes

The plugin disables itself on startup if either the ZonePractice Pro or OldCombatMechanics API is missing.

## » Configuration

### `config.yml`

```yaml
# For optimal out-of-the-box OldCombatMechanics settings,
# copy the contents of example-ocm-config.yml into
# plugins/OldCombatMechanics/config.yml (back up the original first if needed).
# The example file is located in the same folder as this config.yml.

# Which ladders use which mode
ladder-modes:
  BedWars: old
  SkyWars: old
  Bridges: old
  MLGRush: old
  TNTSumo: old
  BattleRush: old
  Boxing: old
  BuildUHC: old
  Debuff: old
  Nodebuff: old
  PearlFight: old
  Soup: old
  Sumo: old
  SG: old
  Fireball: old
  Spleef: old

# Modules to enable for each mode (must match OCM module names)
modules:
  old:
    - "attack-frequency"
    - "old-armour-durability"
    - "old-fishing-knockback"
    - "fishing-rod-velocity"
    - "projectile-knockback"
    - "disable-crafting"
    - "old-brewing-stand"
    - "disable-enderpearl-cooldown"
    - "disable-attack-cooldown"
    - "disable-sword-sweep"
    - "old-tool-damage"
    - "sword-blocking"
    - "shield-damage-reduction"
    - "old-golden-apples"
    - "old-player-knockback"
    - "old-player-regen"
    - "old-armour-strength"
    - "old-potion-effects"
    - "old-critical-hits"
    - "disable-attack-sounds"
  new: []
```

Ladder names are matched case-insensitively, so `nodebuff`, `Nodebuff` and `NODEBUFF` are equivalent. The mode key is **not** case-insensitive — `old` and `new` must match the keys under `modules`.

An `example-ocm-config.yml` is generated next to `config.yml` with recommended OldCombatMechanics settings for the modules listed above.

## » How it works

- Listens to `MatchStartEvent` and `MatchRoundStartEvent`
- Resolves the ladder name from the match at runtime
- Looks up the configured mode (`old` / `new`) for that ladder
- Applies `setModuleOverridesForPlayer()` with `FORCE_ENABLED` on every module listed under that mode — bypasses OCM world-based modeset restrictions
- Clears all overrides on `MatchEndEvent`
- Also clears overrides on `/leave` if the player has no live match, covering the case where ZPP ends a match without firing `MatchEndEvent`

## » Compatibility

- Compatible with any Minecraft version supported by both **ZonePractice Pro** and **OldCombatMechanics**.

Enjoy ZonePracOldCombat!
