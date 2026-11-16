# Enka.Network API - Genshin Impact

## Mục lục

- [Thông tin Cấu trúc Dữ liệu](#cấu-trúc-dữ-liệu)
- [Định nghĩa](#định-nghĩa)
- [Biểu tượng và Hình ảnh](#biểu-tượng-và-hình-ảnh)
- [Bản địa hóa](#bản-địa-hóa)


## Cấu trúc Dữ liệu

| Tên | Mô tả |
| :--- | :---------- |
| [playerInfo](#playerinfo) | Thông tin Hồ sơ |
| [avatarInfoList](#avatarinfolist) | Danh sách thông tin chi tiết cho mỗi nhân vật từ showcase |

### playerInfo

Để lấy dữ liệu cơ bản của nhân vật theo ID, hãy truy cập [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Để biết thêm thông tin bổ sung, hãy kiểm tra [Dữ liệu Nhân vật](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Tên | Mô tả |
| :--- | :--------- |
| nickname | Biệt danh Người chơi |
| signature | Chữ ký Hồ sơ |
| worldLevel | Cấp Thế giới của Người chơi |
| namecardId | ID Danh thiếp Hồ sơ |
| finishAchievementNum | Số lượng Thành tựu Đã hoàn thành |
| towerFloorIndex | Tầng La Hoàn |
| towerLevelIndex | Phòng của Tầng La Hoàn |
| [showAvatarInfoList](#showavatarinfolist) | Danh sách ID và Cấp độ Nhân vật |
| showNameCardIdList | Danh sách ID Danh thiếp |
| profilePicture.avatarId | ID Nhân vật của Ảnh đại diện |

#### showAvatarInfoList

| Tên | Mô tả |
| :--- | :--------- |
| avatarId | ID Nhân vật |
| level | Cấp độ Nhân vật |
| costumeId | ID trang phục của nhân vật. Kiểm tra `"Costumes"` trong [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) |

### avatarInfoList

Để lấy dữ liệu cơ bản của nhân vật theo ID, hãy truy cập [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json).  <br />
Để biết thêm thông tin bổ sung, hãy kiểm tra [Dữ liệu Nhân vật](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarExcelConfigData.json).

| Tên | Mô tả                                                                                                                                                                               |
| :--- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| avatarID | ID Nhân vật                                                                                                                                                                              |
| talentIdList | Danh sách ID Cung mệnh <br /> Không có dữ liệu nếu 0 Cung mệnh                                                                                                                      |
| [propMap](#propmap) | Danh sách Thuộc tính Thông tin Nhân vật                                                                                                                                                            |
| fightPropMap -> `{id: value}` | Map các Thuộc tính Chiến đấu của Nhân vật. <br />Kiểm tra [Định nghĩa cho ID](#fightprop)                                                                                                   |
| skillDepotId | ID Bộ Kỹ năng Nhân vật <br />[Dữ liệu Kỹ năng](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) ->     `"id"`                      |
| inherentProudSkillList | Danh sách ID Kỹ năng Đã mở khóa <br />[Dữ liệu Kỹ năng](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"` |
| skillLevelMap -> `{skill_id: level}`| Map các Cấp độ Kỹ năng <br /> [Dữ liệu Kỹ năng](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/AvatarSkillDepotExcelConfigData.json) -> `"inherentProudSkillOpens"`     |
| [equipList](#equiplist) | Danh sách Trang bị: Vũ khí, Thánh di vật                                                                                                                                                     |
| fetterInfo.expLevel  | Cấp độ Yêu thích Nhân vật                                                                                                                                                                |

#### propMap

| Tên | Mô tả |
| :--- | :--------- |
| type | ID Loại Thuộc tính, Kiểm tra [Định nghĩa cho ID](#prop) |
| ival | Bỏ qua nó |
| val  | Giá trị của Thuộc tính |

#### equipList

| Tên | Mô tả |
| :--- | :--------- |
| itemId | ID Trang bị <br /> [Dữ liệu Thánh di vật](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json) -> `"id"` <br /> [Dữ liệu Vũ khí](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json) -> `"id"` |
| [weapon](#weapon) `[Chỉ Vũ khí]` | Thông tin Cơ bản Vũ khí  |
| [reliquary](#reliquary) `[Chỉ Thánh di vật]` | Thông tin Cơ bản Thánh di vật  |
| [flat](#flat) | Thông tin Chi tiết của Trang bị |

#### weapon

Để biết thêm thông tin bổ sung về vũ khí, kiểm tra [Dữ liệu Vũ khí](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/WeaponExcelConfigData.json)

| Tên | Mô tả |
| :--- | :---------- |
| level | Cấp độ Vũ khí |
| promoteLevel | Cấp độ Đột phá Vũ khí |
| affixMap | Cấp độ Tinh luyện Vũ khí `[0-4]` |


#### reliquary

Để biết thêm thông tin bổ sung về thánh di vật, kiểm tra [Dữ liệu Thánh di vật](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryExcelConfigData.json)

| Tên | Mô tả |
| :--- | :---------- |
| level | Cấp độ Thánh di vật `[1-21]` |
| mainPropId | ID Dòng chính Thánh di vật <br /> [Dữ liệu MainProps](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryMainPropExcelConfigData.json) |
| appendPropIdList | Danh sách ID các dòng phụ của thánh di vật <br /> [Dữ liệu AppendProp](https://gitlab.com/Dimbreath/AnimeGameData/-/blob/master/ExcelBinOutput/ReliquaryAffixExcelConfigData.json) |

#### flat

| Tên | Mô tả |
| :--- | :---------- |
| nameTextHashMap | Hash cho Tên Trang bị <br /> Kiểm tra [Bản địa hóa](#bản-địa-hóa) |
| setNameTextHashMap `[Chỉ Thánh di vật]`| Hash cho Tên Bộ Thánh di vật <br /> Kiểm tra [Bản địa hóa](#bản-địa-hóa)|
| rankLevel | Cấp độ Hiếm của Trang bị |
| [reliquaryMainstat](#reliquarymainstat-reliquarysubstats-weaponstats) `[Chỉ Thánh di vật]` | Dòng chính Thánh di vật |
| [reliquarySubstats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Chỉ Thánh di vật]` | Danh sách Dòng phụ Thánh di vật |
| [weaponStats](#reliquarymainstat-reliquarysubstats-weaponstats) `[Chỉ Vũ khí]`| Danh sách Chỉ số Vũ khí: Tấn công Căn bản, Dòng phụ |
| [itemType](#itemtype) | Loại Trang bị: Vũ khí hoặc Thánh di vật |
| icon | Tên Biểu tượng Trang bị <br /> [Cách dùng tên biểu tượng](#biểu-tượng-và-hình-ảnh)|
| [equipType](#equiptype) `[Chỉ Thánh di vật]` | Loại Thánh di vật |

#### reliquaryMainstat, reliquarySubstats, weaponStats

| Tên | Mô tả |
| :--- | :---------- |
| mainPropId / appendPropID | Tên Thuộc tính Bổ sung Trang bị. Kiểm tra [Định nghĩa cho Tên](#appendprop)|
| propValue | Giá trị Thuộc tính |

## Định nghĩa

### Prop

| Loại | Mô tả |
| :--: | :---------- |
| 1001 | XP |
| 1002 | Đột phá |
| 4001 | Cấp độ |

### FightProp

| Loại | Mô tả                                       |
| :--: |:--------------------------------------------------|
| 1 | HP Căn bản                                           |
| 2 | HP                                                |
| 3 | HP%                                               |
| 4 | Tấn công Căn bản                                          |
| 5 | Tấn công                                               |
| 6 | Tấn công%                                              |
| 7 | Phòng thủ Căn bản                                          |
| 8 | Phòng thủ                                               |
| 9 | Phòng thủ%                                              |
| 10 | Tốc độ Căn bản                                          |
| 11 | Tốc độ%                                              |
| 20 | Tỷ lệ Bạo kích                                         |
| 22 | Sát thương Bạo kích                                |
| 23 | Hiệu quả Nạp Nguyên tố                                   |
| 26 | Tăng Trị liệu                                     |
| 27 | Tăng Trị liệu Nhận vào                            |
| 28 | Tinh thông Nguyên tố                                 |
| 29 | Kháng Vật lý                                      |
| 30 | Tăng Sát thương Vật lý                                |
| 40 | Tăng Sát thương Hỏa                                    |
| 41 | Tăng Sát thương Lôi                                 |
| 42 | Tăng Sát thương Thủy                                   |
| 43 | Tăng Sát thương Thảo                                  |
| 44 | Tăng Sát thương Phong                                   |
| 45 | Tăng Sát thương Nham                                     |
| 46 | Tăng Sát thương Băng                                    |
| 50 | Kháng Hỏa                                          |
| 51 | Kháng Lôi                                       |
| 52 | Kháng Thủy                                         |
| 53 | Kháng Thảo                                        |
| 54 | Kháng Phong                                         |
| 55 | Kháng Nham                                           |
| 56 | Kháng Băng                                          |
| 70 | Tiêu hao Năng lượng Hỏa                                  |
| 71 | Tiêu hao Năng lượng Lôi                               |
| 72 | Tiêu hao Năng lượng Thủy                                 |
| 73 | Tiêu hao Năng lượng Thảo                                |
| 74 | Tiêu hao Năng lượng Phong                                 |
| 75 | Tiêu hao Năng lượng Băng                                  |
| 76 | Tiêu hao Năng lượng Nham                                   |
| 77 | Năng lượng Đặc biệt Tối đa                            |
| 78 | Tiêu hao Năng lượng Đặc biệt                               |
| 80 | Giảm thời gian hồi chiêu                                |
| 81 | Hiệu quả Khiên                                   |
| 1000 | Năng lượng Hỏa Hiện tại                               |
| 1001 | Năng lượng Lôi Hiện tại                            |
| 1002 | Năng lượng Thủy Hiện tại                              |
| 1003 | Năng lượng Thảo Hiện tại                             |
| 1004 | Năng lượng Phong Hiện tại                              |
| 1005 | Năng lượng Băng Hiện tại                               |
| 1006 | Năng lượng Nham Hiện tại                                |
| 1007 | Năng lượng Đặc biệt Hiện tại                            |
| 1010 | HP Hiện tại                                        |
| 2000 | HP Tối đa                                            |
| 2001 | Tấn công                                               |
| 2002 | Phòng thủ                                               |
| 2003 | Tốc độ                                               |
| 3025 | Tỷ lệ Bạo kích phản ứng nguyên tố                      |
| 3026 | Sát thương Bạo kích phản ứng nguyên tố                       |
| 3027 | Tỷ lệ Bạo kích phản ứng nguyên tố (Quá tải)         |
| 3028 | Sát thương Bạo kích phản ứng nguyên tố (Quá tải)          |
| 3029 | Tỷ lệ Bạo kích phản ứng nguyên tố (Khuếch tán)              |
| 3030 | Sát thương Bạo kích phản ứng nguyên tố (Khuếch tán)               |
| 3031 | Tỷ lệ Bạo kích phản ứng nguyên tố (Điện cảm)    |
| 3032 | Sát thương Bạo kích phản ứng nguyên tố (Điện cảm)     |
| 3033 | Tỷ lệ Bạo kích phản ứng nguyên tố (Siêu dẫn)       |
| 3034 | Sát thương Bạo kích phản ứng nguyên tố (Siêu dẫn)        |
| 3035 | Tỷ lệ Bạo kích phản ứng nguyên tố (Thiêu đốt)               |
| 3036 | Sát thương Bạo kích phản ứng nguyên tố (Thiêu đốt)                |
| 3037 | Tỷ lệ Bạo kích phản ứng nguyên tố (Đóng băng (Phá băng)) |
| 3038 | Sát thương Bạo kích phản ứng nguyên tố (Đóng băng (Phá băng))  |
| 3039 | Tỷ lệ Bạo kích phản ứng nguyên tố (Sum suê)              |
| 3040 | Sát thương Bạo kích phản ứng nguyên tố (Sum suê)               |
| 3041 | Tỷ lệ Bạo kích phản ứng nguyên tố (Bung tỏa)            |
| 3042 | Sát thương Bạo kích phản ứng nguyên tố (Bung tỏa)             |
| 3043 | Tỷ lệ Bạo kích phản ứng nguyên tố (Nở rộ)         |
| 3044 | Sát thương Bạo kích phản ứng nguyên tố (Nở rộ)          |
| 3045 | Tỷ lệ Bạo kích phản ứng nguyên tố Căn bản                 |
| 3046 | Sát thương Bạo kích phản ứng nguyên tố Căn bản                  |

### ItemType

| Tên | Mô tả |
| :--- | :---------- |
| ITEM_WEAPON | Vũ khí |
| ITEM_RELIQUARY | Thánh di vật |

### EquipType

| Tên | Mô tả |
| :--- | :---------- |
| EQUIP_BRACER | Hoa |
| EQUIP_NECKLACE | Lông |
| EQUIP_SHOES | Đồng hồ |
| EQUIP_RING | Ly |
| EQUIP_DRESS | Nón |

### AppendProp

| Tên | Mô tả        |
| :--- |:-------------------|
| FIGHT_PROP_BASE_ATTACK `[Vũ khí]` | Tấn công Căn bản           |
| FIGHT_PROP_HP | HP Cộng thẳng            |
| FIGHT_PROP_ATTACK | Tấn công Cộng thẳng           |
| FIGHT_PROP_DEFENSE | Phòng thủ Cộng thẳng           |
| FIGHT_PROP_HP_PERCENT | HP%                |
| FIGHT_PROP_ATTACK_PERCENT | Tấn công%               |
| FIGHT_PROP_DEFENSE_PERCENT | Phòng thủ%               |
| FIGHT_PROP_CRITICAL | Tỷ lệ Bạo kích          |
| FIGHT_PROP_CRITICAL_HURT | Sát thương Bạo kích           |
| FIGHT_PROP_CHARGE_EFFICIENCY | Hiệu quả Nạp Nguyên tố    |
| FIGHT_PROP_HEAL_ADD | Tăng Trị liệu      |
| FIGHT_PROP_ELEMENT_MASTERY | Tinh thông Nguyên tố  |
| FIGHT_PROP_PHYSICAL_ADD_HURT | Tăng Sát thương Vật lý |
| FIGHT_PROP_FIRE_ADD_HURT | Tăng Sát thương Hỏa     |
| FIGHT_PROP_ELEC_ADD_HURT | Tăng Sát thương Lôi  |
| FIGHT_PROP_WATER_ADD_HURT | Tăng Sát thương Thủy    |
| FIGHT_PROP_WIND_ADD_HURT | Tăng Sát thương Phong    |
| FIGHT_PROP_ICE_ADD_HURT | Tăng Sát thương Băng     |
| FIGHT_PROP_ROCK_ADD_HURT | Tăng Sát thương Nham      |
| FIGHT_PROP_GRASS_ADD_HURT | Tăng Sát thương Thảo   |

## Biểu tượng và Hình ảnh

Bạn có thể lấy biểu tượng của nhân vật, vũ khí và thánh di vật thông qua Enka, bằng URL `https://enka.network/ui/[icon_name].png`.
Thông thường tên biểu tượng bắt đầu bằng `"UI_"` hoặc `"Skill_"` cho [thiên phú và cung mệnh nhân vật](#characters-talents-and-consts).
Ví dụ https://enka.network/ui/UI_AvatarIcon_Side_Ambor.png.

### Vũ khí và Thánh di vật

Đi tới [flat](#flat) và tìm kiếm `icon`.

### Nhân vật, Thiên phú và Cung mệnh

Đi tới [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json) và tìm kiếm bất kỳ thứ gì liên quan đến "UI_XXXXXX" hoặc "Skill_XXXXXX" theo ID nhân vật.

## Bản địa hóa

Bạn có thể nhận thấy `"NameTextMapHash"` trong [store/characters.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/characters.json), `"nameTextHashMap"` và `"setNameTextHashMap"` tại [flat](#flat) có thể được sử dụng làm khóa để lấy dữ liệu bản địa hóa cơ bản của nhân vật, vũ khí và thánh di vật từ [store/loc.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/loc.json).
Bạn cũng có thể lấy dữ liệu bản địa hóa của [AppendProp](#appendprop) bằng cách sử dụng tên thuộc tính làm khóa - `"FIGHT_PROP_HP"`, `"FIGHT_PROP_HEAL_ADD"`, v.v.

Để biết thêm thông tin bổ sung về tên, mô tả và v.v., hãy kiểm tra [Dữ liệu TextMap](https://gitlab.com/Dimbreath/AnimeGameData/-/tree/master/TextMap), chỉ bao gồm các ngôn ngữ được hỗ trợ bởi game.

## Wrappers

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Rust - https://github.com/eratou/enkanetwork-rs - [eratou](https://github.com/eratou)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)

Python - https://github.com/seriaati/enka-py - [seriaati](https://github.com/seriaati)

Java - https://github.com/kazuryyx/EnkaNetworkAPI - [kazury](https://github.com/kazuryyx)

C# - https://github.com/aliafuji/EnkaDotnet - [aliafuji](https://github.com/aliafuji)

C# - https://github.com/Carried520/EnkaSharp - [Carried520](https://github.com/Carried520)

Go - https://github.com/kirinyoku/enkanetwork-go - [kirinyoku](https://github.com/kirinyoku)
