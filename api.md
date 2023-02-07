# Enka.Network - API

## Table of Content

- [Getting Started](#getting-started)
- [Data Structure Info](#data-structure-info)
- [Definitions](#definitions)
- [Icons and Images](#icons-and-images)
- [Localizations](#localizations)

## Getting Started

You can utilize some wrappers made by other people, or use the API directly. It's very simple, so it is not hard at all to hand-roll your own data logic based on the responses. The biggest challenge is navigating the datamined game data and attaching it to the data returned in the right way.

See [Wrappers](#wrappers) for the language of your choice.

## Before You Request

A couple of rules on using the API:

1. Please don't try to enumerate UIDs or try to do massive query jobs in an effort to fill a database. There are hundreds of millions of UIDs and you simply won't be able to do this through this API. I may provide batch data at a later point.

2. Please set a custom `User-Agent` header with your requests so I can track them better and help you if needed.

3. There are dynamic ratelimits on UID endpoints - if you're requesting too fast, you'll run into slower response times and eventually a 429 return code. This mean you either need to slow down or contact me to see if it's possible to increase the ratelimits for you. In most cases it's not needed and is a result of unoptimized code.

4. On this note, all UID requests return a `ttl` field - this field is "seconds until the next Showcase request will be made for this UID". Until it runs out, the endpoint will return cached data, but you will still consume your ratelimit if you repeatedly hit it. Try to either cache the data with a timeout of `ttl` upon request, or prevent requests to the UID until its `ttl` runs out. I recommend Redis for this.

If you have difficulties working with the data, hop on the [Discord server](https://discord.gg/PcSZr5sbn3) for help.

## List of APIs

### UID endpoints

#### Get Showcase data with full player info

> https://enka.network/api/uid/618285856/

The response will contain `playerInfo` and `avatarInfoList`. `playerInfo` is the basic data about the game account. If `avatarInfoList` is missing, that means the Showcase of this account is either closed or not populated with characters.


#### Get only player info

> https://enka.network/api/uid/618285856/?info

By attaching `?info` to the request, you are requesting only `playerInfo`. The main request always tries to get `avatarInfoList` as well; if you only need `playerInfo`, please use this endpoint - it will be much faster than getting the full data.

In addition, both responses will contain an `owner` object if:

1. The user has an account on the site
2. The user added their UID to the profile
3. The user verified it
4. The user set its visibility to "public"

More info on user accounts below.

#### HTTP response codes

Please make sure to handle these in your app appropriately.
```
400 = Wrong UID format
404 = Player does not exist (MHY server said that)
424 = Game maintenance / everything is broken after the game update
429 = Rate-limited (either by my server or by MHY server)
500 = General server error
503 = I screwed up massively
```

### Profile endpoints

It is possible to create an account (profile) on the website and attach several game accounts to it. The users are then required to verify that the account belongs to them via a verification code placed in their signature - this way we can ensure the account belongs to them.

Users can "snapshot" builds under custom names, referred as "saved builds".

> https://dev.enka.network/api/profile/Algoinde/

Gets the user info.

> https://dev.enka.network/api/profile/Algoinde/hoyos/

Gets a list of "hoyos" - Genshin accounts and their metadata. This only returns accounts that are `verified` and `public` (users can hide accounts; unverified accounts are hidden by default). Each key in the response is a unique identifier of a hoyo which you need to use for the subsequent requests to actually get the characters/builds information.

> https://dev.enka.network/api/profile/Algoinde/hoyos/4Wjv2e/

Returns metadata for a single hoyo.

> https://dev.enka.network/api/profile/Algoinde/hoyos/4Wjv2e/builds/

Returns saved builds for a given hoyo. This is an object of arrays, where the key is `avatarId` of the character, and objects in arrays are different builds for a given character, in no particular order (but they do have an `order` field you need to order by for display).

If a build has a `live: true` field, that means it's not a "saved" build, but simply what was fetched from their showcase when they clicked "refresh". When refreshed, all old `live` builds are gone and new ones are created. Only the user decides when to perform this refresh - this data will NOT be up-to-date.

As outlined in [UID endpoints](#uid-endpoints), when you make a UID request, you could get an `owner` object. You can construct the URL using these fields in the object:

`https://dev.enka.network/api/profile/{owner.username}/hoyos/{owner.hash}/builds/`



## Data Structure Info

| Name | Description |
| :--- | :---------- |
| [playerInfo](#playerinfo) | Profile Info |
| [avatarInfoList](#avatarinfolist) | List of detailed information for every character from showcase |

### playerInfo

For basic data of characters by ID, go to [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
For any additional info, check the [Characters Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

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
For any additional info, check the [Characters Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Name | Description |
| :--- | :---------- |
| avatarID | Character ID |
| talentIdList | List of Constellation IDs <br /> There is no data if 0 Constellation |
| [propMap](#propmap) | Character Info Properties List |
| fightPropMap -> `{id: value}` |  Map of Character's Combat Properties. <br />Check the [Definitions for IDs](#fightprop)|
| skillDepotId | Character Skill Set ID <br />[Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`|
| inherentProudSkillList | List of Unlocked Skill Ids <br />[Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| Map of Skill Levels <br /> [Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| [equipList](#equiplist) | List of Equipments: Weapon, Ariftacts |
| fetterInfo.expLevel  | Character Friendship Level |

#### propMap

| Name | Description |
| :--- | :--------- |
| type | ID of Property Type, Check the [Definitions for IDs](#prop) |
| ival | Ignore it |
| val  | Value of Property |

#### equipList

| Name | Description |
| :--- | :--------- |
| itemId | Equipment ID <br /> [Artifacts Data](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Weapons Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Weapon Only]` | Weapon Base Info  |
| [reliquary](#reliquary) `[Artifact Only]` | Artifact Base Info  |
| [flat](#flat) | Detailed Info of Equipment |

#### weapon

For any additional info about weapons, check the [Weapons Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)

| Name | Description |
| :--- | :---------- |
| level | Weapon Level |
| promoteLevel | Weapon Ascension Level |
| affixMap | Weapon Refinement Level `[0-4]` |


#### reliquary

For any additional info about artifacts, check the [Artifacts Data](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/ReliquaryExcelConfigData.json)

| Name | Description |
| :--- | :---------- |
| level | Artifact Level `[1-21]` |
| mainPropId | Artifact Main Stat ID <br /> [MainProps Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |
| appendPropIdList | List of IDs of the artifact substats <br /> [AppendProp Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

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

| Type | Description |
| :--: | :---------- |
| 1 | Base HP |
| 2 | HP |
| 3 | HP% |
| 4 | Base ATK |
| 5 | ATK |
| 6 | ATK% |
| 7 | Base DEF |
| 8 | DEF |
| 9 | DEF% |
| 10 | Base SPD |
| 11 | SPD% |
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
| 80 | Cooldown reduction |
| 81 | Shield Strength |
| 1000 | Current Pyro Energy |
| 1001 | Current Electro Energy |
| 1002 | Current Hydro Energy |
| 1003 | Current Dendro Energy |
| 1004 | Current Anemo Energy |
| 1005 | Current Cryo Energy |
| 1006 | Current Geo Energy |
| 1010 | Current HP |
| 2000 | Max HP |
| 2001 | ATK |
| 2002 | DEF |
| 2003 | SPD |
| 3025 | Elemental reaction CRIT Rate |
| 3026 | Elemental reaction CRIT DMG |
| 3027 | Elemental reaction (Overloaded) CRIT Rate |
| 3028 | Elemental reaction (Overloaded) CRIT DMG |
| 3029 | Elemental reaction (Swirl) CRIT Rate |
| 3030 | Elemental reaction (Swirl) CRIT DMG |
| 3031 | Elemental reaction (Electro-Charged) CRIT Rate |
| 3032 | Elemental reaction (Electro-Charged) CRIT DMG |
| 3033 | Elemental reaction (Superconduct) CRIT Rate |
| 3034 | Elemental reaction (Superconduct) CRIT DMG |
| 3035 | Elemental reaction (Burn) CRIT Rate |
| 3036 | Elemental reaction (Burn) CRIT DMG |
| 3037 | Elemental reaction (Frozen (Shattered)) CRIT Rate |
| 3038 | Elemental reaction (Frozen (Shattered)) CRIT DMG |
| 3039 | Elemental reaction (Bloom) CRIT Rate |
| 3040 | Elemental reaction (Bloom) CRIT DMG |
| 3041 | Elemental reaction (Burgeon) CRIT Rate |
| 3042 | Elemental reaction (Burgeon) CRIT DMG |
| 3043 | Elemental reaction (Hyperbloom) CRIT Rate |
| 3044 | Elemental reaction (Hyperbloom) CRIT DMG |
| 3045 | Base Elemental reaction CRIT Rate |
| 3046 | Base Elemental reaction CRIT DMG |

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

| Name | Description |
| :--- | :---------- |
| FIGHT_PROP_BASE_ATTACK `[Weapon]` | Base ATK |
| FIGHT_PROP_HP | Flat HP |
| FIGHT_PROP_ATTACK | Flat ATK |
| FIGHT_PROP_DEFENSE | Flat DEF |
| FIGHT_PROP_HP_PERCENT | HP% |
| FIGHT_PROP_ATTACK_PERCENT | ATK% |
| FIGHT_PROP_DEFENSE_PERCENT | DEF% | 
| FIGHT_PROP_CRITICAL | Crit RATE |
| FIGHT_PROP_CRITICAL_HURT | Crit DMG |
| FIGHT_PROP_CHARGE_EFFICIENCY | Energy Recharge |
| FIGHT_PROP_HEAL_ADD | Healing Bonus |
| FIGHT_PROP_ELEMENT_MASTERY | Elemental Mastery |
| FIGHT_PROP_PHYSICAL_ADD_HURT | Physical DMG Bonus |
| FIGHT_PROP_FIRE_ADD_HURT | Pyro DMG Bonus |
| FIGHT_PROP_ELEC_ADD_HURT | Electro DMG Bonus |
| FIGHT_PROP_WATER_ADD_HURT | Hydro DMG Bonus |
| FIGHT_PROP_WIND_ADD_HURT | Anemo DMG Bonus |
| FIGHT_PROP_ICE_ADD_HURT |  Cryo DMG Bonus |
| FIGHT_PROP_ROCK_ADD_HURT | Geo DMG Bonus |
| FIGHT_PROP_GRASS_ADD_HURT | Dendro DMG Bonus |

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

For any additional info about names, descriptions and etc, check the [TextMap Data](https://github.com/Dimbreath/GenshinData/tree/master/TextMap), only includes languages supported by game. 

## Wrappers

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)