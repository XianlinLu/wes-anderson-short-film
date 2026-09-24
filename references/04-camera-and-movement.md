# 十字轴线镜头与对象运动系统

> 本 Skill 最核心的技术规格。分镜设计、逐镜提示词生成、关键帧验证均以此为准。

## 核心约束

摄影机始终与被摄平面严格正交。镜头运动只允许中央 Z 轴推拉、X 轴横移或 Y 轴升降；画面内人物、车辆、火车和道具同样只允许 X / Y / 中央 Z 轴的单一直线路径。摄影机旋转、摇镜、倾斜与对象斜向 / 曲线路径均不属于本 Skill 的语法。每一段运动前后都必须落在可读的二维静帧。

## 代号表

| 代号 | 中文名 | 轴向 | 触发条件 | 提示词关键词 |
|------|--------|------|---------|------------|
| **M-00** | 静止锁定 | — | 供读取画面；T-10 道具特写；对话面部；框景锁定 | `static locked shot, no camera movement, camera strictly perpendicular to scene plane` |
| **M-Z+** | 快速推焦 | Z 轴推近 | 揭示道具 / 细节 / 人物表情；框景内主体拉近 | `sudden snap zoom in, mechanical zoom acceleration, central axis locked, planimetric framing maintained` |
| **M-Z-** | 快速拉焦 | Z 轴拉远 | 揭示空间规模；场景全貌；章节收束 | `sudden snap zoom out, symmetrical architecture expanding from both sides, central axis maintained` |
| **M-XL** | 水平横移 | X 轴（左↔右）| 展现空间连续横截面；人物横向入场；横向游行构图 | `lateral dolly strictly parallel to scene plane, camera body moves sideways, does not rotate, never pushes into depth` |
| **M-YU** | 垂直升起 | Y 轴（↑）| 沿同一正面平面直线上移，揭示建筑 / 制度空间；不仰俯摇动 | `vertical camera rise only on the Y-axis, camera remains perpendicular to the same scene plane, no tilt` |
| **M-YD** | 垂直下降 | Y 轴（↓）| 沿同一正面平面直线下降，揭示地面或垂直空间信息；不仰俯摇动 | `vertical camera descent only on the Y-axis, camera remains perpendicular to the same scene plane, no tilt` |
| **P-X / P-Y / P-Z** | 对象轴线运动 | X / Y / 中央 Z 轴 | 仅在 M-00 段使用；人物、车辆、火车或道具沿单一轴线直线移动 | `locked camera; subject moves only [left/right/up/down/straight along the central depth axis], no diagonal path` |

## 禁止列表

```
handheld · dutch tilt · rack focus · shaky cam · pan · whip pan · camera pivot · camera tilt ·
arc shot · orbit · 360-degree pan · continuous rotation ·
slow zoom · gradual zoom · slow push · diagonal dolly · oblique tracking · POV handheld push ·
diagonal subject movement · curved subject path · circular subject movement
```

## 正交平面转换

当叙事必须从一个「面」切换到另一个「面」时，用 `[CUT →]` 直接切至新的正交静帧；例如人物 A 正面近景 `[CUT →]` 人物 B 正面近景，或汽车正面车灯 `[CUT →]` 严格正前方的车头第一视角。不得以镜头原地转 90° 或俯仰 90° 模拟切换。

## 运镜分配约束（设计阶段执行）

- 静止不是例外，而是本风格的基本句法：允许整镜 M-00，只要画面内表演、台词、道具读取或对象的单轴运动承担叙事。
- 快速中轴推拉只用于「揭示」；横移 / 竖移只在空间或对象确有对应轴向行动时使用。每段运动须有明确叙事理由，避免无目的炫技。
- 每个 30 秒镜头最多一次镜内 `[CUT →]`；其余时间保持同一场景平面，或在该平面内以静止与单轴行动推进。
- 开场、章节首镜、高潮与结尾都可采用 M-00；不要以强制运动取代画面信息。若采用运动，运动后的稳定画面必须承担信息读取或情绪落点。

| 场景类型 | 优先设计 | 叙事用途 |
|---------|----------|---------|
| A 室内社交 | M-00（正面台词 / 对象 P-X）→ `[CUT →]` → M-00（另一正面） | 平面正反打，或人物横向进出形成节拍 |
| B 外景街道 | M-Z- → M-00 → M-XL | 拉远建立立面系统，再横向展开街道截面 |
| C 夜间行动 | M-00 + P-X → M-Z+ → M-00 | 锁定镜头内横向行动，推近目标后静止读取 |
| D 私人空间 | M-00 + P-Z → M-00 | 人物背对镜头沿中央轴走向门洞 / 书桌，停留收束 |
| E 制度权力 | M-Z- 或 M-YU → M-00 | 沿单轴揭示压迫性建筑，落在正面稳定构图 |
| F 档案记忆 / T-10 道具 | M-00 | 严格俯视或正面平拍，供完整读取手部、文字或物件 |
| G 剖面建筑 | M-Z- → M-00 | 只在满足剖面使用条件时，以中轴拉远揭示系统 |
| 情绪顶点 / 框景 | M-Z+ → M-00，或 M-00 + P-Z | 镜头沿中轴推近，或人物 / 物体沿中轴靠近、离开 |
