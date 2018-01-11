---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://data.rifiniti.com'>Data environment</a>

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
  '{ "from_time": 1438078400, "to_time": 1439078400, "ids": [206, 516], "elasticsearch_types": ["AttendanceIndex::AttendanceBadge", "AttendanceIndex::AttendanceWifi"] }' \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -u 'ibardarov@rifiniti.com:11223344' \
  'https://internal-api.rifiniti.com/manage/api/v1/index_requests/buildings'

```
> The above command POST data

```json
{
  "from_time": 1438078400,
  "to_time": 1439078400,
  "ids": [
    206,
    516
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

- `POST https://internal-api.rifiniti.com/manage/api/v1/index_requests/buildings` - for index requests
- `DELETE https://internal-api.rifiniti.com/manage/api/v1/index_requests/buildings` - for delete requests
- `GET https://internal-api.rifiniti.com/manage/api/v1/index_requests/buildings/<request_id>` - for getting the status/progress of request

### POST/DELETE Query Parameters

Parameter           | Type           | Example    | Description
------------------- | -------------- | ---------- | -----------
from_time           | unix timestamp | 1439078400 | from when the indexing starts (inclusive)
to_time             | unix timestamp | 1439078401 | the end time for the indexing period (inclusive)
ids                 | integer array  | [717, 718] | The ids which we want to index/delete (depending on the https verb). They must be valid building ids.
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

## Index floors

```shell
curl -i -XPOST -d \
  '{ "from_time": 1438078400, "to_time": 1439078400, "ids": [482, 483], "elasticsearch_types": ["AttendanceIndex::AttendanceBadge", "AttendanceIndex::AttendanceWifi"] }' \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -u 'ibardarov@rifiniti.com:11223344' \
  'https://internal-api.rifiniti.com/manage/api/v1/index_requests/floors'

```
> The above command POST data

```json
{
  "from_time": 1438078400,
  "to_time": 1439078400,
  "ids": [
    482,
    483
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

- `POST https://internal-api.rifiniti.com/manage/api/v1/index_requests/floors` - for index requests
- `DELETE https://internal-api.rifiniti.com/manage/api/v1/index_requests/floors` - for delete requests
- `GET https://internal-api.rifiniti.com/manage/api/v1/index_requests/floors/<request_id>` - for getting the status/progress of request

### POST/DELETE Query Parameters

Parameter           | Type           | Example    | Description
------------------- | -------------- | ---------- | -----------
from_time           | unix timestamp | 1439078400 | from when the indexing starts (inclusive)
to_time             | unix timestamp | 1439078401 | the end time for the indexing period (inclusive)
ids                 | integer array  | [482, 483] | The ids which we want to index/delete (depending on the https verb). They must be valid floor ids.
elasticsearch_types | string array   | ["AttendanceIndex::AttendanceBadge", "AttendanceIndex::AttendanceWifi"] | The list of elasticsearch types we want to index/delete (see list of the valid types below).

In one request you can't mix ids from different clients!

### List of valid elasticsearch types
- AttendanceIndex::AttendanceBadge
- AttendanceIndex::AttendanceWifi
- OptimoIndex::BadgeOccupancyByDepartment
- OptimoIndex::BadgeOccupancyByFloor
- OptimoIndex::HourlyArrivalsBadge
- OptimoIndex::HourlyArrivalsBadgeByDepartment
- WifiIndex::Occupancy

## Index departments

```shell
curl -i -XPOST -d \
  '{ "from_time": 1438078400, "to_time": 1439078400, "ids": [1686, 17038], "elasticsearch_types": ["DepartmentIndex::MobilityBadgeDepartment", "OptimoIndex::BadgeOccupancyByDepartment"] }' \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -u 'ibardarov@rifiniti.com:11223344' \
  'https://internal-api.rifiniti.com/manage/api/v1/index_requests/departments'

```
> The above command POST data

```json
{
  "from_time": 1438078400,
  "to_time": 1439078400,
  "ids": [
    482,
    483
  ],
  "elasticsearch_types": [
    "DepartmentIndex::MobilityBadgeDepartment",
    "OptimoIndex::BadgeOccupancyByDepartment"
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

- `POST https://internal-api.rifiniti.com/manage/api/v1/index_requests/departments` - for index requests
- `DELETE https://internal-api.rifiniti.com/manage/api/v1/index_requests/departments` - for delete requests
- `GET https://internal-api.rifiniti.com/manage/api/v1/index_requests/departments/<request_id>` - for getting the status/progress of request

### POST/DELETE Query Parameters

Parameter           | Type           | Example    | Description
------------------- | -------------- | ---------- | -----------
from_time           | unix timestamp | 1439078400 | from when the indexing starts (inclusive)
to_time             | unix timestamp | 1439078401 | the end time for the indexing period (inclusive)
ids                 | integer array  | [1686, 17038] | The ids which we want to index/delete (depending on the https verb). They must be valid department ids.
elasticsearch_types | string array   | ["DepartmentIndex::MobilityBadgeDepartment", "OptimoIndex::BadgeOccupancyByDepartment"] | The list of elasticsearch types we want to index/delete (see list of the valid types below).

In one request you can't mix ids from different clients!

### List of valid elasticsearch types
- DepartmentIndex::MobilityBadgeDepartment
- DepartmentIndex::MobilityWifiDepartment
- OptimoIndex::BadgeOccupancyByDepartment
- OptimoIndex::HourlyArrivalsBadgeByDepartment
- OptimoIndex::PresenceOccupancyByDepartment

## Index bookings

```shell
curl -i -XPOST -d \
  '{ "ids": [63], "elasticsearch_types": ["BookingIndex::Booking"] }' \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -u 'ibardarov@rifiniti.com:11223344' \
  'https://internal-api.rifiniti.com/manage/api/v1/index_requests/bookings'

```
> The above command POST data

```json
{
  "ids": [
    63
  ],
  "elasticsearch_types": [
    "BookingIndex::Booking"
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

- `POST https://internal-api.rifiniti.com/manage/api/v1/index_requests/bookings` - for index requests
- `DELETE https://internal-api.rifiniti.com/manage/api/v1/index_requests/bookings` - for delete requests
- `GET https://internal-api.rifiniti.com/manage/api/v1/index_requests/bookings/<request_id>` - for getting the status/progress of request

### POST/DELETE Query Parameters

Parameter           | Type           | Example    | Description
------------------- | -------------- | ---------- | -----------
ids                 | integer array  | [63] | The ids which we want to index/delete (depending on the https verb). They must be valid client id.
elasticsearch_types | string array   | ["BookingIndex::Booking"] | The list of elasticsearch types we want to index/delete (see list of the valid types below).

In one request you can't mix ids from different clients!

### List of valid elasticsearch types
- BookingIndex::Booking

## Index meetings

```shell
curl -i -XPOST -d \
  '{ "ids": [63], "elasticsearch_types": ["MeetingIndex::Meeting"] }' \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -u 'ibardarov@rifiniti.com:11223344' \
  'https://internal-api.rifiniti.com/manage/api/v1/index_requests/meetings'

```
> The above command POST data

```json
{
  "ids": [
    63
  ],
  "elasticsearch_types": [
    "MeetingIndex::Meeting",
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

- `POST https://internal-api.rifiniti.com/manage/api/v1/index_requests/meetings` - for index requests
- `DELETE https://internal-api.rifiniti.com/manage/api/v1/index_requests/meetings` - for delete requests
- `GET https://internal-api.rifiniti.com/manage/api/v1/index_requests/meetings/<request_id>` - for getting the status/progress of request

### POST/DELETE Query Parameters

Parameter           | Type           | Example    | Description
------------------- | -------------- | ---------- | -----------
ids                 | integer array  | [63] | The ids which we want to index/delete (depending on the https verb). They must be valid client id.
elasticsearch_types | string array   | ["DepartmentIndex::MobilityBadgeDepartment", "OptimoIndex::BadgeOccupancyByDepartment"] | The list of elasticsearch types we want to index/delete (see list of the valid types below).

In one request you can't mix ids from different clients!

### List of valid elasticsearch types
- MeetingIndex::Meeting
