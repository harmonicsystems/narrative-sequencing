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

# Part 3: a full narrative (problem, feeling, action, ending)

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
