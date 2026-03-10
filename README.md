# LLM API Rotator 🚀

A robust, local LLM API gateway that rotates through multiple free-tier API keys (Groq, Gemini, Scaleway, Mistral & OpenRouter) to bypass rate limits. It provides a single OpenAI-compatible endpoint that you can use with tools like **Open WebUI**, **AnythingLLM**, **ONLYOFFICE**, and custom scripts.

## 🌟 Features

- **Automatic Key Rotation**: Seamlessly switches between multiple API keys per provider.
- **Multi-Provider Support**: Groq, Gemini, Scaleway, Mistral, and OpenRouter — all behind one endpoint.
- **Rate Limit Handling**: Automatically retries 429 (Too Many Requests) errors with the next available key.
- **Universal Compatibility**: Mimics the OpenAI API standard (`/v1/chat/completions`).
- **50 Virtual Models**: Access models from 5 providers through a single gateway.
- **Streaming Support**: Real-time text generation compatible with modern UIs.
- **Multimodal Support**: Vision models (Pixtral, Scout, Gemini Flash).
- **Image Generation**: Integrated support for Google's **Imagen 3 / Imagen-4** models.
- **Persistence**: Runs as a background `systemd` service.

## 🛠️ How it Works

The setup uses [LiteLLM](https://github.com/BerriAI/litellm) as a proxy and router. When a request comes in, LiteLLM's `simple-shuffle` strategy picks an API key. If that key is rate-limited, it is placed in a 60-second "cooldown," and the request is immediately retried with a fresh key.

## 🚀 Quick Setup

### 1. Prerequisites
- Linux (optimized for Ryzen 7 environments).D84B-FDF8
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

## 📊 Available Virtual Models (50 models across 5 providers)

### Groq (17 keys rotating)
| Virtual Model | Underlying Engine | Speed | Capability |
| :--- | :--- | :--- | :--- |
| `groq-llama` | Llama 3.3 70B | 280 tps | Flagship chat & coding |
| `groq-llama-small` | Llama 3.1 8B | 560 tps | Ultra-fast lightweight tasks |
| `groq-scout` | Llama 4 Scout 17B | 750 tps | Fast multimodal (vision + text) |
| `groq-gpt-oss` | GPT-OSS 120B | 500 tps | Best reasoning |
| `groq-gpt-oss-mini` | GPT-OSS 20B | 1000 tps | Fastest, good quality |
| `groq-qwen` | Qwen3 32B | 400 tps | Multilingual & reasoning |
| `groq-kimi` | Kimi K2 0905 | 200 tps | 262K context, agentic coding |

### Gemini (5 keys rotating)
| Virtual Model | Underlying Engine | Capability |
| :--- | :--- | :--- |
| `gemini-flash` | Gemini 2.0 Flash | Vision & Long Context |
| `gemini-image` | Imagen 3 / Nano Banana | Image Generation |

### Scaleway (21 keys rotating)
| Virtual Model | Underlying Engine | Capability |
| :--- | :--- | :--- |
| `scw-qwen-235b` | Qwen3 235B | Largest open model |
| `scw-gpt-oss` | GPT-OSS 120B | Reasoning |
| `scw-llama-70b` | Llama 3.3 70B | Chat & coding |
| `scw-llama-8b` | Llama 3.1 8B | Fast lightweight |
| `scw-deepseek-r1` | DeepSeek R1 Distill 70B | Reasoning (CoT) |
| `scw-devstral` | Devstral 2 123B | Coding agent |
| `scw-gemma` | Gemma 3 27B | Compact & capable |
| `scw-holo` | Holo2 30B | Creative writing |
| `scw-mistral-nemo` | Mistral Nemo 12B | Lightweight Mistral |
| `scw-mistral-small` | Mistral Small 3.2 24B | Balanced Mistral |
| `scw-pixtral` | Pixtral 12B | Vision model |
| `scw-qwen-coder` | Qwen3 Coder 30B | Code generation |

### Mistral (2 keys rotating)
| Virtual Model | Underlying Engine | Capability |
| :--- | :--- | :--- |
| `mistral-large` | Mistral Large | Flagship reasoning |
| `mistral-medium` | Mistral Medium | Balanced quality |
| `mistral-small` | Mistral Small | Fast & efficient |
| `mistral-codestral` | Codestral | Code generation |
| `mistral-devstral` | Devstral | Coding agent |
| `mistral-devstral-medium` | Devstral Medium | Coding (balanced) |
| `mistral-devstral-small` | Devstral Small | Coding (fast) |
| `mistral-magistral-med` | Magistral Medium | Reasoning |
| `mistral-magistral-sm` | Magistral Small | Reasoning (fast) |
| `mistral-ministral-3b` | Ministral 3B | Ultra-lightweight |
| `mistral-ministral-8b` | Ministral 8B | Lightweight |
| `mistral-ministral-14b` | Ministral 14B | Mid-range |
| `mistral-pixtral` | Pixtral Large | Vision (large) |
| `mistral-nemo` | Mistral Nemo | Open-weight |
| `mistral-tiny` | Mistral Tiny | Fastest Mistral |

### OpenRouter Free (11 keys rotating)
| Virtual Model | Underlying Engine | Capability |
| :--- | :--- | :--- |
| `or-llama-70b` | Llama 3.3 70B | Chat & coding |
| `or-gpt-oss` | GPT-OSS 120B | Reasoning |
| `or-gpt-oss-mini` | GPT-OSS 20B | Fast reasoning |
| `or-qwen-coder` | Qwen3 Coder | Code generation |
| `or-qwen-next` | Qwen3 Next 80B | Latest Qwen |
| `or-gemma-27b` | Gemma 3 27B | Compact & capable |
| `or-gemma-12b` | Gemma 3 12B | Lightweight |
| `or-mistral-small` | Mistral Small 3.1 | Fast Mistral |
| `or-hermes-405b` | Hermes 3 405B | Largest open model |
| `or-nemotron-30b` | Nemotron 3 Nano 30B | NVIDIA reasoning |
| `or-nemotron-9b` | Nemotron Nano 9B | NVIDIA lightweight |
| `or-step-flash` | Step 3.5 Flash | 256K context |
| `or-glm` | GLM 4.5 Air | Chinese + English |
| `or-trinity` | Trinity Large | Arcee hybrid |

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
