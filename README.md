# Grounded Trading System Builder

A browser-based GUI tool for building and editing trading system configurations for **Grounded** mods. No installation required — just open the HTML file in any modern browser and start working.

---

**Use it here:** https://ebrnrd.github.io/grounded_trading_system_builder/

## What it does

Grounded trading system profiles define what items are available through tiered groups, where each item entry is a pair of values: how many units can spawn, and the probability that a single unit actually appears. Writing and maintaining these sections by hand is tedious and error-prone. This tool gives you a graphical interface to do it instead, and exports clean configuration output ready to paste directly into your mod files.

---

## Features

### Item browser (left panel)
- Parses and displays your categories file grouped by section
- **Collapsible category groups** with item counts, plus a "Collapse all" button
- **Resizable panel** — drag the right edge to make the browser wider or narrower
- **Search** by item ID or translated in-game name simultaneously
- Shows which items are already in the selected trader with a checkmark
- Click any item to instantly add it to the currently selected group
- Selection indicator shows current trader/tier/group path

### Trader & tier editor (middle panel)
- Create as many trader profiles as you need in a single session
- **Collapsible traders** — click the arrow or header to collapse/expand
- Each trader can have unlimited tiers
- Each tier supports an optional **parent inheritance** field
- **Collapsible tiers** — click the arrow or header to collapse/expand
- Per-item controls for **amount** and **spawn chance** (0–1)
- **Overwrite detection** — if the same item appears in multiple tiers of the same trader, both entries are flagged:
  - The lower tier entry is marked as **overwritten** (red) — it has no practical effect
  - The higher tier entry is marked as **overriding** (green) — it takes precedence
  - Hover any flag badge to see which tier is involved

### Export (right panel)
- Live preview of the generated output, updated as you edit
- Export a single trader or all traders at once
- Proper format for the trading system
- **Copy to clipboard** or **download as file**

### File loading
| Button | What it accepts | Purpose |
|---|---|---|
| **Load Categories** | `.ltx` / `.txt` | Item category definitions. Can be reloaded at any time to pick up updates. |
| **Load Names (.xml)** | `.xml` (multiple) | Game string table files containing in-game item names. Select as many as you need in one go. |
| **Import** (per trader) | `.ltx` / `.txt` | An existing trader profile. Extracts all tier sections including items, values, and parent inheritance. |

---

## Getting started

1. Download `index.html`
2. Open it in any modern browser (Chrome, Firefox, Edge — no server needed)
3. **Load your categories file** — click **Load Categories** and select your categories `.ltx` file. The item browser will populate immediately.
4. *(Optional)* **Load name files** — click **Load Names (.xml)** and select any or all of the game's string table XML files. In-game names will appear alongside item IDs throughout the tool.
5. **Create a trader** — type the trader's internal name in the name field and press Enter or click **Add**. To start from an existing file instead, type the name first, then click **Import** and pick the `.ltx` file.
6. **Add tiers** — at the bottom of the trader card, enter a tier name and optionally a parent tier name, then click **+ Tier**.
7. **Add groups** — at the bottom of each tier, enter a group name and click **+ Group**.
8. **Select a group** — click on any group header to select it. The item browser will show a confirmation indicator.
9. **Add items** — click any item in the left panel to add it to the selected group. Adjust amount and chance inline.
10. **Export** — choose which trader to export from the dropdown in the right panel, then click **Copy** or **Download**.

---

## File format reference

### Categories file
Plain `.ltx` format with named sections and one item ID per line. Lines starting with `;` are treated as comments.

```ini
[ammo_rus_common]
ammo_9x18_fmj
ammo_9x18_ap
ammo_5.45x39_fmj
; ...
```

### Name files (`st_items_*.xml`)
Standard string table XML. Five ID patterns are recognised and matched to item IDs:

| XML id attribute | Resolves to item key |
|---|---|
| `st_ITEMID_name` | `ITEMID` |
| `st_ITEMID` | `ITEMID` |
| `ITEMID_name` | `ITEMID` |
| `ITEMID` | `ITEMID` (direct match) |

Description entries (`_descr`, `_desc`, `_description`) and formatting-only text (starting with `%c[` or `\n`) are always ignored.

### Trader profile (import/export)
Standard trading system `.ltx` format.

```ini
[trader_name]
{
    [tier_1]
    {
        [group_1]
        {
            item_name = 10, 0.88
            medkit = 2, 0.75
        }
    }
}
```

The two numbers per item are:
- **Amount** — how many units of this item can appear
- **Chance** — probability (0 to 1) that a single unit actually spawns

---

## Notes

- **No installation, no dependencies, no internet required** after the initial load
- All data lives in memory for the duration of the session — nothing is saved to disk automatically
- The categories file can be swapped out at any time; the tool will re-render immediately with the new item list
- Name files are additive — loading additional XML files merges new names into the existing map without clearing previous ones
- The overwrite detection is purely informational and does not prevent you from adding the same item to multiple tiers intentionally

---

## Compatibility

Designed for **Grounded** mods. The configuration format is specific to the game.

---

## License

Do whatever you want with it. Credit appreciated but not required.
