# GBH 色板（布达佩斯大饭店标准配色 · 优化版）

> 选项 A「模仿布达佩斯大饭店」时使用此完整色板。选项 B 由模型自主设计时可参考或超越此色板。

## 十色核心体系

| 角色 | 颜色名 | Hex（褪色档） | 视频用途 | Prompt 关键词 |
|------|--------|------------|---------|------------|
| **主底/粉墙** | 浅玫粉 | `#E3A6AC` | 墙面 / 外立面主色；空间气质锚点 | `dusty rose powder-pink plaster walls, time-faded candy-pink #E3A6AC, low-saturation warm vintage` |
| **容器/柜面** | 灰薄荷绿 | `#9FB295` | 家具 / 柜橱 / 门扇漆色 | `sage mint-green lacquered cabinetry, muted celadon grey-green #9FB295, old painted wood` |
| **深描边/线脚** | 橄榄绿 | `#637040` | 门框 / 线脚 / 深色描边 | `olive-green architectural trim and frame lines, weathered painted woodwork #637040` |
| **暖中性/光晕** | 奶黄 | `#F4DBB2` | 走廊灯光 / 暖色面 / 高亮区 | `warm butter-yellow incandescent light wash, ivory-gold ambient glow #F4DBB2` |
| **中性缓冲** | 奶油白 | `#F0E8D8` | 台布 / 床品 / 纸品 / 缓冲间色 | `aged cream linen, yellowed-white cotton and paper texture #F0E8D8` |
| **标牌/制服** | 深玫红 | `#A34860` | 制服主色 / 标牌 / 匾额 / 文字 | `deep rose-burgundy staff uniform, bordeaux-rose signage lettering #A34860` |
| **点缀金** | 建筑金 | `#C8A030` | 镀金框线 / 门把 / 胸针 / 装饰 | `tarnished brass-gold frame lines, gilded architectural accents #C8A030, antique gold hardware` |
| **软装藕紫** | 雾霭藕紫 | `#C9A6B4` | 内饰布艺 / 床帷 / 帘幔 | `dusty mauve upholstery, faded rose-grey drapery #C9A6B4, aged velvet textile` |
| **阴影/暗部** | 深棕墨 | `#2C1F14` | 阴影区 / 暗角 / 夜间底色 | `deep warm brown-black shadow, inkwell dark corners #2C1F14` |
| **正文/木质** | 暖褐 | `#6E5140` | 木质表面 / 门板 / 次级描边 | `worn leather-brown woodgrain paneling, warm umber edge #6E5140` |

## GBH 视频 Prompt 色彩总括行（每镜必用，可直接粘贴）

```
dusty rose-pink plaster walls #E3A6AC / sage mint-green cabinetry #9FB295 /
olive-green trim #637040 / warm butter-light #F4DBB2 / aged cream accents #F0E8D8 /
deep rose-red uniform and signage #A34860 / tarnished brass-gold frame lines #C8A030 /
dusty mauve upholstery #C9A6B4 / all colors faded vintage film palette,
low-saturation warm analog, muted period-correct tones, no digital oversaturation
```

## GBH 空间三层色彩分配

```
前景层（人物/道具）：制服深玫红 + 标志道具奶黄/奶油白
中景层（家具/墙面）：粉墙主底 + 薄荷绿柜橱 + 橄榄绿线脚
背景层（封底/天花）：深棕墨阴影 + 奶黄暖光点 + 建筑金边框
```

## 色彩插槽系统

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

## 服装与布景的色彩对话

三种策略：

- **反差对比（制造焦点）**：服装色与背景明显区分。深色制服 + 暖黄环境 → 制度感异质性
- **同色系融入（制造归属感）**：服装接近布景色调。紫色制服 + 粉色酒店 → 角色与场所合并
- **点睛色**：绝大多数中性色，唯有关键道具或局部一个强调色

**硬约束（无例外）**：同帧内服装色不得与背景主色完全相同。最低限度：饱和度差一级或明度有明显区分。

## 章节色彩弧线设计

五种弧线类型：

| 弧线类型 | 颜色走向 | 叙事功能 | 适用题材 |
|---------|---------|---------|---------|
| **降温弧** | 暖饱和 → 中温 → 冷去饱和 | 繁荣 → 衰退 → 消亡 | 衰落叙事 |
| **升温弧** | 冷去饱和 → 中温 → 暖饱和 | 压迫 → 挣扎 → 解放 | 压抑 → 解放结尾 |
| **对比弧** | 章 A 暖 + 章 B 冷 → 消解（灰 / 黑）| 两种对立视角无法整合 | 多视角 / 矛盾现实 |
| **渐褪弧** | 同灵魂色，饱和度章章递减 | 记忆正在消失 / 存在感消退 | 回忆 / 失去题材 |
| **积重弧** | 同灵魂色，明度章章降低 | 压力 / 重量在累积 | 宿命 / 压抑题材 |
