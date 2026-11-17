# Narrative Sequencing — MVP handoff (v2)

## Goal
Static web app for speech-language therapy (K-8).
Student orders story cards; app logs data and
outputs a pasteable SOAP O-line.

## Stack
- Single index.html, vanilla JS/CSS, no build,
  no dependencies, system font stack
- stories.json + /img/<storyId>/<style>/01.png…
- GitHub Pages from main; works offline once
  loaded
- No server, accounts, analytics, or student
  names. Session data in memory only.

## Built-in story copy
- Browsers block fetch() of stories.json when
  index.html is opened from disk, so index.html
  carries an embedded copy on the line ending
  `// @seed`. After editing stories.json, refresh
  it with the script in README ("Updating the
  built-in copy").
- To test stories.json edits directly:
  `python3 -m http.server 8000`.

## Look and feel
- Warm paper palette (cream, beige, taupe, brown
  accent); warm dark mode. Tokens on :root.
- Keep contrast: text 4.5:1+, outlines of cards,
  spots and buttons 3:1+. No pure black/white.
  Never signal state by colour alone.
- Short fades only; honour prefers-reduced-motion.
- Appearance link on setup: device / light / dark
  (memory only).

## Content model
story = {
  id, title, gradeBand,
  reversible: bool,
  endLabels?: [startLabel, endLabel],
  styles: ["emoji" | "text" | "cartoon" |
           "realistic"],
  cards: [ { text, emoji?, img? } ]
}
- Style names are open: "emoji" and "text" are
  built in; any other name is an image folder
  img/<storyId>/<style>/. card.img overrides the
  default 01.png (used for .svg).
- Concept stories (size, shade, position, fill
  pattern) use style "shape": simple SVGs, one
  hue, outlined so they never rely on fill alone.
- Array order = forward order.
- Card renders the chosen style: image if the
  file exists, else emoji, else text.
- Seed 8 stories using emoji/text only:
  4 one-way (3-, 4-, 5-card) and 4 reversible
  (puffer fish, kettle, storm, voice volume).

## Setup screen (clinician)
Pick story, style, and for reversible stories
the direction (forward | reverse), shown with
endLabels, e.g. "calm → puffed".

## Flow
1. Cards shown shuffled (never in target order).
2. Reversible stories: the first card of the
   chosen direction is pinned in slot 1 so the
   target order is unambiguous.
3. Student arranges: drag via pointer events,
   plus tap-card-then-tap-slot fallback.
   Large touch targets.
4. "Check" scores the whole sequence.
5. Wrong, attempt 1: neutral "Try again" only.
   No indication of which cards are wrong.
6. Wrong, attempt 2: show correct order,
   log as modeled.
7. Next story or End session.

## Data per attempt
storyId, cardCount, style, direction,
attemptNumber (1|2), correct (bool),
misplacedPositions [], placedOrder [],
cueLevel, seconds, modeled.
placedOrder = card numbers (from 1, forward
order) as the student left them. Raw data only.
cueLevel defaults to "independent"; small
clinician-only control sets it per attempt:
independent | verbal | visual | model.

## Output (End session)
Summary + "Copy" button. Template:
"Sequenced X/N {k}-card stories ({style}
stimuli{, reverse order}) correctly on first
attempt, Y/N by second attempt, {cue levels}.
{error sentence}"
- Report each cue level separately if more
  than one was used.
- Group by style/direction if they varied.
- Error sentence from misplacedPositions:
  first / middle / last events.
- Always show the denominator.
Also "Copy raw data" (plain text table).

## Out of scope for v1
Image generation, image upload UI, saved
history, student profiles, service worker.

## Acceptance
- Runs by opening index.html locally
- Full session works by touch only
- Copied O-line matches the logged attempts
- Adding a story or an image set = adding
  files + editing stories.json only
- README documents the stories.json format
  and image folder naming