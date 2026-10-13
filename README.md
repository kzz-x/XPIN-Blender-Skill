# XPIN Blender Skill｜差点 Blender

给 Codex / Agent 使用的 Blender 生产 Skill。

目标不是“让 AI 把画面做出来就算完成”，而是：

**高质量画面 + 原生可编辑工程 + 少量有意义的关键帧 + 清晰 Rig + 人工可继续调 + 可模板复用。**

## 核心思想

- **Editable First**：AI 生成的场景必须能被人正常继续修改。
- **Shared motion goes upward**：共享运动上移到 Empty / Rig / Driver / Master；独特运动留在局部。
- **少关键帧，重 F-Curve**：普通 A→B 从 2 个主关键帧开始，用 Bezier/F-Curve 做速度，不用逐帧 key 模拟 easing。
- **Animation ≠ Adjustment**：动画和人工构图调整分离；优先 Animated Parent + Child Local Transform，必要时使用 Delta Transform。
- **Native Control First**：Custom Properties / Drivers / Constraints / Geometry Nodes 优先，Python 主要负责 UI、批处理和一次性 Operator。
- **差点控件**：高频修改项统一暴露到 N Panel / Custom Properties，默认只给 5–15 个真正有价值的控件。
- **Art + Motion Direction First**：静态关键帧与动态预演缺一不可；AI 主动设计三维视差、运镜、主体动作与口播节奏，先验证后精修。
- **Visual Anchor First**：中高视觉复杂度镜头先锁参考图/关键帧，再进入昂贵 BUILD。
- **Asset First**：模型、HDRI、材质、Logo、产品资产优先搜索成熟资产，再原创。
- **Render Budget**：按用户时间预算（默认目标 90 分钟）实测代表帧，选择 Eevee/Cycles 与优化策略；玻璃质感和动画稳定性优先于盲目堆采样。
- **AE Handoff**：按需要交付相机/时间信息、分层 EXR、Alpha/遮罩/深度等，验证 AE 对齐，不承诺无损互通。
- **Human Handoff**：AI 先交可编辑的 Camera/Subject 动态预演；复杂主 Graph 和最终节奏由用户确认或人工精修。

## Skill

- `.agents/skills/chadian-blender-mini/`：简单 Patch、选中对象、小范围修改。
- `.agents/skills/chadian-blender/`：完整场景、Motion、Camera、Rig、Geometry Nodes、模板化、差点控件、复杂工程。

先读 `AGENTS.md`，再按任务渐进加载。不要默认扫描整个 references。

完整镜头重点按需读取：`references/workflows/dynamic-previs.md`（动态导演/时间）和 `references/workflows/render-ae-handoff.md`（预算/材质/分层/AE）。

## Blender 与 AE 的对应关系

| AE 习惯 | Blender 对应 |
|---|---|
| Null | Empty / Controller Object |
| Parent | Object Parent / Constraint |
| 控制层 | CTRL_MASTER + Custom Properties + N Panel |
| Expression | Driver |
| Precomp | Collection / Scene / Linked Asset |
| Graph Editor | Graph Editor / F-Curve |
| value + offset | Parent/Child Local Transform / Delta Transform / additive rig |
| Essential Properties | Custom Properties / Geometry Nodes inputs / exposed rig controls |
| Marker | Timeline Marker |
| 模板工程 | .blend Asset / Scene Rig / Asset Browser / preset |

## 交付标准

一个合格的 Blender 工程应该让用户能快速做到：

- 改主体位置而不破坏原动画；
- 调整体节奏而不逐个移动几十组 key；
- 移动 Target 后 Follow / Look At / Path 关系继续成立；
- 在 Graph Editor 看到少量真正有意义的主曲线；
- 30 秒内找到主要控制入口；
- 不依赖 Agent 重新跑代码，也能完成绝大多数日常微调；
- 将满意结果保存为 preset / template，下一次直接复用。

