---
name: lumina-wes-anderson-short-film
description: "Create a production-ready Wes Anderson-style short drama in Lumina Canvas Agent from a story, script, or visual references. Use for multilingual canvas-native production, a user-approved short seed video followed by true sequential extension to the requested duration, GBH color design, strict orthogonal-axis prompts, continuity verification, and final delivery."
---

# Lumina Wes Anderson Short Film

Produce a Wes Anderson-style short drama at the duration explicitly requested by the user in the selected or currently available Lumina node canvas. Turn story ideas, scripts, and visual references into an approved production graph: persistent specification and generation-step storyboard nodes, reusable character/scene/prop assets, a user-approved short seed video, sequential true-extension nodes, connected audio assets, and verified final delivery.

## Adaptive language rule

Read [language routing](references/language-routing.md) before the first visible response or canvas write. Maintain `interaction_language` and `story_language` separately.

- Resolve `interaction_language` from the user's newest direct instruction, excluding attachments, quotations, pasted scripts, code, metadata, proper names, and tool output. Use it for every visible heading, question, option, node description, plan, status, approval, error, and delivery summary.
- Resolve `story_language` from an explicit content-language request, otherwise retain the language of an existing draft being revised, otherwise use `interaction_language`. Use it for screenplay text, dialogue, narration, title cards, requested subtitles, and TTS.
- If an underlying model requires another prompt language, translate only the hidden technical parameter. Do not change the user's visible language or the final content language.
- A mid-production story-language change increments `SPEC-vN` and invalidates dependent dialogue, narration, title cards, and videos. An interaction-language-only change does not invalidate approved media.

Preserve model names, node IDs, hex colors, and camera codes in their original form.

## Canvas truth and operating contract

- Inspect the current canvas before creating anything. Reuse compatible source assets and completed nodes; never replace usable user material with newly generated substitutes.
- Use only node types, models, controls, and connections actually exposed by the current Lumina canvas. If a required capability is unavailable, preserve the production plan and asset inventory, identify the exact missing capability, and do not claim that nodes, bindings, generations, or exports were completed.
- Keep specifications in persistent editable String/text nodes. Put generation prompts inside the corresponding Image Generation, Video Extension, or Audio Generation node; do not create standalone prompt nodes for video steps.
- A referenced image must have both a real canvas connection to the target generation node and a resolved structured reference chip inserted through Lumina's `@` picker. Typed text such as `@CHAR-01` is not a resolved reference.
- Use the actual canvas node ID or exact current node name in every binding record. Do not invent IDs or infer a binding from a similar label.
- Generation is gated by approval. Use Lumina's native structured-choice dialog when available. If it is unavailable, ask with the two exact semantic choices required below and wait.

## Persistent canvas graph

Create and maintain these node roles. Localize descriptive labels, but preserve the stable prefixes.

```text
SPEC-vN       Final Video Spec (String/text)
STORY-vN      Generation-step storyboard and production ledger (String/text)
CHAR-XX       Character element image
SCENE-XX      Scene element image
PROP-XX       Prop or text-prop element image
FRAME-SEED-S0 Seed start frame
FRAME-EXT-XX  Optional extension reference frame
VIDEO-SEED    Initial short Video Generation node
VIDEO-EXT-XX  Sequential extension node returning a longer complete video
VIDEO-FINAL   Recorded final complete-video output/handle
VOICE-REF-vN  Optional approved voice audition
AUD-NAR-XX    Optional narration layer
AUD-SFX-XX    Optional sound effects/ambience
BGM-XX        Optional instrumental music segment
EDIT-vN       Edit decision list and delivery verification (String/text)
```

Connect reusable element images and the opening frame to `VIDEO-SEED`. Every `VIDEO-EXT-XX` must receive the latest verified complete video, never an older checkpoint or an isolated tail clip. Add other references only when the extension action supports them and they are required for the new beat. Maintain the complete lineage in `STORY-vN` or `EDIT-vN` and verify it after every extension.

## Production workflow

Follow this order. Every approval gate offers exactly:

```text
✅ Confirm and continue
✏️ Modify the current content
```

Localize those labels to the user's working language. If the user chooses Modify, collect the requested change, update the persistent node and dependent bindings, then present the same gate again.

1. Inspect the canvas and analyze the story, script, and source media. Extract characters, scenes, key props, text-prop candidates, emotional arc, chapters, sound cues, and source-image constraints. Read [source analysis](references/02-source-analysis.md).
2. Parse the final target duration only from the user's direct prompt, then collect or propose the title, language, narration preference, aspect ratio (`16:9`, `4:3`, or `9:16`), and resolution (`1080p` or `720p`). If duration is absent or ambiguous, ask one localized technical question and stop. Inspect actual seed/extension duration limits; if the exact target cannot be represented, show supported outcomes and wait. Never silently round.
3. Present the three color routes defined in [workflow](references/01-workflow.md): A · GBH palette, B · model-designed palette, or C · custom. Show the actual selected palette and receive approval before continuing. Read [GBH palette](references/06-gbh-palette.md).
4. Create `SPEC-v1 | Final Video Spec` from [the template](assets/final-video-spec.md). Record models that are actually available in Lumina, narrative mode, chapter structure, extension-step count, continuity locks, node-role conventions, and fallback behavior. Obtain approval and freeze the approved version.
5. When the source story is incomplete, expand it using [storyboard design](references/03-storyboard-design.md). Then follow [video production workflow](references/09-video-production-workflow.md): lock the screenplay, initialize `STORY-v1` and its versioned production ledger, and extract character, scene, prop, and voice requirements. Obtain approval of the screenplay and production profiles.
6. Create or bind reusable `CHAR`, `SCENE`, and `PROP` elements and obtain approval. When dialogue or narration is enabled, generate separate voice auditions, obtain approval, and freeze the voice map before dependent audio or video. Read [media assets](references/07-media-assets.md).
7. Build and approve a generation-step storyboard. Each row defines `video_step_id`, `mode` (`seed` or `extension`), `added_duration_seconds`, `expected_cumulative_duration_seconds`, framing, camera grammar, dependency versions, new story action, audio, environment/props, and a local timeline starting at `00:00`.
8. Generate only `VIDEO-SEED`, using a short supported duration and an independent prompt whose timeline runs from `00:00` to the seed duration. Verify that it plays, then show it to the user and stop. Do not create or invoke any `VIDEO-EXT-XX` until the user explicitly approves the seed. If revision is requested, regenerate only the seed and present the same gate again.
9. After seed approval, create extensions strictly one at a time. Every `VIDEO-EXT-XX` has its own independent prompt starting at `00:00`, receives the latest verified complete video, describes only the new beat, and must return a longer complete video. Verify the duration increase before using that output as the next input. Continue until the verified duration equals the user's locked target. Then apply [editing and compositing](references/08-editing-compositing.md) only for single-video audio finishing or export; never concatenate independent video clips to simulate extension.

Dependencies: color depends on analysis and output settings; `SPEC` depends on the direct-prompt duration and approved color; the locked screenplay and production ledger depend on `SPEC`; reusable assets depend on approved profiles; dialogue and narration depend on approved voices; `VIDEO-SEED` depends on the approved opening storyboard and inputs; every extension depends on explicit seed approval and the immediately preceding verified complete video; final delivery depends on a verified complete-video lineage reaching the locked target.

## Global visual invariants

- **Orthogonal cross-axis grammar:** the camera remains perpendicular to the scene plane. Camera motion is limited to central Z-axis snap push/pull, X-axis lateral move, or Y-axis vertical move. Subject motion is limited to one straight X/Y/central-Z path during an `M-00` segment. No pan, rotation, tilt, arc, orbit, diagonal travel, handheld shake, or slow zoom.
- **Planimetric composition:** prepend one approved planimetric lock phrase to every shot. Architecture may be bilaterally symmetric; furniture, props, and set dressing must not be mirrored duplicates.
- **Three-state character orientation:** each performance segment uses only frontal `0°`, exact profile `90°`, or back-facing `180°`. A change requires a complete 90° or 180° turn and immediate stabilization in the new state.
- **Locked final duration:** `target_duration_seconds` comes only from the user's direct prompt. The approved generation-step table divides that target into one short seed plus supported extension increments; the planned cumulative duration must reconcile exactly with the target.
- **Independent prompt clock:** every seed or extension action has a separate prompt and its own local timeline beginning at `00:00`. Its final timestamp equals only that action's requested seed or added duration. Never write global cumulative ranges such as `00:30–00:40` inside a video prompt.
- **True extension only:** an extension must input the latest verified complete video and return a longer complete video. Never substitute an isolated continuation clip, parallel independent videos, timeline concatenation, looping, freezing, retiming, padding, or silent duration rounding.
- **Camera language:** record camera motion only as a code plus the exact English keywords from the references. Use the working language only for narrative events.
- **Period anchor:** props, furniture, typography, and technology must belong to the 1930s–1980s unless the user explicitly chooses another period. No contemporary brands or digital devices.
- **Color continuity:** costume color must remain visibly distinct from the background. The same scene must not drift in color temperature across shots.
- **Prompt ceiling:** keep each Lumina video prompt, including reference chips, within the current node/model limit; use 4,500 characters as the operational maximum when no smaller limit is shown.

## Generation and recovery rules

- Prefer Lumina's built-in Image Generation, Video Generation, Video Extension, and Audio Generation nodes. Use `Seedance 2.5` when available; otherwise record the approved equivalent in `SPEC-vN`. The workflow requires a true extension action that accepts and returns the complete video. If it is unavailable, stop after the approved seed and report the limitation; do not fall back to concatenation.
- Before submission, verify node name, internal prompt, physical connections, resolved chips, model, ratio, resolution, added duration, and matching `video_step_id`. Do not retry unchanged inputs after a validation or generation error; identify and correct the offending asset, parameter, duration, or reference.
- Preserve successful nodes and versions. Regenerate only failed or explicitly revised assets. If a revision changes a continuity lock, increment the affected `SPEC`, `STORY`, or asset version and reapprove downstream work.
- Never report completion based only on queued jobs. Confirm that generated images display, videos play, connected references resolve, and required audio is present.

## Final verification

Before delivery, verify all of the following against the live canvas:

- The approved `SPEC-vN` records the target duration from the user's direct prompt, and `STORY-vN` contains a seed-plus-extension plan that reconciles to it.
- `VIDEO-SEED` exists, plays, and has explicit user approval recorded before any extension.
- Each `VIDEO-EXT-XX` points to the immediately previous verified complete video and returns a longer complete video.
- Every seed and extension prompt is independent, starts at local `00:00`, and contains no cumulative film timecode.
- The initial frame and required references have real connections and resolved chips where supported.
- Camera codes, character orientations, period anchors, prop appearance, costume identity colors, and scene color temperature remain consistent.
- Narration, SFX, and BGM nodes exist only when approved and connect to the correct layer or video step.
- The final complete video plays and its verified duration equals the locked target exactly. If the action cannot produce it, revise and reapprove the target or extension plan before continuing; do not accept an undeclared tolerance.
- `EDIT-vN` records the extension lineage, actual durations, audio status, failures, open questions, and export status.

Report the approved spec/storyboard versions, locked and verified durations, seed approval, complete extension lineage, asset/audio status, final video handle, and any real limitation.

## Reference routing

| Reference | Read when |
|---|---|
| [Language routing](references/language-routing.md) | Resolving or changing interaction and story languages |
| [Production workflow](references/01-workflow.md) | Starting any new film or changing approvals |
| [Source analysis](references/02-source-analysis.md) | Analyzing scripts or reference media |
| [Storyboard design](references/03-storyboard-design.md) | Expanding a story or planning shots |
| [Camera and movement](references/04-camera-and-movement.md) | Planning, prompting, or reviewing any shot |
| [Prompt writing](references/05-prompt-writing.md) | Creating image, video, title, character, prop, or audio prompts |
| [GBH palette](references/06-gbh-palette.md) | Choosing or enforcing the GBH color system |
| [Media assets](references/07-media-assets.md) | Creating, connecting, generating, or validating canvas media nodes |
| [Editing and compositing](references/08-editing-compositing.md) | Building the final timeline or edit decision list |
| [Video production workflow](references/09-video-production-workflow.md) | Producing confirmed scripts scene by scene with versioned assets and storyboard timing |
