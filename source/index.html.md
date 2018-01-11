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
curl "<api-endpoint>"
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -u "ibardarov@rifiniti.com:11223344" \

```

For authentication you have to use your own manage username/password.

# Indexing API

## Index buildings

```shell
curl -i -XPOST -d \
  '{ "from_time": 1438078400, "to_time": 1439078400, "ids": [206, 516, 569, 581, 582, 583, 695], "elasticsearch_types": ["AttendanceIndex::AttendanceBadge", "AttendanceIndex::AttendanceWifi"] }' \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -u 'ibardarov@rifiniti.com:11223344' \
  'http://localhost:3000/manage/api/v1/index_requests/buildings'

```
> The above command POST data

```json
{
  "from_time": 1438078400,
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
    "AttendanceIndex::AttendanceBadge",
    "AttendanceIndex::AttendanceWifi"
  ]
}
```

> The JSON response

```json
{"data":{"request_id":"8929f95c-b4bd-47e1-9759-3a30ab80d2db"}}
```

This endpoint creates index and delete requests.
The only caveat is that you can't mix ids from different clients and types.

### HTTP Request

`POST http://example.com/manage/api/v1/index_requests/building` - for index requests
`DELETE http://example.com/manage/api/v1/index_requests/building` - for delete requests
`GET http://example.com/manage/api/v1/index_requests/building/<request_id>` - for getting the status/progress of request

### POST/DELETE Query Parameters

Parameter           | Type           | Example    | Description
------------------- | -------------- | ---------- | -----------
from_time           | unix timestamp | 1439078400 | from when the indexing starts (inclusive)
to_time             | unix timestamp | 1439078401 | the end time for the indexing period (inclusive)
ids                 | integer array  | [717, 718] | The ids which we want to index/delete (depending on the http verb). They must be valid building ids.
elasticsearch_types | string array   | ["AttendanceIndex::AttendanceBadge", "AttendanceIndex::AttendanceWifi"] | The list of elasticsearch types we want to index/delete (see list of the valid types below).

In one request you can't mix ids from different clients!

### List of valid elasticsearch types
- AttendanceIndex::AttendanceBadge
- DepartmentIndex::MobilityBadgeDepartment
- OptimoIndex::BadgeOccupancyByDepartment
- OptimoIndex::BadgeOccupancyByBuilding
- OptimoIndex::HourlyArrivalsBadge
- OptimoIndex::MobilityBadge
- OptimoIndex::BadgeOccupancyByFloor
- OptimoIndex::PresenceOccupancy
- OptimoIndex::PresenceOccupancyByDepartment
- OptimoIndex::HourlyArrivalsBadgeByDepartment
- AttendanceIndex::AttendanceWifi
- DepartmentIndex::MobilityWifiDepartment
- OptimoIndex::MobilityWifi
- WifiIndex::HourlyArrivalsWifi
- WifiIndex::Occupancy
