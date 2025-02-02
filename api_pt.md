# Enka.Network - API

## Tabela de Conteúdo

- [Início](#início)
- [Informação sobre a estrutura de dados](#informação-sobre-a-estrutura-de-dados)
- [Definições](#definições)
- [Ícones e imagens](#ícones-e-imagens)
- [Localizações](#localizações)

## Início

Você pode obter dados em formato JSON fazendo uma solicitação à URL - `https://enka.network/u/[UID]/__data.json` <br />
Exemplo https://enka.network/u/700378769/__data.json

## Informação sobre a estrutura de dados

| Nome | Descrição |
| :--- | :---------- |
| [playerInfo](#playerinfo) | Informações de perfil |
| [avatarInfoList](#avatarinfolist) | Lista com informações detalhadas sobre cada personagem visível no perfil |

### playerInfo

Para obter dados básicos de personagens através do ID, vá para [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Para obter informação adicional, cheque [Dados de Personagens](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Nome | Descrição |
| :--- | :--------- |
| nickname | Nome do jogador |
| signature | Assinatura do perfil |
| worldLevel | Nível de mundo do jogador |
| namecardId | ID do card do perfil |
| finishAchievementNum | Número de conquistas adquiridas |
| towerFloorIndex | Piso do abismo |
| towerLevelIndex | Câmara de piso do abismo |
| [showAvatarInfoList](#showavatarinfolist) | Lista de IDs e níveis de personagens |
| showNameCardIdList | Lista de IDs dos cards de perfil |
| profilePicture.avatarID | ID do personagem da foto de perfil |

#### showAvatarInfoList

| Nome | Descrição |
| :--- | :--------- |
| avatarId | ID do personagem |
| level | Nível do personagem |
| costumeId | ID da skin do personagem. Consulte `"Costumes"` em [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) |

### avatarInfoList

Para obter dados básicos de personagens pelo ID, acesse [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Para obter informação adicional, consulte [Dados de Personagens](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Nome | Descrição |
| :--- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| avatarID | ID do personagem |
| talentIdList | Lista dos IDs das constelações <br /> Não há dados se a constelação for 0 |
| [propMap](#propmap) | Lista de propriedades de informação do personagem |
| fightPropMap -> `{id: value}` | Mapa das propriedades de combate do personagem. <br />Consulte a [Definição das IDs](#fightprop) |
| skillDepotId | ID do conjunto de habilidades do personagem <br />[Dados de Habilidades](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"id"` |
| inherentProudSkillList | Lista de IDs das habilidades desbloqueadas <br />[Dados de Habilidades](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| skillLevelMap -> `{skill_id: level}`| Mapa dos níveis das habilidades <br /> [Dados de Habilidades](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| [equipList](#equiplist) | Lista de equipamentos: Arma e Artefatos |
| fetterInfo.expLevel  | Nível de amizade do personagem |

#### propMap

| Nome | Descrição |
| :--- | :--------- |
| type | ID do tipo de propriedade, consulte a [Definição das IDs](#prop) |
| ival | Ignore |
| val  | Valor da propriedade |

#### equipList

| Nome | Descrição |
| :--- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| itemId | ID do equipamento <br /> [Dados dos Artefatos](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Dados das Armas](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Somente Armas]` | Informação básica da arma |
| [reliquary](#reliquary) `[Somente Artefatos]` | Informação básica do artefato |
| [flat](#flat) | Informação detalhada do equipamento |

### Definições

#### Prop

| Tipo | Descrição |
| :--: | :---------- |
| 1001 | XP |
| 1002 | Ascensão |
| 4001 | Nível |

#### FightProp

| Tipo | Descrição |
| :--: | :---------- |
| 1 | Vida Base |
| 4 | ATQ Base |
| 7 | DEF Base |
| 20 | Taxa de Crítico |
| 22 | Dano Crítico |
| 23 | Recarga de Energia |
| 26 | Bônus de Cura |
| 27 | Bônus de Cura Recebida |
| 28 | Proficiência Elemental |
| 29 | RES Física |
| 30 | Bônus de Dano Físico |
| 40 | Bônus de Dano Pyro |
| 41 | Bônus de Dano Electro |
| 42 | Bônus de Dano Hydro |
| 43 | Bônus de Dano Dendro |
| 44 | Bônus de Dano Anemo |
| 45 | Bônus de Dano Geo |
| 46 | Bônus de Dano Cryo |
| 50 | RES Pyro |
| 51 | RES Electro |
| 52 | RES Hydro |
| 53 | RES Dendro |
| 54 | RES Anemo |
| 55 | RES Geo |
| 56 | RES Cryo |
| 70 | Custo de Energia Pyro |
| 71 | Custo de Energia Electro |
| 72 | Custo de Energia Hydro |
| 73 | Custo de Energia Dendro |
| 74 | Custo de Energia Anemo |
| 75 | Custo de Energia Cryo |
| 76 | Custo de Energia Geo |
| 2000 | Vida Máx |
| 2001 | ATQ |
| 2002 | DEF |

### ItemType

| Nome | Descrição |
| :--- | :---------- |
| ITEM_WEAPON | Arma |
| ITEM_RELIQUARY | Artefato |

### EquipType

| Nome | Descrição |
| :--- | :---------- |
| EQUIP_BRACER | Flor |
| EQUIP_NECKLACE | Pena |
| EQUIP_SHOES | Relógio | 
| EQUIP_RING | Cálice |
| EQUIP_DRESS | Tiara |

### AppendProp

| Nome | Descrição |
| :--- | :---------- |
| FIGHT_PROP_BASE_ATTACK `[Arma]` | ATQ Base |
| FIGHT_PROP_HP | Vida plana |
| FIGHT_PROP_ATTACK | ATQ flat |
| FIGHT_PROP_DEFENSE | DEF flat |
| FIGHT_PROP_HP_PERCENT | Vida% |
| FIGHT_PROP_ATTACK_PERCENT | ATQ% |
| FIGHT_PROP_DEFENSE_PERCENT | DEF% | 
| FIGHT_PROP_CRITICAL | Taxa Crítica |
| FIGHT_PROP_CRITICAL_HURT | Dano Crítico |
| FIGHT_PROP_CHARGE_EFFICIENCY | Recarga de Energia |
| FIGHT_PROP_HEAL_ADD | Bônus de Cura |
| FIGHT_PROP_ELEMENT_MASTERY | Proficiência Elemental |
| FIGHT_PROP_PHYSICAL_ADD_HURT | Bônus de Dano Físico |
| FIGHT_PROP_FIRE_ADD_HURT | Bônus de Dano Pyro |
| FIGHT_PROP_ELEC_ADD_HURT | Bônus de Dano Electro |
| FIGHT_PROP_WATER_ADD_HURT | Bônus de Dano Hydro |
| FIGHT_PROP_WIND_ADD_HURT | Bônus de Dano Anemo |
| FIGHT_PROP_ICE_ADD_HURT | Bônus de Dano Cryo |
| FIGHT_PROP_ROCK_ADD_HURT | Bônus de Dano Geo |
| FIGHT_PROP_GRASS_ADD_HURT | Bônus de Dano Dendro |

## Ícones e Imagens

Você pode obter ícones de personagens, armas e artefatos no Enka, com a URL `https://enka.network/ui/[icon_name].png`.  
Normalmente, os nomes dos ícones começam com `"UI_"` ou `"Skill_"` para os [Talentos dos Personagens](#talentos-dos-personagens).
Por exemplo: https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png.

### Armas e Artefatos

Vá até [flat](#flat) e procure pelo `icon`.

### Personagens, Talentos e Constelações

Vá até [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) e procure por algo relacionado com "UI_XXXXXX" ou "Skill_XXXXXX" pela ID do personagem.

## Localizações

Talvez você perceba que `"NameTextMapHash"` em [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json), `"nameTextHashMap"` e `"setNameTextHashMap"` em [flat](#flat) podem ser usados como chave para obter a localização básica dos dados de personagens, armas e artefatos de [store/loc.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/loc.json).  
Você também pode obter a localização dos dados de [AppendProp](#appendprop) usando a propriedade como chave - `"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"`, etc.

Para obter informações adicionais sobre nomes, descrições, etc., consulte os [Dados do TextMap](https://gitlab.com/Dimbreath/AnimeGameData/-/tree/master/TextMap), que incluem apenas idiomas suportados dentro do jogo. 

## Wrappers

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Rust - https://github.com/eratou/enkanetwork-rs - [eratou](https://github.com/eratou)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)

Python - https://github.com/seriaati/enka-py - [seriaati](https://github.com/seriaati)

Java - https://github.com/kazuryyx/EnkaNetworkAPI - [kazury](https://github.com/kazuryyx)

