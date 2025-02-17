
# Messaging Systems Integration

This document provides a comprehensive guide for integrating with our messaging services, covering individual messaging, group messaging, WhatsApp integration, and thread management. It is intended for developers and product managers who need to understand and utilize our messaging APIs.

## Introduction

Our platform offers a variety of messaging endpoints to facilitate communication with users across different channels. This document aims to unify the documentation and provide practical integration examples to streamline the development process.

## Individual Messaging

This section covers sending messages to individual users.

### Endpoint

`/v1/messages/individual/{accountId}/send`

### Description

This endpoint allows you to send a message to a single recipient.

### Request Body (Example)

```json
{
  "recipient": "+15551234567",
  "message": "Hello, this is a test message!"
}
```

### Response (Example)

```json
{
  "messageId": "unique_message_id",
  "status": "sent",
  "timestamp": "2023-10-27T10:00:00Z"
}
```

### Error Handling

| Status Code | Description                                  |
|-------------|----------------------------------------------|
| 400         | Invalid request body or missing parameters |
| 404         | Account not found                            |
| 500         | Internal server error                        |

## Group Messaging

This section covers sending messages to groups of users.

### Endpoint

`/v1/messages/group/{accountId}/send`

### Description

This endpoint allows you to send a message to a group of recipients.

### Request Body (Example)

```json
{
  "groupId": "group_id_123",
  "message": "Announcement: Meeting rescheduled to tomorrow."
}
```

### Response (Example)

```json
{
  "messageId": "unique_group_message_id",
  "status": "sent",
  "timestamp": "2023-10-27T10:05:00Z"
}
```

### Error Handling

| Status Code | Description                                  |
|-------------|----------------------------------------------|
| 400         | Invalid request body or missing parameters |
| 404         | Account or Group not found                   |
| 500         | Internal server error                        |

## WhatsApp Integration

This section details how to integrate with WhatsApp using our platform.

### Endpoint

`/v1/wa/whatsapp/incoming`

### Description

This endpoint receives incoming WhatsApp messages.  You will need to configure a webhook in your WhatsApp Business Account settings to point to this endpoint.

### Webhook Setup Guide

1.  **Configure Webhook URL:** In your WhatsApp Business Account settings, configure the webhook URL to point to `/v1/wa/whatsapp/incoming`.
2.  **Verify Token:**  Set up a verification token in your WhatsApp Business Account settings and ensure your application can handle the verification request.
3.  **Subscribe to Events:** Subscribe to the `messages` event to receive incoming message notifications.

### Incoming Message Payload (Example)

```json
{
  "object": "whatsapp_business_account",
  "entry": [
    {
      "id": "whatsapp_business_account_id",
      "time": 1666886666,
      "changes": [
        {
          "value": {
            "messaging_product": "whatsapp",
            "metadata": {
              "display_phone_number": "+15550000000",
              "phone_number_id": "whatsapp_phone_number_id"
            },
            "contacts": [
              {
                "profile": {
                  "name": "John Doe"
                },
                "wa_id": "+15551234567"
              }
            ],
            "messages": [
              {
                "from": "+15551234567",
                "id": "wamid.xxxxxxxxxxxxxxxxxxxxxxxxx",
                "timestamp": "1666886666",
                "text": {
                  "body": "Hello from WhatsApp!"
                },
                "type": "text"
              }
            ]
          },
          "field": "messages"
        }
      ]
    }
  ]
}
```

### Responding to WhatsApp Messages

To respond to a WhatsApp message, use the `/v1/messages/individual/{accountId}/send` endpoint, ensuring the `recipient` is the `wa_id` from the incoming message.

## Thread Management

This section will cover how to manage message threads.  (This section is a placeholder and will be expanded in a future update.)

*   **Thread IDs:**  Each conversation will have a unique thread ID.
*   **Retrieving Messages:**  Endpoints for retrieving messages within a specific thread.
*   **Archiving Threads:**  Functionality to archive or close threads.

## Service-Specific Features

This section details features specific to each messaging service.

### WhatsApp

*   **Templates:**  Using pre-approved message templates for outbound communication.
*   **Interactive Messages:**  Implementing interactive message types like buttons and lists.
*   **Media Messages:**  Sending images, videos, and other media files.

### SMS

*   **Long SMS:**  Handling messages that exceed the standard SMS character limit.
*   **Delivery Reports:**  Receiving delivery reports for SMS messages.

## Integration Tutorials

### Example: Sending a Welcome Message via WhatsApp

This tutorial demonstrates how to send a welcome message to a new user via WhatsApp when they sign up for your service.

1.  **User Signup:**  When a new user signs up, collect their phone number.
2.  **Format Phone Number:** Ensure the phone number is in the correct format (e.g., +15551234567).
3.  **Send Welcome Message:** Use the `/v1/messages/individual/{accountId}/send` endpoint to send a welcome message to the user's WhatsApp number.  You may need to use a WhatsApp template for this.

```python
import requests
import json

account_id = "your_account_id"
recipient = "+15551234567"
template_name = "welcome_message" # Replace with your template name
template_language = "en_US"

url = f"/v1/messages/individual/{account_id}/send"
headers = {
    "Content-Type": "application/json",
    "Authorization": "Bearer YOUR_API_KEY" # Replace with your API key
}

data = {
    "recipient": recipient,
    "template": {
        "name": template_name,
        "language": template_language,
        "components": [
            {
                "type": "body",
                "parameters": [
                    {
                        "type": "text",
                        "text": "User's Name" # Replace with the user's actual name
                    }
                ]
            }
        ]
    }
}

response = requests.post(url, headers=headers, data=json.dumps(data))

if response.status_code == 200:
    print("Welcome message sent successfully!")
    print(response.json())
else:
    print(f"Error sending welcome message: {response.status_code} - {response.text}")
```

## Conclusion

This document provides a starting point for integrating with our messaging systems.  Refer to the API reference for more detailed information on each endpoint and its parameters.  We will continue to update this document with more examples and tutorials.
