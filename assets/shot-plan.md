# Video Step 计划模板（短种子确认后连续延长）

> 为每个视频生成动作填写一份。`VIDEO-SEED` 与每个 `VIDEO-EXT-XX` 都是独立节点、独立 Prompt 和独立本地时间轴；延长节点仍必须以上一版完整视频为输入。

```text
## Video Step [SEED / EXT-XX]
video_step_id：[VIDEO-SEED / VIDEO-EXT-XX]
mode：[seed / extension]
added_duration_seconds：[本次种子或新增时长 D]
expected_cumulative_duration_seconds：[仅用于台账，不得写进 Prompt]
previous_complete_video：[seed 填 none；extension 填上一节点真实名称/ID]
章节 / 叙事节拍：[名称]
依赖版本：[screenplay / storyboard / character asset / voice]

### 本次新增故事内容
只描述本次 D 秒新增的动作、对白、旁白、声音与收束状态；禁止复述或重演已经存在于输入完整视频中的内容。

### 布景描述
材质层：[材质词]
内容层：[布景在说这里发生过什么]
年代层：[1930s–1980s 物件清单]

### 角色朝向锁定（逐角色、逐表演段）
[角色名]：[正面0° / 严格侧对90°(左/右) / 背面180°]

### 平面构图锁定类型（五选一）
[标准平面 / 轴线纵深 / 轴对称特写 / 严格俯拍 / 严格正拍]

### 对称声明
结构层：[对称]
内容层：[不镜像，视觉重量平衡说明]

### 独立本地时间轴（完整覆盖 00:00–D）
[00:00–00:XX]: 代号 · 英文关键词 · 本次新增叙事事件
[00:XX–00:YY]: 代号 · 英文关键词 · 本次新增叙事事件
[00:YY–00:DD]: 代号 · 英文关键词 · 收束状态

注意：即使 previous_complete_video 已有 20 秒，本次新增 8 秒也必须写 00:00–00:08，禁止写 00:20–00:28。

### 连续性锚点
seed 起始状态：[仅 seed 填写：0 秒构图 / 人物朝向 / 场景中轴 / 道具位置]
extension 承接状态：[仅 extension 填写：上一版完整视频末帧的身份 / 服装 / 道具 / 轴线 / 运动方向 / 光线 / 声音]
本次结束状态：[供下一次延长继承]

### 色彩插槽
灵魂色 / 身份色 / 场景插槽 / 暗部色

### 声音标注
台词 / 旁白：[逐字文本]
主要音效：[描述] · 质感：[handmade/theatrical/naturalistic] · 强度：[background/foreground/statement]
BGM：[段落情绪与骨架乐器，如有]

### Lumina 节点绑定
角色元素：[CHAR-XX 真实节点名/ID，或 planned]
场景元素：[SCENE-XX 真实节点名/ID，或 planned]
道具元素：[PROP-XX 真实节点名/ID，或 planned]
参考帧：[FRAME-SEED-S0 / FRAME-EXT-XX 真实节点名/ID，或 none]
上一版完整视频：[真实节点名/ID，seed 为 none]
输出完整视频：[VIDEO-SEED / VIDEO-EXT-XX 真实节点名/ID，或 planned]
旁白节点：[AUD-NAR-XX 真实节点名/ID，或 off]
音效节点：[AUD-SFX-XX 真实节点名/ID，或 off]
BGM 节点：[BGM-XX 真实节点名/ID，或 off]
连接状态：[planned / connected / verified]
@ 引用状态：[planned / resolved / failed]
生成状态：[planned / queued / generated / approved / failed]
实测累计时长：[生成后填写]
```

## 示例：第二次延长新增 8 秒

```text
video_step_id：VIDEO-EXT-02
mode：extension
added_duration_seconds：8
expected_cumulative_duration_seconds：24
previous_complete_video：VIDEO-EXT-01

### 独立本地时间轴
[00:00–00:02]: M-00 · static locked shot, camera strictly perpendicular to scene plane · 从输入视频末帧状态继续，角色A保持严格侧面读取信封。
[00:02–00:06]: M-XL · lateral dolly strictly parallel to scene plane, camera moves only on the X-axis · 角色A与摄影机沿X轴移动至柜台中央。
[00:06–00:08]: M-00 · static locked shot, camera strictly perpendicular to scene plane · 角色A停住，信封置于中央，形成下一次延长的稳定末帧。
```
