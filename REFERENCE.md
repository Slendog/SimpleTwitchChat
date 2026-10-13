# Reference

Every option and everything you can style. For setup, see the [README](README.md).

- [Options](#options)
  - [URL examples](#url-examples)
  - [The DEFAULTS block](#the-defaults-block)
- [Custom CSS reference](#custom-css-reference)
  - [Rules for OBS Custom CSS](#rules-for-obs-custom-css)
  - [Selectors](#selectors)
  - [Variables](#variables)
  - [Snippets](#snippets)
  - [Testing your CSS](#testing-your-css)

## Options

Add options to the URL after `?`, separated by `&`. This works for the hosted version and for your own copy loaded as a `file:///` URL. With "Local file" ticked in OBS, set them in the [`DEFAULTS` block](#the-defaults-block) inside `chat.html` instead; the names are the same.

The [URL builder](https://slendog.github.io/SimpleTwitchChat/) has a form for all of them.

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
| `emoteUpdates` | `15`         | Which emote change lines to show in chat. A sum of numbers, see [below](#emote-change-notices) |
| `hideBots`     | *(see below)*| Extra usernames to hide, comma-separated |
| `font`         | `Inter`      | Any CSS font-family, e.g. `Arial`, `Comic Sans MS` |
| `gifs`         | `1`          | Show Twitch chat GIFs (GIPHY). `0` shows the `[Title GIF by …]` text instead |
| `gifHeight`    | `150`        | GIF height in px |
| `prefetch`     | `1`          | Download all emote images at startup so they show instantly when first used |
| `loader`       | `1`          | Show the thin loading bar at the top while emotes load |

**On/off options:** `0`, `false`, `no` or `off` turns one off. Any other value turns it on.

**Hidden bots:** nightbot, streamelements, streamlabs, moobot, fossabot, wizebot, sery_bot, soundalerts and kofistreambot are hidden by default. Bots listed on the channel's BTTV page are also hidden automatically. `hideBots` adds names to this list. To unhide a default bot, remove it from `hideBots` in the `DEFAULTS` block (own copy only).

<a id="emote-change-notices"></a>**Emote change notices:** `emoteUpdates` works like Linux file permissions (`chmod 755`). Each kind of line has a number, and you add up the ones you want:

| Number | Line |
|---|---|
| `1` | Emote added |
| `2` | Emote removed |
| `4` | Emote renamed |
| `8` | 7TV emote set switched |

For example `0` = none, `1` = only added, `3` = added + removed, `5` = added + renamed, `15` = all (default). They cover 7TV, BTTV and FFZ channel emotes. The emotes themselves update either way.

**Spaces in values** are written as `%20`, e.g. `font=Comic%20Sans%20MS`.

**URL options win over `DEFAULTS`.** With your own copy as a `file:///` URL, you can keep your usual settings in `DEFAULTS` and only put `?channel=…` in the URL.

### URL examples

Put these after `chat.html` in your URL, e.g. `https://slendog.github.io/SimpleTwitchChat/chat.html?channel=xqc&size=26`:

```
?channel=xqc
?channel=xqc&size=26&fade=30&max=20
?channel=xqc&badges=0&notices=0&hideBots=mybot,otherbot
?channel=xqc&font=Comic%20Sans%20MS&emoteScale=2
?channel=xqc&loader=0
?channel=xqc&emoteUpdates=3
```

### The DEFAULTS block

Your own copy of `chat.html` has this block near the top of its `<script>` section. With "Local file" ticked in OBS, it's the only way to change settings:

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
  emoteUpdates: 15,         // 1 added + 2 removed + 4 renamed + 8 set switched
  gifs: true,
  gifHeight: 150,
  prefetch: true,
  loader: true,
  hideBots: 'nightbot,streamelements,streamlabs,moobot,fossabot,wizebot,sery_bot,soundalerts,kofistreambot',
  font: '',                 // e.g. 'Comic Sans MS'
};
```

Editing rules:
- Keep text values in quotes (`'yourchannel'`).
- Use `true`/`false` for on/off options, and plain numbers for sizes and times.
- Keep the comma at the end of each line.
- After saving, click **Refresh cache of current page** in the OBS source's properties.

## Custom CSS reference

Paste CSS into the browser source's **Custom CSS** box in OBS, replacing what's there. It's added on top of the overlay's own styles, for the hosted version and your own copy alike. With no custom styles, use the [recommended Custom CSS](README.md#recommended-custom-css). For complete, commented examples with screenshots, see [`custom_css_examples`](custom_css_examples/README.md).

### Rules for OBS Custom CSS

- **Font size, emote size, GIF height and the font need `!important`.** The overlay sets `--size`, `--emote`, `--gif` and `--font` from the URL options, which win over your CSS otherwise.
- **Name colors and 7TV paints also need `!important`.** The overlay applies them directly to each name.
- **`@import` (e.g. for a Google font) must be the first thing in the box.** Anything before it makes the browser ignore it.
- **Images from your PC** (`url("my-images/…")`) only work with your own copy of `chat.html`. With the hosted version, use an online image or a `data:` URL. [Example 11](custom_css_examples/11-custom-badges.css) explains all three.

### Selectors

| Selector | What it is |
|---|---|
| `#chat` | The whole message column (pinned to the bottom) |
| `.msg` | One message line |
| `.msg.action` | `/me` messages |
| `.msg.notice`, `.msg.notice .sys` | Sub/raid notice box and its text |
| `.msg.emote-update` | Emote change line ("Slendog added 7TV emote …"). It also has the `notice` class, so notice styles apply to it too |
| `.msg.emote-update[data-source="7tv"]` | Only one source: `7tv`, `bttv` or `ffz` |
| `.msg.emote-update[data-action="added"]` | Only one kind: `added`, `removed`, `renamed`, `set` (7TV set switched) or `many` (more than 5 changes at once, shown as one summary line) |
| `.emote-update .actor`, `.emote-update .emote-name` | Who made the change (7TV only) and the emote name |
| `.msg.status` | "Connected to #channel" line |
| `.msg.fading` | Message while fading out |
| `.msg[data-user="ID"]` | Messages from one Twitch user ID (numeric). Find an ID at `https://api.ivr.fi/v2/twitch/user?login=USERNAME` |
| `.badges img` | Twitch and 7TV badges |
| `.badges img[alt^="moderator/"]` | One kind of badge. The alt text is the badge name and version, e.g. `vip/1`, `subscriber/12` (see [example 11](custom_css_examples/11-custom-badges.css)) |
| `.name` | Username |
| `.name.painted` | Username that has a 7TV paint |
| `.colon` | The `:` after the name |
| `.body` | Message text |
| `.mention` | `@someone` |
| `.emote`, `.stack img` | Emote wrapper and its image(s) |
| `.fx-*` | FFZ modifier effects (`.fx-spin`, `.fx-rainbow`, …) |
| `.gif` | Twitch chat GIF |
| `#loader` | Loading bar |

Each message looks like this in the page:

```html
<div class="msg" data-user="12345678">
  <span class="badges"><img alt="moderator/1"> …</span>
  <span class="name">Username</span>
  <span class="colon">:</span>
  <span class="body">message text and emotes</span>
</div>
```

### Variables

These are set on `:root`:

| Variable | What it sets | Needs `!important` |
|---|---|---|
| `--size` | Font size | Yes |
| `--emote` | Emote height | Yes |
| `--gif` | GIF height | Yes |
| `--font` | Font family | Yes |
| `--outline` | Text shadow behind messages | No |

### Snippets

```css
/* bigger text, overriding the URL option */
:root { --size: 26px !important; --font: "Comic Sans MS", sans-serif !important; }

/* dark box behind each message */
.msg { background: rgba(0, 0, 0, .55); padding: 4px 10px; border-radius: 8px; }

/* no text outline */
:root { --outline: none; }

/* newest message at the top */
#chat { top: 0; bottom: auto; flex-direction: column-reverse; }

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

/* show emote changes, but not removals */
.msg.emote-update[data-action="removed"] { display: none; }

/* hide the loading bar */
#loader { display: none !important; }

/* hide one person (numeric Twitch user ID; hideBots= is easier with usernames) */
.msg[data-user="12345678"] { display: none; }
```

### Testing your CSS

- **In the URL builder:** its live preview shows the example themes on real chat.
- **In your browser:** open your overlay URL in Chrome or Edge, press **F12**, and edit styles live in the **Elements** tab. When it looks right, copy the CSS into OBS.
