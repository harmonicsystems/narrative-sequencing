# Narrative Sequencing

> When a student puts a story in order, they show us how they understand it.
>
> This app gives the student a calm place to arrange events, and gives the
> clinician an honest record of what happened: the order placed, the time
> taken, the help given.
>
> It records and does not judge. A wrong order is information, not a failure.
> The interpretation belongs to the clinician, who knows the child.

A static web app for speech-language therapy (K-8). A student puts story cards
in order; the app logs each attempt and writes a pasteable SOAP O-line.

No server, no accounts, no analytics, no student names. Session data lives in
memory and is gone when the page is closed or reloaded.

## Running it

Open `index.html`. That is the whole app.

One caveat: browsers refuse to read `stories.json` when a page is opened
straight from disk, so in that case the app uses a built-in copy of the stories
stored inside `index.html` (the setup screen says so). To see edits to
`stories.json`, serve the folder instead:

```
python3 -m http.server 8000
```

then visit http://localhost:8000. GitHub Pages behaves the same way as the
local server. Images load either way.

## Research

`research-notes.md` summarises the narrative intervention research (story
grammar, many examples of the same shape, fading picture support, time and
cause words), how this tool maps to it, and its caveats. `image-prompts.md`
Part 4 is the cast bible: how the story grammar pictures were made (character
sheets in `img/_cast/`, cards drawn with Codex from a master layout), with a
log of every round's feedback.

## A session

New to the tool? Add `#tour` to the address (or press "Take the 2-minute tour"
on the first screen) for a guided walk through one session. "About this tool"
explains the thinking behind it and how it was made.

1. **Setup (clinician):** pick a story and press Start. The bar at the bottom
   of the screen shows the story's style and, for reversible stories, its
   direction with the story's end labels (e.g. `calm → puffed`). Session
   settings at the top hold card size, captions, story pointers and appearance.
2. **Arrange (student):** drag cards into the numbered spots, or tap a card and
   then tap a spot. Tapping a placed card while another is selected swaps them.
   In reversible stories the first card is pinned in spot 1. The magnifier in
   a card's corner opens that card full screen; tap anywhere or press Escape
   to close. It is access, not a cue, so it is not logged. It hides while
   retell support is faded, and "Let the student enlarge a card" in Session
   settings turns it off.
3. **Check:** a wrong first attempt gets a neutral "Try again" with no hint
   about which cards are wrong. A wrong second attempt shows the correct order
   and is logged as modeled.
4. **Cue:** the row at the bottom of the play screen gives a cue and logs it:
   Verbal, Place first card, Place last card, Narrow to two, Pointer hint (the
   mouse pointer grows over the next card), Show the order.
   Each press is recorded on the attempt and raises the Level menu beside it
   (independent, verbal, visual, model), which can also be set by hand. The
   level returns to `independent` at the start of each attempt unless a
   cue-placed card is still on the board.
5. **Retell (optional):** once a story is finished, "Retell" keeps the sequence
   on screen while the student tells the story. Tap a card when its part is
   told; tap again if it needed a prompt, and once more to clear it. In
   stories whose cards don't name their part, also mark the story parts the
   student included (character, setting, action, ending…) the same way. Count
   time words (first, then, after, when…) and cause words (because, so…)
   with the − and + buttons. **Support** steps the cards back: Pictures,
   Words only (captions, no pictures), No cards (empty numbered spots, still
   tappable). A new retell starts at the level the last one ended on.
6. **End session:** shows the O-line and the raw data tables, each with a
   copy button.

## stories.json

A JSON array of stories. Array order of `cards` is the forward order.

```json
{
  "id": "puffer-fish",
  "title": "Puffer fish",
  "gradeBand": "K-2",
  "reversible": true,
  "endLabels": ["calm", "puffed"],
  "styles": ["emoji", "text"],
  "cards": [
    { "text": "Small and calm", "emoji": "🐟" },
    { "text": "Half puffed", "emoji": "🐠" },
    { "text": "Fully puffed, spikes out", "emoji": "🐡" }
  ]
}
```

| Field | Notes |
|---|---|
| `id` | Unique, lowercase with hyphens. Also the image folder name. |
| `title` | Shown on the setup and play screens. |
| `set` | Which group the setup screen lists it under: `narrative` (Stories), `routine` (Everyday steps), `change` (Step-by-step change) or `concept` (Concepts). Anything else goes under "Other stories". |
| `gradeBand` | Free text, e.g. `K-2`. |
| `reversible` | `true` if the story also makes sense backwards. Needs at least 3 cards. |
| `endLabels` | Reversible stories only: `[startLabel, endLabel]` for the direction buttons. |
| `styles` | Which styles the setup screen offers. `emoji` and `text` are built in; any other name (`cartoon`, `realistic`, `shape`…) is an image folder. |
| `paper` | Optional. Background color of an image set, per style: `{"cartoon": "#f7f2e7"}`. The card takes that color so the caption strip blends into the picture in light and dark mode. A set with a `paper` color is shown whole, never trimmed, with its edges faded into the card. |
| `zoom` | Optional. How much to enlarge a `paper` image set on the card, per style: `{"cartoon": 1.3}` (1 to 2). Use it when every picture in the set has empty margin all round; pick the largest value that keeps every card's drawing inside the frame. One value per set, so scale never differs between cards. |
| `cover` | Optional. Card number (from 1) shown as the story's picture on the setup screen. Defaults to the last card. |
| `cursor` | Optional. One emoji used as the mouse pointer while the story is played (for example `"💧"`). Decoration only; the setup screen can turn it off. |
| `grammar` | Optional. Story grammar parts offered in the retell step, from `character`, `setting`, `problem`, `feeling`, `action`, `ending`. Leave it out to offer all six; use `[]` for none. List only parts the story really has. |
| `cards[].text` | Required. Caption, and the whole card in `text` style. |
| `cards[].emoji` | Optional. Used in `emoji` style and as the fallback for a missing image. |
| `cards[].img` | Optional. File name that replaces the default `01.png`, `02.png`… |
| `kind` | Optional. `"match"` makes the story a feelings match instead of a sequence (see "Feelings match"). |
| `who` | Feelings match only: the character's name in "How does Jo feel?". |
| `cards[].part` | Optional. The card's story grammar part: `character`, `setting`, `problem`, `feeling`, `action`, `ending` or `end feeling`. Stories with parts show "story parts" on the setup screen, and their retells report parts told. |

For reversible stories, write each card as a **state** ("Half puffed"), not a
change ("Starts to puff up"), so the text reads correctly in both directions.

## Images

```
img/<storyId>/<style>/01.png
img/<storyId>/<style>/02.png
...
```

Numbers follow the forward order of `cards`, two digits, starting at `01`.
`<style>` is whatever name the story lists in `styles`. Cards are 3:4 portrait
and the image is cropped to fill, so use 3:4 images and keep the subject away
from the top and bottom edges (the caption takes some height when words are
shown).

Files other than PNG work through `cards[].img`. The eight concept stories
(small to large, light to dark, low to high, filling up, thin to thick,
few to many, left to right, short to tall) use hand-written SVGs
this way: `"img": "01.svg"` in `img/<storyId>/shape/`. The style name also
appears in the O-line, e.g. "shape stimuli".

To add an image set: put the files in place and add the style name to that
story's `styles`. Each card shows its image if the file exists, otherwise its
emoji, otherwise its text, so a partial set still works.

## Updating the built-in copy

After editing `stories.json` or any concept drawing (`img/*/shape/*.svg`),
refresh the copies inside `index.html` so opening the file from disk matches:

```
python3 - <<'EOF'
import json, re, glob, os
s = json.load(open('stories.json', encoding='utf-8'))
shapes = {f.split('/')[1] + '/' + os.path.basename(f): open(f, encoding='utf-8').read().strip()
          for f in sorted(glob.glob('img/*/shape/*.svg'))}
h = open('index.html', encoding='utf-8').read()
for name, mark, data in (('SEED', 'seed', s), ('SHAPES', 'shapes', shapes)):
    line = 'const ' + name + ' = ' + json.dumps(data, ensure_ascii=False) + ';// @' + mark
    h = re.sub(r'^const ' + name + r' = .*// @' + mark + '$', lambda m: line, h, flags=re.M)
open('index.html', 'w', encoding='utf-8').write(h)
EOF
```

## Concept colours

Concept stories (style `shape`) show a row of colour swatches on the play
screen. The student picks a favourite and the shapes change colour on the
spot; the choice is kept for that story until the page is closed. Each concept
story starts in a different colour. The drawings keep their brown outline, so
no shape depends on its fill, and *Light to dark* gets four shades of the
picked colour at the original lightness steps. A drawing can be recoloured
when its fill is `#b07a55` (and, for shades, `#f3e6d8`, `#d6ae8c`, `#a06c44`,
`#553521`); anything drawn in the outline colour `#6f5f4a` stays as it is.

## Feelings match

The Feelings set has one entry per cast member (`"kind": "match"`). The
student sees one picture at a time and picks how the character feels. Each
card's `text` is the feeling the picture was drawn as, and its image is
`img/<storyId>/<style>/<feeling>.jpg` (for example `img/feelings-jo/subtle/calm.jpg`), in three styles: `subtle`,
`exaggerated` and `colour` (lines and colour). The pictures are cut from the
cast feelings sheets (`image-prompts.md` Part 5).

The start bar sets what the student chooses from (words, emoji or both),
the labels and the feelings in play (at least two; the choices always appear
in the same order).

- **Open labels** (default): the choices are a menu, not a test. "The
  student used another word…" records the student's own word. Nothing is
  compared with the drawn feeling.
- **Fixed words**: the same words every time, and the log records whether
  the choice matches the drawn feeling. "Narrow to two" is offered only
  here.

Either way the student never sees right or wrong; "Next picture" moves on.
The drawn feeling is the illustrator's intent, not the answer: many students,
autistic students especially, read faces differently, and the subtle
pictures are ambiguous on purpose. Each picture records `storyId`, `who`,
`style`, `labels`, `choices`, `offered`, `drawnAs`, `chosen`, `ownWord`,
`matched` (fixed words only), `cues`, `cueLevel` and `seconds`, and the
O-line adds a sentence such as:

> Matched feelings to 5 exaggerated pictures (Ana) with a fixed set of 5
> words shown as emoji (calm, happy, upset, worried, frustrated): chose the
> drawn feeling for 4/5; cues on 1/5 (narrowed to two 1/5). Other choices:
> calm for frustrated (1/5).

## Data and output

Each checked attempt records: `storyId`, `cardCount`, `style`, `direction`,
`attemptNumber`, `correct`, `misplacedPositions`, `placedOrder`, `cueLevel`,
`cues`, `seconds`, `modeled`. `cues` lists each cue given during the attempt,
and the O-line ends with a count of them: "Cues given, by attempt: verbal cue
(1/3), first card placed (2/3)."

`placedOrder` is the order the student left the cards in, as card numbers
counted from 1 in the story's forward order. A correct forward attempt on a
5-card story reads `1,2,3,4,5`; a correct reverse attempt reads `5,4,3,2,1`.
It appears in the raw data only, not in the O-line.

The O-line groups stories by card count, style and direction:

> Sequenced 2/3 4-card stories (emoji stimuli) correctly on first attempt, 3/3
> by second attempt, independently. Errors across 1 incorrect attempt involved
> middle events (1/1), last events (1/1).

When an incorrect attempt has exactly two wrong spots side by side, it is
counted as a swap of two neighboring events and the O-line says where: "2/2
were swaps of two neighboring events at the end of the sequence."

If more than one cue level was used in a group, each level is reported
separately per attempt. Stories whose order had to be shown add "Correct order
modeled for Z/N."

Each retell records: `storyId`, `cardCount`, `style`, `direction`, `told`,
`prompted`, `orderTold`, `partsOffered`, `partsTold`, `partsPrompted`,
`support`, `timeWords`, `causeWords`, `seconds`. The three `parts` fields are
the story grammar parts offered for that story, the ones included, and the
ones among them that needed a prompt; stories with `cards[].part` take them
from the cards tapped, others from the part buttons (`grammar`). `support`
lists each support level used, in order. `told` lists the spots
that were mentioned at all, `prompted` the ones among them that needed a
prompt, and `orderTold` the spots in the order they were first tapped. The app
only counts the clinician's taps. Retells add one sentence to the O-line and a
second table to the raw data:

> Retell of 2 stories: mentioned 8/10 events, 1/8 after a prompt; events told
> in story order in 1/2 retells; least support reached: pictures 1/2, no cards
> 1/2; 3 time words and 1 cause word used. Story parts included: 8/10
> (character 2/2, problem 2/2, feeling 1/2, action 2/2, ending 1/2), 1/8 after
> a prompt.

The support clause appears only when support was faded. Each retell counts
once, at the least support it reached.
