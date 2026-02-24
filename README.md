```                                                                                               
▄▄▄      ▄▄▄   ▄▄▄▄▄   ▄▄▄▄▄▄   ▄▄▄▄▄  ▄▄▄▄▄▄▄ ▄▄▄▄▄  ▄▄▄▄▄▄▄ ▄▄▄▄▄▄     ▄▄▄      ▄▄▄  ▄▄▄▄▄▄▄ 
████▄  ▄████ ▄███████▄ ███▀▀██▄  ███  ███▀▀▀▀▀  ███  ███▀▀▀▀▀ ███▀▀██▄   ████▄  ▄████ ███▀▀▀▀▀ 
███▀████▀███ ███   ███ ███  ███  ███  ███▄▄     ███  ███▄▄    ███  ███   ███▀████▀███ ███      
███  ▀▀  ███ ███▄▄▄███ ███  ███  ███  ███▀▀     ███  ███      ███  ███   ███  ▀▀  ███ ███      
███      ███  ▀█████▀  ██████▀  ▄███▄ ███      ▄███▄ ▀███████ ██████▀    ███      ███ ▀███████ 
```

```
/=================================================================================\
|            C A T A L Y S T   S U I T E   —   CATALYST CORE & API                |
\=================================================================================/
```

| Mod            | Mod ID       | Package                      | One-line description |
|----------------|--------------|------------------------------|----------------------|
| **Catalyst**   | `catalyst`   | `com.modifiedmc.catalyst`     | Core economy, stats, leaderboards, login rewards, integration hub |


# Catalyst

**The foundation of Modified MC's modular server toolkit.**

> Author: jakerzzz / jczlab | Company: Modified MC
> 
> Platform: NeoForge 21.1.219 | Minecraft 1.21.1

---

## Overview

Catalyst is the core mod that powers the entire Catalyst Suite. It provides the shared economy engine, market and exchange systems, player statistics, leaderboards, NPC quest framework, guild point system, hologram displays, and the central GUI hub that ties everything together. Designed as a vanilla-plus foundation, Catalyst works standalone without any optional dependencies but unlocks progressively richer features as companion mods are detected.

When Cobblemon is present alongside the Cobble Core module, the default currency automatically switches to PokeDollars. When Lightman's Currency is installed, wallet and bank operations integrate seamlessly. When FTB Essentials is detected, teleportation buttons appear in the main GUI. All of this happens at runtime with zero configuration required.

---

## Features

### Multi-Tier Currency System

- **Two default currency lines**: Amethyst Coins and Netherite Coins, each with single coin, pile (9 coins), and block (4 piles) variants
- **PokeDollar currency chain** (when Cobble Core is active): `Relic Coin -> PokeDollar -> Master Token`
  - 9 Relic Coins = 1 PokeDollar
  - 100 PokeDollars = 1 Master Token
- **Lightman's Currency integration**: When detected, enables wallet/bank operations and ATM exchange buttons for the full coin chain
- **Physical item fallback**: Players without a wallet use physical currency items directly
- **Configurable currency hierarchy**: Emeralds as base fallback, Lightman's chain when present, PokeDollars when Cobble Core is active
- **Full crafting recipes**: Coins compress into piles, piles into blocks, and reverse

### Market Block

- **Category-first shopping**: Opening the Market block presents a category selector (General, Poke Balls, Medicine, TMs, Berries, Battle Items, Materials, Valuables) before displaying items
- **Datapack-driven inventory**: Every shop page, item, price, and display name is defined in JSON datapacks
- **Color and formatting support**: Shop titles and item lore support Minecraft color codes
- **Per-item currency overrides**: Individual items can use different currencies than the shop default
- **Multi-page navigation**: Shop pages can chain to additional pages

### Daily Exchange Block

- **Automatic item-to-currency conversion**: Drop items in via hopper or manually; the block sells them at configured rates
- **40% sell-rate default**: Sell prices default to 40% of the buy price to prevent arbitrage, fully configurable via `sell_rate_ratio`
- **Hopper I/O**: Items enter from any side except bottom; currency exits from the bottom
- **Accumulated currency buffer**: Currency accumulates internally and converts to physical emerald items in the output slot
- **Datapack price lookup**: Prices are defined in `data/catalyst/exchange/*.json` files
- **Fallback vanilla pricing**: Common vanilla items have built-in fallback prices when no datapack entry exists

### Villager Trade Replacement

- **Full emerald-to-currency swap**: When enabled, all villager trades that use emeralds as cost or result are automatically replaced with Amethyst Coins
- **Toggle system**: `replace_villager_trades` enables/disables the feature; `trade_currency_mode` switches between `CURRENCY` and `EMERALD`
- **Non-destructive**: Wraps original trade listings without modifying them, preserving all other trade properties

### NPC Quest System

- **Six quest types**: Main Story, Side, Repeatable, World Secret, Server-Wide, and Player-Inspired
- **Step-based progression**: Each quest contains ordered steps with objective types (TALK_NPC, CATCH_POKEMON, DEFEAT_TARGET, COLLECT_ITEM, etc.)
- **Gate conditions**: Quests can require other quest completions, player flags, guild membership, or TXP milestones
- **Persistent player flags**: Quest completions set permanent flags that influence future quest availability and NPC dialogue
- **Repeat modes**: ONCE, HOURLY, DAILY, WEEKLY, MONTHLY, SEASONAL
- **Reward distribution**: Currency, items, TXP, Dungeon Tokens, Guild Gems, and Pokemon rewards
- **Datapack-defined**: All quests are loaded from `data/catalyst/quests/*.json`

### Guild Point System

- **Guild membership tracking**: Players join a single guild at a time
- **Guild point economy**: Points earned through guild activities, invested into skill tree nodes
- **Skill tree progression**: Spend points on nodes for permanent bonuses; investment tracked per-player
- **Balance snapshots**: Query total earned, available (unspent), and per-tree investment breakdown

### Player Statistics & Leaderboards

- **Unified stat tracking**: Economy totals, catches, battles, releases, deaths, kills, dungeon runs, guild progression
- **Minecraft stat mirroring**: Built-in Minecraft stats can be mirrored into the Catalyst system
- **Leaderboard service**: Query top players by any stat key with configurable page size
- **Hologram leaderboards**: Display floating leaderboard text at world positions

### Stats & Leaderboards

| Feature | Description |
|--------|-------------|
| **Player stats** | Increment per-player stats by key (e.g. `deaths`, `player_kills`, `mob_kills`, `play_time_minutes`, `login_count`). |
| **Leaderboard service** | Query top-N players for a stat key. Used by holograms and custom UIs. |
| **Stat keys (API)** | `CatalystAPI.StatKeys`: `DEATHS`, `PLAYER_KILLS`, `MOB_KILLS`, `PLAY_TIME_MINUTES`, `LOGIN_COUNT`. |
| **Commands** | `/catalyst balance [currency]`, `/catalyst pay <player> <amount> [currency]` (and others as implemented). |

### Login Rewards

- Configurable daily (or cooldown-based) login rewards.
- Minimum interval (e.g. 12 hours) to prevent abuse.
- Reward items and broadcast message in config.
- Toggle on/off in `catalyst-common.toml`.

### Hologram Display System

- **Decent Holograms-like functionality**: Create, update, and remove floating text displays anywhere in the world
- **Leaderboard presets**: One-call method to create formatted leaderboard holograms with title, separator, and ranked entries
- **Adapter pattern**: Rendering backend can be swapped (entity-based, packet-based, etc.)
- **Live updates**: Update hologram content without removing and recreating

### Central GUI Hub

- **Information hub**: Unified access to dungeon info, event calendar, trainer/gym info, server info
- **FTB Essentials integration**: When detected, adds RTP, TPA, Spawn, Home, and Warp buttons
- **Balance display**: Shows current currency balance
- **Opened via command**: `/catalyst gui` opens the hub from anywhere

## Catalyst API Surface

The API is used by Catalyst Core and by every Catalyst module. It provides:

### Stats & Leaderboards

- **CatalystAPI.MOD_ID** — `"catalyst"` for `ModList.get().isLoaded()`.
- **CatalystAPI.StatKeys** — `DEATHS`, `PLAYER_KILLS`, `MOB_KILLS`, `PLAY_TIME_MINUTES`, `LOGIN_COUNT`.
- **PlayerStatSnapshot** — Snapshot of a player's stat key → value map.
- **Integration** — Access `CatalystCoreMod.core()` and then `playerStatsService()`, `leaderboardService()` (see CatalystAPI.java Javadoc for examples).

### Economy

- **CurrencyMode** — Enum for balance storage strategy.
- **CurrencyBalance** — Currency ID + amount.
- **CurrencyProfileData**, **CurrencyTier** — Datapack currency definitions.
- **ShopPageData**, **ShopItemEntry** — Market shop structure.

### Integration

- **IntegrationService** — Interface: `integrationId()`, `isAvailable()`, `onEnable()`, `onDisable()`. Optional modules implement this and register with Core.


### Commands

| Command | Permission | Description |
|---------|-----------|-------------|
| `/catalyst balance [currency]` | Player | View your currency balance |
| `/catalyst pay <player> <amount> [currency]` | Player | Transfer currency to another player |
| `/catalyst stats [player]` | Player (admin for others) | View tracked statistics |
| `/catalyst leaderboard <stat> [count]` | Player | Display stat leaderboard |
| `/catalyst gui` | Player | Open the main Catalyst GUI hub |
| `/catalyst reload` | Admin (level 2) | Reload all datapacks |

### Blocks & Items (Core)

| ID | Type | Purpose |
|----|------|--------|
| `catalyst:market_block` | Block + BlockEntity | Shop GUI; datapack-driven pages. |
| `catalyst:daily_exchange_block` | Block + BlockEntity | Auto-sell input items for currency output; hopper I/O. |
| `catalyst:catalyst_core_block` | Block | Decorative/crafting; used in Lunar and Cobble Bag recipes. |
| `catalyst:catalyst_crystal` | Item | Quest/reward item; used in crescent and PokeBag crafting. |
| Currency items | Item | Amethyst/netherite coins (and piles/blocks); PokeDollar/Master Token chain when Cobble Core present. |
---

## Configuration

All configuration is managed through NeoForge's config system at `config/catalyst-common.toml`:

| Key | Default | Description |
|-----|---------|-------------|
| `economy.default_currency_profile` | `core_default` | Active currency profile from datapacks |
| `economy.mode` | `PHYSICAL` | `DIGITAL` (wallet) or `PHYSICAL` (items) |
| `economy.base_currency_item` | `minecraft:emerald` | Fallback currency item |
| `economy.cobble_currency_item` | `catalyst:pokedollar` | Currency when Cobble Core is present |
| `economy.sell_rate_ratio` | `0.4` | Sell price as fraction of buy (0.0-1.0) |
| `economy.exchange_cooldown_seconds` | `60` | Cooldown between exchange transactions |
| `economy.max_transaction_cap` | `1000000` | Max single transaction value |
| `economy.lightman_auto_detect` | `true` | Auto-detect Lightman's Currency |
| `economy.lightman_chain_id` | `main` | LC chain id for transactions |
| `villager.replace_trades` | `false` | Replace emerald trades with currency |
| `villager.trade_currency_mode` | `CURRENCY` | `CURRENCY` or `EMERALD` |
| `market.default_rows` | `6` | Default shop GUI rows |
| `market.log_transactions` | `true` | Log market transactions |
| `features.holograms` | `true` | Enable hologram displays |
| `features.guild_system` | `false` | Enable guild system |
| `features.quest_system` | `false` | Enable quest engine |
| `features.contract_system` | `false` | Enable contract system |
| `features.teleport_integration` | `true` | Enable FTB Essentials buttons |
| `stats.leaderboard_page_size` | `10` | Entries per leaderboard page |
| `stats.track_minecraft_stats` | `true` | Mirror MC stats |

---

## Datapack Structure

```
data/
  catalyst/
    shops/
      starter_shop.json
      pokeballs_shop.json
      medicine_shop.json
    exchange/
      baseline_rates.json
    currency_profiles/
      core_default.json
      cobble_default.json
    quests/
      starter_quest.json
```

See the [Datapack Authoring Guide](datapack-authoring.md) for complete schema documentation and examples.

---

## Compatibility

| Mod | Status | Effect |
|-----|--------|--------|
| Cobblemon | Optional | Unlocks Pokemon reward systems via Cobble Core |
| Cobble Core (cobblecore) | Optional | Switches default currency to PokeDollar |
| Lightman's Currency | Optional | Enables wallet/bank integration |
| FTB Essentials | Optional | Adds teleport buttons to GUI |

---

## For Server Operators

1. Drop `catalyst-0.1.0-SNAPSHOT.jar` into your `mods/` folder
2. Start the server to generate default config
3. Place your datapacks in `world/datapacks/` for custom shops, prices, and quests
4. Use `/catalyst reload` to hot-reload datapack changes without restart
5. Enable `replace_villager_trades` if you want a currency-first economy






