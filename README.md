# Twitch Chat Overlay

A single-file Twitch chat overlay for an OBS browser source. It shows 7TV, BTTV and FFZ emotes, plus 7TV paints (name colors) and 7TV badges.

It connects to chat anonymously, so no login or token is needed. There's nothing to install.

## OBS setup

There are two ways to load the overlay in OBS. Both use a **Browser** source.

| | Option A: URL | Option B: Local file |
|---|---|---|
| Where settings live | In the URL (`?channel=…&size=…`) | In the `DEFAULTS` block inside `chat.html` |
| Change a setting | Edit the URL in OBS | Edit `chat.html`, then refresh the source |
| Several overlays (channels or styles) from one file | Yes, one URL each | No; copy the file once per setup |

Either way, set the source's width/height to about `450 × 900`, or whatever fits your layout. The background is transparent, so no custom CSS is needed.

### Option A: URL with options (recommended)

1. Add a **Browser** source.
2. **Untick "Local file"**.
3. Set **URL** to the file's path in `file:///` form, with your options added after `?`:

   **Linux / macOS**
   ```
   file:///home/slendog/Projects/twitch_chat/chat.html?channel=YOURCHANNEL
   ```

   **Windows**
   ```
   file:///C:/Users/YourName/Documents/twitch_chat/chat.html?channel=YOURCHANNEL
   ```

   Windows path rules:
   - Use forward slashes `/`, not backslashes `\`.
   - Start with `file:///` (three slashes), then the drive letter.
   - Write any space in the path as `%20`, e.g. `C:/Users/John%20Smith/…`.
   - To get the path, right-click `chat.html` in Explorer → **Copy as path**, then swap `\` for `/` and remove the quotes.

### Option B: Local file + edit the defaults in the file

In "Local file" mode, OBS opens the file directly and you can't add `?channel=…` options. All settings come from the `DEFAULTS` block in `chat.html` instead.

1. Open `chat.html` in a text editor (Notepad, VS Code, Kate, …).
2. Find `const DEFAULTS = {` near the top of the `<script>` section and change the values you need. At minimum, set `channel`:
   ```js
   const DEFAULTS = {
     channel: 'yourchannel',   // ← your Twitch channel name
     size: 24,                 // font size in px
     emoteScale: 1.6,
     max: 40,
     fade: 30,                 // fade messages after 30s (0 = never)
     badges: true,
     paints: true,
     hideCommands: true,
     notices: true,
     gifs: true,
     gifHeight: 150,
     prefetch: true,
     loader: true,
     hideBots: 'nightbot,streamelements,streamlabs,moobot,fossabot,wizebot,sery_bot,soundalerts,kofistreambot',
     font: '',                 // e.g. 'Comic Sans MS'
   };
   ```
   - Keep text values in quotes (`'yourchannel'`).
   - Use `true`/`false` for on/off options, and plain numbers for sizes and times.
   - Keep the comma at the end of each line.
3. Save the file.
4. In OBS, add a **Browser** source, **tick "Local file"**, click **Browse** and pick `chat.html`.
5. After any later edit to `chat.html`, open the source's properties and click **"Refresh cache of current page"**, or untick and re-tick the source's visibility.

Tips:
- **More than one setup** (different channels or looks): make a copy per setup, e.g. `chat-xqc.html` and `chat-mine.html`, each with its own `DEFAULTS`.
- **Keep your settings when updating:** if you replace `chat.html` with a newer version, copy your `DEFAULTS` block over first, or it goes back to the stock values.
- **Both modes at once:** URL options still override `DEFAULTS` in Option A. You can put your usual settings in `DEFAULTS` and only add `?channel=…` in the URL.

## Options

Add options to the URL with `&` (Option A). Each one can also be set in the `DEFAULTS` block at the top of the script in `chat.html` (Option B; the names are the same).

| Option         | Default      | Description |
|----------------|--------------|-------------|
| `channel`      | *(required)* | Twitch channel login, e.g. `xqc` |
| `size`         | `20`         | Font size in px |
| `emoteScale`   | `1.6`        | Emote height relative to the font size |
| `max`          | `40`         | Max messages kept on screen; the oldest is removed first |
| `fade`         | `0`          | Seconds before a message fades out. `0` = never |
| `badges`       | `1`          | Show Twitch badges (sub, mod, VIP, …) |
| `paints`       | `1`          | Show 7TV paints and 7TV badges |
| `hideCommands` | `1`          | Hide messages starting with `!` |
| `notices`      | `1`          | Show sub, resub, gift and raid notices |
| `hideBots`     | *(see below)*| Extra usernames to hide, comma-separated |
| `font`         | `Inter`      | Any CSS font-family, e.g. `Arial`, `Comic Sans MS` |
| `gifs`         | `1`          | Show Twitch chat GIFs (GIPHY). `0` shows the `[Title GIF by …]` text instead |
| `gifHeight`    | `150`        | GIF height in px |
| `prefetch`     | `1`          | Download all emote images at startup so they show instantly when first used |
| `loader`       | `1`          | Show the thin loading bar at the top while emotes load |

**On/off options:** `0`, `false`, `no` or `off` turns one off. Any other value turns it on.

**Hidden bots:** nightbot, streamelements, streamlabs, moobot, fossabot, wizebot, sery_bot, soundalerts and kofistreambot are hidden by default. Bots listed on the channel's BTTV page are also hidden automatically. `hideBots` adds names to this list. To unhide a default bot, remove it from `DEFAULTS.hideBots`.

**Spaces in values** must be written as `%20`, e.g. `font=Comic%20Sans%20MS`.

### Examples

```
chat.html?channel=xqc
chat.html?channel=xqc&size=26&fade=30&max=20
chat.html?channel=xqc&badges=0&notices=0&hideBots=mybot,otherbot
chat.html?channel=xqc&font=Comic%20Sans%20MS&emoteScale=2
chat.html?channel=xqc&loader=0
```

## Features

- **Emotes:** Twitch, 7TV, BTTV and FFZ, both channel and global. If two emotes share a name, channel emotes win over global ones, and 7TV wins over BTTV, which wins over FFZ.
- **Overlay emotes:** 7TV, BTTV and FFZ emotes that stack on top of the previous emote are stacked.
- **FFZ modifiers:** typed after an emote, they change it, and the modifier word itself is hidden. They work on any emote (Twitch, 7TV, BTTV or FFZ), and several can be combined, e.g. `Kappa ffzW ffzX`.
  - For everyone: `ffzW` (wide), `ffzX` (flip horizontally), `ffzY` (flip vertically), `ffzCursed`.
  - FFZ supporters only: `ffzHyper`, `ffzRainbow`, `ffzSpin`, `ffzSlide`, `ffzArrive`, `ffzLeave`, `ffzJam`, `ffzBounce`. When a non-supporter types one of these after an emote, the word is hidden and the emote isn't changed, same as in FFZ.
  - A modifier typed on its own, with no emote before it, shows as a normal emote.
- **Live emote updates:** channel emote changes reach chat without reloading the overlay. See [Live emote updates](#live-emote-updates).
- **Twitch chat GIFs:** GIFs that Tier 2 and Tier 3 subscribers send are shown as images below the message text, at `gifHeight` px high. The `[Title GIF by …]` text is replaced by the GIF.
- **7TV personal emotes:** 7TV subscribers' personal emotes work, but only in their own messages. 7TV announces who owns which personal set live, through its EventAPI. If the set arrives after someone's first message, their messages on screen are redrawn with the emotes. A personal emote wins over a channel emote with the same name.
- **7TV paints and 7TV badges** for each chatter.
- **Twitch badges**, loaded from [ivr.fi](https://api.ivr.fi), because Twitch's own API needs a token.
- **Moderation:** deleted messages, timeouts, bans and `/clear` are removed from the overlay.
- **Other:** `/me` messages, highlighted @mentions, and very dark name colors brightened so they stay readable.

## Loading and caching

- **Parallel start:** all emote lists, badges and chat start loading at once. The page looks up the channel ID right away instead of waiting for chat to connect.
- **Cached lists:** emote lists are saved in the browser's localStorage. On the next start, emotes work almost immediately. Fresh lists are still fetched in the background every time.
- **Waiting for emotes:** chat messages that arrive while emote lists are loading are held back for up to 8s, so they don't show up as plain text.
- **Prefetch:** with `prefetch=1`, every emote image is downloaded once in the background. Chromium's disk cache keeps them, so later starts don't download them again.
- **Loading bar:** while all this happens, a 3px bar and small label at the top show progress. They fade out about 2s after everything is loaded.

Measured on a channel with about 1,100 emotes:

| Start | Chat ready with emotes | All images loaded |
|-------|------------------------|-------------------|
| First run (nothing cached) | about 6s | about 27s |
| Later runs (cached) | instant | about 2s |

## Live emote updates

| Source | What updates | How fast |
|--------|--------------|----------|
| 7TV | Emote added, removed or renamed | Instant (7TV EventAPI) |
| 7TV | Streamer switches to a different emote set | Instant; the new set is loaded |
| 7TV | Personal emote set changes | Instant |
| BTTV | Emote added, removed or renamed | Instant (BTTV socket) |
| FFZ | Emote added or removed | Within 2 minutes (FFZ has no live feed, so it's re-checked) |
| All | Full re-check of 7TV, BTTV, FFZ and badges | Every 15 minutes, in case a live connection dropped |

Changes apply to new messages; messages already on screen stay as they are. New emote images are downloaded in the background, and the saved copy of the emote lists is updated so the next start is current too.

## Customizing the look

There are two ways to change the look:

- **OBS Custom CSS (recommended):** in the browser source's properties, paste CSS into the **Custom CSS** box. It's applied on top of the overlay's own styles, and `chat.html` stays unchanged. You can leave OBS's default CSS in the box, since it doesn't interfere.
- **Edit `chat.html`:** change the `<style>` block at the top of the file.

### Rules for OBS Custom CSS

- **Font size, emote size, GIF height and the font need `!important`.** The overlay sets these from the URL options, which win over your CSS otherwise.
- **Name colors and 7TV paints also need `!important`.** The overlay applies them directly to each name.

### Selectors

| Selector | What it is |
|---|---|
| `#chat` | The whole message column (pinned to the bottom) |
| `.msg` | One message line |
| `.msg.action` | `/me` messages |
| `.msg.notice`, `.msg.notice .sys` | Sub/raid notice box and its text |
| `.msg.status` | "Connected to #channel" line |
| `.msg.fading` | Message while fading out |
| `.msg[data-user="ID"]` | Messages from one Twitch user ID (numeric) |
| `.badges img` | Twitch and 7TV badges |
| `.name` | Username |
| `.name.painted` | Username that has a 7TV paint |
| `.colon` | The `:` after the name |
| `.body` | Message text |
| `.mention` | `@someone` |
| `.emote`, `.stack img` | Emote wrapper and its image(s) |
| `.fx-*` | FFZ modifier effects (`.fx-spin`, `.fx-rainbow`, …) |
| `.gif` | Twitch chat GIF |
| `#loader` | Loading bar |

### Variables

These are set on `:root`:

| Variable | What it sets |
|---|---|
| `--size` | Font size |
| `--emote` | Emote height |
| `--gif` | GIF height |
| `--font` | Font family |
| `--outline` | Text shadow behind messages |

### Examples

```css
/* bigger text, overriding the URL option */
:root { --size: 26px !important; --font: "Comic Sans MS", sans-serif !important; }

/* dark box behind each message */
.msg { background: rgba(0, 0, 0, .55); padding: 4px 10px; border-radius: 8px; }

/* no text outline */
:root { --outline: none !important; }

/* messages start at the top instead of the bottom */
#chat { top: 0; bottom: auto; }

/* hide badges */
.badges { display: none; }

/* turn off 7TV paints, keep Twitch name colors */
.name.painted { background: none !important; -webkit-text-fill-color: currentColor !important; filter: none !important; }

/* name on its own line above the message */
.colon { display: none; }
.body { display: block; }

/* no slide-in animation */
.msg { animation: none; }

/* mention highlight */
.mention { color: #ffcc00; }

/* hide the loading bar */
#loader { display: none !important; }

/* hide one person (numeric Twitch user ID; hideBots= is easier with usernames) */
.msg[data-user="12345678"] { display: none; }
```

### Example themes

The [`custom_css_examples`](custom_css_examples/) folder has ready-made styles to paste into the Custom CSS box. Every line is commented, explaining what it does and why, and they're numbered from simplest to most advanced, so they also work as a CSS tutorial:

- **Basic:** bigger text, a different font, boxes behind messages, name on its own line, newest message on top, hiding things.
- **Medium:** dark panel with striped rows, speech bubbles, neon glow, retro terminal.
- **Advanced:** fluffy clouds, with the name above the cloud or in a pill on top of it.

See the [examples README](custom_css_examples/README.md) for screenshots of every example, what each one teaches, and how to combine them.

**Tip:** to try changes quickly, open `chat.html?channel=...` in Chrome and edit styles live in DevTools (F12). When it looks right, paste the CSS into OBS.

## Troubleshooting

- **Everything loads slowly the first time:** check your DNS. If the first nameserver in `/etc/resolv.conf` doesn't respond, every new hostname waits about 5s before the system falls back to the next one. To test it:
  ```
  time getent hosts 7tv.io
  ```
  If this takes about 5s, fix or remove the dead DNS server.
- **"404" errors in the console:** these are normal. Each one is a chatter who has no 7TV account.
- **A paint appears a moment after someone's first message:** each chatter is looked up once when they first chat, then cached for the session.
- **Someone's personal emotes don't show:** 7TV only announces personal emotes for chatters whose 7TV extension or app is active in this channel. The official 7TV extension follows the same rule.
- **"Channel not found" on the loading bar:** check the spelling of `channel=`.
