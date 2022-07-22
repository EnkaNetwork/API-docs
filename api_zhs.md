简体中文翻译可能并不完全准确。如有必要请参考原文
[api.md](/api.md)

# Enka.Network - API

## 目录

- [入门](#getting-started)
- [数据结构](#data-structure-info)
- [定义](#definitions)
- [图标和图片](#icons-and-images)
- [本地化](#localizations)

## 入门

你可以通过请求这个 URL 获取 JSON 数据 —— `https://enka.network/u/[UID]/__data.json` <br />
例如 https://enka.network/u/700378769/__data.json

## 数据结构

| 名称 | 描述 |
| :--- | :---------- |
| [playerInfo](#playerinfo) | 档案信息 |
| [avatarInfoList](#avatarinfolist) | 正在展示的每个角色的详细信息列表 |

### playerInfo

对于通过 ID 获取角色的基本信息，请前往 [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)。  <br />
有关任何其他信息，请查看 [Characters Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json)。

| 名称 | 描述 |
| :--- | :--------- | 
| nickname | 名称 |
| signature | 签名 |
| worldLevel | 世界等级 |
| namecardId | 档案名片 ID |
| finishAchievementNum | 已解锁成就数 |
| towerFloorIndex | 深境螺旋层数 |
| towerLevelIndex | 深境螺旋间数 |
| [showAvatarInfoList](#showavatarinfolist) | 角色 ID 与等级的列表 |
| showNameCardIdList | 已拥有名片 ID 列表 |
| profilePicture.avatarId | 玩家头像的角色 ID |

#### showAvatarInfoList

| 名称 | 描述 |
| :--- | :--------- | 
| avatarId | 角色 ID |
| level | 角色等级 |
| costumeId | 角色衣装 ID。请查看 [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) 的 `"Costumes"` |

### avatarInfoList

对于通过 ID 获取角色的基本信息，请前往 [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)。  <br />
有关任何其他信息，请查看 [Characters Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json)。

| 名称 | 描述 |
| :--- | :---------- |
| avatarID | 角色 ID |
| talentIdList | 命之座 ID 列表 <br /> 如果未解锁任何命之座则此数据不存在 |
| [propMap](#propmap) | 角色属性列表 |
| fightPropMap -> `{id: value}` |  角色战斗属性 Map <br />请查看 [ID 定义](#fightprop)|
| skillDepotId | 角色天赋 ID <br />[Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`|
| inherentProudSkillList | 固定天赋列表 ID <br />[Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| 天赋等级 Map <br /> [Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| [equipList](#equiplist) | 装备列表：武器、圣遗物 |
| fetterInfo.expLevel  | 角色好感等级 |

#### propMap

| 名称 | 描述 |
| :--- | :--------- |
| type | 属性类型, 请查看 [ID 定义](#prop) |
| ival | 忽略它 |
| val  | 属性值 |

#### equipList

| 名称 | 描述 |
| :--- | :--------- |
| itemId | 装备 ID <br /> [Artifacts Data](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Weapons Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Weapon Only]` | 武器基本信息  |
| [reliquary](#reliquary) `[Artifact Only]` | 圣遗物基本信息  |
| [flat](#flat) | 装备详细信息 |

#### weapon

有关武器的任何其他信息, 请查看 [Weapons Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)

| 名称 | 描述 |
| :--- | :---------- |
| level | 武器等级 |
| promoteLevel | 武器突破等级 |
| affixMap | 武器精炼等级 `[0-4]` |


#### reliquary

有关圣遗物的任何其他信息, 请查看 [Artifacts Data](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/ReliquaryExcelConfigData.json)

| 名称 | 描述 |
| :--- | :---------- |
| level | 圣遗物等级 `[1-21]` |
| mainPropId | 圣遗物主属性 ID <br /> [MainProps Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |
| appendPropIdList | 圣遗物副属性 ID 列表 <br /> [AppendProp Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

#### flat

| 名称 | 描述 |
| :--- | :---------- |
| nameTextHashMap | 装备名的哈希值 <br /> 请查看 [本地化](#localizations) |
| setNameTextHashMap `[Artifact Only]`| 圣遗物套装的名称的哈希值 <br /> 请查看 [本地化](#localizations)|
| rankLevel | 装备稀有度 |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | 圣遗物主属性 |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | 圣遗物副属性列表 |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Weapon Only]`| 武器属性列表：基础攻击力、副属性 |
| [itemType](#itemtype) | 装备类别：武器、圣遗物 |
| icon | 装备图标名称 <br /> [图标名称使用方法](#icons-and-images)|
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
| 1001 | 经验 |
| 1002 | 突破 | 
| 4001 | 等级 |

### FightProp

| 类型 | 描述 |
| :--: | :---------- |
| 1 | 基础生命值 |
| 4 | 基础攻击力 |
| 7 | 基础防御力 |
| 20 | 暴击率 |
| 22 | 暴击伤害 |
| 23 | 元素充能效率 |
| 26 | 治疗加成 |
| 27 | 受治疗加成 |
| 28 | 元素充能效率 |
| 29 | 冷却缩减 |
| 30 | 护盾强效 |
| 40 | 火元素伤害加成 |
| 41 | 雷元素伤害加成 |
| 42 | 水元素伤害加成 |
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
| 70 | Pyro Enegry Cost |
| 71 | Electro Energy Cost |
| 72 | Hydro Energy Cost |
| 73 | Dendro Energy Cost |
| 74 | Anemo Energy Cost |
| 75 | Cryo Energy Cost |
| 76 | Geo Energy Cost |
| 2000 | 生命值上限 |
| 2001 | 攻击力 |
| 2002 | 防御力 |

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
| FIGHT_PROP_BASE_ATTACK `[Weapon]` | 攻击力 |
| FIGHT_PROP_HP | 生命值 |
| FIGHT_PROP_ATTACK | 普通攻击 |
| FIGHT_PROP_DEFENSE | 普通防御 |
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

有关名称、描述等的任何其他信息，请查看 [TextMap Data](https://github.com/Dimbreath/GenshinData/tree/master/TextMap)，仅包括游戏支持的语言。
