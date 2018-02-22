# Introduction

This documentation is for the Rifiniti client API for data extraction.

The API uses jwt tokens for authorization of the requests and that token must be present in the headers of each request. See [Authentication](#Authentication) for instruction on how to acquire a token.

# Authentication

  > Example authentication request:

  ```shell
  curl -XPOST \
    -H "Content-Type: application/json" \
    -d '{ "email": "name@domain.com", "password": "SuperSecurePass"}' \
    'https://subdomain.rifiniti.com/client_api/authenticate'
  ```

  > If authentication is successfull you will get json with the following structure:

  ```json
  {
    "auth_token": "your token to be used
     in all other request in order to authorize them"
  }
  ```

  To acquire a token you must authenticate with your email and password for Optimo.

  `POST /client_api/authenticate`

  <aside class="notice">
    You must replace `subdomain` in the examples on the side with your company Rifiniti subdomain.
  </aside>

  <aside class="warning">
    The generated token is valid for 24 hours. After that you need to re-authenticate.
  </aside>

  **Headers**

    Type          | Header Name    | Value
    ------------- | -------------- | --------------
    required      | Content-Type   | application/json

  **Parameters**

    Type          | Key           | Example Value
    ------------- | ------------- | -------------
    required      | email         | name@domain.com
    required      | password      | SupperSecurePass

# Buildings
  All the requests need to be authorized with the jwt token returned from the authenticate endpoint. It is also required to specify the version of the API on every request.

  <aside class="notice">
    For now there is only ```v1``` but version is required for future convinience. 
  </aside>

  **Headers**

  Type          | Header Name     | Value
  ------------- | --------------- | --------------
  required      | Authorization   | your jwt token
  required      | Accept-Version  | v1

## Metadata
  Buildings metadata contains the identificator of the building which can be used to query results for that particular building as well as the service level the building have.

### All Buildings

  > Metadata for all buildings:

  ```shell
  curl -XGET \
    -H "Authorization: your jwt token" \
    -H "Accept-Version: v1" \
    'https://subdomain.rifiniti.com/client_api/meta/buildings'
  ```
  > JSON response structure

  ```json
  [
    {
        "building_id": "BLD1",
        "service_level": "advanced"
    },
    {
        "building_id": "BLD2",
        "service_level": "essential"
    }
  ]
  ```

  List all monitored buildings with their identificator and service level.

  `GET /client_api/meta/buildings`

  The call returns an array of JSONs.

### Single Building

  > Metadata for single building:

  ```shell
  curl -XGET \
    -H "Authorization: your jwt token" \
    -H "Accept-Version: v1" \
    'https://subdomain.rifiniti.com/client_api/meta/building/BLD1'
  ```
  > JSON response structure

  ```json
  {
    "building_id": "BLD1",
    "service_level": "advanced"
  }
  ```

  Find out single building service level.

  `GET /client_api/meta/building/{{Building Identifier}}`

  The call returns a single JSON.

## Results

  **Parameters**

  All of the following are ```GET``` parameters.

  *Obligatory*

  Type          | Key           | Value Type    | Example Value
  ------------- | ------------- | ------------- | -------------
  required      | from          | String        | 2018-01-24
  required      | to            | String        | 2018-01-24

  ```from``` is transformed to the beggining of day and
  ```to``` is transformed to the end of day

  So setting ```from``` and ```to``` to the same value will return the results for that day.

  *Optional*

   Type         | Key                        | Value Type | Default Value
  ------------- | -------------------------- | ---------- | -------------
  optional      | only_working_hours         | Boolean    | true
  optional      | only_working_days          | Boolean    | false
  optional      | utilization_over_headcount | Boolean    | false
  optional      | utilization_over_capacity  | Boolean    | true
  optional      | add_meeting_capacity       | Boolean    | false
  optional      | add_support_capacity       | Boolean    | false

  *Note: Some of the params are mutually exclusive

- utilization_over_headcount and utilization_over_capacity are mutually exclusive
- utilization_over_headcount and add_meeting_capacity are mutually exclusive
- utilization_over_headcount and add_support_capacity are mutually exclusive

  By default utilization percentage is calculated with regards to workstation capacity.

  When ```add_meeting_capacity``` is ```true```, the meeting capacity will be taken into account when calculating utilization percentage.

  When ```add_support_capacity``` is ```true```, the support capacity will be taken into account when calculating utilization percentage.

  If ```utilization_over_headcount``` is ```true``` the utilization % will be calculated with regards to the headcount instead of the capacity.

### Single Building

  > Results for single building:

  ```shell
  curl -XGET \
    -H "Authorization: your jwt token" \
    -H "Accept-Version: v1" \
    'https://subdomain.rifiniti.com/client_api/results/building/BLD1?from=2018-01-24&to=2018-01-24'
  ```

  > JSON response structure

  ```json
  {
    "building_id": "BLD1",
    "service_level": "advanced",
    "area": 132435,
    "workstation_space_capacity": 903,
    "meeting_space_capacity": 123,
    "support_space_capacity": 42,
    "headcount": 759,
    "attendance": 123.4,
    "average_utilization": 253.4,
    "peak_utilization": 296,
    "average_utilization_percent": 28.1,
    "peak_utilization_percent": 32.8,
    "deskbound": 77.8,
    "internally_mobile": 36,
    "externally_mobile": 304.3,
    "remote": 14.5,
    "deskbound_percent": 18,
    "internally_mobile_percent": 8.3,
    "externally_mobile_percent": 70.4,
    "remote_percent": 3.4
  }
  ```

  Get results for specific building

  `GET /client_api/results/building/{{Building Identifier}}`

### All Buildings

  > Results for all buildings:

  ```shell
  curl -XGET \
    -H "Authorization: your jwt token" \
    -H "Accept-Version: v1" \
    'https://subdomain.rifiniti.com/client_api/results/buildings?from=2018-01-24&to=2018-01-24'
  ```

  Get results for all buildings

  `GET /client_api/results/building/{{Building Identifier}}`

  Responds with an array of jsons with results for each building.

# Floors

## Metadata
  Floors metadata contains the floor level, the building identificator, the area and the service level of the floor.

### All Floors in a building

  > Metadata for all floors in a building:

  ```shell
  curl -XGET \
    -H "Authorization: your jwt token" \
    -H "Accept-Version: v1" \
    'https://subdomain.rifiniti.com/client_api/meta/building/BLD1/floors'
  ```

  > JSON response structure

  ```json
  [
    {
      "building_id": "BLD1",
      "floor_level": 1,
      "service_level": "advanced",
      "area": 12345
    },
    {
      "building_id": "BLD1",
      "floor_level": 2,
      "service_level": "advanced",
      "area": 54321
    }
  ]
  ```

  List the floors inside of a building with metadata for them.

  `GET /client_api/meta/building/{{Building Identifier}}/floors`

  The call returns an array of JSONs.

### Single Floor

  > Metadata for a floor:

  ```shell
  curl -XGET \
    -H "Authorization: your jwt token" \
    -H "Accept-Version: v1" \
    'https://subdomain.rifiniti.com/client_api/meta/building/BLD1/floors/2'
  ```

  > JSON response structure

  ```json
  {
    "building_id": "BLD1",
    "floor_level": 2,
    "service_level": "advanced",
    "area": 54321
  }
  ```

  Get metadata for floor.

  `GET /client_api/meta/building/{{Building Identifier}}/floors/{{Floor Level}}`

  The call returns a JSON.

## Results

  **Parameters**

  All of the following are ```GET``` parameters.

  *Obligatory*

  Type          | Key           | Value Type    | Example Value
  ------------- | ------------- | ------------- | -------------
  required      | from          | String        | 2018-01-24
  required      | to            | String        | 2018-01-24

  ```from``` is transformed to the beggining of day and
  ```to``` is transformed to the end of day

  So setting ```from``` and ```to``` to the same value will return the results for that day.

  *Optional*

   Type         | Key                        | Value Type | Default Value
  ------------- | -------------------------- | ---------- | -------------
  optional      | only_working_hours         | Boolean    | true
  optional      | only_working_days          | Boolean    | false
  optional      | utilization_over_capacity  | Boolean    | true
  optional      | add_meeting_capacity       | Boolean    | false
  optional      | add_support_capacity       | Boolean    | false

  By default utilization percentage is calculated with regards to workstation capacity.

  When ```add_meeting_capacity``` is ```true```, the meeting capacity will be taken into account when calculating utilization percentage.

  When ```add_support_capacity``` is ```true```, the support capacity will be taken into account when calculating utilization percentage.

### Single floor

  > Results for single floor

  ```shell
  curl -XGET \
    -H "Authorization: your jwt token" \
    -H "Accept-Version: v1" \
    'https://subdomain.rifiniti.com/client_api/results/building/BLD1/floors/2?from=2018-01-24&to=2018-01-24'
  ```

  > JSON response structure

  ```json
  {
    "building": "BLD1",
    "floor_level": 2,
    "service_level": "advanced",
    "area": 54321,
    "workstation_space_capacity": 42,
    "meeting_space_capacity": 10,
    "support_space_capacity": 0,
    "average_utilization": 21,
    "peak_utilization": 31.5,
    "average_utilization_percent": 50,
    "peak_utilization_percent": 75
}
  ```

  Get results for a floor.

  `GET /client_api/results/building/{{Building Identifier}}/floors/{{Floor Level}}`

  The call returns a JSON.

### All floors in a building

  > Results for all floors inside a building

  ```shell
  curl -XGET \
    -H "Authorization: your jwt token" \
    -H "Accept-Version: v1" \
    'https://subdomain.rifiniti.com/client_api/results/building/BLD1/floors?from=2018-01-24&to=2018-01-24'
  ```

  List results for all floors in a building.

  `GET /client_api/results/building/{{Building Identifier}}/floors`

  The call returns an array of JSONs with each floor results.

### All floors in all buildings

  > Results for all buildings all floors

  ```shell
  curl -XGET \
    -H "Authorization: your jwt token" \
    -H "Accept-Version: v1" \
    'https://subdomain.rifiniti.com/client_api/results/floors?from=2018-01-24&to=2018-01-24'
  ```

  List results for all floors all buildings.

  `GET /client_api/results/floors`

  The call returns an array of JSONs with each floor results.