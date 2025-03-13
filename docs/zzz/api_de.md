# Enka.Network API - Zenless Zone Zero 

## Table of Content

- [Wichtige Notizen](#wichtige-notizen)
- [Datenstruktur](#datenstruktur)
- [Definitionen](#definitions)
- [Formeln](#formeln)
- [Icons und Bilder](#icons-und-bilder)
- [Lokalisierungen](#lokalisierungen)

## Wichtige Notizen

- UIDs für Waffen sowie Antriebsscheiben sind für den jeweiligen Gegenstand in einem Spielkonto einzigartig. Sie bleiben bei Verbesserungen der Antriebsscheibe/Waffe bestehen und können verwendet werden, um sie über mehrere Abfragen hinweg zu deduplizieren, wenn Sie den Überblick aller Ausrüstungen behalten möchten, die im Schaukasten angezeigt werden.

- API Antworten werden immer die minimale Anzahl an Daten haben. Um Informationen zu kriegen, müssen sie mit den JSONs in [API-docs/store/zzz](https://github.com/EnkaNetwork/API-docs/tree/master/store/zzz) arbeiten. Wenn sie noch mehr Daten brauchen, gehen sie zu [ZenlessData](https://git.mero.moe/dimbreath/ZenlessData), gepflegt von Dimbreath.

- Während sie mit Charakter, Waffen und Antriebsscheiben Statistiken arbeiten, sollten sie zu [Formeln](#formeln) gehen. Großes Dankeschön an Mero für das Reverse Engineering um die Formeln zu kriegen.

---

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
| Id | Agent ID |
| Exp | Agent Erfahrung |
| Level | Agent Level |
| PromotionLevel | Agent Beförderungsstufe |
| TalentLevel | Agent Sinnbild Level |
| SkinId | Agent Skin ID |
| CoreSkillEnhancement | Freigeschaltete Verbesserungen der Kernfähigkeit - A, B, C, D, E, F |
| TalentToggleList | Sinnbild Filmdarstellungseinstellungen |
| WeaponEffectState | W-Motor Signatureffektstatus `[0: Kein Effektstatus vorhanden, 1: AUS, 2: AN]` |
| IsHidden | Verstecktstatus des Agenten |
| ClaimedRewardList | Agent Beförderungsbelohnungen |
| ObtainmentTimestamp | Agent erhalten Datum |
| WeaponUid | W-Motor UID |
| [Weapon](#weapon) | Ausgerüsteter W-Motor | 
| SkillLevelList | Agent Skill Level Dictionary, überprüfe die Definitionen für Indexe |
| [EquippedList](#EquippedList) | Liste von ausgerüsteten Antriebsscheiben |

Notiz: Wenn der Agent Sinnbild Level 3 freigeschaltet hat, füge 2 zu allen Talenten hinzu. Wenn der Agent Sinnbild Level 5 freigeschaltet hat, füge 2 zu den bereits 2 hinzugefügten zu (4 insgesamt).

#### Weapon

Unter [Formeln](#formeln) erfahren sie, wie man tatsächliche Werte aus den Basiswerten kriegt
Für mehr Informationen, gehe zu [store/zzz/weapons.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/weapons.json)

| Name | Beschreibung |
| :--- | :--------- | 
| Uid | W-Motor UID |
| Id | W-Motor ID |
| Exp | W-Motor Exp |
| Level | W-Motor Level |
| BreakLevel | W-Motor Modifikationslevel |
| UpgradeLevel | W-Motor Levelphase |
| IsAvailable | W-Motor vorhanden |
| IsLocked | Gesperrtstatus des W-Motor |

#### EquippedList
| Name | Beschreibung |
| :--- | :--------- | 
| Slot | Slot index |
| [Equipment](#equipment) | Ausrüstungsdaten |

#### Equipment

| Name | Beschreibung |
| :--- | :--------- | 
| Uid | Antriebsscheibe UID |
| Id | Antriebsscheibe ID |
| Exp | Exp |
| Level | Antriebsscheibe Level `[0-15]` |
| BreakLevel | Anzahl der zufälligen Statistikprozesse |
| IsLocked | Gesperrtstatus der Antriebsscheibe |
| IsAvailable | Verfügbarkeitsstatus der Antriebsscheibe |
| IsTrash | Müllstatus der Antriebsscheibe |
| MainStatList | Antriebsscheibe Hauptstat, überprüfe [Stat](#Stat) für mehr Informationen |
| RandomPropertyList | Antriebsscheiben Substat List, überprüfe [Stat](#Stat) für mehr Informationen |

Notiz: Rarität von Antriebsscheiben kann unter [store/zzz/equipment.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/zzz/equipments.json) gefunden werden.

#### Stat

Überprüfe [Formeln](#formeln) um zu gucken, wie man tatsächliche Werte aus den Basiswerten kriegt

| Name | Beschreibung |
| :--- | :---------- | 
| PropertyValue | Property Basiswert |
| PropertyId | Property ID, überprüfe die [Definitionen](#property-id) für IDs |
| PropertyLevel | Anzahl der Rolls bei einem Substat |

---

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
| 12202 | Stoßkraft% |
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

### Skills

| Index | Beschreibung |
| :--- | :--------- |
| 0 | Standardattacke |
| 1 | Spezialattacke |
| 2 | Dash  |
| 3 | Ultimativ |
| 5 | Kernattacke |
| 6 | Unterstützung |

---

## Formeln

#### Agentstatistiken

Um die Basisstats von einem Agent auszurechnen, musst du [store/zzz/avatars.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/zzz/avatars.json) benutzen.  

- **Totaler Basiswert:**  
  `BaseTotalValue = BaseProps[PropertyId] + GrowthValue + PromotionValue + CoreEnhancementValue`  
- **Wachstum:**  
  `GrowthValue = (GrowthProps[PropertyId] * (Avatar.Level - 1)) / 10000`  
- **Beförderung:**  
  `PromotionValue = PromotionProps[Avatar.PromotionLevel - 1][PropertyId]`  
- **Kernverbesserung:**  
  `CoreEnhancementValue = CoreEnhancementProps[Avatar.CoreSkillEnhancement][PropertyId]`  

**NOTIZ:** Es ist empfohlen, die Ergebnisse runter zu runden bevor sie mit anderen Quellen addiert werden. 

### Spielakkurat  

#### W-Motor

Um mit W-Motor-Statistiken zu arbeiten, musst du die folgenden JSONs benutzen:
  \- [WeaponLevelTemplateTb.json](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/FileCfg/WeaponLevelTemplateTb.json)  
  \- [WeaponStarTemplateTb.json](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/FileCfg/WeaponStarTemplateTb.json)  


- **Hauptattribut:**  
  `Ergebnis = MainStat.PropertyValue * (1 + WeaponLevel.FIELD_XXX / 10000 + WeaponStar.FIELD_YYY / 10000)`  
  **Beispiel (Level 60, BreakLevel 5):**  
  `684 = 46 * (1 + 94090 / 10000 + 44610 / 10000)`  

- **Zweitattribut:**  
  `Ergebnis = SecondaryStat.PropertyValue * (1 + WeaponStar.FIELD_ZZZ / 10000)`  
  **Beispiel (BreakLevel 5):**  
  `2400 = 960 * (1 + 15000 / 10000)`  

**NOTIZ:** Der W-Motor **Stahlpfote** `[14102]` wurde in diesem Beispiel benutzt.  

#### Antriebsscheibe

Um mit Antriebsscheibenstatistiken zu arbeiten, musst du [EquipmentLevelTemplateTb.json](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/FileCfg/EquipmentLevelTemplateTb.json) benutzen.

Diese Datei gibt den Wert basierend auf seinem Level und seiner Rarität an.

- **Hauptattribut:**  
  `Ergebnis = MainStat.PropertyValue * (1 + EquipmentLevel.Field_XXX)`  
  **Beispiel (Level 14, Rarität 4):**  
  `2090 = 550 * (1 + 28000 / 10000)`  

### Ungefähr

#### W-Motor 

- **Hauptattribut:**  
    `Ergebnis = MainStat.PropertyValue * (1 + 0.1568166666666667 * Level + 0.8922 * BreakLevel)`  

- **Zweitattribut:**  
    `Ergebnis = SecondaryStat.PropertyValue * (1 + 0.3 * BreakLevel)`  

#### Antriebsscheibe

- **Hauptattribut:**  
`Ergebnis = MainStat.PropertyValue + (MainStat.PropertyValue * Level * RarityScale)`
- **Rarity Scales**
| Rarität | Skalierung |
| :----- | :------ |
| 4 | 0.2 |
| 3 | 0.25 |
| 2 | 0.3 |

---

## Icons und Bilder

Alle Symbolnamen sind in den analysierten Daten unter [API-docs/store/zzz](https://github.com/EnkaNetwork/API-docs/tree/master/store/zzz) enthalten.

Für weitere Informationen, schaue bei dem [ZenlessData](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/) Projekt vorbei.

## Lokalisierungen

Für die Namen die auf Enka.Network benutzt werden, gehe zu [store/zzz/locs.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/locs.json).

Für weitere Informationen über Namen, Beschreibung, etc. Überprüfe die [TextMap Daten](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/TextMap), enthält nur vom Spiel unterstützte Sprachen.

## Wrappers

Aktuell keine
