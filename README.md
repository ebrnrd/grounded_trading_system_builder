# S.T.A.L.K.E.R. Trader Builder

A browser-based GUI tool for building and editing trader inventory configurations for **S.T.A.L.K.E.R. Anomaly** mods. No installation required — just open the HTML file in any modern browser and start working.

---

## What it does

Anomaly trader profiles define what items a trader stocks through tiered `[supplies_N]` sections, where each item entry is a pair of values: how many units can spawn, and the probability that a single unit actually appears. Writing and maintaining these sections by hand across multiple traders is tedious and error-prone. This tool gives you a graphical interface to do it instead, and exports clean `.ltx` output ready to paste directly into your mod files.

---

## Features

### Item browser (left panel)
- Parses and displays your `grounded_trade_categories.ltx` (or any equivalent categories file) grouped by section
- Collapsible category groups with item counts
- **Resizable panel** — drag the right edge to make the browser wider or narrower
- **Search** by item ID or translated in-game name simultaneously
- Click any item to instantly add it to the currently selected tier

### Trader & tier editor (middle panel)
- Create as many trader profiles as you need in a single session
- Each trader can have unlimited supply tiers
- Each tier supports an optional **parent inheritance** field (e.g. `[supplies_2]:supplies_1`), which is reflected in the exported output
- Per-item controls for **amount** and **spawn chance** (0–1), right-aligned for readability regardless of whether a translated name is available
- **Overwrite detection** — if the same item appears in multiple tiers of the same trader, both entries are flagged:
  - The lower tier entry is marked as **overwritten** (struck through in red) — it has no practical effect
  - The higher tier entry is marked as **overriding** (highlighted in green) — it takes precedence
  - Hover any flag badge to see which tier is involved

### LTX export (right panel)
- Live preview of the generated output, updated as you edit
- Export a single trader or all traders at once
- Includes a commented `buy_supplies` reference line showing the correct tier order
- Proper column alignment in the output to match vanilla file style
- **Copy to clipboard** or **download as `.ltx`**

### File loading
| Button | What it accepts | Purpose |
|---|---|---|
| **Load Categories** | `.ltx` / `.txt` | Item category definitions (e.g. `grounded_trade_categories.ltx`). Can be reloaded at any time to pick up updates. |
| **Load Names (.xml)** | `.xml` (multiple) | Game string table files containing in-game item names. Select as many as you need in one go. |
| **Import** (per trader) | `.ltx` / `.txt` | An existing trader profile. Extracts all `supplies_*` sections including items, values, and parent inheritance. |

---

## Getting started

1. Download `stalker_trader_builder.html`
2. Open it in any modern browser (Chrome, Firefox, Edge — no server needed)
3. **Load your categories file** — click **Load Categories** and select your `grounded_trade_categories.ltx`. The item browser will populate immediately.
4. *(Optional)* **Load name files** — click **Load Names (.xml)** and select any or all of the game's `st_items_*.xml` string table files. In-game names will appear alongside item IDs throughout the tool.
5. **Create a trader** — type the trader's internal name (e.g. `sidorovich`) in the name field and press Enter or click **Add**. To start from an existing file instead, type the name first, then click **Import** and pick the `.ltx` file.
6. **Add tiers** — at the bottom of the trader card, enter a tier name (e.g. `supplies_1`) and optionally a parent tier name, then click **+ Tier**.
7. **Select a tier** — click on any tier header to select it. The item browser will show a confirmation indicator.
8. **Add items** — click any item in the left panel to add it to the selected tier. Adjust amount and chance inline.
9. **Export** — choose which trader to export from the dropdown in the right panel, then click **Copy** or **Download .ltx**.

---

## File format reference

### Categories file (`grounded_trade_categories.ltx`)
Plain `.ltx` format with named sections and one item ID per line. Lines starting with `;` are treated as comments. Sections whose names contain a `:` (aggregate/inheritance macros) are skipped — only leaf sections with direct item lists are imported.

```ini
[ammo_rus_common]
ammo_9x18_fmj
ammo_9x18_ap
ammo_5.45x39_fmj
; ...
```

### Name files (`st_items_*.xml`)
Standard Anomaly string table XML, typically encoded in Windows-1251. The tool handles the encoding automatically. Three ID patterns are recognised and matched to item IDs:

| XML id attribute | Resolves to item key |
|---|---|
| `st_ITEMID_name` | `ITEMID` |
| `st_ITEMID` | `ITEMID` |
| `ITEMID_name` | `ITEMID` |

Description entries (`_descr`, `_desc`, `_description`) and formatting-only text (starting with `%c[` or `\n`) are always ignored.

### Trader profile (import/export)
Standard Anomaly trader `.ltx` format. Only sections whose names contain `supplies` are imported — all other trader settings (buy/sell conditions, discounts, etc.) are ignored.

```ini
[supplies_1]:common_stock
ammo_9x18_fmj        = 10, 0.88
medkit               = 2, 0.75
wpn_pm               = 1, 0.75

[supplies_2]:supplies_1
ammo_5.45x39_fmj     = 10, 0.88
wpn_ak74u_old        = 1, 0.44
```

The two numbers per item are:
- **Amount** — how many units of this item can appear in the trader's stock
- **Chance** — probability (0 to 1) that a single unit actually spawns on a given trader refresh

---

## Notes

- **No installation, no dependencies, no internet required** after the initial load (fonts are loaded from Google Fonts on first open; the tool works offline if fonts are cached or fall back gracefully)
- All data lives in memory for the duration of the session — nothing is saved to disk automatically
- The categories file can be swapped out at any time; the tool will re-render immediately with the new item list
- Name files are additive — loading additional XML files merges new names into the existing map without clearing previous ones
- The overwrite detection is purely informational and does not prevent you from adding the same item to multiple tiers intentionally

---

## Compatibility

Designed for **S.T.A.L.K.E.R. Anomaly** and mods built on top of it (e.g. GAMMA). The `.ltx` format is specific to the X-Ray engine. Other S.T.A.L.K.E.R. titles or total conversions using a different trader system will need adaptation.

---

## License

Do whatever you want with it. Credit appreciated but not required.
