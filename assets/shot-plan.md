# Shot 计划模板（单镜 30 秒）

> 阶段 5 为每个 `shot` 填写一份；作为关键帧与视频提示词生成的唯一依据。

```
## Shot k
场景类型：[A–G / T-10]
章节：[章节名]
目标时长：30s（除非用户明确要求短镜头）

### 布景描述
材质层：[材质词]
内容层：[布景在说这里发生过什么]
年代层：[1930s–1980s 物件清单]

### 故事内容
台词 / 旁白：[逐字文本]

### 角色朝向锁定（逐角色、逐表演段）
[角色名]：[正面0° / 严格侧对90°(左/右) / 背面180°]

### 平面构图锁定类型（五选一）
[标准平面 / 轴线纵深 / 轴对称特写 / 严格俯拍 / 严格正拍]

### 对称声明
结构层：[对称]
内容层：[不镜像，视觉重量平衡说明]

### 运镜时间轴（完整覆盖 0–30s）
[0–Xs]:   代号 · 英文关键词 · 叙事事件（中文）
[Xs–Ys]:  代号 · 英文关键词 · 叙事事件
[Ys–30s]: 代号 · 英文关键词 · 叙事事件

### start_frame（0 秒静帧）
构图 / 人物朝向 / 场景中轴 / 道具起始位置

### 色彩插槽
灵魂色 / 身份色 / 场景插槽 / 暗部色

### 声音标注
主要音效：[描述] · 质感：[handmade/theatrical/naturalistic] · 强度：[background/foreground/statement]
BGM：[段落情绪与骨架乐器，如有]

### 文字道具（如触发）
名称 / 语言 / 字体风格 / 载体 / 出现时间段

### Lumina 节点绑定
角色元素：[CHAR-XX 真实节点名/ID，或 planned]
场景元素：[SCENE-XX 真实节点名/ID，或 planned]
道具元素：[PROP-XX 真实节点名/ID，或 planned]
起始帧：[FRAME-XX-S0 真实节点名/ID，或 planned]
验证帧：[FRAME-XX-KN 真实节点名/ID，或 none]
视频节点：[SHOT-XX 真实节点名/ID，或 planned]
旁白节点：[AUD-NAR-XX 真实节点名/ID，或 off]
音效节点：[AUD-SFX-XX 真实节点名/ID，或 off]
BGM 节点：[BGM-XX 真实节点名/ID，或 off]
连接状态：[planned / connected / verified]
@ 引用状态：[planned / resolved / failed]
生成状态：[planned / queued / generated / approved / failed]
```

## 示例（正面建立 → 横移 → 中轴离场）

```
### 运镜时间轴
[0–6s]:   M-00 · static locked shot, camera strictly perpendicular to scene plane · 角色A正面站定，翻阅信件。
[6–14s]:  M-XL · lateral dolly strictly parallel to scene plane, camera moves only on the X-axis · 摄影机横向跟随角色A穿过柜台。
[14–22s]: M-00 · static locked shot, camera strictly perpendicular to scene plane · 镜头锁定；角色A背对镜头，沿中央Z轴走入正中的门洞。
[22–30s]: M-00 · static locked shot, camera strictly perpendicular to scene plane · 门洞与空场景正面静止，叙事收束并作为下一镜切点。
```
