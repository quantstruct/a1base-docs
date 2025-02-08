
# AI Triage Logic in Next.js Template

This document explains the AI triage logic implemented within the `handleWhatsAppIncoming` function of the Next.js template. This logic is responsible for processing incoming WhatsApp messages, determining the user's intent, and selecting the appropriate workflow to respond. Understanding this logic is crucial for customizing the agent's behavior and extending its capabilities with new workflows.

**Target Audience:** Developers, AI Engineers

**Related Endpoint:** `/api/whatsapp/incoming`

## AI Logic

The `handleWhatsAppIncoming` function acts as the entry point for processing incoming WhatsApp messages.  It receives the message content and metadata from the `/api/whatsapp/incoming` endpoint. The core of the AI triage logic involves the following steps:

1.  **Message Reception and Preprocessing:**
    *   The function receives the incoming message data, typically in JSON format.
    *   It extracts relevant information such as the sender's phone number, message text, and timestamp.
    *   Preprocessing steps might include cleaning the message text (e.g., removing special characters, converting to lowercase) to improve the accuracy of intent recognition.

2.  **Intent Determination:**
    *   This is the most critical step. The template uses an AI model (e.g., a pre-trained NLP model or a custom-trained model) to analyze the message and determine the user's intent.
    *   The intent recognition process typically involves:
        *   **Feature Extraction:** Converting the message text into a numerical representation that the AI model can understand.  This might involve techniques like word embeddings (e.g., Word2Vec, GloVe) or TF-IDF.
        *   **Intent Classification:** Using the AI model to classify the message into one of the predefined intents.  The model outputs a probability score for each possible intent.
        *   **Intent Selection:** Choosing the intent with the highest probability score, provided it exceeds a certain confidence threshold.  If no intent meets the threshold, a fallback intent (e.g., "unclear_intent") is selected.

3.  **Workflow Selection:**
    *   Based on the determined intent, the function selects the appropriate workflow to execute.
    *   A mapping between intents and workflows is typically defined in a configuration file or database.
    *   The selected workflow is then invoked to handle the user's request.

Here's a simplified code snippet illustrating the core logic:

```javascript
async function handleWhatsAppIncoming(data) {
  const messageText = data.content;
  const senderNumber = data.sender_number;

  // 1. Preprocess the message (example: lowercase)
  const processedMessage = messageText.toLowerCase();

  // 2. Determine the intent using an AI model (replace with actual AI model integration)
  const intent = await determineIntent(processedMessage); // Assume determineIntent is an async function

  // 3. Select the appropriate workflow based on the intent
  switch (intent) {
    case "simple_response":
      await handleSimpleResponse(senderNumber, messageText);
      break;
    case "email_action":
      await handleEmailAction(senderNumber, messageText);
      break;
    case "task_confirmation":
      await handleTaskConfirmation(senderNumber, messageText);
      break;
    default:
      await handleUnclearIntent(senderNumber);
      break;
  }
}

async function determineIntent(message) {
  // Placeholder for AI model integration
  // In a real implementation, this function would use an NLP model
  // to classify the intent of the message.
  // Example:
  // const result = await nlpModel.predict(message);
  // return result.intent;

  // For demonstration purposes, let's use a simple keyword-based approach
  if (message.includes("hello") || message.includes("hi")) {
    return "simple_response";
  } else if (message.includes("email") || message.includes("send mail")) {
    return "email_action";
  } else if (message.includes("task") || message.includes("confirm")) {
    return "task_confirmation";
  } else {
    return "unclear_intent";
  }
}
```

**Important Considerations:**

*   **AI Model Integration:** The `determineIntent` function is a placeholder.  You'll need to integrate a real AI model for intent recognition.  Consider using services like Dialogflow, Rasa, or LlamaIndex.
*   **Confidence Threshold:**  Implement a confidence threshold for intent selection to avoid misinterpreting user requests.
*   **Error Handling:**  Include robust error handling to gracefully handle unexpected errors during intent recognition or workflow execution.
*   **Context Management:**  For more complex interactions, consider implementing context management to track the conversation history and provide more personalized responses.

## Workflows

The Next.js template supports various workflows to handle different types of user requests. Here's a description of the available workflows:

*   **Simple Response:** This workflow provides a basic response to the user's message.  It's typically used for greetings, acknowledgments, or answering simple questions.

    ```javascript
    async function handleSimpleResponse(senderNumber, messageText) {
      const responseMessage = `Hello! You said: ${messageText}`;
      // Send the response message back to the user using the messaging API
      await sendMessage(senderNumber, responseMessage); // Assume sendMessage function exists
    }
    ```

*   **Email Action:** This workflow allows the agent to send emails on behalf of the user.  It requires the user to provide the recipient's email address, subject, and body.

    ```javascript
    async function handleEmailAction(senderNumber, messageText) {
      // Extract email details from the message text (e.g., using regular expressions or NLP)
      const { recipient, subject, body } = extractEmailDetails(messageText);

      // Send the email using the email API
      const emailResult = await sendEmail(recipient, subject, body); // Assume sendEmail function exists

      // Send a confirmation message to the user
      if (emailResult.success) {
        await sendMessage(senderNumber, "Email sent successfully!");
      } else {
        await sendMessage(senderNumber, "Failed to send email.");
      }
    }
    ```

*   **Task Confirmation:** This workflow is used to confirm tasks or appointments with the user.  It typically involves retrieving task details from a database and presenting them to the user for confirmation.

    ```javascript
    async function handleTaskConfirmation(senderNumber, messageText) {
      // Extract task ID from the message text
      const taskId = extractTaskId(messageText);

      // Retrieve task details from the database
      const task = await getTaskDetails(taskId); // Assume getTaskDetails function exists

      // Present the task details to the user for confirmation
      const confirmationMessage = `Please confirm the following task: ${task.description} on ${task.dueDate}. Reply with 'yes' to confirm or 'no' to cancel.`;
      await sendMessage(senderNumber, confirmationMessage);

      // (In a real implementation, you would need to handle the user's confirmation response)
    }
    ```

*   **Unclear Intent:** This workflow is triggered when the AI model cannot determine the user's intent.  It typically involves asking the user to rephrase their request or providing a list of available options.

    ```javascript
    async function handleUnclearIntent(senderNumber) {
      const responseMessage = "I'm sorry, I didn't understand your request. Please try rephrasing it or choose from the following options:\n1. Send an email\n2. Confirm a task\n3. Ask a question";
      await sendMessage(senderNumber, responseMessage);
    }
    ```

**Adding New Workflows:**

To add a new workflow, you need to:

1.  Define a new intent in your AI model.
2.  Create a new function to handle the workflow logic.
3.  Add a new case to the `switch` statement in the `handleWhatsAppIncoming` function to map the new intent to the new workflow function.

**Example Flowchart:**

```mermaid
graph LR
    A[Incoming WhatsApp Message] --> B{Message Preprocessing};
    B --> C[Intent Determination (AI Model)];
    C --> D{Intent Identified?};
    D -- Yes --> E[Workflow Selection];
    D -- No --> F[Unclear Intent Workflow];
    E --> G{Simple Response Workflow};
    E --> H{Email Action Workflow};
    E --> I{Task Confirmation Workflow};
    G --> J[Send Response];
    H --> K[Send Email];
    I --> L[Confirm Task];
    F --> M[Ask User to Rephrase];
    J --> N[End];
    K --> N;
    L --> N;
    M --> N;
```

This flowchart visually represents the flow of the AI triage logic, from receiving the message to executing the appropriate workflow.

By understanding the AI triage logic and the available workflows, you can effectively customize the Next.js template to meet your specific needs and create a powerful and intelligent agent.
