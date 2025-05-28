# Enka.Network - API

## Daftar Isi

<!-- TODO -->
- [Memulai](#memulai)
- [Info Struktur Data](#info-struktur-data)
- [Definisi](#definisi)
- [Ikon dan Gambar](#ikon-dan-gambar)
- [*Localizations*](#localizations)

## Memulai

Kamu bisa memanfaatkan beberapa wrapper yang sudah dibuat oleh orang lain, atau menggunakan API langsung. Ini sangat sederhana, jadi tidak perlu membuat sendiri logika pengolahan data berdasarkan responsnya. Tantangan terbesarnya adalah saat menavigasi data *game* yang ditambang (*mining*) lalu menggabungkannya dengan data yang dikembalikan.

Cek [Wrappers](#wrappers) untuk bahasa pemrograman pilihanmu.

## Sebelum Kamu Melakukan *Request*

Beberapa aturan ketika menggunakan API:

1. Diharapkan untuk tidak mengenumerasi UID atau mencoba melakukan *query* besar-besaran dengan tujuan untuk mengisi *database*. Ada ratusan juta UID dan kamu tentu tidak akan bisa melakukannya dengan API ini. Kami dapat memberikan data *batch* di kemudian hari.

2. Dimohon untuk mengatur *custom* `User-Agent` *header* ke dalam *request*-mu agar kami dapat melacaknya dengan lebih baik sehingga dapat membantumu ketika dibutuhkan.

3. Ada *rate-limit* pada *endpoint* UID. Kalau kamu melakukan *request* terlalu cepat, maka waktu respons akan mulai melambat dan langsung mendapat kode *error* 429. Maka solusinya antara kamu mulai melakukan *request* dengan lebih pelan atau hubungi developer untuk melihat apakah memungkinkan bagimu untuk mendapat peningkatan *rate-limit*. Dalam kebanyakan kasus ini tidak dibutuhkan dan hanya menghasilkan kode yang tidak teroptimasi.

4. Sebagai catatan, semua *request* UID mengembalikan *field* `ttl`. *field* ini adalah "detik hingga permintaan *Showcase* selanjutnya akan dibuat untuk UID ini". Hingga `ttl` habis, *endpoint* akan mengembalikan data *cache*, tapi kamu masih bisa menggunakan *rate-limit*-mu jika kamu berulang kali melakukan *hit* ke *endpoint*-nya. Cobalah untuk menyimpan data pada *cache* dengan batas waktu `ttl` berdasarkan *request*, atau cegah *request* ke UID sampai `ttl`-nya habis. Kami merekomendasikan Redis untuk kasus ini.

Apabila kamu mengalami kendala selama bekerja dengan data, silakan bergabung ke [server Discord](https://discord.gg/PcSZr5sbn3) untuk bantuan.


## Daftar API

### Endpoint UID

#### Mengambil data *Showcase* beserta semua info *player*

> https://enka.network/api/uid/618285856/

Responsnya akan mengandung `playerInfo` dan `avatarInfoList`. `playerInfo` adalah data dasar tekait akun *game*. Jika `avatarInfoList` tidak ditemukan, itu artinya *Showcase* dari akun ini antara ditutup atau tidak ada karakter *game* yang mengisi *Showcase* tersebut.


#### Hanya mengambil info *player*

> https://enka.network/api/uid/618285856/?info

Dengan melampirkan `?info` ke *request*, kamu sedang sedang meminta data `playerInfo` saja. *Request* utama selalu mencoba untuk mengambil `avatarInfoList` juga; Jika kamu hanya butuh `playerInfo` saja, mohon untuk menggunakan *endpoint* ini karena akan lebih cepat untuk mendapatkan keseluruhan data tersebut.

Sebagai tambahan, kedua respons akan mengandung sebuah objek `owner` jika:

1. Pengguna memiliki sebuah akun di situs web
2. Pengguna menambahkan UID-nya ke profil
3. Pengguna memverifikasinya
4. Pengguna mengatur visibilitasnya ke "*public*"

Info lebih lanjut terkait akun pengguna ada di bawah.

#### Kode respons HTTP

Mohon pastikan untuk menanganinya dengan benar di dalam aplikasimu.
```
400 = Kesalahan format UID
404 = Pemain tidak ditemukan (Server MiHoYo yang mengatakannya)
424 = Game maintenance / ada kendala setelah pembaruan game
429 = Terkena Rate-limit (antara dari server EnkaNetwork atau dari server MiHoYo-nya)
500 = EnkaNetwork mengalami server error
503 = EnkaNetwork sedang mengalami kendala
```

### Endpoint Profil

Dimungkinkan untuk membuat akun (profil) di situs web dan melampirkan beberapa akun game ke dalamnya. Pengguna kemudian diminta untuk memverifikasi bahwa akun tersebut adalah milik mereka melalui kode verifikasi yang ditempatkan di *signature* mereka. Dengan cara ini kami dapat memastikan bahwa akun tersebut milik mereka.

Pengguna dapat melakukan "*snapshot*" *build* dengan nama khusus, yang disebut sebagai "*saved build*".

> https://enka.network/api/profile/Algoinde/

Mengambil info pengguna.

> https://enka.network/api/profile/Algoinde/hoyos/

Mengambil daftar "*hoyos*" alias akun genshin dan *metadata*-nya. Ini hanya mengembalikan akun yang `terverifikasi` dan bersifat `public` (pengguna dapat menyembunyikan akunnya; akun yang tidak terverifikasi sudah disembunyikan secara *default*). Setiap *key* di dalam respons adalah sebuah *identifier* unik dari MiHoYo yang perlu kamu gunakan untuk *request* berikutnya guna benar-benar mendapatkan informasi karakter/*build*.

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/

Mengembalikan *metadata* untuk satu akun *game*.

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/builds/

Mengembalikan *build* tersimpan untuk akun *game*. Ini adalah *array* objek, dimana *key*-nya adalah `avatarId` dari karakter dan objek di dalam *array* adalah beberapa *build* yang berbeda-beda untuk tiap-tiap karakter. *Array* ini tidak memiliki urutan tertentu (tapi memiliki sebuah *field* `order` yang perlu kamu urutkan ketika ingin menampilkan isi *array*).

Apabila sebuah *build* memiliki sebuah *field* `live: true`, itu artinya *build* tersebut bukanlah sebuah "*saved build*", tetapi merupakan sebuah *build* yang di-*fetch* dari *Showcase* ketika pengguna menekan "*refresh*". Ketika sudah di-*refresh*, semua *build* `live` yang lama akan hilang dan akan dibuatkan yang baru. Hanya penggunalah yang dapat memutuskan kapan harus melakukan *refresh* ini. Data ini TIDAK akan menjadi *up-to-date*.

Sebagaimana telah diuraikan di [Endpoint UID](#endpoint-uid), ketika membuat sebuah *request* UID, kamu bisa mendapatkan sebuah objek `owner`. Kamu bisa membangun URL-nya dengan menggunakan *field* berikut di dalam objeknya:

`https://enka.network/api/profile/{owner.username}/hoyos/{owner.hash}/builds/`



## Info Struktur Data

| Nama | Deskripsi   |
| :--- | :---------- |
| [playerInfo](#playerinfo) | Info Profil |
| [avatarInfoList](#avatarinfolist) | Daftar informasi mendetail untuk semua karakter yang ada di dalam *Showcase* |

### playerInfo

Untuk data dasar dari karakter berdasarkan ID, silakan menuju [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Untuk info tambahan, silakan cek [Characters Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Nama | Deskripsi |
| :--- | :--------- |
| nickname | *Nickname* pemain |
| signature | *Signature* profil |
| worldLevel | *World Level* pemain |
| namecardId | ID *namecard* profil |
| finishAchievementNum | Jumlah *achievement* yang diselesaikan |
| towerFloorIndex | *Abyss Floor* |
| towerLevelIndex | *Abyss Floor's Chamber* |
| [showAvatarInfoList](#showavatarinfolist) | Daftar ID karakter dan levelnya |
| showNameCardIdList | Daftar ID *namecard* |
| profilePicture.avatarId | ID karakter dari *Profile Picture* |

#### showAvatarInfoList

| Nama | Deskripsi |
| :--- | :--------- | 
| avatarId | ID karakter |
| level | Level karakter |
| costumeId | ID dari *skin* yang dimiliki karakter. Cek `"Costumes"` di [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) |

### avatarInfoList

Untuk data dasar dari karakter berdasarkan ID, silakan menuju [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Untuk info tambahan, silakan cek [Characters Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Nama | Deskripsi |
| :--- | :---------- |
| avatarID | ID karakter |
| talentIdList | Daftar ID konstelasi <br /> Tidak akan ada data jika konstelasi bernilai 0 (C0) |
| [propMap](#propmap) | Daftar properti info karakter |
| fightPropMap -> `{id: value}` |  Pemetaan dari properti *Character's Combat*. <br />Cek [Definisi untuk setiap ID](#fightprop)|
| skillDepotId | ID dari *Skill Set* milik karakter <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`|
| inherentProudSkillList | Daftar ID *skill* yang telah terbuka <br />[Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| Pemetaan dari level *skill* <br /> [Skills Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| [equipList](#equiplist) | Daftar *Equipment*: Senjata, Artefak |
| fetterInfo.expLevel  | *Friendship Level* milik karakter |

#### propMap

| Nama | Deskripsi |
| :--- | :--------- |
| type | ID dari *Property Type*, Cek [Definisi untuk setiap ID](#prop) |
| ival | Abaikan ini |
| val  | *Value* dari *Property* |

#### equipList

| Nama | Deskripsi |
| :--- | :--------- |
| itemId | ID *equipment* <br /> [Artifacts Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Weapons Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Weapon Only]` | Info dasar senjata  |
| [reliquary](#reliquary) `[Artifact Only]` |Info dasar artefak  |
| [flat](#flat) | Info lengkap *equipment* |

#### weapon

Untuk info tambahan terkait senjata, cek [Weapons Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)

| Nama | Deskripsi |
| :--- | :---------- |
| level | Level senjata |
| promoteLevel | Level *ascension* senjata |
| affixMap | Level *refinement* senjata `[0-4]` |


#### reliquary

Untuk info tambahan terkait artefak, cek [Artifacts Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json)

| Nama | Deskripsi |
| :--- | :---------- |
| level | Level artefak `[1-21]` |
| mainPropId | ID *Main Stat* artefak <br /> [MainProps Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |
| appendPropIdList | Daftar ID *subtat* artefak <br /> [AppendProp Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

#### flat

| Nama | Deskripsi |
| :--- | :---------- |
| nameTextHashMap | *Hash* untuk nama *equipment* <br /> Cek [Localizations](#localizations) |
| setNameTextHashMap `[Artifact Only]`| *Hash* untuk nama set artefak <br /> Cek [Localizations](#localizations)|
| rankLevel | Level *rarity* dari sebuah *equipment* |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | *Artifact Main Stat* |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Artifact Only]` | Daftar *Artifact Substats* |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Weapon Only]`| Daftar *Weapon Stat*: Base ATK, *Substat* |
| [itemType](#itemtype) | Tipe *Equipment*: Senjata atau Artefak |
| icon | Nama ikon *equipment* <br /> [Icon name usage](#ikon-dan-gambar)|
| [equipType](#equiptype) `[Artifact Only]` | Tipe artefak |

#### reliquaryMainstat, reliquarySubstats, weaponStats

| Nama | Deskripsi |
| :--- | :---------- |
| mainPropId / appendPropID | Nama *Equipment Append Property*. Cek [Definisi untuk setiap nama](#appendprop)|
| propValue | Nilai properti |

## Definisi

### Prop

| Tipe | Deskripsi |
| :--: | :---------- |
| 1001 | XP |
| 1002 | Ascension | 
| 4001 | Level |

### FightProp

| Tipe | Deskripsi |
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
| 43 | Dendro DMG Bonus |
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

| Nama | Deskripsi |
| :--- | :---------- |
| ITEM_WEAPON | Senjata |
| ITEM_RELIQUARY | Artefak |

### EquipType

| Nama | Deskripsi |
| :--- | :---------- |
| EQUIP_BRACER | Flower |
| EQUIP_NECKLACE | Feather |
| EQUIP_SHOES | Sands | 
| EQUIP_RING | Goblet |
| EQUIP_DRESS | Circlet |

### AppendProp

| Nama | Deskripsi |
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

## Ikon dan Gambar

Kamu bisa memperoleh ikon karakter, senjata, dan artefak melalui Enka lewat URL `https://enka.network/ui/[icon_name].png`.  
Biasanya nama ikon dimulai dengan `"UI_"` atau `"Skill_"` untuk [*talent* karakter](#karakter-talent-dan-konstelasi).  
Sebagai contoh https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png.

### Senjata dan Artefak

Pergi ke [flat](#flat) dan cari `icon`.

### Karakter, *Talent*, dan Konstelasi

Pergi ke [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) dan cari apapun yang terkait dengan "UI_XXXXXX" atau "Skill_XXXXXX" berdasarkan ID karakter.

## *Localizations*

Kamu mungkin sadar `"NameTextMapHash"` di [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json), `"nameTextHashMap"` dan `"setNameTextHashMap"` di [flat](#flat) bisa juga digunakan sebagai *key* untuk mendapatkan data *localization* dasar milik karakter, senjata, dan artefak dari [store/loc.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/loc.json).  
Kamu juga bisa mendapatkan data *localization* milik [AppendProp](#appendprop) dengan menggunakan nama properti sebagai *key* seperti `"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"` dan lain-lain.

Untuk info tambahan tentang nama-nama, deskripsi, dan lain-lain, silakan cek [TextMap Data](https://gitlab.com/Dimbreath/AnimeGameData/-/tree/master/TextMap). Data ini hanya mencakup bahasa yang didukung oleh *game*.

## Wrappers

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Rust - https://github.com/eratou/enkanetwork-rs - [eratou](https://github.com/eratou)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)

Python - https://github.com/seriaati/enka-py - [seriaati](https://github.com/seriaati)

Java - https://github.com/kazuryyx/EnkaNetworkAPI - [kazury](https://github.com/kazuryyx)

C# - https://github.com/aliafuji/EnkaDotnet - [aliafuji](https://github.com/aliafuji)

C# - https://github.com/Carried520/EnkaSharp - [Carried520](https://github.com/Carried520)