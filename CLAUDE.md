# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

PersoLootRoll is a World of Warcraft addon (Interface: 120000 / Patch 12.0) implementing a Need-Greed-Pass loot rolling system for Personal Loot. Built on the **Ace3 framework** (LibStub-based). Current version: 25.00.

## Running Tests

**CLI tests (requires Lua binary):**
```bash
lua Test.lua          # run all tests
lua Test.lua -b       # build test mode
```

**In-game tests:** Load WoWUnit addon, tests auto-run on login. Manual: `/plr test`

Code marked `#@do-not-package@` ... `#@end-do-not-package@` is excluded from releases (test utilities, debug code).

## Architecture

### Layer Structure

```
Libs/       External libraries (Ace3 suite, LibUtil, LibRealmInfo, LibDBIcon)
Core/       Addon lifecycle, events, hooks, options
Models/     Roll and Item data objects
Modules/    Session (masterloot), Inspect, Trade
Util/       Comm, Unit, Locale, Registrar, general utilities
GUI/        Roll frames, Rolls window, Actions window, Widgets
Plugins/    EPGP, PersonalLootHelper, RCLootCouncil integrations
Data/       Static item/trinket lists (Items.lua), instance metadata (Instances.lua)
Locale/     11 language files (enUS/deDE/zhCN/zhTW + partials)
Tests/      Test runner (Test.lua), WoW API mocks, Unit/ specs
```

### Addon State Machine

The addon cycles through: `DISABLED → ENABLED → ACTIVE → TRACKING`

Transitions are managed in `Core/Addon.lua`. State gates all loot-roll operations.

### Event Flow

```
WoW events (CHAT_MSG_LOOT, LOOT_*, GROUP_*)
  → Core/Events.lua listeners
  → Create/update Roll objects (Models/Roll.lua)
  → Roll fires internal events (BID, START, AWARD, …)
  → GUI subscribes and redraws frames
  → Comm broadcasts status to group members
```

### Communication

`Util/Comm.lua` wraps AceComm-3.0 with ChatThrottleLib:
- Addon users: AceComm channel messages
- Non-addon users: in-game chat (whisper/say/raid)
- Cross-realm: whisper fallback

### Key Models

**`Models/Roll.lua`** (~55 KB) — core roll lifecycle: pending → running → done. Tracks bids, winner selection, status sync, and fires events to subscribers.

**`Models/Item.lua`** (~56 KB) — item eligibility: ilvl comparison, class/spec restrictions, transmog detection, async item data loading via callbacks.

### SavedVariables

Defined in `PersoLootRoll.toc`:
- `PersoLootRollDB` — user settings (AceDB, per-profile)
- `PersoLootRollIconDB` — minimap icon position
- `PersoLootRollML` — active masterlooter session
- `PersoLootRollDebug` — debug mode flag

Settings schema and migration logic live in `Core/Options.lua`.

### Plugin System

Plugins in `Plugins/` hook into Roll events via `Util/Registrar.lua`. Each plugin is an optional AceAddon module; the main addon loads them if the target addon is present.

## Development Notes

- Loading order is defined by the XML manifests: `libs.xml → core.xml → models.xml → modules.xml → gui.xml → plugins.xml → tests.xml`
- `Init.lua` defines module namespaces before any module code loads.
- Item data queries are always async — use callbacks, never expect synchronous results from `Item` methods.
- Throttle all outbound chat/comm via ChatThrottleLib (already handled in `Util/Comm.lua`); never call `SendChatMessage` directly.
- Localization keys are defined in `Locale/enUS.lua` and accessed via `addon.L["key"]`.
