# Telegram AI Assistant

An AI assistant that answers Telegram messages instantly, powered by Google Gemini and built as an [n8n](https://n8n.io) workflow.

## How it works

```
Telegram message  →  n8n (Telegram Trigger)  →  Google Gemini  →  n8n (Send a text message)  →  Reply in Telegram
```

1. **Telegram Trigger** receives every new message sent to the bot.
2. **Message a model** sends the message text to Google Gemini (`gemini-3.5-flash`).
3. **Send a text message** returns Gemini's answer to the same chat.

## Tech stack

- n8n (workflow automation, webhooks)
- Google Gemini API
- Telegram Bot API
- Docker (containerized deployment)
- Cloudflare Tunnel (secure public webhook endpoint)

## Setup

### 1. Prerequisites

- A Telegram bot token from [@BotFather](https://t.me/BotFather)
- A Google Gemini API key from [Google AI Studio](https://aistudio.google.com)
- A running n8n instance (for example with Docker)

### 2. Run n8n with Docker

```bash
docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n n8nio/n8n
```

### 3. Expose the webhook securely

Telegram needs a public HTTPS URL to deliver messages to n8n. A Cloudflare Tunnel provides one without opening ports on your server, and keeps the endpoint protected.

Set the public URL in n8n with the `WEBHOOK_URL` environment variable.

### 4. Import the workflow

1. In n8n, go to **Workflows → Import from file**.
2. Select `telegram-ai-assistant-workflow.json`.

### 5. Add your credentials

The workflow does not contain any secrets. Create these two credentials in n8n and select them in the nodes:

- **Telegram API**: your bot token
- **Google Gemini (PaLM) API**: your Gemini API key

### 6. Activate

Turn the workflow on, then send a message to your bot.

## Security notes

- Never commit API keys or bot tokens. n8n stores credentials separately from the exported workflow.
- Keep the webhook behind Cloudflare so it is not directly exposed.

## License

MIT
