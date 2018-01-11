---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='http://data.rifiniti.com'>Data environment</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Rifiniti API!

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -u ibardarov@rifiniti.com:11223344 \

  You must replace <code>11223344</code> with your personal API key.
```

For authentication you have to use your own manage username/password.



# Indexing API

## Index buildings

```shell
curl -i -XPOST -d \
  '{ "from_time": 0, "to_time": 1439078400, "ids": [206, 516, 569, 581, 582, 583, 695], "elasticsearch_types": ["AttendanceIndex::AttendanceBadge"] }' \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -u 'ibardarov@rifiniti.com:11223344' \
  'http://localhost:3000/manage/api/v1/index_requests/buildings'

```
> The above command POST data

```json
{
  "from_time": 0,
  "to_time": 1439078400,
  "ids": [
    206,
    516,
    569,
    581,
    582,
    583,
    695
  ],
  "elasticsearch_types": [
    "AttendanceIndex::AttendanceBadge"
  ]
}
```

> The JSON response

```json
{"data":{"request_id":"8929f95c-b4bd-47e1-9759-3a30ab80d2db"}}
```

This endpoint creates index or removal requests.
Also it supports buildings, floors, departments. The only caveat is that you can't mix ids from different clients and types.

### HTTP Request

`POST http://example.com/manage/api/v1/index_requests/building`

### Query Parameters

Parameter | Type | Example | Description
--------- | ---- | ------- | -----------
from_time | unix timestamp | 1439078400 | from when the indexing starts - inclusive
to_time | unix timestamp | 1439078401 | the end time for the indexing period - inclusive
ids | comma separated list of ids | 717,718 | The ids which we want to index. They could be building/floor/departments. check the id_type
id_type | 1, 2 or 3 | 1 |1 - building ids, 2 - floor ids, 3 - department ids
from_date | date | 1 |1 - building ids, 2 - floor ids, 3 - department ids

In one request you can't mix ids from different clients!


- "AttendanceIndex::AttendanceBadge"
- "AttendanceIndex::AttendanceWifi"
- "DepartmentIndex::MobilityBadgeDepartment"
- "DepartmentIndex::MobilityWifiDepartment"
- "OptimoIndex::BadgeOccupancyByBuilding"
- "OptimoIndex::BadgeOccupancyByDepartment"
- "OptimoIndex::BadgeOccupancyByFloor"
- "OptimoIndex::HourlyArrivalsBadge"
- "OptimoIndex::HourlyArrivalsBadgeByDepartment"
- "OptimoIndex::MobilityBadge"
- "OptimoIndex::MobilityWifi"
- "OptimoIndex::PresenceOccupancy"
- "OptimoIndex::PresenceOccupancyByDepartment"
- "WifiIndex::HourlyArrivalsWifi"
- "WifiIndex::Occupancy"

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

### Response

index_request_id - is the id of the worker which will process the index requests. It might fail or it might succeed.

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

