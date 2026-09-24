# Lumina 短剧视频制作流程

> 用于剧本和创作方向已经确认、即将进入角色资产、声音、分镜与视频生产的阶段。韦斯·安德森视觉语法仍由 `03-storyboard-design.md`、`04-camera-and-movement.md`、`05-prompt-writing.md` 和 `06-gbh-palette.md` 约束。

## 一、能力预检

检查当前 Lumina Agent 实际连接的动作和节点。完整流程需要：

- 可使用参考图的图像生成；
- 图像参考视频生成；
- 视频时长和比例元数据；
- 用户要求对白或旁白时所需的 TTS/音频生成；
- 用户要求连续单片时，可选的完整视频顺序扩展或真实合成能力。

只使用真实存在的能力。缺少必要能力时在最早安全节点停止，保留已完成内容并说明限制；不虚构节点、句柄、文件、时长或成功结果。

## 二、锁定剧本与生产台账

不要把长草稿自动视为定稿。先确认故事方向与剧本文本，再建立 `STORY-vN` 生产台账：

```yaml
screenplay_version: 1
interaction_language: zh-CN
story_language: zh-CN
characters:
  character_id:
    profile_version: 1
    asset_version: 1
    asset_status: pending | confirmed | revise
    voice_version: 1
    voice_status: off | pending | confirmed | revise
scenes:
  scene_id:
    script_version: 1
    storyboard_version: 1
    storyboard_status: pending | confirmed | revise
    planned_duration_seconds: number
    video_status: pending | confirmed | revise | stale
    shots:
      shot_id:
        shot_duration_seconds: number
        asset_versions: map
        audio_versions: map
        status: planned | generated | confirmed | revise | stale
```

任何下游节点不得使用 `pending`、`revise` 或 `stale` 的依赖。

## 三、角色、场景和道具资产

从确认剧本中提取所有影响剧情或出现在生成场景中的主要角色。为每个角色创建稳定 `character_id` 与角色圣经：可见外形、发型、比例、服装层次、鞋履、配件、身份色、标志道具、姿态习惯、关系压力和情绪基线。把剧本明确事实与制作选择分开；缺失信息只有在会显著改变设计时才询问。

按照本项目角色元素规范，为每个角色创建独立 `CHAR-XX`。同时建立需要复用的 `SCENE-XX` 与 `PROP-XX`。展示全部资产及其版本并停止，等待逐项确认。所有角色资产确认前，不开始声音试听或视频生成。

角色外观被修改时只重生成该角色，递增 `asset_version`，并把包含该角色的关键帧和视频标记为 `stale`。

## 四、声音选择与锁定

只有用户启用对白或旁白时才执行：

1. 每个需要说话的角色或旁白者创建一个独立 `VOICE-REF-vN`，生成 20–30 秒原创试听。
2. 试听使用 `story_language`，覆盖平静、交流、紧张、决断和收束语气，不模仿真人、演员或受保护角色。
3. 展示全部试听并等待逐项确认；任何声音未确认时，不生成场景对白或相关视频。
4. 确认后记录 `character_id → voice handle + voice_version + language + delivery profile`。

声音改变时递增 `voice_version`，把该角色对白和所有依赖视频标记为 `stale`。没有角色的剧本跳过角色试听；需要旁白时可直接使用经确认的中性旁白音色。

## 五、逐场分镜表

一次只处理一个场景。记录场景地点、时间、环境、角色、戏剧目的、进入状态、退出状态、动作、道具、对白顺序、情绪、停顿和连续性。

每镜一行：

```text
shot_id | shot_duration_seconds | framing | camera_code_and_keywords | character_and_asset_versions | action | dialogue_or_audio_cue | environment_and_props | transition
```

### 时长单一事实源

- `shot_duration_seconds` 是该镜时长的唯一权威字段。不存在固定默认镜长。
- 场景计划时长必须等于该场所有镜头字段之和；全片计划时长必须等于所有场景与已批准标题卡时长之和。
- 每条 Prompt 的时间轴都从本镜本地 `00:00` 开始，并精确结束于 `shot_duration_seconds`。禁止把全片累计时间写入视频节点 Prompt。
- 时间段连续、无重叠、无空洞并覆盖整镜。运动结束后尽可能保留至少 2 秒稳定画面；短镜头无法保留 2 秒时，在分镜确认阶段明确说明。
- 视频节点的请求时长、声音计划、关键帧和 `EDIT-vN` 入出点必须引用同一字段，不得分别推断。
- 提交前检查当前模型支持的离散时长、最短/最长时长与步长。精确时长不可表示时，展示支持值并修改、重新确认分镜表；禁止静默取整。

分镜表获确认后才生成本场声音、关键帧和视频。

## 六、逐镜声音与关键帧

- 按剧本顺序为本场每条对白生成声音，使用已确认 `voice_version`，保留台词、情绪、停顿、中断和说话顺序。
- 只重试失败的音频单元；连接视频前验证实际编码时长。短对白优先放入带自然停顿或房间底噪的场景混音，不新增台词。
- 每镜创建 `FRAME-XX-S0`；只有轴线行动或收束状态需要额外验证时才创建 `FRAME-XX-KN`。
- 关键帧、音频和视频节点均记录其依赖的角色、道具、声音和分镜版本。

## 七、视频生成路径

根据用户意图和真实能力选择一种路径，并记录在 `SPEC-vN`：

### 路径 A：逐镜节点与真实合成

为每个分镜行创建一个 `SHOT-XX`，请求时长严格等于 `shot_duration_seconds`。完整 Prompt 放在节点内部，相关图片同时具备真实连线与已解析 `@` 标签。逐镜验证实际时长、身份、动作、轴线、道具和结尾状态。最后使用真实合成能力按 `EDIT-vN` 组装；没有合成能力时只交付有序镜头与 EDL，不声称存在连续成片。

### 路径 B：连续完整视频扩展

只有扩展动作接收最新完整视频并返回更长完整视频时使用。先从第一行或一个经确认的镜头组生成初始视频；后续扩展严格串行，每次输入上一版完整视频。每次扩展：

1. 只描述本次新增的分镜行动。
2. 时间轴从本次调用的 `00:00` 开始，结束于本次增加的表内时长。
3. 保持角色、场景几何、光线、道具、运动方向、色彩和声音策略。
4. 等待返回完整视频，验证累计时长确实增加后才进入下一次扩展。

尾部片段、独立剪辑或简单拼接不属于真实扩展。禁止用循环、冻结、变速、填充或静默取整达到目标时长。若扩展能力的时长粒度无法对应分镜行，可在分镜确认阶段合并连续行或修改时长，但必须重新确认。

## 八、逐场确认

生成并展示第一场实际结果及依赖版本摘要，将状态设为 `pending` 并停止。用户确认后才设为 `confirmed` 并处理下一场。一次只生成和确认一个场景。

如果用户修改剧本、外观、声音或分镜：

- 递增对应版本；
- 标记直接依赖和所有下游节点为 `stale`；
- 只按依赖顺序重新生成失效部分；
- 连续扩展链中的某个检查点被修改时，该检查点和所有后续检查点都失效。

## 九、完成判定

只有全部必需角色资产、声音、分镜、场景视频和编辑结果均已确认时，流程才完成。最终用 `interaction_language` 报告：

- 剧本、角色、声音和分镜版本；
- 每场分镜表与计划/实测时长；
- 每镜节点、引用和音频状态；
- 逐镜合成路径或连续扩展 lineage；
- 实际最终时长、比例、声音和导出状态；
- 任何真实限制或未完成项。
