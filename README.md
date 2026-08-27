# Home

**A browser home page in one HTML file.** Your bookmarks as a start page you can actually see all at once, sticky notes for the things you keep forgetting, and a fresh photo every morning.

![single file](https://img.shields.io/badge/build-none-brightgreen)
![dependencies](https://img.shields.io/badge/dependencies-0-brightgreen)
![size](https://img.shields.io/badge/size-143%20KB-blue)
![vanilla](https://img.shields.io/badge/vanilla-HTML%20%2B%20CSS%20%2B%20JS-orange)

No build step. No npm install. No account, no server, no tracking. Download one file, open it, point your browser's home button at it — done.

Everything you add lives in your browser's own storage, never in the file. So the file itself is safe to share, fork and commit: it arrives **empty** on someone else's machine.

<!--
  Add a screenshot here before publishing, e.g.:
  ![Screenshot](docs/screenshot.png)
  A 1600x900 grab in dark mode with a photo backdrop shows it off best.
-->

---

## Why

Most start pages make you scroll, or hide your links behind folders, or want an account. This one puts every bookmark on screen at once — 68 links across 9 groups fit on a 1080p display with no scrolling — and keeps a notepad in the corner of your eye.

## Features

**Bookmarks**

- Collapsible **named groups** that flow into masonry columns, so everything is visible without scrolling
- **Drag to reorder** anything: bookmarks within a group, bookmarks between groups, whole groups
- Compact rows — favicon and title, with the full URL in the tooltip
- **Live search** across bookmarks *and* notes (`/` to focus). Matching groups open automatically without disturbing their saved collapsed state
- Favicons from the site's own icon where your import provided one, a favicon service otherwise, and a coloured letter tile as a last resort — so a row always has an icon, even offline

**Things to remember**

- Sticky notes in a right-hand column, edited in place
- Six colours, **pin to top**, drag to reorder
- Notes grow to fit what you type and scroll inside themselves if they get long

**Look**

- **A new photo every day**, seeded by the date so it changes at midnight and is cached in between. One click in the corner for a different one
- Or a generated **colour wash** — same idea, zero network, works offline
- Or **plain**, if you want none of it
- **System / light / dark** themes, with frosted panels that keep text readable over any photo

**Data**

- One-click **JSON backup** and restore
- **Import your existing browser bookmarks** straight from an Edge / Chrome / Firefox HTML export

---

## Quick start

1. Download [`home-site.html`](home-site.html).
2. Save it somewhere permanent — moving it later breaks the address you set below.
3. Open it in your browser.
4. Open **⚙ Settings → How to set this page as your home page**, copy the address shown, and paste it into your browser:

| Browser | Where |
| --- | --- |
| **Chrome / Edge** | Settings → Appearance → turn on *Show home button* → choose the custom address, paste |
| **Firefox** | Settings → Home → *Homepage and new windows* → Custom URLs, paste |
| **Safari** | Settings → General → Homepage, paste |

> **Note:** this sets the **home button**. The *new tab* page cannot be pointed at a local file without a browser extension — that's a browser restriction, not a limitation of this page.

## Bringing your existing bookmarks

Don't retype anything. Export from your browser, then import:

1. **Edge / Chrome**: Bookmarks manager → ⋯ → *Export bookmarks*. **Firefox**: Bookmarks → Manage → Import and Backup → *Export Bookmarks to HTML*.
2. Here: **⚙ Settings → Import bookmarks HTML…**
3. Choose **Merge** (keeps what you have) or **Replace**.

What it does with the file:

- Folders become groups; a nested folder becomes `Parent / Child`
- Your bookmarks-bar folder isn't used as a redundant prefix — its contents land at the top level
- Loose bookmarks outside any folder are grouped as *Other favourites*
- **Icons embedded in the export are used directly**, so those bookmarks need no favicon service at all and work offline
- Duplicate URLs are skipped, and anything the page can't open (`chrome-native://newtab/` and friends) is skipped and *reported* rather than silently dropped

## Your data

Everything is stored in `localStorage` under the single key `homepage.v1`. Nothing is written into the HTML file, and nothing leaves your machine.

**That cuts both ways.** Clearing your browser's "cookies and other site data" erases it, as does a browser profile reset. There is no cloud copy and no undo for that.

**So take a backup.** ⚙ Settings → **Download backup** writes a JSON file with your groups, notes and preferences. **Restore from file…** brings it back, on this machine or any other. **Copy JSON** is there if downloads are awkward.

The stored shape is plain and hand-editable:

```json
{
  "version": 1,
  "groups": [
    {
      "id": "…",
      "name": "Work",
      "collapsed": false,
      "bookmarks": [
        { "id": "…", "url": "https://…", "title": "…", "icon": "" }
      ]
    }
  ],
  "notes": [
    { "id": "…", "text": "…", "color": 1, "pinned": false, "createdAt": 0, "updatedAt": 0 }
  ],
  "settings": {
    "theme": "system",
    "background": "photo",
    "favicons": true,
    "newTab": false,
    "clock": true
  }
}
```

Anything malformed is repaired or discarded on load rather than breaking the page, so a hand-edited or truncated file won't lock you out.

## Privacy and network use

The page makes **at most two kinds of outbound request**, both optional:

| Request | What for | Turn it off |
| --- | --- | --- |
| `picsum.photos` | One backdrop photo per day | Backdrop → *Colour wash* or *Plain* |
| `www.google.com/s2/favicons` | Favicon for a bookmark with no embedded icon | Uncheck *Load favicons from the internet* |

Both are plain image loads sent with `no-referrer`. Turn both off and the page is **completely offline** — coloured letter tiles and a generated colour wash, no requests at all.

There is no analytics, no telemetry, no fonts fetched from a CDN, no third-party script. Only `http`, `https`, `mailto`, `ftp`, `ftps` and host-less `file:` links are accepted, so a crafted bookmark or imported backup can't smuggle in a `javascript:` URL.

## Keyboard

| Key | Does |
| --- | --- |
| `/` | Focus search |
| `b` | Add bookmark |
| `n` | Add note |
| `g` | Add group |
| `Enter` in search | Open the first match |
| `Esc` | Clear search, or close a dialog |
| `Alt` + `←` `→` | Move the focused bookmark within its group |
| `Alt` + `↑` `↓` | Move the focused bookmark to the previous / next group |
| `Alt` + `↑` `↓` in a note | Move that note up / down |

Every action is reachable without a mouse — drag and drop is a convenience, not the only route.

## Browser support

Needs a current browser: `<dialog>`, CSS multi-column, `color-mix()` and `backdrop-filter`.

Used and checked on **Chrome** and **Edge**. **Firefox** works. **Safari** should, but its rules for storage on `file://` pages are stricter — if it blocks storage the page says so plainly in a banner and in Settings, rather than silently forgetting your data.

Below 900px wide the two columns stack and the page scrolls normally, so it's usable on a phone, though it's built for a desktop.

## Hacking on it

One file, three plain sections: `<style>`, the markup, then a single `<script>` with no framework. Inside the script, in order: helpers, storage and schema repair, rendering, drag and drop, dialogs, settings, backdrop, boot.

Things you might want to change:

| Want | Look for |
| --- | --- |
| Different colours | The `:root` custom properties at the top of `<style>` |
| Note palette | `--n1` … `--n6` |
| Wider or narrower groups | `#groups { columns: 300px }` |
| Notes column width | `grid-template-columns: minmax(0, 1fr) clamp(270px, 21vw, 380px)` |
| A different photo source | `PHOTO_HOST` and `photoUrl()` |
| A different favicon service | `faviconSrc()` |

## Credits

- Backdrop photos via [Lorem Picsum](https://picsum.photos/), which serves imagery from [Unsplash](https://unsplash.com/)
- Fallback favicons via Google's public favicon service
- No other third-party code — the icons are hand-written inline SVG

## Licence

[MIT](LICENSE) — do whatever you like with it.
