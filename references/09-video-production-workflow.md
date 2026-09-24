# Lumina 短片首段确认与连续延长流程

> 用于剧本、视觉方向和资产已经确认，准备生成视频的阶段。唯一视频生产路线是：短首段试制 → 用户确认 → 基于最新完整视频串行延长 → 达到用户直接提示词中的目标总时长。

## 一、能力预检

开始生成前检查当前 Lumina Agent 的真实能力。必需能力：

- 参考图像生成或可复用的已上传图片；
- 从关键帧生成短视频；
- 接收最新完整视频并返回更长完整视频的真实 Video Extension；
- 返回可靠的视频实际时长与比例元数据。

声音能力只在用户启用对白、旁白、音效或 BGM 时需要。若缺少真实 Video Extension，仍可完成分镜、资产和一段经确认的短首段，但必须在此停止并报告限制。禁止改用多个独立片段拼接来冒充延长。

## 二、锁定目标总时长

`target_duration_seconds` 只能从用户最新的直接提示词解析，支持秒、分钟、分秒混合或时钟写法。不要从附件、剧本长度、分镜数量、示例或默认值推断。

若缺失或有歧义，使用 `interaction_language` 询问一个纯技术问题并停止。不要先生成首段。

读取实际视频动作的限制：

- 首段允许的时长；
- 单次扩展可增加的时长与步长；
- 最大累计时长；
- 实际时长元数据精度。

选择最短且足以判断角色、构图、色彩和运动语法的首段时长，并计算有序扩展计划。若精确目标不可达，先展示可支持的最近结果并等待用户选择；禁止静默取整。

## 三、生产台账

在 `STORY-vN` 中记录：

```yaml
screenplay_version: 1
storyboard_version: 1
interaction_language: zh-CN
story_language: zh-CN
target_duration_seconds: number
seed:
  node: VIDEO-SEED
  requested_duration_seconds: number
  actual_duration_seconds: null
  status: planned | generated | confirmed | revise | failed
extensions:
  - step_id: VIDEO-EXT-01
    input_complete_video: null
    added_duration_seconds: number
    expected_cumulative_duration_seconds: number
    actual_cumulative_duration_seconds: null
    status: planned | generated | verified | revise | failed
final_video:
  node_or_handle: null
  verified_duration_seconds: null
```

同时记录角色、场景、道具、声音和关键帧版本。任何 `pending`、`revise`、`stale` 或 `failed` 依赖不得进入视频生成。

## 四、生成步骤分镜表

把完整故事映射为一个首段行和若干扩展行：

```text
video_step_id | mode(seed/extension) | added_duration_seconds | expected_cumulative_duration_seconds | framing | camera_code_and_keywords | characters_and_versions | new_action | dialogue_or_audio | environment_and_props | continuation_end_state
```

规则：

- 首段行的 `added_duration_seconds` 是首段请求时长。
- 每个扩展行的 `added_duration_seconds` 只是本次调用新增的时长，不是累计总时长。
- `expected_cumulative_duration_seconds` 只用于台账和进度核验，禁止写进视频 Prompt。
- 所有新增时长之和必须精确等于 `target_duration_seconds`。
- 每行只描述一个生成调用能够执行的新故事段落，并以可继续延长的稳定画面结束。
- 韦斯·安德森构图、角色三态朝向、十字轴线运镜、GBH 色彩与年代锚定继续适用。

确认该表后才生成视频。

## 五、每个视频 Prompt 独立计时

`VIDEO-SEED` 和每个 `VIDEO-EXT-XX` 都有自己独立的 Prompt。不得把一个全局长时间轴复制到多个节点。

每个 Prompt 必须：

1. 从本次调用的 `00:00` 开始；
2. 最后一个时间戳等于本行的 `added_duration_seconds`；
3. 只描述本次调用要生成或新增的内容；
4. 使用连续、无重叠、无空洞的局部时间段；
5. 不出现任何全片累计时间或上一段时间范围。

例如，影片已有 20 秒，本次扩展 8 秒，正确写法是：

```text
00:00-00:02 保持上一完整视频的结尾构图，角色正面静止。
00:02-00:06 角色沿画面 X 轴直线移动，摄影机保持 M-00。
00:06-00:08 角色停在中央门框，稳定收束。
```

错误写法是 `00:20-00:28`。累计位置只记录在 `expected_cumulative_duration_seconds`，不进入 Prompt。

## 六、短首段试制与强制确认

1. 创建 `FRAME-SEED-S0`，验证角色身份、服装、道具、正交构图、色彩和年代。
2. 创建且只创建一个 `VIDEO-SEED` Video Generation 节点。
3. 把完整独立 Prompt 写入节点内部；图片使用真实连接与已解析 `@` 标签。
4. 生成短首段并验证它可以播放、实际时长正确、比例正确、人物一致、运动符合轴线规则、结尾适合继续。
5. 向用户展示真实首段，提供本地化的确认或修改选项，然后结束当前执行。

在 `seed.status` 变为 `confirmed` 前：

- 不创建任何 `VIDEO-EXT-XX`；
- 不调用扩展动作；
- 不生成最终 BGM 或宣称视频生产已开始批量运行；
- 不把沉默、查看或下载视为确认。

用户要求修改时，只修正并重生成 `VIDEO-SEED`，再次展示同一确认门槛。

## 七、串行真实扩展

首段明确确认后，按表逐个处理扩展行。每个 `VIDEO-EXT-XX`：

1. 输入上一步返回的最新完整视频；第一扩展输入已确认的 `VIDEO-SEED`。
2. 使用一个新的独立 Prompt，从 `00:00` 开始，结束于本次 `added_duration_seconds`。
3. 只描述要新增的动作、剧情、声音和收束状态。
4. 保持角色身份、服装、场景几何、光线、色彩、道具状态、动作方向和声音策略。
5. 等待返回完整视频，不并行启动依赖扩展。
6. 验证结果是更长的完整视频，且实际累计时长等于计划值。
7. 只有验证通过，才把该输出作为下一扩展输入。

以下结果均视为失败：

- 只返回新尾段，而不是更长完整视频；
- 实际时长没有按预期增加；
- 角色或场景无意重置；
- 使用旧视频而不是最新完整视频作为输入；
- 通过独立片段拼接、循环、冻结、变速、补帧或静默取整达到目标。

失败时保留上一个已验证完整视频，只修复并重试当前扩展；不得重启成功的上游步骤。

## 八、声音与最终完成

对白或旁白必须使用已确认声音，并在依赖视频调用前完成时长验证。原创 BGM 可以在首段确认后生成，时长以最终目标为准。

嵌入声音只允许：

1. 通过同一连续视频链的原生音频输入；或
2. 使用接收一个完整视频并保持其画面时长不变的单视频混音/替换动作。

不得为了添加音乐或旁白而拆分、拼接多个视频。没有合适能力时，分别交付完整视频和音频，并提供同步说明。

最终完成必须同时满足：

- 首段存在明确用户确认；
- lineage 为 `VIDEO-SEED → VIDEO-EXT-01 → … → VIDEO-FINAL`；
- 每一步都输入上一版最新完整视频；
- 每个 Prompt 都从本地 `00:00` 开始；
- 最终实测时长准确等于 `target_duration_seconds`；若动作无法精确达到，必须先让用户修改并重新确认目标或延长计划，不能用容差替代；
- 没有独立片段拼接、循环、冻结、变速、填充或静默取整；
- 角色、情节、视觉风格、动作、道具、场景和声音连续。

最终用 `interaction_language` 报告锁定与实测时长、首段审批状态、完整扩展 lineage、声音状态、最终视频节点/句柄以及任何真实限制。
