# Workflow｜Visual Anchor + Asset First

## 1｜Visual Anchor

中高视觉复杂度镜头默认先锁：

- 构图；
- 主体比例；
- Camera framing；
- 空间层级；
- 材质语言；
- 灯光方向；
- 背景；
- 信息密度。

已有参考图 / Blockout / 已批准关键帧 → 直接作为视觉真值。

**图不满意，不进入昂贵 BUILD。**

如果任务属于平面设计、3D/2.5D、软件/UI 等“电脑生成设计视觉”，GPT 生图明显偏写实时，优先建议使用 Banana 做 Visual Anchor，再进入 Blender。

## 2｜Asset First

默认优先搜索成熟资产：
- 产品 / 官方模型；
- 通用 3D Model；
- HDRI；
- Texture；
- Material；
- Logo / SVG；
- UI / Icon；
- 扫描资产；
- 已有模板 / Asset Browser 内容。

顺序：
1. 当前工程 / 本地资产库；
2. 用户已有资产；
3. 官方资源；
4. 授权清晰的开源 / 专业库；
5. AI / Blender 自制；
6. Placeholder。

抽象几何、独特造型、没有成熟资产的部分再原创。

## 3｜正式 BUILD 前

最少确认：
- Visual Anchor 已明确；
- Hero Asset 的来源 / Placeholder 已明确；
- Camera / framing 可执行；
- 需要真实产品准确性时不拿 AI 随机模型冒充；
- 不因为“代码更容易生成”就忽略成熟资产。

## 4｜AI 与人工边界

视觉锁定后：
- AI 可以搭场景、材质、灯光、构图、Rig；
- 动画默认先做简单主 Motion；
- 复杂 Graph / Camera Path 可以人工精修。

**简单但正确 > 复杂但失控。**
