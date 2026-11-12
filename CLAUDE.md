# Awake at 3AM — video channel

English "What if...?" 3D science explainer videos for YouTube (Shorts + long-form) and TikTok.

## Workflow
1. Research the topic — every fact must be correct.
2. Write the script, then build the video with the code-built 3D pipeline (Zack D Films style).
3. Render locally on this PC's GPU. Check the GPU first and use GPU rendering wherever the renderer supports it.
4. Save finished videos to `shorts/` (9:16, 1080x1920) or `long/` (16:9, 1920x1080), with lowercase snake_case names like `what_if_the_moon_disappeared.mp4`.
5. Commit and push to `main`. Videos are scheduled on YouTube via Metricool from the raw GitHub URL:
   `https://raw.githubusercontent.com/Seppecottenie-creator/awake-at-3am-media/main/<path>`
   Keep each file well under 100 MB (GitHub limit).
6. Schedule on YouTube with the Metricool MCP (`createScheduledPost`):
   - Brand / blogId: `7816349`, timezone `Europe/Brussels`
   - Default slot: next free day at 16:00 (best time per Metricool). Max one video per day; check `getScheduledPosts` first.
   - `media`: the raw GitHub URL above (push first, then schedule)
   - `youtubeData`: type `short` (or `video` for long-form), privacy `public`, madeForKids `false`, category `SCIENCE_TECHNOLOGY`, 5–8 tags
   - Title: "What If …?" + 1–2 emoji, under 70 characters. Description: 1–2 hook sentences + `#whatif #science #shorts` + a topic hashtag.
   - Show the user title, description and slot before scheduling — except when running the `/short` command, which schedules automatically.
   - Default rhythm: one video per day, weekends included.

## Style
- Language: always English. Broad international audience — no country-specific or personal references.
- Narrator: warm male voice, genuinely funny: dry jokes, absurd comparisons, a punchline every ~10 s, a self-aware aside now and then. Ollie's reactions (physical comedy) carry half the humour.
- Bleeped swearing: at most 1–2 per short, only as a punchline. Fully cover the word with a classic bleep, show it as grawlix in the captions (`#$%!`), never in the first 7 seconds, never in title, description or thumbnail, and never slurs or sexual words. Mild words only (e.g. "holy sh*t", "what the f*ck").
- Pacing: calm and followable. Narration max ~135 words per minute (≈ 105–115 words for a 50 s short). If the script is too long, cut content — never speed up the voice to make it fit. Leave a short beat (~0.4 s) after each punchline and big number.
- Build a reusable library of shots and props over time.
- Look: photoreal-leaning, like a high-end archviz / documentary render, not a game. Characters (Ollie) may stay stylised, but the world around them must look real.
  - Render in Blender (Cycles with OptiX on the RTX 3070, low samples + OptiX denoiser; EEVEE only for fast tests), not the WebGL/three.js renderer.
  - Real assets only: Poly Haven (CC0) HDRIs, PBR textures and models as the base library; other models only with a clear free/commercial licence, logged in the script file. No primitive boxes, spheres or capsules standing in for buildings, trees, cars, people or balls.
  - Lighting: HDRI + a sun/key light, real shadows, bounce light, atmospheric depth (haze/fog in big scenes), subtle depth of field and motion blur. Real-world scale for everything.
  - Physical camera behaviour: 24–35 mm for wides, 50–85 mm for close-ups, smooth eased moves, slight handheld drift; no instant snaps.
- Editing rhythm: 10–15 shots per short (≈3–5 s each), not a cut every 1–2 s. Let a shot breathe after a big moment.
- On-screen info: max one info box or counter on screen at a time, and max ~5 per short. Captions show at most 4–5 words at once. The image should explain itself; overlays only for the key number.
- Recurring character: **Ollie**, the kid who can't sleep (oversized red hoodie, blue plaid pajama pants, grey slippers). He is an actor who experiences the scenario. He never narrates or talks. He is optional, not the default: use him only when a human perspective makes the scene funnier or clearer, roughly every other short at most. Many shorts have no humans at all.
  - In the pipeline: `lib/ollie.js` (`loadOllie`, `ollie.update(t, timeline)`, `ollie.hold('mug' | 'flashlight')`), model `assets/character/ollie.glb` (Mixamo rig, PBR textures, ~1.39 m), props in `assets/props/`. Sources and converter scripts: `D:\mixamo` → `assets/character/src/`. The old boy (`lib/boy.js`) is retired.
  - Never show him in his bind pose (T-pose / A-pose). Every shot uses a real animation clip from the library in the pipeline (`assets/character/anims/*.glb`, Mixamo):
    - Standing: `idle`, `breathing_idle`, `look_around`, `look_around_alt`, `look_nervous`, `point`, `point_alt`, `wave`, `wave_alt`
    - Reactions: `surprised`, `react`, `terrified`, `shiver` (procedural: arms hugged + fast tremble, for cold scenes)
    - Low: `crouch_idle`, `crouch_to_stand`, `sit_idle`
    - Moving: `walk`, `run`, `jump`, `jump_alt`, `jumping`, `jumping_alt`
    - Air / water: `falling` (prone flail), `fall_idle` (upright, arms up), `fall_land`, `float`, `tread_water`
  - He reacts to what happens in the scene (cold → `shiver`, dark → `look_nervous`, falling → `fall_idle`/`falling`). Match the clip to the narration line. He has no face rig, so emotions must read from body and framing.
  - Feet on the ground (or clearly floating/falling on purpose), with a soft contact shadow. Grounded shots must not hover.
  - Close-ups: frame his head centred or on a third with headroom; never crop his head at the frame edge (track a smoothed head position).
  - Reactions (surprised/shocked): quick camera push-in plus `ollie.eyes(1.15)` for a few frames. Blinks run automatically.
  - Vary framing and camera per shot (close-up face, wide, over-the-shoulder, low angle). Never the same centered full-body pose twice in a row.
  - Keep him out of the caption zone: captions sit in the lower third, so frame him above it or move captions.

## Quality checks (before publishing)
- Extract frames and check for: bind poses, hovering feet, captions covering the character, unreadable small text.
- All on-screen text must be readable on a phone: minimum ~42 px at 1080 width, high contrast (dark box behind text on light backgrounds).
- Loudness: normalize every video to -14 LUFS integrated, true peak ≤ -1 dBTP.
- Audio mix — the voice must always be clearly on top:
  - Music bed sits ~20 dB under the voice (around -34 LUFS when the voice is ~-14), with sidechain ducking (extra ~6 dB) whenever the narrator speaks.
  - Sound effects never peak above the voice. While the narrator talks, SFX stay at least 6 dB under the voice; big hits (explosions, impacts) only in gaps between lines.
  - Check: measure voice-only vs music+SFX-only stems; if music/SFX are within 15 dB of the voice during speech, lower them and re-mix.
- Every video has its script + sources in `scripts/`.
- Every image/clip not made by us is logged with source + license in the script file.

## Topics
- The more absurd and extreme the premise, the better: exaggerated scale (100× bigger), tiny time spans (for 1 second, one zeptosecond), broken laws of physics (gravity off, light speed doubled), impossible worlds (cube Earth).
- Give well-known topics an own twist rather than repeating what big channels already did (e.g. "Sun disappeared for 1 second" instead of "Sun disappeared").
- Sports "what ifs" (football, basketball, tennis, F1, cycling, golf, baseball, American football, darts, Olympic sports, …) are welcome as physics or absurd scenarios: ball at light speed, goal/hoop 10× bigger, a match on the Moon, an F1 car with no air resistance, a 1,000 m high jump. Use generic 3D athletes, no real athletes' likeness, team/league logos, kits or broadcast footage.

- History "what ifs" are welcome: alternate history (Roman Empire never fell) and time-travel scenarios (Ollie in ancient Rome for a day). Stick to well-documented facts for the real part, and say clearly when the rest is speculation ("historians think…", "probably"). Keep it light: no wars, genocides or recent politics as entertainment, and no real people's likeness from the last ~100 years.
- Not everything needs people: animals (an elephant the size of an ant, a world run by octopuses), objects and machines (a phone battery that never dies), nature and planets can carry a whole short on their own. Give the main animal/object some personality through animation and camera, but keep it realistic-stylised and never a copy of a known cartoon character.
- Topic backlog lives in `topics.md`.

## Loop ending
- Make the last line flow seamlessly into the first line/shot, so the Short loops naturally and people watch it twice (e.g. end on "…which is exactly why your floor just opened up" → back to the opening shot). No outro, no "subscribe" card, no fade to black.

## Engagement
- Every short gets a first comment with a short, fun question about the topic that invites an answer (e.g. "Would you jump in? 👇", "Which sport should we break next?"). Put it in Metricool's `firstCommentText`. Remind the user to pin it on YouTube (pinning can't be done through the API).

## Hook (first second)
- Open on the most spectacular image of the video, with the question big on screen. No slow intro, no title card. The first spoken line is the question or a shocking fact.

## External images
- Allowed only with a clear free license: public domain (NASA, ESA, NOAA, USGS), CC0, Pexels/Pixabay/Unsplash license, or Wikimedia Commons CC BY / CC BY-SA with credit in the description.
- Never: Google Images, Getty/AP/Reuters photos, sports broadcast footage, player photos without a free license, logos, movie/TV stills.

## Formats
- Shorts: 40–55 s, vertical.
- Long-form: one topic worked out in depth (e.g. day by day), 8–10 min, horizontal.

## Repo rules
- This repo is public: only put videos here that are meant to be published.
