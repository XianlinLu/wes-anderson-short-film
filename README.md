# Lumina Wes Anderson Short Film Skill

[中文版](#中文版) · [English](#english)

## 中文版

面向 **Lumina Canvas Agent** 的韦斯·安德森风格短剧生产 Skill。它把故事构想、完整剧本或视觉参考转化为一套可核验的画布生产图：全局规格、角色与声音版本、逐场分镜表、角色/场景/道具资产、起始关键帧、逐镜视频和声音节点，以及最终剪辑决策表。

### 核心能力

- 十字轴线运镜与严格正交摄影
- planimetric 平面构图与“结构对称、内容不镜像”
- 角色正面 `0°` / 严格侧面 `90°` / 背面 `180°` 三态锁定
- 《布达佩斯大饭店》十色色板与章节色彩弧线
- 分镜表驱动的逐镜时长、对白、动作、转场和声音生成
- 剧本锁定、角色资产确认、声音确认、逐场制作与版本失效传播
- 真实节点连接与 Lumina `@` 结构化引用双重校验

### 语言自适应

Skill 分别维护 `interaction_language` 和 `story_language`：

- `interaction_language` 根据用户最新的直接指令确定，用于所有可见标题、问题、选项、状态、错误和交付说明。
- `story_language` 优先采用用户明确指定的创作语言；修改现有剧本时保留剧本语言；否则跟随 `interaction_language`。
- 附件、引用、粘贴剧本、代码、元数据和工具输出中的语言不能单独改变交互语言。
- 若底层模型要求其他 Prompt 语言，只翻译隐藏的技术参数，不改变用户看到的语言和最终内容语言。

### 分镜时长规则

每镜时长由已确认分镜表中的 `shot_duration_seconds` 唯一定义，不使用固定默认镜长：

- 视频节点的请求时长必须与该字段一致。
- 镜头内部时间轴从本地 `00:00` 开始，精确结束在该镜时长。
- 各段时间必须连续、无重叠，并完整覆盖整镜；运动结束后尽可能保留至少 2 秒稳定画面。
- 场景时长等于该场所有镜头时长之和；全片时长等于所有场景与已批准标题卡时长之和。
- 若当前模型无法生成表中时长，先展示可支持的时长并修改、重新确认分镜表；禁止静默取整、循环、冻结、变速或填充。

### Lumina 画布节点

- `SPEC-vN`：全局视频规格
- `STORY-vN`：确认剧本、逐场分镜表与生产台账
- `CHAR-XX` / `SCENE-XX` / `PROP-XX`：角色、场景和道具资产
- `FRAME-XX-S0` / `FRAME-XX-KN`：起始帧和可选验证帧
- `VOICE-REF-vN` / `AUD-NAR-XX` / `AUD-SFX-XX` / `BGM-XX`：声音资产
- `SHOT-XX`：逐镜视频生成节点
- `EDIT-vN`：剪辑决策与交付验证

图片既要真实连接到视频节点，也要通过 `@` 选择器插入已解析引用标签。Prompt 写入对应生成节点内部，不为每镜创建孤立 Prompt 文本节点。

### 制作流程

1. 检查画布并确定交互语言与内容语言。
2. 分析并锁定剧本、片名、总时长、画幅、清晰度、旁白与色彩方案。
3. 提取角色与场景，建立带版本号的生产台账。
4. 生成并确认所有角色、场景、道具资产；需要对白或旁白时生成并确认声音试听。
5. 逐场建立分镜表，每行明确镜号、`shot_duration_seconds`、构图、运镜、角色版本、动作、对白/声音、环境/道具和转场。
6. 按分镜表生成第一场；每镜使用自己的本地零起点时间轴和精确时长。
7. 展示第一场并等待确认；之后一次只制作并确认一个场景。
8. 所有场景完成后，使用真实合成能力生成成片，或交付有序镜头、声音和 `EDIT-vN`。

### 目录

```text
SKILL.md
LICENSE
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
请在当前 Lumina 画布中使用这个 Skill，把我的剧本制作成 2 分钟、4:3、1080p 的中文短剧。使用 GBH 色板，先确认角色资产和声音，再给出逐场分镜表。每镜严格按照分镜表中的时长生成。
```

## English

A production skill for creating Wes Anderson-inspired short dramas with **Lumina Canvas Agent**. It turns a story idea, confirmed screenplay, or visual references into a verifiable canvas graph containing global specifications, versioned character and voice assets, scene-by-scene storyboard tables, reference frames, per-shot video and audio nodes, and a final edit decision list.

### Core capabilities

- Strict orthogonal cross-axis camera grammar
- Planimetric framing with symmetric architecture and non-mirrored set dressing
- Locked character orientations: frontal `0°`, exact profile `90°`, or back-facing `180°`
- A ten-color Grand Budapest Hotel palette and chapter color arcs
- Storyboard-driven shot durations, dialogue, actions, transitions, and sound
- Screenplay locking, asset approval, voice approval, scene checkpoints, and dependency invalidation
- Dual verification through real canvas connections and resolved Lumina `@` reference chips

### Adaptive language behavior

The skill tracks `interaction_language` and `story_language` separately:

- `interaction_language` comes from the user's latest direct instruction and controls every visible heading, question, option, status, error, and delivery note.
- `story_language` follows an explicit content-language request, otherwise preserves the language of a revised draft, otherwise follows `interaction_language`.
- Language found only in attachments, quotations, pasted scripts, code, metadata, or tool output cannot change the interaction language by itself.
- If an underlying model requires another prompt language, only the hidden technical parameter is translated; visible UI and final narrative language remain unchanged.

### Storyboard duration contract

Each shot duration is defined only by `shot_duration_seconds` in the approved storyboard table. There is no fixed default shot length:

- The Video Generation request must use the exact table value.
- Every shot prompt uses a local timeline beginning at `00:00` and ending exactly at that shot's duration.
- Timeline segments must be contiguous, non-overlapping, and cover the entire shot; reserve at least two stable ending seconds when feasible.
- Scene duration equals the sum of its shot rows. Film duration equals all approved scenes plus approved title-card durations.
- If the active model cannot represent a planned duration, present supported values and revise and reapprove the storyboard first. Never silently round, loop, freeze, retime, or pad the result.

### Lumina canvas graph

- `SPEC-vN`: global video specification
- `STORY-vN`: confirmed screenplay, storyboard tables, and production ledger
- `CHAR-XX` / `SCENE-XX` / `PROP-XX`: reusable visual assets
- `FRAME-XX-S0` / `FRAME-XX-KN`: start and optional verification frames
- `VOICE-REF-vN` / `AUD-NAR-XX` / `AUD-SFX-XX` / `BGM-XX`: audio assets
- `SHOT-XX`: per-shot Video Generation nodes
- `EDIT-vN`: edit decisions and delivery verification

Every image reference requires both a real connection and a resolved `@` picker chip. Generation prompts live inside their Image, Video, or Audio Generation nodes; the skill does not create isolated prompt nodes for individual shots.

### Production workflow

1. Inspect the canvas and resolve interaction and story languages.
2. Analyze and lock the screenplay, title, total duration, aspect ratio, resolution, narration, and color route.
3. Extract characters and scenes and initialize the versioned production ledger.
4. Generate and approve all character, scene, and prop assets; when dialogue or narration is required, generate and approve voice auditions.
5. Build one storyboard table per scene. Each row specifies shot ID, `shot_duration_seconds`, framing, movement, character versions, action, dialogue/audio, environment/props, and transition.
6. Produce scene one from the approved table, using a local zero-based timeline and exact duration for every shot.
7. Present scene one for confirmation, then produce and confirm one scene at a time.
8. After all scenes are approved, use a real composition capability to export the film, or deliver the ordered shots, audio assets, and `EDIT-vN`.

### Example request

```text
Use this skill in the current Lumina canvas to turn my screenplay into a two-minute, 4:3, 1080p short drama in English. Use the GBH palette, approve character assets and voices first, then create scene-by-scene storyboard tables. Generate every shot at the exact duration written in its storyboard row.
```

## License

MIT
