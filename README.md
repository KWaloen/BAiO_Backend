# How to run

Start App Command:
python manage.py runserver

Run Tests Command:
python manage.py test

Migrate Database:
python manage.py makemigrations
python manage.py migrate

# Backend API Documentation

This document outlines the endpoints available in the backend API, including request formats, HTTP methods, and the expected responses. Use this documentation to integrate the backend API with your frontend or other services.

## Base URL
The base URL for all endpoints is:
```
http://localhost:8000/
```

## User Endpoints

### 1. Get User Information
**URL:** `/user/getInfo/`  
**Method:** `GET`  
**Authentication:** Requires Bearer Token  
**Description:** Retrieves the user's information.  

**Request Example:**
```
GET /user/getInfo/
Authorization: Bearer %ACCESS_TOKEN%
```

**Response Example:**
```json
{
  "first_name": "Ole",
  "last_name": "Normann",
  "username": "olenor",
  "email": "olenorm@ole.no",
  "country": "Norway",
  "affiliation": "University of Bergen",
  "position": "scientist",
  "field_of_study": "AI researcher"
}
```

### 2. Update User Information
**URL:** `/user/updateInfo/`  
**Method:** `POST`  
**Authentication:** Requires Bearer Token  
**Description:** Updates the user's information.  

**Request Example:**
```json
POST /user/updateInfo/
{
  "first_name": "Ole",
  "last_name": "Normann",
  "country": "Norway",
  "affiliation": "University of Bergen",
  "position": "scientist",
  "field_of_study": "AI researcher"
}
```

**Response Example:**
```json
{
  "detail": "User information updated successfully."
}
```

### 3. Authenticate User
**URL:** `/user/auth/`  
**Method:** `POST`  
**Description:** Authenticates a user and provides an access token.  

**Request Example:**
```json
POST /user/auth/
{
  "username": "olenor",
  "password": "oleSterktPassord123"
}
```

**Response Example:**
```json
{
  "refresh": "%REFRESH_TOKEN%",
  "access": "%ACCESS_TOKEN%"
}
```

### 4. Register User
**URL:** `/user/register/`  
**Method:** `POST`  
**Description:** Registers a new user.  

**Request Example:**
```json
POST /user/register/
{
  "first_name": "Ole",
  "last_name": "Normann",
  "username": "olenor",
  "email": "olenorm@ole.no",
  "password": "oleSterktPassord123",
  "country": "Norway",
  "affiliation": "University of Bergen",
  "position": "scientist",
  "field_of_study": "AI researcher"
}
```

**Response Example:**
```json
{
  "detail": "User registered successfully."
}
```

## Chat Endpoints

### 1. Get a Conversation
**URL:** `/chat/getConversation/`  
**Method:** `GET`  
**Authentication:** Requires Bearer Token  
**Description:** Retrieves a specific conversation. The conversation id needs to be specified in the query parameters.

**Request Example:**
```
GET /chat/getConversation/?conversation_id=1
Authorization: Bearer %ACCESS_TOKEN%
```

**Response Example:**
```json
{
  "id": 1,
  "title": "Research Discussion",
  "messages": [
    {
      "role": "user",
      "content": "What are the key aspects of AI in genomics?",
      "created_at": "2024-10-15T10:00:00Z"
    },
    {
      "role": "assistant",
      "content": "Key aspects include predictive analysis, data integration, etc.",
      "created_at": "2024-10-15T10:01:00Z"
    }
  ]
}
```

### 2. Get All Conversations
**URL:** `/chat/getConversations/`  
**Method:** `GET`  
**Authentication:** Requires Bearer Token  
**Description:** Retrieves a list of all conversations for the authenticated user.

**Request Example:**
```
GET /chat/getConversations/
Authorization: Bearer %ACCESS_TOKEN%
```

**Response Example:**
```json
[
  {
    "id": 1,
    "title": "Research Discussion",
    "created_at": "2024-10-15T10:00:00Z"
  },
  {
    "id": 2,
    "title": "Project Meeting",
    "created_at": "2024-10-16T11:30:00Z"
  }
]
```

### 3. Rename a Conversation
**URL:** `/chat/renameConversation/`  
**Method:** `POST`  
**Authentication:** Requires Bearer Token  
**Description:** Renames a conversation.

**Request Example:**
```json
POST /chat/renameConversation/
{
  "conversation_id": 1,
  "new_title": "Updated Discussion"
}
```

**Response Example:**
```json
{
  "detail": "Conversation renamed successfully."
}
```

### 4. Create a Conversation
**URL:** `/chat/createConversation/`  
**Method:** `POST`  
**Authentication:** Requires Bearer Token  
**Description:** Creates a new conversation.

**Request Example:**
```json
POST /chat/createConversation/
{
  "title": "New Research Topic"
}
```

**Response Example:**
```json
{
  "id": 3,
  "title": "New Research Topic",
  "created_at": "2024-10-17T09:00:00Z"
}
```

### 5. Add an API Key
**URL:** `/chat/addAPIKey/`  
**Method:** `POST`  
**Authentication:** Requires Bearer Token  
**Description:** Adds an API key for the user.

**Request Example:**
```json
POST /chat/addAPIKey/
{
  "key": "%API_KEY%",
  "nickname": "OpenAI Key"
}
```

**Response Example:**
```json
{
  "detail": "API key added successfully."
}
```

### 6. Send a Message (Stream Response)
**URL:** `/chat/sendMessage/`  
**Method:** `POST`  
**Authentication:** Requires Bearer Token  
**Description:** Sends a message in a conversation and streams the response.

**Note:** This request cannot be tested via Postman due to streaming response. Use the `curl` command or the frontend to test.

**Request Example (using `curl`)**:
```bash
curl -X POST http://localhost:8000/chat/sendMessage/ \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer %ACCESS_TOKEN%" \
     -d '{"conversation_id": %CONVERSATION_ID%, "apikey_nickname": "%APIKEY_NICKNAME%", "content": "%TEXT_PROMPT%", "model": "%LLM_MODEL%"}'
```

**Response Example:**
The response will be streamed in chunks. The content will include messages generated by the AI model in response to the user's prompt.

---

