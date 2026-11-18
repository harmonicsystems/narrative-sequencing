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
  feeling. Five story grammar stories use it
  (kite-in-the-tree, spilled-paint,
  lost-library-book, block-tower,
  puppy-at-the-door). Each has its own child
  (Sam, Ana, Jo, Max; no pronouns in captions)
  plus Gran and the puppy.
- Two-feelings stories (puppy-at-the-door): two
  characters feel differently at the same
  moment. Same moment = one split card (both
  people, one picture), never two cards, or
  the target order would count a free choice
  as a swap. Feelings are the characters',
  never the student's; no single right
  reading, so nothing here is scored.
- Cast bible (image-prompts.md Part 4): sheets
  in img/_cast/<name>/ (turnaround first). Cards
  are made with Codex (`codex exec -i <refs>`,
  the ChatGPT app's bundled CLI, not Homebrew's
  old one): card 2 first as the master layout,
  then the rest from it. One tool per story.
  Drafts go in img/_drafts/ (git-ignored) with a
  review.html per round.
- Cards with a paper colour show the whole
  picture, so at install trim each set to the
  band its scenes use: one shared crop for a
  story's full scenes (scale never changes), its
  own top trim for the close-up. Keep captions
  to about three lines. Every paper set is
  trimmed (the older 640×857 sets too): a tall
  picture with empty paper shrinks to fit the
  near-square picture area and looks tiny.
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
Setup asks one thing per screen (pick.step), so
there is never a long list to scan:
1. kind: "What are you working on today?" One
   door per story.set, in SETS order: narrative
   "Stories", routine "Everyday steps", change
   "Step-by-step change", concept "Concepts",
   feelings "Feelings" (unknown set: "Other
   stories"). A door has up to three covers
   fanned out, a line icon, one line on what
   the set holds and its count. New sets go in
   the SETS array. If stories.json has only one
   set, this screen is skipped.
2. story: that set's stories as tiles: cover,
   title, "5 cards · K-2".
3. ready: the picked story. Its choices (style
   and, reversible only, direction buttons with
   endLabels, e.g. "calm → puffed"), a preview
   of its cards, then Start under them. Anything
   that depends on the picked story goes here.
Top line on every step: app name, the session
count and End session (once something is
logged), tour, About and Session settings (a
pop-over: things set once for the whole
session). Under it: Back (top left: "All kinds"
or the set's name) and the three numbered
steps; a finished step shows what was chosen
and goes back to it. Leaving a story lands on
step 2 for the same set.
The ready preview must never show the answer: a
student may be watching the screen. Cards are
drawn mixed up (mixedUp(): the same order every
time for a story, never the story order or its
reverse). A reversible story's placed card
comes first, labelled "Placed first". Story
parts are listed in words under the cards,
never on a card. A feelings match shows its
pictures mixed up with no words and no emoji
fallback; the feelings in play stay word
buttons.
Card size: standard | large | largest (memory
only; sets --cw through data-size on :root).
On the play screen fitCards() overrides --cw
(and --cap): all of a story's cards fit one
row and spots + tray + Check fit the window,
max 420px. Height is measured, not guessed:
it sizes once, measures what isn't cards
(header, labels, padding, Check) and solves;
a finished story keeps its size. Keep play
margins tight: every px of chrome comes out
of both card rows. large/largest scale by 1.2 /
1.45 (may wrap). Other screens use the CSS.
View: pictures (default) | list (no covers on
doors or tiles).
A pill (icon + word, never colour alone) marks
what is done this session: "done ×N" on a
story's tile and ready screen, "N done" on a
door. Style and direction choices carry an
icon. Pill tints are color-mix() of the role
tokens, so every theme gets them for free.
A cover is a close-up of one card: optional
story.cover (card number from 1, default the
last card), from the first image style, else
emoji, else words only.
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
   Large touch targets. A magnifier in each
   card's corner opens it full screen (<dialog>;
   its presses stop at the button, so they never
   pick up, drag or mark the card). Access, not
   a cue: not logged. Hidden while retell support
   is faded; a session setting turns it off.
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
One "Clinician tools" tray under the student's
area (.toolTray; the feelings match has one
too): a header with the Level menu and an
"Attempt k of 2" pill, then one icon button per
cue, lightest first:
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
  clinician through one real session (10 steps in
  the TOUR array): a story, its retell, then a
  feelings set. Steps advance on real events
  via tour('pick' | 'start' | 'filled' | 'wrong' |
  'done' | 'retell' | 'back' | 'match' |
  'summary'); nothing is blocked or faked. A
  step's `focus` is a list ("a || b"): the first
  one on screen gets the ring, so it follows the
  setup steps. A link ending in #tour opens
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

## Feelings match (set "feelings")
Stories with kind: "match" (one per cast
member: feelings-jo … feelings-puppy) are not
sequenced: one picture at a time, the student
picks a word / emoji / both, or (open labels)
the clinician types the student's own word.
- card.text is the feeling the picture was
  drawn as; story.who names the character for
  "How does {who} feel?". Styles: subtle |
  exaggerated | colour (lines and colour).
  Images: img/<storyId>/<style>/<feeling>.jpg,
  cut from the F1-F3 sheets (image-prompts.md
  Part 5): same nine feelings, same order.
- Ready screen: Choose from (words | emoji |
  both), Labels (open | fixed words), Feelings
  in play (min 2; same words for the whole
  cast; choices always in card order, so
  positions stay consistent).
- Open: nothing is compared. "Drawn as" is the
  illustrator's intent, never "the answer".
  Fixed: logs matched (chosen = drawn). The
  student never sees right or wrong in either.
- Cues (logged per picture): Verbal | Narrow to
  two (fixed only) | Show the drawn feeling.
- Record: storyId, who, style, labels, choices,
  offered [], drawnAs, chosen, ownWord, matched
  (fixed only), cues [], cueLevel, seconds.
- O-line adds one sentence per group (labels,
  style, choices, feelings): open = listed vs
  own words + words used; fixed = drawn feeling
  chosen X/N + "Other choices: A for B (k/N)".
  Raw data gets a "Feelings" table.
- Never log anything about the student's own
  feelings ("that's how I feel").

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