# Enka.Network API - Zenless Zone Zero 

## Table of Content

- [Datenstruktur](#datenstruktur)
- [Definitionen](#definitions)
- [Formeln](#formeln)
- [Icons und Bilder](#icons-und-bilder)
- [Lokalisierungen](#lokalisierungen)

## Datenstruktur

| Name | Beschreibung |
| :--- | :---------- |
| uid | Spieler UID |
| ttl | Anzahl der Sekunden bis zum aktualisieren von diesem Profil |
| [PlayerInfo](#playerinfo) | Profil Informationen |

#### PlayerInfo

| Name | Beschreibung |
| :--- | :--------- | 
| [SocialDetail](#socialdetail) | Soziale Informationen |
| [ShowcaseDetail](#showcasedetail) | Schaukasten Informationen |

#### SocialDetail

| Name | Beschreibung |
| :--- | :--------- | 
| Desc | Profilsignatur |
| [ProfileDetail](#profiledetail) | Profildetails |
| [MedalList](#medallist) | Liste von Abzeichen | 

#### MedalList
| Name | Beschreibung |
| :--- | :--------- |
| MedalType | [Abzeichentyp](#abzeichentyp) |
| Value | Fortschritt |
| MedalIcon | Abzeichen Symbol ID | 

### ProfileDetail

| Name | Beschreibung |
| :--- | :--------- |
| Uid | Spieler UID |
| Nickname | Spieler Nickname |
| ProfileId | Spieler Profilbild ID |
| Level | Inter-Knot Level | 
| Title | Titel Id |
| CallingCardId | Namenskärtchen ID |
| AvatarId | Hauptcharakter ID (Wise oder Belle) |

#### ShowcaseDetail

| Name | Beschreibung |
| :--- | :--------- | 
| [AvatarList](#AvatarList) | Liste von Charaktern |

### AvatarList

| Name | Beschreibung |
| :--- | :--------- | 
| Exp | Agent Erfahrung |
| Level | Agent Level |
| PromotionLevel | Agent Beförderungsstufe |
| TalentLevel | Agent Sinnbild level |
| SkinId | Agent Skin ID |
| CoreSkillEnhancement | Freigeschaltete Verbesserungen der Kernfähigkeit - A, B, C, D, F, G |
| TalentToggleList | Sinnbild Filmdarstellung Einstellungenzzz |
| WeaponEffectState | W-Engine Signatur Special Effekt Status `[0: AUS, 1: AN]` |
| ClaimedRewardList | Agent Beförderungsbelohnungen |
| ObtainmentTimestamp | Agent erhalten Datum |
| [Weapon](#weapon) | Ausgerüstete W-Engine | 
| SkillLevelList | Agent skill level dict, überprüfe die Definitionen für Indexe |
| [EquippedList](#EquippedList) | Liste von Disc Laufwerke mit UID |

#### Weapon

Unter [Formeln](#formeln) erfahren sie, wie man tatsächliche Werte aus den Basiswerten kriegt
Für mehr Info, gehe zu [store/zzz/weapons.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/weapons.json)

| Name | Beschreibung |
| :--- | :--------- | 
| Uid | W-Engine UID |
| Id | W-Engine ID |
| Exp | W-Engine Exp |
| Level | W-Engine Level |
| BreakLevel | W-Engine Modifikationslevel |
| IsAvailable | Antriebsscheibe vorhanden |
| IsLocked | Gesperrtstatus der W-Engine |
| UpgradeLevel | W-Engine Levelphase |

#### EquippedList
| Name | Beschreibung |
| :--- | :--------- | 
| Slot | Slot index |
| [Equipment](#equipment) | Equipment Daten |

#### Equipment

| Name | Beschreibung |
| :--- | :--------- | 
| Uid | Drive Laufwerk UID |
| Id | Antriebsscheibe ID |
| Exp | Exp |
| Level | Antriebsscheibe Level `[0-15]` |
| BreakLevel | Anzahl der zufälligen Statistikprozesse |
| IsLocked | Gesperrtstatus der Antriebsscheibe |
| IsAvailable | Verfügbarkeitsstatus der Antriebsscheibe |
| IsTrash | Müllstatus der Antriebsscheibe |
| MainStatList | Antriebsscheibe Hauptstat, überprüfe [Stat](#Stat) für mehr Informationen |
| RandomPropertyList | Antriebsscheiben Substat List, überprüfe [Stat](#Stat) für mehr Informationen |

#### Stat

Überprüfe [Formeln](#formeln) um zu gucken, wie man tatsächliche Werte aus den Basiswerten kriegt

| Name | Beschreibung |
| :--- | :---------- | 
| PropertyValue | Property Basiswert |
| PropertyId | Property ID, überprüfe die [Definitionen](#property-id) für IDs |
| PropertyLevel | Anzahl der Rolls bei einem Substat |

## Definitionen

### Property Id

Orientiere dich an der Tabelle unten und [store/zzz/property.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/property.json) für mehr Informationen.

| Type | Beschreibung |
| :--- | :--------- | 
| 11101 | LP `[Base]` |
| 11102 | LP% |
| 11103 | LP `[Flat]` |
| 12101 | ANG `[Base]` |
| 12102 | ANG% |
| 12103 | ANG `[Flat]` |
| 12201 | Stoßkraft `[Base]` |
| 12202 | Break% |
| 13101 | Def `[Base]` |
| 13102 | Def% |
| 13103 | Def `[Flat]` |
| 20101 | KRIT-Rate `[Base]` |
| 20103 | KRIT-Rate `[Flat]` |
| 21101 | KRIT-SCH `[Base]` |
| 21103 | KRIT-SCH `[Flat]` |
| 23101 | Durchschlagsrate `[Base]` |
| 23103 | Durchschlagsrate `[Flat]` |
| 23201 | Durchschlag `[Base]` |
| 23203 | Durchschlag `[Flat]` |
| 30501 | Energie-Regeneration `[Base]` |
| 30502 | Energie-Regeneration% |
| 30503 | Energie-Regeneration `[Flat]` |
| 31201 | Anomaliekunde `[Base]` |
| 31203 | Anomaliekunde `[Flat]` |
| 31401 | Anomalie-Kontrolle `[Base]` |
| 31402 | Anomalie-Kontrolle% |
| 31403 | Anomalie-Kontrolle `[Flat]` |
| 31501 | Phys. SCH-Bonus `[Base]` |
| 31503 | Phys. SCH-Bonus `[Flat]` |
| 31601 | Feuer-SCH-Bonus `[Base]` |
| 31603 | Feuer-SCH-Bonus `[Flat]` |
| 31701 | Eis-SCH-Bonus `[Base]` |
| 31703 | Eis-SCH-Bonus `[Flat]` |
| 31801 | Elektro-SCH-Bonus `[Base]` |
| 31803 | Elektro-SCH-Bonus `[Flat]` |
| 31901 | Äther-SCH-Bonus `[Base]` |
| 31903 | Äther-SCH-Bonus `[Flat]` |

### Rarität

| Type | Beschreibung |
| :--- | :---------- |
| 4 | S | 
| 3 | A |
| 2 | B |

### Abzeichentyp 

| Type | Beschreibung |
| :--- | :---------- |
| 1 | Shiyu-Verteidigung |
| 2 | Endloser Aufstieg |
| 3 | Gefährlicher Überfall |
| 4 | Unendliches Gefecht – Sackgasse | 

## Formeln

### Antriebsscheibe

#### Hauptattribut
```Ergebnis = MainStat.PropertyValue * (MainStat.PropertyValue * Level * RarityScale)```
#### Subattribut
```Ergebnis = PropertyValue * PropertyLevel```

#### Rarity Scales
| Rarity | Scale |
| :----- | :------ |
| 4 | 0.2 |
| 3 | 0.25 |
| 2 | 0.2 |


### W-Engine 

#### Hauptattribut

```Ergebnis = MainStat.BaseValue * (1 + 0.1568166666666667 * Level + 0.8922 * BreakLevel)```

#### Subattribut

```Ergebnis = SubStat.BaseValue * (1 + 0.3 * BreakLevel) ```

## Icons und Bilder

Alle Symbolnamen sind in den analysierten Daten unter [API-docs/store/zzz](https://github.com/EnkaNetwork/API-docs/tree/master/store/zzz) enthalten.

Für weitere Informationen, schaue bei dem [ZenlessData](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/) Projekt vorbei.

## Lokalisierungen

Für die Namen die auf Enka.Network benutzt werden, gehe zu [store/zzz/locs.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/locs.json).

Für weitere Informationen über Namen, Beschreibung, etc. Überprüfe die [TextMap Daten](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/TextMap), enthält nur vom Spiel unterstützte Sprachen.

## Wrappers

Aktuell keine
