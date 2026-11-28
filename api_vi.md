# Enka.Network - API

## Tài liệu API Game
- [Genshin Impact](docs/gi/api_vi.md)
- [Zenless Zone Zero](docs/zzz/api_vi.md)

## Bắt đầu

Bạn có thể sử dụng các wrapper được tạo bởi người khác, hoặc sử dụng API trực tiếp. Nó rất đơn giản, vì vậy không khó để tự xây dựng logic dữ liệu của riêng bạn dựa trên các phản hồi. Thách thức lớn nhất là điều hướng dữ liệu game được khai thác (datamined) và gắn nó với dữ liệu trả về một cách chính xác.

Xem phần [Wrappers](#wrappers) cho ngôn ngữ lập trình bạn chọn.

## Trước khi bạn yêu cầu

Một vài quy tắc khi sử dụng API:

1. Vui lòng không cố gắng liệt kê UID hoặc thực hiện các tác vụ truy vấn lớn để lấp đầy cơ sở dữ liệu. Có hàng trăm triệu UID và bạn đơn giản là sẽ không thể làm điều này thông qua API này. Tôi có thể cung cấp dữ liệu hàng loạt vào một thời điểm sau này.

2. Vui lòng đặt header `User-Agent` tùy chỉnh với các yêu cầu của bạn để tôi có thể theo dõi chúng tốt hơn và giúp bạn nếu cần.

3. Có các giới hạn tốc độ (ratelimit) động trên các endpoint UID - nếu bạn yêu cầu quá nhanh, bạn sẽ gặp thời gian phản hồi chậm hơn và cuối cùng là trả về mã 429. Điều này có nghĩa là bạn cần giảm tốc độ hoặc liên hệ với tôi để xem liệu có thể tăng giới hạn tốc độ cho bạn hay không. Trong hầu hết các trường hợp, điều này là không cần thiết và là kết quả của mã chưa được tối ưu hóa.

4. Về lưu ý này, tất cả các yêu cầu UID đều trả về trường `ttl` - trường này là "số giây cho đến khi yêu cầu Showcase tiếp theo được thực hiện cho UID này". Cho đến khi nó hết hạn, endpoint sẽ trả về dữ liệu được cache, nhưng bạn vẫn sẽ tiêu tốn giới hạn tốc độ của mình nếu bạn liên tục truy cập nó. Hãy cố gắng cache dữ liệu với thời gian chờ là `ttl` khi yêu cầu, hoặc ngăn chặn các yêu cầu đến UID cho đến khi `ttl` của nó hết hạn. Tôi khuyên dùng Redis cho việc này.

Nếu bạn gặp khó khăn khi làm việc với dữ liệu, hãy tham gia [Discord server](https://discord.gg/PcSZr5sbn3) để được trợ giúp.

## Danh sách các API

### Endpoint UID

#### Lấy dữ liệu Showcase với đầy đủ thông tin người chơi

> https://enka.network/api/uid/618285856/

Phản hồi sẽ chứa `playerInfo` và `avatarInfoList`. `playerInfo` là dữ liệu cơ bản về tài khoản game. Nếu thiếu `avatarInfoList`, điều đó có nghĩa là Showcase của tài khoản này đã bị đóng hoặc chưa được thêm nhân vật.


#### Chỉ lấy thông tin người chơi

> https://enka.network/api/uid/618285856/?info

Bằng cách thêm `?info` vào yêu cầu, bạn chỉ yêu cầu `playerInfo`. Yêu cầu chính luôn cố gắng lấy cả `avatarInfoList`; nếu bạn chỉ cần `playerInfo`, vui lòng sử dụng endpoint này - nó sẽ nhanh hơn nhiều so với việc lấy đầy đủ dữ liệu.

Ngoài ra, cả hai phản hồi sẽ chứa đối tượng `owner` nếu:

1. Người dùng có tài khoản trên trang web
2. Người dùng đã thêm UID vào hồ sơ của họ
3. Người dùng đã xác minh nó
4. Người dùng đặt chế độ hiển thị là "công khai"

Thông tin thêm về tài khoản người dùng bên dưới.

#### Mã phản hồi HTTP

Hãy đảm bảo xử lý những mã này trong ứng dụng của bạn một cách thích hợp.
```
400 = Sai định dạng UID
404 = Người chơi không tồn tại (Server MHY báo vậy)
424 = Bảo trì game / mọi thứ bị hỏng sau khi cập nhật game
429 = Bị giới hạn tốc độ (bởi server của tôi hoặc server MHY)
500 = Lỗi server chung
503 = Tôi đã làm hỏng nghiêm trọng
```

### Endpoint Hồ sơ (Profile)

Có thể tạo một tài khoản (hồ sơ) trên trang web và gắn nhiều tài khoản game vào đó. Người dùng sau đó được yêu cầu xác minh rằng tài khoản thuộc về họ thông qua mã xác minh được đặt trong chữ ký (signature) - bằng cách này chúng tôi có thể đảm bảo tài khoản thuộc về họ.

Người dùng có thể "snapshot" (lưu nhanh) các build dưới tên tùy chỉnh, được gọi là "saved builds" (build đã lưu).

> https://enka.network/api/profile/Algoinde/

Lấy thông tin người dùng.

> https://enka.network/api/profile/Algoinde/hoyos/

Lấy danh sách các "hoyos" - tài khoản Genshin và metadata của chúng. Cái này chỉ trả về các tài khoản `verified` (đã xác minh) và `public` (công khai) (người dùng có thể ẩn tài khoản; tài khoản chưa xác minh được ẩn theo mặc định). Mỗi khóa trong phản hồi là một định danh duy nhất của một hoyo mà bạn cần sử dụng cho các yêu cầu tiếp theo để thực sự lấy thông tin nhân vật/build.

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/

Trả về metadata cho một hoyo đơn lẻ.

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/builds/

Trả về các build đã lưu cho một hoyo nhất định. Đây là một đối tượng chứa các mảng, trong đó khóa là `avatarId` của nhân vật, và các đối tượng trong mảng là các build khác nhau cho một nhân vật nhất định, không theo thứ tự cụ thể (nhưng bạn có thể dựa vào trường `order` để sắp xếp khi hiển thị).

Nếu một build có trường `live: true`, điều đó có nghĩa là nó không phải là một build "đã lưu", mà đơn giản là những gì được lấy từ showcase của họ khi họ nhấp vào "làm mới". Khi được làm mới, tất cả các build `live` cũ sẽ biến mất và các build mới được tạo ra. Chỉ người dùng mới quyết định khi nào thực hiện việc làm mới này - dữ liệu này sẽ KHÔNG được cập nhật tự động.

Như đã nêu trong [UID endpoints](#uid-endpoints), khi bạn thực hiện yêu cầu UID, bạn có thể nhận được một đối tượng `owner`. Bạn có thể xây dựng URL bằng cách sử dụng các trường này trong đối tượng:

`https://enka.network/api/profile/{owner.username}/hoyos/{owner.hash}/builds/`


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
