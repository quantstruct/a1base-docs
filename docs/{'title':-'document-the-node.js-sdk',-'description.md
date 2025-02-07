
# Node.js SDK Documentation

## Introduction

This document provides a comprehensive guide to the Node.js SDK, designed to simplify integration with our messaging platform. It covers installation, configuration, usage, API reference, and best practices. This SDK allows Node.js developers to easily interact with our API endpoints for sending messages, managing threads, and handling email functionalities.

## Installation

You can install the SDK using npm or yarn:

```bash
npm install your-sdk-name
```

or

```bash
yarn add your-sdk-name
```

## Configuration

Before using the SDK, you need to configure it with your API credentials. This typically involves setting your account ID, API key, and API secret.

```javascript
const YourSDK = require('your-sdk-name');

const sdk = new YourSDK({
  accountId: 'YOUR_ACCOUNT_ID',
  apiKey: 'YOUR_API_KEY',
  apiSecret: 'YOUR_API_SECRET',
});
```

Replace `YOUR_ACCOUNT_ID`, `YOUR_API_KEY`, and `YOUR_API_SECRET` with your actual credentials.  These can be found in your account dashboard.

## SDK Reference

This section provides a detailed reference for the SDK's methods and functionalities.

### Messages

#### Sending Individual Messages

```javascript
/**
 * Sends an individual message.
 *
 * @param {string} to - Recipient phone number.
 * @param {string} from - Sender phone number.
 * @param {string} body - Message body text.
 * @param {string} service - Messaging service (e.g., whatsapp, telegram).
 * @param {string} [attachmentUri] - Optional file/media attachment URI.
 * @returns {Promise<object>} - A promise that resolves with the message details.
 */
async function sendMessage(to, from, body, service, attachmentUri) {
  try {
    const response = await sdk.messages.sendIndividualMessage({
      accountId: sdk.accountId,
      to: to,
      from: from,
      body: body,
      service: service,
      attachmentUri: attachmentUri,
    });
    return response;
  } catch (error) {
    console.error("Error sending message:", error);
    throw error;
  }
}
```

**Example Usage:**

```javascript
sendMessage('61433174782', '61421868490', 'Hello from Node.js SDK!', 'whatsapp')
  .then(message => {
    console.log('Message sent:', message);
  })
  .catch(error => {
    console.error('Failed to send message:', error);
  });
```

**API Endpoint:** `POST /v1/messages/individual/{accountId}/send`

**Parameters:**

| Parameter      | Type   | Description                               | Required |
|----------------|--------|-------------------------------------------|----------|
| `accountId`    | string | Your account ID.                          | Yes      |
| `to`           | string | Recipient phone number.                   | Yes      |
| `from`         | string | Sender phone number.                      | Yes      |
| `body`         | string | Message body text.                        | Yes      |
| `service`      | string | Messaging service (whatsapp, telegram).  | Yes      |
| `attachmentUri`| string | Optional file/media attachment URI.       | No       |

**Returns:**

```json
{
  "to": "61433174782",
  "from": "61421868490",
  "body": "Hello from Node.js SDK!",
  "status": "queued"
}
```

#### Sending Group Messages

```javascript
/**
 * Sends a message to a group.
 *
 * @param {string} threadId - The ID of the chat group/thread.
 * @param {string} from - Sender phone number.
 * @param {string} body - Message body text.
 * @param {string} service - Messaging service (e.g., whatsapp, telegram).
 * @param {string} [attachmentUri] - Optional file/media attachment URI.
 * @returns {Promise<object>} - A promise that resolves with the message details.
 */
async function sendGroupMessage(threadId, from, body, service, attachmentUri) {
  try {
    const response = await sdk.messages.sendGroupMessage({
      accountId: sdk.accountId,
      threadId: threadId,
      from: from,
      body: body,
      service: service,
      attachmentUri: attachmentUri,
    });
    return response;
  } catch (error) {
    console.error("Error sending group message:", error);
    throw error;
  }
}
```

**Example Usage:**

```javascript
sendGroupMessage('group123', '61421868490', 'Hello everyone!', 'whatsapp')
  .then(message => {
    console.log('Group message sent:', message);
  })
  .catch(error => {
    console.error('Failed to send group message:', error);
  });
```

**API Endpoint:** `POST /v1/messages/group/{accountId}/send`

**Parameters:**

| Parameter      | Type   | Description                               | Required |
|----------------|--------|-------------------------------------------|----------|
| `accountId`    | string | Your account ID.                          | Yes      |
| `threadId`     | string | The ID of the chat group/thread.          | Yes      |
| `from`         | string | Sender phone number.                      | Yes      |
| `body`         | string | Message body text.                        | Yes      |
| `service`      | string | Messaging service (whatsapp, telegram).  | Yes      |
| `attachmentUri`| string | Optional file/media attachment URI.       | No       |

**Returns:**

```json
{
  "thread_id": "group123",
  "body": "Hello everyone!",
  "status": "queued"
}
```

#### Getting Message Details

```javascript
/**
 * Retrieves details for a specific message.
 *
 * @param {string} messageId - The ID of the message to retrieve.
 * @returns {Promise<object>} - A promise that resolves with the message details.
 */
async function getMessageDetails(messageId) {
  try {
    const response = await sdk.messages.getMessageDetails({
      accountId: sdk.accountId,
      messageId: messageId,
    });
    return response;
  } catch (error) {
    console.error("Error getting message details:", error);
    throw error;
  }
}
```

**Example Usage:**

```javascript
getMessageDetails('message123')
  .then(details => {
    console.log('Message details:', details);
  })
  .catch(error => {
    console.error('Failed to get message details:', error);
  });
```

**API Endpoint:** `GET /v1/messages/individual/{accountId}/get-details/{messageId}`

**Parameters:**

| Parameter   | Type   | Description                  | Required |
|-------------|--------|------------------------------|----------|
| `accountId` | string | Your account ID.             | Yes      |
| `messageId` | string | The ID of the message.       | Yes      |

**Returns:**

```json
{
  "id": "message123",
  "to": "61433174782",
  "from": "61421868490",
  "body": "Hello from Node.js SDK!",
  "status": "sent",
  "timestamp": "2024-01-01T00:00:00Z"
}
```

#### Getting Recent Messages in a Thread

```javascript
/**
 * Retrieves recent messages from a specific thread.
 *
 * @param {string} threadId - The ID of the thread to retrieve messages from.
 * @returns {Promise<object>} - A promise that resolves with the recent messages.
 */
async function getRecentMessages(threadId) {
  try {
    const response = await sdk.messages.getRecentMessages({
      accountId: sdk.accountId,
      threadId: threadId,
    });
    return response;
  } catch (error) {
    console.error("Error getting recent messages:", error);
    throw error;
  }
}
```

**Example Usage:**

```javascript
getRecentMessages('thread123')
  .then(messages => {
    console.log('Recent messages:', messages);
  })
  .catch(error => {
    console.error('Failed to get recent messages:', error);
  });
```

**API Endpoint:** `GET /v1/messages/threads/{accountId}/get-recent/{threadId}`

**Parameters:**

| Parameter   | Type   | Description                  | Required |
|-------------|--------|------------------------------|----------|
| `accountId` | string | Your account ID.             | Yes      |
| `threadId` | string | The ID of the thread.       | Yes      |

**Returns:**

```json
[
  {
    "id": "message1",
    "body": "Hello!",
    "sender": "61421868490",
    "timestamp": "2024-01-01T00:00:00Z"
  },
  {
    "id": "message2",
    "body": "Hi there!",
    "sender": "61433174782",
    "timestamp": "2024-01-01T00:00:10Z"
  }
]
```

#### Getting Chat Group Details

```javascript
/**
 * Retrieves details for a specific chat group.
 *
 * @param {string} threadId - The ID of the chat group to retrieve.
 * @returns {Promise<object>} - A promise that resolves with the chat group details.
 */
async function getChatGroupDetails(threadId) {
  try {
    const response = await sdk.messages.getChatGroupDetails({
      accountId: sdk.accountId,
      threadId: threadId,
    });
    return response;
  } catch (error) {
    console.error("Error getting chat group details:", error);
    throw error;
  }
}
```

**Example Usage:**

```javascript
getChatGroupDetails('group123')
  .then(details => {
    console.log('Chat group details:', details);
  })
  .catch(error => {
    console.error('Failed to get chat group details:', error);
  });
```

**API Endpoint:** `GET /v1/messages/threads/{accountId}/get-details/{threadId}`

**Parameters:**

| Parameter   | Type   | Description                  | Required |
|-------------|--------|------------------------------|----------|
| `accountId` | string | Your account ID.             | Yes      |
| `threadId` | string | The ID of the chat group.       | Yes      |

**Returns:**

```json
{
  "id": "group123",
  "name": "My Group",
  "participants": ["61421868490", "61433174782"]
}
```

#### Getting All Threads

```javascript
/**
 * Retrieves all threads for an account.
 *
 * @returns {Promise<object>} - A promise that resolves with all threads.
 */
async function getAllThreads() {
  try {
    const response = await sdk.messages.getAllThreads({
      accountId: sdk.accountId,
    });
    return response;
  } catch (error) {
    console.error("Error getting all threads:", error);
    throw error;
  }
}
```

**Example Usage:**

```javascript
getAllThreads()
  .then(threads => {
    console.log('All threads:', threads);
  })
  .catch(error => {
    console.error('Failed to get all threads:', error);
  });
```

**API Endpoint:** `GET /v1/messages/threads/{accountId}/get-all`

**Parameters:**

| Parameter   | Type   | Description                  | Required |
|-------------|--------|------------------------------|----------|
| `accountId` | string | Your account ID.             | Yes      |

**Returns:**

```json
[
  {
    "id": "thread1",
    "name": "John Doe"
  },
  {
    "id": "thread2",
    "name": "Jane Smith"
  }
]
```

#### Getting All Threads for a Phone Number

```javascript
/**
 * Retrieves all threads for a specific phone number.
 *
 * @param {string} phoneNumber - The phone number to retrieve threads for.
 * @returns {Promise<object>} - A promise that resolves with all threads for the phone number.
 */
async function getAllThreadsForPhoneNumber(phoneNumber) {
  try {
    const response = await sdk.messages.getAllThreadsForPhoneNumber({
      accountId: sdk.accountId,
      phoneNumber: phoneNumber,
    });
    return response;
  } catch (error) {
    console.error("Error getting all threads for phone number:", error);
    throw error;
  }
}
```

**Example Usage:**

```javascript
getAllThreadsForPhoneNumber('61433174782')
  .then(threads => {
    console.log('All threads for phone number:', threads);
  })
  .catch(error => {
    console.error('Failed to get all threads for phone number:', error);
  });
```

**API Endpoint:** `GET /v1/messages/threads/{accountId}/get-all/{phone_number}`

**Parameters:**

| Parameter     | Type   | Description                      | Required |
|---------------|--------|----------------------------------|----------|
| `accountId`   | string | Your account ID.                 | Yes      |
| `phoneNumber` | string | The phone number to search for. | Yes      |

**Returns:**

```json
[
  {
    "id": "thread1",
    "name": "John Doe"
  }
]
```

### Emails

#### Creating an Email

```javascript
/**
 * Creates a new email.
 *
 * @returns {Promise<object>} - A promise that resolves with the email creation details.
 */
async function createEmail() {
  try {
    const response = await sdk.emails.createEmail({
      accountId: sdk.accountId,
    });
    return response;
  } catch (error) {
    console.error("Error creating email:", error);
    throw error;
  }
}
```

**Example Usage:**

```javascript
createEmail()
  .then(email => {
    console.log('Email created:', email);
  })
  .catch(error => {
    console.error('Failed to create email:', error);
  });
```

**API Endpoint:** `POST /v1/emails/{accountId}/create-email`

**Parameters:**

| Parameter   | Type   | Description                  | Required |
|-------------|--------|------------------------------|----------|
| `accountId` | string | Your account ID.             | Yes      |

**Returns:**

```json
{
  "id": "email123",
  "status": "created"
}
```

#### Sending an Email

```javascript
/**
 * Sends an email.
 *
 * @param {string} senderAddress - The sender's email address.
 * @param {string} recipientAddress - The recipient's email address.
 * @param {string} subject - The email subject.
 * @param {string} body - The email body.
 * @param {object} [headers] - Optional email headers (e.g., cc, bcc, reply-to).
 * @param {string} [attachmentUri] - Optional file/media attachment URI.
 * @returns {Promise<object>} - A promise that resolves with the email sending details.
 */
async function sendEmail(senderAddress, recipientAddress, subject, body, headers, attachmentUri) {
  try {
    const response = await sdk.emails.sendEmail({
      accountId: sdk.accountId,
      senderAddress: senderAddress,
      recipientAddress: recipientAddress,
      subject: subject,
      body: body,
      headers: headers,
      attachmentUri: attachmentUri,
    });
    return response;
  } catch (error) {
    console.error("Error sending email:", error);
    throw error;
  }
}
```

**Example Usage:**

```javascript
sendEmail(
  'jane@a101.bot',
  'john@a101.bot',
  'Hello from Jane',
  'Have a nice day',
  {
    bcc: ['jim@a101.bot', 'james@a101.bot'],
    cc: ['sarah@a101.bot', 'janette@a101.bot'],
    'reply-to': 'jane@a101.bot',
  },
  'https://a101.bot/attachment.pdf'
)
  .then(email => {
    console.log('Email sent:', email);
  })
  .catch(error => {
    console.error('Failed to send email:', error);
  });
```

**API Endpoint:** `POST /v1/emails/{accountId}/send`

**Parameters:**

| Parameter        | Type   | Description                               | Required |
|------------------|--------|-------------------------------------------|----------|
| `accountId`      | string | Your account ID.                          | Yes      |
| `senderAddress`  | string | The sender's email address.               | Yes      |
| `recipientAddress`| string | The recipient's email address.            | Yes      |
| `subject`        | string | The email subject.                        | Yes      |
| `body`           | string | The email body.                           | Yes      |
| `headers`        | object | Optional email headers (cc, bcc, reply-to).| No       |
| `attachmentUri`  | string | Optional file/media attachment URI.       | No       |

**Returns:**

```json
{
  "to": "john@a101.bot",
  "from": "jane@email",
  "subject": "Hello from Jane",
  "body": "have a nice day",
  "status": "queued"
}
```

### Webhooks

The SDK also provides utilities for handling incoming webhooks.

#### Handling Incoming WhatsApp Messages

```javascript
/**
 * Handles incoming WhatsApp messages.
 *
 * @param {object} data - The webhook data.
 */
function handleIncomingWhatsAppMessage(data) {
  // Process the incoming WhatsApp message data
  console.log('Incoming WhatsApp message:', data);
}
```

**Example Usage (Express.js):**

```javascript
app.post('/v1/wa/whatsapp/incoming', (req, res) => {
  handleIncomingWhatsAppMessage(req.body);
  res.status(200).send('OK');
});
```

**API Endpoint:** `POST /v1/wa/whatsapp/incoming`

**Request Body Example:**

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

### Error Handling

The SDK throws exceptions for API errors.  It's important to catch these exceptions and handle them appropriately.

```javascript
try {
  await sendMessage('invalid-phone', '61421868490', 'Hello!', 'whatsapp');
} catch (error) {
  console.error('API Error:', error.message);
  // Handle the error, e.g., retry, log, or notify the user
}
```

### Best Practices

*   **Secure your API credentials:** Never expose your API key and secret in client-side code. Use environment variables or a secure configuration management system.
*   **Implement proper error handling:** Catch exceptions and handle them gracefully to prevent application crashes.
*   **Use asynchronous operations:**  All SDK methods are asynchronous, so use `async/await` or Promises to avoid blocking the main thread.
*   **Rate limiting:** Be mindful of API rate limits and implement appropriate retry mechanisms.

## Node.js Examples

This section provides complete examples of using the SDK in Node.js applications.

### Example: Sending a WhatsApp Message

```javascript
const YourSDK = require('your-sdk-name');

const sdk = new YourSDK({
  accountId: 'YOUR_ACCOUNT_ID',
  apiKey: 'YOUR_API_KEY',
  apiSecret: 'YOUR_API_SECRET',
});

async function sendWhatsAppMessage(to, from, body) {
  try {
    const response = await sdk.messages.sendIndividualMessage({
      accountId: sdk.accountId,
      to: to,
      from: from,
      body: body,
      service: 'whatsapp',
    });
    console.log('WhatsApp message sent:', response);
  } catch (error) {
    console.error('Failed to send WhatsApp message:', error);
  }
}

sendWhatsAppMessage('61433174782', '61421868490', 'Hello from Node.js!');
```

### Example: Handling Incoming Webhooks (Express.js)

```javascript
const express = require('express');
const bodyParser = require('body-parser');

const app = express();
const port = 3000;

app.use(bodyParser.json());

app.post('/v1/wa/whatsapp/incoming', (req, res) => {
  const data = req.body;
  console.log('Incoming WhatsApp message:', data);
  // Process the message data here
  res.status(200).send('OK');
});

app.listen(port, () => {
  console.log(`Server listening on port ${port}`);
});
```

## Support

If you encounter any issues or have questions about the SDK, please contact our support team.
