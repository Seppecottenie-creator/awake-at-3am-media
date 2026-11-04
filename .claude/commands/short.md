---
description: Make, render, publish and schedule a new What If short in one go
argument-hint: [optional topic]
---

Make a new What If short fully automatically, following CLAUDE.md. Do not stop to ask for approval unless something fails or a fact can't be verified.

Topic: $ARGUMENTS

1. **Topic** — If no topic is given above, pick one yourself: a strong, broadly appealing "What if…?" science question that is not already in `shorts/` or `long/`. Prefer topics with dramatic visuals.
2. **Research** — Verify every claim against reliable sources. Drop any claim you can't confirm. Save the script plus sources to `scripts/<snake_case_name>.md`.
3. **Build & render** — Build the video with the pipeline in `D:\render-cache\awake-at-3am-pipeline`, reusing shots/props from the library and adding new ones. Render on the GPU. Output 1080x1920, 60–100 s, saved as `shorts/what_if_<topic>.mp4`, under 100 MB.
4. **Self-check** — Watch-check the result: extract frames at a few timestamps and look at them, confirm audio and video lengths match, no black frames, subtitles readable. Fix and re-render if needed.
5. **Publish** — Commit the video and script, push to `main`.
6. **Schedule** — Use Metricool `getScheduledPosts` (blogId 7816349) to find the next day without a post, and schedule the short there at 16:00 Europe/Brussels with the raw GitHub URL, using the title/description/tag rules from CLAUDE.md.
7. **Report** — Reply in Dutch with: topic, title, scheduled date/time, render time, and the Metricool planner link.
