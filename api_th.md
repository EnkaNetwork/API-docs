การแปลภาษาไทยนี้อาจไม่ถูกต้อง 100% โปรดอ้างอิงจากข้อมูลต้นฉบับได้ที่
[api.md](/api.md)

# Enka.Network - API

## เนื้อหา

- [เริ่มต้นการใช้งาน](#เริ่มต้นการใช้งาน)
- [โครงสร้างข้อมูล](#โครงสร้างข้อมูล)
- [คำนิยาม](#คำนิยาม)
- [ไอคอน และรูปภาพ](#ไอคอน-และรูปภาพ)
- [ภาษา](#ภาษา)

## เริ่มต้นการใช้งาน

คุณสามารถดึงข้อมูล JSON จากลิ้งก์ - `https://enka.network/u/[UID]/__data.json` <br />
ยกตัวอย่างเช่น https://enka.network/u/700378769/__data.json

## โครงสร้างข้อมูล

| ชื่อ | คำอธิบาย |
| :--- | :---------- |
| [playerInfo](#playerinfo) | ข้อมูลโปรไฟล์ |
| [avatarInfoList](#avatarinfolist) | ลิสข้อมูลรายละเอียดของหน้าตู้ตัวละคร |

### playerInfo

สำหรับข้อมูลเบื้องต้นของตัวละครโดยไอดีนั้น, ไปที่ [store/characters.json](/store/characters.json).  <br />
สำหรับข้อมูลเพิ่มเติม ตรวจสอบได้ที่ [Characters Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).


| ชื่อ   | คำอธิบาย |
| :--- | :--------- | 
| nickname | ชื่อเล่นของผู้เล่น |
| signature | ลายเซ็นโปรไฟล์ |
| worldLevel | ระดับโลกของผู้เล่น |
| namecardId | เลขนามบัตร |
| finishAchievementNum | จำนวนความสำเร็จที่ได้รับ |
| towerFloorIndex | ชั้น Abyss |
| towerLevelIndex | ห้อง Abyss |
| [showAvatarInfoList](#showavatarinfolist) | ลิสไอดีตัวละครและเลเวล |
| showNameCardIdList | ลิสนามบัตร |
| profilePicture.avatarID | ไอดีตัวละครของรูปโปรไฟล์ |

#### showAvatarInfoList

| ชื่อ   | คำอธิบาย    |
| :--- | :--------- | 
| avatarId |ไอดีตัวละคร |
| level | เลเวลตัวละคร |
| costumeId | ไอดีชุดคอสตูมของตัวละคร ตรวจสอบได้ที่ `"Costumes"` ใน [store/characters.json](/store/characters.json) |

### avatarInfoList

สำหรับข้อมูลเบื้องต้นของตัวละครโดยไอดีนั้น, ไปที่ [store/characters.json](/store/characters.json).  <br />
สำหรับข้อมูลเพิ่มเติม ตรวจสอบได้ที่ [Characters Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| ชื่อ   | คำอธิบาย     |
| :--- | :---------- |
| avatarID | ไอดีตัวละคร |
| talentIdList | ลำดับกลุ่มดาว <br /> หากเป็น 0 ก็คือไม่มีข้อมูล  |
| [propMap](#propmap) | คุณสมบัติของตัวละคร |
| fightPropMap -> `{id: value}` | ข้อมูลตัวละคร <br />โปรดตรวจสอบได้ที่ [คำนิยามสำหรับไอดี](#fightprop)|
| skillDepotId | เซ็ทไอดีสกิลตัวละคร <br />[Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`|
| inherentProudSkillList | ระดับการปลดล็อกพรรสวรรค์ <br />[Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| เลเวลพรรสวรรค์ <br /> [Skills Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| [equipList](#equiplist) | รายชื่ออาวุธและอาติแฟส  |
| fetterInfo.expLevel  | เลเวลความประทับใจของตัวละคร |

#### propMap

| ชื่อ   | คำอธิบาย    |
| :--- | :--------- |
| type | ไอดีชนิดคุณสมบัติ ตรวจสอบได้ที่ [Definitions for IDs](#prop) |
| ival | ไม่ต้องสนใจ |
| val  | ค่าคุณสมบัติ |

#### equipList

| ชื่อ   | คำอธิบาย    |
| :--- | :--------- |
| itemId | ไอดีไอเทม <br /> [Artifacts Data](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Weapons Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Weapon Only]` | ข้อมูลพื้นฐานของอาวุธ  |
| [reliquary](#reliquary) `[Artifact Only]` | ข้อมูลพื้นฐานของอาติแฟส  |
| [flat](#flat) | รายละเอียดข้อมูลของไอเทม |

#### weapon

สำหรับข้อมูลเพิ่มเติมเกี่ยวกับอาวุธนั้น ตรวจสอบได้ที่ [Weapons Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)

| ชื่อ   | คำอธิบาย      |
| :--- | :---------- |
| level | เลเวลอาวุธ |
| promoteLevel | ระดับเลเวลอาวุธ |
| affixMap | ระดับขัดเกลา `[0-4]` |


#### reliquary

สำหรับข้อมูลเพิ่มเติมเกี่ยวกับอาติแฟสนั้น ตรวจสอบได้ที่ [Artifacts Data](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/ReliquaryExcelConfigData.json)

| ชื่อ   | คำอธิบาย     |
| :--- | :---------- |
| level | เลเวลอาติแฟส `[1-21]` |
| mainPropId | Stats อาติแฟสหลัก <br /> [MainProps Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |

#### flat

| ชื่อ   | คำอธิบาย     |
| :--- | :---------- |
| nameTextHashMap | ไอดีชื่อไอเทม <br /> ตรวจสอบได้ที่ [ภาษา](#ภาษา) |
| setNameTextHashMap `[Artifact Only]`| ไอดีชื่อของอาติแฟส <br /> ตรวจสอบได้ที่ [ภาษา](#ภาษา)|
| rankLevel | ระดับความหายากของไอเทม |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | Stats อาติแฟสหลัก |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | ลิส Stats อาติแฟสรอง |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Weapon Only]`| ลิส Stats อาวุธ |
| [itemType](#itemtype) | ชนิดไอเทม (อาวุธ หรือ อาติแฟส) |
| icon | ไอคอนไอเทม <br /> [ไอคอน และรูปภาพ](#ไอคอน-และรูปภาพ)|
| [equipType](#equiptype) `[Artifact Only]` | ชนิดอาติแฟส |

#### reliquaryMainstat, reliquarySubstats, weaponStats

| ชื่อ   | คำอธิบาย     |
| :--- | :---------- |
| mainPropId / appendPropID | ชื่อคุณสมบัติของไอเทม ตรวจสอบใน [คำนิยามสำหรับชื่อ](#appendprop)|
| propValue | ค่าคุณสมบัติ |

## คำนิยาม

### Prop

| ชนิด | คำอธิบาย     |
| :--: | :---------- |
| 1001 | XP |
| 1002 | ระดับ | 
| 4001 | เลเวล |

### FightProp

| ชนิด | คำอธิบาย     |
| :--: | :---------- |
| 1 | พลังชีวิตพื้นฐาน |
| 4 | พลังโจมตีพื้นฐาน |
| 7 | พลังป้องกันพื้นฐาน |
| 20 | อัตราคริ |
| 22 | ความแรงคริ |
| 23 | ฟื้นฟูพลังงาน |
| 26 | โบนัสรักษา |
| 27 | โบนัสการถูกรักษา |
| 28 | ความชำนาญธาตุ |
| 29 | ความต้านทานกายภาพ |
| 30 | โบนัสความเสียหายกายภาพ |
| 40 | โบนัสความเสียหายไฟ |
| 41 | โบนัสความเสียหายไฟฟ้า |
| 42 | โบนัสความเสียหายนํ้า |
| 44 | โบนัสความเสียหายลม |
| 45 | โบนัสความเสียหายหิน |
| 46 | โบนัสความเสียหายนํ้าแข็ง |
| 50 | ความต้านทานไฟ |
| 51 | ความต้านทานไฟฟ้า |
| 52 | ความต้านทานนํ้า |
| 53 | ความต้านทานไม้ |
| 54 | ความต้านทานลม |
| 55 | ความต้านทานหิน |
| 56 | ความต้านทานนํ้าแข็ง |
| 70 | Pyro Enegry Cost |
| 71 | Electro Energy Cost |
| 72 | Hydro Energy Cost |
| 73 | Dendro Energy Cost |
| 74 | Anemo Energy Cost |
| 75 | Cryo Energy Cost |
| 76 | Geo Energy Cost |
| 2000 | พลังชีวิตสูงสุด |
| 2001 | พลังโจมตี |
| 2002 | พลังป้องกัน |

### ItemType

| ชื่อ   | คำอธิบาย    |
| :--- | :---------- |
| ITEM_WEAPON | อาวุธ |
| ITEM_RELIQUARY | อาติแฟส |

### EquipType

| ชื่อ   | คำอธิบาย    |
| :--- | :---------- |
| EQUIP_BRACER | ดอกไม้ |
| EQUIP_NECKLACE | ขนนก |
| EQUIP_SHOES | นาฬิกาทราย | 
| EQUIP_RING | ถ้วย |
| EQUIP_DRESS | มงกุฏ |

### AppendProp

| ชื่อ   | คำอธิบาย    |
| :--- | :---------- |
| FIGHT_PROP_BASE_ATTACK `[Weapon]` | พลังชีวิตพื้นฐาน |
| FIGHT_PROP_HP | พลังชีวิต |
| FIGHT_PROP_ATTACK | พลังโจมตี |
| FIGHT_PROP_DEFENSE | พลังป้องกัน |
| FIGHT_PROP_HP_PERCENT | พลังชีวิต% |
| FIGHT_PROP_ATTACK_PERCENT | พลังโจมตี% |
| FIGHT_PROP_DEFENSE_PERCENT | พลังป้องกัน% | 
| FIGHT_PROP_CRITICAL | อัตราคริ |
| FIGHT_PROP_CRITICAL_HURT | ความแรงคริ |
| FIGHT_PROP_CHARGE_EFFICIENCY | ฟื้นฟูพลังงาน |
| FIGHT_PROP_HEAL_ADD | โบนัสรักษา |
| FIGHT_PROP_ELEMENT_MASTERY | ความชำนาญธาตุ |
| FIGHT_PROP_PHYSICAL_ADD_HURT | โบนัสความเสียหายกายภาพ |
| FIGHT_PROP_FIRE_ADD_HURT | โบนัสความเสียหายไฟ |
| FIGHT_PROP_ELEC_ADD_HURT | โบนัสความเสียหายไฟฟ้า |
| FIGHT_PROP_WATER_ADD_HURT | โบนัสความเสียหายนํ้า |
| FIGHT_PROP_WIND_ADD_HURT | โบนัสความเสียหายลม |
| FIGHT_PROP_ICE_ADD_HURT |  โบนัสความเสียหายนํ้าแข็ง |
| FIGHT_PROP_ROCK_ADD_HURT | โบนัสความเสียหายหิน |

## ไอคอน และรูปภาพ

คุณสามารถดึงรูปไอคอนจาก ตัวละคร, อาวุธ และ อาติแฟสจาก Enka ได้โดยใช้ลิ้งก์ `https://enka.network/ui/[icon_name].png`
โดยปกติแล้วชื่อไอคอนจะขึ้นต้นด้วย `"UI_"` หรือ `"Skill_"` จาก [พรรสวรรค์ตัวละคร](#ตัวละคร,-พรรสวรรค์-และกลุ่มดาว)
ยกตัวอย่างเช่น https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png

### อาวุธ และอาติแฟส

ไปที่ [flat](#flat) และมองหาคำว่า `icon`.

### ตัวละคร, พรรสวรรค์ และกลุ่มดาว

ไปที่ [store/characters.json](/store/characters.json) และมองหาข้อมูลสิ่งที่เกี่ยวข้องอย่าง "UI_XXXXXX" หรือ "Skill_XXXXXX" จากไอดีตัวละคร

## ภาษา

คุณอางสังเกตุเห็นว่า `"NameTextMapHash"` ในข้อมูล [store/characters.json](/store/characters.json), `"nameTextHashMap"` และ `"setNameTextHashMap"` จาก [flat](#flat) นั้นที่จะสามารถใช้เป็นคีย์สำหรับในการหาข้อมูลภาษาจากตัวละคร, อาวุธ และ อาติแฟสจาก [store/loc.json](/store/loc.json).  
นอกจากนี้คุณยังสามารถดึงข้อมูลภาษาจาก [AppendProp](#appendprop) โดยใช้ชื่อคีย์คุณสมบัติอย่าง - `"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"` และอิ่น ๆ 


สำหรับข้อมูลเพิ่มเติมเกี่ยวกับชื่อ, คำอธิบาย และ อื่น ๆ นั้น ตรวจสอบได้ที่ [TextMap Data](https://github.com/Dimbreath/GenshinData/tree/master/TextMap) เฉพาะภาษาที่เกมส์รองรับเท่านั้น

