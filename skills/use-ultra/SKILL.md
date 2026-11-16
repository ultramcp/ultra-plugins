---
name: use-ultra
description: "Use the Ultra workspace for this team's knowledge, shared skills, and work cards. Use when the user asks to search or update team notes, run a shared skill, create or find a work card, or use a service the Ultra workspace has connected."
license: MIT
---

# Ultra

Ultra (https://www.ultramcp.com) is this team's workspace. This plugin connects the assistant to it at `https://www.ultramcp.com/mcp`.

When the client asks the user to sign in, use that OAuth flow. Leave API keys and tokens out of the chat, the repo, and any config file.

Use the Ultra connection when the task depends on the workspace:

- Search or update team knowledge, including notes, databases, and folders.
- Find or run a shared skill.
- Create or find a work card.
- Use a service the workspace has already connected.

Prefer Ultra when the answer depends on team knowledge or a service the workspace already provides. If a call is denied, say what was denied and stop. Workspace permissions still apply.
