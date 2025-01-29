# Enka.Network - API

## Game API Docs
- [Genshin Impact](docs/gi/api.md)
- [Zenless Zone Zero](docs/zzz/api.md)

## Getting Started

You can utilize some wrappers made by other people, or use the API directly. It's very simple, so it is not hard at all to hand-roll your own data logic based on the responses. The biggest challenge is navigating the datamined game data and attaching it to the data returned in the right way.

See [Wrappers](#wrappers) for the language of your choice.

## Before You Request

A couple of rules on using the API:

1. Please don't try to enumerate UIDs or try to do massive query jobs in an effort to fill a database. There are hundreds of millions of UIDs and you simply won't be able to do this through this API. I may provide batch data at a later point.

2. Please set a custom `User-Agent` header with your requests so I can track them better and help you if needed.

3. There are dynamic ratelimits on UID endpoints - if you're requesting too fast, you'll run into slower response times and eventually a 429 return code. This mean you either need to slow down or contact me to see if it's possible to increase the ratelimits for you. In most cases it's not needed and is a result of unoptimized code.

4. On this note, all UID requests return a `ttl` field - this field is "seconds until the next Showcase request will be made for this UID". Until it runs out, the endpoint will return cached data, but you will still consume your ratelimit if you repeatedly hit it. Try to either cache the data with a timeout of `ttl` upon request, or prevent requests to the UID until its `ttl` runs out. I recommend Redis for this.

If you have difficulties working with the data, hop on the [Discord server](https://discord.gg/PcSZr5sbn3) for help.

## List of APIs

### UID endpoints

#### Get Showcase data with full player info

> https://enka.network/api/uid/618285856/

The response will contain `playerInfo` and `avatarInfoList`. `playerInfo` is the basic data about the game account. If `avatarInfoList` is missing, that means the Showcase of this account is either closed or not populated with characters.


#### Get only player info

> https://enka.network/api/uid/618285856/?info

By attaching `?info` to the request, you are requesting only `playerInfo`. The main request always tries to get `avatarInfoList` as well; if you only need `playerInfo`, please use this endpoint - it will be much faster than getting the full data.

In addition, both responses will contain an `owner` object if:

1. The user has an account on the site
2. The user added their UID to the profile
3. The user verified it
4. The user set its visibility to "public"

More info on user accounts below.

#### HTTP response codes

Please make sure to handle these in your app appropriately.
```
400 = Wrong UID format
404 = Player does not exist (MHY server said that)
424 = Game maintenance / everything is broken after the game update
429 = Rate-limited (either by my server or by MHY server)
500 = General server error
503 = I screwed up massively
```

### Profile endpoints

It is possible to create an account (profile) on the website and attach several game accounts to it. The users are then required to verify that the account belongs to them via a verification code placed in their signature - this way we can ensure the account belongs to them.

Users can "snapshot" builds under custom names, referred as "saved builds".

> https://enka.network/api/profile/Algoinde/

Gets the user info.

> https://enka.network/api/profile/Algoinde/hoyos/

Gets a list of "hoyos" - Genshin accounts and their metadata. This only returns accounts that are `verified` and `public` (users can hide accounts; unverified accounts are hidden by default). Each key in the response is a unique identifier of a hoyo which you need to use for the subsequent requests to actually get the characters/builds information.

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/

Returns metadata for a single hoyo.

> https://enka.network/api/profile/Algoinde/hoyos/4Wjv2e/builds/

Returns saved builds for a given hoyo. This is an object of arrays, where the key is `avatarId` of the character, and objects in arrays are different builds for a given character, in no particular order (but they do have an `order` field you need to order by for display).

If a build has a `live: true` field, that means it's not a "saved" build, but simply what was fetched from their showcase when they clicked "refresh". When refreshed, all old `live` builds are gone and new ones are created. Only the user decides when to perform this refresh - this data will NOT be up-to-date.

As outlined in [UID endpoints](#uid-endpoints), when you make a UID request, you could get an `owner` object. You can construct the URL using these fields in the object:

`https://enka.network/api/profile/{owner.username}/hoyos/{owner.hash}/builds/`


## Wrappers

TS/JS - https://www.npmjs.com/package/enkanetwork.js - [Jelosus1](https://github.com/Jelosus2)

TS/JS - https://github.com/yuko1101/enka-network-api - [yuko1101](https://github.com/yuko1101)

Rust - https://github.com/eratou/enkanetwork-rs - [eratou](https://github.com/eratou)

Python - https://github.com/mrwan200/enkanetwork.py - [mrwan200](https://github.com/mrwan200)

Python - https://github.com/seriaati/enka-py - [seriaati](https://github.com/seriaati)

Java - https://github.com/kazuryyx/EnkaNetworkAPI - [kazury](https://github.com/kazuryyx)