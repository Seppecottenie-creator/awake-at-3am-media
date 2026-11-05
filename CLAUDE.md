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
- Narrator: warm male voice, slightly funny, with occasional small jokes.
- Pacing: brisk. Short pauses, no slow build-up.
- Build a reusable library of shots and props over time.
- Look: strong 3D animation feel, realistic environments.
- Recurring character: **Ollie**, the kid who can't sleep (oversized red hoodie, blue plaid pajama pants, grey slippers). He is an actor who experiences the scenario. He never narrates or talks, and only appears when the topic involves a human.
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
- Every video has its script + sources in `scripts/`.
- Every image/clip not made by us is logged with source + license in the script file.

## Topics
- The more absurd and extreme the premise, the better: exaggerated scale (100× bigger), tiny time spans (for 1 second, one zeptosecond), broken laws of physics (gravity off, light speed doubled), impossible worlds (cube Earth).
- Give well-known topics an own twist rather than repeating what big channels already did (e.g. "Sun disappeared for 1 second" instead of "Sun disappeared").
- Sports "what ifs" (football, basketball, tennis, F1, cycling, golf, baseball, American football, darts, Olympic sports, …) are welcome as physics or absurd scenarios: ball at light speed, goal/hoop 10× bigger, a match on the Moon, an F1 car with no air resistance, a 1,000 m high jump. Use generic 3D athletes, no real athletes' likeness, team/league logos, kits or broadcast footage.

## Hook (first second)
- Open on the most spectacular image of the video, with the question big on screen. No slow intro, no title card. The first spoken line is the question or a shocking fact.

## External images
- Allowed only with a clear free license: public domain (NASA, ESA, NOAA, USGS), CC0, Pexels/Pixabay/Unsplash license, or Wikimedia Commons CC BY / CC BY-SA with credit in the description.
- Never: Google Images, Getty/AP/Reuters photos, sports broadcast footage, player photos without a free license, logos, movie/TV stills.

## Formats
- Shorts: 40–50 s, vertical.
- Long-form: one topic worked out in depth (e.g. day by day), 8–10 min, horizontal.

## Repo rules
- This repo is public: only put videos here that are meant to be published.
