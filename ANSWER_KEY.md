# ANSWER KEY — for you, not your cousin

Four bugs are planted, all visible by just using the app. Don't share this file.
The bugs all live in `src/App.svelte`.

---

### Bug 1 — The 20% tip button only applies 2%
- **How to spot it:** Pick the **20%** preset. The tip is way too small
  (about a tenth of what it should be). 10% and 15% work fine.
- **Where:** the `presets` array — the third entry has `value: 2` instead of `20`.
- **Fix:** change `{ label: '20%', value: 2 }` to `value: 20`.

### Bug 3 — The Total ignores the tip
- **How to spot it:** With any tip selected, **Total** equals the bill amount —
  the tip shown right above it is never added in.
- **Where:** `$: total = bill;`
- **Fix:** `$: total = bill + tipAmount;`

### Bug 4 — The Tip amount isn't rounded to cents
- **How to spot it:** With bill `85.50` and 15% tip, the Tip shows `$12.825`
  (three decimals). Other amounts show two decimals, so it looks inconsistent.
- **Where:** `<span class="r-value">${tipAmount}</span>` — missing `.toFixed(2)`.
- **Fix:** `${tipAmount.toFixed(2)}`.

---

Note: bugs 2 and 3 compound, so once your cousin flags "each person pays the
full total" they may also notice the total itself looks wrong. That's fine —
two separate reports.
