# Simple Twitch Chat

A Twitch chat overlay for an OBS browser source. It shows 7TV, BTTV and FFZ emotes, plus 7TV paints (name colors) and 7TV badges.

It connects to chat anonymously, so no login or token is needed. It's a single HTML file: there's nothing to install.

## Quick start

1. Open the **URL builder: [slendog.github.io/SimpleTwitchChat](https://slendog.github.io/SimpleTwitchChat/)**
2. Type your channel name and pick options. The live preview shows the real chat with your settings.
3. Optional: under **Look**, pick one of the [example themes](custom_css_examples/README.md).
4. Click **Copy URL**. In OBS, add a **Browser** source, untick **Local file**, and paste it into **URL**. Set the size to about `450 × 900`.
5. Replace everything in the same source's **Custom CSS** box with the CSS from the builder's **Look** section (**Copy CSS**): your theme's CSS, or with no theme, the [recommended CSS](#recommended-custom-css).

That's it. The rest of this page explains the two ways to use the overlay, and how to run your own copy.

## Two ways to use it

| | Hosted version | Your own copy |
|---|---|---|
| What you need | Nothing, just a URL | `chat.html` downloaded to your PC |
| Updates | Automatic, always the latest version | Only when you download a new copy |
| Settings | In the URL (`?channel=…&size=…`) | In the URL, or in the `DEFAULTS` block inside the file |
| Custom CSS in OBS | Yes | Yes |
| Images from your PC in Custom CSS | No (use an online image or a `data:` URL) | Yes |
| Edit the overlay's code | No | Yes |

Both use the same overlay and the same [options](REFERENCE.md#options). Custom CSS works the same way in both: OBS adds it to whatever page the source shows.

### 1. Hosted version

The overlay is hosted on GitHub Pages. The [URL builder](https://slendog.github.io/SimpleTwitchChat/) writes the URL for you, but you can also write it yourself:

```
https://slendog.github.io/SimpleTwitchChat/chat.html?channel=YOURCHANNEL
https://slendog.github.io/SimpleTwitchChat/chat.html?channel=YOURCHANNEL&size=26&fade=30
```

In OBS:

1. Add a **Browser** source.
2. **Untick "Local file"**, and paste the URL into **URL**.
3. Set width and height, e.g. `450 × 900`. The background is transparent.
4. Replace what's in **Custom CSS** with the [recommended CSS](#recommended-custom-css), or with a theme (see [Changing the look](#changing-the-look)).

Good to know:

- **Updates are automatic.** OBS loads the newest version each time the source starts. If you want a fixed version that never changes, use your own copy.
- **All settings go in the URL.** Every setting is a URL option, so nothing is missing compared to your own copy.
- **Files from your PC can't be used,** for example as custom badges in Custom CSS. Browsers don't let websites read files from your computer. Use an image that's online, or a `data:` URL (see [example 11](custom_css_examples/11-custom-badges.css)).

### 2. Your own copy

Download `chat.html`: either the whole project (green **Code** button on GitHub → **Download ZIP**, which includes the CSS examples), or just the file ([open it raw](https://raw.githubusercontent.com/Slendog/SimpleTwitchChat/main/chat.html), then **Save page as…**). Put it anywhere you like.

There are two ways to load it in OBS. Both use a **Browser** source.

| | a) As a `file:///` URL | b) With "Local file" ticked |
|---|---|---|
| Settings | In the URL, like the hosted version | In the `DEFAULTS` block inside `chat.html` |
| Change a setting | Edit the URL in OBS | Edit `chat.html`, then refresh the source |
| Several overlays from one file (other channels or looks) | Yes, one URL each | No, copy the file once per setup |

Either way, set the source size to about `450 × 900`, or whatever fits your layout.

#### a) As a `file:///` URL (recommended)

1. Add a **Browser** source.
2. **Untick "Local file"**.
3. Set **URL** to the file's path in `file:///` form, with your options added after `?`:

   **Windows**
   ```
   file:///C:/Users/YourName/Documents/SimpleTwitchChat/chat.html?channel=YOURCHANNEL
   ```

   **Linux / macOS**
   ```
   file:///home/yourname/SimpleTwitchChat/chat.html?channel=YOURCHANNEL
   ```

   Path rules:
   - Use forward slashes `/`, not backslashes `\`.
   - Start with `file:///` (three slashes). On Windows, the drive letter comes next.
   - Write any space in the path as `%20`, e.g. `C:/Users/John%20Smith/…`.
   - Windows: right-click `chat.html` in Explorer → **Copy as path**, then swap `\` for `/` and remove the quotes.

4. Replace what's in **Custom CSS** with the [recommended CSS](#recommended-custom-css), or with a theme.

Tip: build the options part in the [URL builder](https://slendog.github.io/SimpleTwitchChat/) and copy everything from `?channel=` onward.

#### b) With "Local file" ticked

In this mode OBS opens the file itself, so you can't add `?channel=…` options. All settings come from the `DEFAULTS` block in `chat.html` instead.

1. Open `chat.html` in a text editor (Notepad, VS Code, TextEdit, …).
2. Find `const DEFAULTS = {` near the top of the `<script>` section, and change the values you need. At minimum, set `channel`. See [the DEFAULTS block](REFERENCE.md#the-defaults-block) for a full example and the editing rules.
3. Save the file.
4. In OBS, add a **Browser** source, **tick "Local file"**, click **Browse** and pick `chat.html`. Replace what's in **Custom CSS** with the [recommended CSS](#recommended-custom-css), or with a theme.
5. After any later edit to `chat.html`, open the source's properties and click **Refresh cache of current page**.

Tips:
- **More than one setup** (different channels or looks): make a copy per setup, e.g. `chat-xqc.html` and `chat-mine.html`, each with its own `DEFAULTS`.
- **Keep your settings when updating:** before replacing `chat.html` with a newer version, copy your `DEFAULTS` block out of it. Otherwise it goes back to the stock values.

## Changing the look

Paste CSS into the browser source's **Custom CSS** box in OBS. This works with the hosted version and with your own copy, and `chat.html` stays unchanged.

### Recommended Custom CSS

If you don't use a theme, replace everything in the **Custom CSS** box with this:

```css
body {
  background-color: rgba(0, 0, 0, 0);
  margin: 0px auto;
}
```

If you use a theme, replace the box's contents with the theme's CSS instead.

### Themes and your own CSS

- **Ready-made themes:** the [`custom_css_examples`](custom_css_examples/README.md) folder has 12 commented examples with screenshots, from bigger text and message boxes to speech bubbles, neon, a retro terminal, custom badges for mods/VIPs/the streamer, and fluffy clouds. Every line explains what it does and why, so they double as a CSS tutorial. You can also try them in the [URL builder](https://slendog.github.io/SimpleTwitchChat/)'s preview.
- **Writing your own:** [REFERENCE.md](REFERENCE.md#custom-css-reference) lists every class and variable you can style, the `!important` rules, and short snippets.

With your own copy you can also edit the `<style>` block at the top of `chat.html` directly.

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

The cache belongs to the address the overlay is loaded from. Switching between the hosted version and your own copy (or moving the file) means one slow first start again.

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

## Troubleshooting

- **Nothing shows up:** a quiet channel may have no messages for a while. Check the channel name, and try a busy channel to be sure the overlay works.
- **"Channel not found" on the loading bar:** check the spelling of `channel=` (or `channel` in `DEFAULTS`).
- **A change to `chat.html` or the Custom CSS doesn't show:** open the source's properties and click **Refresh cache of current page**.
- **Custom CSS has no effect on the size or font:** those need `!important`. See [the rules](REFERENCE.md#rules-for-obs-custom-css).
- **Everything loads slowly the first time:** the first start downloads every emote image (see [Loading and caching](#loading-and-caching)). If it's slow every time, check your DNS. On Linux, if the first nameserver in `/etc/resolv.conf` doesn't respond, every new hostname waits about 5s. Test it with `time getent hosts 7tv.io`; about 5s means a dead DNS server.
- **"404" errors in the browser console:** these are normal. Each one is a chatter who has no 7TV account.
- **A paint appears a moment after someone's first message:** each chatter is looked up once when they first chat, then cached for the session.
- **Someone's personal emotes don't show:** 7TV only announces personal emotes for chatters whose 7TV extension or app is active in this channel. The official 7TV extension follows the same rule.
