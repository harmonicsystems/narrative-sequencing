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
3. `[ANCHOR] [STYLE]` The child, waist up, holds the sealed envelope in one
   hand and presses a small red stamp onto its top corner with the other.
4. `[ANCHOR] [STYLE]` The child stands beside a blue street mailbox and
   pushes the stamped envelope into its slot. The envelope is half in.
5. `[STYLE]` A grey-haired grandparent stands in an open doorway holding
   the same envelope with the red stamp, now opened, and reads the sheet of
   paper from inside it with a smile. The child is not in this picture.

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
19 images). Evening 1: the references, then `kite-in-the-tree` and
`spilled-paint`. Evening 2: `lost-library-book` and `block-tower`. Save every
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

Attach the child anchor, R2 and S2 to every card. The drawing is always the
same sheet of paper at the same place on the table.

1. `[REFS] [STYLE] [SCALE]` *character* The child sits on the stool at the
   table, painting with a brush. On the paper: a simple house with a red
   roof, nothing below it. A clear cup of blue-tinted water and two brushes
   stand to the right of the paper. Calm, focused face.
2. `[REFS] [STYLE] [SCALE]` *problem* The cup lies on its side. Blue water
   runs out of it across the bottom half of the paper, a wide blue puddle
   under the house. The child's arm is pulled back, eyes wide.
3. `[REFS] [STYLE] [SCALE]` *feeling* Waist-up close view of the child,
   both hands on their cheeks, eyebrows pulled together, mouth turned down,
   looking down at the wet paper at the bottom of the picture.
4. `[REFS] [STYLE] [SCALE]` *action* The cup stands upright again. The
   child leans over the paper, brush in hand, and paints a small boat with
   a white sail on the blue patch. One boat is finished and one is half
   painted. Calm, thinking face.
5. `[REFS] [STYLE] [SCALE]` *ending* The child stands beside the table and
   holds the paper up in front of their chest with both hands, smiling
   proudly. On it: the house, a blue lake under it, two small boats.

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
