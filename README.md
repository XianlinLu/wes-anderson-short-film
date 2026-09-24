# Lumina Wes Anderson Short Film Skill

[中文版](#中文版) · [English](#english)

## 中文版

面向 **Lumina Canvas Agent** 的韦斯·安德森风格短剧生产 Skill。它把故事构想、完整剧本或视觉参考转化为一套可核验的画布生产图：全局规格、角色与声音版本、用户确认的分镜总汇表、角色/场景/道具资产、第一分镜种子视频、连续延长节点，以及最终交付验证表。

### 核心能力

- 十字轴线运镜与严格正交摄影
- planimetric 平面构图与“结构对称、内容不镜像”
- 角色正面 `0°` / 严格侧面 `90°` / 背面 `180°` 三态锁定
- 《布达佩斯大饭店》十色色板与章节色彩弧线
- 用户提示词驱动的最终时长，以及分镜表驱动的增量时长、对白、动作和声音生成
- 剧本锁定、角色资产确认、短种子视频确认、顺序延长与版本失效传播
- 真实节点连接与 Lumina `@` 结构化引用双重校验

### 语言自适应

Skill 分别维护 `interaction_language` 和 `story_language`：

- `interaction_language` 根据用户最新的直接指令确定，用于所有可见标题、问题、选项、状态、错误和交付说明。
- `story_language` 优先采用用户明确指定的创作语言；修改现有剧本时保留剧本语言；否则跟随 `interaction_language`。
- 附件、引用、粘贴剧本、代码、元数据和工具输出中的语言不能单独改变交互语言。
- 若底层模型要求其他 Prompt 语言，只翻译隐藏的技术参数，不改变用户看到的语言和最终内容语言。

### 视频时长与延长规则

最终时长只读取用户直接提示词中的 `target_duration_seconds`。视频时长不能由 Agent 另行选择，而必须逐行继承用户之前确认的分镜总汇表：

- 分镜总汇表必须先经用户确认，并为每行明确 `storyboard_shot_id` 与 `shot_duration_seconds`；所有行之和等于目标总时长。
- `VIDEO-SEED` 严格对应第一行分镜，请求时长和 Prompt 结束时间必须准确等于第一行的 `shot_duration_seconds`。“短种子”只表示先供用户评审，不能擅自缩短第一分镜。
- 第一分镜视频生成后立即展示并停止。用户明确确认前，不得创建第二个视频。
- 用户确认后，第二个视频 `VIDEO-EXT-01` 才可按分镜总汇表第二行生成；后续视频继续按表逐行、一次一个生成。
- 每个视频节点都有独立 Prompt，内部时间轴必须从本地 `00:00` 开始，结束时间只等于该次种子或新增片段的时长；累计时长只记在生产台账中，禁止写入 Prompt。
- 每个延长节点必须输入上一节点输出的完整视频，并返回更长的完整视频；不得用独立尾段、片段拼接、循环、冻结、变速或填充冒充延长。
- 每次延长后核验实测总时长，直到它准确等于用户提示词规定的最终时长。若模型不支持所需增量，先展示可支持值并重新确认计划，禁止静默取整。

### Lumina 画布节点

- `SPEC-vN`：全局视频规格
- `STORY-vN`：确认剧本、分镜总汇表与生产台账
- `CHAR-XX` / `SCENE-XX` / `PROP-XX`：角色、场景和道具资产
- `FRAME-SEED-S0` / `FRAME-EXT-XX`：种子起始帧和可选延长参考帧
- `VOICE-REF-vN` / `AUD-NAR-XX` / `AUD-SFX-XX` / `BGM-XX`：声音资产
- `VIDEO-SEED`：等待用户确认的短种子视频
- `VIDEO-EXT-XX`：基于上一版完整视频的顺序延长节点
- `VIDEO-FINAL`：已核验达到目标时长的最终完整视频
- `EDIT-vN`：剪辑决策与交付验证

图片既要真实连接到视频节点，也要通过 `@` 选择器插入已解析引用标签。Prompt 写入对应生成节点内部，不为每个视频步骤创建孤立 Prompt 文本节点。

### 制作流程

1. 检查画布并确定交互语言与内容语言。
2. 分析并锁定剧本、片名、总时长、画幅、清晰度、旁白与色彩方案。
3. 提取角色与场景，建立带版本号的生产台账。
4. 生成并确认所有角色、场景、道具资产；需要对白或旁白时生成并确认声音试听。
5. 建立并让用户确认分镜总汇表；每行明确 `storyboard_shot_id`、`shot_duration_seconds`、构图、运镜、依赖版本、动作和声音。
6. 只生成第一行对应的 `VIDEO-SEED`，其时长严格等于第一行，独立 Prompt 从 `00:00` 开始。
7. 展示第一分镜视频并等待明确确认；确认前停止，不创建第二个视频。
8. 确认后从第二行开始逐次延长：每行对应一个独立视频节点和本地 `00:00` Prompt，并以上一版完整视频为输入，直至达到目标时长。

### 目录

```text
SKILL.md
license.txt
references/
  01-workflow.md
  02-source-analysis.md
  03-storyboard-design.md
  04-camera-and-movement.md
  05-prompt-writing.md
  06-gbh-palette.md
  07-media-assets.md
  08-editing-compositing.md
  09-video-production-workflow.md
  language-routing.md
assets/
  final-video-spec.md
  shot-plan.md
  canvas-production-manifest.md
```

### 使用示例

```text
请在当前 Lumina 画布中使用这个 Skill，把我的剧本制作成 2 分钟、4:3、1080p 的中文短剧。使用 GBH 色板，先确认角色资产、声音和分镜总汇表。按照总汇表第一行的完整时长生成 VIDEO-SEED，等我确认后再按照第二行生成第二个视频，并连续延长到 2 分钟；每次视频 Prompt 都从 00:00 开始。
```

## English

A production skill for creating Wes Anderson-inspired short dramas with **Lumina Canvas Agent**. It turns a story idea, confirmed screenplay, or visual references into a verifiable canvas graph containing global specifications, versioned character and voice assets, a user-approved master storyboard, a first-shot seed video, sequential extension nodes, and final delivery verification.

### Core capabilities

- Strict orthogonal cross-axis camera grammar
- Planimetric framing with symmetric architecture and non-mirrored set dressing
- Locked character orientations: frontal `0°`, exact profile `90°`, or back-facing `180°`
- A ten-color Grand Budapest Hotel palette and chapter color arcs
- Prompt-locked final duration plus storyboard-driven seed and extension increments, dialogue, actions, and sound
- Screenplay locking, asset approval, voice approval, seed-video approval, sequential extension, and dependency invalidation
- Dual verification through real canvas connections and resolved Lumina `@` reference chips

### Adaptive language behavior

The skill tracks `interaction_language` and `story_language` separately:

- `interaction_language` comes from the user's latest direct instruction and controls every visible heading, question, option, status, error, and delivery note.
- `story_language` follows an explicit content-language request, otherwise preserves the language of a revised draft, otherwise follows `interaction_language`.
- Language found only in attachments, quotations, pasted scripts, code, metadata, or tool output cannot change the interaction language by itself.
- If an underlying model requires another prompt language, only the hidden technical parameter is translated; visible UI and final narrative language remain unchanged.

### Video duration and extension contract

The final `target_duration_seconds` comes only from the user's direct prompt. Video durations are not selected separately by the agent; they inherit the previously user-approved master storyboard row by row:

- The user must first approve a master storyboard whose rows define `storyboard_shot_id` and `shot_duration_seconds`; the row sum equals the target duration.
- `VIDEO-SEED` maps exactly to row 1. Its requested duration and prompt end time must equal row 1 `shot_duration_seconds`. “Short seed” describes its review role and does not permit shortening the first shot.
- Present the generated first-shot video and stop. Do not create the second video before explicit user approval.
- After approval, generate `VIDEO-EXT-01` from storyboard row 2, then continue one row and one video node at a time.
- Every video node has an independent prompt whose local timeline begins at `00:00` and ends at that action's seed or added duration. Cumulative duration belongs only in the ledger, never in a video prompt.
- Every extension must take the previous complete video as input and return a longer complete video. Never substitute an isolated tail clip, concatenation, looping, freezing, retiming, or padding.
- Verify the measured total duration after every extension until it exactly matches the prompt-locked target. If an increment is unsupported, present supported values and reapprove the plan; never silently round.

### Lumina canvas graph

- `SPEC-vN`: global video specification
- `STORY-vN`: confirmed screenplay, master storyboard, and production ledger
- `CHAR-XX` / `SCENE-XX` / `PROP-XX`: reusable visual assets
- `FRAME-SEED-S0` / `FRAME-EXT-XX`: seed start and optional extension reference frames
- `VOICE-REF-vN` / `AUD-NAR-XX` / `AUD-SFX-XX` / `BGM-XX`: audio assets
- `VIDEO-SEED`: the short seed video awaiting user approval
- `VIDEO-EXT-XX`: sequential extensions based on the latest complete video
- `VIDEO-FINAL`: the verified complete video at target duration
- `EDIT-vN`: edit decisions and delivery verification

Every image reference requires both a real connection and a resolved `@` picker chip. Generation prompts live inside their Image, Video, or Audio Generation nodes; the skill does not create isolated prompt nodes for individual video steps.

### Production workflow

1. Inspect the canvas and resolve interaction and story languages.
2. Analyze and lock the screenplay, title, total duration, aspect ratio, resolution, narration, and color route.
3. Extract characters and scenes and initialize the versioned production ledger.
4. Generate and approve all character, scene, and prop assets; when dialogue or narration is required, generate and approve voice auditions.
5. Build and obtain approval for a master storyboard whose rows specify `storyboard_shot_id`, `shot_duration_seconds`, framing, movement, dependency versions, action, and sound.
6. Generate only `VIDEO-SEED` from row 1, with the exact row-1 duration and an independent prompt beginning at local `00:00`.
7. Present the first-shot video and stop until the user explicitly approves it; do not create the second video yet.
8. After approval, generate row 2 as `VIDEO-EXT-01`, then continue sequentially. Each row has an independent prompt starting at `00:00`, consumes the latest complete video, and returns a longer complete video until the target duration is reached.

### Example request

```text
Use this skill in the current Lumina canvas to turn my screenplay into a two-minute, 4:3, 1080p short drama in English. Use the GBH palette and approve the character assets, voices, and master storyboard first. Generate VIDEO-SEED at the full duration of storyboard row 1 and wait for my approval; only then generate video two from row 2 and continue extending to two minutes. Start every video prompt at 00:00.
```

## License

MIT
