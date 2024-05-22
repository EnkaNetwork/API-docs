Esta traducción puede ser inexacta. Consulta el texto original si es necesario.
[api.md](/api.md)

# Enka.Network - API

## Tabla de Contenidos

- [Empezando](#empezando)
- [Información sobre la estructura de los datos](#información-sobre-la-estructura-de-los-datos)
- [Definiciones](#definiciones)
- [Iconos e Imágenes](#iconos-e-imágenes)
- [Localizaciones](#localizaciones)

## Empezando

Puedes obtener datos en formato JSON haciendo una solicitud a la URL - `https://enka.network/u/[UID]/__data.json` <br />
Por ejemplo https://enka.network/u/700378769/__data.json

## Información sobre la estructura de los datos

| Nombre | Descripción |
| :--- | :---------- |
| [playerInfo](#playerinfo) | Información del Perfil |
| [avatarInfoList](#avatarinfolist) | Lista con detallada información sobre cada personaje que muestras en el perfil |

### playerInfo

Para obtener datos básicos de personajes por la ID, ve a [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Para obtener información adicional, revisa los [Datos de Personajes](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarExcelConfigData.json).

| Nombre | Descripción |
| :--- | :--------- | 
| nickname | Nombre del jugador |
| signature | Firma del perfil |
| worldLevel | Nivel de mundo del jugador |
| namecardId | ID de la tarjeta de presentación del perfil |
| finishAchievementNum | Número de logros completados |
| towerFloorIndex | Piso del abismo |
| towerLevelIndex | Cámara del piso del abismo |
| [showAvatarInfoList](#showavatarinfolist) | Lista de IDs y niveles de los personajes |
| showNameCardIdList | Lista de IDs de las tarjetas de presentación |
| profilePicture.avatarID | ID del personaje de la foto de perfil |

#### showAvatarInfoList

| Nombre | Descripción |
| :--- | :--------- | 
| avatarId | ID del personaje |
| level | Nivel del personaje |
| costumeId | ID de la skin del personaje. Revisa `"Costumes"` en [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) |

### avatarInfoList

Para obtener datos básicos de personajes por la ID, ve a [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Para obtener información adicional, revisa los [Datos de Personajes](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarExcelConfigData.json).

| Nombre | Descripción                                                                                                                                                                                                           |
| :--- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| avatarID | ID del personaje                                                                                                                                                                                                      |
| talentIdList | Lista de las IDs de las constelaciones <br /> No hay datos si la constelación es 0                                                                                                                                    |
| [propMap](#propmap) | Lista de propiedades de información del personaje                                                                                                                                                                     |
| fightPropMap -> `{id: value}` | Mapa de las propiedades de combate del personaje. <br />Revisa la [Definición de las IDs](#fightprop)                                                                                                                 |
| skillDepotId | ID del conjunto de habilidades del personaje <br />[Datos de las Habilidades](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`               |
| inherentProudSkillList | Lista de IDs de la habilidad desbloqueada <br />[Datos de las Habilidades](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` | 
| skillLevelMap -> `{skill_id: level}`| Mapa de los niveles de las habilidades <br /> [Datos de las Habilidades](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"`   |
| [equipList](#equiplist) | Lista del equipo: Arma y Artefactos                                                                                                                                                                                   |
| fetterInfo.expLevel  | Nivel de amistad del personaje                                                                                                                                                                                        |

#### propMap

| Nombre | Descripción |
| :--- | :--------- |
| type | ID del tipo de propiedad, Revisa la [Definición de las IDs](#prop) |
| ival | Ignoralo |
| val  | Valor de la propiedad |

#### equipList

| Nombre | Descripción                                                                                                                                                                                                                                                                                          |
| :--- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| itemId | ID del quipo <br /> [Datos de los Artefactos](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Datos de las Armas](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Solo Armas]` | Información base del arma                                                                                                                                                                                                                                                                            |
| [reliquary](#reliquary) `[Solo Artefactos]` | Información base del artefacto                                                                                                                                                                                                                                                                       |
| [flat](#flat) | Información detallada del equipo                                                                                                                                                                                                                                                                     |

#### weapon

Para obtener información adicional sobre las armas, revisa los [Datos de las Armas](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/WeaponExcelConfigData.json)

| Nombre | Descripción |
| :--- | :---------- |
| level | Nivel del arma |
| promoteLevel | Nivel de ascensión del arma |
| affixMap | Nivel de refinamiento del arma `[0-4]` |


#### reliquary

Para obtener información adicional sobre los artefactos, revisa los [Datos de los Artefactos](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryExcelConfigData.json)

| Nombre | Descripción                                                                                                                                                                                    |
| :--- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| level | Nivel del artefacto `[1-21]`                                                                                                                                                                   |
| mainPropId | ID del stat principal <br /> [Datos de los MainProps](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json)                             |
| appendPropIdList | Lista de ids de los stats secundarios del artefacto <br /> [Datos de los AppendProp](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/main/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

#### flat

| Nombre | Descripción |
| :--- | :---------- |
| nameTextHashMap | Hash para el nombre del equipo <br /> Revisa [Localizaciones](#localizaciones) |
| setNameTextHashMap `[Solo Artefactos]`| Hash para el nombre del conjunto del artefacto <br /> Revisa [Localizaciones](#localizaciones) |
| rankLevel | Nivel de rareza del equipo |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Solo Artefactos]` | Stat principal del artefactos |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Solo Artefactos]` | Lista de los stats secundarios del artefacto |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Solo Armas]`| Lista de Stats del arma: ATQ Base, Stat secundario |
| [itemType](#itemtype) | Tipo de equipo: Arma o Artefacto |
| icon | Nombre del icono del equipo <br /> [Uso de los nombres de iconos](#iconos-e-imágenes)|
| [equipType](#equiptype) `[Solo Artefactos]` | Tipo de artefacto |

#### reliquaryMainstat, reliquarySubstats, weaponStats

| Nombre | Descripción |
| :--- | :---------- |
| mainPropId / appendPropID | Nombre de la propiedad añadida del equipo. Revisa la [Definición de los Nombres](#appendprop)|
| propValue | Valor de la propiedad |

## Definiciones

### Prop

| Tipo | Descripción |
| :--: | :---------- |
| 1001 | XP |
| 1002 | Ascensión | 
| 4001 | Nivel |

### FightProp

| Tipo | Descripción |
| :--: | :---------- |
| 1 | Vida Base |
| 4 | ATQ Base |
| 7 | DEF Base |
| 20 | Probabilidad de Crítico |
| 22 | Daño Crítico |
| 23 | Recarga de Energía |
| 26 | Bono de Curación |
| 27 | Bono de Curación recibido |
| 28 | Maestría Elemental |
| 29 | RES Física |
| 30 | Bono de Daño Físico |
| 40 | Bono de Daño Pyro |
| 41 | Bono de Daño Electro |
| 42 | Bono de Daño Hydro |
| 43 | Bono de Daño Dendro |
| 44 | Bono de Daño Anemo |
| 45 | Bono de Daño Geo |
| 46 | Bono de Daño Cryo |
| 50 | RES Pyro |
| 51 | RES Electro |
| 52 | RES Hydro |
| 53 | RES Dendro |
| 54 | RES Anemo |
| 55 | RES Geo |
| 56 | RES Cryo |
| 70 | Coste de Energía Pyro |
| 71 | Coste de Energía Electro |
| 72 | Coste de Energía Hydro |
| 73 | Coste de Energía Dendro |
| 74 | Coste de Energía Anemo |
| 75 | Coste de Energía Cryo |
| 76 | Coste de Energía Geo |
| 2000 | Vida Máx |
| 2001 | ATQ |
| 2002 | DEF |

### ItemType

| Nombre | Descripción |
| :--- | :---------- |
| ITEM_WEAPON | Arma |
| ITEM_RELIQUARY | Artefacto |

### EquipType

| Nombre | Descripción |
| :--- | :---------- |
| EQUIP_BRACER | Flor |
| EQUIP_NECKLACE | Pluma |
| EQUIP_SHOES | Reloj | 
| EQUIP_RING | Copa |
| EQUIP_DRESS | Anillo |

### AppendProp

| Nombre | Descripción |
| :--- | :---------- |
| FIGHT_PROP_BASE_ATTACK `[Arma]` | ATQ Base |
| FIGHT_PROP_HP | Vida plana |
| FIGHT_PROP_ATTACK | ATQ plano |
| FIGHT_PROP_DEFENSE | DEF plana |
| FIGHT_PROP_HP_PERCENT | Vida% |
| FIGHT_PROP_ATTACK_PERCENT | ATQ% |
| FIGHT_PROP_DEFENSE_PERCENT | DEF% | 
| FIGHT_PROP_CRITICAL | Probabilidad de Crítico |
| FIGHT_PROP_CRITICAL_HURT | Daño Crítico |
| FIGHT_PROP_CHARGE_EFFICIENCY | Recarga de Energía |
| FIGHT_PROP_HEAL_ADD | Bono de Curación |
| FIGHT_PROP_ELEMENT_MASTERY | Maestría Elemental |
| FIGHT_PROP_PHYSICAL_ADD_HURT | Bono de Daño Físico |
| FIGHT_PROP_FIRE_ADD_HURT | Bono de Daño Pyro |
| FIGHT_PROP_ELEC_ADD_HURT | Bono de Daño Electro |
| FIGHT_PROP_WATER_ADD_HURT | Bono de Daño Hydro |
| FIGHT_PROP_WIND_ADD_HURT | Bono de Daño Anemo |
| FIGHT_PROP_ICE_ADD_HURT |  Bono de Daño Cryo |
| FIGHT_PROP_ROCK_ADD_HURT | Bono de Daño Geo |
| FIGHT_PROP_GRASS_ADD_HURT | Bono de Daño Dendro |

## Iconos e Imágenes

Puedes obtener iconos de personajes, armas y artefactos en Enka, con la URL `https://enka.network/ui/[icon_name].png`.  
Normalmente los nombres de los iconos empiezan por `"UI_"` o `"Skill_"` para los [Talentos de los Personajes](#talentos-de-los-personajes)
Por ejemplo https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png.

### Armas y Artefactos

Ve a [flat](#flat) y busca por el `icon`.

### Personajes, Talentos y Constelaciones

Ve a [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) y busca por algo relacionado con "UI_XXXXXX" o "Skill_XXXXXX" por la ID del personaje.

## Localizaciones

A lo mejor notas que `"NameTextMapHash"` en [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json), `"nameTextHashMap"` y `"setNameTextHashMap"` en [flat](#flat) podrían ser usados como llave para obtener la localización básica de los datos de personajes, armas y artefactos de [store/loc.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/loc.json).  
También puedes obtener la localización de los datos de [AppendProp](#appendprop) usando la propiedad como llave - `"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"` etc.

Para obtener información adicional sobre nombres, descripciones, etc, revisa los [Datos de los TextMap](https://gitlab.com/Dimbreath/AnimeGameData/-/tree/main/TextMap), solo incluye idiomas soportados dentro del juego. 

## Wrappers

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Rust - https://github.com/eratou/enkanetwork-rs - [eratou](https://github.com/eratou)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)

Python - https://github.com/seriaati/enka-py - [seriaati](https://github.com/seriaati)

Java - https://github.com/kazuryyx/EnkaNetworkAPI - [kazury](https://github.com/kazuryyx)