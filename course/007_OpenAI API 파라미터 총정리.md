
---
# OpenAI API 파라미터 총정리 (Text / Chat / Embeddings)
---

## 1. 🔥 Chat Completion / Text Generation API

(Endpoint 예: `POST /v1/chat/completions`)

### ### 주요 파라미터 정리

| 파라미터                  | 타입              | 설명                                               |          |                             |
| --------------------- | --------------- | ------------------------------------------------ | -------- | --------------------------- |
| **model**             | string          | 사용할 모델 이름                                        |          |                             |
| **messages**          | array           | 대화 기록 배열. `{ role: "user"                        | "system" | "assistant", content: "" }` |
| **temperature**       | number          | 창의성 조절 (0~2). 낮으면 결정적, 높으면 랜덤                    |          |                             |
| **top_p**             | number          | Nucleus Sampling. temperature 대신 사용 가능           |          |                             |
| **max_tokens**        | number          | 생성할 최대 토큰                                        |          |                             |
| **presence_penalty**  | number          | 새로운 주제를 더 많이 도입하도록 유도 (-2 ~ 2)                   |          |                             |
| **frequency_penalty** | number          | 반복 억제 (-2 ~ 2)                                   |          |                             |
| **stop**              | string or array | 생성 중단 시킬 문자열                                     |          |                             |
| **stream**            | boolean         | 스트리밍 응답을 받을지 여부                                  |          |                             |
| **n**                 | number          | 몇 개의 응답을 생성할지                                    |          |                             |
| **logprobs**          | number          | 토큰별 확률 출력 (일부 모델만 지원)                            |          |                             |
| **response_format**   | object          | 예: JSON 형태 강제 (`{ "type": "json_schema", ... }`) |          |                             |
| **tools**             | array           | 함수 호출 정의                                         |          |                             |
| **tool_choice**       | string/object   | 사용할 tool 지정                                      |          |                             |
| **metadata**          | object          | 요청에 임의 메타데이터 추가                                  |          |                             |

---

## 📌 Chat Completion 요청 예시 (JSON)

### **1) 기본 텍스트 생성 예시**

```json
POST /v1/chat/completions
{
  "model": "gpt-4.1-mini",
  "messages": [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Explain quantum computing in simple terms"}
  ],
  "temperature": 0.7,
  "max_tokens": 300
}
```

---

### **2) JSON 형태로 강제 응답 받기**

```json
{
  "model": "gpt-4.1",
  "messages": [
    {"role": "user", "content": "Give me movie information about Matrix"}
  ],
  "response_format": {
    "type": "json_schema",
    "json_schema": {
      "name": "movie_info",
      "schema": {
        "type": "object",
        "properties": {
          "title": {"type": "string"},
          "year": {"type": "number"},
          "genres": {"type": "array", "items": {"type": "string"}}
        },
        "required": ["title", "year"]
      }
    }
  }
}
```

---

### **3) 스트리밍 응답 예시**

```json
{
  "model": "gpt-4.1-mini",
  "messages": [
    {"role": "user", "content": "Write a poem about stars."}
  ],
  "stream": true
}
```

---

### **4) 함수 호출 / Tool 호출 예시**

```json
{
  "model": "gpt-4.1",
  "messages": [
    {"role": "user", "content": "What's the weather in Seoul?"}
  ],
  "tools": [
    {
      "name": "getWeather",
      "description": "Get current weather info",
      "input_schema": {
        "type": "object",
        "properties": {
          "city": { "type": "string" }
        },
        "required": ["city"]
      }
    }
  ],
  "tool_choice": "getWeather"
}
```

---

---

## 2. 🧠 Embeddings API

(Endpoint: `POST /v1/embeddings`)

### 주요 파라미터

| 파라미터                | 타입            | 설명                                           |
| ------------------- | ------------- | -------------------------------------------- |
| **model**           | string        | Embedding 모델 이름 (`text-embedding-3-large` 등) |
| **input**           | string, array | 임베딩할 텍스트 (여러 개 넣을 수 있음)                      |
| **encoding_format** | string        | `"float"`(기본), `"base64"` 가능                 |
| **dimensions**      | number        | 출력 벡터 차원 수 조절 (일부 모델만)                       |
| **metadata**        | object        | 요청 메타데이터                                     |

---

### Embeddings 생성 요청 예시

#### **1) 기본 Embedding 요청**

```json
POST /v1/embeddings
{
  "model": "text-embedding-3-large",
  "input": "OpenAI provides powerful models for developers."
}
```

---

#### **2) 여러 문장을 한번에 임베딩 요청**

```json
{
  "model": "text-embedding-3-small",
  "input": [
    "Hello world",
    "Machine learning is amazing",
    "OpenAI embeddings help search"
  ]
}
```

---

#### **3) Base64 형식으로 임베딩 반환**

```json
{
  "model": "text-embedding-3-large",
  "input": "Convert embedding output to base64",
  "encoding_format": "base64"
}
```

---

### 응답 구조 예시

```json
{
  "data": [
    {
      "embedding": [...],
      "index": 0
    }
  ],
  "model": "text-embedding-3-large",
  "usage": {
    "prompt_tokens": 12,
    "total_tokens": 12
  }
}
```

---

# 📌 전체 정리

## ☑ Text/Chat Completion에서 자주 쓰는 핵심 변수

* `model`
* `messages`
* `temperature`
* `max_tokens`
* `top_p`
* `presence_penalty`
* `frequency_penalty`
* `stop`
* `stream`
* `response_format`

## ☑ Embeddings에서 자주 쓰는 핵심 변수

* `model`
* `input`
* `encoding_format`
* `dimensions`

---

필요하시면 **각 요청을 Java/Spring**, **Node.js**, **Python**, **cURL** 예제로도 만들어 드릴까요?
