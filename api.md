# Enka.Network - API

## Getting Started

You can fetch JSON-data by doing request via URL - `https://enka.network/u/[UID]/__data.json` <br />
For example https://enka.network/u/700378769/__data.json

## Data Structure Info

| Name | Description
| :--- | :---------- |
| [playerInfo](#playerinfo) | Profile Info |
| [avatarInfoList](#avatarinfolist) | List of detailed information for every character from showcase |

### playerInfo

<<<<<<< HEAD
For any additional info, check the [Characters Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json), includes IDs and other stuff.
=======
For any additional info, check the [Characters Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json) incuding IDs and other stuff.
>>>>>>> 1ba0ef77a3de712e407310a05facb9f39253fda5

| Name | Description |
| :--- | :--------- | 
| nickname | Player Nickname |
| signature | Profile Signature |
| worldLevel | Player World Level |
| namecardId | Profile Namecard ID |
| finishAchievementNum | Number of Completed Achievements |
| towerFloorIndex | Abyss Floor |
| towerLevelIndex | Abyss Floor's Level |
| [showAvatarInfoList](#showavatarinfolist-avatarinfo) | List of Character IDs and Levels |
| showNameCardIdList | List of Namecard IDs |
| profilePicture.avatarID | Character ID of Profile Picture |

#### showAvatarInfoList - AvatarInfo

| Name | Description |
| :--- | :--------- | 
| avatarId | Character ID |
| level | Character Level |

### avatarInfoList

| Name | Description |
| :--- | :---------- |
| avatarID | Character ID |
| [propMap](#propmap) | Character Info Properties List |
| fightPropMap -> `{id: value}` |  Map of Character's Combat Properties. <br />Check the [Definitions for IDs](#fightprop)|
| skillDepotId | Character Skill Set ID <br />[Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`|
| inherentProudSkillList | List of Unlocked Skill Ids <br />[Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| Map of Skill Levels <br /> [Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| [equipList](#equiplist-equip) | List of Equipments: Weapon, Ariftacts |
| fetterInfo.expLevel  | Character Friendship Level |

#### propMap

| Name | Description |
| :--- | :--------- |
| type | ID of Property Type, Check the [Definitions for IDs](#prop) |
| ival | Ignore it |
| val  | Value of Property |

#### equipList - Equip

| Name | Description |
| :--- | :--------- |
| itemId | Equipment ID <br /> [Equipments Data](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/GadgetExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Weapon Only]` | Weapon Base Info  |
| [reliquary](#reliquary) `[Artifact Only]` | Artifact Base Info  |
| [flat](#flat) | Detailed Info of Equipment |

#### weapon

| Name | Description |
| :--- | :---------- |
| level | Weapon Level |
| promoteLevel | Weapon Ascension Level |
| affixMap | Weapon Refinement Level `[0-4]` |


### reliquary

| Name | Description |
| :--- | :---------- |
| level | Artifact Level `[1-21]` |
| mainPropId | Artifact Main Stat ID <br /> [MainProps Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |

### flat

<<<<<<< HEAD
For any additional info about names, descriptions and etc, check the [TextMap Data](https://github.com/Dimbreath/GenshinData/tree/master/TextMap), includes any languages supported by game.
=======
For any additional info about names, descriptions and etc, check the [TextMap Data](https://github.com/Dimbreath/GenshinData/tree/master/TextMap) including any languages supported by game.
>>>>>>> 1ba0ef77a3de712e407310a05facb9f39253fda5

| Name | Description |
| :--- | :---------- |
| nameTextHashMap | Hash for Equipment Name |
| setNameTextHashMap `[Artifact Only]`| Hash for Artifact Set Name |
| rankLevel | Rarity Level of Equipment |
| weaponStats `[Weapon Only]`| Weapon Stats |
| reliquaryMainstat `[Artifact Only]` | Artifact Main Stat |
| reliquarySubstats `[Artifact Only]` | Arifact Substats |
| itemType | Equipment Type: Weapon or Artifact|
| icon | Equipment Icon Name |
| [equipType](#equiptype) `[Artifact Only]` | Artifact Type |

## Definitions

### Prop

| Type | Description |
| :--: | :---------- |
| 1001 | XP |
| 1002 | Ascension | 
| 4001 | Level |

### FightProp

| Type | Description |
| :--: | :---------- |
| 1 | Base HP |
| 4 | Base ATK |
| 7 | Base DEF |
| 20 | CRIT Rate |
| 22 | CRIT DMG |
| 23 | Energy Recharge |
| 26 | Healing Bonus |
| 27 | Incoming Healing Bonus |
| 28 | Elemental Mastery |
| 29 | Physical RES |
| 30 | Physical DMG Bonus |
| 40 | Pyro DMG Bonus |
| 41 | Electro DMG Bonus |
| 42 | Hydro DMG Bonus |
| 44 | Anemo DMG Bonus |
| 45 | Geo DMG Bonus |
| 46 | Cryo DMG Bonus |
| 50 | Pyro RES |
| 51 | Electro RES |
| 52 | Hydro RES |
| 53 | Dendro RES |
| 54 | Anemo RES |
| 55 | Geo RES |
| 56 | Cryo RES |
| 70 | Pyro Enegry Cost |
| 71 | Electro Energy Cost |
| 72 | Hydro Energy Cost |
| 73 | Dendro Energy Cost |
| 74 | Anemo Energy Cost |
| 75 | Cryo Energy Cost |
| 76 | Geo Energy Cost |
| 2000 | Max HP |
| 2001 | ATK |
| 2002 | DEF |

### EquipType
