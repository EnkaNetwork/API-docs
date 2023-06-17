# Enka.Network - API

## Inhoudsopgave

- [Aan de slag](#aan-de-slag)
- [Informatie over gegevensstructuur](#informatie-over-gegevensstructuur)
- [Definities](#definities)
- [Pictogrammen en afbeeldingen](#pictogrammen-en-afbeeldingen)
- [Lokalisaties](#lokalisaties)

## Aan de slag

U kunt JSON-gegevens ophalen door een verzoek te doen aan de URL - `https://enka.network/api/uid/[UID]` <br />
Als voorbeeld https://enka.network/api/uid/700378769. Deze haalt de informatie van de speler en de karakters. Wilt u alleen de informatie van het profiel van de speler? Doe dan `?info` aan uw verzoek toe. Als voorbeeld `https://enka.network/api/uid/700378769?info`

## Informatie over gegevensstructuur

| Naam                              | Beschrijving                                                   |
|:----------------------------------|:---------------------------------------------------------------|
| [playerInfo](#playerinfo)         | Profiel informatie                                             |
| [avatarInfoList](#avatarinfolist) | Lijst met gedetailleerde informatie voor elk personage uit de showcase |

### playerInfo

Voor basisgegevens van karakters op ID ga naar [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Kijk voor meer informatie op de [Personages gegevens](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Naam                                      | Beschrijving                        |
|:------------------------------------------|:------------------------------------| 
| nickname                                  | Speler bijnaam                      |
| signature                                 | Speler handtekening                 |
| worldLevel                                | Speler wereld level                 |
| namecardId                                | Profiel naamkaart-ID                |
| finishAchievementNum                      | Aantal voltooide prestaties         |
| towerFloorIndex                           | Abyss verdieping                    |
| towerLevelIndex                           | Abyss verdiepings kamer             |
| [showAvatarInfoList](#showavatarinfolist) | Lijst met personage-ID's en niveaus |
| showNameCardIdList                        | Lijst met naamkaart-ID's            |
| profilePicture.avatarId                   | Personage-ID van profielfoto        |

#### showAvatarInfoList

| Naam      | Beschrijving                                                                                                                                                          |
|:----------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| avatarId  | Personage-ID                                                                                                                                                          |
| level     | Personage level                                                                                                                                                       |
| costumeId | Kleding-ID van de huidige personage. Controleer op `"Costumes"` in [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) |

### avatarInfoList

Voor basisgegevens van personages op ID kijk naar [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Kijk voor meer informatie in de [Personages gegevens](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Naam                                 | Beschrijving                                                                                                                                                                                                    |
|:-------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| avatarID                             | Personage-ID                                                                                                                                                                                                    |
| talentIdList                         | Lijst met constellaties-ID's <br /> Er zijn geen gegevens als er 0 constellaties zijn                                                                                                                           |
| [propMap](#propmap)                  | Eigenschappenlijst van de personage                                                                                                                                                                             |
| fightPropMap -> `{id: value}`        | Kaart met gevechtseigenschappen van personages. <br />Bekijk de [Definitions for IDs](#fightprop)                                                                                                               |
| skillDepotId                         | ID van de vaardighedenset van het personage <br />[Vaardigheden gegevens](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`               |
| inherentProudSkillList               | Lijst met ontgrendelde vaardigheids-ID's <br />[Vaardigheden gegevens](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}` | Kaart met vaardigheidsniveaus <br /> [Vaardigheden gegevens](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"`           |
| [equipList](#equiplist)              | Lijst met uitrustingen: Wapen, Artefacts                                                                                                                                                                        |
| fetterInfo.expLevel                  | Personage vriendschapniveau                                                                                                                                                                                     |

#### propMap

| Naam | Beschrijving                                                      |
|:-----|:------------------------------------------------------------------|
| type | ID van eigendomstype, controleer de [Definities voor ID's](#prop) |
| ival | Negeer dit                                                        |
| val  | Waarde van het eigendom                                           |

#### equipList

| Naam                                             | Beschrijving                                                                                                                                                                                                                                                                                           |
|:-------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| itemId                                           | Uitrusting-ID <br /> [Gegevens over artefacten](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Weapons Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Alleen voor wapen]`          | Wapenbasis informatie                                                                                                                                                                                                                                                                                  |
| [reliquary](#reliquary) `[Alleen voor artefact]` | Artefactenbasis informatie                                                                                                                                                                                                                                                                             |
| [flat](#flat)                                    | Gedetailleerde informatie over de uitrusting                                                                                                                                                                                                                                                           |

#### weapon

Voor meer informatie over wapens kijk in de [Gegevens over wapens](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)

| Naam         | Beschrijving                     |
|:-------------|:---------------------------------|
| level        | Wapen niveau                     |
| promoteLevel | Wapen Ascension-niveau           |
| affixMap     | Wapen verfijnings niveau `[0-4]` |


#### reliquary

Voor meer informatie over artefacten kijk in de [Gegevens over artefacten](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json)

| Naam             | Beschrijving                                                                                                                                                          |
|:-----------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| level            | Artefact level `[1-21]`                                                                                                                                               |
| mainPropId       | Artefact hoofdstatistieken-ID <br /> [MainProps Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json)              |
| appendPropIdList | Lijst met ID's van de artefact-substatistieken <br /> [AppendProp Data](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

#### flat

| Naam                                                                                             | Beschrijving                                                                               |
|:-------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------|
| nameTextHashMap                                                                                  | Hash voor uitrusting naam <br /> Check [Lokalisaties](#lokalisaties)                       |
| setNameTextHashMap `[Alleen voor artefacten]`                                                    | Hash voor artefact set naam <br /> Check [Lokalisaties](#lokalisaties)                     |
| rankLevel                                                                                        | Zeldzaamheid van de uitrusting                                                             |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Alleen voor artefacten]` | Hoofdstatistieken van de artefacten                                                        |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Alleen voor artefacten]` | Lijst met artefact substatistieken                                                         |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Alleen voor wapen]`            | Lijst met wapen statistieken: Basis ATK, substatistieken                                   |
| [itemType](#itemtype)                                                                            | Type uitrusting: wapen of artefact                                                         |
| icon                                                                                             | Naam uitrusting-pictogram <br /> [Gebruik van pictogrammen](#pictogrammen-en-afbeeldingen) |
| [equipType](#equiptype) `[Alleen voor artefacten]`                                               | Type artefact                                                                              |

#### reliquaryMainstat, reliquarySubstats, weaponStats

| Naam                      | Beschrijving |
|:--------------------------| :---------- |
| mainPropId / appendPropID | Eigenschapsnaam van uitrusting. Controleer de [Definities voor namen](#appendprop)|
| propValue                 | Eigendoms-waarde |

## Definities

### Prop

| Type | Beschrijving |
| :--: | :---------- |
| 1001 | XP |
| 1002 | Ascension |
| 4001 | Level |

### FightProp

| Type | Beschrijving |
| :--: | :---------- |
| 1 | Basis HP |
| 2 | HP |
| 3 | HP% |
| 4 | Basis ATK |
| 5 | ATK |
| 6 | ATK% |
| 7 | Basis DEF |
| 8 | DEF |
| 9 | DEF% |
| 10 | Basis SPD |
| 11 | SPD% |
| 20 | CRIT Rate |
| 22 | CRIT DMG |
| 23 | Energie opladen |
| 26 | Genezingsbonus |
| 27 | Inkomende genezingsbonus |
| 28 | Elementair meesterschap |
| 29 | Fysieke RES |
| 30 | Fysieke DMG-bonus |
| 40 | Pyro DMG-bonus |
| 41 | Electro DMG-bonus |
| 42 | Hydro DMG-bonus |
| 44 | Anemo DMG-bonus |
| 45 | Geo DMG-bonus |
| 46 | Cryo DMG-bonus |
| 50 | Pyro RES |
| 51 | Electro RES |
| 52 | Hydro RES |
| 53 | Dendro RES |
| 54 | Anemo RES |
| 55 | Geo RES |
| 56 | Cryo RES |
| 70 | Pyro Energie Kosten |
| 71 | Electro Energie Kosten |
| 72 | Hydro Energie Kosten |
| 73 | Dendro Energie Kosten |
| 74 | Anemo Energie Kosten |
| 75 | Cryo Energie Kosten |
| 76 | Geo Energie Kosten |
| 80 | Afkoelingsreductie |
| 81 | Schild sterkte |
| 1000 | Huidige Pyro Energie |
| 1001 | Huidige Electro Energie |
| 1002 | Huidige Hydro Energie |
| 1003 | Huidige Dendro Energie |
| 1004 | Huidige Anemo Energie |
| 1005 | Huidige Cryo Energie |
| 1006 | Huidige Geo Energie |
| 1010 | Huidige HP |
| 2000 | Max HP |
| 2001 | ATK |
| 2002 | DEF |
| 2003 | SPD |
| 3025 | Elementaire reactie CRIT Rate |
| 3026 | Elementaire reactie CRIT DMG |
| 3027 | Elementaire reactie (Overbelast) CRIT Rate |
| 3028 | Elementaire reactie (Overbelast) CRIT DMG |
| 3029 | Elementaire reactie (Swirl) CRIT Rate |
| 3030 | Elementaire reactie (Swirl) CRIT DMG |
| 3031 | Elementaire reactie (Elektrisch geladen) CRIT Rate |
| 3032 | Elementaire reactie (Elektrisch geladen) CRIT DMG |
| 3033 | Elementaire reactie (Superconduct) CRIT Rate |
| 3034 | Elementaire reactie (Superconduct) CRIT DMG |
| 3035 | Elementaire reactie (Branden) CRIT Rate |
| 3036 | Elementaire reactie (Branden) CRIT DMG |
| 3037 | Elementaire reactie (Bevroren (verbrijzeld)) CRIT Rate |
| 3038 | Elementaire reactie (Bevroren (verbrijzeld)) CRIT DMG |
| 3039 | Elementaire reactie (Bloeien) CRIT Rate |
| 3040 | Elementaire reactie (Bloeien) CRIT DMG |
| 3041 | Elementaire reactie (Burgeon) CRIT Rate |
| 3042 | Elementaire reactie (Burgeon) CRIT DMG |
| 3043 | Elementaire reactie (Hyperbloei) CRIT Rate |
| 3044 | Elementaire reactie (Hyperbloei) CRIT DMG |
| 3045 | Basis Elementaire reactie CRIT Rate |
| 3046 | Basis Elementaire reactie CRIT DMG |

### ItemType

| Naam | Beschrijving |
| :--- |:------------|
| ITEM_WEAPON | Wapen       |
| ITEM_RELIQUARY | Artefact    |

### EquipType

| Naam | Beschrijving |
| :--- |:-------------|
| EQUIP_BRACER | Bloem        |
| EQUIP_NECKLACE | Veer         |
| EQUIP_SHOES | Zand         | 
| EQUIP_RING | Beker        |
| EQUIP_DRESS | Hoofddeksel  |

### AppendProp

| Naam                             | Beschrijving            |
|:---------------------------------|:------------------------|
| FIGHT_PROP_BASE_ATTACK `[wapen]` | Basis ATK               |
| FIGHT_PROP_HP                    | Vlak HP                 |
| FIGHT_PROP_ATTACK                | Vlak ATK                |
| FIGHT_PROP_DEFENSE               | Vlak DEF                |
| FIGHT_PROP_HP_PERCENT            | HP%                     |
| FIGHT_PROP_ATTACK_PERCENT        | ATK%                    |
| FIGHT_PROP_DEFENSE_PERCENT       | DEF%                    | 
| FIGHT_PROP_CRITICAL              | Crit RATE               |
| FIGHT_PROP_CRITICAL_HURT         | Crit DMG                |
| FIGHT_PROP_CHARGE_EFFICIENCY     | Energie opladen         |
| FIGHT_PROP_HEAL_ADD              | Genezingsbonus          |
| FIGHT_PROP_ELEMENT_MASTERY       | Elementair meesterschap |
| FIGHT_PROP_PHYSICAL_ADD_HURT     | Fysieke DMG-bonus       |
| FIGHT_PROP_FIRE_ADD_HURT         | Pyro DMG-bonus          |
| FIGHT_PROP_ELEC_ADD_HURT         | Electro DMG-bonus       |
| FIGHT_PROP_WATER_ADD_HURT        | Hydro DMG-bonus         |
| FIGHT_PROP_WIND_ADD_HURT         | Anemo DMG-bonus         |
| FIGHT_PROP_ICE_ADD_HURT          | Cryo DMG-bonus          |
| FIGHT_PROP_ROCK_ADD_HURT         | Geo DMG-bonus           |
| FIGHT_PROP_GRASS_ADD_HURT        | Dendro DMG-bonus        |

## Pictogrammen en afbeeldingen

Je kunt pictogrammen van personages, wapens en artefacten krijgen van Enka via URL `https://enka.network/ui/[icon_name].png`.
Gewoonlijk begint de pictogramnaam met `"UI_"` of `"Skill_"` voor [personages talenten](#personages-talenten-en-constellaties).
Bijvoorbeeld https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png.

### Wapens en artefacten

Ga naar [flat](#flat) en zoek voor `icon`.

### Personages, talenten en constellaties

Ga naar [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) en zoek naar alles gerelateerd aan "UI_XXXXXX" of "Skill_XXXXXX" op karakter-ID.

## Lokalisaties

Mogelijk ziet u `"NameTextMapHash"` in [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json), `"nameTextHashMap"` en `" setNameTextHashMap"` op [flat](#flat) die kan worden gebruikt als sleutel om basis lokalisatie gegevens van personages, wapens en artefacten op te halen uit [store/loc.json](https://github.com/EnkaNetwork/API- docs/blob/master/store/loc.json).
U kunt ook lokalisatiegegevens van [AppendProp](#appendprop) verkrijgen door de eigenschapnaam als sleutel te gebruiken - `"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"` enz.

Raadpleeg voor aanvullende informatie over namen, beschrijvingen en dergelijke de [TextMap Data](https://gitlab.com/Dimbreath/AnimeGameData/-/tree/master/TextMap), bevat alleen talen die door het spel worden ondersteund.

