# PersoLootRoll

> **Midnight Compatibility Fork** — This is an unofficial maintenance fork updated to work with **World of Warcraft: Midnight** (Interface 120000+). All credit for the original addon goes to [shrugal](https://gitlab.com/shrugal/PersoLootRoll), the original creator and author. I ([creazy231](https://github.com/creazy231)) only made it compatible with the latest WoW patches — I do not take credit for the core concept or implementation.

This addon brings back the Need-Greed loot system and makes it work with the new loot rules introduced in Midnight. The goal is to make asking for, giving away and trading loot easy and straight forward, in PUGs as well as organized guild groups. Everything is designed to be as streamlined and out of your way as possible, so you can focus on playing the game instead of manually checking and keeping track of items you and others want.

Get the original addon from [Curse](https://www.curseforge.com/wow/addons/persolootroll) or [WoWInterface](http://www.wowinterface.com/downloads/info24667-PersoLootRoll.html).

## Features

### <span style="color: red">Roll on tradable personal loot from others</span>
Whenever an item drops for someone in your group that might be an upgrade for you, you will get a good old Need-Greed-Pass roll window (remember those? :P)
to decide if you want it or not. Rolling "Need" or "Greed" will add the item to your actions list (see below) for easy asking and trading, and optionally
automatically send a whisper message to the owner asking for the item. If the owner uses PLR or another compatible addon as well, then all this happens in
the background without need for whisper messages.

<p align="center">
  <img src="https://imgur.com/GzgQjvk.jpg">
</p>

### <span style="color: red">Give away loot you don't want</span>
You also get a similar Keep-Greed-GiveAway roll window when you loot something that your party members might be interested in. If you choose "Greed" or
"Give Away", then PLR offers the item to your group through party chat and handles accepting bids, picking a winner and trading the item for you. If you
choose "Keep" on the other hand, then it'll automatically answer incoming whisper requests with "I need that myself".

<p align="center">
  <img src="https://imgur.com/8CkGcVE.jpg">
</p>

### <span style="color: red">Keep track of pending actions, and easily complete them</span>
All your pending actions (asking for loot, trading, ...) will be shown in a simple list on screen, along with buttons for completing them.

<p align="center">
  <img src="https://imgur.com/3eXcSaX.jpg">
</p>

### <span style="color: red">Only bothers you when it actually makes sense</span>
PLR is especially smart when it comes to choosing the right items for you. Many factors are checked before you are asked to roll on or give away an item,
e.g. if it is actually tradable, what ilvl you and your party members have equipped, class restrictions, trinket type, and so on. It will only ask you to
decide when it actually makes sense, while making sure you don't miss any loot you might be interested in.

### <span style="color: red">Works for PUGs, organized groups and even masterlooting</span>
It works great for randomly giving away loot in PUGs and organized groups, but it also has a masterloot mode where one person decides who should get which
item. The masterlooter can also configure things like custom answers and a loot council.

<p align="center">
  <img src="https://imgur.com/njPScmx.jpg">
</p>

### <span style="color: red">Can be configured to your liking</span>
Just about everything can be easily tweaked in the options menu. This includes whether or not to send messages to other players and customizing them, what items
are considered "useful" (e.g. only certain specs or transmog missing) and which parts of the UI you want to see. See
[this Wiki page](https://www.curseforge.com/wow/addons/persolootroll/pages/options) for details.

### <span style="color: red">Works with other addons</span>
This includes the popular loot roll addon Personal Loot Helper, [Pawn](https://www.curseforge.com/wow/addons/persolootroll/pages/options) to only roll on
stuff that actually has your preferred stats, and [EPGP](https://www.curseforge.com/wow/addons/persolootroll/pages/options/masterloot) to give away loot
based on PR value or credit GP for awarded items.

More details can be found [here](https://www.curseforge.com/wow/addons/persolootroll/pages/home).

## Commands
Use /plr or /PersoLootRoll to open the rolls overview window, manually start rolling for items in your bag etc.

- `/plr`: Open rolls window
- `/plr help`: Print this help message
- `/plr roll [item]* (<owner> <timeout>)`: Start a roll for one or more item(s)
- `/plr bid [item] (<owner> <bid>)`: Bid for an item from another player
- `/plr trade (<player>)` Trade with the given player or your current target
- `/plr test` Start a test roll (only you will see it)
- `/plr options`: Open options window
- `/plr config`: Change settings through the command line
- `/plr log` Show log

Development commands:
- `/plr debug` Toggle debug mode
- `/plr trinkets` Generate and show a new list of trinkets
- `/plr instances` Generate and show a new list of instances

*Legend: `[..]` = item link, `*` = one or more times, `(..)` = optional*

## Translation
PLR is translated (incl. chat messages) to

- English: 100%
- German: 100%
- Traditional Chinese: 100% (by [BNS333](https://www.curseforge.com/members/bns333))
- Simplified Chinese: 100% (by [john_yasen](https://wow.curseforge.com/members/john_yasen))
- Spanish: 75% (by [jolugon](https://wow.curseforge.com/members/jolugon), [sinnkin](https://wow.curseforge.com/members/sinnkin))
- Russian: 60% (by [Voopie](https://wow.curseforge.com/members/voopie))
- French: 20% (by [Harkann](https://wow.curseforge.com/members/harkann))

 If you want to help translate it to your language or correct translation errors you found then please visit the
 [Curseforge Translation section](https://www.curseforge.com/wow/addons/persolootroll/localization) and also check out
 [this wiki page](https://www.curseforge.com/wow/addons/persolootroll/pages/translation) for some tips.

## Midnight Compatibility Changes

The following fixes were applied to make this addon work with WoW: Midnight (patch 12.0):

- Guard outbound addon messages against Midnight communication lockdown (`C_ChatInfo.InChatMessagingLockdown`)
- Fix group chat announce: `LE_PARTY_CATEGORY_INSTANCE` global removed in Midnight
- Register `LEGACY_LOOT_RULES_CHANGED` event to refresh addon state on loot mode changes
- Replace deprecated `GetRaidRosterInfo` with unit token iteration across all call sites
- Remove dead Personal-to-NeedBeforeGreed loot conversion (personal loot no longer exists in Midnight raids)
- Fix M+ dropped item count: `C_ChallengeMode.GetCompletionInfo` removed in Midnight; defaults to 2 items
- Restore missing embedded libraries (LibUtil, LibRealmInfo, LibDataBroker-1.1)
- Activate addon for Need Before Greed loot without masterlooter (legacy loot rules in Midnight use NBG by default)
- Guard `GroupLootContainer_RemoveFrame` SecureHook: function may not exist if legacy loot UI is disabled
- Guard `C_DelvesUI.HasActiveDelve` against nil in case the Delves API signature changed

## Development
The original project is fully open-source and the source code can be found on [GitLab](https://gitlab.com/shrugal/PersoLootRoll). This Midnight fork is maintained on [GitHub](https://github.com/creazy231/PersoLootRoll).

### Bugs
For bugs related to Midnight compatibility, please open an issue on the [GitHub fork](https://github.com/creazy231/PersoLootRoll/issues). For issues with the original addon, use the [original issue tracker](https://gitlab.com/shrugal/PersoLootRoll/issues). Also, in order to better identify the problem please type in `/plr log` right after the bug happened, and add the result to your issue/post.

### Features
You can suggest new features and vote on existing suggestions [here](https://persolootroll.featureupvote.com/).

### Build
To build a package from source you need to copy `.release/.env.example` to `.release/.env`, fill in the relevant infos (projects IDs and API tokens) and then
run `.release/release.sh`. The completed file(s) will be available in the `.release` folder. More information about possible arguments to the
release script can be found [here](https://gitlab.com/shrugal/wow-packager).

### Test
You can run tests in-game or on the commandline. For in game tests install the [WoWUnit](https://github.com/Jaliborc/WoWUnit) addon so the tests will automatically
run on login. For the CLI install the LUA binary for your OS and then run `lua Test.lua` to test the current working dir, or `lua Test.lua -b` to run the
tests against a finished build.

### Donate
Click on the "Donate" button if you want to support the development of this addon or just buy me a beer. Always appreciated, never required!

[![Donate](http://www.wowinterface.com/images/paypalSM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=H3EE7MDA5XFCW)
