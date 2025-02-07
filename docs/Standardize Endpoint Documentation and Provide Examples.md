
# Standardized API Endpoint Documentation

This document outlines the standardized format for documenting API endpoints. Consistent and complete endpoint documentation is essential for developers to understand and use the API effectively. This standard includes descriptions of request parameters, request body examples (where applicable), and response examples.

## API Reference

This section provides detailed documentation for each API endpoint, following the standardized format.

### General Guidelines

*   **Clarity:** Use clear and concise language. Avoid jargon where possible.
*   **Completeness:** Include all necessary information for a developer to successfully use the endpoint.
*   **Consistency:** Follow the format outlined below for all endpoints.
*   **Examples:** Provide realistic and practical examples for both requests and responses.

### Endpoint Documentation Format

Each endpoint documentation should include the following sections:

1.  **Endpoint:** The full URL of the endpoint.
2.  **Method:** The HTTP method used for the endpoint (e.g., GET, POST, PUT, DELETE).
3.  **Summary:** A brief description of what the endpoint does.
4.  **Description:** A more detailed explanation of the endpoint's functionality, including any important considerations or limitations.
5.  **Parameters:** A table describing the request parameters.
6.  **Request Body (if applicable):** A JSON example of the request body.
7.  **Response:** A description of the possible responses.
8.  **Response Example:** A JSON example of a successful response.
9.  **Error Codes:** A list of possible error codes and their meanings.

### Parameter Table Format

| Parameter | Type | In | Required | Description | Example |
|---|---|---|---|---|---|
| `parameter_name` | `string`, `integer`, `boolean`, etc. | `path`, `query`, `header`, `cookie` | `true`, `false` | A description of the parameter and its purpose. | Example value |

### Examples

Below are examples of how to document specific endpoints, adhering to the standardized format.

#### 1. Send Individual Message

*   **Endpoint:** `/v1/messages/individual/{accountId}/send`
*   **Method:** POST
*   **Summary:** Sends a message to an individual recipient.
*   **Description:** This endpoint allows you to send a message to a single recipient via WhatsApp or Telegram. The message content, sender, and recipient phone numbers are required. The message sent status is returned via webhook.

*   **Parameters:**

    | Parameter | Type | In | Required | Description | Example |
    |---|---|---|---|---|---|
    | `accountId` | `string` | `path` | `true` | The ID of the account sending the message. | `your_account_id` |
    | `x-api-key` | `string` | `header` | `true` | Your API key for authentication. | `your_api_key` |
    | `x-api-secret` | `string` | `header` | `true` | Your API secret for authentication. | `your_api_secret` |

*   **Request Body:**

    ```json
    {
      "content": "Hello, how are you?",
      "attachment_uri": "https://example.com/image.jpg",
      "from": "61421868490",
      "to": "61433174782",
      "service": "whatsapp"
    }
    ```

*   **Response:**

    A successful request returns a JSON object containing the message details and status.

*   **Response Example:**

    ```json
    {
      "to": "61433174782",
      "from": "61421868490",
      "body": "Hello, how are you?",
      "status": "queued"
    }
    ```

*   **Error Codes:**

    *   `400 Bad Request`: Invalid request body or parameters.
    *   `401 Unauthorized`: Invalid API key or secret.
    *   `422 Unprocessable Entity`: Validation error (e.g., invalid phone number format).
    *   `500 Internal Server Error`: An unexpected error occurred on the server.

#### 2. Get Chat Group Details

*   **Endpoint:** `/v1/messages/threads/{accountId}/get-details/{threadId}`
*   **Method:** GET
*   **Summary:** Retrieves details for a specific chat group.
*   **Description:** This endpoint retrieves information about a chat group, identified by its `threadId`.

*   **Parameters:**

    | Parameter | Type | In | Required | Description | Example |
    |---|---|---|---|---|---|
    | `accountId` | `string` | `path` | `true` | The ID of the account. | `your_account_id` |
    | `threadId` | `string` | `path` | `true` | The ID of the chat group. | `group_123` |
    | `x-api-key` | `string` | `header` | `true` | Your API key for authentication. | `your_api_key` |
    | `x-api-secret` | `string` | `header` | `true` | Your API secret for authentication. | `your_api_secret` |

*   **Request Body:** N/A

*   **Response:**

    A successful request returns a JSON object containing the chat group details.

*   **Response Example:**

    ```json
    {
      "thread_id": "group_123",
      "name": "My Chat Group",
      "participants": ["61421868490", "61433174782"],
      "created_at": "2023-10-27T10:00:00Z"
    }
    ```

*   **Error Codes:**

    *   `400 Bad Request`: Invalid request parameters.
    *   `401 Unauthorized`: Invalid API key or secret.
    *   `404 Not Found`: Chat group with the specified `threadId` not found.
    *   `500 Internal Server Error`: An unexpected error occurred on the server.

#### 3. Send Email

*   **Endpoint:** `/v1/emails/{accountId}/send`
*   **Method:** POST
*   **Summary:** Sends an email.
*   **Description:** This endpoint allows you to send an email.  It requires the sender's address, recipient's address, subject, and body.  You can also specify headers like `bcc`, `cc`, and `reply-to`, and include an attachment URI.

*   **Parameters:**

    | Parameter | Type | In | Required | Description | Example |
    |---|---|---|---|---|---|
    | `accountId` | `string` | `path` | `true` | The ID of the account sending the email. | `your_account_id` |
    | `x-api-key` | `string` | `header` | `true` | Your API key for authentication. | `your_api_key` |
    | `x-api-secret` | `string` | `header` | `true` | Your API secret for authentication. | `your_api_secret` |

*   **Request Body:**

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

*   **Response:**

    A successful request returns a JSON object containing the email details and status.

*   **Response Example:**

    ```json
    {
        "to": "john@a101.bot",
        "from": "jane@email",
        "subject": "Hello from Jane",
        "body": "have a nice day",
        "status": "queued"
    }
    ```

*   **Error Codes:**

    *   `400 Bad Request`: Invalid request body or parameters.
    *   `401 Unauthorized`: Invalid API key or secret.
    *   `422 Unprocessable Entity`: Validation error (e.g., invalid email address format).
    *   `500 Internal Server Error`: An unexpected error occurred on the server.

#### 4. Whatsapp Incoming

*   **Endpoint:** `/v1/wa/whatsapp/incoming`
*   **Method:** POST
*   **Summary:** Receives incoming WhatsApp messages.
*   **Description:** This endpoint receives incoming WhatsApp messages and related data.

*   **Parameters:** N/A (Data is sent in the request body)

*   **Request Body:**

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

*   **Response:**

    A successful request returns a 200 OK response. The response body may be empty or contain a confirmation message.

*   **Response Example:**

    ```json
    {
        "status": "received"
    }
    ```

*   **Error Codes:**

    *   `400 Bad Request`: Invalid request body or parameters.
    *   `401 Unauthorized`: Invalid secret key.
    *   `500 Internal Server Error`: An unexpected error occurred on the server.

### Endpoints Requiring Updates

The following endpoints require documentation updates to meet the standardized format:

*   `/v1/messages/group/{accountId}/send`
*   `/v1/messages/send/{accountId}`
*   `/v1/messages/individual/{accountId}/get-details/{messageId}`
*   `/v1/messages/threads/{accountId}/get-recent/{threadId}`
*   `/v1/messages/threads/{accountId}/get-all`
*   `/v1/messages/threads/{accountId}/get-all/{phone_number}`
*   `/v1/emails/{accountId}/create-email`
*   `/v1/emails/mailserver/incoming`

This standardized documentation format will ensure that developers have the information they need to effectively use the API.  Regular review and updates are crucial to maintain accuracy and completeness.
