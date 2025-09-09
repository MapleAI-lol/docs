# MapleAI API Documentation

MapleAI provides a collection of AI-powered endpoints for text, images, audio, embeddings, and moderation. All requests must be authenticated with a Bearer token.

**Base URL**

```
https://api.mapleai.de/v1
```

---

## Authentication

All requests require an API key passed in the `Authorization` header:

```
Authorization: Bearer sk-mapleai-xxxxxxxx
```

---

## Models

List and inspect available models.

**List Models**

```
GET /v1/models
```

---

## Chat Completions

Interact with AI models in a conversational format.

**Endpoint**

```
POST /v1/chat/completions
```

**Body Parameters**

| Parameter  | Type    | Required | Description                    |
| ---------- | ------- | -------- | ------------------------------ |
| `model`    | string  | Yes      | The model ID to use            |
| `messages` | array   | Yes      | Conversation history           |
| `stream`   | boolean | No       | Stream responses incrementally |

**Example**

```bash
curl https://api.mapleai.de/v1/chat/completions \
  -H "Authorization: Bearer sk-mapleai-xxxxxxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o-mini",
    "messages": [{"role":"user","content":"Hello!"}],
    "stream": false
  }'
```

---

## Images

### Image Generation

Generate new images from text prompts.

**Endpoint**

```
POST /v1/images/generations
```

**Body Parameters**

| Parameter | Type    | Required | Description                      |
| --------- | ------- | -------- | -------------------------------- |
| `model`   | string  | Yes      | Model ID (e.g., `dall-e-3`)      |
| `prompt`  | string  | Yes      | Text description of the image    |
| `size`    | string  | No       | Image size (`1024x1024` default) |
| `n`       | integer | No       | Number of images (default: 1)    |

### Image Edits

Edit or extend existing images.

**Endpoint**

```
POST /v1/images/edits
```

**Body Parameters**

| Parameter | Type    | Required | Description                                                |
| --------- | ------- | -------- | ---------------------------------------------------------- |
| `model`   | string  | Yes      | Model ID (e.g., `dall-e-2`)                                |
| `prompt`  | string  | Yes      | Description of the desired edit                            |
| `image`   | file    | Yes      | Base image to edit (`jpeg`, `png`, or `webp`)              |
| `mask`    | file    | No       | Optional mask image where transparent areas indicate edits |
| `size`    | string  | No       | Image size (`1024x1024` default)                           |
| `n`       | integer | No       | Number of edited images to generate (default: 1)           |

---

## Responses API (Unified)

A single endpoint that supports both chat-style and instruction-style responses.

**Endpoint**

```
POST /v1/responses
```

**Example Request**

```bash
curl https://api.mapleai.de/v1/responses \
  -H "Authorization: Bearer sk-mapleai-xxxxxxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "o3-pro",
    "input": [
      {"role": "user", "content": "Write a short bedtime story about a fox."}
    ],
    "stream": true
  }'
```

---

## Embeddings

Convert text into vector representations.

**Endpoint**

```
POST /v1/embeddings
```

**Body Parameters**

| Parameter         | Type         | Required | Description                                   |
| ----------------- | ------------ | -------- | --------------------------------------------- |
| `model`           | string       | Yes      | Embedding model ID (e.g., `text-embedding-3-large`) |
| `input`           | string/array | Yes      | Input text or array of texts                  |
| `encoding_format` | string       | No       | `float` (default) or `base64`                 |

---

## Audio

### Text-to-Speech

Generate lifelike speech from text.

**Endpoint**

```
POST /v1/audio/speech
```

**Body Parameters**

| Parameter         | Type   | Required | Description                                  |
| ----------------- | ------ | -------- | -------------------------------------------- |
| `model`           | string | Yes      | TTS model ID (e.g., `gpt-4o-mini-tts`)       |
| `input`           | string | Yes      | Text to synthesize                           |
| `voice`           | string | Yes      | Voice ID                                     |
| `response_format` | string | No       | Output format: `mp3` (default), `wav`, `ogg` |

### Transcriptions

Convert audio into text.

**Endpoint**

```
POST /v1/audio/transcriptions
```

**Body Parameters**

| Parameter | Type   | Required | Description                     |
| --------- | ------ | -------- | ------------------------------- |
| `model`   | string | Yes      | Transcription model ID          |
| `file`    | file   | Yes      | Audio file (`mp3`, `wav`, etc.) |

### Translations

Translate spoken audio into English.

**Endpoint**

```
POST /v1/audio/translations
```

**Body Parameters**

| Parameter | Type   | Required | Description                     |
| --------- | ------ | -------- | ------------------------------- |
| `model`   | string | Yes      | Translation model ID            |
| `file`    | file   | Yes      | Audio file (`mp3`, `wav`, etc.) |

---

## Moderation

Check whether content complies with policy.

**Endpoint**

```
POST /v1/moderations
```

**Body Parameters**

| Parameter | Type   | Required | Description         |
| --------- | ------ | -------- | ------------------- |
| `model`   | string | Yes      | Moderation model ID |
| `input`   | string | Yes      | Text to evaluate    |

---

## Key Info

Retrieve metadata about your API key.

**Endpoint**

```
GET /v1/key-info
POST /v1/key-info
```

**Response Example**

```json
{
  "username": "demoUser",
  "rpm": 60,
  "rpm_used": 12,
  "rpd": 5000,
  "rpd_used": 123,
  "banned": false,
  "ban_reason": null,
  "ban_expires": null,
  "plan": "Pro",
  "admin": false
}
```

---

## Usage History

View request usage for the past 7 days.

**Endpoint**

```
GET /v1/usage-history
POST /v1/usage-history
```

**Response Example**

```json
{
  "labels": ["2025-09-03", "2025-09-04", "2025-09-05", "2025-09-06", "2025-09-07", "2025-09-08", "2025-09-09"],
  "data": [23, 45, 30, 12, 40, 51, 20]
}
```
