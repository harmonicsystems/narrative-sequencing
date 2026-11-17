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

## A session

1. **Setup (clinician):** pick a story, a style, and for reversible stories a
   direction, shown with the story's end labels (e.g. `calm → puffed`).
2. **Arrange (student):** drag cards into the numbered spots, or tap a card and
   then tap a spot. Tapping a placed card while another is selected swaps them.
   In reversible stories the first card is pinned in spot 1.
3. **Check:** a wrong first attempt gets a neutral "Try again" with no hint
   about which cards are wrong. A wrong second attempt shows the correct order
   and is logged as modeled.
4. **Cue:** the small control at the bottom right sets the cue level for the
   attempt in progress. It returns to `independent` at the start of each attempt.
5. **End session:** shows the O-line and the raw attempt table, each with a
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
| `gradeBand` | Free text, e.g. `K-2`. |
| `reversible` | `true` if the story also makes sense backwards. Needs at least 3 cards. |
| `endLabels` | Reversible stories only: `[startLabel, endLabel]` for the direction buttons. |
| `styles` | Which styles the setup screen offers. `emoji` and `text` are built in; any other name (`cartoon`, `realistic`, `shape`…) is an image folder. |
| `paper` | Optional. Background color of an image set, per style: `{"cartoon": "#f7f2e7"}`. The card takes that color so the caption strip blends into the picture in light and dark mode. |
| `cards[].text` | Required. Caption, and the whole card in `text` style. |
| `cards[].emoji` | Optional. Used in `emoji` style and as the fallback for a missing image. |
| `cards[].img` | Optional. File name that replaces the default `01.png`, `02.png`… |

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

After editing `stories.json`, refresh the copy inside `index.html` so opening
the file from disk matches:

```
python3 - <<'EOF'
import json, re
s = json.load(open('stories.json', encoding='utf-8'))
h = open('index.html', encoding='utf-8').read()
line = 'const SEED = ' + json.dumps(s, ensure_ascii=False) + ';// @seed'
open('index.html', 'w', encoding='utf-8').write(
    re.sub(r'^const SEED = .*// @seed$', lambda m: line, h, flags=re.M))
EOF
```

## Data and output

Each checked attempt records: `storyId`, `cardCount`, `style`, `direction`,
`attemptNumber`, `correct`, `misplacedPositions`, `placedOrder`, `cueLevel`,
`seconds`, `modeled`.

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
