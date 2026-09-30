# Pigeon

A Telegram-style messenger in cool browns and beiges. Free to run: no accounts, keys or paid services.

## Run it

```
npm install
npm run dev
```

Open http://localhost:3000 and sign up. `npm run dev` restarts the server whenever you change its code (`npm start` doesn't).

## Chat with friends

- **Same Wi-Fi:** friends open `http://<your-PC's-IP>:3000` (find your IP with `ipconfig`, "IPv4 Address"). Allow Node through the Windows firewall on private networks.
- **Anywhere, free:** install [cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/) and run `cloudflared tunnel --url http://localhost:3000`. It prints a public `https://….trycloudflare.com` link. Your PC must stay on; the link changes on each restart.

The microphone (voice messages, calls), location sharing and the passcode lock need `localhost` or an `https` link — they won't work over plain `http://<your-IP>`.

## Install on a phone (no App Store)

Open the https link on the phone: **iPhone (Safari):** Share → Add to Home Screen. **Android (Chrome):** menu → Install app.

## What's in it

- **Chats:** 1:1 chats, group chats, Saved Messages; pin, archive, mute (1h / 8h / 2 days / forever), mark as unread, clear history or delete chat (for you or both), folders, drafts.
- **Messages:** reply, edit, delete (for you or everyone), forward, reactions, pinned messages, select multiple, search in chat, jump to unread/mentions, @mentions, links, big emoji, emoji picker, export chat history (HTML/JSON), auto-delete timers.
- **Media:** photos & videos (with optional compression), albums, files up to 50 MB, voice messages (with transcripts), round video messages, location, full-screen viewer, shared media pages.
- **Sending:** messages appear instantly with a clock, and are sent automatically when the connection comes back (even after closing the tab). Right-click or hold the send button to **send with an effect** (confetti, hearts, fireworks, stars, balloons), **without sound**, or **schedule** it. Swipe a message left to reply on phones. Link previews. "Seen by" in groups (right-click your message).
- **Polls & quizzes:** attach menu → Poll. Anonymous or public, one or several answers, or a quiz with one right answer and an explanation.
- **Stories:** photos or videos for 24 hours, in the bar above the chat list. Choose who sees them (contacts, or everyone you chat with), see who viewed and reacted, and reply privately.
- **Photo editor:** in the send dialog, the brush button on a photo: crop (free, square, 4:3, 16:9), rotate, flip, draw, add text, blur parts.
- **Voice messages:** keep playing while you scroll or switch chats (bar at the top), 1.5x / 2x speed, and the next one plays by itself.
- **Picture-in-picture:** videos (viewer or right-click a video message) and calls can pop out into a floating window.
- **Command palette:** Ctrl+P (or Ctrl+K outside the message box) — jump to any chat or run an action by typing.
- **Big chats stay fast:** only about 250 messages are on screen at a time; more load as you scroll.
- **Motion:** new messages slide in, deleted ones crumble into dust, pages glide, the chat list reorders smoothly, and on phones the wallpaper moves a little as you tilt.
- **Voice & video calls:** camera on/off mid-call, screen sharing, front/back camera on phones, minimize to keep chatting, call history.
- **Profiles:** photo, name, username, bio, birthday, name color; contacts with custom names; last seen; groups in common.
- **Privacy & security:** who can see last seen / photo / bio / birthday, who can call, message or add you to groups; blocking; two-step verification; active sessions; local passcode; delete account.
- **Look:** Day / Night (or match the system), accent colors, chat wallpapers, interface scale.
- **Translation:** uses the browser's built-in on-device translator when available (recent Chrome); hidden otherwise.

## Text formatting

Select text to get a formatting toolbar, right-click the message box (**Formatting**), use the shortcuts, or type the markers yourself. Shift + right-click opens the browser's own menu, which has spelling suggestions.

| Style | Type | Shortcut |
| --- | --- | --- |
| **Bold** | `**text**` | Ctrl+B |
| *Italic* | `__text__` | Ctrl+I |
| <u>Underline</u> | `++text++` | Ctrl+U |
| ~~Strikethrough~~ | `~~text~~` | Ctrl+Shift+X |
| `Monospace` | `` `text` `` (or ```` ``` ```` for a block) | Ctrl+Shift+M |
| Quote | `> ` at the start of each line | Ctrl+Shift+. |
| Link | `[text](https://address)` | Ctrl+K |
| Clear formatting | | Ctrl+Shift+N |
| Spoiler | `\|\|text\|\|` | Ctrl+Shift+P |

## Notifications when Pigeon is closed

**Settings → Notifications and Sounds → Notify when Pigeon is closed.** The browser must allow notifications, and Pigeon must be opened over `https://` or `localhost` (browsers only allow push there). It works for every account saved on the device. The keys are created on first run in `data/vapid.json`.

## Group invite links

Group admins: open the group's info → **Invite via link**. Anyone who opens the link (`…/#join=…`) sees the group and can join. **Reset link** stops the old one working.

## Backups

The server makes a snapshot of the database every day (the first one a minute after it starts) and keeps the last 7, and keeps copies of all uploaded files and profile photos. They go to `data/backups/` by default. Since this folder is inside OneDrive, OneDrive already copies them to the cloud. To use another folder (e.g. an external drive), set it in `config.json`:

```json
{ "backupDir": "D:/PigeonBackups" }
```

To restore, stop the server, copy the snapshot you want (e.g. `pigeon-2026-09-29.db`) to `data/pigeon.db`, and copy the `uploads` and `avatars` folders from the backup folder back into `data/`.

## Secret chats

Open someone's chat menu (⋮) → **Start secret chat**. Secret chats are **end-to-end encrypted**: the two browsers agree on a key (ECDH P-256 → AES-256-GCM) and the server only ever stores and passes on ciphertext — including photos, files, voice and video messages, which are encrypted before uploading. Other differences, like Telegram:

- A secret chat works **only on the device where it was started or accepted** (the key never leaves that browser).
- A **self-destruct timer** (5 seconds to 1 week) starts once a message has been read, and deletes it for both of you.
- No forwarding, pinning, scheduling or server-side search. Deleting the chat deletes it for both people.
- **Encryption key** (in the chat menu or profile): if the picture looks the same on both devices, nobody is in the middle.

## Group video chats

In a group, press the video-chat button in the header (or **Video chat** in the group info). Everyone in the group sees a **Join** bar. Up to 8 people; everyone connects directly to everyone else, so it works best with a few people on good connections. Camera, screen sharing, mute, pop-out and minimize work like in 1:1 calls; whoever is talking gets a green glow.

## Security

- **Login alerts:** the **Pigeon** service chat (with a check badge) tells you about every new login, password and two-step verification changes, and repeated wrong passwords. It can't receive messages.
- **Password guessing is slowed down:** after 5 wrong passwords an account waits 15 minutes (20 per network address). Sign-ups, uploads and message floods are rate-limited too.

## Voice & video calls

Calls go directly between the two devices, using Google's free STUN servers to find each other. That works on most home Wi-Fi. Some networks — most mobile data, school/office/hotel Wi-Fi, some routers — block direct connections; those calls need a **TURN relay** that passes the audio/video along.

### Turning on the free Cloudflare relay (5 minutes)

Cloudflare's TURN service is free for the first 1,000 GB each month (roughly 1,000+ hours of video calls). No card needed.

1. Create a free account at https://dash.cloudflare.com/sign-up and log in.
2. In the left menu open **Realtime** (sometimes listed as *Calls*), then **TURN Server**, and click **Create**. Give it any name, e.g. `pigeon`.
3. Cloudflare shows a **Turn Token ID** and an **API Token**. Copy both (the API token is shown only once).
4. In the Pigeon folder, copy `config.example.json` to `config.json` and paste them in:
   ```json
   { "cloudflareTurn": { "keyId": "your Turn Token ID", "apiToken": "your API Token" } }
   ```
5. That's it — no restart needed. In Pigeon, **Settings → Speakers and Camera** should now say **Call relay: On**.

Calls still try a direct connection first and only use the relay when they have to. Any other TURN server works too: `{ "turn": [{ "urls": "turn:host:3478", "username": "...", "credential": "..." }] }`. `config.json` holds secrets and is ignored by git.

## Accounts, phone numbers & QR codes

- **Multiple accounts:** ☰ menu → **Add Account** (up to 3). Switch from the menu; the others keep getting notifications and unread counts in the background.
- **Phone numbers:** add yours at sign-up or in **Settings → My Account**. You can log in with it and people can find you by it; **Privacy and Security** controls who sees it and who can find you. Numbers aren't verified by SMS — sending verification texts costs money with every provider.
- **QR codes:** your code is in **Settings** and **My Profile** (the QR icon); anyone's code is next to their username. **Contacts → Scan QR code** opens someone's profile from their code (camera, or pick a picture of one). Codes contain a link like `https://your-pigeon-address/#@username`.

## Where things are

- `server.js` — starts the web server
- `server/db.js` — SQLite database (built into Node) in `data/pigeon.db`; `data/uploads/` and `data/avatars/` hold files. Set `DATA_DIR` to use a different folder.
- `server/store.js` — all the rules: privacy, chats, messages, groups
- `server/routes.js` — login/sign-up, uploads, photos
- `server/socket.js` — real-time events and call signaling
- `server/ice.js` — which STUN/TURN servers calls use (reads `config.json`)
- `server/groupcalls.js` — group video chat signaling; `server/stories.js` — stories; `server/limits.js` — rate limits
- `server/push.js` — notifications to closed apps; `server/preview.js` — link previews; `server/backup.js` — daily backups
- `public/js/` — the app (chat list, chat view, composer, profiles, settings…), `public/style.css` — its look
#   P i g e o n  
 