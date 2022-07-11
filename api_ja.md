この日本語翻訳は不正確な場合があります。必要に応じて原文を参照してください
[api.md](/api.md)

# Enka.Network - API

## 目次

- [入門](#getting-started)
- [データ構造情報](#data-structure-info)
- [定義](#definitions)
- [アイコンと画像](#icons-and-images)
- [言語情報](#localizations)

## 入門

特定のURLに対してGETリクエストを行うことでJSONデータを取得できます-`https://enka.network/u/[UID]/__data.json`  
例：https://enka.network/u/700378769/__data.json

## データ構造情報

| 名前 | 説明 |
| :--- | :---------- |
| [playerInfo](#playerinfo) | プロフィール情報 |
| [avatarInfoList](#avatarinfolist) | 公開されたキャラクター詳細のリスト |

### playerInfo

ID別の文字の基本データについては、[store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)にアクセスしてください。  
追加情報については、[キャラクターデータ](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json)を確認してください。

| 名前 | 説明 |
| :--- | :--------- | 
| nickname | プレイヤーのニックネーム |
| signature | ゲーム内ステータスメッセージ |
| worldLevel | 世界ランク |
| namecardId | 名刺ID |
| finishAchievementNum | 達成したアチーブメント数 |
| towerFloorIndex | 深境螺旋：層 |
| towerLevelIndex | 深境螺旋：間 |
| [showAvatarInfoList](#showavatarinfolist) | キャラクターIDとレベルのリスト |
| showNameCardIdList | 飾られた名刺のリスト |
| profilePicture.avatarId | プロフィールキャラクターのID |

#### showAvatarInfoList

| 名前 | 説明 |
| :--- | :--------- | 
| avatarId | キャラクターID |
| level | キャラクターLv |
| costumeId | キャラクター衣装ID 詳細は[store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)内の`"Costumes"`に定義されています  |

### avatarInfoList

ID別の文字の基本データについては、[store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)にアクセスしてください。  
追加情報については、[キャラクターデータ](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json)を確認してください。

| 名前 | 説明 |
| :--- | :---------- |
| avatarID | キャラクターID |
| talentIdList | 命ノ星座IDのリスト<br>0重の場合はデータがありません |
| [propMap](#propmap) | 文字情報プロパティ一覧 |
| fightPropMap -> `{id: value}` |  キャラクターの戦闘プロパティのマップ。<br>[IDの定義](#fightprop)を確認してください |
| skillDepotId | キャラクタースキルセットID <br />[Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`|
| inherentProudSkillList | 解放された固有天賦のIDリスト<br />[詳細情報](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| スキルレベルのマップ <br /> [詳細情報](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| [equipList](#equiplist) | 武器と聖遺物のリスト |
| fetterInfo.expLevel  | 好感度Lv |

#### propMap

| 名前 | 説明 |
| :--- | :--------- |
| type | プロパティタイプのID 詳細は[IDの定義](#prop)を参照してください |
| ival | それを無視します(?) |
| val  | プロパティの値 |

#### equipList

| 名前 | 説明 |
| :--- | :--------- |
| itemId | 識別ID <br /> [聖遺物情報](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br />[武器情報](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Weapon Only]` | 武器の基本情報  |
| [reliquary](#reliquary) `[Artifact Only]` | 聖遺物の基本情報  |
| [flat](#flat) | このアイテムの詳細情報 |

#### weapon

武器に関する追加情報については、[武器データ](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)を確認してください

| 名前 | 説明 |
| :--- | :---------- |
| level | 武器Lv |
| promoteLevel | 突破段階 |
| affixMap | 精錬ランク `[0-4]` |


#### reliquary

聖遺物に関する追加情報については、[聖遺物情報](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/ReliquaryExcelConfigData.json)を確認してください

| 名前 | 説明 |
| :--- | :---------- |
| level | 聖遺物Lv `[1-21]` |
| mainPropId | 聖遺物メインステータスID(?)<br /> [MainProps Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |

#### flat

| 名前 | 説明 |
| :--- | :---------- |
| nameTextHashMap | 表示名の識別番号 <br /> 参照：[言語情報](#localizations) |
| setNameTextHashMap `[Artifact Only]`| セット効果の識別番号<br /> 参照：[言語情報](#localizations)|
| rankLevel | レアリティ |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | 聖遺物のメインステータス |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | 聖遺物のサブステータス |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Weapon Only]`| 武器の基礎攻撃力とサブステータス |
| [itemType](#itemtype) | 種別(武器：`Weapon`,聖遺物：`Artifact`) |
| icon | アイテムアイコン名 <br /> [アイコン名の使用法](#icons-and-images)|
| [equipType](#equiptype) `[Artifact Only]` | 聖遺物種別 |

#### reliquaryMainstat, reliquarySubstats, weaponStats

| 名前 | 説明 |
| :--- | :---------- |
| mainPropId / appendPropID | プロパティ名。[名前の定義](#appendprop)を確認してください|
| propValue | プロパティ値 |

## 定義

### Prop

| ID | 説明 |
| :--: | :---------- |
| 1001 | XP |
| 1002 | Ascension | 
| 4001 | Level |

### FightProp

| ID | 説明 |
| :--: | :---------- |
| 1 | 基礎HP |
| 4 | 基礎攻撃力 |
| 7 | 基礎防御力 |
| 20 | 会心率 |
| 22 | 会心ダメージ |
| 23 | 元素チャージ効率 |
| 26 | 与える治癒効果 |
| 27 | 受ける治癒効果 |
| 28 | 元素熟知 |
| 29 | 物理耐性 |
| 30 | 物理ダメージ |
| 40 | 炎元素ダメージ |
| 41 | 雷元素ダメージ |
| 42 | 水元素ダメージ |
| 44 | 風元素ダメージ |
| 45 | 岩元素ダメージ |
| 46 | 氷元素ダメージ |
| 50 | 炎元素耐性 |
| 51 | 雷元素耐性 |
| 52 | 水元素耐性 |
| 53 | Dendro RES |
| 54 | 風元素耐性 |
| 55 | 岩元素耐性 |
| 56 | 氷元素耐性 |
| 70 | Pyro Enegry Cost |
| 71 | Electro Energy Cost |
| 72 | Hydro Energy Cost |
| 73 | Dendro Energy Cost |
| 74 | Anemo Energy Cost |
| 75 | Cryo Energy Cost |
| 76 | Geo Energy Cost |
| 2000 | 最大HP |
| 2001 | 攻撃力 |
| 2002 | 防御力 |

### ItemType

| 名前 | 説明 |
| :--- | :---------- |
| ITEM_WEAPON | 武器 |
| ITEM_RELIQUARY | 聖遺物 |

### EquipType

| 名前 | 説明 |
| :--- | :---------- |
| EQUIP_BRACER | 花 |
| EQUIP_NECKLACE | 羽 |
| EQUIP_SHOES | 時計 | 
| EQUIP_RING | 杯 |
| EQUIP_DRESS | 冠 |

### AppendProp

| 名前 | 説明 |
| :--- | :---------- |
| FIGHT_PROP_BASE_ATTACK `[Weapon]` | 基礎攻撃力 |
| FIGHT_PROP_HP | HP固定値 |
| FIGHT_PROP_ATTACK | 攻撃力固定値 |
| FIGHT_PROP_DEFENSE | 防御力固定値 |
| FIGHT_PROP_HP_PERCENT | HP% |
| FIGHT_PROP_ATTACK_PERCENT | 攻撃力% |
| FIGHT_PROP_DEFENSE_PERCENT | 防御力% | 
| FIGHT_PROP_CRITICAL | 会心率 |
| FIGHT_PROP_CRITICAL_HURT | 会心ダメージ |
| FIGHT_PROP_CHARGE_EFFICIENCY | 元素チャージ効率 |
| FIGHT_PROP_HEAL_ADD | 与える治癒効果 |
| FIGHT_PROP_ELEMENT_MASTERY | 元素熟知 |
| FIGHT_PROP_PHYSICAL_ADD_HURT | 物理ダメージ |
| FIGHT_PROP_FIRE_ADD_HURT | 炎元素ダメージ |
| FIGHT_PROP_ELEC_ADD_HURT | 雷元素ダメージ |
| FIGHT_PROP_WATER_ADD_HURT | 水元素ダメージ |
| FIGHT_PROP_WIND_ADD_HURT | 風元素ダメージ |
| FIGHT_PROP_ICE_ADD_HURT |  氷元素ダメージ |
| FIGHT_PROP_ROCK_ADD_HURT | 岩元素ダメージ |

## アイコンと画像

`https://enka.network/ui/[icon_name].png`でキャラクター、武器、聖遺物のアイコンを得られます  
通常、アイコン名は`"UI_"`または`"Skill_"`で始まる[characters talents](#キャラクターと天賦)を表します  
例： https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png

### 武器と聖遺物

[flat](#flat)の`icon`を参照

### キャラクターと天賦

[store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)から"UI_XXXXXX"または"Skill_XXXXXX"を探します

## Localizations

You may notice `"NameTextMapHash"` in [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json), `"nameTextHashMap"` and `"setNameTextHashMap"` at [flat](#flat) that could be used as a key to get basic localization data of characters, weapons and artifacts from [store/loc.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/loc.json).  
Also you can get localization data of [AppendProp](#appendprop) by using property name as a key - `"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"` etc.

For any additional info about names, descriptions and etc, check the [TextMap Data](https://github.com/Dimbreath/GenshinData/tree/master/TextMap), only includes languages supported by game. 
