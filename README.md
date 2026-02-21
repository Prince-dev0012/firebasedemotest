# Firebase ENVAPI Integration

This example demonstrates how to use the ENVAPI Worker to proxy requests to your Firebase project (`envapi-demo-b49d`).

## 1. Configure the Admin Dashboard

You cannot simply paste the Firebase SDK configuration into the Admin dashboard. Instead, you must configure a **REST API Proxy** that points to the Firebase REST endpoints.

Copy and paste one of the following JSON configurations into the **"Advanced JSON Configuration"** field in your Admin Dashboard.

### Option A: Firestore (Recommended)
Use this if you are using Cloud Firestore.

```json
{
  "provider": "custom",
  "endpoint": "https://firestore.googleapis.com/v1/projects/envapi-demo-b49d/databases/(default)/documents/messages?key={{apiKey}}",
  "method": "POST",
  "headers": {
    "Content-Type": "application/json"
  },
  "apiKey": "AIzaSyDvoJ0p9EHynU-lhOtz0B8qnqXPDGYnTww"
}
```
*   **Note:** This creates documents in a collection named `messages`. You can change `messages` to any collection name you prefer.
*   **Security:** Ensure your Firestore Security Rules allow writes, or use a Service Account key instead of the public API key if you need server-side privileges.

### Option C: Read Data (Firestore GET)
Use this configuration to read documents from the `messages` collection.

```json
{
  "provider": "custom",
  "endpoint": "https://firestore.googleapis.com/v1/projects/envapi-demo-b49d/databases/(default)/documents/messages",
  "method": "GET",
  "headers": {},
  "apiKey": ""
}
```
*   **Note:** This endpoint is public if your security rules allow it. You can also append `?key=YOUR_API_KEY` to the endpoint if needed, but often read access is controlled by rules. If you need the API key, add `?key={{apiKey}}` and set the `apiKey` field.

## 2. Update Client Code

The `index.html` file has been updated to work with the **Firestore** configuration above.

### Key Changes:
1.  **Project Key:** Use your Project Key from the Admin Dashboard.
2.  **API Name:** Use the name you gave to the API config above (e.g., `firebase-firestore`).
3.  **Payload:** Firestore requires a specific JSON format (`fields` wrapper). The updated client code handles this automatically for simple objects!

## 3. Testing

1.  Open `index.html` in your browser.
2.  Enter your **Worker URL** and **Project Key**.
3.  Enter the **API Name** (e.g., `firebase-firestore`).
4.  Enter a simple JSON payload (e.g., `{"text": "Hello World", "user": "test"}`).
5.  Click **Send Request**.
6.  Check your Firebase Console to see the new document!
