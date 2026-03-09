# LLM API Rotator: API Endpoints & Programmatic Access Report

This report provides a detailed guide on how to integrate and use your local LLM API rotation service.

## 1. Connection Details

The service runs locally and mimics the OpenAI API standard, making it compatible with almost any LLM client.

- **Base URL:** `http://localhost:8000/v1`
- **Default Port:** `8000`
- **Auth Key:** Not required (you can pass any dummy string like `sk-xxxx`).

## 2. Available Models

The service rotates through your underlying keys whenever one hits a rate limit. All Groq models rotate across 11 API keys; Gemini models rotate across 5 keys.

| Model Name | Provider | Underlying Model | Speed | Best Used For |
| :--- | :--- | :--- | :--- | :--- |
| `groq-llama` | Groq | Llama 3.3 70B | 280 tps | Flagship chat & coding |
| `groq-llama-small` | Groq | Llama 3.1 8B | 560 tps | Ultra-fast lightweight tasks |
| `groq-scout` | Groq | Llama 4 Scout 17B | 750 tps | Fast multimodal (vision + text) |
| `groq-gpt-oss` | Groq | GPT-OSS 120B | 500 tps | Best reasoning, flagship open |
| `groq-gpt-oss-mini` | Groq | GPT-OSS 20B | 1000 tps | Fastest, good quality |
| `groq-qwen` | Groq | Qwen3 32B | 400 tps | Multilingual & reasoning |
| `groq-kimi` | Groq | Kimi K2 0905 | 200 tps | 262K context, agentic coding |
| `gemini-flash` | Gemini | Gemini 2.0 Flash | — | Vision & long context |
| `gemini-image` | Gemini | Imagen 3 | — | Image generation |

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
