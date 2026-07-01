# Miles & Baskets

A tiny single-file web app for splitting a basket of [KrisShop](https://www.krisshop.com) products between two people, each with their own KrisFlyer miles budget.

**Stack:** vanilla HTML + CSS + JS · **Build:** none · **Dependencies:** none (Google Fonts loaded via CDN)

Open `index.html` in any modern browser — that's the whole install story.

---

## What it does

KrisShop prices products in KrisFlyer miles, which makes it awkward when two people are sharing an order — you want to split who is paying for what without going over either person's miles budget. Miles & Baskets gives you a visual three-column board for exactly that:

| Column       | Purpose                                                                |
| ------------ | ---------------------------------------------------------------------- |
| **Products** | The shared pool of everything you'd consider buying.                   |
| **Person A** | Allocated to Person A. Shows their budget, miles spent, and remaining. |
| **Person B** | Allocated to Person B. Same as above.                                  |

Drag a card from one column to another to assign it. The progress bar and remaining-miles counter update live. Edit the budget on either person's column header to change their allowance.

Each card also shows:

- Product name (with hover tooltip for the full title)
- A small external-link icon that opens the item on KrisShop
- An optional **note** (e.g. size, capacity)
- A coloured **tag** (Audio, Gaming, F1, LEGO, etc.) so you can scan the basket by category
- The miles cost — or "No price" if `miles` is unset

If a card's total would push the budget over, the remaining counter turns red and the bar flips to red as well.

---

## Features

- **Drag-and-drop partition UI** — pure HTML5 drag-and-drop, no framework.
- **Configurable per-person budgets** — typed input above each column.
- **Data-driven tag colours** — every tag's appearance comes from a single `TAGS` object (no scattered CSS classes); matching `.tag-<name>` rules are injected automatically on load.
- **Fallback styling** — products with an unrecognised tag still render, just with a neutral grey pill.
- **Responsive** — three columns on desktop, stacked on narrow screens.

### Limitations

- **No persistence.** On reload, all assignments and budget edits reset to defaults from the source code. (Live development — adding localStorage is on the roadmap.)

---

## Customising

Everything configurable lives as a handful of `const`s near the top of the `<script>` in `index.html`.

### `PRODUCTS`

Add, remove, or edit entries. Only `name` is mandatory — `note`, `miles`, `tag`, `url` are all optional. Pass `miles: null` (or just omit it) to render a "No price" card; pass an unrecognised `tag` and the fallback grey pill will be used. A full entry:

```js
{
  id: "K12345",                       // KrisShop product code
  name: "Some Cool Headphones",
  miles: 7000,                        // cost in KrisFlyer miles (integer or null)
  note: "Black",                      // optional subtitle
  tag: "Audio",                       // should exist in TAGS, otherwise fallback style
  url: "https://www.krisshop.com/en/product/K12345/..."
}
```

### `TAGS`

Each entry defines a tag's pill colour. Add new tags here and the matching `.tag-<name>` CSS rule will be injected automatically at startup:

```js
const TAGS = {
  Audio: { bg: "#e8f0fe", fg: "#1a56db" },
  Watch: { bg: "#fef9c3", fg: "#854d0e" },
  // add more…
};
```

### `BUDGETS`

The initial miles allowance for each person. Keys must match the `key` of each column in the `columns` array (see below). After load, the values live in `state.budgetA` / `state.budgetB` and become user-editable through the budget input:

```js
const BUDGETS = {
  a: 19730,
  b: 23386,
};
```

### Adding a third person (or renaming columns)

The `columns` array inside `render()` is what defines each basket column — title, dot colour, fill colour, and which key in `state` holds its budget. To add a third person, append an entry there (declaring a matching `BUDGETS` key and `state.budgetC`) and add a `.dot-c`/`.fill-c` rule to the CSS.

---

## Running locally

Just open the file:

```bash
# macOS
open index.html

# Linux
xdg-open index.html

# Windows
start index.html
```

Or drag `index.html` into a browser window. That's it.

---

## Tech

- Plain HTML + CSS + vanilla JS — no framework, no bundler
- Google Fonts: DM Sans (loaded via CDN)

---

## License

See [`LICENSE`](./LICENSE).

## Credits

Developed by [Clyde D'Souza](https://clydedsouza.net).
