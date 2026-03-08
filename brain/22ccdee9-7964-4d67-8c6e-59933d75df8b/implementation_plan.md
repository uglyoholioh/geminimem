# Telegram AI Interaction Implementation Plan

We'll implement a Telegram bot that allows users to interact with their CraftCanvas data (tasks, assignments, materials) using natural language.

## User Review Required

> [!IMPORTANT]
> To use the Telegram bot, users will need to link their account by providing a unique linking code generated in the CraftCanvas web app settings.

## Proposed Changes

### [Backend]

#### [MODIFY] [user.py](file:///Users/oli/Desktop/CraftCanvas/backend/models/user.py)
- Add `telegram_chat_id` (Optional[str]) and `telegram_username` (Optional[str]) fields.

#### [NEW] [telegram_service.py](file:///Users/oli/Desktop/CraftCanvas/backend/services/telegram_service.py)
- Use `python-telegram-bot` (or similar) for bot interaction.
- Handle `/start` to initiate linking.
- Implement a message handler that passes user queries to `ai_service.py`.
- Add specific commands for quick access: `/tasks`, `/assignments`, `/schedule`.

#### [MODIFY] [config.py](file:///Users/oli/Desktop/CraftCanvas/backend/config.py)
- Add `TELEGRAM_BOT_TOKEN` to environment variables and configuration.

#### [MODIFY] [settings.py](file:///Users/oli/Desktop/CraftCanvas/backend/routers/settings.py)
- Add an endpoint to generate/retrieve a Telegram linking code.

---

### [Frontend]

#### [MODIFY] [settings/page.tsx](file:///Users/oli/Desktop/CraftCanvas/frontend/app/settings/page.tsx)
- Add a "Telegram Integration" section.
- Display instructions and the linking code for the user.

## Verification Plan

### Automated Tests
- Mock Telegram API responses to verify bot command handlers.
- Test the user linking logic with mock chat IDs.

### Manual Verification
1. Open the Telegram bot and type `/start`.
2. Follow instructions to link account using the code from the web app.
3. Test commands `/tasks` and `/assignments`.
4. Ask the bot natural language questions like "What assignments are due next week?" or "Find my CS2103 lecture notes."
5. Test smart scheduling: "Schedule a 2-hour study block for my math quiz tomorrow."
