
# Define Request and Response Schemas for API Endpoints

## Description

Many API endpoints currently lack defined request and response schemas in the OpenAPI specification. This makes it difficult for developers to understand the expected data format and can lead to integration errors.  This document outlines the need to define these schemas for improved developer experience and reduced integration issues.

## Priority

High

## Rationale

Clear API schemas are crucial for developer understanding and ease of integration. Without them, developers must rely on example code or trial and error, which is inefficient and error-prone.  Defined schemas provide a contract that developers can rely on, leading to faster development cycles and fewer errors.

## Target Audience

Developers

## API Reference

The following endpoints are missing schema definitions and require updates:

*   `POST /messages/group/{accountId}/send`
*   `GET /messages/threads/{accountId}/get-all`
*   `GET /messages/threads/{accountId}/get-all/{phone_number}`
*   `GET /messages/threads/{accountId}/get-recent/{threadId}`
*   `GET /messages/threads/{accountId}/get-details/{threadId}`
*   `POST /emails/{accountId}/create-email`
*   `POST /emails/{accountId}/send`
*   `POST /wa/whatsapp/incoming`

## Suggested Schema Definitions (Examples)

The following are examples of how the schemas could be defined.  These are suggestions and should be reviewed and adjusted based on the actual API requirements.  These examples use OpenAPI Specification (Swagger) format.

### 1. `POST /messages/group/{accountId}/send`

**Request Body (Example):**

```yaml
requestBody:
  required: true
  content:
    application/json:
      schema:
        type: object
        properties:
          content:
            type: string
            description: Message body text
          attachment_uri:
            type: string
            description: Optional file/media attachment URI
          from:
            type: string
            description: Sender phone number
          service:
            type: string
            description: Chat service (e.g., whatsapp, telegram)
            enum: [whatsapp, telegram]
        required:
          - content
          - from
          - service
```

**Response (Example):**

```yaml
responses:
  '200':
    description: Successful Response
    content:
      application/json:
        schema:
          type: object
          properties:
            thread_id:
              type: string
              description: The ID of the thread.
            body:
              type: string
              description: The content of the message.
            status:
              type: string
              description: The status of the message (e.g., queued, sent).
              enum: [queued, sent]
          required:
            - thread_id
            - body
            - status
  '422':
    description: Validation Error
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/HTTPValidationError'
```

### 2. `GET /messages/threads/{accountId}/get-all`

**Response (Example):**

```yaml
responses:
  '200':
    description: Successful Response
    content:
      application/json:
        schema:
          type: array
          items:
            type: object
            properties:
              thread_id:
                type: string
                description: The ID of the thread.
              participants:
                type: array
                items:
                  type: string
                  description: Phone number of a participant.
              last_message:
                type: string
                description: The content of the last message in the thread.
              last_message_timestamp:
                type: integer
                format: int64
                description: Timestamp of the last message.
            required:
              - thread_id
              - participants
              - last_message
              - last_message_timestamp
  '422':
    description: Validation Error
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/HTTPValidationError'
```

### 3. `GET /messages/threads/{accountId}/get-all/{phone_number}`

**Response (Example):**

```yaml
responses:
  '200':
    description: Successful Response
    content:
      application/json:
        schema:
          type: array
          items:
            type: object
            properties:
              thread_id:
                type: string
                description: The ID of the thread.
              participants:
                type: array
                items:
                  type: string
                  description: Phone number of a participant.
              last_message:
                type: string
                description: The content of the last message in the thread.
              last_message_timestamp:
                type: integer
                format: int64
                description: Timestamp of the last message.
            required:
              - thread_id
              - participants
              - last_message
              - last_message_timestamp
  '422':
    description: Validation Error
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/HTTPValidationError'
```

### 4. `GET /messages/threads/{accountId}/get-recent/{threadId}`

**Response (Example):**

```yaml
responses:
  '200':
    description: Successful Response
    content:
      application/json:
        schema:
          type: array
          items:
            type: object
            properties:
              message_id:
                type: string
                description: The ID of the message.
              sender:
                type: string
                description: The sender's phone number.
              content:
                type: string
                description: The content of the message.
              timestamp:
                type: integer
                format: int64
                description: Timestamp of the message.
            required:
              - message_id
              - sender
              - content
              - timestamp
  '422':
    description: Validation Error
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/HTTPValidationError'
```

### 5. `GET /messages/threads/{accountId}/get-details/{threadId}`

**Response (Example):**

```yaml
responses:
  '200':
    description: Successful Response
    content:
      application/json:
        schema:
          type: object
          properties:
            thread_id:
              type: string
              description: The ID of the thread.
            participants:
              type: array
              items:
                type: string
                description: Phone number of a participant.
            created_at:
              type: integer
              format: int64
              description: Timestamp of thread creation.
          required:
            - thread_id
            - participants
            - created_at
  '422':
    description: Validation Error
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/HTTPValidationError'
```

### 6. `POST /emails/{accountId}/create-email`

**Request Body (Example):**

```yaml
requestBody:
  required: true
  content:
    application/json:
      schema:
        type: object
        properties:
          sender_address:
            type: string
            format: email
            description: Sender email address
          recipient_address:
            type: string
            format: email
            description: Recipient email address
          subject:
            type: string
            description: Email subject
          body:
            type: string
            description: Email body
          headers:
            type: object
            description: Optional email headers (e.g., CC, BCC)
            properties:
              cc:
                type: array
                items:
                  type: string
                  format: email
              bcc:
                type: array
                items:
                  type: string
                  format: email
              reply-to:
                type: string
                format: email
          attachment_uri:
            type: string
            format: uri
            description: URI of the email attachment
        required:
          - sender_address
          - recipient_address
          - subject
          - body
```

**Response (Example):**

```yaml
responses:
  '200':
    description: Successful Response
    content:
      application/json:
        schema:
          type: object
          properties:
            email_id:
              type: string
              description: The ID of the created email.
            status:
              type: string
              description: The status of the email (e.g., created, queued).
              enum: [created, queued]
          required:
            - email_id
            - status
  '422':
    description: Validation Error
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/HTTPValidationError'
```

### 7. `POST /emails/{accountId}/send`

**Request Body (Example):**

```yaml
requestBody:
  required: true
  content:
    application/json:
      schema:
        type: object
        properties:
          sender_address:
            type: string
            format: email
            description: Sender email address
          recipient_address:
            type: string
            format: email
            description: Recipient email address
          subject:
            type: string
            description: Email subject
          body:
            type: string
            description: Email body
          headers:
            type: object
            description: Optional email headers (e.g., CC, BCC)
            properties:
              cc:
                type: array
                items:
                  type: string
                  format: email
              bcc:
                type: array
                items:
                  type: string
                  format: email
              reply-to:
                type: string
                format: email
          attachment_uri:
            type: string
            format: uri
            description: URI of the email attachment
        required:
          - sender_address
          - recipient_address
          - subject
          - body
```

**Response (Example):**

```yaml
responses:
  '200':
    description: Successful Response
    content:
      application/json:
        schema:
          type: object
          properties:
            to:
              type: string
              format: email
              description: Recipient email address
            from:
              type: string
              format: email
              description: Sender email address
            subject:
              type: string
              description: Email subject
            body:
              type: string
              description: Email body
            status:
              type: string
              description: The status of the email (e.g., queued, sent).
              enum: [queued, sent]
          required:
            - to
            - from
            - subject
            - body
            - status
  '422':
    description: Validation Error
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/HTTPValidationError'
```

### 8. `POST /wa/whatsapp/incoming`

**Request Body (Example):**

```yaml
requestBody:
  required: true
  content:
    application/json:
      schema:
        type: object
        properties:
          external_thread_id:
            type: string
            description: External thread ID (e.g., WhatsApp thread ID)
          external_message_id:
            type: string
            description: External message ID (e.g., WhatsApp message ID)
          chat_type:
            type: string
            description: Type of chat (e.g., group, individual, broadcast)
            enum: [group, individual, broadcast]
          content:
            type: string
            description: Message content
          sender_name:
            type: string
            description: Sender's name
          sender_number:
            type: string
            description: Sender's phone number
          participants:
            type: array
            items:
              type: string
              description: Phone number of a participant
          a1_account_number:
            type: string
            description: Account number
          timestamp:
            type: integer
            format: int64
            description: Timestamp of the message
          secret_key:
            type: string
            description: Secret key for verification
        required:
          - external_thread_id
          - external_message_id
          - chat_type
          - content
          - sender_name
          - sender_number
          - participants
          - a1_account_number
          - timestamp
          - secret_key
```

**Response (Example):**

```yaml
responses:
  '200':
    description: Successful Response
    content:
      application/json:
        schema:
          type: object
          properties:
            status:
              type: string
              description: Status of the incoming message processing (e.g., received, processed).
              enum: [received, processed]
          required:
            - status
  '422':
    description: Validation Error
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/HTTPValidationError'
```

## OpenAPI Specification Updates

The above schema definitions should be incorporated into the OpenAPI specification.  This will involve adding `requestBody` and `responses` sections with the appropriate schema definitions to each of the listed endpoints.  The `components/schemas` section may also need to be updated to include reusable schema definitions.

## Next Steps

1.  **Review and Refine:**  The suggested schemas should be reviewed and refined by the development team to ensure they accurately reflect the API's behavior.
2.  **Implement in OpenAPI Specification:**  The finalized schemas should be implemented in the OpenAPI specification.
3.  **Validate:**  The updated OpenAPI specification should be validated to ensure it is syntactically correct.
4.  **Test:**  The API should be tested with the new schemas to ensure that requests and responses are correctly validated.
