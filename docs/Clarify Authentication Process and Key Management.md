
# Authentication and Key Management

This document outlines the authentication process for accessing the API and provides guidance on managing your API keys and secrets securely.

## Authentication

All API endpoints require authentication using an API key and a secret key. These keys are passed in the header of each request.

**Header Parameters:**

*   `x-api-key`: Your API key.
*   `x-api-secret`: Your API secret.

**Example Request (cURL):**

```bash
curl -X POST \
  'https://your-api-endpoint/v1/messages/individual/{accountId}/send' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'x-api-secret: YOUR_API_SECRET' \
  -d '{
    "content": "Hello, world!",
    "attachment_uri": null,
    "from": "61421868490",
    "to": "61433174782",
    "service": "whatsapp"
  }'
```

**Example Request (Python):**

```python
import requests

url = "https://your-api-endpoint/v1/messages/individual/{accountId}/send"
headers = {
    "Content-Type": "application/json",
    "x-api-key": "YOUR_API_KEY",
    "x-api-secret": "YOUR_API_SECRET"
}
data = {
    "content": "Hello, world!",
    "attachment_uri": None,
    "from": "61421868490",
    "to": "61433174782",
    "service": "whatsapp"
}

response = requests.post(url, headers=headers, json=data)

print(response.status_code)
print(response.json())
```

**Note:** Replace `YOUR_API_KEY` and `YOUR_API_SECRET` with your actual API key and secret.  Also, replace `{accountId}` with the appropriate account ID.

## Getting Started

### Obtaining API Keys and Secrets

Currently, due to the Alpha status of the API, API keys and secrets are provided manually. Please contact the support team at `support@example.com` to request your API keys.

**Future Key Generation (Planned):**

In future releases, you will be able to generate API keys and secrets directly through the API or a dedicated management console.  This will involve the following steps:

1.  **Account Creation:** Create an account on the platform.
2.  **Project Creation:** Create a project within your account.  Each project can have its own set of API keys.
3.  **Key Generation:**  Navigate to the project settings and generate a new API key and secret.  You will be prompted to provide a description for the key.
4.  **Secure Storage:**  Store the API key and secret securely.  **Treat your secret key like a password.**

### Secure Key Management

It is crucial to manage your API keys and secrets securely to prevent unauthorized access to your account and data.

**Best Practices:**

*   **Never hardcode API keys directly into your application code.**  This is a major security risk.
*   **Use environment variables to store API keys.**  This allows you to configure your application without exposing the keys in your codebase.
*   **Restrict API key permissions.**  If possible, limit the scope of each API key to only the resources and actions it needs to access.  This feature is planned for future releases.
*   **Monitor API key usage.**  Regularly review your API key usage to detect any suspicious activity.  This feature is planned for future releases.
*   **Implement proper logging and auditing.**  Log all API requests and responses to help identify and investigate security incidents.
*   **Use a secrets management tool.**  Consider using a dedicated secrets management tool like HashiCorp Vault or AWS Secrets Manager to store and manage your API keys and secrets.

**Example: Using Environment Variables (Python):**

```python
import os
import requests

api_key = os.environ.get("API_KEY")
api_secret = os.environ.get("API_SECRET")
account_id = os.environ.get("ACCOUNT_ID")

url = f"https://your-api-endpoint/v1/messages/individual/{account_id}/send"
headers = {
    "Content-Type": "application/json",
    "x-api-key": api_key,
    "x-api-secret": api_secret
}
data = {
    "content": "Hello, world!",
    "attachment_uri": None,
    "from": "61421868490",
    "to": "61433174782",
    "service": "whatsapp"
}

response = requests.post(url, headers=headers, json=data)

print(response.status_code)
print(response.json())
```

**Setting Environment Variables (Example):**

```bash
export API_KEY="your_actual_api_key"
export API_SECRET="your_actual_api_secret"
export ACCOUNT_ID="your_account_id"
```

### Key Rotation (Planned)

Key rotation is the process of periodically generating new API keys and secrets and invalidating the old ones. This helps to reduce the risk of key compromise.

**Future Key Rotation Process:**

1.  **Generate a new API key and secret.**
2.  **Update your application to use the new API key and secret.**
3.  **Invalidate the old API key and secret.**  This will prevent them from being used to access the API.

We plan to provide tools and APIs to automate the key rotation process in future releases.  We will also provide notifications when key rotation is required.

**Important Considerations:**

*   **Plan your key rotation strategy carefully.**  Consider the impact on your application and ensure that you have a smooth transition process.
*   **Test your key rotation process thoroughly.**  Before invalidating old keys, verify that your application is working correctly with the new keys.
*   **Monitor your API key usage after key rotation.**  Ensure that all requests are being authenticated correctly.

### Related Endpoints

The following API endpoints require authentication using the `x-api-key` and `x-api-secret` headers:

*   `/v1/messages/individual/{accountId}/send`
*   `/v1/messages/group/{accountId}/send`
*   `/v1/messages/send/{accountId}`
*   `/v1/messages/individual/{accountId}/get-details/{messageId}`
*   `/v1/messages/threads/{accountId}/get-recent/{threadId}`
*   `/v1/messages/threads/{accountId}/get-details/{threadId}`
*   `/v1/messages/threads/{accountId}/get-all`
*   `/v1/messages/threads/{accountId}/get-all/{phone_number}`
*   `/v1/emails/{accountId}/create-email`
*   `/v1/emails/{accountId}/send`

### Error Handling

If authentication fails, the API will return a `401 Unauthorized` error.  Ensure that you are providing the correct API key and secret in the headers of your request.  Double-check for typos and ensure that the keys are properly encoded.
