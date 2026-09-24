---
name: lumina-wes-anderson-short-film
description: "Create a production-ready 1–3 minute Wes Anderson-style short drama in Lumina Canvas Agent from a story, script, or visual references. Use for canvas-native source analysis, approved storyboards, GBH color design, strict orthogonal-axis shot prompts, connected image/video/audio generation nodes, continuity verification, and final edit planning."
---

# Lumina Wes Anderson Short Film

Produce a 1–3 minute Wes Anderson-style short drama in the Lumina node canvas selected by the user or currently available. Turn story ideas, scripts, and visual references into an approved production graph: persistent specification and storyboard nodes, reusable character/scene/prop assets, per-shot start frames, connected video and audio generation nodes, and a verified final edit plan.

Use the user's language for all visible canvas work, including node names, specifications, storyboards, prompts, statuses, approval questions, and summaries. Preserve model names, node IDs, hex colors, and camera codes in their original form.

## Canvas truth and operating contract

- Inspect the current canvas before creating anything. Reuse compatible source assets and completed nodes; never replace usable user material with newly generated substitutes.
- Use only node types, models, controls, and connections actually exposed by the current Lumina canvas. If a required capability is unavailable, preserve the production plan and asset inventory, identify the exact missing capability, and do not claim that nodes, bindings, generations, or exports were completed.
- Keep specifications in persistent editable String/text nodes. Put generation prompts inside the corresponding Image Generation, Video Generation, or Audio Generation node; do not create standalone per-shot prompt nodes.
- A referenced image must have both a real canvas connection to the target generation node and a resolved structured reference chip inserted through Lumina's `@` picker. Typed text such as `@CHAR-01` is not a resolved reference.
- Use the actual canvas node ID or exact current node name in every binding record. Do not invent IDs or infer a binding from a similar label.
- Generation is gated by approval. Use Lumina's native structured-choice dialog when available. If it is unavailable, ask with the two exact semantic choices required below and wait.

## Persistent canvas graph

Create and maintain these node roles. Localize descriptive labels, but preserve the stable prefixes.

```text
SPEC-vN       Final Video Spec (String/text)
STORY-vN      Storyboard and shot plans (String/text)
CHAR-XX       Character element image
SCENE-XX      Scene element image
PROP-XX       Prop or text-prop element image
FRAME-XX-S0   Shot start frame
FRAME-XX-KN   Optional mid/end verification frame
SHOT-XX       Final per-shot Video Generation node
VOICE-REF-vN  Optional approved voice audition
AUD-NAR-XX    Optional shot narration
AUD-SFX-XX    Optional shot sound effects/ambience
BGM-XX        Optional instrumental music segment
EDIT-vN       Edit decision list and delivery verification (String/text)
```

Connect reusable element images to every shot that actually uses them. Connect each `FRAME-XX-S0` and optional `FRAME-XX-KN` only to its matching `SHOT-XX`. Connect audio only to the shot or edit stage for which it was approved. Maintain a binding table in `STORY-vN` or `EDIT-vN` and verify it against the live canvas after every structural change.

## Production workflow

Follow this order. Every approval gate offers exactly:

```text
✅ Confirm and continue
✏️ Modify the current content
```

Localize those labels to the user's working language. If the user chooses Modify, collect the requested change, update the persistent node and dependent bindings, then present the same gate again.

1. Inspect the canvas and analyze the story, script, and source media. Extract characters, scenes, key props, text-prop candidates, emotional arc, chapters, sound cues, and source-image constraints. Read [source analysis](references/02-source-analysis.md).
2. Collect or propose the title, 1/2/3-minute duration, language, narration preference, aspect ratio (`16:9`, `4:3`, or `9:16`), and resolution (`1080p` or `720p`). Do not silently override explicit choices.
3. Present the three color routes defined in [workflow](references/01-workflow.md): A · GBH palette, B · model-designed palette, or C · custom. Show the actual selected palette and receive approval before continuing. Read [GBH palette](references/06-gbh-palette.md).
4. Create `SPEC-v1 | Final Video Spec` from [the template](assets/final-video-spec.md). Record models that are actually available in Lumina, narrative mode, chapter structure, shot count, continuity locks, node-role conventions, and fallback behavior. Obtain approval and freeze the approved version.
5. When the source story is incomplete, expand it using the character and narrative rules in [storyboard design](references/03-storyboard-design.md). Create `STORY-v1 | Storyboard` with ordered 30-second shot plans from [the shot template](assets/shot-plan.md), key elements, audio layers, and a binding manifest. Every shot must obey [camera and movement](references/04-camera-and-movement.md). Obtain approval.
6. Create or bind `CHAR`, `SCENE`, and `PROP` elements first, followed by each `FRAME-XX-S0`. Create `FRAME-XX-KN` only when it verifies a mid-shot axis action or final state. Inspect every result for continuity, orientation, composition, period accuracy, and usable aspect ratio. Obtain approval of the asset set before video generation. Read [media assets](references/07-media-assets.md).
7. For each shot, write the complete prompt inside its `SHOT-XX` Video Generation node using [prompt writing](references/05-prompt-writing.md). Use the approved element and frame connections plus resolved `@` reference chips. Show and approve each shot prompt before invoking that shot. Generate and review `SHOT-01` first; do not generate shots 02–N until the user approves the first shot's visual grammar and continuity.
8. Generate narration, sound effects, ambience, and instrumental BGM only when approved. Keep voice, sound, and music in separate nodes/layers. Validate every audio input and its duration before connecting it to a video request. Never connect a voice audition directly as final narration.
9. After all shots and audio are approved, create `EDIT-v1 | Edit Decision List`, assemble with the real composition/editor capability if the canvas exposes one, and follow [editing and compositing](references/08-editing-compositing.md). If Lumina cannot assemble or export the final film, deliver the verified ordered shot/audio set and edit decision list without claiming a final composite exists.

Dependencies: color depends on analysis and output settings; `SPEC` depends on approved color; `STORY` depends on `SPEC`; assets depend on `STORY`; shots depend on approved assets; audio depends on `STORY` and can run alongside approved shot generation; final assembly depends on approved shots and audio.

## Global visual invariants

- **Orthogonal cross-axis grammar:** the camera remains perpendicular to the scene plane. Camera motion is limited to central Z-axis snap push/pull, X-axis lateral move, or Y-axis vertical move. Subject motion is limited to one straight X/Y/central-Z path during an `M-00` segment. No pan, rotation, tilt, arc, orbit, diagonal travel, handheld shake, or slow zoom.
- **Planimetric composition:** prepend one approved planimetric lock phrase to every shot. Architecture may be bilaterally symmetric; furniture, props, and set dressing must not be mirrored duplicates.
- **Three-state character orientation:** each performance segment uses only frontal `0°`, exact profile `90°`, or back-facing `180°`. A change requires a complete 90° or 180° turn and immediate stabilization in the new state.
- **Thirty-second shot unit:** default to a complete `[0–30s]` timeline: static establishment, one axis action or at most one hard cut, then at least two seconds of stable resolution. Use another duration only when the user explicitly requests it.
- **Camera language:** record camera motion only as a code plus the exact English keywords from the references. Use the working language only for narrative events.
- **Period anchor:** props, furniture, typography, and technology must belong to the 1930s–1980s unless the user explicitly chooses another period. No contemporary brands or digital devices.
- **Color continuity:** costume color must remain visibly distinct from the background. The same scene must not drift in color temperature across shots.
- **Prompt ceiling:** keep each Lumina video prompt, including reference chips, within the current node/model limit; use 4,500 characters as the operational maximum when no smaller limit is shown.

## Generation and recovery rules

- Prefer Lumina's built-in Image Generation, Video Generation, and Audio Generation nodes. Use `Seedance 2.5` for multimodal video when available; otherwise record the user-approved equivalent in `SPEC-vN`. Use only `720p` or `1080p` and the approved aspect ratio.
- Before submission, verify node name, internal prompt, physical connections, resolved chips, model, ratio, resolution, duration, and matching shot ID. Do not retry unchanged inputs after a validation or generation error; identify and correct the offending asset, parameter, duration, or reference.
- Preserve successful nodes and versions. Regenerate only failed or explicitly revised assets. If a revision changes a continuity lock, increment the affected `SPEC`, `STORY`, or asset version and reapprove downstream work.
- Never report completion based only on queued jobs. Confirm that generated images display, videos play, connected references resolve, and required audio is present.

## Final verification

Before delivery, verify all of the following against the live canvas:

- The approved `SPEC-vN` and `STORY-vN` exist and match the current graph.
- N planned shots map to exactly N `SHOT-XX` nodes in order.
- Each shot has a valid `FRAME-XX-S0`, correct element-image connections, resolved reference chips, and the approved internal prompt.
- Camera codes, character orientations, period anchors, prop appearance, costume identity colors, and scene color temperature remain consistent.
- Narration, SFX, and BGM nodes exist only when approved and connect to the correct layer/shot.
- Videos play and the first-shot approval convention is reflected in later shots.
- `EDIT-vN` records track order, levels, transitions, chapter cards, failures, open questions, and final assembly/export status.

Report the approved spec version, storyboard version, shot count, asset/shot binding summary, generation status, and whether a real final composite/export exists.

## Reference routing

| Reference | Read when |
|---|---|
| [Production workflow](references/01-workflow.md) | Starting any new film or changing approvals |
| [Source analysis](references/02-source-analysis.md) | Analyzing scripts or reference media |
| [Storyboard design](references/03-storyboard-design.md) | Expanding a story or planning shots |
| [Camera and movement](references/04-camera-and-movement.md) | Planning, prompting, or reviewing any shot |
| [Prompt writing](references/05-prompt-writing.md) | Creating image, video, title, character, prop, or audio prompts |
| [GBH palette](references/06-gbh-palette.md) | Choosing or enforcing the GBH color system |
| [Media assets](references/07-media-assets.md) | Creating, connecting, generating, or validating canvas media nodes |
| [Editing and compositing](references/08-editing-compositing.md) | Building the final timeline or edit decision list |
