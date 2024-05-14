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

คุณสามารถใช้ Wrapper ที่ถูกสร้างขึ้นโดยคนอื่น หรือใช้ API โดยตรงได้ ซึ่งเป็นเรื่องที่ง่ายมาก ดังนั้นจึงเป็นเรื่องที่ไม่ใช่ยากเลยที่จะต้องจัดข้อมูลหรือ Logic ของคุณโดยจากข้อมูลตอบกลับ โดยการท้ายที่ยิ่งใหญ่ที่สุดก็คือการสำรวจข้อมูลที่ขุดขึ้นมาและการแนบข้อมูลส่งคืนด้วยวิธีถูกต้อง

โปรดดู [Wrappers](#wrappers) สำหรับภาษาที่คุณต้องการ

## ก่อนที่คุณจะส่งข้อมูล

กฏของการใช้งาน API มีอยู่ 3 ข้อ:

1. โปรดอย่าแยก UIDs หรือพยามที่จะส่งข้อมูลเข้าฐานข้อมูลเป็นจำนวนมาก เนื่องจากมี UIDs เป็นจำนวนมาก และ API นี้ไม่มีความสามารถที่จะกระทำข้างต้นได้

2. โปรดกำหนด `User-Agent` ไว้ที่ Header ของ requests เพื่อให้สามารถติดตามและให้ความช่วยเหลือในสิ่งที่เธอต้องการ

3. ใน API นี้มีการจำกัดอัตราแบบ Dynamic บน UID  - ถ้าหาก request ของคุณเร็ว คุณจะได้พบกับเวลาตอบกลับที่ช้า และส่งกลับมาด้วย 429 code นั้นหมายความว่าต้องลดความเร็วของการ request หรือติดต่อเราเพื่อให้เพิ่ม ratelimits ให้คุณ แต่ในกรณีส่วนใหญ่นั้นไม่มีความจำเป็นและเกิดจาก code ที่ส่งมานั้นไม่มีประสิทธิภาพมากพอ

4. ในทุก UID request นั้นจะได้รับ `ttl` field - field นี้คือ "จำนวนวินาทีในการแสดงตู้ Showcase ในครั้งถัดไปจาก UID นี้ " จนกว่าจะหมด โดย endpoint จะส่งคืนข้อมูลที่แคชไว้ แต่คุณจะยังคงใช้ ratelimit ของคุณหากคุณกดซ้ำหลายครั้ง โปรดลอง cache ข้อมูลหลังจากหมดเวลาของ `ttl` หรือ prevent requests ไปยัง UID จนกว่า `ttl` จะหมด เราแนะนำให้ใช้ Redis สำหรับสิ่งนี้

หากคุณประสบปัญหาในการทำงานเกี่ยวข้องกับข้อมูล [เซิร์ฟเวอร์ Discord](https://discord.gg/PcSZr5sbn3) เพื่อขอความช่วยเหลือ

## รายการ API

### UID endpoints

#### รับข้อมูลตู้ Showcase โดยข้อมูลผู้เล่นแบบเต็ม

> https://enka.network/api/uid/618285856/

การตอบสนองจะมี `playerInfo` และ `avatarInfoList` `playerInfo` เป็นข้อมูลพื้นฐานเกี่ยวกับบัญชีเกม หากไม่มี `avatarInfoList` แสดงว่า Showcase ของบัญชีนี้ถูกปิดหรือไม่มีตัวละคร

#### รับข้อมูลรายละเอียดข้อมูลผู้เล่นเท่านั้น 
> https://enka.network/api/uid/618285856/?info

การแนบ `?info` ไปกับคำขอ คุณกำลังร้องขอเฉพาะ `playerInfo` คำขอหลักพยายามรับ `avatarInfoList` เสมอเช่นกัน หากคุณต้องการเพียง `playerInfo` โปรดใช้จุดสิ้นสุดนี้ - มันจะเร็วกว่าการรับข้อมูลทั้งหมด

นอกจากนี้คำขอทั้งสองจะมี object `owner` หาก:

1. ผู้ใช้มีบัญชีบนเว็บไซต์
2. ผู้ใช้เพิ่ม UID ลงในโปรไฟล์
3. ผู้ใช้ได้ทำการ verified บัญชี
4. ผู้ใช้ตั้งค่าการมองเห็นเป็น "สาธารณะ"

ข้อมูลเพิ่มเติมเกี่ยวกับบัญชีผู้ใช้ด้านล่าง

#### HTTP response codes
โปรดให้มั่นใจว่าคุณได้ Handle สิ่งเล่านี้ในแอพของคุณอย่างเหมาะสม
```
400 = รูปแบบ UID ไม่ถูกต้อง
404 = ไม่พบผู้เล่น (เซิฟ MHY บอกแบบนี้อ่ะนะ)
424 = บำรุงรักษาเกมส์ / ทุกอย่างจะพังลงหลังจากอัพเดตตัวเกมส์
429 = ถูกจำกัด (ทั้งเซิฟของฉันและเซิฟ MHY)
500 = ข้อผิดพลาดทั่วไป
503 = ฉันเมามากมาย
```

### Endpoint โปรไฟล์ 

สามารถสร้างบัญชี (โปรไฟล์) บนเว็บไซต์และแนบบัญชีที่มีหลาย ๆ เกม จากนั้น ผู้ใช้จะต้องยืนยันว่าบัญชีนั้นเป็นของพวกเขาผ่านรหัสยืนยันที่อยู่ในลายเซ็น วิธีนี้ทำให้เรามั่นใจได้ว่าบัญชีนั้นเป็นของพวกเขา

ผู้ใช้สามารถสร้าง "snapshot" ภายใต้ชื่อที่กำหนดเอง ซึ่งเรียกว่า "saved builds"

> https://dev.enka.network/api/profile/Algoinde/

รับข้อมูลผู้ใช้

> https://dev.enka.network/api/profile/Algoinde/hoyos/

รับรายการ "hoyos" - บัญชี Genshin และข้อมูลเมตาของพวกเขา วิธีนี้ส่งคืนเฉพาะบัญชีที่ `ยืนยันแล้ว` และ `สาธารณะ` เท่านั้น (ผู้ใช้สามารถซ่อนบัญชีได้ บัญชีที่ไม่ได้รับการยืนยันจะถูกซ่อนโดยค่าเริ่มต้น) แต่ละคีย์ในการตอบกลับคือตัวระบุเฉพาะของ hoyo ซึ่งคุณต้องใช้สำหรับคำขอที่ตามมาเพื่อรับข้อมูลตัวละคร/builds information

> https://dev.enka.network/api/profile/Algoinde/hoyos/4Wjv2e/

ส่งคืนข้อมูลเมตาสำหรับ single hoyo

> https://dev.enka.network/api/profile/Algoinde/hoyos/4Wjv2e/builds/

ส่งคืน saved build สำหรับ hoyo ที่กำหนด นี่คือออบเจ็กต์ของอาร์เรย์ โดยที่คีย์คือ `avatarId` ของตัวละคร และออบเจ็กต์ในอาร์เรย์เป็นโครงสร้างที่แตกต่างกันสำหรับตัวละครที่กำหนด โดยไม่เรียงลำดับเฉพาะเจาะจง (แต่พวกมันมีฟิลด์ `order` ที่คุณต้องเรียงลำดับสำหรับ แสดง).

หากการ build มี `live: true` แสดงว่าไม่ใช่การ build ที่ "บันทึกไว้" แต่เป็นเพียงงานสร้างที่ดึงมาจากงานแสดงเมื่อพวกเขาคลิก "รีเฟรช" เมื่อรีเฟรช build `live` เก่าทั้งหมดจะหายไปและสร้างใหม่ มีเพียงผู้ใช้เท่านั้นที่ตัดสินใจว่าจะทำการรีเฟรชเมื่อใด - ข้อมูลนี้จะไม่เป็นปัจจุบัน

ตามที่ระบุไว้ใน [UID endpoints](#uid-endpoints) เมื่อคุณส่งคำขอ UID คุณจะได้รับวัตถุ `owner` คุณสามารถสร้าง URL โดยใช้ฟิลด์เหล่านี้ในวัตถุ:

`https://dev.enka.network/api/profile/{owner.username}/hoyos/{owner.hash}/builds/`

## โครงสร้างข้อมูล

| ชื่อ | คำอธิบาย |
| :--- | :---------- |
| [playerInfo](#playerinfo) | ข้อมูลโปรไฟล์ |
| [avatarInfoList](#avatarinfolist) | ลิสข้อมูลรายละเอียดของหน้าตู้ตัวละคร |

### playerInfo

สำหรับข้อมูลเบื้องต้นของตัวละครโดยไอดีนั้น, ไปที่ [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
สำหรับข้อมูลเพิ่มเติม ตรวจสอบได้ที่ [Characters Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarExcelConfigData.json).


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
| costumeId | ไอดีชุดคอสตูมของตัวละคร ตรวจสอบได้ที่ `"Costumes"` ใน [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) |

### avatarInfoList

สำหรับข้อมูลเบื้องต้นของตัวละครโดยไอดีนั้น, ไปที่ [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
สำหรับข้อมูลเพิ่มเติม ตรวจสอบได้ที่ [Characters Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarExcelConfigData.json).

| ชื่อ   | คำอธิบาย                                                                                                                                                                                |
| :--- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| avatarID | ไอดีตัวละคร                                                                                                                                                                             |
| talentIdList | ลำดับกลุ่มดาว <br /> หากเป็น 0 ก็คือไม่มีข้อมูล                                                                                                                                         |
| [propMap](#propmap) | คุณสมบัติของตัวละคร                                                                                                                                                                     |
| fightPropMap -> `{id: value}` | ข้อมูลตัวละคร <br />โปรดตรวจสอบได้ที่ [คำนิยามสำหรับไอดี](#fightprop)                                                                                                                   |
| skillDepotId | เซ็ทไอดีสกิลตัวละคร <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`                       |
| inherentProudSkillList | ระดับการปลดล็อกพรรสวรรค์ <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| skillLevelMap -> `{skill_id: level}`| เลเวลพรรสวรรค์ <br /> [Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"`          |
| [equipList](#equiplist) | รายชื่ออาวุธและอาติแฟส                                                                                                                                                                  |
| fetterInfo.expLevel  | เลเวลความประทับใจของตัวละคร                                                                                                                                                             |

#### propMap

| ชื่อ   | คำอธิบาย    |
| :--- | :--------- |
| type | ไอดีชนิดคุณสมบัติ ตรวจสอบได้ที่ [Definitions for IDs](#prop) |
| ival | ไม่ต้องสนใจ |
| val  | ค่าคุณสมบัติ |

#### equipList

| ชื่อ   | คำอธิบาย                                                                                                                                                                                                                                                                           |
| :--- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| itemId | ไอดีไอเทม <br /> [Artifacts Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Weapons Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Weapon Only]` | ข้อมูลพื้นฐานของอาวุธ                                                                                                                                                                                                                                                              |
| [reliquary](#reliquary) `[Artifact Only]` | ข้อมูลพื้นฐานของอาติแฟส                                                                                                                                                                                                                                                            |
| [flat](#flat) | รายละเอียดข้อมูลของไอเทม                                                                                                                                                                                                                                                           |

#### weapon

สำหรับข้อมูลเพิ่มเติมเกี่ยวกับอาวุธนั้น ตรวจสอบได้ที่ [Weapons Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/WeaponExcelConfigData.json)

| ชื่อ   | คำอธิบาย      |
| :--- | :---------- |
| level | เลเวลอาวุธ |
| promoteLevel | ระดับเลเวลอาวุธ |
| affixMap | ระดับขัดเกลา `[0-4]` |


#### reliquary

สำหรับข้อมูลเพิ่มเติมเกี่ยวกับอาติแฟสนั้น ตรวจสอบได้ที่ [Artifacts Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryExcelConfigData.json)

| ชื่อ   | คำอธิบาย     |
| :--- | :---------- |
| level | เลเวลอาติแฟส `[1-21]` |
| mainPropId | Stats อาติแฟสหลัก <br /> [MainProps Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |
| appendPropIdList | ลิสไอดี Stats อาติแฟสรอง <br /> [AppendProp Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

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
| 43 | โบนัสความเสียหายไม้ |
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
| 70 | อนุธาตุไฟ |
| 71 | อนุธาตุไฟฟ้า |
| 72 | อนุธาตุนํ้า |
| 73 | อนุธาตุไม้ |
| 74 | อนุธาตุลม |
| 75 | อนุธาตุนํ้าแข็ง |
| 76 | อนุธาตุหิน |
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
| FIGHT_PROP_BASE_ATTACK `[Weapon]` | พลังโจมตีพื้นฐาน |
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
| FIGHT_PROP_GRASS_ADD_HURT | โบนัสความเสียหายไม้ |

## ไอคอน และรูปภาพ

คุณสามารถดึงรูปไอคอนจาก ตัวละคร, อาวุธ และ อาติแฟสจาก Enka ได้โดยใช้ลิ้งก์ `https://enka.network/ui/[icon_name].png`
โดยปกติแล้วชื่อไอคอนจะขึ้นต้นด้วย `"UI_"` หรือ `"Skill_"` จาก [พรรสวรรค์ตัวละคร](#ตัวละคร,-พรรสวรรค์-และกลุ่มดาว)
ยกตัวอย่างเช่น https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png

### อาวุธ และอาติแฟส

ไปที่ [flat](#flat) และมองหาคำว่า `icon`.

### ตัวละคร, พรรสวรรค์ และกลุ่มดาว

ไปที่ [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) และมองหาข้อมูลสิ่งที่เกี่ยวข้องอย่าง "UI_XXXXXX" หรือ "Skill_XXXXXX" จากไอดีตัวละคร

## ภาษา

คุณอางสังเกตุเห็น `"NameTextMapHash"` ในข้อมูล [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json), `"nameTextHashMap"` และ `"setNameTextHashMap"` จาก [flat](#flat) นั้นที่จะสามารถใช้เป็นคีย์สำหรับในการหาข้อมูลภาษาจากตัวละคร, อาวุธ และ อาติแฟสจาก [store/loc.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/loc.json).  
นอกจากนี้คุณยังสามารถใช้ข้อมูลภาษาจาก [AppendProp](#appendprop) โดยใช้ชื่อคีย์คุณสมบัติอย่าง - `"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"` และอิ่น ๆ 

สำหรับข้อมูลเพิ่มเติมเกี่ยวกับชื่อ, คำอธิบาย และ อื่น ๆ นั้น ตรวจสอบได้ที่ [TextMap Data](https://gitlab.com/Dimbreath/AnimeGameData/-/tree/main/TextMap) เฉพาะภาษาที่เกมส์รองรับเท่านั้น

## Wrappers

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Rust - https://github.com/eratou/enkanetwork-rs - [eratou](https://github.com/eratou)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)

Python - https://github.com/seriaati/enka-py - [seriaati](https://github.com/seriaati)

Java - https://github.com/kazuryyx/EnkaNetworkAPI - [kazury](https://github.com/kazuryyx)