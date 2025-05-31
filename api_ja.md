この日本語ドキュメントは翻訳されたものであり、不正確な場合があります。必要に応じて[英語版の原文](/api.md)を参照してください

# Enka.Network - API

## 各ゲームのAPIドキュメント

- [原神](docs/gi/api_ja.md)
- [ゼンレスゾーンゼロ](docs/zzz/api_ja.md)


## はじめに

他の人が作ったラッパーを利用してもいいし、APIを直接使っても良いと思います。
非常にシンプルなので、レスポンスをもとに独自のデータロジックを作成するのは全く難しくありません。

最大の課題は、データマイニングされたゲームデータをナビゲートし、適切な方法で返されたデータを使用することです。

様々な言語のラッパーについては、[Wrappers](#wrappers)を参照してください。

## 利用する前に
APIを使用する際のいくつかのルールです。

1. UIDを列挙したり、データベースを埋めるために大量のリクエストを実行しようとしないでください。
UIDは何億もあり、このAPIでこれを実行することはできません。後日、バッチデータを提供することがあります。

2. リクエストにはカスタムした`User-Agent` ヘッダを設定してください。
そうすることで、リクエストの追跡が簡単になり、必要に応じてあなたを助けることもできます。

3. UIDのエンドポイントには動的な速度制限があります。
あまりに速く再リクエストすると、応答時間が遅くなり、最終的にはステータスコード429が返されます。
この場合、速度を落とすか、私に連絡してレートリミットを増やすことが可能かどうかを確認する必要があります。
殆どの場合、これは必要ではなく、最適化されていないコードが原因です。

4. 全てのUIDリクエストには`ttl`というフィールドを返します。
このフィールドは「リクエストされたUIDに対して次のデータ更新が行われるまでの秒数」です。
値が0になるまでエンドポイントはキャッシュされたデータを返しますが、その間であってもリクエストするとレートリミットを消費してしまいます。
リクエスト時に`ttl`のタイムアウトを設けてデータをキャッシュするか、`ttl`が切れるまでそのUIDへのリクエストを行わないようにすることを試してみてください。
この処理にはRedisなどを用いることをお勧めします。

もしデータの扱いに困ったら、[Discordサーバー](https://discord.gg/PcSZr5sbn3)でヘルプを受けられます。

## API一覧
### UIDエンドポイント
#### キャラ情報を含む全ての情報を取得

> https://enka.network/api/uid/618285856/

レスポンスには `playerInfo` と `avatarInfoList` が含まれます。
`playerInfo`はゲームアカウントに関する基本的なデータです。
もし `avatarInfoList` が見つからない場合は、当該UIDのアカウントはプロフィールが非公開にされているか、キャラクターが設定されていないことを意味します。

#### プレイヤー情報のみを取得

> https://enka.network/api/uid/618285856/?info

リクエストに `?info` を付けることで、`playerInfo` のみをリクエストすることができます。
もし `playerInfo` だけが必要であれば、このエンドポイントを使用してください。
全てのデータを取得するよりもずっと速く取得する事が出来ます。

さらに、以下の条件を満たす場合、両方のレスポンスに `owner` オブジェクトが含まれます。

1. ユーザがこのサイトにアカウントを持っている。
2. ユーザーが自分のUIDをプロファイルに追加した。
3. ユーザーが認証を行った。
4. ユーザーがその公開状態を"公開"に設定した。

`owner` オブジェクトに関する詳細は以下を参照ください。

#### HTTPレスポンスコード

アプリ内でこれらが適切に処理されるようにしてください。

```
400 = UIDの形式不正 (入力された値が範囲外であるなど)
404 = そのUIDを持つプレイヤーが存在しない (これはmiHoYoサーバーからのレスポンスです)
424 = ゲームメンテナンス (更新等) / ゲームアップデート後にシステムが大幅に変更されて更新が必要
429 = レートリミット (Enka.NetWorkかmiHoYoサーバーのどちらか)
500 = Enka.NetWorkサーバーのエラー
503 = Enka.NetWorkサーバーの一時停止中
```

### プロファイルエンドポイント

ウェブサイト上でアカウント（プロファイル）を作成し、そのアカウントに複数のゲームアカウントを設定することが可能です。
ユーザーは、認証ページに記載された認証コードによって、そのアカウントが自分のものであることを証明する必要があります。

ユーザーは、好きな名称でビルドを「スナップショット」することができ、「保存されたビルド」と呼ばれています。

> https://enka.network/api/profile/Algoinde/

ユーザー情報を取得します。

> https://enka.network/api/profile/Algoinde/hoyos/

「hoyos」-　ゲームアカウントとそのメタデータのリストを取得します。
これは `認証状態` 且つ `公開状態` であるアカウントのみを返します(ユーザーはアカウントを隠すことができます。認証されていないアカウントはデフォルトで隠されます)。
レスポンスの各キーはmihoyoの一意な識別子であり、この識別子を使って後続のリクエストで実際にキャラクター/ビルドの情報を取得する必要があります。

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/

1つのゲームアカウントのメタデータを返します。

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/builds/

指定されたゲームアカウントの保存されたビルドを返します。これはオブジェクトの配列で、キーはキャラクターの `avatarId` です。
オブジェクトの配列は与えられたキャラクターの異なるビルドを順不同で表します（ただし、表示の為に順序が記載された `order` フィールドを持ちます）。

ビルドに `live:　true` フィールドがある場合、それは「保存」されたビルドではなく、単に「更新」をクリックした時に取得されたものであることを意味します。
更新すると、古い`live`ビルドはすべて削除され、新しいビルドが作成されます。この更新をいつ行うかは、ユーザーだけが決めることができます。

[UIDエンドポイント](#uidエンドポイント)で説明したように、UIDリクエストを行うと、`owner`オブジェクトを取得することができます。
このオブジェクトのフィールドを使用して、URLを作成することができます。

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