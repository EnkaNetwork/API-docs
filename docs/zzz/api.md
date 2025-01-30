# Enka.Network API - Zenless Zone Zero 

## Table of Content

- [Data Structure](#data-structure)
- [Definitions](#definitions)
- [Formulas](#formulas)
- [Icons and Images](#icons-and-images)
- [Localizations](#localizations)

## Data Structure

| Name | Description |
| :--- | :---------- |
| uid | Player UID |
| ttl | ttl |
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
| Title | TitleId |
| CallingCardId | Namecard ID |
| AvatarId | Main Character ID (Wise or Belle) |

#### ShowcaseDetail

| Name | Description |
| :--- | :--------- | 
| [AvatarList](#AvatarList) | List of Characters |

### AvatarList

| Name | Description |
| :--- | :--------- | 
| Exp | Agent Exp |
| Level | Agent Level |
| PromotionLevel | Agent promotion level |
| TalentLevel | Agent mindscape level |
| SkinId | Agent Skin ID |
| CoreSkillEnhancement | Core Skill Unlocked Enhancements - A, B, C, D, F, G |
| TalentToggleList | Mindscape Cinema visual toggles |
| WeaponEffectState | W-Engine signature special effect state `[0: OFF, 1: ON]` |
| ClaimedRewardList | Agent promotion rewards |
| ObtainmentTimestamp | Agent obtainment timestamp |
| [Weapon](#weapon) | Equipped W-Engine | 
| SkillLevelList | Agent skill level dict, check the definitions for indexes |
| [EquippedList](#EquippedList) | List of Disc Drives with UID |

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
| IsAvailable | Available state of Drive Disc |
| IsLocked | Locked state of W-Engine |
| UpgradeLevel | W-Engine Phase Level |

#### EquippedList
| Name | Description |
| :--- | :--------- | 
| Slot | Slot index |
| [Equipment](#equipment) | Equipment data |

#### Equipment

| Name | Description |
| :--- | :--------- | 
| Uid | Drive Disc UID |
| Id | Disc Drive ID |
| Exp | Exp |
| Level | Drive Disc Level `[0-15]` |
| BreakLevel | Amount of random stat procs |
| IsLocked | Lockelock marked state of Drive Disc |
| IsAvailable | Available state of Drive Disc |
| IsTrash | Trash marked state of Drive Disc |
| MainStatList | Drive Disc Main Stat, check [Stat](#Stat) for additional info |
| RandomPropertyList | Drive Discs Substat List, check [Stat](#Stat) for additional info |

#### Stat

Check [Formulas](#formulas) to see how to get actual values from base values

| Name | Description |
| :--- | :---------- | 
| PropertyValue | Property Base Value |
| PropertyId | Property ID, check the [definitions](#property-id) for IDs |
| PropertyLevel | Amount of rolls, only matters if substat |

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
| 12202 | Break% |
| 13101 | Def `[Base]` |
| 13102 | Def% |
| 13103 | Def `[Flat]` |
| 20101 | Crit Rate `[Base]` |
| 20103 | Crit Rate `[Flat]` |
| 21101 | Crit DMG `[Base]` |
| 21103 | CritDMG `[Flat]` |
| 23101 | Pen Ratio `[Base]` |
| 23103 | Pen Ratio `[Flat]` |
| 23201 | PEN  `[Base]` |
| 23203 | PEN `[Flat]` |
| 30501 | Energy Regen `[Base]` |
| 30502 | Energy Regen% |
| 30503 | Energy Regen `[Flat]` |
| 31201 | Anomaly Power `[Base]` |
| 31203 | Anomaly Power `[Flat]` |
| 31401 | Anomaly Mastery `[Base]` |
| 31402 | Amonaly Mastery% |
| 31403 | Anomaly Mastery `[Flat]` |
| 31501 | Physical DMG Bonus `[Base]` |
| 31503 | Physical DMG Bonus `[Flat]` |
| 31601 | Fire DMG Bonus `[Base]` |
| 31603 | Fire DMG Bouns `[Flat]` |
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

## Formulas

### Drive Disc

#### Main Stat
```Result = MainStat.PropertyValue * (MainStat.PropertyValue * Level * RarityScale)```
#### Sub Stat
```Result = PropertyValue * PropertyLevel```

#### Rarity Scales
| Rarity | Scale |
| :----- | :------ |
| 4 | 0.2 |
| 3 | 0.25 |
| 2 | 0.2 |


### W-Engine 

#### Main Stat

```Result = MainStat.BaseValue * (1 + 0.1568166666666667 * Level + 0.8922 * BreakLevel)```

#### Sub Stat

```Result = SubStat.BaseValue * (1 + 0.3 * BreakLevel) ```

## Icons and Images

All the icon names are included in the parsed data at [API-docs/store/zzz](https://github.com/EnkaNetwork/API-docs/tree/master/store/zzz).

For any additional info, check the [ZenlessData](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/) repository.

## Localizations

For the names used in Enka.Network, refer to [store/zzz/locs.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/locs.json)

For any additional info about names, descriptions and etc, check the [TextMap Data](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/TextMap), only includes languages supported by the game.

## Wrappers

Currently none
