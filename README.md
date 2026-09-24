# Lumina Wes Anderson Short Film Skill

面向 **Lumina Canvas Agent** 的韦斯·安德森风格短剧生产 Skill。它把故事 idea、完整剧本或视觉参考转化为一套可核验的画布生产图：全局规格、分镜蓝图、角色/场景/道具资产、起始关键帧、逐镜视频与声音节点，以及最终剪辑决策表。

本项目基于 [SeastZhu/wes-anderson-short-film](https://github.com/SeastZhu/wes-anderson-short-film) 的完整创作方法论进行 Lumina 画布原生化改造，并保留其全部核心内容：

- 十字轴线运镜与严格正交摄影
- planimetric 平面构图与“结构对称、内容不镜像”
- 角色正面 `0°` / 严格侧面 `90°` / 背面 `180°` 三态锁定
- 《布达佩斯大饭店》十色色板与章节色彩弧线
- 默认每镜 30 秒的完整镜头单元
- 剧本分析、故事拓展、逐镜提示词、声音设计与剪辑合成

## Lumina 画布增强

- 开始前检查现有画布，复用用户素材与已完成节点。
- 用持久化 String/text 节点保存 `SPEC-vN`、`STORY-vN` 与 `EDIT-vN`。
- 图片既要真实连接到生成节点，也要通过 `@` 选择器插入已解析引用标签。
- 每个 Prompt 写入对应的 Image/Video/Audio Generation 节点内部，不为逐镜 Prompt 创建孤立文本节点。
- 先确认规格、分镜和资产，再逐镜生成；首镜通过后才批量生成后续镜头。
- 使用真实节点 ID 或精确节点名维护资产绑定表，并在每次结构变更后复核。
- 如果当前画布没有合成或导出能力，交付有序镜头、音频和剪辑决策表，不虚报成片已导出。

## 目录

```text
wes-anderson-short-film/
├── SKILL.md
├── README.md
├── LICENSE
├── references/
│   ├── 01-workflow.md
│   ├── 02-source-analysis.md
│   ├── 03-storyboard-design.md
│   ├── 04-camera-and-movement.md
│   ├── 05-prompt-writing.md
│   ├── 06-gbh-palette.md
│   ├── 07-media-assets.md
│   └── 08-editing-compositing.md
└── assets/
    ├── final-video-spec.md
    ├── shot-plan.md
    └── canvas-production-manifest.md
```

## 使用方式

把本目录作为 Skill 提供给 Lumina Canvas Agent，然后给出故事、剧本或参考素材。Skill 会依次完成：

1. 检查画布并分析素材
2. 确认片名、时长、语言、旁白、画幅和清晰度
3. 确认 GBH / 自主 / 自定义色彩方案
4. 创建并确认 `SPEC-vN`
5. 创建并确认 `STORY-vN` 与逐镜 30 秒计划
6. 创建和绑定角色、场景、道具、起始关键帧
7. 首镜试制与确认，随后生成剩余镜头
8. 生成并绑定旁白、音效、环境声和 BGM
9. 合成成片，或输出 `EDIT-vN` 剪辑决策表

典型请求：

```text
请在当前 Lumina 画布里使用这个 Skill，把下面的故事制作成一部 2 分钟、4:3、1080p、中文旁白的韦斯·安德森风格短剧。使用 GBH 色板，先给我确认全局规格和分镜，再生成资产与首镜。
```

## 模型与能力

Skill 不把单一模型视为平台前提。存在时优先使用 Lumina 内置的参考图像生成、`Seedance 2.5` 多模态视频、语音、音效和器乐生成能力；若当前画布提供等效能力，应先把实际选择写入并确认在 `SPEC-vN` 中。

## License

MIT。原始方法论仓库同样以 MIT License 发布。
