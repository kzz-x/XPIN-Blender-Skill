# XPIN Blender Skill Routing

本仓库提供两套 Blender Skill。目标：**能力不缩水，但默认上下文尽可能小。**

## 1｜先选 Skill

默认使用 `chadian-blender-mini`：
- 选中对象 / 单对象 / 少量对象；
- 改 Transform、材质、颜色、文字、灯光；
- 简单 M0–M1 动画；
- 普通局部 Patch；
- 已有结构下的小修。

直接使用 `chadian-blender`：
- 从零完整场景 / 镜头；
- 中高视觉复杂度、Visual Anchor、Reference / Asset Search；
- 多对象 Motion、Camera、Rig、Constraints、Drivers、Geometry Nodes；
- 明确要求后续人工易改、少关键帧、模板化；
- 新建 N Panel / Custom Properties / “差点控件”；
- 复杂 Python、结构重构、Asset Browser / Library 体系；
- 完整工程审计或深度 Debug。

**明显命中 Full 时不要先读 Mini。**

## 2｜直接操作 Blender 前

只咨询、出方案、解释时不要求连接 Blender。

要直接读取 / 修改 / 制作 Blender：
- 使用当前可用的 Blender MCP / Python / Agent 工具读取真实状态；
- 未连接时不得假装已修改；
- 修改现有 .blend 前先确认 Recovery Point；
- 高风险操作优先复制对象 / Collection / 文件，或保留可回退状态。

## 3｜永远生效的底线

- **Read Before Write**：只读当前任务需要的真实状态。
- **Patch First**：能局部改，不重建。
- **Preserve Manual Work**：保护人工 F-Curve、Action、Driver、Constraint、Parent、Modifier、Node、材质、灯光与命名。
- 用户说“当前 / 这个 / 选中的” → 实时读取 selection，不猜。
- **Editable First**：成片之外必须留下正常可编辑的 Blender 工程。
- **No Dense Keys**：除 Tracking / Simulation / Mocap / Bake 等合理场景外，禁止逐帧或高密度写 Transform Key。
- **Relationships should be encoded**：能用 Parent / Constraint / Driver / Target / Path 表达的关系，不手工同步多套 key。
- **Animation ≠ Adjustment**：动画和人工调整分离。
- **Native Control First**：Custom Properties / Drivers / Constraints / Nodes 优先；Python 不应成为日常微调的唯一入口。
- 默认不完整高质量渲染；先用代表帧 / viewport preview 验收。
- 写后必须回读关键状态；脚本“执行成功” ≠ 工程正确。

## 4｜Visual / Asset

中高视觉复杂度默认先锁 Visual Anchor。已有准确参考图 / Blockout / 已批准关键帧就直接复用。

模型、HDRI、Texture、Material、Logo、产品资产等现实中高度可能已有成熟资产的内容，优先 Asset First。

## 5｜AI / Human 分工

默认：
- AI：Scene / Model Assembly / Material / Lighting / Composition / Rig / Controls / 参数化 / 简单持续动画；
- Human：复杂 Camera Path、复杂多段主动画、最终 Graph、节奏精修。

除非叙事确实需要，不为了“显得高级”堆复杂运镜。

## 6｜Context Budget｜渐进加载硬规则

**不要预读整个 references。**

- Mini：除 Mini `SKILL.md` 外默认读取 0 个 reference。
- Full：先只读 Full `SKILL.md`；执行前默认新增 0–2 个直接命中的 reference / recipe。
- 足够执行就停止读取。
- 同一会话已读过的文件不重复读取，除非内容变化。
- 故障文档只在真实故障出现后加载。
- 模板 / 控件任务才读对应模块。

默认路径：

`AGENTS → Mini → Tool`

复杂任务：

`AGENTS → Full → 命中的 1–2 个模块 → Tool`
