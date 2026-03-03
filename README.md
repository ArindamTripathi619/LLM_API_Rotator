# LLM API Rotator 🚀

A robust, local LLM API gateway that rotates through multiple free-tier API keys (Groq & Gemini) to bypass rate limits. It provides a single OpenAI-compatible endpoint that you can use with tools like **Open WebUI**, **AnythingLLM**, **ONLYOFFICE**, and custom scripts.

## 🌟 Features

- **Automatic Key Rotation**: Seamlessly switches between 10+ Groq and Gemini keys.
- **Rate Limit Handling**: Automatically retries 429 (Too Many Requests) errors with the next available key.
- **Universal Compatibility**: Mimics the OpenAI API standard (`/v1/chat/completions`).
- **Streaming Support**: Real-time text generation compatible with modern UIs.
- **Multimodal Support**: Use Gemini models for image analysis (Vision).
- **Image Generation**: Integrated support for Google's **Imagen 3 / Imagen-4** models.
- **Persistence**: Runs as a background `systemd` service.

## 🛠️ How it Works

The setup uses [LiteLLM](https://github.com/BerriAI/litellm) as a proxy and router. When a request comes in, LiteLLM's `simple-shuffle` strategy picks an API key. If that key is rate-limited, it is placed in a 60-second "cooldown," and the request is immediately retried with a fresh key.

## 🚀 Quick Setup

### 1. Prerequisites
- Linux (optimized for Ryzen 7 environments).
- Python 3.10+
- `pip install litellm[proxy] python-dotenv PyYAML`

### 2. Configure Keys
Create a `config.yaml` based on the provided `config.example.yaml`. Add as many Groq and Gemini keys as you have.

### 3. Run the Proxy
```bash
litellm --config config.yaml --port 8000 --host 0.0.0.0
```

### 4. Setup as a System Service (Optional)
To make the rotator run automatically in the background:
1. Copy `llm-rotator.service` to `/etc/systemd/system/`.
2. Update the paths in the file to match your project location.
3. Run:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable llm-rotator.service
   sudo systemctl start llm-rotator.service
   ```

## 📊 Available Virtual Models

| Virtual Model | Underlying Engine | Capability |
| :--- | :--- | :--- |
| `groq-llama` | Groq Llama 3.3 70B | Lightning-fast Chat & Coding |
| `gemini-flash` | Gemini 2.0 Flash | Vision & Long Context |
| `gemini-image` | Imagen 3 / Nano Banana | Image Generation |

## 🔗 Integration Examples

### ONLYOFFICE / Open WebUI
- **Provider**: OpenAI
- **Base URL**: `http://localhost:8000/v1`
- **API Key**: `anything` (local auth not enabled by default)

### Python
```python
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1", api_key="sk-local")

response = client.chat.completions.create(
    model="groq-llama",
    messages=[{"role": "user", "content": "Hello!"}]
)
```

## 📜 Project Origin
This project was developed to maximize the utility of free-tier LLM API keys by treating them as a single, combined resource pool. It handles the complexity of cooldowns, retries, and model mapping automatically.

---
*Created by DevCrewX & Anti-Gravity Agent*
