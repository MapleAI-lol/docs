# MapleAI API Documentation

Welcome to the **MapleAI API**! Our goal is to provide a powerful, fast, and easy-to-use interface for integrating state-of-the-art AI models into your applications.  

With our API, you get:
- Drop-in compatibility with OpenAI API – use your existing code and SDKs
- Access to the latest AI models from multiple providers
- Competitive pricing and flexible rate limits
- Enterprise-grade reliability and support

---

## Getting Started

### 1. Get Your API Key
Generate your API key in the [MapleAI Dashboard](https://mapleai.de/dashboard).

### 2. Make Your First Request
Include your API key in the `Authorization` header:

```http
Authorization: Bearer YOUR_API_KEY
````

### 3. Base URL

All API requests should be made to:

```
https://api.mapleai.lol
```

---

## Endpoints

### List Models

Retrieve a list of all available models compatible with the API.

**Endpoint**

```
GET /v1/models
```

**Example Request**

```bash
curl https://api.mapleai.de/v1/models \
  -H "Authorization: Bearer sk-mapleai-xxxxxxxx"
```

---

### Chat Completions

Create AI chat completions for natural conversations, creative writing, code assistance, and more. Supports both standard responses and streaming for real-time interactions.

**Endpoint**

```
POST /v1/chat/completions
```

**Key Features**

* Stream responses in real-time
* Control response creativity with temperature
* Function calling capabilities
* JSON mode for structured outputs

**Body Parameters**

| Parameter     | Type    | Required | Description                                        |
| ------------- | ------- | -------- | -------------------------------------------------- |
| `model`       | string  | Yes      | Model ID (e.g., `gpt-4o`, `claude-3`)              |
| `messages`    | array   | Yes      | Array of message objects with `role` and `content` |
| `stream`      | boolean | No       | Enable streaming responses (`false` by default)    |
| `temperature` | number  | No       | Controls randomness (0-2, default: 1)              |
| `max_tokens`  | integer | No       | Maximum tokens in response                         |

**Example Request**

```bash
curl https://api.mapleai.de/v1/chat/completions \
  -H "Authorization: Bearer sk-mapleai-xxxxxxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o",
    "messages": [
      { "role": "system", "content": "You are a helpful assistant." },
      { "role": "user", "content": "Hello! Who are you?" }
    ]
  }'
```

---

### Image Generation

Transform your text descriptions into stunning images using state-of-the-art AI models. Perfect for creative projects, marketing materials, and visual content generation.

**Endpoint**

```
POST /v1/images/generations
```

**Key Features**

* High-resolution image generation
* Multiple images per request
* HD quality option
* Flexible size options

**Body Parameters**

| Parameter | Type    | Required | Description                                                 |
| --------- | ------- | -------- | ----------------------------------------------------------- |
| `model`   | string  | Yes      | Model ID (e.g., `dall-e-3`)                                 |
| `prompt`  | string  | Yes      | Detailed description of the desired image                   |
| `size`    | string  | No       | Image size: `1024x1024` (default), `1024x1792`, `1792x1024` |
| `quality` | string  | No       | `standard` (default) or `hd` quality                        |
| `n`       | integer | No       | Number of images to generate (default: 1)                   |

**Example Response**

```json
{
  "created": 1702505298,
  "data": [
    {
      "url": "https://example.com/..."
    }
  ]
}
```

---

### Content Moderation

Keep your application safe by detecting potentially harmful content. Our moderation API helps identify inappropriate, harmful, or unsafe content in text.

**Endpoint**

```
POST /v1/moderations
```

**Key Features**

* Fast and accurate content screening
* Multiple categories of harmful content detection
* Confidence scores for each category
* Batch processing support

**Body Parameters**

| Parameter | Type         | Required | Description                               |
| --------- | ------------ | -------- | ----------------------------------------- |
| `model`   | string       | Yes      | Model ID (e.g., `text-moderation-latest`) |
| `input`   | string/array | Yes      | Text to analyze or array of texts         |

**Example Response**

```json
{
  "id": "modr-xxxxx",
  "model": "text-moderation-latest",
  "results": [
    {
      "flagged": false,
      "categories": {
        "hate": false,
        "threatening": false,
        "self-harm": false,
        "sexual": false,
        "violence": false
      },
      "category_scores": {
        "hate": 0.001,
        "threatening": 0.003,
        "self-harm": 0.000,
        "sexual": 0.002,
        "violence": 0.001
      }
    }
  ]
}
```
