# Parallel

A daily timed analogy game. 12 clues, 90 seconds, one puzzle a day for everyone, with a Wordle-style shareable result.

Two files run the whole thing:

- `index.html` is the game. You rarely touch it.
- `puzzles.json` is the question schedule. This is the file you edit every day.

If a date has no entry in `puzzles.json`, the game falls back to a built-in bank, so it never shows a blank screen.

---

## Your daily loop

1. Decide tomorrow's date, e.g. `2026-11-07`.
2. Add a key for that date in `puzzles.json` with exactly 12 clues, ordered easy to hard.
3. Commit the change. Your host redeploys in under a minute.

That's it. Players on that date get your set automatically.

### Clue format

```json
"2026-11-07": [
  {"a": "Puppy", "b": "Dog", "c": "Kitten", "ans": ["cat"]},
  ... 11 more ...
]
```

The game shows `a is to b what c is to ___`. `ans` is the list of answers it will accept. Matching ignores case, spacing, punctuation, and a leading "a/an/the", so "The Floor", "floor", and "FLOOR " all pass. Put the cleanest answer first (that one shows in the answer key), then add real synonyms and common spellings so you don't reject a correct guess on a technicality.

Needs to be valid JSON: double quotes, commas between items, no trailing comma after the last one. If the file is malformed, that day falls back to the bank, so a typo degrades gracefully instead of breaking the game.

### Writing good ones

This is the part worth doing yourself. A few rules of thumb:
- One clean relationship per clue (part-to-whole, tool-to-measure, thinker-to-field). Avoid clues where two different relationships both fit.
- Front-load the obvious ones, save the specialist knowledge for 9 through 12.
- If a clue has more than one defensible answer, either add every defensible answer to `ans`, or rewrite it.
- Send me the 12 before you publish and I'll stress-test them for ambiguity, wrong answers, and gaps in `ans`.

---

## Hosting it (pick one)

All of these are free for something this size and give you a link to send friends.

**GitHub Pages** (best if you want to edit questions from your phone or browser)
1. Make a free GitHub account, create a repo, upload `index.html` and `puzzles.json`.
2. Repo Settings > Pages > set Source to the `main` branch, root folder, Save.
3. Your link is `https://<username>.github.io/<repo>/`.
4. Daily edits: open `puzzles.json` on github.com, click the pencil, add tomorrow's block, Commit. Live in about a minute.

**Netlify Drop** (fastest to get a link right now)
1. Go to app.netlify.com/drop and drag the whole `parallel-site` folder in.
2. You get a link immediately. To update questions, drag the folder again, or connect it to a GitHub repo for auto-deploy.

**Cloudflare Pages / Vercel** work the same way if you prefer them. Connect a repo, point it at the folder, done.

Any of them lets you add your own domain (e.g. parallel.game) later in settings.

---

## Testing locally

Opening `index.html` by double-clicking won't load `puzzles.json` (browsers block file reads). Run a tiny local server from inside the folder instead:

```
python3 -m http.server 8000
```

Then open `http://localhost:8000`. Once it's hosted, this isn't needed.

---

## Things to know

- **The puzzle number** ("Parallel #N") counts days from Nov 5, 2026. Change `EPOCH` near the top of the script in `index.html` if you want a different day one.
- **Dates are local.** A player sees the set for their own calendar day. Friends in the same place stay in sync; someone across the world could be a day off. Fine for a friends' game.
- **One play per day** is enforced in the browser, so clearing site data lets you replay. Good enough for friends, not for anything with stakes.
- **No backend.** Comparing scores is paste-your-result-card into the group chat. Leaderboards or private groups would need a server, which is a bigger step.
