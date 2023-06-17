# Enka.Network - API

## 목차

- [시작하기](#시작하기)
- [데이터 구조](#데이터-구조)
- [정의](#정의)
- [아이콘 및 이미지](#아이콘-및-이미지)
- [현지화](#현지화)

## 시작하기

다른 사람이 제작한 [패키지](#패키지)를 활용하거나 API를 직접 사용할 수 있습니다.
API 응답을 기반으로 하여 자체적인 데이터 로직을 구성하는 것은 매우 간단합니다.


## 사용 전 유의 사항

아래는 API 사용에 대한 몇가지 규칙입니다.

1. 데이터베이스를 수집하기 위해 UID를 열거하거나 대규모 요청을 수행하지 마세요. 수많은 UID가 존재하기 때문에 해당 작업을 수행할 수 없습니다. 추후에 일괄적으로 요청이 가능한 API를 제공할 수도 있습니다.

2. 요청 주체를 식별하고 필요한 경우 도움을 드릴 수 있도록 요청 헤더에 `User-Agent` 항목을 직접 설정해 주세요.

3. UID 엔드포인트에는 유동적인 속도 제한이 있습니다. 요청이 너무 빠르다면 응답 시간이 점차 느려지고 결국엔 HTTP 429 오류를 반환합니다. 따라서 요청 속도를 늦추거나 저에게 연락하셔서 이러한 제한을 늘릴 수 있는지 확인하세요. 그러나 대부분의 경우는 제한에 걸리지 않으며 제한에 걸린다면 비효율적인 코드가 원인일 것입니다.

4. 모든 UID 엔드포인트에서는 'ttl' 필드를 반환합니다. 'ttl' 필드는 캐시된 응답이 만료되기까지 남은 초를 의미합니다. 캐시된 데이터가 만료될 때까지 엔드포인트는 동일한 데이터를 반환하므로 'ttl' 필드를 활용하여 불필요한 요청을 방지하세요. 캐시 저장소로 Redis를 추천합니다.

데이터 활용에 어려움이 있으면 [Discord 서버](https://discord.gg/PcSZr5sbn3)에서 도움을 구하세요.

## API 목록

### UID 엔드포인트

#### 캐릭터 진열장 데이터와 플레이어 정보 가져오기

> https://enka.network/api/uid/618285856/

`playerInfo`, `avatarInfoList`를 응답합니다. `playerInfo`는 플레이어 계정의 기본적인 데이터입니다. `avatarInfoList`가 비어있다면, 해당 플레이어의 캐릭터 상세보기가 비공개 상태거나 전시된 캐릭터가 없다는 의미입니다.


#### 플레이어 정보 가져오기

> https://enka.network/api/uid/618285856/?info

`?info` 쿼리를 붙이면 `playerInfo`만 요청할 수 있습니다.

UID 엔드포인트에서는 다음 조건을 충족할 때 `owner` 항목을 포함합니다:
1. 요청한 플레이어가 enka.network 회원 등록이 되어 있음
2. 해당 사용자가 자신의 프로필에 UID를 등록함
3. 해당 사용자의 원신 계정 소유가 인증됨
4. 해당 사용자가 자신의 프로필을 공개 상태로 설정함

사용자 계정에 대한 자세한 내용은 아래를 참고하세요.

#### HTTP 응답 코드

앱에서 응답 코드를 적절히 처리하십시오.
```
400 = 올바르지 않은 UID 포맷
404 = 플레이어가 존재하지 않음 (미호요 서버의 응답)
424 = 게임 점검 중이거나 게임 업데이트 이후 작동할 수 없음
429 = 요청 속도 제한 (enka.network 서버 또는 미호요 서버의 응답)
500 = 서버 오류
503 = 나의 엄청난 실수
```

### 프로필 엔드포인트

웹사이트에서 계정(프로필)을 만들고 여러 원신 계정을 등록하는 것이 가능합니다. 그런 다음, 사용자는 인게임 프로필의 자기 소개에 인증 코드를 입력하여 계정 소유를 인증할 수 있습니다.

유저는 임의의 이름으로 빌드를 스냅샷 할 수 있습니다. 

> https://enka.network/api/profile/Algoinde/

유저 정보를 반환합니다.

> https://enka.network/api/profile/Algoinde/hoyos/

원신 계정과 계정의 메타데이터 목록 `"hoyos"`를 반환합니다. 계정 소유가 인증되었고 공개 상태인 경우에만 반환합니다. (유저는 계정을 비공개 상태로 설정할 수 있으며, 소유가 인증되지 않은 계정은 기본적으로 비공개 상태입니다)
응답의 각 키는 hoyo의 고유 식별자로, 캐릭터/빌드 정보를 얻기 위한 추가 요청에 사용하면 됩니다.

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/

hoyo에 대한 메타데이터를 반환합니다.

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/builds/

hoyo에 대한 저장된 빌드를 반환합니다. 이것은 객체 형식으로 이루어진 배열이며, 각 키는 캐릭터의 'avatarId'를 의미하고, 키의 순서는 특정한 순서를 의미하는 것이 아니라 특정 캐릭터에 대한 서로 다른 빌드입니다. (그래도 진열 순서를 알 수 있도록 order 항목이 존재합니다)

빌드에 `live: true` 항목이 있으면 "저장된" 빌드가 아니라 "새로고침"을 클릭했을 때  진열장에서 가져온 것임을 의미합니다. 새로고침을 클릭했을 때는 `live` 빌드가 모두 사라지고 새 빌드가 만들어집니다. 새로고침은 유저만 수행할 수 있습니다. 이 데이터는 최신 데이터가 아닙니다.

[UID 엔드포인트](#uid-엔드포인트)에서 설명한 대로, UID 엔드포인트에 요청할 때 `owner` 객체를 얻을 수 있습니다. 이 객체를 다음과 같이 활용할 수 있습니다:

`https://enka.network/api/profile/{owner.username}/hoyos/{owner.hash}/builds/`


## 데이터 구조

| Name | Description |
| :--- | :---------- |
| [playerInfo](#playerinfo) | 프로필 정보 |
| [avatarInfoList](#avatarinfolist) | 캐릭터 진열장의 모든 캐릭터별 세부 정보 |

### playerInfo

캐릭터 ID별 기본적인 데이터는 [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)을 참고하세요. <br />
더 상세한 정보는 [Characters Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json)을 참고하세요.

| Name | Description |
| :--- | :--------- | 
| nickname | 플레이어 닉네임 |
| signature | 플레이어 자기 소개 |
| worldLevel | 플레이어 월드 레벨 |
| namecardId | 명함 ID |
| finishAchievementNum | 획득한 업적 개수 |
| towerFloorIndex | 나선 비경 - 층 번호 |
| towerLevelIndex | 나선 비경 - 방 번호 |
| [showAvatarInfoList](#showavatarinfolist) | 캐릭터 ID 및 레벨 목록 |
| showNameCardIdList | 진열된 명함 ID 목록 |
| profilePicture.avatarId | 프로필 사진 캐릭터 ID |

#### showAvatarInfoList

| Name | Description |
| :--- | :--------- | 
| avatarId | 캐릭터 ID |
| level | 캐릭터 레벨 |
| costumeId | 캐릭터 코스튬 ID ([store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)에서 `"Costumes"` 항목 참고) |

### avatarInfoList

캐릭터 ID별 기본적인 데이터는 [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)을 참고하세요. <br />
더 상세한 정보는 [Characters Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json)을 참고하세요.

| Name | Description |
| :--- | :---------- |
| avatarID | 캐릭터 ID |
| talentIdList | 운명의 자리 ID 목록. <br /> 해금된 운명의 자리가 없으면 항목이 비어 있습니다. |
| [propMap](#propmap) | 캐릭터 속성 맵 |
| fightPropMap -> `{id: value}` | 캐릭터의 전투 능력치에 대한 맵. <br />[ID 정의](#fightprop)를 참고하세요. |
| skillDepotId | 캐릭터 스킬셋 ID <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`|
| inherentProudSkillList | 해금된 특성 목록 <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| 스킬 레벨 맵 <br /> [Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| [equipList](#equiplist) | 장착한 무기, 성유물 목록 |
| fetterInfo.expLevel  | 캐릭터 호감도 레벨 |

#### propMap

| Name | Description |
| :--- | :--------- |
| type | 속성 타입 ID ([속성 정의](#prop)) |
| ival | (무시) |
| val  | 수치 |

#### equipList

| Name | Description |
| :--- | :--------- |
| itemId | 장비 ID <br /> [Artifacts Data(성유물 데이터)](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Weapons Data(무기 데이터)](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[무기 전용]` | 무기 정보 |
| [reliquary](#reliquary) `[성유물 전용]` | 성유물 정보 |
| [flat](#flat) | 장비 상세 정보 |

#### weapon

무기에 대한 자세한 정보는 [Weapons Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)을 참고하세요.

| Name | Description |
| :--- | :---------- |
| level | 무기 레벨 |
| promoteLevel | 무기 돌파 레벨 |
| affixMap | 무기 재련 레벨 `[0-4]` |


#### reliquary

성유물에 대한 자세한 정보는 [Artifacts Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json)을 참고하세요.

| Name | Description |
| :--- | :---------- |
| level | 성유물 레벨 `[1-21]` |
| mainPropId | 성유물 주 옵션 ID <br /> [MainProps Data(주 옵션 데이터)](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |
| appendPropIdList | 성유물 부 옵션 ID 목록 <br /> [AppendProp Data(부 옵션 데이터)](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

#### flat

| Name | Description |
| :--- | :---------- |
| nameTextHashMap | 장비 이름 해시 <br /> [현지화](#현지화) 확인 |
| setNameTextHashMap `[성유물 전용]`| 성유물 세트 이름 해시 <br /> [현지화](#현지화) 확인 |
| rankLevel | 장비 등급 (n성) |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[성유물 전용]` | 성유물 주 옵션 |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[성유물 전용]` | 성유물 부 옵션 목록 |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[무기 전용]`| 무기 능력치 목록: 기초 공격력, 부 옵션 |
| [itemType](#itemtype) | 장비 타입: 무기 또는 성유물 |
| icon | 장비 아이콘 이름 <br /> [활용](#아이콘-및-이미지)|
| [equipType](#equiptype) `[성유물 전용]` | 성유물 장착 분류 |

#### reliquaryMainstat, reliquarySubstats, weaponStats

| Name | Description |
| :--- | :---------- |
| mainPropId / appendPropID | 장비 옵션의 이름 ([appendProp](#appendprop))|
| propValue | 수치 |

## 정의

### Prop

| Type | Description |
| :--: | :---------- |
| 1001 | 경험치 |
| 1002 | 돌파 단계 | 
| 4001 | 레벨 |

### FightProp

| Type | Description |
| :--: | :---------- |
| 1 | 기초 HP |
| 2 | HP |
| 3 | HP% |
| 4 | 기초 공격력 |
| 5 | 공격력 |
| 6 | 공격력% |
| 7 | 기초 방어력 |
| 8 | 방어력 |
| 9 | 방어력% |
| 10 | 기초 이동 속도 |
| 11 | 이동 속도% |
| 20 | 치명타 확률 |
| 22 | 치명타 피해 |
| 23 | 원소 충전 효율 |
| 26 | 치유 보너스 |
| 27 | 받는 치유 보너스 |
| 28 | 원소 마스터리 |
| 29 | 물리 내성 |
| 30 | 물리 피해 보너스 |
| 40 | 불 원소 피해 보너스 |
| 41 | 번개 원소 피해 보너스 |
| 42 | 물 원소 피해 보너스 |
| 43 | 풀 원소 피해 보너스 |
| 44 | 바람 원소 피해 보너스 |
| 45 | 바위 원소 피해 보너스 |
| 46 | 얼음 원소 피해 보너스 |
| 50 | 불 원소 내성 |
| 51 | 번개 원소 내성 |
| 52 | 물 원소 내성 |
| 53 | 풀 원소 내성 |
| 54 | 바람 원소 내성 |
| 55 | 바위 원소 내성 |
| 56 | 얼음 원소 내성 |
| 70 | 필요 불 원소 에너지 |
| 71 | 필요 번개 원소 에너지 |
| 72 | 필요 물 원소 에너지 |
| 73 | 필요 풀 원소 에너지 |
| 74 | 필요 바람 원소 에너지 |
| 75 | 필요 얼음 원소 에너지 |
| 76 | 필요 바위 원소 에너지 |
| 80 | 재사용 대기시간 감소 |
| 81 | 보호막 강화 |
| 1000 | 현재 불 원소 에너지 |
| 1001 | 현재 번개 원소 에너지 |
| 1002 | 현재 물 원소 에너지 |
| 1003 | 현재 풀 원소 에너지 |
| 1004 | 현재 바람 원소 에너지 |
| 1005 | 현재 얼음 원소 에너지 |
| 1006 | 현재 바위 원소 에너지 |
| 1010 | 현재 HP |
| 2000 | 최대 HP |
| 2001 | 공격력 |
| 2002 | 방어력 |
| 2003 | 이동 속도 |
| 3025 | 원소 반응 치명타 확률 |
| 3026 | 원소 반응 치명타 피해 |
| 3027 | 원소 반응 (과부하) 치명타 확률 |
| 3028 | 원소 반응 (과부하) 치명타 피해 |
| 3029 | 원소 반응 (확산) 치명타 확률 |
| 3030 | 원소 반응 (확산) 치명타 피해 |
| 3031 | 원소 반응 (감전) 치명타 확률 |
| 3032 | 원소 반응 (감전) 치명타 피해 |
| 3033 | 원소 반응 (초전도) 치명타 확률 |
| 3034 | 원소 반응 (초전도) 치명타 피해 |
| 3035 | 원소 반응 (연소) 치명타 확률 |
| 3036 | 원소 반응 (연소) 치명타 피해 |
| 3037 | 원소 반응 (빙결 (쇄빙)) 치명타 확률 |
| 3038 | 원소 반응 (빙결 (쇄빙)) 치명타 피해 |
| 3039 | 원소 반응 (개화) 치명타 확률 |
| 3040 | 원소 반응 (개화) 치명타 피해 |
| 3041 | 원소 반응 (발화) 치명타 확률 |
| 3042 | 원소 반응 (발화) 치명타 피해 |
| 3043 | 원소 반응 (만개) 치명타 확률 |
| 3044 | 원소 반응 (만개) 치명타 피해 |
| 3045 | 기초 원소 반응 치명타 확률 |
| 3046 | 기초 원소 반응 치명타 피해 |

### ItemType

| Name | Description |
| :--- | :---------- |
| ITEM_WEAPON | 무기 |
| ITEM_RELIQUARY | 성유물 |

### EquipType

| Name | Description |
| :--- | :---------- |
| EQUIP_BRACER | 생명의 꽃 |
| EQUIP_NECKLACE | 죽음의 깃털 |
| EQUIP_SHOES | 시간의 모래 | 
| EQUIP_RING | 공간의 성배 |
| EQUIP_DRESS | 이성의 왕관 |

### AppendProp

| Name | Description |
| :--- | :---------- |
| FIGHT_PROP_BASE_ATTACK `[Weapon]` | 기초 공격력 |
| FIGHT_PROP_HP | HP |
| FIGHT_PROP_ATTACK | 공격력 |
| FIGHT_PROP_DEFENSE | 방어력 |
| FIGHT_PROP_HP_PERCENT | HP% |
| FIGHT_PROP_ATTACK_PERCENT | 공격력% |
| FIGHT_PROP_DEFENSE_PERCENT | 방어력% | 
| FIGHT_PROP_CRITICAL | 치명타 확률 |
| FIGHT_PROP_CRITICAL_HURT | 치명타 피해 |
| FIGHT_PROP_CHARGE_EFFICIENCY | 원소 충전 효율 |
| FIGHT_PROP_HEAL_ADD | 치유 보너스 |
| FIGHT_PROP_ELEMENT_MASTERY | 원소 마스터리 |
| FIGHT_PROP_PHYSICAL_ADD_HURT | 물리 피해 보너스 |
| FIGHT_PROP_FIRE_ADD_HURT | 불 원소 피해 보너스 |
| FIGHT_PROP_ELEC_ADD_HURT | 번개 원소 피해 보너스 |
| FIGHT_PROP_WATER_ADD_HURT | 물 원소 피해 보너스 |
| FIGHT_PROP_WIND_ADD_HURT | 바람 원소 피해 보너스 |
| FIGHT_PROP_ICE_ADD_HURT |  얼음 원소 피해 보너스 |
| FIGHT_PROP_ROCK_ADD_HURT | 바위 원소 피해 보너스 |
| FIGHT_PROP_GRASS_ADD_HURT | 풀 원소 피해 보너스 |

## 아이콘 및 이미지

캐릭터, 무기, 성유물 등의 아이콘은 `https://enka.network/ui/[icon_name].png` 경로로 제공됩니다. 운명의 자리 아이콘 이름은 일반적으로 `"UI_"` 또는 `"Skill_"`로 시작합니다.

예시: https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png

### 무기, 성유물

[flat](#flat)에서 `icon` 항목을 활용하세요.

### 캐릭터, 운명의 자리 및 특성

[store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)에서 캐릭터 ID별로 "UI_XXXXXX" 또는 "Skill_XXXXXX" 항목을 활용하세요.

## 현지화

[store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json)에서 `"NameTextMapHash"`, [flat](#flat)에서 `"nameTextHashMap"` 또는 `"setNameTextHashMap"`가 존재할 수 있으며 이는 캐릭터, 무기 및 성유물의 현지화 데이터를 가져오는 키로 사용할 수 있습니다.
또한 전투 능력치 이름을 키로 사용하여 [AppendProp](#appendprop)의 현지화 데이터를 얻을 수 있습니다. (`"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"` 등)

이름, 설명 등 현지화된 텍스트에 대한 자세한 정보는 [TextMap Data](https://gitlab.com/Dimbreath/AnimeGameData/-/tree/master/TextMap)를 참고하세요.

## 패키지

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Rust - https://github.com/eratou/enkanetwork-rs - [eratou](https://github.com/eratou)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)