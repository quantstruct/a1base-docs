
# API Reference: Example Requests and Responses

This document provides example requests and responses for various API endpoints to help developers understand how to use the API and what to expect in return. These examples are designed to reduce integration time and improve overall developer experience.

## Messages Endpoints

### POST /messages/individual/{accountId}/send

**Description:** Send a message to an individual.

**Parameters:**

*   `accountId` (path): Your account ID.
*   `x-api-key` (header): Your API key.
*   `x-api-secret` (header): Your API secret.

**Request Body:**

```json
{
  "content": "Hello, how are you?",
  "attachment_uri": "https://example.com/image.jpg",
  "from": "61421868490",
  "to": "61433174782",
  "service": "whatsapp"
}
```

**Example Request (cURL):**

```bash
curl -X POST \
  'https://api.example.com/v1/messages/individual/your_account_id/send' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: your_api_key' \
  -H 'x-api-secret: your_api_secret' \
  -d '{
    "content": "Hello, how are you?",
    "attachment_uri": "https://example.com/image.jpg",
    "from": "61421868490",
    "to": "61433174782",
    "service": "whatsapp"
  }'
```

**Response (200 OK):**

```json
{
  "to": "61433174782",
  "from": "61421868490",
  "body": "Hello, how are you?",
  "status": "queued"
}
```

### POST /messages/group/{accountId}/send

**Description:** Send a message to a group.

**Parameters:**

*   `accountId` (path): Your account ID.
*   `x-api-key` (header): Your API key.
*   `x-api-secret` (header): Your API secret.

**Request Body:**

```json
{
  "content": "Important announcement!",
  "attachment_uri": "https://example.com/document.pdf",
  "from": "61421868490",
  "service": "whatsapp"
}
```

**Example Request (Python):**

```python
import requests
import json

url = "https://api.example.com/v1/messages/group/your_account_id/send"
headers = {
    "Content-Type": "application/json",
    "x-api-key": "your_api_key",
    "x-api-secret": "your_api_secret"
}
data = {
  "content": "Important announcement!",
  "attachment_uri": "https://example.com/document.pdf",
  "from": "61421868490",
  "service": "whatsapp"
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```

**Response (200 OK):**

```json
{
  "thread_id": "group_thread_123",
  "body": "Important announcement!",
  "status": "queued"
}
```

### GET /messages/threads/{accountId}/get-details/{threadId}

**Description:** Get details of a specific thread.

**Parameters:**

*   `accountId` (path): Your account ID.
*   `threadId` (path): The ID of the thread.
*   `x-api-key` (header): Your API key.
*   `x-api-secret` (header): Your API secret.

**Example Request (cURL):**

```bash
curl -X GET \
  'https://api.example.com/v1/messages/threads/your_account_id/get-details/thread_123' \
  -H 'x-api-key: your_api_key' \
  -H 'x-api-secret: your_api_secret'
```

**Response (200 OK):**

```json
{
  "thread_id": "thread_123",
  "participants": ["61421868490", "61433174782"],
  "created_at": "2024-01-01T10:00:00Z"
}
```

### GET /messages/threads/{accountId}/get-all/{phone_number}

**Description:** Get all threads associated with a specific phone number.

**Parameters:**

*   `accountId` (path): Your account ID.
*   `phone_number` (path): The phone number to search for.
*   `x-api-key` (header): Your API key.
*   `x-api-secret` (header): Your API secret.

**Example Request (cURL):**

```bash
curl -X GET \
  'https://api.example.com/v1/messages/threads/your_account_id/get-all/61433174782' \
  -H 'x-api-key: your_api_key' \
  -H 'x-api-secret: your_api_secret'
```

**Response (200 OK):**

```json
[
  {
    "thread_id": "thread_123",
    "participants": ["61421868490", "61433174782"],
    "created_at": "2024-01-01T10:00:00Z"
  },
  {
    "thread_id": "thread_456",
    "participants": ["61433174782", "61498765432"],
    "created_at": "2024-01-05T14:30:00Z"
  }
]
```

### GET /messages/threads/{accountId}/get-recent/{threadId}

**Description:** Get the most recent messages from a specific thread.

**Parameters:**

*   `accountId` (path): Your account ID.
*   `threadId` (path): The ID of the thread.
*   `x-api-key` (header): Your API key.
*   `x-api-secret` (header): Your API secret.

**Example Request (cURL):**

```bash
curl -X GET \
  'https://api.example.com/v1/messages/threads/your_account_id/get-recent/thread_123' \
  -H 'x-api-key: your_api_key' \
  -H 'x-api-secret: your_api_secret'
```

**Response (200 OK):**

```json
[
  {
    "message_id": "message_1",
    "sender": "61421868490",
    "body": "Hello!",
    "timestamp": "2024-01-01T10:05:00Z"
  },
  {
    "message_id": "message_2",
    "sender": "61433174782",
    "body": "Hi there!",
    "timestamp": "2024-01-01T10:10:00Z"
  }
]
```

### GET /messages/threads/{accountId}/get-all

**Description:** Get all threads for an account.

**Parameters:**

*   `accountId` (path): Your account ID.
*   `x-api-key` (header): Your API key.
*   `x-api-secret` (header): Your API secret.

**Example Request (cURL):**

```bash
curl -X GET \
  'https://api.example.com/v1/messages/threads/your_account_id/get-all' \
  -H 'x-api-key: your_api_key' \
  -H 'x-api-secret: your_api_secret'
```

**Response (200 OK):**

```json
[
  {
    "thread_id": "thread_123",
    "participants": ["61421868490", "61433174782"],
    "created_at": "2024-01-01T10:00:00Z"
  },
  {
    "thread_id": "thread_456",
    "participants": ["61433174782", "61498765432"],
    "created_at": "2024-01-05T14:30:00Z"
  }
]
```

### GET /messages/individual/{accountId}/get-details/{messageId}

**Description:** Get details of a specific individual message.

**Parameters:**

*   `accountId` (path): Your account ID.
*   `messageId` (path): The ID of the message.
*   `x-api-key` (header): Your API key.
*   `x-api-secret` (header): Your API secret.

**Example Request (cURL):**

```bash
curl -X GET \
  'https://api.example.com/v1/messages/individual/your_account_id/get-details/message_123' \
  -H 'x-api-key: your_api_key' \
  -H 'x-api-secret: your_api_secret'
```

**Response (200 OK):**

```json
{
  "message_id": "message_123",
  "sender": "61421868490",
  "recipient": "61433174782",
  "body": "Hello!",
  "timestamp": "2024-01-01T10:05:00Z",
  "status": "sent"
}
```

## WhatsApp Endpoints

### POST /wa/whatsapp/incoming

**Description:** Endpoint to receive incoming WhatsApp messages. This is typically used as a webhook.

**Request Body:**

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

**Response (200 OK):**

```json
{
  "status": "received"
}
```

## Email Endpoints

### POST /v1/emails/{accountId}/send

**Description:** Send an email.

**Parameters:**

*   `accountId` (path): Your account ID.
*   `x-api-key` (header): Your API key.
*   `x-api-secret` (header): Your API secret.

**Request Body:**

```json
{
    "sender_address": "jane@a101.bot",
    "recipient_address": "john@a101.bot",
    "subject": "Hello from Jane",
    "body": "have a nice day",
    "headers": {
        "bcc": ["jim@a101.bot", "james@a101.bot"],
        "cc": ["sarah@a101.bot", "janette@a101.bot"],
        "reply-to": "jane@a101.bot"
    },
    "attachment_uri": "https://a101.bot/attachment.pdf"
}
```

**Example Request (cURL):**

```bash
curl -X POST \
  'https://api.example.com/v1/emails/your_account_id/send' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: your_api_key' \
  -H 'x-api-secret: your_api_secret' \
  -d '{
    "sender_address": "jane@a101.bot",
    "recipient_address": "john@a101.bot",
    "subject": "Hello from Jane",
    "body": "have a nice day",
    "headers": {
        "bcc": ["jim@a101.bot", "james@a101.bot"],
        "cc": ["sarah@a101.bot", "janette@a101.bot"],
        "reply-to": "jane@a101.bot"
    },
    "attachment_uri": "https://a101.bot/attachment.pdf"
  }'
```

**Response (200 OK):**

```json
{
  "to": "john@a101.bot",
  "from": "jane@email",
  "subject": "Hello from Jane",
  "body": "have a nice day",
  "status": "queued"
}
```

## Error Handling

Most endpoints return a 422 status code for validation errors. The response body will contain details about the specific validation errors encountered.

**Example Error Response (422 Validation Error):**

```json
{
  "detail": [
    {
      "loc": [
        "body",
        "content"
      ],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

This documentation will be updated regularly with more examples and information. Please refer to it during your integration process.
