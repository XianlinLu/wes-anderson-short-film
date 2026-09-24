# 提示词生成规则

---

## 【前置步骤】视觉规格转化

将用户输入转化为正向物理描述，解决两类问题：
- **叙事干扰**：过滤叙事语境，只传递物理描述
- **概念字面化**：将隐喻 / 概念翻译为具体视觉规格

**第一步 · 过滤叙事语境**
保留：场景在哪里 / 有什么 / 有谁（物理描述）
过滤：情绪意图 / 情节功能 / 概念隐喻 / 叙事动词

**第二步 · 识别空间风险**（命中任一 → 第三步替换）
- 信号一：概念 / 隐喻描述空间，无材质词
- 信号二：叙事功能 × 宏大规模 × 观看行为同时出现
- 信号三：景深层次无法被数出（开阔自然 / 无边界城市）
- 信号四：无任何物理边界描述

**第三步 · 空间兼容性判断**

安德森兼容空间条件（全部满足）：3 层可数景深 + 可用材质词描述 + 人体相对尺度 + 有围合结构

不满足时执行替换：

| 原始空间 | 错误原型 | 替换方向 |
|---------|---------|---------|
| 竞技 / 观众 / 擂台 | 戏剧舞台 | 围墙庭院 / 室内竞技厅 |
| 概念隐喻空间 | 建筑模型 / 博物馆陈列 | 最近的具体物理空间 |
| 开阔山野 | CG 风景 | 建筑外立面 + 自然退为背景 |
| 皇宫大殿 | 影视城搭景 | 室内对称接待厅 / 宫廷偏厅 |
| 无边界城市街道 | 透视纵深街景 | 单条小街，建筑立面封底 |

**第四步 · 填充物理规格**

```
【空间规格】
· 空间类型：[封闭室内 / 半开放廊道 / 围合室外]
· 平面形状：[方形 / 矩形 / 纵深走廊]
· 尺度感：[人体尺度 / 宏大]
· 边界材质：[左右墙] + [地面] + [天花板/顶部]

【光源规格】
· 光源数量 / 主光源位置+色温+方向 / 副光源（如有）/ 整体明暗

【道具规格】
· 固定道具（3–5 项，含材质位置）
· 功能性道具（1–3 项）
· 文字道具（语言+字体风格+载体形式）
· 道具密度

【摄影规格】
· 起始帧：0 秒静止画面，指定正面平视 / 严格俯视 / 严格仰视 / 中央轴线纵深之一；人物朝向、门洞中轴、道具位置均可读
· 构图：[轴对称 / 轴线纵深（汇聚点居中）]
· 景深
· 镜头运动：[M-00 / M-Z+ / M-Z- / M-XL / M-YU / M-YD 之一；写对应英文关键词]
· 对象运动：[仅在 M-00 时，P-X / P-Y / P-Z 之一；写对象的直线轴向、起止位置]
· 收束帧：运动后至少 2 秒的正交静帧，供下一镜硬切或镜尾停留

【人物规格（如有）】
· 人数 / 站位 / 人物与空间比例 / 动作状态

【色彩规格】
· 主色调 / 辅色 / 点缀色 / 整体饱和度
```

**第五步 · 比例锚点传递**：人物与空间比例在此统一定义，角色图与场景图共享同一比例值。

GBH 色板（十色体系、色彩插槽、色彩弧线）见 `references/06-gbh-palette.md`。

---

## 一、输出比例与构图特征

输出比例在阶段 2 选定后锁定全片，不因情绪变化而切换。在此比例内通过色彩、构图疏密、人物占比实现情绪分化。

| 输出比例 | 视觉特征 | 适合题材 |
|---------|---------|---------|
| **16:9** | 标准宽幕，横向空间充裕，建筑立面展示效果最佳 | 外景 / 旅程 / 制度 / 城市叙事 |
| **4:3** | 接近学院比例，画面更方正紧凑，亲密感与仪式感强 | 室内社交 / 私人空间 / 章节化叙事 |
| **9:16（竖屏）** | 垂直画幅，单角色突出，上下留白可强化孤独感或建筑纵深 | 单人叙事 / 手机优先 / 短视频场景 |

> **注意**：提示词中的比例约束字段由此决定。9:16 竖屏时，对称构图的「中轴」变为垂直中轴，M-YU / M-YD 运镜叙事效果更强，M-XL 横移相对弱化。

---

## 二、色彩插槽系统

| 插槽 | 功能 | GBH 默认值 |
|-----|------|----------|
| 灵魂色 | 全片主调 | 浅玫粉 `#E3A6AC`（由情绪调整） |
| 身份色 | 主角制服 / 标志道具 | 深玫红 `#A34860` |
| 室内暖调 | 社交 / 人情 / 记忆空间 | 奶黄琥珀 `#F4DBB2` |
| 外景冷调 | 城市 / 等待 / 公共空间 | 钢铁蓝 `#3A6070` |
| 暗部色 | 危险 / 夜行 / 冲突 | 深棕墨 `#2C1F14` |
| 私人色 | 书房 / 独处空间 | 橄榄鼠尾草 `#637040` |
| 制度色 | 权力 / 压迫 / 官僚 | 冷灰 `#5A6070` |
| 档案色 | 回忆 / 历史 / B&W | 全灰阶 |

年代跳变时：灵魂色插槽随年代切换——辉煌期填入暖饱和色，衰败期切换为去饱和冷色，饱和度同步下降，与布景材质退化描述同步执行。
章节跳变时：灵魂色插槽改为逐章指定，其余插槽（室内暖调 / 外景冷调等）保持不变。

### 服装与布景的色彩对话

三种策略：
- **反差对比（制造焦点）**：服装色与背景明显区分。深色制服 + 暖黄环境 → 制度感异质性
- **同色系融入（制造归属感）**：服装接近布景色调。紫色制服 + 粉色酒店 → 角色与场所合并
- **点睛色**：绝大多数中性色，唯有关键道具或局部一个强调色

**硬约束（无例外）**：同帧内服装色不得与背景主色完全相同。最低限度：饱和度差一级或明度有明显区分。

### 章节色彩弧线设计

五种弧线类型：

| 弧线类型 | 颜色走向 | 叙事功能 | 适用题材 |
|---------|---------|---------|---------|
| **降温弧** | 暖饱和 → 中温 → 冷去饱和 | 繁荣 → 衰退 → 消亡 | 衰落叙事 |
| **升温弧** | 冷去饱和 → 中温 → 暖饱和 | 压迫 → 挣扎 → 解放 | 压抑 → 解放结尾 |
| **对比弧** | 章 A 暖 + 章 B 冷 → 消解（灰 / 黑）| 两种对立视角无法整合 | 多视角 / 矛盾现实 |
| **渐褪弧** | 同灵魂色，饱和度章章递减 | 记忆正在消失 / 存在感消退 | 回忆 / 失去题材 |
| **积重弧** | 同灵魂色，明度章章降低 | 压力 / 重量在累积 | 宿命 / 压抑题材 |

---

## 三、构图规则（所有镜头通用）

第 1–3 条为每镜强制写入；第 4–6 条酌情添加，每镜至少补充其中一条：

1. ★ `planimetric composition` `flat framing` `camera perpendicular to scene`
   — 轴线纵深合法条件：摄影机在中央对称轴上 + 汇聚点居中 + 左右等量对称
   — 提示词：`camera on central axis, centered vanishing point, symmetric perspective depth`
   — 禁止偏轴斜角
2. ★ `centered symmetry` `bilateral architectural symmetry`（仅结构层，内容层不镜像）
3. ★ 运镜轴向约束关键词（从 §运镜快查表直接引用 M-代号英文关键词，确保 `perpendicular` / `central axis` / `no arc` 等约束词出现在运镜描述段中）
4. 尺度博弈：`tiny figure dwarfed by architecture` / `human-scale proportion` / `figure filling frame`
5. `prominent prop as co-protagonist`
6. `signage integrated into composition`
7. `multiple visible spatial layers` `readable depth`

---

## 四、视频提示词结构（种子与延长）

在 Lumina 中，把完整提示词直接写入 `VIDEO-SEED` 或对应 `VIDEO-EXT-XX` 节点。每个节点的 Prompt 都是独立指令，时间轴一律从本节点的本地 `00:00` 开始；禁止沿用全片累计时码。以下 `@CHAR-XX` 等仅表示标签位置；图片必须同时真实连接并通过 `@` 选择器插入已解析标签。

### A. `VIDEO-SEED` 独立提示词

```
@CHAR-XX [已解析角色元素图：身份/人脸]
@SCENE-XX [已解析场景元素图：布景/色彩]
@PROP-XX [仅在种子段实际使用关键道具时插入]
@FRAME-SEED-S0 [已解析 0 秒正交构图、人物朝向与物体位置锚点]

[平面构图锁定语（见下，头部前置锚定）]
[场景类型A–G] Wes Anderson film still,
[GBH色彩总括行 或 自定义色彩规格]
[布景三层：材质层+内容层+年代层]
[角色描述：服装+站位+道具+角色朝向锁定语]
[第一分镜完整时长 D 秒的本地时间轴：D 必须等于已批准分镜总汇表第一行 shot_duration_seconds；00:00 起始静帧 → 单一X/Y/中央Z轴运动或一次[CUT →] → 尽可能至少2秒收束静帧；仅使用M-00/M-Z+/M-Z-/M-XL/M-YU/M-YD或M-00内对象P-X/P-Y/P-Z]
[NEGATIVE PROMPT（见下）]
[质量后缀（见下）]
```

### B. `VIDEO-EXT-XX` 独立提示词

```text
@PREVIOUS-COMPLETE-VIDEO [上一节点输出的完整视频；必须真实连接]
@CHAR-XX / @SCENE-XX / @PROP-XX [仅本次新增叙事真正需要时插入]
@FRAME-EXT-XX [可选新增段参考帧]

Extend the connected complete video by D seconds and return the longer complete video.
Preserve the exact final frame state, character identity, wardrobe, props, palette, geometry, camera axis, motion direction, lighting, grain, and audio continuity from the input video.
[只描述本次新增叙事，不复述已完成内容]
[本次新增 D 秒的独立本地时间轴：00:00–00:XX；不得写累计时码]
[平面构图、运镜、角色朝向和声音约束]
Do not restart the story, do not generate an isolated tail clip, and do not replace or shorten the connected input video.
[NEGATIVE PROMPT]
[质量后缀]
```

**独立提示词时长规则**：`D = 用户已批准分镜总汇表对应行的 shot_duration_seconds`。对 `VIDEO-SEED`，D 来自第一行；对第 `k >= 2` 个分镜，`VIDEO-EXT-(k-1).added_duration_seconds = D`。每段以准确秒数连续、无重叠地覆盖完整 `[00:00–D]`；`expected_cumulative_duration_seconds` 只记在台账中，绝不写入 Prompt。例如上一版完整视频已到 20 秒、本次对应分镜为 8 秒，正确写法仍是 `00:00–00:08`，错误写法是 `00:20–00:28`。模型不支持该行时长时，必须回到分镜总汇表修改并重新获得用户确认；禁止自行缩短第一分镜或重新分配行时长。单段只允许摄影机或对象之一作为主导运动者；镜内硬切写作 `[CUT →]`，两端必须都是稳定正交画面。

**Lumina 提交前检查**：节点名与 `video_step_id` 一致；Prompt 在视频节点内部；每个引用都有真实连线和已解析 `@` 标签；模型、画幅、清晰度和新增时长与 `SPEC-vN` 及生成步骤表一致；延长节点真实连接上一版完整视频；引用总数只覆盖本次需要的资产；含引用标签的完整 Prompt 不超过当前节点限制（未显示更小限制时以 4,500 字符为上限）。

### 角色朝向锁定语（每个出现人物的表演段必写）

- 在角色描述后，按该人物当前表演状态原样写入其一：`facing camera directly, face and torso frontal, 0-degree orientation`、`strict 90-degree profile facing [left/right], face and torso in exact side view`、`back facing camera, 180-degree orientation, face fully hidden`。
- 同一段内角色保持所选朝向；若分段改变朝向，时间轴须写明完成的 90° 或 180° 转身，新的表演段立即改用对应锁定语。
- 每个视频步骤的负向提示词追加：`no three-quarter face, no three-quarter body pose, no oblique eyeline, no over-the-shoulder shot, no diagonal stance, no sustained in-between turning pose`。

### 平面构图锁定语（五选一，每个视频 Prompt 头部前置）

```
// 标准平面（A–F 类、T-10）
camera strictly perpendicular to scene plane throughout, planimetric framing locked, no camera rotation, no perspective drift

// 轴线纵深（走廊/车厢/隧道）
camera fixed on central axis of [space], centered vanishing point, symmetric perspective maintained, no off-axis drift

// 轴对称特写（T-10 道具/标题卡）
overhead or frontal orthographic framing, flat studio lighting, object centered and filling frame

// 严格俯拍（信件/书本/托盘/桌面等水平面道具与手部动作）
direct overhead orthographic view, camera perpendicular to the horizontal surface plane, object and hands centered, no perspective tilt, no depth of field

// 严格正拍（标牌/瓶标/文件墙/画框等垂直面内容）
direct frontal orthographic view, camera centered and level with the vertical surface plane, flat frontal composition, no upward or downward tilt, no depth of field
```

### 质量后缀（每镜必追加）

```
35mm film still aesthetic, analog film grain, physical set photography,
practical light sources with natural warmth and fall-off,
handcrafted surfaces with visible age and imperfection,
muted period-correct analog color, not over-saturated,
fine physical detail, crisp natural texture
```

---

## 五、镜头与对象轴线提示词快查表

> 每条镜头提示词从此表直接引用，禁止用中文改写运镜方式。对象运动只有在 M-00 静止段内使用；不得把镜头运动和对象运动写成同一段的双主导动作。

| 代号 | 英文关键词（直接用于 Prompt） |
|------|--------------------------|
| M-00 | `static locked shot, no camera movement, camera strictly perpendicular to scene plane` |
| M-Z+ | `sudden snap zoom in along the central axis, mechanical zoom acceleration, planimetric framing maintained` |
| M-Z- | `sudden snap zoom out along the central axis, symmetrical architecture expanding from both sides, planimetric framing maintained` |
| M-XL | `lateral dolly strictly parallel to scene plane, camera moves only on the X-axis, no rotation, no push into depth` |
| M-YU | `vertical camera rise only on the Y-axis, camera remains perpendicular to the same scene plane, no tilt` |
| M-YD | `vertical camera descent only on the Y-axis, camera remains perpendicular to the same scene plane, no tilt` |
| P-X | `locked camera; subject moves only horizontally along the X-axis, no diagonal path` |
| P-Y | `locked camera; subject moves only vertically along the Y-axis, no diagonal path` |
| P-Z | `locked camera; subject moves straight along the central depth axis, no diagonal path` |

**完整 Prompt 写法示例（分镜表指定 18 秒；正面建立 → 横移 → 中轴离场）：**

```
[0–4s] static locked frontal planimetric shot; character A faces camera directly, holding a letter.
[4–10s] lateral dolly strictly parallel to scene plane, camera moves only on the X-axis, no rotation, no push into depth; it follows character A moving horizontally across the counter.
[10–15s] static locked shot; camera does not move; character A turns fully to a back-facing 180-degree orientation, then walks straight along the central depth axis into the centered doorway.
[15–18s] static locked frontal planimetric shot of the centered doorway and empty room, hold still for reading.
```

---

## 六、负向提示词（全局，每镜必加）

```
no CGI render, no 3D illustration, no digital smoothness, no plastic sheen, no airbrushed surfaces,
no lens distortion, no perspective warp, no handheld camera shake,
no shallow depth of field blur, no harsh naturalistic shadows, no casual snapshot aesthetics,
no motion blur, no subtitles, no watermark, no lens flare, no artificial vignette filter,
no over-saturated colors, no contemporary objects, no digital devices,
no mirror-image furniture arrangement, no identical duplicate objects on both sides of frame,
no off-axis oblique camera angle, no asymmetric perspective convergence, no off-center vanishing point,
no pan, no whip pan, no camera pivot, no camera tilt, no arc shot, no orbit, no 360-degree pan, no rotation,
no gradual rotation, no slow zoom, no slow push, no diagonal dolly, no oblique tracking,
no camera traveling around subject, no circular camera path, no diagonal or curved subject movement
```

---

## 七、布景提示词规范

每个场景提示词必须包含三层：

**材质层**：`aged wood paneling / cracked plaster walls / worn linoleum / brass fixtures / hand-stitched upholstery`

**内容层**：`shelves lined with leather-bound volumes / pinned maps / framed certificates / stacked ledgers`（布景在说这里发生过什么）

**年代层**：`1930s–1980s, no contemporary objects` + 具体物件：`rotary telephone / typewriter / leather luggage / brass key cabinet`

年代跳变时：年代层切换为材质退化描述（`same space but walls peeling, fixtures replaced with cheaper period alternatives, visible decay`），内容层同步替换为空置 / 残缺版本。色彩按辉煌 → 衰败偏移（暖饱和色 → 去饱和冷色）；画幅可配合变化（辉煌 → 11:8，衰败 → 12:5）。

---

## 八、角色提示词规范

角色图生成为电影静帧（非设计稿），全身（head to toe，脚部入画）。

**正面全身**

```
[场景类型], Wes Anderson style,
[身份色] [服装材质] [职业/身份],
full body shot head to toe, standing facing camera directly, feet visible,
[标志道具] as visual anchor,
physical costume with visible fabric texture and period-correct wear,
[光线],
35mm film still, analog film grain, physical set photography,
handcrafted costume with visible age and imperfection,
muted period-correct analog color, not over-saturated
```

侧面：`standing in strict 90-degree profile`
背面：`standing with back to camera`

禁止：`turnaround` `character sheet` `three views` `T-pose` `reference sheet` `flat design` `line art` `vector`

色彩：60% 主色 · 30% 辅色 · 10% 强调色，全片不变。

---

## 九、道具提示词规范

**俯拍特写**（纸张 / 手册 / 信件 / 小件）

```
overhead close-up planimetric, Wes Anderson style,
[道具描述] centered and filling frame,
[材质]: aged paper / worn leather / brass / cracked wood grain / cloth binding,
1930s–1960s object, no contemporary elements,
extremely flat overhead lighting, no depth of field blur,
35mm film still, analog film grain, handcrafted surface with visible age,
muted period-correct analog color, not over-saturated
```

**正面平拍特写**（书册 / 行李箱 / 立体道具）

```
frontal close-up static framing, Wes Anderson style,
[道具描述] centered,
[材质]: worn leather / aged cloth / brass fittings / painted wood / enamel,
1930s–1960s object, soft single-direction practical light,
35mm film still, analog film grain, handcrafted surface with visible age,
muted period-correct analog color, not over-saturated
```

禁止：`product shot` `studio render` `3D render` `CGI` `white background` `product photography` `clean background`

---

## 十、图片提示词结构

```
[场景类型], Wes Anderson style,
[GBH色彩总括行 或 自定义色彩规格], [材质层], [内容层], [年代层],
[构图关键词], [角色描述],
[光线], [情绪注脚],
35mm film still, analog film grain, physical set photography,
practical light sources with natural warmth and fall-off,
handcrafted surfaces with visible age and imperfection,
muted period-correct analog color, not over-saturated
```

**A 室内社交**

```
frontal static framing, Wes Anderson style, amber mustard warm interior,
[布景三层], centered symmetry, layered background depth visible,
[人物互动], warm practical lamp light with natural amber spill,
35mm film still, analog grain, physical set with aged paint and worn wood, muted analog palette
```

**B 外景街道**

```
planimetric exterior shot, Wes Anderson style,
steel teal [建筑立面] fills frame, [布景三层],
overcast diffused daylight, no perspective distortion,
signage integrated into composition, [点景人物或留空],
35mm film exterior still, physical building facade with weathering, analog grain
```

**C 夜间行动**

```
deep navy blue artificial color wash, Wes Anderson style,
extreme non-natural night lighting, [布景三层],
[人物], single amber point light source as sole warm accent,
figure dwarfed by dark architecture, mood of danger and urgency,
35mm film still, analog grain in shadows, physical night set
```

**C · GBH 夜间变体**（选项 A「布达佩斯大饭店」色彩方案时使用）：

```
GBH night variant: deep warm brown-black backdrop #2C1F14,
dusty rose-pink walls fade to near-black in shadow,
single butter-gold lamp source #F4DBB2 as sole warm point light,
tarnished brass frame lines #C8A030 catch the lamplight,
deep rose-red uniform #A34860 barely visible in shadow silhouette,
faded vintage night palette — inkwell dark with one warm anchor,
no cold blue digital night, no moonlight, no neon
```

**D 私人空间**

```
frontal static framing, Wes Anderson style,
sage olive green desaturated interior, [布景三层],
[人物，优先背对或侧对], books and papers as foreground prop layer,
soft single-direction window light, mood of solitary focus,
35mm film still, warm analog grain, tactile surfaces with natural age and wear
```

**E 制度权力**

```
planimetric composition, Wes Anderson style,
monumental cold grey [建筑类型] fills entire frame, [布景三层],
[渺小人物] at base or center,
flat institutional lighting without warmth,
35mm film still, aged concrete and worn stone surfaces, analog grain
```

**F 档案记忆**

```
black and white fully desaturated, Wes Anderson style,
Academy ratio frontal symmetry, [历史场景],
[布景三层], fine grain black and white film photography, archival print quality,
physical photographic grain, documentary register, sense of historical remove
```

**G 剖面建筑**（触发条件：空间是叙事主角 / 全片定场 / 章节开场；全片 ≤20%）

```
architectural cross-section revealing [N] floors/compartments, Wes Anderson style,
[建筑类型] cutaway view, each compartment occupied by different activity or figure,
[逐层布景: 顶/中/底],
structural cross-section reveals floor slab thickness, exposed conduits at junctions,
human figures at realistic scale occupying at least one-third of compartment height,
single dominant practical light source per compartment with natural falloff and cast shadows,
aged weathered materials with visible wear and staining,
compartments at varied heights and widths, organic architectural accumulation,
each compartment with visible depth receding into shadow,
35mm film still, analog grain, physical set construction, not a miniature model
```

---

## 十一、章节标题卡提示词

**静态版**

```
flat overhead close-up of [叙事世界底材],
[底材颜色质感], [字体风格] title text in [文字颜色],
[装饰元素: 徽章/印章/边框],
flat studio lighting, no depth of field,
signage as scene, Wes Anderson chapter title card
```

标题卡保持静态，不制作动态标题卡，以免与「标题卡为额外静帧」的生成和合成规则冲突。

---

## 十二、旁白语调

干冷、平整（deadpan），快速、信息密度高，像朗读文件而非表演。声音模型按成片语言选择；提示词只描述嗓音、节奏和逐字旁白内容，不混入画面或音效指令。

---

## 十三、音效规范

设计原则：默认轻度自然，陈述时刻大胆使用。

| 类型 | 典型声音 |
|-----|---------|
| 文字物质性 | 纸张翻动、打字机、蜡封压印 |
| 机械道具声 | 钥匙转动、拨号、行李箱锁扣 |
| 建筑 / 空间音 | 大厅回响、走廊脚步、沉重门扇 |
| 戏剧性陈述音 | 雷声（无雨）、钟声、警报 |
| 留白 / 静默 | 环境音切至静默 |

逐镜标注格式：`主要音效: [描述] · 质感: handmade/theatrical/naturalistic · 强度: background/foreground/statement`

---

## 十四、BGM 方向

**策略 A（全片情绪基本一致）**：单段 BGM，全片循环或拉伸。

**策略 B（有明显情感弧度）**：触发条件：不同章节情绪明显转折 / 全片 ≥2 分钟有章节结构 / 启用了色彩跳变。

多段 BGM 规则：

1. 弧度分析：从分镜情绪标注提取情感节点
2. 确定乐器骨架（全片共享）：钢琴 + 弦乐 / 手风琴 + 木吉他 / 木管小乐队 / 拨弦乐 / 弦乐四重奏
3. 逐段生成：`[骨架乐器], [情绪], [速度], [时长]s, instrumental, no vocals, [opening/continuation/closing]`
4. 段落时长 = 叙事时长 + 15s 缓冲

情绪方向参考（作为出发点，不要作为固定库套用）：辉煌仪式 → 管弦乐小品 / 温暖人际 → 法语香颂、轻爵士 / 孤独失落 → 钢琴独奏 / 危险夜行 → 急促弦乐 / 智识私密 → 弦乐四重奏 / 档案记忆 → 旧唱片感 / 荒诞幽默 → 班卓琴、木管
