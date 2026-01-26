# Enka.Network API - Zenless Zone Zero

## Mục lục

- [Ghi chú Quan trọng](#important-notes)
- [Cấu trúc Dữ liệu](#data-structure)
- [Định nghĩa](#definitions)
- [Công thức](#formulas)
- [Biểu tượng và Hình ảnh](#icons-and-images)
- [Bản địa hóa](#localizations)

## Ghi chú Quan trọng

- UID cho vũ khí và đĩa là duy nhất cho vật phẩm cụ thể đó trong một tài khoản game. Chúng sẽ tồn tại qua các lần nâng cấp đĩa/vũ khí và có thể được sử dụng để loại bỏ trùng lặp trên nhiều truy vấn nếu bạn muốn theo dõi tất cả trang bị trên tài khoản đã được nhìn thấy trong showcase.

- Phản hồi sẽ luôn chứa một lượng dữ liệu tối thiểu. Để lấy một số thông tin cơ bản, bạn phải làm việc với các file JSON đã phân tích trong [API-docs/store/zzz](https://github.com/EnkaNetwork/API-docs/tree/master/store/zzz). Nếu bạn cần nhiều dữ liệu hơn để làm việc, vui lòng tham khảo kho lưu trữ [ZenlessData](https://git.mero.moe/dimbreath/ZenlessData) được duy trì bởi Dimbreath.

- Trong khi làm việc với chỉ số nhân vật, vũ khí và đĩa, bạn nên tham khảo [Công thức](#formulas). Cảm ơn rất nhiều đến Mero vì đã đảo ngược kỹ thuật và lấy được các công thức thực tế được sử dụng trong game.

---

## Cấu trúc Dữ liệu

| Tên | Mô tả |
| :--- | :---------- |
| uid | UID Người chơi |
| ttl | Số giây cho đến lần cập nhật tiếp theo |
| [PlayerInfo](#playerinfo) | Thông tin Hồ sơ |

#### PlayerInfo

| Tên | Mô tả |
| :--- | :--------- |
| [SocialDetail](#socialdetail) | Chi tiết thông tin xã hội |
| [ShowcaseDetail](#showcasedetail) | Chi tiết Showcase |

#### SocialDetail

| Tên | Mô tả |
| :--- | :--------- |
| Desc | Chữ ký hồ sơ |
| [ProfileDetail](#profiledetail) | Chi tiết hồ sơ |
| [MedalList](#medallist) | Danh sách Huy hiệu |

#### MedalList
| Tên | Mô tả |
| :--- | :--------- |
| MedalType | [Loại Huy hiệu](#badge-type) |
| Value | Số Tiến độ |
| MedalIcon | ID Biểu tượng |

### ProfileDetail

| Tên | Mô tả |
| :--- | :--------- |
| Uid | UID Người chơi |
| Nickname | Biệt danh Người chơi |
| ProfileId | ID Ảnh đại diện |
| Level | Cấp Inter-Knot |
| Title | ID Danh hiệu |
| CallingCardId | ID Danh thiếp |
| AvatarId | ID Nhân vật Chính (Wise hoặc Belle) |

#### ShowcaseDetail

| Tên | Mô tả |
| :--- | :--------- |
| [AvatarList](#AvatarList) | Danh sách Nhân vật |

### AvatarList

| Tên | Mô tả |
| :--- | :--------- |
| Id | ID Người đại diện |
| Exp | Exp Người đại diện |
| Level | Cấp độ Người đại diện |
| PromotionLevel | Cấp độ Đột phá Người đại diện |
| TalentLevel | Cấp độ Mindscape (Ảnh Động) |
| SkinId | ID Trang phục Người đại diện |
| CoreSkillEnhancement | Nâng cấp Kỹ năng Cốt lõi đã mở khóa - A, B, C, D, E, F |
| TalentToggleList | Bật tắt hiển thị Mindscape Cinema (Rạp Phim Ảnh Động) |
| WeaponEffectState | Trạng thái hiệu ứng đặc biệt chữ ký W-Engine `[0: Không, 1: TẮT, 2: BẬT]` |
| IsHidden | Trạng thái ẩn của Người đại diện |
| ClaimedRewardList | Phần thưởng đột phá Người đại diện |
| ObtainmentTimestamp | Thời gian nhận Người đại diện |
| [Weapon](#weapon) | W-Engine đã trang bị |
| SkillLevelList | Dict cấp độ kỹ năng Người đại diện, kiểm tra [định nghĩa](#skills) cho các chỉ mục |
| [EquippedList](#EquippedList) | Danh sách Đĩa |

Lưu ý: Nếu người đại diện đã mở khóa mindscape 3, tăng tất cả cấp độ kỹ năng lên 2. Nếu người đại diện đã mở khóa mindscape 5, tăng tất cả cấp độ kỹ năng lên 2 trên cơ sở của lần tăng trước đó (tổng cộng 4).

#### Weapon

Kiểm tra [Công thức](#formulas) để xem cách lấy giá trị thực tế từ giá trị cơ bản
Để biết thêm thông tin, tham khảo [store/zzz/weapons.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/weapons.json)

| Tên | Mô tả |
| :--- | :--------- |
| Uid | UID W-Engine |
| Id | ID W-Engine |
| Exp | Exp W-Engine |
| Level | Cấp độ W-Engine |
| BreakLevel | Cấp độ sửa đổi W-Engine |
| UpgradeLevel | Cấp độ Giai đoạn W-Engine |
| IsAvailable | Trạng thái khả dụng của W-Engine |
| IsLocked | Trạng thái khóa của W-Engine |

#### EquippedList
| Tên | Mô tả |
| :--- | :--------- |
| Slot | Chỉ mục khe cắm |
| [Equipment](#equipment) | Dữ liệu trang bị |

#### Equipment

| Tên | Mô tả |
| :--- | :--------- |
| Uid | UID Đĩa |
| Id | ID Đĩa |
| Exp | Exp |
| Level | Cấp độ Đĩa `[0-15]` |
| BreakLevel | Số lần nhảy chỉ số ngẫu nhiên |
| IsLocked | Trạng thái đánh dấu khóa của Đĩa |
| IsAvailable | Trạng thái khả dụng của Đĩa |
| IsTrash | Trạng thái đánh dấu rác của Đĩa |
| MainStatList | Dòng chính Đĩa, kiểm tra [Stat](#Stat) để biết thêm thông tin |
| RandomPropertyList | Danh sách Dòng phụ Đĩa, kiểm tra [Stat](#Stat) để biết thêm thông tin |

Lưu ý: Độ hiếm của Đĩa có thể được tìm thấy trong [store/zzz/equipment.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/zzz/equipments.json).

#### Stat

Kiểm tra [Công thức](#formulas) để xem cách lấy giá trị thực tế từ giá trị cơ bản

| Tên | Mô tả |
| :--- | :---------- |
| PropertyValue | Giá trị Cơ bản Thuộc tính |
| PropertyId | ID Thuộc tính, kiểm tra [định nghĩa](#property-id) cho các ID |
| PropertyLevel | Số lần nhảy (roll), chỉ quan trọng nếu là dòng phụ |

---

## Định nghĩa

### Property Id

Tham khảo bảng dưới đây và [store/zzz/property.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/property.json) để biết thêm thông tin.

| Loại | Mô tả|
| :--- | :--------- |
| 11101 | HP `[Cơ bản]` |
| 11102 | HP% |
| 11103 | HP `[Cộng thẳng]` |
| 12101 | Tấn công `[Cơ bản]` |
| 12102 | Tấn công% |
| 12103 | Tấn công `[Cộng thẳng]` |
| 12201 | Xung kích `[Cơ bản]` |
| 12202 | Xung kích% |
| 13101 | Phòng thủ `[Cơ bản]` |
| 13102 | Phòng thủ% |
| 13103 | Phòng thủ `[Cộng thẳng]` |
| 20101 | Tỷ lệ Bạo kích `[Cơ bản]` |
| 20103 | Tỷ lệ Bạo kích `[Cộng thẳng]` |
| 21101 | Sát thương Bạo kích `[Cơ bản]` |
| 21103 | Sát thương Bạo kích `[Cộng thẳng]` |
| 23101 | Tỷ lệ Xuyên thấu `[Cơ bản]` |
| 23103 | Tỷ lệ Xuyên thấu `[Cộng thẳng]` |
| 23201 | Xuyên thấu  `[Cơ bản]` |
| 23203 | Xuyên thấu `[Cộng thẳng]` |
| 30501 | Tự hồi Năng lượng `[Cơ bản]` |
| 30502 | Tự hồi Năng lượng% |
| 30503 | Tự hồi Năng lượng `[Cộng thẳng]` |
| 31201 | Khống chế Bất thường `[Cơ bản]` |
| 31203 | Khống chế Bất thường `[Cộng thẳng]` |
| 31401 | Tinh thông Bất thường `[Cơ bản]` |
| 31402 | Tinh thông Bất thường% |
| 31403 | Tinh thông Bất thường `[Cộng thẳng]` |
| 31501 | Tăng Sát thương Vật lý `[Cơ bản]` |
| 31503 | Tăng Sát thương Vật lý `[Cộng thẳng]` |
| 31601 | Tăng Sát thương Hỏa `[Cơ bản]` |
| 31603 | Tăng Sát thương Hỏa `[Cộng thẳng]` |
| 31701 | Tăng Sát thương Băng `[Cơ bản]` |
| 31703 | Tăng Sát thương Băng `[Cộng thẳng]` |
| 31801 | Tăng Sát thương Điện `[Cơ bản]` |
| 31803 | Tăng Sát thương Điện `[Cộng thẳng]` |
| 31901 | Tăng Sát thương Ether `[Cơ bản]` |
| 31903 | Tăng Sát thương Ether `[Cộng thẳng]` |

### Độ hiếm

| Loại | Mô tả |
| :--- | :---------- |
| 4 | S |
| 3 | A |
| 2 | B |

### Badge Type (Loại Huy hiệu)

| Loại | Mô tả |
| :--- | :---------- |
| 1 | Bảo Vệ Trụ Shiyu |
| 2 | Tòa Tháp Bất Tận |
| 3 | Tập Kích Nguy Cấp |
| 4 | Tòa Tháp Bất Tận: Đường Cùng |

### Kỹ năng

| Chỉ mục | Mô tả|
| :--- | :--------- |
| 0 | Tấn công Thường |
| 1 | Chiến kỹ |
| 2 | Né tránh  |
| 3 | Tuyệt kỹ |
| 5 | Kỹ năng Cốt lõi |
| 6 | Hỗ trợ |

---

## Công thức

#### Chỉ số Người đại diện

Để tính toán các chỉ số cơ bản của một Người đại diện, bạn cần sử dụng
[store/zzz/avatars.json](https://github.com/EnkaNetwork/API-docs/blob/master/store/zzz/avatars.json).

- **Tổng Cơ bản:**
  `BaseTotalValue = BaseProps[PropertyId] + GrowthValue + PromotionValue + CoreEnhancementValue`
- **Tăng trưởng:**
  `GrowthValue = (GrowthProps[PropertyId] * (Avatar.Level - 1)) / 10000`
- **Đột phá:**
  `PromotionValue = PromotionProps[Avatar.PromotionLevel - 1][PropertyId]`
- **Nâng cấp Cốt lõi:**
  `CoreEnhancementValue = CoreEnhancementProps[Avatar.CoreSkillEnhancement][PropertyId]`

**LƯU Ý:** Nên làm tròn xuống (floor) kết quả trước khi cộng tổng chúng với các chỉ số từ các nguồn khác, bao gồm cả những chỉ số từ các nguồn khác.

### Chính xác theo Game

#### W-Engine

Để làm việc với chỉ số W-Engine, bạn cần sử dụng:
  \- [WeaponLevelTemplateTb.json](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/FileCfg/WeaponLevelTemplateTb.json)
  \- [WeaponStarTemplateTb.json](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/FileCfg/WeaponStarTemplateTb.json)


- **Dòng Chính:**
  `Result = MainStat.PropertyValue * (1 + WeaponLevel.FIELD_XXX / 10000 + WeaponStar.FIELD_YYY / 10000)`
  **Ví dụ (Cấp 60, BreakLevel 5):**
  `684 = 46 * (1 + 94090 / 10000 + 44610 / 10000)`

- **Dòng Phụ:**
  `Result = SecondaryStat.PropertyValue * (1 + WeaponStar.FIELD_ZZZ / 10000)`
  **Ví dụ (BreakLevel 5):**
  `2400 = 960 * (1 + 15000 / 10000)`

**LƯU Ý:** W-Engine **Đệm Thịt Thép** `[14102]` đã được sử dụng trong ví dụ.

#### Đĩa

Để làm việc với chỉ số Đĩa, bạn cần sử dụng [EquipmentLevelTemplateTb.json](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/FileCfg/EquipmentLevelTemplateTb.json)
File này xác định giá trị Đĩa dựa trên cấp độ và độ hiếm của nó.

- **Dòng Chính:**
  `Result = MainStat.PropertyValue * (1 + EquipmentLevel.Field_XXX / 10000)`
  **Ví dụ (Cấp 14, Độ hiếm 4):**
  `2090 = 550 * (1 + 28000 / 10000)`

### Xấp xỉ

#### W-Engine

- **Dòng Chính:**
    `Result = MainStat.PropertyValue * (1 + 0.1568166666666667 * Level + 0.8922 * BreakLevel)`

- **Dòng Phụ:**
    `Result = SecondaryStat.PropertyValue * (1 + 0.3 * BreakLevel)`

#### Đĩa

- **Dòng Chính:**
`Result = MainStat.PropertyValue + (MainStat.PropertyValue * Level * RarityScale)`
- **Thang đo Độ hiếm**
| Độ hiếm | Thang đo |
| :----- | :------ |
| 4 | 0.2 |
| 3 | 0.25 |
| 2 | 0.3 |

---

## Biểu tượng và Hình ảnh

Tất cả tên biểu tượng được bao gồm trong dữ liệu đã phân tích tại [API-docs/store/zzz](https://github.com/EnkaNetwork/API-docs/tree/master/store/zzz).

Để biết thêm thông tin bổ sung, hãy kiểm tra kho lưu trữ [ZenlessData](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/).

## Bản địa hóa

Đối với các tên được sử dụng trong Enka.Network, tham khảo [store/zzz/locs.json](https://raw.githubusercontent.com/EnkaNetwork/API-docs/refs/heads/master/store/zzz/locs.json)

Để biết thêm thông tin bổ sung về tên, mô tả, v.v., hãy kiểm tra [Dữ liệu TextMap](https://git.mero.moe/dimbreath/ZenlessData/src/branch/master/TextMap), chỉ bao gồm các ngôn ngữ được hỗ trợ bởi game.

---

## Wrappers

Python - https://github.com/seriaati/enka-py - [seriaati](https://github.com/seriaati)

C# - https://github.com/aliafuji/EnkaDotnet - [aliafuji](https://github.com/aliafuji)

Go - https://github.com/kirinyoku/enkanetwork-go - [kirinyoku](https://github.com/kirinyoku)
