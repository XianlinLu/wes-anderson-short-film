# Lumina Canvas Production Manifest

> 把本模板填写进 `EDIT-vN | Edit Decision List`，用于复核实际画布与最终交付。所有节点字段必须使用当前画布的真实 ID 或精确节点名。

```text
SPEC 版本：[SPEC-vN] · 状态：[draft / approved]
STORY 版本：[STORY-vN] · 状态：[draft / approved]
分镜总汇表批准记录：[用户 / 时间 / approved]
target_duration_seconds：[用户直接提示词中的秒数]
输出：[比例] / [清晰度] / [语言]
interaction_language：[语言]
story_language：[语言]
真实 Video Extension 能力：[可用 / 不可用 / 待确认]
生产路径：短种子确认 → 顺序延长完整视频 → 目标时长核验

资产清单：
节点名/ID | 类型 | 内容 | 尺寸/比例 | 版本 | 批准状态

视频延长血缘：
storyboard_shot_id | shot_duration_seconds | video_step_id | mode(seed/extension) | requested_or_added_duration_seconds | expected_cumulative_duration_seconds | actual_cumulative_duration_seconds | previous_complete_video | output_complete_video | CHAR | SCENE | PROP | FRAME | 真实连接 | @ 引用 | 生成 | 用户批准/验证

声音绑定：
Layer | 节点名/ID | 类型 | 时长 | 目标镜头/时间线 | 实际连接 | 批准

单一完整视频与声音：
视频：[VIDEO-FINAL 节点名/ID；唯一完整视频]
旁白：[列出 AUD-NAR-XX、入点、出点、电平]
音效：[列出 AUD-SFX-XX、入点、出点、电平]
BGM：[列出 BGM-XX、入点、出点、电平]

转场与标题卡：
[切点 / 类型 / 时长 / 亮度匹配 / 状态]

验证：
[ ] 规格与分镜版本已批准
[ ] 分镜总汇表在视频生成前已获用户明确批准，所有行时长之和等于目标
[ ] VIDEO-SEED 对应总汇表第 1 行，请求/实测时长和 Prompt 结束时间均等于第一行 shot_duration_seconds
[ ] VIDEO-SEED 已单独生成、展示并获得用户明确批准
[ ] 用户批准第一分镜视频前没有创建第二个视频或 VIDEO-EXT 节点
[ ] VIDEO-EXT-01 对应总汇表第 2 行；后续节点继续逐行一对一映射
[ ] 每个 VIDEO-EXT 的唯一视频输入是上一版完整视频，输出是更长完整视频
[ ] 每个视频 Prompt 独立从 00:00 开始，结束时间等于对应行 shot_duration_seconds
[ ] 累计时长只存在于台账，不出现在视频 Prompt
[ ] 最终实测时长准确等于 target_duration_seconds
[ ] 每步图片真实连接与已解析 @ 引用一致
[ ] 角色、服装、道具、色温与轴线连续
[ ] 音频实际时长与连接已核验
[ ] 不存在独立尾段拼接、循环、冻结、变速或填充
[ ] 单一完整视频与 EDL 记录完整
[ ] 导出文件真实存在并可播放（无导出能力时标记 N/A）

失败与未决项：
[节点 / 错误 / 已尝试修正 / 下一步]

最终状态：[种子待确认 / 延长中 / 目标时长已验证 / 已导出 / 受限交付]
```
