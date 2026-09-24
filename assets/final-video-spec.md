# SPEC-vN | Final Video Spec 模板

> 色彩确认后，把本模板填写进 Lumina 持久 String/text 节点 `SPEC-v1 | Final Video Spec`；分镜设计、媒体资产生成、剪辑合成均以它为全局唯一基准。修改时递增版本并记录受影响的下游节点。

```
片名：[用户输入]
target_duration_seconds：[只取自用户直接提示词；缺失时先询问]
interaction_language：[从用户最新直接指令确定]
story_language：[明确指定 / 现有剧本语言 / interaction_language]
输出比例：[16:9 / 4:3 / 9:16]
清晰度：[1080p / 720p]
色彩设计方案：[A·GBH / B·自主设计 / C·自定义]
色彩规格：[确认后的色板与插槽分配]
图像生成节点/模型：[当前 Lumina 实际可用选择]
视频生成节点/模型：[存在时默认 Seedance 2.5；否则为已确认的等效模型]
声音生成节点/模型：[当前 Lumina 实际可用选择]
短片语言：[中 / 英 / 其他]；旁白、对白与标题卡均遵循此语言
旁白：[是 / 否]
分镜总汇表版本：[STORY-vN / approved]
第一分镜时长：[总汇表第一行 shot_duration_seconds；即 VIDEO-SEED 请求时长]
后续视频步数：[总汇表第 2 行至末行]
时长来源：VIDEO-SEED 严格等于总汇表第一行；第 k>=2 行的 shot_duration_seconds 严格等于 VIDEO-EXT-(k-1) 的 added_duration_seconds；所有行之和等于 target_duration_seconds
时长校验：每步预计累计时长 / 每步实测累计时长 / 最终目标时长
叙事梗概（逐镜）：
  镜1：[一句话]
  镜2：[一句话]
叙事模式：[线性 / 嵌套章节 / 时间跳变]
连贯锁（全片不变）：角色身份 / 服装主色与标志道具 / 色温
Lumina 节点前缀：SPEC / STORY / CHAR / SCENE / PROP / FRAME-SEED / FRAME-EXT / VIDEO-SEED / VIDEO-EXT / VIDEO-FINAL / VOICE-REF / AUD-NAR / AUD-SFX / BGM / EDIT
引用合约：图片到视频节点必须同时具备真实连接与 @ 选择器生成的已解析标签
生产路径：确认分镜总汇表 → 按第一行完整时长生成 VIDEO-SEED → 用户确认第一分镜视频 → 按第 2 行生成第二个视频 → 继续顺序延长 → 达到目标时长
提示词时钟：每个 VIDEO-SEED / VIDEO-EXT 节点都是独立 Prompt，时间轴从本地 00:00 开始；累计时码禁止进入 Prompt
生成顺序：规格确认 → 剧本锁定 → 角色资产确认 → 可选声音确认 → 分镜总汇表确认 → 按第 1 行生成 VIDEO-SEED → 用户明确确认 → 按第 2 行生成 VIDEO-EXT-01 → 其余行顺序延长 → VIDEO-FINAL 核验/交付
真实 Video Extension 能力：[可用 / 不可用 / 待确认]
不可用时的交付：已确认 VIDEO-SEED + 独立声音/资产节点 + EDIT-vN 限制说明；不得使用片段拼接冒充延长
版本：[v1]
批准状态：[draft / approved]
```
