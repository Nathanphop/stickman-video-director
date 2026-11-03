# B default media providers: Codex images + AI Studio TTS

Scope: **B · Autopilot only**. A1–A6 keep their existing generation and narration contracts.

For this fork, resolve media providers in this order: **explicit user choice → Codex image generation plus `$ai-studio-tts` defaults below**. Style references still control visual language, caption design, preferred language and any preferred voice name. A preferred voice such as Bodi or Fola is usable only when the live AI Studio options list returns it; never invent an unavailable voice or silently switch an accepted recording.

## Codex image generation

- Use the host Codex image-generation capability for new character sheets, poses, props, backgrounds and composed scene artwork. Do not route the default path through `gemini-media.mjs` or require `GEMINI_API_KEY`.
- Save every accepted result as a frozen local file inside the user project, normally under `assets/images/`; the renderer consumes local files and never remote image URLs. Record the prompt, intended shot(s), provider/tool and accepted file path in the checkpoint.
- Use a transparent background only for isolated characters, props or other layers that must composite over a separate background. Use an opaque background for full scenes and reference boards.
- Apply the selected B-style reference in every generation prompt. Preserve the approved cast and drawing grammar while changing expression, pose, action, viewpoint and story-specific props. Use reference images when the host supports them and continuity requires them.
- Generate bounded batches and inspect full-size outputs or contact sheets before continuing. Reuse accepted assets; revise only the affected image or layer. Do not spend indefinitely on retries, and disclose any billable generation before the call.

## AI Studio narration

- Invoke `$ai-studio-tts`; do not recreate its RPC, Playwright, cookie or account-selection logic in this Skill.
- Before generation, run its read-only `options` command. If it reports `selectionRequired: account`, present the returned account labels and let the user choose. Then rerun `options --account ...` and, when useful, `rpc-status --account ...`.
- Confirm the script, language, account, live model ID and live voice ID before the first quota-consuming request. Recommend a style reference's preferred voice only when it appears in the live list. Otherwise recommend the returned default or ask the user to choose from the returned choices.
- Preserve B's native-voice rule: do not invent descriptive age, accent, pitch, texture, emotion or pacing instructions. If AI Studio requires a style choice, use the returned provider default unless the user or selected B-style reference explicitly chooses a live style.
- Generate only after the narration text is approved. Save to an explicit absolute `.wav` path inside the project, normally `assets/audio/narration.wav`. For long scripts, use the TTS Skill's `--text-file` path. Record the selected account label/authuser, model, voice, style and output hash without storing cookies or session secrets.
- Use `auto` mode unless the user requests a specific route. Stop after one failed generation attempt and follow `$ai-studio-tts` diagnostics; never loop quota-consuming retries.

## Readiness and fallback

The local `doctor` command checks only the renderer. Separately report whether Codex image generation is available and whether AI Studio `options`/`rpc-status` succeeded. The default B path does **not** require `GEMINI_API_KEY`.

Keep `scripts/gemini-media.mjs` only as an optional compatibility adapter when the user explicitly selects Gemini API generation. Do not silently fall back to it, local OS TTS or another image provider. Existing approved images and narration remain frozen unless the user requests a replacement.
