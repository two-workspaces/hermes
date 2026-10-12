# Deploy and Host Hermes Agent on Railway

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/new/template/hermes-agent-dashboard?referralCode=sLjDXb&utm_medium=integration&utm_source=button&utm_campaign=hermes-agent-dashboard)

[Hermes Agent](https://github.com/NousResearch/hermes-agent) is Nous Research's self-improving AI agent. It remembers you across sessions, writes its own skills, runs scheduled jobs and talks to you on Telegram, Discord, Slack and more. This template runs the official image with the password-protected web dashboard, the in-browser chat and the messaging gateway, with all agent state stored on a volume.

## About Hosting Hermes Agent

One service, one volume at `/opt/data`. The official `nousresearch/hermes-agent` image runs under s6 with two supervised processes. The **gateway** handles messaging platforms and the cron scheduler. The **web dashboard** gives you chat, sessions, models, API keys, skills, cron, logs and config from a browser. The dashboard is bound publicly and sits behind a username/password login. The password is generated for you on deploy. Memory, skills, sessions, config and API keys all live on the volume, so they survive redeploys and image upgrades.

## Common Use Cases

- A personal assistant on Telegram that remembers context and runs tasks while you are away
- Scheduled agent jobs: daily briefings, feed or price monitoring, reports delivered to chat
- A browser-accessible agent with terminal, web and headless-browser tools, without running anything locally
- Trying Hermes with any model via OpenRouter, Nous Portal, Anthropic, OpenAI and other providers

## Dependencies for Hermes Agent Hosting

- An LLM provider: an OpenRouter key, another provider's API key, or a Nous Portal login
- Optional: a Telegram bot token from [@BotFather](https://t.me/BotFather) (or Discord/Slack credentials)

### Deployment Dependencies

- [Hermes Agent on GitHub](https://github.com/NousResearch/hermes-agent)
- [Hermes Agent docs](https://hermes-agent.nousresearch.com/docs/)
- [OpenRouter](https://openrouter.ai/keys) for API keys

### Implementation Details

**First login:** open your Railway domain. Sign in as `admin` with `HERMES_DASHBOARD_BASIC_AUTH_PASSWORD` from the service's Variables tab.

**Connect a model:** do one of the following:

- Set `OPENROUTER_API_KEY` at deploy time.
- Add a key on the dashboard's **Keys** page.
- Type `/login` in the **Chat** tab to use Nous Portal.

Then open **Models** and pick a model. The default is Claude Opus 4.6, which is capable but expensive. Choose a cheaper model for everyday use.

**Telegram:** set `TELEGRAM_BOT_TOKEN`. Put your numeric Telegram user ID in `TELEGRAM_ALLOWED_USERS` (comma-separated for several). Anyone not on the list gets a pairing code instead of access, which you can approve on the **Pairing** page. Discord, Slack and other channels are configured on the **Channels** and **Keys** pages.

**Upgrading:** change the image tag in the service's Settings → Source (e.g. `nousresearch/hermes-agent:v2026.9.24` → a newer tag) and redeploy. The volume keeps your data.

Notes and limits:

- 1 GB of RAM covers the dashboard, gateway and chat (about 400 MB idle, 650 MB chatting). Give it 2 GB or more if the agent uses the headless browser tool a lot. `MALLOC_ARENA_MAX=2` is preset to keep Python's memory in check on small plans.
- The agent's terminal tool runs commands inside the container. The Docker terminal backend is not available on Railway.
- The dashboard password protects an agent that can run shell commands. Keep it strong and rotate it by changing the variable and redeploying.

## Why Deploy Hermes Agent on Railway?

Railway is a singular platform to deploy your infrastructure stack. Railway will host your infrastructure so you don't have to deal with configuration, while allowing you to vertically and horizontally scale it.

By deploying Hermes Agent on Railway, you are one step closer to supporting a complete full-stack application with minimal burden. Host your servers, databases, AI agents, and more on Railway.
