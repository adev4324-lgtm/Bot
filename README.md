# Yama Discord Bot (standalone)

AI chat (main-chat channel, web search, link reading), AI moderation, captcha verification panel and staff commands.

## Run on Railway
1. Push this folder to a GitHub repo, then Railway → New Project → Deploy from GitHub repo.
2. Variables tab: set `DISCORD_TOKEN` and `OPENROUTER_API_KEY`.
   (`firebase-applet-config.json` is already included, or set `FIREBASE_CONFIG` to its JSON instead.)
3. Start command is `npm start` (already set in `railway.json`).
4. **Run only ONE copy of the bot** (stop Render/Vercel/etc). Two copies = double replies. The code has guards, but one copy is the safe setup.

## Discord developer portal
Bot → Privileged Gateway Intents: turn on **Server Members** and **Message Content**.

## Bot permissions
View Channels, Send Messages, Embed Links, Attach Files, Read Message History, Manage Messages,
Manage Roles, Manage Channels, Manage Nicknames, Moderate Members, Ban Members, Kick Members.
Put the bot's role **above** any role it must give or manage (verified role, members it mutes).

## First-time setup (in Discord, staff only)
- `?vcrole @role`, `?vpd <text>`, `?verifypanel #channel`  → verification
- `?aichat #channel` → main AI chat   |   `?aimod on`, `?modlog #channel` → AI moderation
- `?aichat on|off` → AI chat on/off (moderation stays live) · `?aimod links on|off`, `?aimod spam on|off`, `?linkallow add <domain>`
- `?help` shows every command

## Games (everyone)
- `start chess` or `?chess` → pick **vs AI** or **PvP** (both players confirm). A match channel is created that everyone can watch but only the two players can type in. Type moves like `e2e4`, `e4`, `Nf3`, `O-O` or `resign`.
- `start coinflip` or `?coinflip` → same lobby flow, best of 3.
- The bot needs **Manage Channels**. If the bot restarts mid-game, delete the leftover `chess-...` / `coinflip-...` channel by hand.

## Notes
- The AI only answers when you @mention it, reply to it, or call it by name at the start of a message.
- Reading images/videos/PDFs and making images use OpenRouter vision/image models (`VISION_MODEL`, `IMAGE_MODEL`). Change them if a model name stops working.

## Local run
```
npm install
cp .env.example .env   # fill it in
npm start
```

## Server Manager (`?sm`)
- `?sm on` / `?sm off` (**server owner only**) turns Server Manager on or off.
- While it is on, staff (the staff role and the staff user IDs) can ask the bot for any server task in plain words, for example "rename #general to #lobby", "move the Gamers role above Members", "make a category Events and put #raids in it", "announce the update in #news".
- Before anything runs, the bot checks every command: whether it has the Discord permission it needs, whether the owner-only rules allow it, and whether the details are complete. Impossible commands are reported and skipped. The rest run.
- Regular members who ask the bot to change the server get a **DM warning** and nothing is changed.
- Bulk requests such as "ban everyone" are still refused unless the owner asks.
- Only the server owner can grant the Administrator permission.
- Reading: text/code files up to 3 MB (long files are condensed in parts), images up to 8 MB, videos up to 25 MB, PDFs up to 15 MB.
- GIFs: post a GIF link, or ask for a GIF by description. Searching by description needs `TENOR_API_KEY` (free Tenor key) in the environment.

**Security:** set `OPENROUTER_API_KEY` in the environment. The source no longer contains a fallback key. If an old copy of this repo was public, rotate that key on OpenRouter.

### Auto mode (`?sm auto`)
- `?sm on` first, then `?sm auto on [minutes]` (default 60, minimum 15). The AI reviews the server on that timer and makes small organising fixes on its own.
- Allowed without anyone asking: create categories and channels, rename, move and reorder channels and categories, reorder roles, set topics, create roles, change role colours and names.
- Never: delete anything, ban/kick/mute, touch members, change permissions, or edit powerful or staff roles.
- At most 5 changes per pass. Every pass is logged to `?sm log #channel` (or sent to the owner's DMs if no log channel is set).
- `?sm auto off` stops it. `?sm off` turns Server Manager off entirely.
