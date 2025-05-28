# Enka.Network API - Genshin Impact

## Table of Content

- [Data Structure Info](#data-structure)
- [Definitions](#definitions)
- [Icons and Images](#icons-and-images)
- [Localizations](#localizations)


## Data Structure

| Name | Description |
| :--- | :---------- |
| [playerInfo](#playerinfo) | Profile Info |
| [avatarInfoList](#avatarinfolist) | List of detailed information for every character from showcase |

### playerInfo

For basic data of characters by ID, go to [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
For any additional info, check the [Characters Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Name | Description |
| :--- | :--------- | 
| nickname | Player Nickname |
| signature | Profile Signature |
| worldLevel | Player World Level |
| namecardId | Profile Namecard ID |
| finishAchievementNum | Number of Completed Achievements |
| towerFloorIndex | Abyss Floor |
| towerLevelIndex | Abyss Floor's Chamber |
| [showAvatarInfoList](#showavatarinfolist) | List of Character IDs and Levels |
| showNameCardIdList | List of Namecard IDs |
| profilePicture.avatarId | Character ID of Profile Picture |

#### showAvatarInfoList

| Name | Description |
| :--- | :--------- | 
| avatarId | Character ID |
| level | Character Level |
| costumeId | ID of character's skin. Check `"Costumes"` in [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) |

### avatarInfoList

For basic data of characters by ID, go to [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
For any additional info, check the [Characters Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Name | Description                                                                                                                                                                               |
| :--- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| avatarID | Character ID                                                                                                                                                                              |
| talentIdList | List of Constellation IDs <br /> There is no data if 0 Constellation                                                                                                                      |
| [propMap](#propmap) | Character Info Properties List                                                                                                                                                            |
| fightPropMap -> `{id: value}` | Map of Character's Combat Properties. <br />Check the [Definitions for IDs](#fightprop)                                                                                                   |
| skillDepotId | Character Skill Set ID <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`                      |
| inherentProudSkillList | List of Unlocked Skill Ids <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| Map of Skill Levels <br /> [Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"`     |
| [equipList](#equiplist) | List of Equipments: Weapon, Ariftacts                                                                                                                                                     |
| fetterInfo.expLevel  | Character Friendship Level                                                                                                                                                                |

#### propMap

| Name | Description |
| :--- | :--------- |
| type | ID of Property Type, Check the [Definitions for IDs](#prop) |
| ival | Ignore it |
| val  | Value of Property |

#### equipList

| Name | Description |
| :--- | :--------- |
| itemId | Equipment ID <br /> [Artifacts Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Weapons Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Weapon Only]` | Weapon Base Info  |
| [reliquary](#reliquary) `[Artifact Only]` | Artifact Base Info  |
| [flat](#flat) | Detailed Info of Equipment |

#### weapon

For any additional info about weapons, check the [Weapons Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)

| Name | Description |
| :--- | :---------- |
| level | Weapon Level |
| promoteLevel | Weapon Ascension Level |
| affixMap | Weapon Refinement Level `[0-4]` |


#### reliquary

For any additional info about artifacts, check the [Artifacts Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json)

| Name | Description |
| :--- | :---------- |
| level | Artifact Level `[1-21]` |
| mainPropId | Artifact Main Stat ID <br /> [MainProps Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |
| appendPropIdList | List of IDs of the artifact substats <br /> [AppendProp Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

#### flat

| Name | Description |
| :--- | :---------- |
| nameTextHashMap | Hash for Equipment Name <br /> Check [Localizations](#localizations) |
| setNameTextHashMap `[Artifact Only]`| Hash for Artifact Set Name <br /> Check [Localizations](#localizations)|
| rankLevel | Rarity Level of Equipment |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | Artifact Main Stat |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | List of Artifact Substats |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Weapon Only]`| List of Weapon Stat: Base ATK, Substat |
| [itemType](#itemtype) | Equipment Type: Weapon or Artifact |
| icon | Equipment Icon Name <br /> [Icon name usage](#icons-and-images)|
| [equipType](#equiptype) `[Artifact Only]` | Artifact Type |

#### reliquaryMainstat, reliquarySubstats, weaponStats

| Name | Description |
| :--- | :---------- |
| mainPropId / appendPropID | Equipment Append Property Name. Check the [Definitions for Names](#appendprop)|
| propValue | Property Value |

## Definitions

### Prop

| Type | Description |
| :--: | :---------- |
| 1001 | XP |
| 1002 | Ascension | 
| 4001 | Level |

### FightProp

| Type | Description                                       |
| :--: |:--------------------------------------------------|
| 1 | Base HP                                           |
| 2 | HP                                                |
| 3 | HP%                                               |
| 4 | Base ATK                                          |
| 5 | ATK                                               |
| 6 | ATK%                                              |
| 7 | Base DEF                                          |
| 8 | DEF                                               |
| 9 | DEF%                                              |
| 10 | Base SPD                                          |
| 11 | SPD%                                              |
| 20 | CRIT Rate                                         |
| 22 | CRIT DMG                                          |
| 23 | Energy Recharge                                   |
| 26 | Healing Bonus                                     |
| 27 | Incoming Healing Bonus                            |
| 28 | Elemental Mastery                                 |
| 29 | Physical RES                                      |
| 30 | Physical DMG Bonus                                |
| 40 | Pyro DMG Bonus                                    |
| 41 | Electro DMG Bonus                                 |
| 42 | Hydro DMG Bonus                                   |
| 43 | Dendro DMG Bonus                                  |
| 44 | Anemo DMG Bonus                                   |
| 45 | Geo DMG Bonus                                     |
| 46 | Cryo DMG Bonus                                    |
| 50 | Pyro RES                                          |
| 51 | Electro RES                                       |
| 52 | Hydro RES                                         |
| 53 | Dendro RES                                        |
| 54 | Anemo RES                                         |
| 55 | Geo RES                                           |
| 56 | Cryo RES                                          |
| 70 | Pyro Energy Cost                                  |
| 71 | Electro Energy Cost                               |
| 72 | Hydro Energy Cost                                 |
| 73 | Dendro Energy Cost                                |
| 74 | Anemo Energy Cost                                 |
| 75 | Cryo Energy Cost                                  |
| 76 | Geo Energy Cost                                   |
| 77 | Maximum Special Energy                            |
| 78 | Special Energy Cost                               |
| 80 | Cooldown reduction                                |
| 81 | Shield Strength                                   |
| 1000 | Current Pyro Energy                               |
| 1001 | Current Electro Energy                            |
| 1002 | Current Hydro Energy                              |
| 1003 | Current Dendro Energy                             |
| 1004 | Current Anemo Energy                              |
| 1005 | Current Cryo Energy                               |
| 1006 | Current Geo Energy                                |
| 1007 | Current Special Energy                            |
| 1010 | Current HP                                        |
| 2000 | Max HP                                            |
| 2001 | ATK                                               |
| 2002 | DEF                                               |
| 2003 | SPD                                               |
| 3025 | Elemental reaction CRIT Rate                      |
| 3026 | Elemental reaction CRIT DMG                       |
| 3027 | Elemental reaction (Overloaded) CRIT Rate         |
| 3028 | Elemental reaction (Overloaded) CRIT DMG          |
| 3029 | Elemental reaction (Swirl) CRIT Rate              |
| 3030 | Elemental reaction (Swirl) CRIT DMG               |
| 3031 | Elemental reaction (Electro-Charged) CRIT Rate    |
| 3032 | Elemental reaction (Electro-Charged) CRIT DMG     |
| 3033 | Elemental reaction (Superconduct) CRIT Rate       |
| 3034 | Elemental reaction (Superconduct) CRIT DMG        |
| 3035 | Elemental reaction (Burn) CRIT Rate               |
| 3036 | Elemental reaction (Burn) CRIT DMG                |
| 3037 | Elemental reaction (Frozen (Shattered)) CRIT Rate |
| 3038 | Elemental reaction (Frozen (Shattered)) CRIT DMG  |
| 3039 | Elemental reaction (Bloom) CRIT Rate              |
| 3040 | Elemental reaction (Bloom) CRIT DMG               |
| 3041 | Elemental reaction (Burgeon) CRIT Rate            |
| 3042 | Elemental reaction (Burgeon) CRIT DMG             |
| 3043 | Elemental reaction (Hyperbloom) CRIT Rate         |
| 3044 | Elemental reaction (Hyperbloom) CRIT DMG          |
| 3045 | Base Elemental reaction CRIT Rate                 |
| 3046 | Base Elemental reaction CRIT DMG                  |

### ItemType

| Name | Description |
| :--- | :---------- |
| ITEM_WEAPON | Weapon |
| ITEM_RELIQUARY | Artifact |

### EquipType

| Name | Description |
| :--- | :---------- |
| EQUIP_BRACER | Flower |
| EQUIP_NECKLACE | Feather |
| EQUIP_SHOES | Sands | 
| EQUIP_RING | Goblet |
| EQUIP_DRESS | Circlet |

### AppendProp

| Name | Description        |
| :--- |:-------------------|
| FIGHT_PROP_BASE_ATTACK `[Weapon]` | Base ATK           |
| FIGHT_PROP_HP | Flat HP            |
| FIGHT_PROP_ATTACK | Flat ATK           |
| FIGHT_PROP_DEFENSE | Flat DEF           |
| FIGHT_PROP_HP_PERCENT | HP%                |
| FIGHT_PROP_ATTACK_PERCENT | ATK%               |
| FIGHT_PROP_DEFENSE_PERCENT | DEF%               | 
| FIGHT_PROP_CRITICAL | Crit RATE          |
| FIGHT_PROP_CRITICAL_HURT | Crit DMG           |
| FIGHT_PROP_CHARGE_EFFICIENCY | Energy Recharge    |
| FIGHT_PROP_HEAL_ADD | Healing Bonus      |
| FIGHT_PROP_ELEMENT_MASTERY | Elemental Mastery  |
| FIGHT_PROP_PHYSICAL_ADD_HURT | Physical DMG Bonus |
| FIGHT_PROP_FIRE_ADD_HURT | Pyro DMG Bonus     |
| FIGHT_PROP_ELEC_ADD_HURT | Electro DMG Bonus  |
| FIGHT_PROP_WATER_ADD_HURT | Hydro DMG Bonus    |
| FIGHT_PROP_WIND_ADD_HURT | Anemo DMG Bonus    |
| FIGHT_PROP_ICE_ADD_HURT | Cryo DMG Bonus     |
| FIGHT_PROP_ROCK_ADD_HURT | Geo DMG Bonus      |
| FIGHT_PROP_GRASS_ADD_HURT | Dendro DMG Bonus   |

## Icons and Images

You can get icons of characters, weapons and artifacts via Enka, by URL `https://enka.network/ui/[icon_name].png`.  
Usually icon name starts with `"UI_"` or `"Skill_"` for [characters talents](#characters-talents-and-consts).  
For example https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png.

### Weapons and Artifacts

Go to [flat](#flat) and look for `icon`.

### Characters, Talents and Consts

Go to [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) and look for anything related to "UI_XXXXXX" or "Skill_XXXXXX" by character ID.

## Localizations

You may notice `"NameTextMapHash"` in [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json), `"nameTextHashMap"` and `"setNameTextHashMap"` at [flat](#flat) that could be used as a key to get basic localization data of characters, weapons and artifacts from [store/loc.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/loc.json).  
Also you can get localization data of [AppendProp](#appendprop) by using property name as a key - `"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"` etc.

For any additional info about names, descriptions and etc, check the [TextMap Data](https://gitlab.com/Dimbreath/AnimeGameData/-/tree/master/TextMap), only includes languages supported by game.

## Wrappers

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Rust - https://github.com/eratou/enkanetwork-rs - [eratou](https://github.com/eratou)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)

Python - https://github.com/seriaati/enka-py - [seriaati](https://github.com/seriaati)

Java - https://github.com/kazuryyx/EnkaNetworkAPI - [kazury](https://github.com/kazuryyx)

C# - https://github.com/aliafuji/EnkaDotnet - [aliafuji](https://github.com/aliafuji)

C# - https://github.com/Carried520/EnkaSharp - [Carried520](https://github.com/Carried520)