# Narrative Sequencing — MVP handoff (v2)

## Goal
Static web app for speech-language therapy (K-8).
Student orders story cards; app logs data and
outputs a pasteable SOAP O-line.

## Mission
The order a student builds shows how they
understand the story. The app records that
faithfully and never judges: a wrong order is
information, not a failure. Interpretation
belongs to the clinician. If a change makes the
app score, label or diagnose the student, it is
probably wrong. Full statement: top of README.

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
  `// @seed` (and the shape drawings on the line
  ending `// @shapes`). After editing stories.json
  or a shape SVG, refresh
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
- Buttons have roles: go (sage green: Start,
  Check, Next story, Back to stories), talk
  (dusty blue: Retell), plain outline (End
  session, Stories). Every action button also
  carries an icon and plain wording.
- Theme (memory only, data-look on :root):
  colour-coded buttons (default) | high contrast
  | one colour. Each works in light and dark.
  New colours go in as tokens for all of them.
- Navigation: back is always top left; the next
  step is always the filled button; a finished
  story drops the empty tray so its buttons sit
  under the cards.
- Short fades only; honour prefers-reduced-motion.
- Appearance link on setup: device / light / dark
  (memory only).

## Content model
- Optional card.part: character | setting |
  problem | feeling | action | ending | end
  feeling. Four story grammar stories use it
  (kite-in-the-tree, spilled-paint,
  lost-library-book, block-tower). Each has its
  own child (Sam, Ana, Jo, Max; no pronouns in
  captions) plus Gran and the puppy.
- Cast bible (image-prompts.md Part 4): sheets
  in img/_cast/<name>/ (turnaround first). Cards
  are made with Codex (`codex exec -i <refs>`,
  the ChatGPT app's bundled CLI, not Homebrew's
  old one): card 2 first as the master layout,
  then the rest from it. One tool per story.
  Drafts go in img/_drafts/ (git-ignored) with a
  review.html per round.
- Caption-safe band: with words shown the card
  keeps about the middle 65% of the picture, so
  heads and key props sit between 20% and 80%
  of the height. Full-scene cards are padded
  evenly at install (same amount per story).
  Keep captions to about three lines.
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
  Fill #b07a55 (shades #f3e6d8 #d6ae8c #a06c44
  #553521), outline #6f5f4a. index.html keeps a
  built-in copy on the line ending `// @shapes`
  (same README script as the seed) so they can
  be recoloured: the student picks one of 8
  muted swatches on the play screen (memory
  only, per story; each story starts in its own
  colour). Shades keep their lightness steps.
  Colour is decoration, not data: not logged.
- Optional story.paper = { <style>: "#rrggbb" }:
  the image set's background colour. Cards take
  it so captions blend into the picture; caption
  ink is picked for contrast. Cards with a
  paper colour show the whole picture (contain);
  sets without one fill the card and are trimmed
  (cover), so only full-scene sets like storm
  should go without. Picture edges fade into
  the card. Optional story.zoom = { <style>: n }
  (1 to 2) enlarges a paper set that has margin
  to spare: one value per set, so scale stays
  constant across its cards.
- Generated image sets (puffer-fish/cartoon is
  the first): make each card as an edit of the
  previous one in the same chat, trim all images
  identically, keep scale and position constant
  so only the target property changes.
- Gemini sets are saved as 01.jpg… (640 wide), so
  each card names its file with "img". Stories
  with the child share one Gemini chat ("the same
  child as the very first image") to keep the
  character consistent. Prompts and the full
  workflow: image-prompts.md.
- O-line names neighbouring swaps (exactly two
  adjacent wrong spots) and where they fell.
- Array order = forward order.
- Card renders the chosen style: image if the
  file exists, else emoji, else text.
- Seed 8 stories using emoji/text only:
  4 one-way (3-, 4-, 5-card) and 4 reversible
  (puffer fish, kettle, storm, voice volume).

## Setup screen (clinician)
Setup is two steps: pick a story, press Start.
Three parts, top to bottom:
- Session settings (closed panel, summary line
  shows the choices): things set once for the
  whole session.
- Story list.
- Start bar, stuck to the bottom of the screen:
  story title, its style and (reversible only)
  direction buttons with endLabels, e.g.
  "calm → puffed", and Start. Anything that
  depends on the picked story goes here, never
  below the list.
Card size: standard | large | largest (memory
only; sets --cw through data-size on :root).
Story list view: pictures (default) | list.
The list is grouped by story.set, in this
order: narrative "Stories", routine "Everyday
steps", change "Step-by-step change", concept
"Concepts" (unknown set: "Other stories"). Each
set has a heading with a line icon and one line
on what it holds; the icon repeats small on its
tiles. New sets go in the SETS array.
Pictures shows each story as a small deck with a
close-up of one card: optional story.cover (card
number from 1, default the last card), from the
first image style, else emoji, else words only.
Optional story.cursor (one emoji): the mouse
pointer on the play screen, with a setup
checkbox to turn it off. Decoration only: it
must look the same over every card and spot. A
pointer that reacts to the right card is a cue
and would need clinician control and logging.

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
cueLevel, cues [], seconds, modeled.
placedOrder = card numbers (from 1, forward
order) as the student left them. Raw data only.
cueLevel defaults to "independent"; small
clinician-only control sets it per attempt:
independent | verbal | visual | model.

## Cue ladder (clinician-only, play screen)
Buttons beside the Level menu, lightest first:
Verbal (log only) | Place first card (not for
reversible stories, whose first card is pinned)
| Place last card | Narrow to two (dims the tray
to the right card and one other for the next
open spot) | Pointer hint (toggle, mouse only:
the pointer grows over the card that belongs in
the first spot not yet right; uses story.cursor
or 👆; logged once when turned on, off again on
attempt 2) | Show the order (ends the attempt,
logged as modeled, not counted as an error).
Each press adds its name to the attempt's
`cues` list and raises Level to at least
verbal / visual / model. A cue-placed card
cannot be moved and still counts on attempt 2.
O-line adds "Cues given, by attempt: name (k/N),
…". The app never gives a cue on its own.

## Tour and About
- Tour: a strip at the top that walks a new
  clinician through one real session (8 steps in
  the TOUR array). Steps advance on real events
  via tour('pick' | 'start' | 'filled' | 'wrong' |
  'done' | 'retell' | 'summary'); nothing is
  blocked or faked. A link ending in #tour opens
  straight into it; #about opens the About page.
- About: what the tool is, why it works this way,
  how it was made and who did what. Written in
  the author's voice. Keep its counts (stories,
  cards) true when content changes.

## Retell (optional, after a story is finished)
"Retell" keeps the finished sequence on screen
while the student tells the story. The clinician
taps a card as its part is told (tap again: told
with a prompt; again: clear) and counts time
words and cause words with − / +. Support
(clinician fades it): pictures | words only |
no cards (spots stay tappable); a new retell
starts at the last level used. The app only
counts taps: it never decides what was said.
Story parts: stories whose cards name their
part (card.part) get them from the card taps;
other stories get chips for the parts in
story.grammar (all six if absent, none if []),
tapped the same way. Only list parts a story
really has: routines and change stories have no
problem or feeling. One record per retell:
storyId, cardCount, style, direction, told [],
prompted [], orderTold [] (spot numbers, in the
order first marked), partsOffered, partsTold,
partsPrompted, support [] (levels in order
used), timeWords, causeWords, seconds. Leaving
the story saves it; "Discard this retell" drops
it. O-line adds: "Retell of N stories:
mentioned X/T events, P/X after a prompt;
events told in story order in A/B retells{;
least support reached: …}; W time words and C
cause words used.{ Story parts included: k/n
(part k/n, …), p/k after a prompt.}" Support
clause only when faded. Raw data gets a second
"Retell" table.

## Research
research-notes.md: what narrative intervention
research (mostly Story Champs) suggests, how the
app maps to it, caveats. Borrow principles only:
never Story Champs stories, icons or names in
the app. Parts are counts of clinician taps,
never a score.

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