# Custom CSS examples

Ready-made styles for `chat.html`. Each file is a full working example with comments explaining what every line does and why, so you can also use them to learn how to write your own.

## How to use one

1. Open the file and copy everything in it.
2. In OBS, open the browser source's **Properties** and paste it into the **Custom CSS** box.
3. Click **OK**.

`chat.html` itself stays unchanged. To go back to the normal look, delete what you pasted.

You can combine several examples by pasting them one after another, e.g. `01-bigger-text.css` with `03-message-boxes.css`. If two examples change the same thing, the one pasted last wins. If one of them uses `@import` (like `02-different-font.css`), that line has to go at the very top of the box.

## The examples

They're numbered from simplest to most advanced. If you're new to CSS, start at the top: each one adds only a couple of new ideas.

### Basic: a few lines each

| File | What it does | What you learn |
|---|---|---|
| `01-bigger-text.css` | Bigger text, emotes and GIFs | CSS variables, why some need `!important` |
| `02-different-font.css` | Uses a Google font | `@import`, font fallbacks |
| `03-message-boxes.css` | Dark rounded box behind each message | `background`, `padding`, `border-radius`, opacity |
| `04-name-on-own-line.css` | Name on one line, message below | How a message is built, `display: block` vs inline |
| `05-newest-on-top.css` | Newest message at the top, right-aligned | Moving the chat column, `flex-direction` |
| `06-hide-things.css` | Hide badges, notices, GIFs, the loading bar, one user, 7TV paints… | `display: none`, picking elements with selectors |

### Medium: a complete look

| File | What it does | What you learn |
|---|---|---|
| `07-dark-panel.css` | Solid dark chat panel with striped rows | Page background, `:nth-child()`, `font-weight` |
| `08-speech-bubbles.css` | White comic-style bubbles with a tail | `::after`, the CSS triangle trick, `position` |
| `09-neon.css` | Names glow in their own color | Stacked `text-shadow`, `currentColor`, `drop-shadow` |
| `10-terminal.css` | Green monospace "hacker terminal" with a `>` prompt | `::before` with text, overriding name colors |

### Advanced

| File | What it does | What you learn |
|---|---|---|
| `11-clouds.css` | Name above, message in a fluffy white cloud. A second layout puts the name in a pill on the cloud (change `@media not all` to `@media all` in section 7) | CSS masks built from gradients, stacking contexts, grid, `content-box` |

## Writing your own

The main [README](../README.md#customizing-the-look) lists every class you can style (`.msg`, `.name`, `.body`, `.emote`, …) and the variables (`--size`, `--emote`, …).

The quickest way to experiment: open `chat.html?channel=yourchannel` in Chrome or Edge, press **F12**, and edit styles live in the **Elements** tab. When it looks right, copy your CSS into OBS.

Three rules that catch most people out:

- **`--size`, `--emote`, `--gif` and `--font` need `!important`.** The overlay sets them from the URL options, which beat normal CSS.
- **Name colors and 7TV paints need `!important` too.** The overlay puts them directly on each name.
- **`@import` must be the first thing in the Custom CSS box.** Anything before it, even OBS's default CSS, makes the browser ignore it.
