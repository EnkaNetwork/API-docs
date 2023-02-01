# Enka.Network - API

## Daftar isi

- [Pendahuluan](#Pendahuluan)
- [Info Struktur Data](#info-struktur-data)
- [Definisi](#definisi)
- [Ikon dan Gambar](#ikon-dan-gambar)
- [Lokalisasi](#lokalisasi)

## Pendahuluan
Kamu bisa mengambil JSON-data dengan menggunakan request via URL - `https://enka.network/u/[UID]/__data.json` <br />
Contoh https://enka.network/u/700378769/__data.json

## Info Struktur Data
| Nama | Deskripsi |
| :--- | :---------- |
| [playerInfo](#playerinfo) | Info Profile |
| [avatarInfoList](#avatarinfolist) | Daftar informasi terperinci untuk setiap karakter showcase |

### playerInfo

Untuk data basic karakter menggunakan ID, pergi ke [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Untuk info data lain-lain, check di [Data Karakter](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Nama | Deskripsi |
| :--- | :--------- | 
| nickname | Nickname Pengguna |
| signature | Profile Pengenal |
| worldLevel | Level World Pengguna |
| namecardId | Profile Kartu Nama ID |
| finishAchievementNum | Nomor Penghargaan yang Sudah Komplit |
| towerFloorIndex | Lantai Abyss |
| towerLevelIndex | Lantai Abyss Chamber |
| [showAvatarInfoList](#showavatarinfolist) | List Karakter ID dan Level |
| showNameCardIdList | List Kartu Nama ID |
| profilePicture.avatarId | ID Karakter Gambar Profil |

#### showAvatarInfoList

| Nama | Deskripsi |
| :--- | :--------- | 
| avatarId | ID Karakter |
| level | Level Karakter |
| costumeId | ID Karakter Skin. Cek `"Kostum"` di [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) |

### avatarInfoList

Untuk data basic karakter menggunakan ID, pergi ke [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Untuk info data lain-lain, check di [Data Karakter](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Nama | Deskripsi |
| :--- | :---------- |
| avatarID | ID Karakter |
| talentIdList | List Konsetelasi ID <br /> Tidak ada data jika Konstelasi 0 |
| [propMap](#propmap) | Daftar Properti Info Karakter |
| fightPropMap -> `{id: value}` |  Kartu Pertarungan Properti Karakter. <br />Check the [Definitions for IDs](#fightprop)|
| skillDepotId | ID Set Keterampilan Karakter <br />[Data Keterampilan](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`|
| inherentProudSkillList | ID List Keterampilan yang sudah Terbuka <br />[Data Keterampilan](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| Peta Level Keterampilan <br /> [Data Keterampilan](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| [equipList](#equiplist) | List Peralatan: Senjata, Artefak |
| fetterInfo.expLevel  | Level Pertemanan Karakter |

#### propMap

| Nama | Deskripsi |
| :--- | :--------- |
| type | ID Tipe Properti, Cek di [Definisi untuk ID](#prop) |
| ival | Abaikan |
| val  | Nilai Properti |

#### equipList

| Nama | Deskripsi |
| :--- | :--------- |
| itemId | ID Peralatan <br /> [Data Artefak](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Weapons Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Senjata Saja]` | Info Dasar Senjata  |
| [reliquary](#reliquary) `[Artefak Saja]` | Info Dasar Artefak  |
| [flat](#flat) | Info Detail Peralatan |

#### weapon

Untuk info tambahan tentang Senjata, Cek di [Data Senjata](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)

| Nama | Deskripsi |
| :--- | :---------- |
| level | Level Senjata |
| promoteLevel | Level Kenaikan Senjata |
| affixMap | Level Perbaikan Senjata `[0-4]` |


#### reliquary

Untuk info tambahan tengan Artefak, cek di [Data Artefak](https://raw.githubusercontent.com/Dimbreath/GenshinData/master/ExcelBinOutput/ReliquaryExcelConfigData.json)

| Nama | Deskripsi |
| :--- | :---------- |
| level | Level Artefak `[1-21]` |
| mainPropId | ID Status Utama Artefak <br /> [Data Alat Peraga Utama](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |
| appendPropIdList | List ID aftefak substat <br /> [AppendProp Data](https://github.com/Dimbreath/GenshinData/blob/master/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

#### flat

| Nama | Deskripsi |
| :--- | :---------- |
| nameTextHashMap | Hash untuk Nama Peralatan <br /> Cek [Lokalisasi](#localizations) |
| setNameTextHashMap `[Artifak Saja]`| Hash untuk set Artefak <br /> Cek [Lokalisasi](#localizations)|
| rankLevel | Tingkat Kelangkaan Peralatan |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifak Saja]` | Status Utama Artefak |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifak Saja]` | List Substat Artefak |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Weapon Only]`| List Status Senjata: ATK Dasar, Substat |
| [itemType](#itemtype) | Tipe Peralatan: Senjata atau Artefak |
| icon | Equipment Icon Name <br /> [Penggunaan nama Ikon](#icons-and-images)|
| [equipType](#equiptype) `[Artifak Saja]` | Artifact Type |

#### reliquaryMainstat, reliquarySubstats, weaponStats

| Nama | Deskripsi |
| :--- | :---------- |
| mainPropId / appendPropID | Peralatan Nama Tambahan Properti. Cek di [Definisi untuk Nama](#appendprop)|
| propValue | Nilai Properti |

## Definisi

### Prop

| Tipe | Deskripsi |
| :--: | :---------- |
| 1001 | XP |
| 1002 | Kenaikan | 
| 4001 | Level |

### FightProp

| Tipe | Deskripsi |
| :--: | :---------- |
| 1 | HP Dasar |
| 2 | HP |
| 3 | HP% |
| 4 | ATK Dasar |
| 5 | ATK |
| 6 | ATK% |
| 7 | DEF Dasar |
| 8 | DEF |
| 9 | DEF% |
| 10 | SPD Dasar |
| 11 | SPD% |
| 20 | CRIT Rate |
| 22 | CRIT DMG |
| 23 | Energy Recharge |
| 26 | Bonus Penyembuhan |
| 27 | Bonus Penyembuhan yang Masuk |
| 28 | Penguasaan Elemental |
| 29 | RES Fisik |
| 30 | Bonus DMG Fisik |
| 40 | Bonus Pyro DMG |
| 41 | Bonus Electro DMG |
| 42 | Bonus Hydro DMG |
| 44 | Bonus Anemo DMG |
| 45 | Bonus Geo DMG |
| 46 | Bonus Cryo DMG |
| 50 | Pyro RES |
| 51 | Electro RES |
| 52 | Hydro RES |
| 53 | Dendro RES |
| 54 | Anemo RES |
| 55 | Geo RES |
| 56 | Cryo RES |
| 70 | Biaya Energi Pyro |
| 71 | Biaya Energi Electro |
| 72 | Biaya Energi Hydro |
| 73 | Biaya Energi Dendro |
| 74 | Biaya Energi Anemo |
| 75 | Biaya Energi Cryo |
| 76 | Biaya Energi Geo |
| 80 | Pengurangan Cooldown |
| 81 | Ketahanan Perisai |
| 1000 | Energi Pyro Saat Ini |
| 1001 | Energi Electro Saat Ini |
| 1002 | Energi Hydro Saat Ini |
| 1003 | Energi Dendro Saat Ini |
| 1004 | Energi Anemo Saat Ini |
| 1005 | Energi Cyro Saat Ini |
| 1006 | Energi Geo Saat Ini |
| 1010 | HP Saat Ini |
| 2000 | HP Maksumal |
| 2001 | ATK |
| 2002 | DEF |
| 2003 | SPD |
| 3025 | Tingkat CRIT Rate |
| 3026 | Tingkat CRIT DMG |
| 3027 | Tingkat (Overloaded) CRIT Rate |
| 3028 | Tingkat (Overloaded) CRIT DMG |
| 3029 | Tingkat (Swirl) CRIT Rate |
| 3030 | Tingkat (Swirl) CRIT DMG |
| 3031 | Tingkat (Electro-Charged) CRIT Rate |
| 3032 | Tingkat (Electro-Charged) CRIT DMG |
| 3033 | Tingkat (Superconduct) CRIT Rate |
| 3034 | Tingkat (Superconduct) CRIT DMG |
| 3035 | Tingkat (Burn) CRIT Rate |
| 3036 | Tingkat (Burn) CRIT DMG |
| 3037 | Tingkat (Frozen (Shattered)) CRIT Rate |
| 3038 | Tingkat (Frozen (Shattered)) CRIT DMG |
| 3039 | Tingkat (Bloom) CRIT Rate |
| 3040 | Tingkat (Bloom) CRIT DMG |
| 3041 | Tingkat (Burgeon) CRIT Rate |
| 3042 | Tingkat (Burgeon) CRIT DMG |
| 3043 | Tingkat (Hyperbloom) CRIT Rate |
| 3044 | Tingkat (Hyperbloom) CRIT DMG |
| 3045 | Tingkat Dasar CRIT Rate |
| 3046 | Tingkat Dasar CRIT DMG |

### ItemType

| Nama | Deskripsi |
| :--- | :---------- |
| ITEM_WEAPON | Senjata |
| ITEM_RELIQUARY | Artefak |

### EquipType

| Nama | Deskripsi |
| :--- | :---------- |
| EQUIP_BRACER | Bunga |
| EQUIP_NECKLACE | Bulu |
| EQUIP_SHOES | Pasir | 
| EQUIP_RING | Goblet |
| EQUIP_DRESS | Lingkaran |

### AppendProp

| Nama | Deskripsi |
| :--- | :---------- |
| FIGHT_PROP_BASE_ATTACK `[Senjata]` | ATK Dasar |
| FIGHT_PROP_HP | HP Datar |
| FIGHT_PROP_ATTACK | ATK Datar |
| FIGHT_PROP_DEFENSE | DEF Darat |
| FIGHT_PROP_HP_PERCENT | HP% |
| FIGHT_PROP_ATTACK_PERCENT | ATK% |
| FIGHT_PROP_DEFENSE_PERCENT | DEF% | 
| FIGHT_PROP_CRITICAL | Crit RATE |
| FIGHT_PROP_CRITICAL_HURT | Crit DMG |
| FIGHT_PROP_CHARGE_EFFICIENCY | Pengisian Ulang Energi |
| FIGHT_PROP_HEAL_ADD | Bonus Penyembuhan |
| FIGHT_PROP_ELEMENT_MASTERY | Penguasaan Elemental |
| FIGHT_PROP_PHYSICAL_ADD_HURT | Bonus DMG Fisik |
| FIGHT_PROP_FIRE_ADD_HURT | Bonus DMG Pyro |
| FIGHT_PROP_ELEC_ADD_HURT | Bonus DMG Electro |
| FIGHT_PROP_WATER_ADD_HURT | Bonus DMG Hydro |
| FIGHT_PROP_WIND_ADD_HURT | Bonus DMG Anemo |
| FIGHT_PROP_ICE_ADD_HURT |  Bonus DMG Cryo |
| FIGHT_PROP_ROCK_ADD_HURT | Bonus DMG Geo |
| FIGHT_PROP_GRASS_ADD_HURT | Bonus DMG Dendro |

## Ikon dan Gambar

Kamu bisa mendapatkan ikon karakter, senjata dan artefak via Enka, menggunakan URL `https://enka.network/ui/[icon_name].png`.  
Biasanya nama ikon di awali dengan `"UI_"` atau `"Skill_"` untuk [talenta karakter](#characters-talents-and-consts).  
Contoh https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png.

### Senjata dan Artefak

Pergi ke[flat](#flat) dan mencari `icon`.

### Karakter, Bakat dan Konst

Pergi ke [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) and look for anything related to "UI_XXXXXX" atau "Skill_XXXXXX" menggunakan karakter ID.

## Lokalisasi

Kamu mungkin meperhatikan `"NameTextMapHash"` di [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json), `"nameTextHashMap"` dan `"setNameTextHashMap"` di [flat](#flat) yang dapat digunakan sebagai kunci untuk mendapatkan data lokalisasi dasar dari karakter, senjata, dan artefak [store/loc.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/loc.json).  
Kamu juga bisa mendapatkan data lokalisasi [AppendProp](#appendprop) dengan menggunakan nama properti sebagai kunci - `"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"` dll.

Untuk info tambahan tentang nama, deskripsi, dll, periksa [TextMap Data](https://github.com/Dimbreath/GenshinData/tree/master/TextMap), hanya termasuk bahasa yang didukung oleh game. 
