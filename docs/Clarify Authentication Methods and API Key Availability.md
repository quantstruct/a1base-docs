
# Authentication

This document explains how to authenticate with the API.  Proper authentication is required to access any of the API endpoints.

## Introduction

Currently, the API uses a combination of `X-API-Key` and `X-API-Secret` for authentication.  These headers must be included in every request you make to the API.

**Important:** API keys are currently not self-service. Please contact the founders to request your API key and secret. We are working towards providing a self-service option in the future.  Check back here for updates.

## Quickstart

To make a successful API call, you'll need to include the `X-API-Key` and `X-API-Secret` headers in your request.  Here's an example using `curl`:

```bash
curl -X POST \
  'https://your-api-endpoint/v1/messages/individual/{accountId}/send' \
  -H 'Content-Type: application/json' \
  -H 'X-API-Key: YOUR_API_KEY' \
  -H 'X-API-Secret: YOUR_API_SECRET' \
  -d '{
    "content": "Hello, world!",
    "attachment_uri": null,
    "from": "61421868490",
    "to": "61433174782",
    "service": "whatsapp"
  }'
```

Replace `YOUR_API_KEY`, `YOUR_API_SECRET`, and `{accountId}` with your actual API key, secret, and account ID.  Also, replace `https://your-api-endpoint` with the actual base URL of the API.

## Authentication

### API Keys

API keys are used to identify and authenticate your application when it makes requests to the API.  They are essential for security and usage tracking.

#### Obtaining API Keys

As mentioned in the introduction, API keys are currently **not self-service**. To obtain an API key and secret, please contact the founders directly.  Provide details about your intended use case and expected API usage.

We are actively developing a self-service portal for API key management.  This will allow you to:

*   Generate new API keys
*   Rotate existing API keys
*   Monitor API usage
*   Manage API access permissions

We will announce the availability of the self-service portal in the documentation and through other communication channels.

### Using `X-API-Key` and `X-API-Secret`

The API uses two headers for authentication:

*   **`X-API-Key`**:  This header contains your unique API key.  It identifies your application to the API.
*   **`X-API-Secret`**: This header contains a secret key associated with your API key.  It is used to verify the authenticity of your requests.  **Treat this secret like a password and keep it confidential.**

Both headers are required for every API request.  Failing to include these headers, or providing invalid values, will result in an authentication error (typically a `401 Unauthorized` error).

#### Example Headers

```
X-API-Key: your_api_key_here
X-API-Secret: your_api_secret_here
```

#### Security Best Practices

*   **Never expose your `X-API-Secret` in client-side code (e.g., JavaScript in a web browser).**  This could allow malicious users to access your API key and make unauthorized requests.
*   **Store your `X-API-Secret` securely.**  Use environment variables or a secure configuration management system.
*   **Rotate your API keys periodically.**  This reduces the risk of unauthorized access if your keys are compromised.  (This feature will be available in the future self-service portal).
*   **Monitor your API usage.**  Look for any unusual activity that might indicate a security breach.

#### Example Request (Python)

```python
import requests

api_key = "your_api_key_here"
api_secret = "your_api_secret_here"
account_id = "your_account_id"

url = f"https://your-api-endpoint/v1/messages/individual/{account_id}/send"

headers = {
    "X-API-Key": api_key,
    "X-API-Secret": api_secret,
    "Content-Type": "application/json"
}

data = {
    "content": "Hello from Python!",
    "attachment_uri": None,
    "from": "61421868490",
    "to": "61433174782",
    "service": "whatsapp"
}

response = requests.post(url, headers=headers, json=data)

if response.status_code == 200:
    print("Message sent successfully!")
    print(response.json())
else:
    print(f"Error sending message: {response.status_code} - {response.text}")
```

Remember to replace the placeholder values with your actual API key, secret, account ID, and the API endpoint.

### Future Authentication Methods

We are exploring other authentication methods to improve security and ease of use, including:

*   **OAuth 2.0:**  A standard protocol for delegated authorization.
*   **JWT (JSON Web Tokens):**  A compact and self-contained way to securely transmit information between parties as a JSON object.

We will announce any changes to the authentication methods in the documentation and through other communication channels.
