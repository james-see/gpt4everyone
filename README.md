# GPT4Everyone

Desktop chat app for running and chatting with large language models (LLMs) locally or via OpenAI-compatible APIs. Built on [GPT4All](https://github.com/nomic-ai/gpt4all).

## Downloads

Installers are published on [GitHub Releases](https://github.com/james-see/gpt4everyone/releases). Download the latest for your OS:

- **macOS** — `gpt4everyone-installer-darwin.dmg` (Monterey 12.6+, Apple Silicon or Intel)
- **Windows** — `gpt4everyone-installer-win64.exe` (or ARM64 build if available)
- **Linux** — `gpt4everyone-installer-linux.run` or `.tar.gz` (x86-64)

Drag the app to **Applications** (macOS) or run the installer (Windows/Linux). No extra path setup required.

## Using Ollama with GPT4Everyone

You can use [Ollama](https://ollama.com) (local or cloud) as the model backend so you don’t have to download GGUF files in the app.

### 1. Get an API key (recommended)

- For **Ollama Cloud**: sign up at [ollama.com/signup](https://ollama.com/signup) and create a free API key under [Settings → Keys](https://ollama.com/settings/keys).
- For **local Ollama only**: you can leave the API key blank; the app will talk to `http://localhost:11434` without authentication.

Using a free Ollama API key is recommended so the app can work with both local and cloud models.

### 2. Install and run Ollama (local use)

```bash
# macOS / Linux
curl -fsSL https://ollama.com/install.sh | sh

# Then pull a model, e.g.:
ollama pull llama3.2
ollama pull glm-4-flash
```

Keep Ollama running in the background (or start it before opening GPT4Everyone).

### 3. Add Ollama as a remote in GPT4Everyone

1. Open **GPT4Everyone** → **Settings** (gear) → **Models**.
2. Go to the **Remote** (or **Add Model** → Remote) section.
3. Add a **Custom** remote:
   - **Base URL**: `http://localhost:11434/v1` (local) or your Ollama Cloud endpoint if using cloud.
   - **API key**: your [Ollama API key](https://ollama.com/settings/keys), or leave blank for local only.
4. Save; the model list should refresh. Select the model you pulled (e.g. `llama3.2`, `glm-4-flash`) and start a chat.

### Example (local Ollama)

- Base URL: `http://localhost:11434/v1`
- API key: *(leave empty for local)*
- Model: `llama3.2` or any model you’ve run `ollama pull <name>` for.

## Local models and LocalDocs

GPT4Everyone also runs **local** GGUF models (download from the in-app model list or install your own) and supports **LocalDocs** (chat over your documents with embeddings). See in-app settings for model path and LocalDocs collections.

## System requirements

- **macOS**: Monterey 12.6 or newer (Apple Silicon or Intel).
- **Windows**: Intel Core i3 2nd Gen / AMD Bulldozer or better; ARM64 build for Snapdragon/SQ.
- **Linux**: x86-64; see [gpt4all-chat/system_requirements.md](gpt4all-chat/system_requirements.md) for details.

## Changelog

See [gpt4all-chat/CHANGELOG.md](gpt4all-chat/CHANGELOG.md) for version history. Recent highlights:

- **v1.0.0** — First GPT4Everyone release: rebranded app (GPT4Everyone.app), macOS/Windows/Linux installers via GitHub Actions, Ollama/custom remote support, LocalDocs fixes and embedding error handling.

## License and attribution

GPT4Everyone is a fork of [GPT4All](https://github.com/nomic-ai/gpt4all) by Nomic. See repository and [GPT4All documentation](https://docs.gpt4all.io) for upstream features and licensing.
