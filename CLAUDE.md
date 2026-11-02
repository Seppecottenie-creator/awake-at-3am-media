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

## Style
- Language: always English. Broad international audience — no country-specific or personal references.
- Narrator: warm male voice, slightly funny, with occasional small jokes.
- Pacing: a bit faster than voice speed 0.9 with ~1.25 s pauses (that felt slightly too slow); aim between that and a brisk ~65 s short.
- Look: strong 3D animation feel, realistic environments.
- Recurring character: the rigged boy (red sweater) is an actor who experiences the scenario. He never narrates or talks, and only appears when the topic involves a human.
- Build a reusable library of shots and props over time.

## Formats
- Shorts: ~60–100 s, vertical.
- Long-form: one topic worked out in depth (e.g. day by day), 8–10 min, horizontal.

## Repo rules
- This repo is public: only put videos here that are meant to be published.
