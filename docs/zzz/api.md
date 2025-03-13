# Enka.Network API - Zenless Zone Zero 

## Table of Content

- [Important Notes](#important-notes)
- [Data Structure](#data-structure)
- [Definitions](#definitions)
- [Formulas](#formulas)
- [Icons and Images](#icons-and-images)
- [Localizations](#localizations)

## Important Notes

- UID for weapons and discs are unique to that specific item in a game account. They will persist across upgrades to the disc/weapon and can be used to deduplicate them across multiple queries if you want to keep track of all equipment on the account that was seen in the showcase.

- Responses will always contain a minimal amount of data. To get some basic info, you have to work with parsed JSON files in [API-docs/store/zzz](https://github.com/EnkaNetwork/API-docs/tree/master/store/zzz). If you need more data to work with, please refer to the [ZenlessData](https://git.mero.moe/dimbreath/ZenlessData) repository maintained by Dimbreath.

- While working with character, weapon and disc stats, you should refer to [Formulas](#formulas). Big thanks to Mero for reverse engineering and getting actual formulas used in the game.

---

## Data Structure

| Name | Description |
| :--- | :---------- |
| uid | Player UID |
| ttl | Seconds until next update |
| [PlayerInfo](#playerinfo) | Profile Info |

#### PlayerInfo

| Name | Description |
| :--- | :--------- | 
| [SocialDetail](#socialdetail) | Social info details |
| [ShowcaseDetail](#showcasedetail) | Showcase details |

#### SocialDetail

| Name | Description |
| :--- | :--------- | 
| Desc | Profile signature |
| [ProfileDetail](#profiledetail) | Profile details |
| [MedalList](#medallist) | List of Badges | 

#### MedalList
| Name | Description |
| :--- | :--------- |
| MedalType | [Badge Type](#badge-type) |
| Value | Progress Number |
| MedalIcon | Icon Id | 

### ProfileDetail

| Name | Description |
| :--- | :--------- |
| Uid | Player UID |
| Nickname | Player Nickname |
| ProfileId | Profile Picture ID |
| Level | Inter-Knot Level | 
| Title | Title ID |
| CallingCardId | Namecard ID |
| AvatarId | Main Character ID (Wise or Belle) |

#### ShowcaseDetail

| Name | Description |
| :--- | :--------- | 
| [AvatarList](#AvatarList) | List of Characters |

### AvatarList

| Name | Description |
| :--- | :--------- | 
| Id | Agent ID |
| Exp | Agent Exp |
| Level | Agent Level |
| PromotionLevel | Agent promotion level |
| TalentLevel | Agent mindscape level |
| SkinId | Agent Skin ID |
| CoreSkillEnhancement | Core Skill Unlocked Enhancements - A, B, C, D, E, F |
| TalentToggleList | Mindscape Cinema visual toggles |
| WeaponEffectState | W-Engine signature special effect state `[0: None, 1: OFF, 2: ON]` |
| IsHidden | Hidden state of Agent |
| ClaimedRewardList | Agent promotion rewards |
| ObtainmentTimestamp | Agent obtainment timestamp |
| [Weapon](#weapon) | Equipped W-Engine | 
| SkillLevelList | Agent skill level dict, check the [definitions](#skills) for indexes |
| [EquippedList](#EquippedList) | List of Drive Discs |

Note: If the agent has unlocked mindscape 3, increase all skill levels by 2. If the agent has unlocked mindscape 5, increase all skill levels by 2 on the base of the previous increase (4 total).

#### Weapon

Check [Formulas](#formulas) to see how to get actual values from base values
For more info, refer to [store/zzz/weapons.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/weapons.json)

| Name | Description |
| :--- | :--------- | 
| Uid | W-Engine UID |
| Id | W-Engine ID |
| Exp | W-Engine Exp |
| Level | W-Engine Level |
| BreakLevel | W-Engine modification Level |
| UpgradeLevel | W-Engine Phase Level |
| IsAvailable | Available state of W-Engine |
| IsLocked | Locked state of W-Engine |

#### EquippedList
| Name | Description |
| :--- | :--------- | 
| Slot | Slot index |
| [Equipment](#equipment) | Equipment data |

#### Equipment

| Name | Description |
| :--- | :--------- | 
| Uid | Drive Disc UID |
| Id | Drive Disc ID |
| Exp | Exp |
| Level | Drive Disc Level `[0-15]` |
| BreakLevel | Amount of random stat procs |
| IsLocked | Lock marked state of Drive Disc |
| IsAvailable | Available state of Drive Disc |
| IsTrash | Trash marked state of Drive Disc |
| MainStatList | Drive Disc Main Stat, check [Stat](#Stat) for additional info |
| RandomPropertyList | Drive Disc Substat List, check [Stat](#Stat) for additional info |

Note: Rarity of drive discs can be found in [store/zzz/equipment.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/zzz/equipments.json).

#### Stat

Check [Formulas](#formulas) to see how to get actual values from base values

| Name | Description |
| :--- | :---------- | 
| PropertyValue | Property Base Value |
| PropertyId | Property ID, check the [definitions](#property-id) for IDs |
| PropertyLevel | Amount of rolls, only matters if substat |

---

## Definitions

### Property Id

Refer to the table below and [store/zzz/property.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/property.json) for more info.

| Type | Description|
| :--- | :--------- | 
| 11101 | HP `[Base]` |
| 11102 | HP% |
| 11103 | HP `[Flat]` |
| 12101 | ATK `[Base]` |
| 12102 | ATK% |
| 12103 | ATK `[Flat]` |
| 12201 | Impact `[Base]` |
| 12202 | Impact% |
| 13101 | Def `[Base]` |
| 13102 | Def% |
| 13103 | Def `[Flat]` |
| 20101 | Crit Rate `[Base]` |
| 20103 | Crit Rate `[Flat]` |
| 21101 | Crit DMG `[Base]` |
| 21103 | Crit DMG `[Flat]` |
| 23101 | Pen Ratio `[Base]` |
| 23103 | Pen Ratio `[Flat]` |
| 23201 | PEN  `[Base]` |
| 23203 | PEN `[Flat]` |
| 30501 | Energy Regen `[Base]` |
| 30502 | Energy Regen% |
| 30503 | Energy Regen `[Flat]` |
| 31201 | Anomaly Proficiency `[Base]` |
| 31203 | Anomaly Proficiency `[Flat]` |
| 31401 | Anomaly Mastery `[Base]` |
| 31402 | Anomaly Mastery% |
| 31403 | Anomaly Mastery `[Flat]` |
| 31501 | Physical DMG Bonus `[Base]` |
| 31503 | Physical DMG Bonus `[Flat]` |
| 31601 | Fire DMG Bonus `[Base]` |
| 31603 | Fire DMG Bonus `[Flat]` |
| 31701 | Ice DMG Bonus `[Base]` |
| 31703 | Ice DMG Bonus `[Flat]` |
| 31801 | Electric DMG Bonus `[Base]` |
| 31803 | Electric DMG Bonus `[Flat]` |
| 31901 | Ether DMG Bonus `[Base]` |
| 31903 | Ether DMG Bonus `[Flat]` |

### Rarity

| Type | Description |
| :--- | :---------- |
| 4 | S | 
| 3 | A |
| 2 | B |

### Badge Type 

| Type | Description |
| :--- | :---------- |
| 1 | Shiyu Defense |
| 2 | Simulated Battle Tower |
| 3 | Deadly Assault |
| 4 | Simulated Battle Tower - Last Stand | 

### Skills

| Index | Description|
| :--- | :--------- |
| 0 | Basic Attack |
| 1 | Special Attack |
| 2 | Dash  |
| 3 | Ultimate |
| 5 | Core Skill |
| 6 | Assist |

---

## Formulas

#### Agent Stats  

To calculate the base stats of an Agent, you need to use
[store/zzz/avatars.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/zzz/avatars.json).  

- **Base Total:**  
  `BaseTotalValue = BaseProps[PropertyId] + GrowthValue + PromotionValue + CoreEnhancementValue`  
- **Growth:**  
  `GrowthValue = (GrowthProps[PropertyId] * (Avatar.Level - 1)) / 10000`  
- **Promotion:**  
  `PromotionValue = PromotionProps[Avatar.PromotionLevel - 1][PropertyId]`  
- **Core Enhancement:**  
  `CoreEnhancementValue = CoreEnhancementProps[Avatar.CoreSkillEnhancement][PropertyId]`  

**NOTE:** It is recommended to floor results before summing them up with stats from other sources, including those from other sources.  

### Game-Accurate  

#### W-Engine  

To work with W-Engine stats, you need to use:  
  \- [WeaponLevelTemplateTb.json](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/FileCfg/WeaponLevelTemplateTb.json)  
  \- [WeaponStarTemplateTb.json](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/FileCfg/WeaponStarTemplateTb.json)  


- **Main Stat:**  
  `Result = MainStat.PropertyValue * (1 + WeaponLevel.FIELD_XXX / 10000 + WeaponStar.FIELD_YYY / 10000)`  
  **Example (Level 60, BreakLevel 5):**  
  `684 = 46 * (1 + 94090 / 10000 + 44610 / 10000)`  

- **Secondary Stat:**  
  `Result = SecondaryStat.PropertyValue * (1 + WeaponStar.FIELD_ZZZ / 10000)`  
  **Example (BreakLevel 5):**  
  `2400 = 960 * (1 + 15000 / 10000)`  

**NOTE:** The W-Engine **Steel Cushion** `[14102]` was used in the example.  

#### Drive Disc  

To work with Drive Disc stats, you need to use [EquipmentLevelTemplateTb.json](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/FileCfg/EquipmentLevelTemplateTb.json)  
This file determines the Drive Disc value based on its level and rarity.  

- **Main Stat:**  
  `Result = MainStat.PropertyValue * (1 + EquipmentLevel.Field_XXX / 10000)`  
  **Example (Level 14, Rarity 4):**  
  `2090 = 550 * (1 + 28000 / 10000)`  

### Approximate

#### W-Engine 

- **Main Stat:**  
    `Result = MainStat.PropertyValue * (1 + 0.1568166666666667 * Level + 0.8922 * BreakLevel)`  

- **Secondary Stat:**  
    `Result = SecondaryStat.PropertyValue * (1 + 0.3 * BreakLevel)`  

#### Drive Disc

- **Main Stat:**  
`Result = MainStat.PropertyValue + (MainStat.PropertyValue * Level * RarityScale)`
- **Rarity Scales**
| Rarity | Scale |
| :----- | :------ |
| 4 | 0.2 |
| 3 | 0.25 |
| 2 | 0.3 |

---

## Icons and Images

All the icon names are included in the parsed data at [API-docs/store/zzz](https://github.com/EnkaNetwork/API-docs/tree/master/store/zzz).

For any additional info, check the [ZenlessData](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/) repository.

## Localizations

For the names used in Enka.Network, refer to [store/zzz/locs.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/locs.json)

For any additional info about names, descriptions, etc., check the [TextMap Data](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/TextMap), only includes languages supported by the game.

---

## Wrappers

Currently none
