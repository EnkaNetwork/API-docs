简体中文翻译可能并不完全准确。如有必要请参考原文
[api.md](/api.md)

# Enka.Network - API

## 目录

- [入门](#入门)
- [数据结构](#数据结构)
- [定义](#定义)
- [图标和图片](#图标和图片)
- [本地化](#本地化)

## 入门

您可以直接使用他人的封装，或直接使用API。根据响应编写自己的数据逻辑非常简单，最大的挑战是分辨解包的游戏数据并将其正确地应用在返回的数据上。

在[封装](#封装)章节查看您所选的语言。

## 在发送请求之前

使用本API的几条规则：

1. 请不要尝试枚举 UID 或大量查询来填充数据库，在成千上万的 UID 中使用本 API 做到这一点根本是无稽之谈。我可能会在之后提供批量数据。

2. 请为您的请求自定义 `User-Agent` 头，以便我进行追踪，并给您提供帮助。

3. UID 端点有动态频率限制 - 如果您请求太快的话，会被降低响应时间或直接返回 429 状态码。这种情况您需要降低请求频率，或联系我看看是否可能为您提高限制（大多数情况下没有必要，仅仅是未经优化的代码导致的）。

4. 在此说明，所有 UID 查询请求都会返回一个 `ttl` 字段 - 此字段表示“下次查询此 UID 的角色展柜前的秒数”，在这之前，端点将返回缓存数据，但是这种情况下依旧会消耗您的请求限制。请尝试在请求后使用 `ttl` 的超时缓存数据（推荐使用 Redis），或避免在 `ttl` 耗尽前再次查询此 UID。

如果您处理这些数据时遇到困难，可以到 [Discord 服务器](https://discord.gg/PcSZr5sbn3) 寻求帮助。

## API 列表

### UID 端点

#### 获取角色展柜以及玩家信息

> https://enka.network/api/uid/618285856/

返回包括 `playerInfo` 和 `avatarInfoList`。`playerInfo` 是关于此游戏账号的基本数据。如果没有 `avatarInfoList`，则表示此账号关闭了角色展柜“显示角色详情”或没有展示任何角色。


#### 仅获取玩家信息

> https://enka.network/api/uid/618285856/?info

在请求中加入 `?info` 以单独查询 `playerInfo`。前文的请求方式总会获取 `avatarInfoList`，如果仅需要 `playerInfo`，使用本端点会比获取全部数据快上许多。

此外，如果满足下述条件，这两种请求都会包含一个 `owner` 对象：

1. 此用户在本站有账号
2. 此用户在用户资料中添加了TA的UID
3. 此用户验证了该UID
4. 此用户将其可见性设置为“公开”

下面有更多关于用户账号的信息。

#### HTTP 响应状态码

请确保您的应用对此有适当的处理。
```
400 = UID 格式错误
404 = 玩家不存在（MHY 服务器说的）
424 = 游戏维护中 / 游戏更新后一切都崩溃了
429 = 请求频率限制（被我的或者MHY的服务器）
500 = 服务器错误
503 = 我搞砸了
```

### 用户资料端点

网站上可以创建账户（用户资料）并添加游戏账号，用户们之后会被要求将验证码写入游戏签名以验证账号，这样我们可以确保此账号属于他们。

用户可以以自定义名称“快照”配装，或者说“已保存的配装”。

> https://enka.network/api/profile/Algoinde/

获取用户信息。

> https://enka.network/api/profile/Algoinde/hoyos/

获取“hoyo”的列表 - 原神账号与其元数据。仅返回已验证和公开的账号（用户可以隐藏账号，未验证的账号默认隐藏），返回的每个键都是一个 hoyo 的唯一标识，在后面的请求中您需要它们来获取其角色/配装信息。

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/

返回单个 hoyo 的元数据。

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/builds/

返回给定 hoyo 保存的配装。根是一个值为数组的对象，键是该角色的 `avatarId`，数组中的对象是此角色的不同配装（未经排序，不过其中有 `order` 可供排序显示）。

如果某个配装包含 `live: true` 字段，则表示其不是“已保存”的配装，而只是该用户上次点击“刷新”时从角色展柜中抓取的数据，每次刷新时，旧的 `live` 配装会被新的代替。此数据**并不**总是保持最新，仅由用户决定什么时候刷新。

在 [UID 端点](#uid-端点) 章节提到过，发送 UID 请求时会得到 `owner` 对象，您可以用此对象中的字段构造URL：

`https://enka.network/api/profile/{owner.username}/hoyos/{owner.hash}/builds/`



## 数据结构

| 名称 | 描述 |
| :--- | :---------- |
| [playerInfo](#playerinfo) | 玩家资料信息 |
| [avatarInfoList](#avatarinfolist) | 正在展示的每个角色的详细信息列表 |

### playerInfo

对于通过 ID 获取角色的基本信息，请前往 [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)。  <br />
有关任何其他信息，请查看 [Characters Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarExcelConfigData.json)。

| 名称 | 描述 |
| :--- | :--------- | 
| nickname | 名称 |
| signature | 签名 |
| worldLevel | 世界等级 |
| namecardId | 资料名片 ID |
| finishAchievementNum | 已解锁成就数 |
| towerFloorIndex | 本期深境螺旋层数 |
| towerLevelIndex | 本期深境螺旋间数 |
| [showAvatarInfoList](#showavatarinfolist) | 角色 ID 与等级的列表 |
| showNameCardIdList | 正在展示的名片 ID 列表 |
| profilePicture.avatarId | 玩家头像的角色 ID |

#### showAvatarInfoList

| 名称 | 描述 |
| :--- | :--------- | 
| avatarId | 角色 ID |
| level | 角色等级 |
| costumeId | 角色衣装 ID。请查看 [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) 的 `"Costumes"` |

### avatarInfoList

对于通过 ID 获取角色的基本信息，请前往 [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)。  <br />
有关任何其他信息，请查看 [Characters Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarExcelConfigData.json)。

| 名称 | 描述                                                                                                                                                                       |
| :--- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| avatarID | 角色 ID                                                                                                                                                                    |
| talentIdList | 命之座 ID 列表 <br /> 如果未解锁任何命之座则此数据不存在                                                                                                                                       |
| [propMap](#propmap) | 角色属性列表                                                                                                                                                                   |
| fightPropMap -> `{id: value}` | 角色战斗属性 Map <br />请查看 [ID 定义](#fightprop)                                                                                                                                 |
| skillDepotId | 角色天赋 ID <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`                    |
| inherentProudSkillList | 固定天赋列表 ID <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| 天赋等级 Map <br /> [Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| [equipList](#equiplist) | 装备列表：武器、圣遗物                                                                                                                                                              |
| fetterInfo.expLevel  | 角色好感等级                                                                                                                                                                   |

#### propMap

| 名称 | 描述 |
| :--- | :--------- |
| type | 属性类型，请查看 [ID 定义](#prop) |
| ival | 忽略它 |
| val  | 属性值 |

#### equipList

| 名称 | 描述                                                                                                                                                                                                                                                                               |
| :--- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| itemId | 装备 ID <br /> [Artifacts Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Weapons Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Weapon Only]` | 武器基本信息                                                                                                                                                                                                                                                                           |
| [reliquary](#reliquary) `[Artifact Only]` | 圣遗物基本信息                                                                                                                                                                                                                                                                          |
| [flat](#flat) | 装备详细信息                                                                                                                                                                                                                                                                           |

#### weapon

有关武器的任何其他信息，请查看 [Weapons Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/WeaponExcelConfigData.json)

| 名称 | 描述 |
| :--- | :---------- |
| level | 武器等级 |
| promoteLevel | 武器突破等级 |
| affixMap | 武器精炼等级 `[0-4]` |


#### reliquary

有关圣遗物的任何其他信息，请查看 [Artifacts Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryExcelConfigData.json)

| 名称 | 描述 |
| :--- | :---------- |
| level | 圣遗物等级 `[1-21]` |
| mainPropId | 圣遗物主属性 ID <br /> [MainProps Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |
| appendPropIdList | 圣遗物副属性 ID 列表 <br /> [AppendProp Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

#### flat

| 名称 | 描述 |
| :--- | :---------- |
| nameTextHashMap | 装备名的哈希值 <br /> 请查看 [本地化](#本地化) |
| setNameTextHashMap `[Artifact Only]`| 圣遗物套装的名称的哈希值 <br /> 请查看 [本地化](#本地化)|
| rankLevel | 装备稀有度 |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | 圣遗物主属性 |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | 圣遗物副属性列表 |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Weapon Only]`| 武器属性列表：基础攻击力、副属性 |
| [itemType](#itemtype) | 装备类别：武器、圣遗物 |
| icon | 装备图标名称 <br /> [图标名称使用方法](#图标和图片)|
| [equipType](#equiptype) `[Artifact Only]` | 圣遗物类型 |

#### reliquaryMainstat, reliquarySubstats, weaponStats

| 名称 | 描述 |
| :--- | :---------- |
| mainPropId / appendPropID | 装备属性名称。请查看 [名称定义](#appendprop)|
| propValue | 属性值 |

## 定义

### Prop

| 类型 | 描述 |
| :--: | :---------- |
| 1001 | 经验值 |
| 1002 | 突破 | 
| 4001 | 等级 |

### FightProp

| 类型 | 描述 |
| :--: | :---------- |
| 1 | 基础生命值 |
| 2 | 生命值 |
| 3 | 生命值百分比 |
| 4 | 基础攻击力 |
| 5 | 攻击力 |
| 6 | 攻击力百分比 |
| 7 | 基础防御力 |
| 8 | 防御力 |
| 9 | 百分比防御力 |
| 10 | 基础速度 |
| 11 | 速度百分比 |
| 20 | 暴击率 |
| 22 | 暴击伤害 |
| 23 | 元素充能效率 |
| 26 | 治疗加成 |
| 27 | 受治疗加成 |
| 28 | 元素精通 |
| 29 | 物理抗性 |
| 30 | 物理伤害加成 |
| 40 | 火元素伤害加成 |
| 41 | 雷元素伤害加成 |
| 42 | 水元素伤害加成 |
| 43 | 草元素伤害加成 |
| 44 | 风元素伤害加成 |
| 45 | 岩元素伤害加成 |
| 46 | 冰元素伤害加成 |
| 50 | 火元素抗性 |
| 51 | 雷元素抗性 |
| 52 | 水元素抗性 |
| 53 | 草元素抗性 |
| 54 | 风元素抗性 |
| 55 | 岩元素抗性 |
| 56 | 冰元素抗性 |
| 70 | 火元素能量需求（元素爆发） |
| 71 | 雷元素能量需求（元素爆发） |
| 72 | 水元素能量需求（元素爆发） |
| 73 | 草元素能量需求（元素爆发） |
| 74 | 风元素能量需求（元素爆发） |
| 75 | 冰元素能量需求（元素爆发） |
| 76 | 岩元素能量需求（元素爆发） |
| 80 | 冷却缩减 |
| 81 | 护盾强效 |
| 1000 | 当前火元素能量 |
| 1001 | 当前雷元素能量 |
| 1002 | 当前水元素能量 |
| 1003 | 当前草元素能量 |
| 1004 | 当前风元素能量 |
| 1005 | 当前冰元素能量 |
| 1006 | 当前岩元素能量 |
| 1010 | 当前生命值 |
| 2000 | 生命值上限 |
| 2001 | 攻击力 |
| 2002 | 防御力 |
| 2003 | 速度 |
| 3025 | 元素反应暴击率 |
| 3026 | 元素反应暴击伤害|
| 3027 | 元素反应（超载）暴击率 |
| 3028 | 元素反应（超载）暴击伤害 |
| 3029 | 元素反应（扩散）暴击率 |
| 3030 | 元素反应（扩散）暴击伤害 |
| 3031 | 元素反应（感电）暴击率 |
| 3032 | 元素反应（感电）暴击伤害 |
| 3033 | 元素反应（超导）暴击率 |
| 3034 | 元素反应（超导）暴击伤害 |
| 3035 | 元素反应（燃烧）暴击率 |
| 3036 | 元素反应（燃烧）暴击伤害 |
| 3037 | 元素反应（碎冰）暴击率 |
| 3038 | 元素反应（碎冰）暴击伤害 |
| 3039 | 元素反应（绽放）暴击率 |
| 3040 | 元素反应（绽放）暴击伤害 |
| 3041 | 元素反应（烈绽放）暴击率 |
| 3042 | 元素反应（烈绽放）暴击伤害 |
| 3043 | 元素反应（超绽放）暴击率 |
| 3044 | 元素反应（超绽放）暴击伤害 |
| 3045 | 基础元素反应暴击率 |
| 3046 | 基础元素反应暴击伤害 |

### ItemType

| 名称 | 描述 |
| :--- | :---------- |
| ITEM_WEAPON | 武器 |
| ITEM_RELIQUARY | 圣遗物 |

### EquipType

| 名称 | 描述 |
| :--- | :---------- |
| EQUIP_BRACER | 生之花 |
| EQUIP_NECKLACE | 死之羽 |
| EQUIP_SHOES | 时之沙 | 
| EQUIP_RING | 空之杯 |
| EQUIP_DRESS | 理之冠 |

### AppendProp

| 名称 | 描述 |
| :--- | :---------- |
| FIGHT_PROP_BASE_ATTACK `[Weapon]` | 基础攻击力 |
| FIGHT_PROP_HP | 生命值 |
| FIGHT_PROP_ATTACK | 攻击力 |
| FIGHT_PROP_DEFENSE | 防御力 |
| FIGHT_PROP_HP_PERCENT | 生命值百分比 |
| FIGHT_PROP_ATTACK_PERCENT | 攻击力百分比 |
| FIGHT_PROP_DEFENSE_PERCENT | 防御力百分比 | 
| FIGHT_PROP_CRITICAL | 暴击率 |
| FIGHT_PROP_CRITICAL_HURT | 暴击伤害 |
| FIGHT_PROP_CHARGE_EFFICIENCY | 元素充能效率 |
| FIGHT_PROP_HEAL_ADD | 治疗加成 |
| FIGHT_PROP_ELEMENT_MASTERY | 元素精通 |
| FIGHT_PROP_PHYSICAL_ADD_HURT | 物理伤害加成 |
| FIGHT_PROP_FIRE_ADD_HURT | 火元素伤害加成 |
| FIGHT_PROP_ELEC_ADD_HURT | 雷元素伤害加成 |
| FIGHT_PROP_WATER_ADD_HURT | 水元素伤害加成 |
| FIGHT_PROP_WIND_ADD_HURT | 风元素伤害加成 |
| FIGHT_PROP_ICE_ADD_HURT |  冰元素伤害加成 |
| FIGHT_PROP_ROCK_ADD_HURT | 岩元素伤害加成 |
| FIGHT_PROP_GRASS_ADD_HURT | 草元素伤害加成 |

## 图标和图片

您可以通过 Enka 获取角色、武器和圣遗物的图标，通过 URL `https://enka.network/ui/[icon_name].png`。  
通常图标名称以 `"UI_"` 或 `"Skill_"`（对于[角色天赋](#characters-talents-and-consts)）为前缀。  
例如 https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png

### 武器和圣遗物

前往 [flat](#flat) 并寻找 `icon`。

### 角色，天赋和命座

前往 [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) 并通过角色 ID 寻找与 "UI_XXXXXX" 或 "Skill_XXXXXX" 相关的数据。

## 本地化

您可能会注意到 [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) 中的 `"NameTextMapHash"`、[flat](#flat) 中的 `"nameTextHashMap"` 和 `"setNameTextHashMap"`，它们可以用作从 [store/loc.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/loc.json) 获取角色、武器和圣遗物的基本的本地化数据的键。  

您还可以通过使用属性名称作为键来获取 [AppendProp](#appendprop) 中的本地化数据 —— 例如 `"FIGHT_PROP_HP"`、`"FIGHT_PROP_HEAL_ADD"` 等。

有关名称、描述等的任何其他信息，请查看 [TextMap Data](https://gitlab.com/Dimbreath/AnimeGameData/-/tree/main/TextMap)，仅包括游戏支持的语言。

## 封装

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)

Python - https://github.com/seriaati/enka-py - [seriaati](https://github.com/seriaati)

Java - https://github.com/kazuryyx/EnkaNetworkAPI - [kazury](https://github.com/kazuryyx)