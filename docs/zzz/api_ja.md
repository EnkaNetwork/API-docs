この日本語ドキュメントは翻訳されたものであり、不正確な場合があります。必要に応じて[英語版の原文](/api.md)を参照してください

# Enka.Network API - ゼンレスゾーンゼロ 

## 目次

- [データ構造情報](#データ構造情報)
- [定義](#定義)
- [計算式](#計算式)
- [アイコンと画像](#アイコンと画像)
- [言語情報](#言語情報)

## データ構造情報

| キー | 説明 |
| :--- | :---------- |
| uid | プレイヤーのUID |
| ttl | 次の更新までの秒数 |
| [PlayerInfo](#playerinfo) | プロフィール情報 |

#### PlayerInfo

| キー | 説明 |
| :--- | :--------- | 
| [SocialDetail](#socialdetail) | ソーシャル情報 |
| [ShowcaseDetail](#showcasedetail) | 展示されたキャラクターの情報 |

#### SocialDetail

| キー | 説明 |
| :--- | :--------- | 
| Desc | プロフィールのステータスメッセージ |
| [ProfileDetail](#profiledetail) | プロフィール概要 |
| [MedalList](#medallist) | バッジ一覧 | 

#### MedalList
| キー | 説明 |
| :--- | :--------- |
| MedalType | [バッジのタイプ](#badge-type)を参照|
| Value | バッジの値 |
| MedalIcon | バッジアイコンのID | 

### ProfileDetail

| キー | 説明 |
| :--- | :--------- |
| Uid | UID |
| Nickname | ニックネーム |
| ProfileId | プロフ画像のID |
| Level | インターノットレベル | 
| Title | 称号ID |
| CallingCallId | 名刺ID |
| AvatarId | メインキャラID (アキラまたはリン) |

#### ShowcaseDetail

| キー | 説明 |
| :--- | :--------- | 
| [AvatarList](#AvatarList) | 展示キャラのリスト |

### AvatarList

| キー | 説明 |
| :--- | :--------- | 
| Exp | エージェント経験値 |
| Level | エージェントレべル |
| promotionLevel | エージェント昇格レべル |
| TalentLevel | エージェント心象映画レべル |
| SkinId | エージェントスキンID |
| CoreSkillEnhancment | コアスキルの開放状況 - A, B, C, D, F, G |
| TalentToggles | 心象映画の外観のトグル状態 |
| WeaponEffectState | モチーフ音動機のエフェクトのトグル状態 `[0: OFF, 1: ON]` |
| ClaimedRewards | エージェント昇格報酬の受取状況 |
| ObtainmentTimestamp | エージェント獲得日時のタイムスタンプ |
| [Weapon](#weapon) | 装備した音動機 | 
| SkillLevelList | スキルレベルのリスト、インデックスは定義ファイルを参照 |
| [EquippedList](#EquipmentList) | UIDを含むドライバディスクのリスト |

#### Weapon

基礎値から実際の値を得る方法については [計算式](#計算式) を参照。
詳細な情報については [store/zzz/weapons.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/weapons.json) を参照。

| キー | 説明 |
| :--- | :--------- | 
| Uid | 音動機UID |
| Id | 音動機ID |
| Exp | 音動機の経験値 |
| Level | 音動機のレベル |
| BreakLevel | 音動機の更新レベル |
| IsAvailable | 音動機が利用可能かどうか |
| IsLocked | 音動機が保護ロックされているか |
| UpgradeLevel | 音動機の突破レベル |

#### EquipmentList

| キー | 説明 |
| :--- | :--------- | 
| Uid | ドライバディスクUID |
| Id | ドライバディスクID |
| Exp | ドライバディスクの経験値 |
| Level | ドライバディスクのレベル `[0-15]` |
| BreakLevel | サブステの伸び数の合計 |
| IsLocked | ドライバディスクが保護ロックされているか |
| IsAvailable | ドライバディスクが利用可能かどうか |
| IsTrash | ドライバディスクに廃棄マークがついているか |
| MainStatList | ドライバディスクのメインステリスト、追加情報は[Stat](#Stat)を参照 |
| RandomPropertyList | ドライバディスクのサブステリスト、追加情報は[Stat](#Stat)を参照 |

#### Stat

実際の値を計算する方法は [計算式](#計算式) を参照。

| キー | 説明 |
| :--- | :---------- | 
| PropertyValue | ステータス基礎値 |
| PropertyId | ステータスID<br>詳細は[定義](#property-id)を参照 |
| PropertyLevel | 伸び数 (サブステのみ) |

## 定義

### Property Id

詳しい情報については下の表と [store/zzz/property.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/property.json) を参照。

| 値 | 説明 |
| :--- | :--------- | 
| 11101 | HP `[Base]` |
| 11102 | HP% |
| 11103 | HP `[Flat]` |
| 12101 | 攻撃力 `[Base]` |
| 12102 | 攻撃力% |
| 12103 | 攻撃力 `[Flat]` |
| 12201 | 衝撃力 `[Base]` |
| 12202 | 衝撃力% |
| 13101 | 防御力 `[Base]` |
| 13102 | 防御力% |
| 13103 | 防御力 `[Flat]` |
| 20101 | 会心率 `[Base]` |
| 20103 | 会心率 `[Flat]` |
| 21101 | 会心ダメージ `[Base]` |
| 21103 | 会心ダメージ `[Flat]` |
| 23101 | 貫通率 `[Base]` |
| 23103 | 貫通率 `[Flat]` |
| 23201 | 貫通値  `[Base]` |
| 23203 | 貫通値 `[Flat]` |
| 30501 | エネルギー自動回復 `[Base]` |
| 30502 | エネルギー自動回復% |
| 30503 | エネルギー自動回復 `[Flat]` |
| 31201 | 異常マスタリー `[Base]` |
| 31203 | 異常マスタリー `[Flat]` |
| 31401 | 異常掌握 `[Base]` |
| 31402 | 異常掌握% |
| 31403 | 異常掌握 `[Flat]` |
| 31501 | 物理属性ダメージ `[Base]` |
| 31503 | 物理属性ダメージ `[Flat]` |
| 31601 | 火属性ダメージ `[Base]` |
| 31603 | 火属性ダメージ `[Flat]` |
| 31701 | 氷属性ダメージ `[Base]` |
| 31703 | 氷属性ダメージ `[Flat]` |
| 31801 | 電気属性ダメージ `[Base]` |
| 31803 | 電気属性ダメージ `[Flat]` |
| 31901 | エーテル属性ダメージ `[Base]` |
| 31903 | エーテル属性ダメージ `[Flat]` |

### Rarity

| 値 | レアリティ |
| :--- | :---------- |
| 4 | S | 
| 3 | A |
| 2 | B |

### Badge Type 

| 値 | 説明 |
| :--- | :---------- |
| 1 | 式興防衛戦 |
| 2 | 疑似的激戦試練 |
| 3 | 危局強襲戦 |
| 4 | 疑似的激戦試練 (末路) | 

## 計算式

### ドライバディスク

#### メインステータス
```値 = MainStat.PropertyValue * (MainStat.PropertyValue * Level * RarityScale)```
#### サブステータス
```値 = PropertyValue * PropertyLevel```

#### Rarity Scales
| レア度 | スケール |
| :----- | :------ |
| 4 | 0.2 |
| 3 | 0.25 |
| 2 | 0.2 |


### 音動機 

#### メインステータス

```値 = MainStat.BaseValue * (1 + 0.1568166666666667 * Level + 0.8922 * BreakLevel)```

#### サブステータス

```値 = SubStat.BaseValue * (1 + 0.3 * BreakLevel) ```

## アイコンと画像

全てのアイコン名は [API-docs/store/zzz](https://github.com/EnkaNetwork/API-docs/tree/master/store/zzz) のパース済データ内にあります。

より詳しい情報については [ZenlessData](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/) リポジトリを参照.

## 言語情報

[store/zzz/locs.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/locs.json) でIDから各言語に対応した文字列を得る事が出来ます。

その他の追加情報を[TextMap Data](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/TextMap)で得る事が出来ます。ゲームでサポートされている言語のみが含まれます。

## ラッパー
ラッパーはまだありません。
