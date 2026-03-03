# LLM API Rotator: API Endpoints & Programmatic Access Report

This report provides a detailed guide on how to integrate and use your local LLM API rotation service.

## 1. Connection Details

The service runs locally and mimics the OpenAI API standard, making it compatible with almost any LLM client.

- **Base URL:** `http://localhost:8000/v1`
- **Default Port:** `8000`
- **Auth Key:** Not required (you can pass any dummy string like `sk-xxxx`).

## 2. Available Models

The service rotates through your underlying keys whenever one hits a rate limit.

| Model Name | Provider | Best Used For | Features |
| :--- | :--- | :--- | :--- |
| `groq-llama` | Groq | Lightning-fast Chat / Coding | Multi-key rotation |
| `gemini-flash` | Gemini | Vision (Image Analysis) / Long Context | Free-tier rotation |
| `gemini-image` | Gemini | Image Generation (Imagen 3) | Imagen 3 Integration |

## 3. Programmatic Access Examples

### Python (using OpenAI library)
```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:8000/v1",
    api_key="sk-local"
)

# Chat with Groq (Fast)
response = client.chat.completions.create(
    model="groq-llama",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

### Curl (CLI)
```bash
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "groq-llama",
    "messages": [{"role": "user", "content": "How does key rotation work?"}]
  }'
```

---
*Generated Documentation Template*
