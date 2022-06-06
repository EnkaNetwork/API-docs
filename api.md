# Enka.Network - API

## Getting Started

You can fetch JSON-data by doing request via URL - `https://enka.network/u/[UID]/__data.json` <br />
For example https://enka.network/u/700378769/__data.json

## Data Structure Info

| Name | Description |
| :--- | :---------- |
| [playerInfo](#playerinfo) | Profile Info |
| [avatarInfoList](#avatarinfolist) | List of detailed information for every character from showcase |

### playerInfo

| Name | Description |
| :--- | :--------- | 
| nickname | Player Nickname |
| signature | Profile Signature |
| worldLevel | Player World Level |
| namecardId | Profile Namecard ID |
| finishAchievementNum | Number of Completed Achievements |
| towerFloorIndex | Abyss Floor |
| towerLevelIndex | Abyss Floor's Level |
| [showAvatarInfoList](#showavatarinfolist) | List of Character IDs and Levels |
| showNameCardIdList | List of Namecard IDs |
| profilePicture.avatarID | Character ID of Profile Picture |

#### showAvatarInfoList

| Name | Description |
| :--- | :--------- | 
| avatarId | Character ID |
| level | Character Level |

### avatarInfoList

| Name | Description |
| :--- | :---------- |
| avatarID | Character ID |
| [propMap](#propmap) | Character Info Properties List |
| fightPropMap | Map of Character's Combat Properties  `{id: value}`. [Definitions for IDs](#fightprop)|
| skillDepotId | Charater Skill Set ID. [Additional Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"id"`|
| inherentProudSkillList | List of Unlocked Skill Ids. [Additional Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 

### propMap

| Name | Description |
| :--- | :--------- |
| type | ID of Property Type, Check the [Definitions for IDs](#prop) |
| ival | Ignore it |
| val  | Value of Property |

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