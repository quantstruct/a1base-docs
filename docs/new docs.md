
# Comprehensive API Request and Response Examples

This document provides detailed request and response examples for various API endpoints, including successful scenarios and common error cases. These examples are intended to help developers understand how to properly interact with the API.

## Request Examples

This section provides examples of how to construct requests to different API endpoints.  Examples are provided in both JSON (for request bodies) and cURL formats.  Remember to replace placeholder values with your actual data.

### POST /v1/messages/individual/{accountId}/send

This endpoint sends a message to an individual.

**JSON Request Body Example:**

```json
{
  "content": "Hello, this is a test message!",
  "attachment_uri": "https://example.com/attachment.pdf",
  "from": "61421868490",
  "to": "61433174782",
  "service": "whatsapp"
}
```

**cURL Example:**

```bash
curl -X POST \
  'https://your-api-domain.com/v1/messages/individual/your_account_id/send' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'x-api-secret: YOUR_API_SECRET' \
  -d '{
    "content": "Hello, this is a test message!",
    "attachment_uri": "https://example.com/attachment.pdf",
    "from": "61421868490",
    "to": "61433174782",
    "service": "whatsapp"
  }'
```

### POST /v1/messages/group/{accountId}/send

This endpoint sends a message to a group.

**JSON Request Body Example:**

```json
{
  "content": "Important announcement for the group!",
  "attachment_uri": "https://example.com/group_image.jpg",
  "from": "61421868490",
  "service": "whatsapp"
}
```

**cURL Example:**

```bash
curl -X POST \
  'https://your-api-domain.com/v1/messages/group/your_account_id/send' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'x-api-secret: YOUR_API_SECRET' \
  -d '{
    "content": "Important announcement for the group!",
    "attachment_uri": "https://example.com/group_image.jpg",
    "from": "61421868490",
    "service": "whatsapp"
  }'
```

### GET /v1/messages/threads/{accountId}/get-details/{threadId}

This endpoint retrieves details for a specific thread.

**cURL Example:**

```bash
curl -X GET \
  'https://your-api-domain.com/v1/messages/threads/your_account_id/get-details/your_thread_id' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'x-api-secret: YOUR_API_SECRET'
```

### POST /v1/wa/whatsapp/incoming

This endpoint receives incoming WhatsApp messages (webhook).

**JSON Request Body Example:**

```json
{
  "external_thread_id": "3456098@s.whatsapp",
  "external_message_id": "2asd5678cfvgh123",
  "chat_type": "group",
  "content": "Hello everyone!",
  "sender_name": "Bobby",
  "sender_number": "61421868490",
  "participants": ["61421868490", "61433174782"],
  "a1_account_number": "61421868490",
  "timestamp": 1734486451000,
  "secret_key": "xxx"
}
```

**cURL Example:**

```bash
curl -X POST \
  'https://your-api-domain.com/v1/wa/whatsapp/incoming' \
  -H 'Content-Type: application/json' \
  -d '{
  "external_thread_id": "3456098@s.whatsapp",
  "external_message_id": "2asd5678cfvgh123",
  "chat_type": "group",
  "content": "Hello everyone!",
  "sender_name": "Bobby",
  "sender_number": "61421868490",
  "participants": ["61421868490", "61433174782"],
  "a1_account_number": "61421868490",
  "timestamp": 1734486451000,
  "secret_key": "xxx"
}'
```

### GET /v1/messages/individual/{accountId}/get-details/{messageId}

This endpoint retrieves details for a specific individual message.

**cURL Example:**

```bash
curl -X GET \
  'https://your-api-domain.com/v1/messages/individual/your_account_id/get-details/your_message_id' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'x-api-secret: YOUR_API_SECRET'
```

### GET /v1/messages/threads/{accountId}/get-all/{phone_number}

This endpoint retrieves all threads associated with a specific phone number.

**cURL Example:**

```bash
curl -X GET \
  'https://your-api-domain.com/v1/messages/threads/your_account_id/get-all/61433174782' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'x-api-secret: YOUR_API_SECRET'
```

### GET /v1/messages/threads/{accountId}/get-recent/{threadId}

This endpoint retrieves the most recent messages in a specific thread.

**cURL Example:**

```bash
curl -X GET \
  'https://your-api-domain.com/v1/messages/threads/your_account_id/get-recent/your_thread_id' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'x-api-secret: YOUR_API_SECRET'
```

### GET /v1/messages/threads/{accountId}/get-all

This endpoint retrieves all threads for an account.

**cURL Example:**

```bash
curl -X GET \
  'https://your-api-domain.com/v1/messages/threads/your_account_id/get-all' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'x-api-secret: YOUR_API_SECRET'
```

## Response Examples

This section provides examples of successful and error responses for the API endpoints.

### Successful Response (200 OK)

**Example: POST /v1/messages/individual/{accountId}/send**

```json
{
  "to": "61433174782",
  "from": "61421868490",
  "body": "Hello, this is a test message!",
  "status": "queued"
}
```

**Example: GET /v1/messages/threads/{accountId}/get-details/{threadId}**

```json
{
  "thread_id": "your_thread_id",
  "participants": ["61421868490", "61433174782"],
  "created_at": "2024-01-01T00:00:00Z"
}
```

### Error Handling

This section provides examples of common error responses.

#### Validation Error (422 Unprocessable Entity)

This error indicates that the request body failed validation. The response body will contain details about the validation errors.

**Example Response:**

```json
{
  "detail": [
    {
      "loc": [
        "body",
        "to"
      ],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

This example indicates that the `to` field is missing from the request body.

#### Unauthorized (401 Unauthorized)

This error indicates that the API key or secret is invalid or missing.

**Example Response:**

```json
{
  "detail": "Invalid API key or secret"
}
```

#### Not Found (404 Not Found)

This error indicates that the requested resource was not found.

**Example Response:**

```json
{
  "detail": "Thread not found"
}
```

## Important Notes

*   Always replace placeholder values (e.g., `your_account_id`, `your_thread_id`, `YOUR_API_KEY`, `YOUR_API_SECRET`) with your actual data.
*   Ensure that your API key and secret are properly configured in the request headers.
*   Refer to the API reference documentation for a complete list of endpoints, parameters, and response codes.
*   The `x-api-key` and `x-api-secret` headers are mandatory for all requests.
*   The structure of the response bodies may vary depending on the specific endpoint.
