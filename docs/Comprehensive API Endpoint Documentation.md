
# Comprehensive API Endpoint Documentation

This document provides detailed information about the API endpoints, including request parameters, body schemas, response schemas, and examples. It is intended for API developers and system integrators who need to integrate with the API.

## Endpoint Summary

This section provides a brief overview of the endpoint, its purpose, and its functionality.

## Request Parameters

This section describes the parameters that can be passed in the request.

| Parameter | Type | Description | Required | Example |
|---|---|---|---|---|
| `param1` | `string` | A string parameter | Yes | `value1` |
| `param2` | `integer` | An integer parameter | No | `123` |

## Request Body

This section describes the structure of the request body, including the data types and descriptions of each field.

**Content-Type:** `application/json`

```json
{
  "field1": "string",
  "field2": 123,
  "field3": {
    "nestedField1": "string"
  }
}
```

**Schema:**

```json
{
  "type": "object",
  "properties": {
    "field1": {
      "type": "string",
      "description": "A string field"
    },
    "field2": {
      "type": "integer",
      "description": "An integer field"
    },
    "field3": {
      "type": "object",
      "description": "A nested object",
      "properties": {
        "nestedField1": {
          "type": "string",
          "description": "A nested string field"
        }
      }
    }
  },
  "required": [
    "field1"
  ]
}
```

## Response Codes

This section lists the possible HTTP response codes and their meanings.

| Code | Description |
|---|---|
| `200 OK` | The request was successful. |
| `201 Created` | A new resource was created successfully. |
| `400 Bad Request` | The request was invalid. |
| `401 Unauthorized` | Authentication is required. |
| `403 Forbidden` | The user does not have permission to access the resource. |
| `404 Not Found` | The resource was not found. |
| `500 Internal Server Error` | An unexpected error occurred on the server. |

## Response Body

This section describes the structure of the response body, including the data types and descriptions of each field.

**Content-Type:** `application/json`

```json
{
  "status": "success",
  "data": {
    "field1": "string",
    "field2": 123
  }
}
```

**Schema:**

```json
{
  "type": "object",
  "properties": {
    "status": {
      "type": "string",
      "description": "The status of the request (success or error)"
    },
    "data": {
      "type": "object",
      "description": "The response data",
      "properties": {
        "field1": {
          "type": "string",
          "description": "A string field"
        },
        "field2": {
          "type": "integer",
          "description": "An integer field"
        }
      }
    }
  },
  "required": [
    "status",
    "data"
  ]
}
```

## Example Requests

This section provides example requests using `curl`.

**Example 1: Successful Request**

```bash
curl -X POST \
  'https://api.example.com/endpoint' \
  -H 'Content-Type: application/json' \
  -d '{
    "field1": "example",
    "field2": 42
  }'
```

**Example 2: Request with Parameters**

```bash
curl -X GET \
  'https://api.example.com/endpoint?param1=value1&param2=123'
```

## Example Responses

This section provides example responses for different scenarios.

**Example 1: Successful Response (200 OK)**

```json
{
  "status": "success",
  "data": {
    "field1": "example",
    "field2": 42
  }
}
```

**Example 2: Error Response (400 Bad Request)**

```json
{
  "status": "error",
  "message": "Invalid request body"
}
```

**Example 3: Error Response (404 Not Found)**

```json
{
  "status": "error",
  "message": "Resource not found"
}
```

## Related Endpoints

*   `/v1/messages`: Documentation for the messages endpoint.
*   `/v1/emails`: Documentation for the emails endpoint.
