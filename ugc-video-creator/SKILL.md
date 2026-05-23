---
name: ugc-video-creator
description: Creates UGC video scripts and Higgsfield prompts per chunk. Supports user-provided scripts (Mode A) or AI-generated scripts (Mode B). Activate with /ugc-video-creator.
compatibility: claude-code-only
---

## Required inputs

```
PRODUCT_PHOTO     Path to product PNG (clean packshot, white/transparent bg)
PRODUCT_DOC       Path to product PDF (benefits, mechanism, tone, audience)
SCRIPT_MODE       A (your own script) or B (AI writes it)
VIDEO_FORMAT      full_stack (28-32s, 5 chunks) or mid_funnel (18-22s, 3 chunks)
OUTPUT_LANGUAGE   es / en / etc.
OUTPUT_LOCALE     MX / ES / US / etc.
OUTPUT_FOLDER     Local path (e.g. /Users/.../Desktop/UGC Output)
```

**Mode A only:**
```
SCRIPT           [paste your full voiceover script here]
```

**Mode B only:**
```
VARIATION_TYPE   confessional / tested_it / myth_buster / accidental_discovery / before_after
PAIN_POINT       [the specific frustration the hook calls out]
AVATAR_DOC       Path to avatar PDF (optional but recommended)
CREATOR_PROFILE  [gender, age range, vibe — e.g. "woman, late 30s, calm and relatable"]
```

---

## ⛔ HARDCODED RULES

1. **NO EM-DASHES IN VOICEOVER** — Em dashes (—) and en-dashes (–) break speech rhythm. Use commas and periods only.
2. **NO BOLDING IN VOICEOVER** — Has no function in audio. Plain text only.
3. **SOFT CTAs ONLY** — "I'll leave a link below" not "Buy now" or "Click here".
4. **PRODUCT APPEARS ONLY IN CORRECT CHUNK** — Full Stack: chunk 4 only. Mid-Funnel: chunk 2 only. Never in opening or closing chunks.
5. **SCRIPT APPROVAL GATE** — In Mode B, always show the full script and wait for user approval before building prompts. In Mode A, confirm you have read the full script before proceeding.
6. **ONE CHUNK = ONE HIGGSFIELD CALL** — Never combine chunks. N chunks = N video generations.
7. **VERIFY FILES** — After every file write, run `ls -la OUTPUT_FOLDER` to confirm.

---

## BLOCK 0 — Setup

1. Read PRODUCT_DOC → extract:
   - Product name and mechanism
   - Key benefit (the one most relevant to the pain point)
   - Tactile analogy (if one exists in the doc — the "what it feels like" description)
   - Brand name and pronunciation (flag any non-standard spelling)
   - Visual identity (packaging color, shape, size)

2. Upload PRODUCT_PHOTO to Higgsfield → store as `PRODUCT_MEDIA_ID`

3. Create OUTPUT_FOLDER and estado.md:
```
PRODUCT_MEDIA_ID: [id]
FORMAT: [full_stack / mid_funnel]
CHUNKS: [5 / 3]
SCRIPT_MODE: [A / B]

chunk_01: script ⬜ | prompt ⬜ | video ⬜ | file ⬜
chunk_02: script ⬜ | prompt ⬜ | video ⬜ | file ⬜
...
```

Print: `BLOCK 0 COMPLETE`

---

## BLOCK 1 — Script

### Mode A — User provides script

1. Read the SCRIPT input in full
2. Confirm word count and estimated runtime:
   - Full Stack: 150-180 words / 28-32 seconds
   - Mid-Funnel: 55-70 words / 18-22 seconds
3. Flag any issues (em-dashes, bolding, hard CTAs, words AI voiceover tools mispronounce)
4. Print: `SCRIPT RECEIVED — [N] words, ~[N] seconds estimated`
5. Proceed to Block 2

### Mode B — AI writes the script

Read `references/ugc-framework.md` before writing anything.

Write the script following this structure:

**Full Stack (5 beats):**
1. **Hook** (first 5 words identify the exact person + frustration)
2. **Reframe** (why the surface-level solution fails)
3. **Mechanism** (what actually works + tactile analogy — most important sentence)
4. **Payoff** (sensory, specific description of life after)
5. **Soft CTA** (suggestion-style, never direct sales)

**Mid-Funnel (3 beats):**
1. **Sharp hook + reframe folded in**
2. **Mechanism + analogy**
3. **Soft payoff + close**

Rules:
- First 5 words call out the specific person and their frustration
- No two-word stutter patterns ("No chemicals. No scents. No replacing." → WRONG)
- Mechanism paired with a tactile analogy a 12-year-old understands
- Payoff is sensory and specific, not generic ("I sleep through the night now" not "I feel better")
- No em-dashes, no bolding, no direct CTAs

Show full script to user. Wait for approval or edits before continuing.

Print: `BLOCK 1 COMPLETE`

---

## BLOCK 2 — Chunk breakdown

Divide the script into chunks following the format:

**Full Stack — 5 chunks:**
```
Chunk 1 — HOOK (4-5 sec)         Product: NO
Chunk 2 — REFRAME pt1 (5-6 sec)  Product: NO
Chunk 3 — REFRAME pt2 (5-6 sec)  Product: NO
Chunk 4 — MECHANISM + REVEAL (8-10 sec) Product: YES ← first and only appearance
Chunk 5 — PAYOFF + CTA (5-6 sec) Product: NO
```

**Mid-Funnel — 3 chunks:**
```
Chunk 1 — HOOK + REFRAME (6-8 sec)  Product: NO
Chunk 2 — MECHANISM + REVEAL (7-9 sec) Product: YES ← first and only appearance
Chunk 3 — PAYOFF + CTA (5-6 sec)   Product: NO
```

For each chunk print:
```
CHUNK [N] — [BEAT NAME]
Voiceover: "[exact lines for this chunk]"
Runtime: ~[N] seconds
Product appears: YES / NO
```

Update estado.md: `chunk_0N: script ✅`

Print: `BLOCK 2 COMPLETE`

---

## BLOCK 3 — Build Higgsfield prompts

For each chunk, build the complete video generation prompt.

**Character lock (paste into every chunk):**
Based on CREATOR_PROFILE (Mode B) or inferred from PRODUCT_DOC audience (Mode A):
```
[GENDER] in their [AGE RANGE], [VIBE — 2-3 words], [HAIR DESCRIPTION], wearing [OUTFIT].
Natural look, [MAKEUP LEVEL]. In [SETTING WITH LIGHTING].
```

**Four universal direction blocks (paste into every chunk):**

**Skin/Hair/Body Direction Block:**
```
Important direction: The creator has naturally [beautiful/healthy/clear] [skin/hair/teeth].
No visible [CONDITION THE PRODUCT TREATS] at any point.
She has the look of someone who has been using the product for months, because she has.
This applies to every scene in this chunk.
```
Adapt to product category: skin → hair → teeth → home → etc.

**Application Direction Block** (only in chunk where product appears):
```
Application is effortless and satisfying. Product applies evenly, leaves no residue,
no excess, no mess. The action reads as practiced, not performed.
Tight shot of product in hand, then application to [target area], then immediate result visible.
```

**B-Roll Sequencing Block:**
```
Opening shot: NO product visible. Creator in natural movement, not posed.
Cut rhythm: every [3-4] seconds. Avoid static shots longer than 4 seconds.
No product until voiceover names it.
```

**UGC Realism Direction Block:**
```
Phone-camera feel throughout. Slight natural movement, not tripod-locked.
No professional studio lighting. Natural [window/lamp/outdoor] light.
Creator is not performing — she is speaking directly to one specific person.
Blink naturally. No held smiles.
```

**Per-chunk visual direction:**

```
CHUNK [N] PROMPT:

[CHARACTER LOCK]

[SKIN/HAIR/BODY DIRECTION BLOCK]
[B-ROLL SEQUENCING BLOCK]
[UGC REALISM DIRECTION BLOCK]
[APPLICATION DIRECTION BLOCK — only in product reveal chunk]

Camera: [handheld drift / phone propped on counter / etc.]
Setting: [specific — e.g. "morning kitchen, soft window light from left"]
Product in frame: [YES — at second [N] when voiceover says "[trigger word]"] / [NO]

Voiceover for this chunk:
"[exact lines]"
```

Update estado.md: `chunk_0N: script ✅ | prompt ✅`

Print: `BLOCK 3 COMPLETE`

---

## BLOCK 4 — Generate in Higgsfield

For each chunk, call generate_video:

**Chunks WITHOUT product:**
```
Model: [best available video model — check with models_explore]
Prompt: [from Block 3]
Duration: [runtime from Block 2]
```

**Chunk WITH product:**
```
Model: [best available video model]
Medias: [{ value: PRODUCT_MEDIA_ID, role: "image" }]
Prompt: [from Block 3]
Duration: [runtime from Block 2]
```

After each generation:
```
VERIFICATION [chunk N]:
  Creator looks natural (not AI-generated)? [YES/NO]
  Product appears at correct moment?         [YES/NO — only in designated chunk]
  Lighting matches setting?                  [YES/NO]
  → PASS / FAIL (retry once) / DIVERGENCE (stop + report)
```

Max 2 retries per chunk. If both fail → report and wait for user before continuing.

Update estado.md: `chunk_0N: script ✅ | prompt ✅ | video ✅`

---

## BLOCK 5 — Save to desktop

For each chunk, download video:
```bash
curl -L "HIGGSFIELD_CDN_URL" -o "OUTPUT_FOLDER/chunk_0N.mp4"
ls -la "OUTPUT_FOLDER/chunk_0N.mp4"  # verify size > 0
```

Write the full script file:
```bash
# OUTPUT_FOLDER/script.txt
# Contains: full voiceover, chunk breakdown, runtime estimates, caption templates
```

Write caption file:
```bash
# OUTPUT_FOLDER/caption.txt
# Caption for social post (lowercase, conversational, max 2 sentences, 1 emoji optional)
# First comment text
```

Update estado.md: `chunk_0N: script ✅ | prompt ✅ | video ✅ | file ✅`

---

## Final output

```
OUTPUT_FOLDER/
  chunk_01.mp4    ← hook
  chunk_02.mp4    ← reframe
  chunk_03.mp4    ← reframe pt2 (full stack only)
  chunk_04.mp4    ← mechanism + product reveal
  chunk_05.mp4    ← payoff + CTA (full stack only)
  script.txt      ← full script + chunk breakdown
  caption.txt     ← social caption + first comment
  estado.md       ← completion status
```

Print:
```
========================================
  UGC VIDEO COMPLETE
========================================
Format:   [Full Stack / Mid-Funnel]
Chunks:   [N] videos generated
Output:   OUTPUT_FOLDER
Stitch:   chunks in order, bookend match chunk 1 ↔ last chunk
========================================
```
