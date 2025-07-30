この日本語ドキュメントは翻訳されたものであり、不正確な場合があります。必要に応じて[英語版の原文](/docs/gi/api.md)を参照してください

# Enka.Network API - 原神

- [データ構造情報](#データ構造情報)
- [定義](#定義)
- [アイコンと画像](#アイコンと画像)
- [言語情報](#言語情報)
- [ラッパー](#ラッパー)


## データ構造情報

| 名前 | 説明 |
| :--- | :---------- |
| [playerInfo](#playerinfo) | プロフィール情報 |
| [avatarInfoList](#avatarinfolist) | 公開されたキャラクター詳細のリスト |

### playerInfo

キャラクターのID別の基本データについては、[store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)を参照。
より詳細な情報については、[キャラクターデータ](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json)を参照。

| 名前 | 説明 |
| :--- | :--------- |
| nickname | プレイヤーのニックネーム |
| signature | ゲーム内ステータスメッセージ |
| worldLevel | 世界ランク |
| namecardId | メインで設定された名刺ID |
| finishAchievementNum | 達成したアチーブメント数 |
| towerFloorIndex | クリアした深境螺旋の層数 |
| towerLevelIndex | クリアした深境螺旋の間数 |
| [showAvatarInfoList](#showavatarinfolist) | 展示されたキャラクターIDとレべルのリスト |
| showNameCardIdList | 展示された名刺のリスト |
| profilePicture.avatarId | プロフィール画像のID |

#### showAvatarInfoList

| 名前 | 説明 |
| :--- | :--------- |
| avatarId | キャラクターID |
| level | キャラクターレべル |
| costumeId | キャラクター衣装ID<br>詳細は[store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)内の`"Costumes"`を参照  |

### avatarInfoList

キャラクターのID別の基本データについては、[store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)を参照。
追加情報については、[キャラクターデータ](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json)を参照。

| 名前 | 説明 |
| :--- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| avatarID | キャラクターID                                                                                                                                                                 |
| talentIdList | 解放済みの命ノ星座IDのリスト<br>C0の場合はこの項目はundefined                                                                                                                                            |
| [propMap](#propmap) | キャラクター情報のプロパティ                                                                                                                                                              |
| fightPropMap -> `{id: value}` | キャラクターの戦闘ステータスのMap。<br>[FightPropの定義](#fightprop)を確認してください                                                                                                                      |
| skillDepotId | キャラクタースキルID <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`             |
| inherentProudSkillList | 解放された天賦のIDリスト<br />[詳細情報](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| skillLevelMap -> `{skill_id: level}`| スキルレベルのマップ <br /> [詳細情報](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"`    |
| [equipList](#equiplist) | 装備済の武器と聖遺物のリスト                                                                                                                                                               |
| fetterInfo.expLevel  | 好感度レべル                                                                                                                                                                    |

#### propMap

| 名前 | 説明 |
| :--- | :--------- |
| type | プロパティタイプのID 詳細は[IDの定義](#prop)を参照してください |
| ival | 無効な値 (これは使用しないでください) |
| val  | プロパティの値 |

#### equipList

| 名前 | 説明 |
| :--- | :--------- |
| itemId | 装備品のID <br /> [聖遺物情報](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br />[武器情報](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Weapon Only]` | 武器の基本情報  |
| [reliquary](#reliquary) `[Artifact Only]` | 聖遺物の基本情報  |
| [flat](#flat) | 各装備品の詳細情報 |

#### weapon

武器に関する追加情報については、[武器データ](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)を確認してください

| 名前 | 説明 |
| :--- | :---------- |
| level | 武器レべル |
| promoteLevel | 突破段階 |
| affixMap | 精錬ランク `[0-4]` |


#### reliquary

聖遺物に関する追加情報については、[聖遺物情報](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json)を確認してください

| 名前 | 説明 |
| :--- | :---------- |
| level | 聖遺物Lv `[1-21]` |
| mainPropId | 聖遺物メインステータスID<br /> [MainProps Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |
| appendPropIdList | 聖遺物サブステータスIDのリスト<br /> [AppendProp Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

#### flat

| 名前 | 説明 |
| :--- | :---------- |
| nameTextHashMap | 表示名の識別番号 <br /> 参照：[言語情報](#言語情報) |
| setNameTextHashMap `[Artifact Only]`| セット効果の識別番号<br /> 参照：[言語情報](#言語情報)|
| rankLevel | レアリティ |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | 聖遺物のメインステータス |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | 聖遺物のサブステータス |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Weapon Only]`| 武器の基礎攻撃力とサブステータス |
| [itemType](#itemtype) | 種別(武器：`Weapon`,聖遺物：`Artifact`) |
| icon | アイテムアイコン名 <br /> [アイコン名の使用法](#アイコンと画像)|
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
| 1001 | 経験値 |
| 1002 | 突破段階 |
| 4001 | レベル |

### FightProp

| ID | 説明 |
| :--: | :---------- |
| 1 | 基礎HP |
| 2 | HP |
| 3 | HP% |
| 4 | 基礎攻撃力 |
| 5 | 攻撃力 |
| 6 | 攻撃力% |
| 7 | 基礎防御力 |
| 8 | 防御力 |
| 9 | 防御力% |
| 10 | 基礎速度 |
| 11 | 速度% |
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
| 43 | 草元素ダメージ |
| 44 | 風元素ダメージ |
| 45 | 岩元素ダメージ |
| 46 | 氷元素ダメージ |
| 50 | 炎元素耐性 |
| 51 | 雷元素耐性 |
| 52 | 水元素耐性 |
| 53 | 草元素耐性 |
| 54 | 風元素耐性 |
| 55 | 岩元素耐性 |
| 56 | 氷元素耐性 |
| 70 | 炎元素 元素エネルギー 要求量(元素爆発) |
| 71 | 雷元素 元素エネルギー 要求量(元素爆発) |
| 72 | 水元素 元素エネルギー 要求量(元素爆発) |
| 73 | 草元素 元素エネルギー 要求量(元素爆発) |
| 74 | 風元素 元素エネルギー 要求量(元素爆発) |
| 75 | 氷元素 元素エネルギー 要求量(元素爆発) |
| 76 | 岩元素 元素エネルギー 要求量(元素爆発) |
| 80 | クールタイム短縮 |
| 81 | シールド強化 |
| 1000 | 現在の炎元素 元素エネルギー |
| 1001 | 現在の雷元素 元素エネルギー |
| 1002 | 現在の水元素 元素エネルギー |
| 1003 | 現在の草元素 元素エネルギー |
| 1004 | 現在の風元素 元素エネルギー |
| 1005 | 現在の氷元素 元素エネルギー |
| 1006 | 現在の岩元素 元素エネルギー |
| 1010 | 現在HP |
| 2000 | 最大HP |
| 2001 | 攻撃力 |
| 2002 | 防御力 |
| 2003 | 速度 |
| 3025 | 元素反応 会心率 |
| 3026 | 元素反応 会心ダメージ |
| 3027 | 元素反応(過負荷)会心率 |
| 3028 | 元素反応(過負荷)会心ダメージ |
| 3029 | 元素反応(拡散)会心率 |
| 3030 | 元素反応(拡散)会心ダメージ |
| 3031 | 元素反応(感電)会心率 |
| 3032 | 元素反応(感電)会心ダメージ |
| 3033 | 元素反応(超伝導)会心率 |
| 3034 | 元素反応(超伝導)会心ダメージ |
| 3035 | 元素反応(燃焼)会心率 |
| 3036 | 元素反応(燃焼)会心ダメージ |
| 3037 | 元素反応(凍結(氷砕き))会心率 |
| 3038 | 元素反応(凍結(氷砕き))会心ダメージ |
| 3039 | 元素反応(開花)会心率 |
| 3040 | 元素反応(開花)会心ダメージ |
| 3041 | 元素反応(烈開花)会心率 |
| 3042 | 元素反応(烈開花)会心ダメージ |
| 3043 | 元素反応(超開花)会心率 |
| 3044 | 元素反応(超開花)会心ダメージ |
| 3045 | 基礎元素反応会心率 |
| 3046 | 基礎元素反応会心ダメージ |

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
| FIGHT_PROP_GRASS_ADD_HURT | 草元素ダメージ |

## アイコンと画像

`https://enka.network/ui/[icon_name].png`でキャラクター、武器、聖遺物のアイコンを得られます
通常、アイコン名は`"UI_"`または`"Skill_"`で始まる[characters talents](#キャラクターと天賦)を表します
例： https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png

### 武器と聖遺物

[flat](#flat)の`icon`を参照

### キャラクターと天賦

[store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)から"UI_XXXXXX"または"Skill_XXXXXX"を探します

## 言語情報
[store/loc.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/loc.json)でIDから各言語に対応した文字列を得る事が出来ます。
この時に使用されるキーとなるIDは
* [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)内の`"NameTextMapHash"`
* [flat](#flat)の`"nameTextHashMap"` `"setNameTextHashMap"`
* [AppendProp](#appendprop) の名前。例：`"FIGHT_PROP_HP"` `"FIGHT_PROP_HEAL_ADD"`
等です

その他の追加情報を[TextMap Data](https://gitlab.com/Dimbreath/AnimeGameData/-/tree/master/TextMap)で得る事が出来ます。ゲームでサポートされている言語のみが含まれます。

## ラッパー

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Rust - https://github.com/eratou/enkanetwork-rs - [eratou](https://github.com/eratou)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)

Python - https://github.com/seriaati/enka-py - [seriaati](https://github.com/seriaati)

Java - https://github.com/kazuryyx/EnkaNetworkAPI - [kazury](https://github.com/kazuryyx)

C# - https://github.com/aliafuji/EnkaDotnet - [aliafuji](https://github.com/aliafuji)

C# - https://github.com/Carried520/EnkaSharp - [Carried520](https://github.com/Carried520)

Go - https://github.com/kirinyoku/enkanetwork-go - [kirinyoku](https://github.com/kirinyoku)