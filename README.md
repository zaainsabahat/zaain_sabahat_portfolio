# Track Record — Operating Manual

The track record is the most important thing on this site. The whole point is **trust through transparency**, which means the discipline matters more than the design.

Read this before posting your first call.

---

## The four rules

**1. Post before you trade.** Add the entry to the data file *and push the change live* before the order goes in. The published timestamp is the proof — it only means something if it's pre-trade.

**2. Never edit a published call.** If you fat-finger an entry price, *don't fix it.* Add a corrective entry as a new trade with a fresh timestamp and a note. Edits destroy the meaning of every other timestamp on the page.

**3. Every trade goes up.** Every trade. Winners, losers, the embarrassing ones, the lucky ones, the stop-outs that hurt to look at. The page is worthless if it's curated.

**4. Numbers come from the data file, never typed by hand.** Win rate, average return, R-multiples — all computed automatically from the entries. Never type a stat into the page directly. The moment you hand-edit a number, the credibility leaks out.

---

## The two surfaces

- `index.html` — homepage. Shows the live track record panel with the 3 stats and most recent trade. Pulls from an inline copy of the trade data.
- `track.html` — the full archive page. Scoreboard, filters, every trade. Also pulls from an inline copy of the trade data.

For now, both files have the trade data embedded inline. **When you add a trade, you must update both files.** Eventually, the better setup is a single `data/trades.json` that both pages fetch — but inline works fine until you have more than ~50 trades.

---

## The data schema

Each trade is one object in the array:

```json
{
  "id": "2026-05-03-engro",
  "date": "2026-05-03T09:42:00+05:00",
  "instrument": "ENGRO",
  "instrument_full": "Engro Corporation",
  "market": "PSX",
  "direction": "LONG",
  "entry": 287.50,
  "target": 305.00,
  "stop": 278.00,
  "thesis": "Q1 results beat on margins; sector rotation building.",
  "status": "open",
  "exit": null,
  "exit_date": null,
  "exit_reason": null,
  "notes": ""
}
```

### Field rules

- `id` — `YYYY-MM-DD-ticker` lowercase. Must be unique.
- `date` — ISO 8601 with PKT offset `+05:00`. This is the entry timestamp. Pre-trade.
- `instrument` — the ticker as it shows on PSX. Uppercase.
- `direction` — `"LONG"` or `"SHORT"`. Uppercase.
- `entry` / `target` — numbers, not strings.
- `stop` — number, OR `null` if you didn't set one. Old trades pre-discipline can be `null`. New trades should always have a stop.
- `status` — `"open"` or `"closed"`.
- `exit`, `exit_date`, `exit_reason` — fill in only when closing. `exit_date` is ISO 8601 with PKT offset.
- `exit_reason` — short string. Examples: `"target hit"`, `"stop hit"`, `"trailing exit"`, `"thesis broke"`, `"partial target"`, `"time stop"`.
- `notes` — short post-mortem (optional). Only fill in if the thesis changed or there's a real lesson to share.

---

## Adding a new trade

1. Open `track.html` and `index.html`.
2. In each file, find the `const TRADES = [` array.
3. Add your new trade as the **first item** in the array (newest first).
4. Set `status: "open"`, leave exit fields as `null`.
5. Save. Push live. Then place the order.

## Closing a trade

1. Find the trade in both files.
2. Change `status` to `"closed"`.
3. Fill in `exit`, `exit_date`, `exit_reason`.
4. If the thesis changed during the trade, add a short `notes` entry. Otherwise leave `notes` empty.
5. Save and push.

---

## What the page computes for you

You never enter these — they all come from the data:

- Total calls, open count, closed count, winners, losers
- Win rate (% of closed calls that finished green)
- Average return (% across all closed)
- Average R (only counts trades that had a stop set)
- Best win, worst loss
- "Last updated" timestamp on the archive page
- Most recent trade on the homepage panel
- "Since [first trade date]" line on the scoreboard

---

## What goes in the `notes` field

Most trades get no note. The thesis was written at entry, and the exit was on plan — there's nothing to add.

A note belongs when:

- The thesis broke and you had to exit early ("Sold off into results — sector rotated out before the print")
- A macro event overrode the bottom-up case ("Stop hit on geopolitical bid — should have waited for the OPEC meeting")
- There's a process lesson worth keeping ("No formal stop on this one — going forward, every trade gets one on entry")

The note should never be a brag or an excuse. It's a journal entry for future you, public-facing.

---

## What NOT to do

- Don't post a trade after the move. Even if the trade was real. The point is the timestamp.
- Don't edit historical trades to look better.
- Don't delete losers. If a trade is on the page, it stays.
- Don't write thesis statements that are vague enough to be retrofitted ("watching for a move"). The thesis must be specific enough that it can be wrong.
- Don't use the page to advise anyone. The disclaimer is real. Keep it active.

---

## When this becomes a moat

After ~30 trades, the page starts to do something most "trader portfolios" can't: it shows a **pattern**. Recruiters can see your hit rate, your loss discipline, the kinds of names you trade, the way you write a thesis. Whether the average R is +0.4 or +1.2 matters less than the fact that *the data is real and the discipline is visible.*

Most candidates can't show that. You can.

---

## Future upgrades (not v1)

- Move the data to a single `data/trades.json` file, fetched by both pages.
- Auto-cross-post to X with the call card embedded.
- OpenTimestamps hash anchored to Bitcoin for absolute proof of pre-trade timestamps.
- Auto-pull current price for open positions to show live P&L.
- Add separate sections for fundamental picks and macro direction calls (gold, oil, FX).
- Per-instrument performance breakdown.

None of these matter until the discipline is solid for 3 months.

---

*If at any point you find yourself wanting to skip a loss, that is the moment the page is doing its job — and the moment you must post it.*
