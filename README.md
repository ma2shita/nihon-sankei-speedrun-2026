# nihon-sankei-speedrun-2026

日本三景を1日で巡る弾丸ツアーで取得した、GPSおよびセンサーデータです。実施日は 2026/9/11 です。

GPS and sensor data collected during a one-day speedrun across Japan's Three Scenic Views at Sep. 11, 2026.

## 日本語

### 概要

日本三景である以下の3か所を1日で巡った際の軌跡を記録しています。

- 松島
- 天橋立
- 宮島

データはSORACOMを利用して収集し、SORACOM Harvest Dataから取得しました。

### データ

ファイル:

`nihon-sankei-speedrun-soracom-harvest-data.json`

- レコード数: 616件
- 記録日: 2026年9月11日
- 並び順: 古いデータから新しいデータ
- 時刻形式: UNIX時間（ミリ秒）
- IMSI、`resourceId`、`resourceType`は公開データから削除済みです

### データ形式

```json
{
  "content": "{\"lat\":38.370669,\"lon\":141.069288,\"bat\":3,\"rs\":0,\"temp\":21.1,\"humi\":88.4,\"x\":0,\"y\":64,\"z\":-960,\"type\":0,\"binaryParserEnabled\":true}",
  "contentType": "application/json",
  "time": 1789071389169
}
```

`content`はJSON文字列です。利用時には、さらにJSONとして解析してください。

主なフィールド:

| フィールド | 内容 |
|---|---|
| `time` | 計測時刻（UNIX時間、ミリ秒） |
| `contentType` | データのContent-Type |
| `content.lat` | 緯度。取得できなかった場合は`null` |
| `content.lon` | 経度。取得できなかった場合は`null` |
| `content.temp` | 温度 |
| `content.humi` | 湿度 |
| `content.x` | 加速度センサーのX軸値 |
| `content.y` | 加速度センサーのY軸値 |
| `content.z` | 加速度センサーのZ軸値 |
| `content.bat` | デバイス固有のバッテリー値 |
| `content.rs` | デバイス固有の状態値 |
| `content.type` | デバイス固有のデータ種別 |
| `content.binaryParserEnabled` | SORACOM Binary Parserによる変換の有無 |

### `jq`でcontentを展開する

```sh
jq 'map(. + {content: (.content | fromjson)})' \
  nihon-sankei-speedrun-soracom-harvest-data.json
```

CSVへ変換する例:

```sh
jq -r '
  ["time","lat","lon","temp","humi","x","y","z"],
  (.[] |
    (.content | fromjson) as $c |
    [.time, $c.lat, $c.lon, $c.temp, $c.humi, $c.x, $c.y, $c.z]
  ) |
  @csv
' nihon-sankei-speedrun-soracom-harvest-data.json
```

### 注意事項

- GPS座標やセンサー値は実際の移動中に取得した値です。
- GPSを取得できなかったレコードでは、`lat`と`lon`が`null`です。
- 測定値の正確性、完全性、特定用途への適合性は保証しません。
- 公開用データにはSIMを識別するIMSIを含めていません。

## English

### Overview

This repository contains GPS and sensor data collected while visiting all three of Japan's officially recognized Three Scenic Views in a single day:

- Matsushima
- Amanohashidate
- Miyajima

The data was collected using SORACOM and retrieved from SORACOM Harvest Data.

### Dataset

File:

`nihon-sankei-speedrun-soracom-harvest-data.json`

- Records: 616
- Recording date: September 11, 2026
- Order: Oldest to newest
- Timestamp format: Unix time in milliseconds
- The IMSI, `resourceId`, and `resourceType` have been removed from the public dataset

### Data Format

```json
{
  "content": "{\"lat\":38.370669,\"lon\":141.069288,\"bat\":3,\"rs\":0,\"temp\":21.1,\"humi\":88.4,\"x\":0,\"y\":64,\"z\":-960,\"type\":0,\"binaryParserEnabled\":true}",
  "contentType": "application/json",
  "time": 1789071389169
}
```

The `content` property is a JSON-encoded string and must be parsed separately.

Main fields:

| Field | Description |
|---|---|
| `time` | Measurement time as Unix time in milliseconds |
| `contentType` | Content-Type of the data |
| `content.lat` | Latitude, or `null` when unavailable |
| `content.lon` | Longitude, or `null` when unavailable |
| `content.temp` | Temperature |
| `content.humi` | Humidity |
| `content.x` | X-axis accelerometer value |
| `content.y` | Y-axis accelerometer value |
| `content.z` | Z-axis accelerometer value |
| `content.bat` | Device-specific battery value |
| `content.rs` | Device-specific status value |
| `content.type` | Device-specific data type |
| `content.binaryParserEnabled` | Whether SORACOM Binary Parser was applied |

### Parse `content` with `jq`

```sh
jq 'map(. + {content: (.content | fromjson)})' \
  nihon-sankei-speedrun-soracom-harvest-data.json
```

Example CSV conversion:

```sh
jq -r '
  ["time","lat","lon","temp","humi","x","y","z"],
  (.[] |
    (.content | fromjson) as $c |
    [.time, $c.lat, $c.lon, $c.temp, $c.humi, $c.x, $c.y, $c.z]
  ) |
  @csv
' nihon-sankei-speedrun-soracom-harvest-data.json
```

### Notes

- The GPS coordinates and sensor readings were collected during the actual journey.
- Records without a GPS fix contain `null` for `lat` and `lon`.
- The accuracy, completeness, and fitness of the data for any particular purpose are not guaranteed.
- The public dataset does not contain the IMSI or other SIM identifiers.

## License

This dataset is licensed under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

When using or redistributing the data, please provide appropriate attribution, link to this repository and the license, and indicate whether changes were made.

### Attribution

```text
Source: nihon-sankei-speedrun-2026 by Kohei "Max" MATSUSHITA
License: CC BY 4.0
```
