# Gemini prompts for the cartoon image sets

Prompts for the seven stories that have no image set yet. They match the
existing `img/puffer-fish/cartoon/` set: flat cartoon, soft brown outline,
muted warm colours, cream background, portrait 3:4 (640 x 852).

## Rules for every set

- All seven sets are done (2026-11-16). The prompts below are kept for
  redoing a card.
- Object and scene stories (kettle, storm) get one chat each. The child
  stories all go in the anchor's chat, with "the same child as the very
  first image" in each prompt: this kept the child consistent without
  uploading the anchor.
- The free Gemini image limit ran out after about 19 images in one evening
  and reset at 11:30 PM.
- Wait for each image to finish before sending the next prompt: a prompt
  sent too early left the one before it with no image.
- Paste the STYLE block into every prompt, word for word.
- If a card drifts (size, position, colours), regenerate it from the previous
  card. Do not try to repair it later.
- Reject any card with text, numbers, arrows, borders or panels: they give
  the order away.
- Neighbouring cards must be easy to tell apart at thumbnail size.
- Gemini downloads are 896 x 1200 JPEG. Save as
  `img/<storyId>/cartoon/01.jpg`, `02.jpg`, … resized to 640 wide, set each
  card's `"img"`, add `"cartoon"` to the story's `styles` in `stories.json`, set
  `paper.cartoon` to the background colour sampled from the files, and
  refresh the built-in copy in `index.html` (see README).

## STYLE block

```
Flat, simple cartoon illustration for a children's picture card. Even
medium-brown outlines, soft muted warm colours (tan, cream, sage green,
dusty blue), no gradients, no shading, no shadows. Plain flat cream
background (#f7f2e7) with nothing else in it. Portrait 3:4. Subject centred
with wide, even margins. No text, letters, numbers, arrows, borders, frames
or panels.
```

---

# Part 1: one subject changing (edit chain)

Generate card 1, then make each later card as an edit of the one before it.

Every edit prompt starts with this EDIT block:

```
Edit the previous image. Keep the subject, its size, position, angle,
outline, colours and background exactly the same. Change only what is
described below. No text, numbers or arrows.
```

## kettle (4 cards, cold → boiling)

1. `[STYLE]` A clear glass kettle with a curved spout and a handle, seen
   from the side, sitting on a small round stove burner. It is two-thirds
   full of pale blue water. The water is perfectly still: no bubbles, no
   steam.
2. `[EDIT]` Add a few tiny bubbles, only along the bottom of the water. No
   steam.
3. `[EDIT]` Fill the water with many bubbles of mixed sizes rising to the
   surface. Add one thin wisp of steam above the spout.
4. `[EDIT]` Make the water a rolling boil with big bubbles breaking the
   surface. A strong jet of steam shoots from the spout, with three short
   curved lines beside it to show a whistle.

## storm (5 cards, clear → storm)

The scene fills the whole card, so replace the background sentence in STYLE
with: `The scene fills the whole picture, edge to edge.` No `paper` colour
is needed for this set.

1. `[STYLE]` A small rounded green hill with one round tree on it, low in
   the picture. Above it a wide, clear, bright blue sky with a pale yellow
   sun. No clouds at all.
2. `[EDIT]` Add three small white puffy clouds. The sun is still fully
   visible and the sky is still blue.
3. `[EDIT]` Cover the whole sky with thick gray clouds. No sun, no blue sky.
   The hill and tree are slightly dimmer. No rain.
4. `[EDIT]` Add steady rain falling from the gray clouds across the whole
   picture.
5. `[EDIT]` Make the clouds darker gray. Keep the rain. Add one large bright
   yellow lightning bolt from the clouds, to the side of the tree.

## voice-volume (5 cards, silent → yelling)

Start card 1 from the child anchor image in Part 2 so the same child appears
across the stories. Keep the face friendly and relaxed in every card:
yelling is loud, not angry.

1. `[STYLE]` Head and shoulders of the same child as the attached image,
   facing slightly to the right. Mouth closed in a relaxed line. No sound
   lines anywhere.
2. `[EDIT]` Mouth open just a little. One hand cupped beside the mouth. One
   very small curved sound line in front of the mouth.
3. `[EDIT]` Hand down again. Mouth open in a normal talking shape. Two
   medium curved sound lines in front of the mouth.
4. `[EDIT]` Mouth open wide. Three large curved sound lines in front of the
   mouth.
5. `[EDIT]` Mouth open as wide as it goes. Both hands cupped around the
   mouth. Four very large curved sound lines reaching the edge of the
   picture. The face stays friendly, not angry.

---

# Part 2: different scenes, same child (anchor image)

Make the anchor once. Attach it to every card prompt below, and start each
of those prompts with this ANCHOR block:

```
Use the attached image as the reference for the child and the drawing
style. Same child, same clothes, same outline and colours. New scene:
```

## Anchor

`[STYLE]` A child about seven years old standing and facing forward, arms
relaxed at their sides, whole body visible. Short dark curly hair, a
sage-green t-shirt, tan trousers, a calm friendly face.

## brushing-teeth (3 cards)

1. `[ANCHOR] [STYLE]` The child, waist up, holds a toothbrush in one hand
   and squeezes a tube of toothpaste onto the bristles with the other.
   Mouth closed. No foam.
2. `[ANCHOR] [STYLE]` The child, waist up, brushes their teeth. The
   toothbrush is in the open mouth and there is white foam around the
   teeth. No tube in the picture.
3. `[ANCHOR] [STYLE]` The child, waist up, leans over a small sink holding
   a cup of water, rinsing. The toothbrush lies on the edge of the sink. No
   foam.

## planting-a-seed (4 cards)

Same patch of brown soil, same viewpoint in all four.

1. `[ANCHOR] [STYLE]` The child kneels beside a patch of brown soil and
   digs a small hole with a hand trowel. The hole is empty. No seed, no
   watering can.
2. `[ANCHOR] [STYLE]` The child kneels beside the same patch of soil and
   drops one seed from their fingers into the open hole. The trowel lies on
   the soil.
3. `[ANCHOR] [STYLE]` The hole is now filled in as a small smooth mound.
   The child pours water over the mound from a watering can. Nothing is
   growing.
4. `[ANCHOR] [STYLE]` A small green sprout with two leaves stands up from
   the mound. The child kneels beside it, looking at it. The watering can
   sits on the ground.

## making-a-sandwich (4 cards)

Same plate, same table-top view in cards 1 to 3.

1. `[STYLE]` Two slices of bread lying side by side on a round plate, seen
   from slightly above. Nothing on the bread.
2. `[EDIT]` A slice of yellow cheese and a green lettuce leaf now lie on
   the left slice. The right slice is still empty beside it.
3. `[EDIT]` The right slice is now on top of the left one, making one
   closed sandwich in the middle of the plate. Cheese and lettuce show at
   the edges. The sandwich is whole.
4. `[ANCHOR] [STYLE]` The child, waist up, holds the same sandwich in both
   hands. One clear bite is missing from it and the child is chewing.

## mailing-a-letter (5 cards)

1. `[ANCHOR] [STYLE]` The child sits at a table writing on a sheet of paper
   with a pencil. Wavy scribble lines stand in for writing: no real letters
   or words. No envelope in the picture.
2. `[ANCHOR] [STYLE]` The child, waist up, presses the flap of a white
   envelope closed with both hands. The envelope has no stamp.
Cards 3, 4 and 5 carry this ENVELOPE block so the stamp is on the address
side, not on the flap:

```
We see the FRONT of the envelope, the address side: a plain white rectangle
with NO triangular flap and NO diagonal fold lines. In the middle are three
short wavy scribble lines standing in for an address (no real letters). The
small red square stamp is in the top right corner as we look at it. Exactly
one envelope, exactly one stamp.
```

3. `[ANCHOR] [STYLE] [ENVELOPE]` The child, waist up, holds the envelope in
   front of their chest with one hand and presses one finger of the other
   on the stamp.
4. `[ANCHOR] [STYLE] [ENVELOPE]` A blue street mailbox on the right with a
   wide dark slot near the top of its front. The child stands on the left,
   turned towards it, one arm stretched out, pushing the envelope into the
   slot. The envelope is half in.
5. `[STYLE] [ENVELOPE]` A grey-haired grandparent stands in an open doorway
   holding the same envelope, its top edge torn open, and reads the sheet
   of paper from inside it with a smile. The child is not in this picture.

Redone 2026-11-17: the first set had the stamp on the flap side. Card 4
needed four tries. "The envelope has no other lines on it at all: no flap,
no triangle, no diagonal lines, no folds" removed the flap. Asking for the
same picture "again" with the envelope further in changed almost nothing;
asking for "a different composition" with the child turned towards the
mailbox and an arm stretched out is what made it read as posting. Gemini
would not draw the envelope level and half hidden in the slot. Card 2
still shows the back, which is right for sealing.

---

# Part 3a: the first kite set (Gemini, replaced)

The app now uses the Part 4 kite set (Sam and Gran, drawn with Codex). These
prompts and notes are kept for the lessons in them.

## kite-in-the-tree (5 cards)

The first story with a problem and a feeling, so the retell step can offer
all six story grammar parts. Made in the child anchor chat.

Every prompt carries this KITE block, word for word, so the kite never
changes:

```
The kite is always the same: a classic diamond-shaped kite, dusty red, with
a cream cross on it (one line top to bottom, one line side to side), a thin
string from its centre, and a tail with three small sage-green bows.
```

Cards 1, 2, 4 and 5 share one scene: a strip of green grass with one round
leafy tree on the left. Card 2 is an edit of card 1 so the scene is
identical.

1. `[ANCHOR] [STYLE] [KITE]` The child stands on the grass holding the kite
   string in both hands, looking up and smiling. The kite flies high in the
   upper right. Nothing is in the tree. (character, setting)
2. `[EDIT] [KITE]` The kite is now stuck in the top of the tree among the
   leaves, its tail hanging down and its string dangling loose. Nothing is
   in the sky. The child's hands are empty and the child looks up at the
   tree, mouth open in surprise. (problem)
3. `[ANCHOR] [STYLE] [KITE]` Head and shoulders of the child looking up,
   sad: eyebrows tilted, mouth turned down, no tears. In the top corner,
   leafy branches with the kite stuck in them. (feeling)
4. `[ANCHOR] [STYLE] [KITE]` Same grass and tree. The grandparent from the
   doorway picture reaches up with a long garden rake and hooks the kite,
   which is still in the tree. The child stands beside them pointing up.
   (action)
5. `[ANCHOR] [STYLE] [KITE]` Same grass and tree, nothing in the tree. The
   child hugs the kite with both arms, smiling. The grandparent stands
   beside them holding the rake upright, smiling. (ending, feeling)

Cards 1 and 5 both show a happy child with the kite: card 5 must have the
grandparent and the rake, and the kite held, not flying.

Notes from generating this set (2026-11-17):

- State sizes that matter to the story. The first tree came out shorter than
  the child, so a stuck kite was within reach; "about twice as tall as the
  child" fixed it.
- "Exactly one tree, exactly one kite" stopped Gemini adding a second tree.
- An "Edit the previous image" prompt sometimes returns nothing. Asking for
  "a new image in the same series, using the last picture as the scene" and
  listing what is different worked every time.
- Say where a character is looking, and how: "seen in side view facing the
  tree, head tilted back, eyes on the kite". Without it the child faces the
  viewer.
- The limit ran out after about 10 images this time and reset at 9:30 AM.
- Cards 2 and 3 were redone after the reset so the child is clearly looking
  at the kite. Both worked first time with the side-view wording above;
  card 3 was asked for as "a closer view of the last picture", which kept
  the tree, kite and child identical to card 2.
- The kite sits at the very top of cards 2 and 3, and the app used to trim
  the top of each picture. Cards with a `paper` colour now show the whole
  picture, so nothing is cut off.

---

# Part 3: story grammar stories (character, problem, feeling, action, ending)

Four short stories with a character, a problem, a feeling, an action and an
ending: `kite-in-the-tree`, `spilled-paint`, `lost-library-book`,
`block-tower`. They are already in `stories.json` with emoji and text; each
card names its story part (`"part"`). This part makes their cartoon sets.

A story needs the same people, objects and place in every card, at the same
size. Gemini keeps those far better when it is shown them than when it is
told, so this part makes **reference images first** and attaches them to
every card prompt.

## Budget

About 6 reference images and 21 cards: two evenings at the free limit (about
19 images). `spilled-paint` is now the Part 4 trial (about 13 images on its
own). Evening 1: the references, then `kite-in-the-tree`. Evening 2: `lost-library-book` and `block-tower`. Save every
reference image in `img/_refs/` (not used by the app) so evening 2 can attach
the same files.

## Settings stay quiet

Each setting is a few simple pieces of furniture or scenery drawn on the
plain cream background, the same way `mailing-a-letter/05` draws a door
frame: no wall colour, no sky fill, no patterned floor. This keeps the cards
calm and lets `paper.cartoon` blend the caption into the picture.

## SCALE block

Paste after STYLE in every card prompt, word for word.

```
Scale: the child is about 7 years old. In full-body pictures the child
fills about two-thirds of the picture height, with their feet about a
quarter of the way up from the bottom edge. In waist-up pictures the
child's head and shoulders fill the top half. Every object is the same
size as in the attached size line-up. Camera at the child's eye level,
looking straight on, exactly as in the attached setting picture.
```

## REFS block

Start every card prompt with this. Attach the files it names.

```
Use the attached images as references. The child, the grandparent and the
puppy must look exactly as in their reference pictures: same face, hair,
clothes, outline and colours. Objects keep the sizes in the size line-up.
The place matches the attached setting picture: same furniture, same
positions, same camera. New picture:
```

## Reference images

R1 and R2 are made in one new chat. Attach the child anchor (any full-body
card with the child, or the anchor if kept) to R1 and R2.

**R0, the grandparent.** Use `img/mailing-a-letter/cartoon/05.jpg` as it is.
In prompts call them "the grandparent": grey curly hair, round glasses,
dusty-blue cardigan, tan trousers.

**R1, the puppy.**
`[STYLE]` A small tan puppy with floppy brown ears, a short wagging tail and
a sage-green collar, standing side-on facing right, whole body visible. Calm,
friendly face. Nothing else in the picture.

**R2, the size line-up.** Attach the child anchor, R0 and R1.
`[STYLE]` Landscape 4:3 for this one picture only. A row of things standing
on one flat floor line, evenly spaced, all the same distance from the
viewer. Left to right: the child from the attached picture, standing; the
grandparent from the attached picture, standing, one head taller than the
child; the puppy from the attached picture, its back level with the child's
knee; a dusty-blue diamond kite with a tan bow tail, propped up, as wide as
the child's shoulders; a garden rake with a long wooden handle, standing
upright, its top level with the grandparent's head; a small clear cup of
water, as tall as the child's hand; one wooden toy block, the size of the
child's fist; a red book with a simple white whale on the cover and no words,
as big as the child's two hands side by side. No text or labels.

Check R2 before going on: if any size is off, regenerate it. Every card
depends on it.

**Setting plates S1 to S4.** One each, in the same chat, no people. Each one
sets the camera for its story.

- **S1, park.** `[STYLE]` A low, gently rounded grassy hill across the
  bottom third of the picture. One big round tree on the right, its trunk on
  the hill and its crown reaching near the top edge. Open cream space on the
  left half above the hill. No people, no kite.
- **S2, painting table.** `[STYLE]` A small wooden table seen from the
  side, its top a third of the way up the picture, with one low stool on the
  left. A simple window frame on the wall area above the right half of the
  table. Nothing on the table. No people.
- **S3, bedroom.** `[STYLE]` A low single bed along the bottom left, with
  a sage-green blanket and a dark gap under it. A small bookshelf on the
  right with one shelf of four books standing up and one empty gap between
  them. A tan backpack lying on the floor in front of the shelf. No people.
- **S4, living room.** `[STYLE]` A low dusty-blue sofa across the back,
  its seat a third of the way up the picture. A round tan rug on the floor in
  front of it, filling the bottom third. Nothing on the rug. No people.

## kite-in-the-tree (5 cards)

Attach the child anchor, R0, R2 and S1 to every card.

1. `[REFS] [STYLE] [SCALE]` *character* The child stands on the hill on the
   left, full body, holding a kite string in one raised hand, smiling. The
   string runs up to the kite flying high in the open space at the top left.
   The tree is empty.
2. `[REFS] [STYLE] [SCALE]` *problem* The kite is caught in the top of the
   tree's crown, tilted, its tail tangled in the leaves. The string hangs
   down from it to the ground. The child stands on the left looking up, mouth
   open in surprise. Nobody else is there.
3. `[REFS] [STYLE] [SCALE]` *feeling* Waist-up close view of the child,
   looking up and to the right, with a sad face: eyebrows raised in the
   middle, mouth turned down, one tear. Only a few tree leaves at the top
   right corner. No kite visible.
4. `[REFS] [STYLE] [SCALE]` *action* The grandparent stands under the tree
   and lifts the rake high, its teeth hooking the kite in the crown. The
   child stands beside the grandparent, pointing up at the kite.
5. `[REFS] [STYLE] [SCALE]` *ending* The child holds the kite against their
   chest in both hands, smiling widely. The grandparent stands beside them
   holding the rake upright, smiling. The tree is empty again.

## spilled-paint (5 cards)

Moved to Part 4: this story is the trial for the cast bible, with Ana as her
own character. Use the prompts there.

## lost-library-book (6 cards)

Attach the child anchor, R2 and S3 to every card. The red whale book is the
only red thing in the room.

1. `[REFS] [STYLE] [SCALE]` *setting* The bedroom from S3. The child
   stands in the middle, full body, holding the empty backpack open with
   both hands, looking at the viewer. The gap on the shelf is empty. No book
   anywhere.
2. `[REFS] [STYLE] [SCALE]` *problem* The child stands in front of the
   shelf, pointing at the empty gap between the books. Puzzled face. The
   backpack lies on the floor.
3. `[REFS] [STYLE] [SCALE]` *feeling* Waist-up close view of the child, one
   hand on top of their head, eyebrows pulled together, mouth a small wavy
   line: worried. The shelf is a plain edge behind them.
4. `[REFS] [STYLE] [SCALE]` *action* The child lies on their tummy on the
   floor beside the bed, shining a yellow flashlight into the dark gap under
   it. A pale beam of light goes under the bed. The book cannot be seen yet.
5. `[REFS] [STYLE] [SCALE]` *ending* The child kneels beside the bed and
   holds the red whale book up high in one hand, mouth open in a happy
   surprise. The flashlight lies on the floor.
6. `[REFS] [STYLE] [SCALE]` *end feeling* The child stands beside the
   backpack and slides the red book into it. Shoulders relaxed, eyes soft,
   a gentle smile: relieved. The gap on the shelf is still empty, because the
   book is going back to the library.

## block-tower (5 cards)

Attach the child anchor, R1, R2 and S4 to every card. The tower is always
six blocks stacked straight up, as tall as the kneeling child's shoulder.

1. `[REFS] [STYLE] [SCALE]` *character* The child kneels on the rug on the
   left, placing the sixth block on top of a straight tower of six wooden
   blocks in the middle of the rug. Proud smile. The puppy sits on the right
   edge of the rug, watching.
2. `[REFS] [STYLE] [SCALE]` *problem* The blocks lie scattered across the
   rug. The puppy stands where the tower was, tail wagging, looking happy.
   The child kneels on the left, mouth open, hands up in surprise.
3. `[REFS] [STYLE] [SCALE]` *feeling* Waist-up close view of the child,
   both hands in fists at their sides, eyebrows pulled down, lips pressed
   together: frustrated, not shouting. A few blocks at the bottom edge.
4. `[REFS] [STYLE] [SCALE]` *action* The child kneels with cheeks puffed,
   breathing out slowly (three small curved breath lines in front of the
   mouth), and rolls a sage-green ball across the rug to the puppy on the
   right. The blocks are still scattered.
5. `[REFS] [STYLE] [SCALE]` *ending* The six-block tower stands again in
   the middle of the rug. The child kneels beside it, calm and smiling. The
   puppy lies on the right edge of the rug, chewing the green ball.

## Checks before saving a set

- Same child, same clothes, in every card. Regenerate a card that drifts.
- Neighbouring cards look clearly different at thumbnail size. The feeling
  card is always a waist-up close view, so it never looks like the problem
  card before it.
- The size line-up holds: kite about shoulder-wide, book two hands big,
  puppy knee-high, blocks fist-sized.
- No text anywhere, including on the book cover and the painting.
- Then follow the saving steps in "Rules for every set" above (resize to
  640 wide, `img`, `cartoon` in `styles`, `paper.cartoon`, refresh the
  built-in copy).

---

# Part 4: the cast bible

A small set of characters, each drawn once in detail and then reused. Every
card attaches the exact sheet it needs ("Ana, upset"), so faces and sizes
come from a picture, not from words. The sheets are also teaching cards for
story grammar: **character**, **setting**, the **props** and the
**feelings**, shown before the story is told.

The painting story (`spilled-paint`) is the trial. If Ana stays the same
across its cards, the other stories get their own characters the same way.

## The cast (planned)

| Who | Stories | Status |
|---|---|---|
| Ana | spilled-paint | trial, below |
| Sam | kite-in-the-tree | after the trial |
| Jo | lost-library-book | after the trial |
| Max and the puppy | block-tower | after the trial |
| the grandparent | kite-in-the-tree | `mailing-a-letter/05` is the reference |

Each child looks clearly different (hair, skin tone, clothes colour) so the
names in the captions mean something. Keep the cast small: every extra
character costs a set of sheets and makes it easier for faces to drift.

## Rules for character sheets

- **One chat per character.** Make the sheets in this order, each attaching
  the ones before. If files can't be attached (for example when Claude is
  pasting the prompts), keep the whole trial in that one chat and swap
  "the attached X" for "X earlier in this chat", which is how the Part 2
  child stayed consistent. Attach a file by hand only if something drifts.
- **Grids, then crop.** Faces drawn together in one grid image match each
  other better than faces made one at a time. Crop each cell into its own
  file afterwards (see "Files" below).
- **Feelings show in the body as well as the face.** Hands, shoulders and
  posture carry the feeling too, because many children (autistic children
  especially) don't read or show feelings mainly through the face. Feeling
  cards are words for the story character, never labels for the student.
- **Coping shown as ordinary.** A deep breath, headphones, a fidget, a quiet
  corner, flapping or rocking when excited: drawn calmly and plainly, never
  as a problem.
- **No text** in any sheet: no labels under cells, no captions.

## Ana

About 7 years old. Warm brown skin, black hair in two round puffs tied with
sage-green bands, a round face, dark eyes. Dusty-blue overalls over a cream
t-shirt, tan sneakers. She looks clearly different from the anchor child
(short dark curls, sage t-shirt).

### A1, Ana turnaround

`[STYLE]` Landscape 4:3 for this sheet only. A character reference sheet:
the same girl drawn three times side by side at exactly the same size,
standing on one floor line. Left: facing forward. Middle: facing right,
side view. Right: from behind. She is about 7 years old, with warm brown
skin, black hair in two round puffs tied with sage-green bands, a round
face and dark eyes. She wears dusty-blue overalls over a cream t-shirt and
tan sneakers. Relaxed pose, arms at her sides, calm friendly face. No text.

### A2, Ana feelings grid

Attach A1.

`[STYLE]` Square 1:1 for this sheet only. A 3 by 3 grid of nine equal
cells, separated only by plain cream space (no lines). In every cell, the
same girl from the attached sheet, head and shoulders, facing forward, at
exactly the same size and position. Same hair, skin, clothes and outline in
every cell. Only the face, hands and shoulders change. Row 1: calm (soft
smile, relaxed shoulders); happy (big smile, eyes crinkled); surprised (eyes
wide, mouth a small round O, eyebrows high). Row 2: focused (eyes looking
down, lips pressed lightly, tongue tip just showing at the corner); upset
(eyebrows pulled together, mouth turned down, both hands on her cheeks);
worried (eyebrows raised in the middle, mouth a small wavy line, one hand
holding the other). Row 3: frustrated (eyebrows pulled down, lips pressed
tight, shoulders up); proud (chin up, closed-mouth smile, one hand on her
chest); relieved (eyes half closed, a small smile, shoulders dropped low).
No text or labels.

Check: same girl in all nine? Can each feeling be told apart at thumbnail
size? Regenerate the whole grid rather than fixing one cell.

### A3, Ana poses for this story

Attach A1.

`[STYLE]` Landscape 4:3 for this sheet only. The same girl from the
attached sheet drawn four times side by side at the same size, all on one
floor line. 1: sitting on a low stool, side view facing right, one arm
reaching forward holding a paintbrush. 2: standing, side view facing right,
both arms pulled back in surprise. 3: standing, leaning forward over an
unseen table, brush in hand. 4: standing facing forward, both arms up in
front of her chest as if holding up a sheet of paper (no paper). No text.

## Props and the painting

### P1, painting props line-up

Attach A1.

`[STYLE]` Landscape 4:3 for this sheet only. On one floor line, evenly
spaced, all seen from the side at the same distance: the girl from the
attached sheet standing, for scale; a small wooden table whose top reaches
her waist; a low wooden stool whose seat reaches her knee; on the table, a
clear cup of blue-tinted water as tall as her hand; two paintbrushes as
long as her forearm; a small white paint tray with red, yellow and blue
paint; a sheet of white paper lying flat, as wide as her shoulders. Beside
the table on the floor, the same cup tipped on its side with a small blue
puddle. No text.

### P2, the painting in four stages (edit chain)

The painting is drawn flat and face-on, filling the picture, so it can be
copied onto the paper in each card. Make 1, then edit for 2, 3 and 4 with
the EDIT block from Part 1.

1. `[STYLE]` A child's painting on a sheet of white paper, seen straight on,
   the paper filling the picture with a thin cream margin. Painted in simple
   brush strokes: a small house with a red roof, a yellow door and one
   window, standing on a green line of grass in the upper half. The bottom
   half of the paper is blank white. No text, no signature.
2. `[EDIT]` A wide puddle of blue water now covers the bottom half of the
   paper, with soft wavy edges, a few drips reaching the bottom edge. The
   house is untouched.
3. `[EDIT]` On the blue area, one small boat with a brown hull and a white
   triangle sail is painted on the left. A second boat on the right is only
   half painted: hull only, no sail.
4. `[EDIT]` The second boat now has its sail. Add three short blue wave
   lines between the boats. The blue area now reads as a lake.

### S2, the painting table

Use the S2 prompt in Part 3 (table, stool, window, nothing on the table),
and attach P1 so the table and stool match.

## spilled-paint cards (trial)

Every card attaches A1, the A2 cell it names, the A3 pose it names, P1, S2
and the P2 stage it names. Start each prompt with CAST, then STYLE and
SCALE (Part 3).

```
Use the attached images as references. The girl must look exactly as in
her character sheet: same face, hair, skin, clothes, outline and colours.
Her face and body show the feeling in the attached feeling picture. Her pose
follows the attached pose picture. The table, stool, cup and brushes match
the attached props sheet in shape and size. The room matches the attached
setting. The painting on the paper is the attached painting, seen at the
angle of the paper in this picture. New picture:
```

1. *character* (A2 focused, A3 pose 1, P2 stage 1) `[CAST] [STYLE] [SCALE]`
   Ana sits on the stool at the table, side view facing right, painting. The
   paper lies flat on the table, tilted toward the viewer so the house shows.
   The cup of water stands upright to the right of the paper, two brushes
   beside it.
2. *problem* (A2 surprised, A3 pose 2, P2 stage 2) `[CAST] [STYLE]
   [SCALE]` Same view. Ana has jumped up beside the stool, arms pulled back.
   The cup lies on its side; blue water runs from it over the bottom half of
   the paper and drips off the table edge.
3. *feeling* (A2 upset) `[CAST] [STYLE] [SCALE]` Waist-up close view of
   Ana, facing forward, hands on her cheeks, looking down at the wet paper
   along the bottom edge of the picture. The window and table are only an
   edge behind her.
4. *action* (A2 focused, A3 pose 3, P2 stage 3) `[CAST] [STYLE] [SCALE]`
   Same view as card 1. The cup stands upright again. Ana leans over the
   paper and paints the second boat on the blue.
5. *ending* (A2 proud, A3 pose 4, P2 stage 4) `[CAST] [STYLE] [SCALE]`
   Ana stands beside the table facing forward, holding the finished
   painting up in front of her chest with both hands so the viewer sees it
   straight on: house, lake, two boats.

## Later sheets for each character

After the trial, each character gets the same A1 to A3 sheets, plus:

- **Energy row** (5 full-body figures): sleepy (eyes half closed, slumped),
  calm (relaxed, standing still), alert (upright, eyes open, ready), excited
  (bouncing on toes, hands flapping), overwhelmed (hands over ears, eyes
  shut). This can become a reversible concept story, like voice volume.
- **Body needs** (grid): hungry, tired, too loud, cold, unwell.
- **Coping** (grid): a deep breath, headphones on, squeezing a fidget,
  sitting in a quiet corner with a cushion, rocking calmly, asking a grown-up
  for help.

## Files

Drafts are kept out of the app until they pass review:

```
img/_drafts/spilled-paint/round-1/A1-turnaround.jpg
                                  A2-feelings.jpg       (whole grid)
                                  A2-feelings-upset.jpg (cropped cells)
                                  A3-poses.jpg
                                  P1-props.jpg
                                  P2-painting-1.jpg … 4
                                  S2-table.jpg
                                  card-01.jpg … card-05.jpg
```

Each round is a new folder, so earlier versions stay for comparison. Accepted
cards are copied to `img/spilled-paint/cartoon/01.jpg…` at 640 wide, then
follow the saving steps in "Rules for every set". Accepted sheets go to
`img/_cast/ana/`.

## Prompt log

One entry per round: what was tried, what came back, what changed and why.
This is how the prompts get better.

| Round | Image | Feedback | Prompt change |
|---|---|---|---|
| 1 | A1 turnaround | On model; Gemini added a thin floor line. | None. |
| 1 | A2 feelings | All nine faces match. "Focused" reads cheeky (tongue out, eyes sideways); "worried" has a hand on the chest, not hands held; grid has thin white gutters (fine for cropping). | Pending your review. Possible: "focused: eyes down on her work, mouth closed, no tongue". |
| 1 | A3 poses | Figures overlap; "surprised" arms not pulled back. | Pending your review. Possible: "wide even gaps between figures". |
| 1 | P1 props | Sizes right (table at waist, stool at knee). First try came back as text only; "Please generate … as an image now" fixed it. | Start every prompt with "Generate an image." |
| 1 | P2 stage 1, 2 | Clean; the spill edit kept the house exactly. Stage 2 is still in the Gemini chat, not saved (download didn't start once the limit hit). | None. |

| 1 | card-01 to 05 (Codex) | All five are the same Ana. Cards 1, 2, 4, 5 share the table, camera and scale (card 2 as the layout reference worked). Card 3 is a close view of an upset face; a corner of the wet paper shows what she's looking at. **Continuity:** card 1 has the stool on the left, where she sits; cards 2, 4, 5 have it on the right. Card 4 adds a thin floor line. About 33–39k Codex tokens per card. | Pending your review. |

| 1 | spilled-paint installed | Cards padded evenly for the caption crop, then saved to `img/spilled-paint/cartoon/`; Ana's sheets kept in `img/_cast/ana/`. | Caption-safe band added to the STAGE notes. |
| 1 | kite-in-the-tree (Codex) | Sam and Gran turnarounds made first (Gran redrawn from `mailing-a-letter/05`). Card 2 made alone as the master layout, then 1, 3, 4, 5 together from it: same tree, hill and scale in all. Gran is only a little taller than Sam in card 4. 7 images, about 170k Codex tokens in all. | Pending your review. |

| 1 | kite-in-the-tree installed | Padded by stretching edge colours (cream above, grass below), same amount on cards 1, 2, 4, 5. Card 3 lost the kite to the caption crop; padding it looked wrong, so it was redrawn with the kite inside the safe band (the tree became a smaller one there). | Write the safe band into every brief, close views included. |
| 1 | lost-library-book (Codex) | Jo sheet, then card 2 as layout. First card 2 had an angry face and pointed at a book; redone with the first as the layout, and cards 1, 3–6 built from that layout. Red book is the only red thing; the shelf gap stays empty to the end. 8 images, about 150k tokens. | Pending your review. |

| 1 | lost-library-book fixes | Card 5 flashlight was blue (card 4 yellow): recoloured in place with PIL, no new image. Card 3 shelf books were thin: redrawn at Jo's scale. Card 6 backpack had a phantom second flap: redrawn to match card 2's single open compartment. Installed. | Name small props' colours in every brief they appear in (flashlight: yellow). |
| 1 | block-tower (Codex) | Max and puppy sheets, card 2 master, then 1, 3, 4, 5. Six blocks and the sage ball hold across cards. Card 3's scowl may read angry rather than frustrated. About 160k tokens. | Pending your review. |
| 1 | Jo feelings F1–F3 (Codex) | Three 3×3 sheets, same nine feelings and order as Ana's A2: subtle, exaggerated, and lines-and-colour (one muted tint and mark per feeling). Briefs attach Jo's A1 and library card 3. All three hold Jo and an even grid. "Focused" reads close to frustrated in all three (eyebrows angled down); F2 relieved's open mouth can read as a yawn; F2 calm and proud are both eyes-closed smiles. About 28k tokens each. Review page: `img/_drafts/jo-feelings/round-1/`. | Pending your review. |
| 2 | Jo feelings F1–F3 (Codex) | Edit of round 1 (the old sheet attached as Image 3, "change only what is listed"): other cells held. Focused now level brows, eyes on hands, in all three. F2/F3 noses were dots: named the nose "a small curved line, like a c, exactly like Image 1", fixed everywhere. F2 relieved now wipes the forehead, mouth closed. F2 calm first came back smug (one brow up, lopsided smile); a one-cell edit made it closed-eyed and even. F3 frustrated came back almost the same pink as proud; recoloured to a pale brick with PIL. About 30k tokens a sheet. | Name the nose in every face brief. Describe calm as "both brows level, same height, symmetrical smile". When asking a tint to be paler, also name what it must stay apart from. |
| 1 | Cast feelings F1–F3 (Codex) | Ana, Sam, Max, the grandparent and the puppy, three sheets each, with Jo's round 2 sheets attached as the layout reference and the round 2 fixes written into every brief. All 15 held the character and the even grid on the first try; noses are lines, focused reads as concentrating, frustrated stays apart from proud. The puppy uses dog body language. The ChatGPT limit hit after Ana; the rest ran after the reset. 14k–62k tokens a sheet; the spread is how much of the image skill's reference files Codex chose to read. Cut into 162 single pictures for the feelings match. | Try the lean helper next: skip the skill files and use low reasoning; the image model and brief are unchanged. |
| 1 | puppy-at-the-door (Codex, lean helper) | Card 2 (split card) as the master, attaching Jo, Gran and the puppy plus the feelings-sheet cells for the two faces; then 1, 3, 4 from it and 5, 6, 7 in their own settings (7 reuses block-tower card 2 for the rug and sofa). All seven on the first try: same hallway, door and sign; Gran looks at the sign, not at Jo, in 3 and 4. **Lean helper** (skip the skill files, low reasoning, brief passed word for word): 7.5k–27k tokens a card, about 128k for all seven, against 20–35k a card before, with no visible loss of quality. Possible note: in card 3 Gran points while frowning, which could read as scolding. | Use the lean helper by default. Attach feelings-sheet cells for faces on feeling cards. |
| 1 | puppy-at-the-door fixes, installed | Card 3: Gran's glasses floated off the face in side view; a one-change Codex edit sat them on the nose. Card 7: the puppy had no nose; the Codex edit added one but also erased its brown eye patch, so the nose was drawn onto the original with PIL instead. Installed: the six full scenes share one crop, the close-up (card 4) its own top trim; paper #f7eee3. | Edits can drop a character's marks: name the marks to keep ("keep the brown patch around one eye"), or fix tiny things locally. |

### Workflow notes from round 1

- The free limit hit after **6 images** this time (resets 2:30 PM), not ~19.
  Budget a round as two sittings, or one stage at a time.
- Return sends the prompt even when the screen hasn't updated yet. Don't
  press the arrow after it: it turns into Stop.
- After each download the prompt box moves to the bottom of the page; click
  there before typing.
- Downloads land in ~/Downloads as `Gemini_Generated_Image_*.jpeg` and are
  moved straight into the round folder.

## Making cards with Codex (from round 1 on)

Gemini's free tier ran out after 6 images, and its web app can't take
reference files from Claude. Codex's image tool can: each reference is
attached as a file and named in the prompt ("Image 1 is …"), which is what
keeps Ana's face steady. One story comes from one tool, so every
`spilled-paint` card is made in Codex.

Claude runs each card with `codex exec -i <refs…>` and the brief below.
Codex saves one image to the round folder and changes nothing else.

### CAST block (Ana)

```
Match the supplied Ana reference exactly: a seven-year-old girl with warm
brown skin, round face, dark oval eyes, black hair parted in the middle in
two round puffs with sage-green bands, dusty-blue overalls with a front
pocket over a cream short-sleeve t-shirt, tan sneakers. Preserve her age,
skin tone, hair silhouette, proportions and clothing design. Flat simple
children's picture-card illustration, even medium-brown outlines, muted
warm tan, cream, sage and dusty-blue palette, plain uniform cream #f7f2e7
background, solid fills, no shading, gradients or shadows. No text,
lettering, numbers, arrows, logos, frames or watermark.
```

### STAGE block (every full-scene card)

The master layout is card 2 (the spill). Attach it to every other
full-scene card as the layout reference.

```
Portrait 3:4. Match the layout reference exactly: same camera height and
angle, same table and stool in the same place and at the same size, same
floor level. When Ana stands, her head top is about 15% from the top edge
and her shoes rest about 12% above the bottom edge, so she fills about
three-quarters of the card height. The table top is level with her waist.
Generous plain cream space around everything.
```

Card 3 (feeling) is a waist-up close view and uses only CAST plus the
matching A2 cell.

**Trim at install.** Cards with a `paper` colour now show the whole picture
(no crop), so the padding used in round 1 is gone. Instead each set is
trimmed to the band its scenes actually use, so the drawing fills the card:
the full-scene cards of a story share one crop (full width, same top and
bottom, so scale never changes between them) with about 30 px of margin;
the close-up feeling card gets its own top trim. Then resize to 640 wide.
Codex leaves 20–35% empty cream above most scenes, so this roughly doubles
the drawing's size on the card.

### References per card

| Card | Attach | Feeling |
|---|---|---|
| 1 character | A1, P1, P2-1, card 2 (layout) | calm, focused on her work |
| 2 problem | A1, P1, P2-1 | surprised (master layout) |
| 3 feeling | A1, A2 | upset |
| 4 action | A1, P1, P2-2, card 2 (layout) | calm, thinking it through |
| 5 ending | A1, P1, P2-1, card 2 (layout) | proud |

---

# Part 5: feelings sheets and a two-feelings story

## Feelings sheets (F1–F3)

Every cast member gets three 3×3 feelings sheets with the same nine
feelings in the same cells: calm, happy, surprised / focused, upset,
worried / frustrated, proud, relieved.

| Sheet | What changes |
|---|---|
| F1 subtle | Small everyday changes; some pairs sit close on purpose. |
| F2 exaggerated | Picture-book big; the whole upper body joins in. |
| F3 lines and colour | Medium face plus one pale tint and simple marks per feeling. |

Jo's round 2 sheets are the layout reference for everyone else (attach as
Image 2, "use only for the grid, poses, marks and colours"), so a feeling
has the same pose across the cast. Each sheet has one brief template
(Jo's, with the round 2 fixes written in) filled with the character's
description; the puppy gets dog body language instead (ears, tail, whole
body). Drafts:
`img/_drafts/jo-feelings/`, `img/_drafts/cast-feelings/`. Lessons from
Jo's rounds are in the prompt log: name the nose, describe calm as level
brows with a symmetrical smile, and say what a pale tint must stay apart
from.

The cells double as references for story cards: crop the cell you need
("Gran, worried, F1") and attach it with the character sheet.

## puppy-at-the-door (7 cards, 3-5)

Two characters feel differently about the same event. It's also how the
puppy in `block-tower` came to Max's family.

**The split card rule.** When two people feel something at the same
moment, both go on one card: Jo on the left, Gran on the right, one
scene. Two separate cards would let the student put either first, and the
app would count that free choice as a swap.

| # | Part | Caption | Picture |
|---|---|---|---|
| 1 | setting | Jo finds a puppy outside Gran's door | Hallway. Gran's door half open, Jo kneeling on the doormat beside the puppy, which sits looking up. |
| 2 | feeling | Jo is excited. Gran is worried. | Split: Jo (left) hugs the puppy, big smile; Gran (right) in the doorway, hands held together, eyebrows raised in the middle. |
| 3 | problem | Gran's building has a no-pets rule | Gran points to a small sign by the door: a paw print in a circle with one line through it (a symbol, no words). Jo holds the puppy. |
| 4 | feeling | Jo is upset. Gran is frustrated. | Split, waist up: Jo (left) hands on cheeks, mouth down; Gran (right) brows down, lips pressed, arms crossed, looking at the sign, not at Jo. |
| 5 | action | Jo and Gran make posters to find the puppy a home | Kitchen table: both drawing posters with a puppy picture (no words), the puppy asleep in a basket. |
| 6 | ending | Max's family gives the puppy a home | Max's front door (a different, mustard door): Max kneels and hugs the puppy; Jo and Gran stand on the step. |
| 7 | end feeling | Jo visits the puppy. Jo and Gran feel relieved. | Max's living-room rug: the puppy plays with Jo and Max; Gran sits on the sofa, shoulders low, soft smile. |

Gran's frustration is with the rule, never with Jo: Gran looks at the
sign, not at the child. Card 7's rug and sofa are the same as
`block-tower`, so the two stories meet.

### Settings

- **S-hall** (cards 1–3): a building hallway seen straight on. A sage
  door with a brass handle and a doormat; plain cream wall; the no-pets
  sign at the child's head height beside the door (shown in card 3, small
  and plain in 1–2).
- **S-kitchen** (card 5): a small table seen from the side, two chairs,
  a basket on the floor.
- **S-max** (card 6): a mustard front door with one step.
- **block-tower rug** (card 7): reuse `img/block-tower/cartoon/02.jpg` as
  the layout.

### Making it

1. Master layout: card 2 (the first split card), with Jo A1, Gran A1,
   puppy A1, and the cells "Jo happy F1" and "Gran worried F1".
2. Cards 1 and 3 from card 2 (same hallway, camera and scale).
3. Card 4 as a waist-up close view (like the other feeling cards), with
   "Jo upset" and "Gran frustrated" cells.
4. Cards 5, 6 and 7 with their own settings; card 7 also attaches
   block-tower card 2 and Max A1.
5. Review page, then install as in Part 4 (trim at install, 640 wide,
   `img` names, `cartoon` in `styles`, `paper.cartoon`, refresh the
   built-in copy).

About 8 images including one redo, roughly 250–300k Codex tokens. Wait
until Gran's feelings sheet exists so the two feeling cards can attach
Gran's cells.
