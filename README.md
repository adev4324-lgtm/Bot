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
- Reading images/videos/PDFs and making images use OpenRouter vision/image models (`VISION_MODEL`, `IMAGE_MODEL`).

## Local run
```
npm install
cp .env.example .env   # fill it in
npm start
```

## Server Manager (owner only)
- The server owner can give the bot any server task in plain words, for example "rename all channels beautifully", "rename #general to #lobby", "move the Gamers role above Members", "make a category Events and put #raids in it". The bot does it without arguing; it only reports what it can't do because of Discord permissions.
- Only the owner gets commands. Anyone else who asks gets a refusal and nothing changes.
- Bulk requests such as "ban everyone" are for the owner only, and run.
- Up to 60 commands per owner message. For bigger jobs, ask again for the rest.
- Reading: text/code files up to 3 MB (long files are condensed in parts), images up to 8 MB, videos up to 25 MB, PDFs up to 15 MB.
- GIFs: post a GIF link, or ask for a GIF by description. Searching by description needs `TENOR_API_KEY` (free Tenor key) in the environment.

**Security:** set `OPENROUTER_API_KEY` in the environment. The source no longer contains a fallback key. If an old copy of this repo was public, rotate that key on OpenRouter.

## Welcome and tickets (owner/staff)
- `?welcome #channel` · `?welcome off` — where new members are welcomed.
- `?wm set <text>` — welcome text. Placeholders: `{user}` `{name}` `{server}` `{count}`. `?wm` (no text) sends a preview to the welcome channel.
- `?em <image link>` · `?em off` — image on the welcome embed.
- `?ticketbuttons Label 1 | Label 2` (up to 5) · `?ticketrole @role` · `?ticketpanel #channel`. Clicking a button opens a private ticket channel that pings the user. The AI tries to solve it. If it can't, the ticket role is pinged. Anyone can close their own ticket with **Close ticket**.
- AI commands: "create voice channel ...", "create forum channel ...", "give everyone the @X role" / "give role All Users to everyone".

