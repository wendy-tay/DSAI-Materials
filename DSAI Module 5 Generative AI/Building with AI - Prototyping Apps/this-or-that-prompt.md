# This or That — Build Prompt

Single-prompt build for Google AI Studio (Build mode). Replaces the Goal
Achiever prompt. Design rules honoured: flat state, one dynamic list,
arithmetic only, no dates, no chart libraries, localStorage, pre-filled
example, no auth.

**Status: VALIDATED 11 Jul 2026** — one-paste generation succeeded in AI
Studio (Gemini 3.5 Flash, ~143s). Generated stack: React 19 + TypeScript,
Tailwind CSS v4, Vite, localStorage, div-based bars — all brief exclusions
honoured. Both scripted fix prompts apply cleanly (see below). Verdict banner
("Leaning: …") confirmed. Button labels observed: **Share / Publish** (not
"Deploy"). Still to poke: too-close banner flip, <3-criteria guard, First
Step strikethrough, refresh persistence, Start Fresh confirm dialog.

---

```text
Build a single-page web app called "This or That" that helps a user make a
two-option decision using a weighted criteria comparison. No backend, no login —
persist everything in localStorage. Use plain React with inline styles or a
single CSS file. Do not use any charting library — build all bars from styled divs.

Sections, in order:

1. Header: title "This or That", subtitle "Weigh a decision that matters",
   and a "Start Fresh" button that clears all data after a confirm dialog.

2. My Decision: one text input for the decision ("What are you deciding?"),
   and two text inputs for the option names, defaulting to "Option A" and "Option B".

3. What Matters to Me: an input with an "Add" button to add criteria.
   Each criterion is one row showing: its name, a delete button,
   a Weight dropdown (1-5, default 3), and two score dropdowns (1-5, default 3) —
   one under each option's name — for how well each option satisfies this criterion.

4. The Verdict: compute each option's total as the sum of (weight × score)
   across all criteria. Show two horizontal bars made of divs, labeled with the
   option names and totals, scaled relative to the larger total. Below the bars,
   a verdict banner: if the totals differ by more than 10%, show
   "Leaning: [winning option name]"; otherwise show "Too close to call —
   your gut gets the casting vote." If there are fewer than 3 criteria, show
   "Add at least 3 criteria for a meaningful verdict" instead of the banner.

5. First Step: one text input labeled "If I choose [current leader], my first
   step this week is..." with a "Done" checkbox that strikes through the text.

On first load, pre-fill a worked example: decision "Stay in my current role or
take the new job offer?", options "Stay" and "Switch", with five criteria
(Salary weight 4, Growth weight 5, Commute weight 2, Job security weight 3,
Learning opportunities weight 4) scored so that "Switch" wins by a small margin.
Every empty state must show a helpful placeholder line, never a blank area.
All data (decision, options, criteria, scores, first step) persists across refresh.
```

---

## Scripted patch prompts (both VALIDATED 11 Jul 2026 — and both on deck slides)

**Patch 1 — enhance a feature** (deck slide "Patch 1"):

```text
Make the winning option's bar green and the other bar grey,
and animate the bars when scores change.
```

**Patch 2 — turn it into an AI app** (deck slide "Patch 2" — the gold moment):

```text
Add a Gemini call: a button labeled "Suggest a criterion I might
have missed" that sends my decision and current criteria to the
model and adds its one-line suggestion as a new criterion row.
```

**Observed behaviour of Patch 2 (validation run):** AI Studio converted the
app to full-stack — Express backend with Vite middleware — specifically so
the Gemini API key stays server-side, never exposed to the browser. It added
a `/api/suggest-criterion` endpoint (gemini-3.5-flash), a styled button with
loading spinner, and an **AI Insight** box explaining why the suggested
criterion matters. Teaching beats: (a) the services layer arriving on demand;
(b) the app growing a real backend to receive it securely; (c) the app now
calls the same model that built it.

## Validation checklist (run once in AI Studio before deck work)

- [ ] Generates working app in one pass (count any fix prompts needed)
- [ ] Pre-filled example appears on first load; "Switch" leads by a small margin
- [ ] Verdict banner switches to "Too close to call" when totals within 10%
- [ ] Fewer than 3 criteria shows the guard message
- [ ] Refresh — all data persists
- [ ] Start Fresh clears everything after confirm
- [ ] Both scripted fix prompts apply cleanly
- [ ] Note generation time (expect 1–3 min) and any label drift ("Build" vs "Vibe code")
