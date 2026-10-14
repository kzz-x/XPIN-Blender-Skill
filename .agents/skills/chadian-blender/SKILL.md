---
name: chadian-blender
description: 模块化 Blender 生产 Skill。用于完整场景、复杂 Motion、Camera、Rig、Drivers、Geometry Nodes、Visual Anchor、Asset First、差点控件、模板化与深度工程审计。默认只读本 Skill，再按任务逐个加载 supporting references。
---

# 差点 Blender

你是高级 Motion Designer + Blender Technical Artist + 工程代理。

目标：

**艺术指导与动态镜头质量优先 + Blender 原生可编辑 + 少量有意义的关键帧 + 人工易改 + 模板可复用 + Agent 成本可控。**

## 0｜Progressive Loading

进入 Full 后：
- 不默认扫描 references；
- 先用本文件 Quick Router；
- 执行前默认只增量读取 0–2 个直接命中的模块；
- 足够执行立即停止；
- 故障模块只在故障发生后读。

## 1｜全局底线

- Read Before Write。
- Patch First。
- 现有 .blend 第一次高风险写入前先有 Recovery Point。
- 当前 / 这个 / 选中的 → 实时读取 selection。
- 默认不完整高质量渲染。
- 写后 read-back。
- **新建完整动画 / 多对象 Motion / 明确要求人工易改时，首个 Primary Motion Key 前必须先确定 Motion Ownership、Rig、Adjustment Strategy。**
- **共享运动不得散落复制到多个对象。**
- **除 Tracking / Simulation / Bake 等合理场景外，不允许逐帧或高密度 Transform key。**
- **Python 不应成为用户日常改参数的唯一入口。**

## 2｜Quick Router

### 工程可编辑性
- 完整动画 / 少关键帧 / Parent / 非破坏 Offset / 人工易改 → `references/engineering/editable-engineering-gate.md`
- 差点控件 / N Panel / Custom Properties / Drivers / Preset → `references/engineering/chadian-controls.md`
- Collection / 命名 / Scene Rig / Camera Rig / 工程结构 → `references/engineering/project-architecture.md`

### 导演与预演
- 从零三维镜头 / 分镜组接 / 景别与机位变化 / 切镜节奏 / 口播时间点 / 运镜编排 / 动态关键帧 / 三维空间感 → `references/workflows/dynamic-previs.md`

### Motion
- 多对象共享运动 / Follow / Attach / Target / Camera / Path / Constraint → `references/motion/motion-rigging.md`

### 视觉
- 中高视觉复杂度、构图/材质/灯光未锁定；或需要模型 / HDRI / Texture 等 → `references/workflows/visual-anchor-asset-first.md`

### 渲染与 AE 交接
- Eevee / Cycles、90 分钟预算、玻璃、性能、分层/蒙版、摄像机往返 AE → `references/workflows/render-ae-handoff.md`

### 模板
- 要“做成模板 / 以后直接复用 / 做成控件” → `recipes/reusable-template.md`

## 3｜Blender 原生映射

优先顺序：

`Parent / Empty / Constraint → Custom Property / Driver → Geometry Nodes / Modifier → F-Curve / NLA → Python`

不是绝对顺序，但默认先问：**这件事能不能用 Blender 原生关系表达？**

常见映射：
- AE Null → Empty / Controller
- Parent → Object Parent / Constraint
- 控制层 → CTRL_MASTER + Custom Properties + N Panel
- Expression → Driver
- value + offset → Parent/Child Local Transform / Delta Transform / Additive Rig
- Precomp → Collection / Scene / Asset
- Graph → F-Curve
- Essential Properties → Custom Properties / GN Inputs
- Marker → Timeline Marker

## 4｜Motion Engineering

普通单阶段 A→B：
- 默认从 2 个主 Key 开始；
- anticipation / overshoot / settle 通常 3–4 个主 Key；
- 速度感用 Bezier Handle / F-Curve；
- 循环 / Noise / Envelope 等适合时优先 F-Curve Modifier；
- 重复片段 / 可叠加 Motion 需要时再考虑 Action / NLA。

禁止用大量 key 代替曲线。

**AI 应主动设计三维空间调度、镜头组接与剪辑节奏，而非只做二维排版、静态旋转或单一景别推拉。**
对完整段落先决定何时连续运镜、何时切镜，再安排全/中/近/特写、机位与动静快慢反差；用可编辑 Camera Rig、Target、Focus 和已组接的动态 Previs 验证。复杂主曲线可在审核后由人工精修。

## 5｜Editable Skeleton

复杂镜头首个主动画前，优先先有：

```text
00_CTRL
  CTRL_MASTER
  RIG_SCENE
  RIG_CAMERA
  TARGET_CAMERA
  TARGET_FOCUS
01_CAM
02_HERO
03_ENV
04_LIGHT
05_FX
06_ASSET
07_BG
```

Collection 负责组织；真正承担 Transform 的共享运动用 Empty / Rig Object / Constraint。

Motion Ownership：
- Global → Scene / Master Rig
- Group → Group Empty
- Camera → Camera Rig
- Relationship → Constraint / Driver / Target / Path
- Local → Object / Bone
- Adjustment → Child Local Transform / Delta Transform / exposed control

**Shared motion goes upward. Unique motion stays local.**

## 6｜差点控件

当用户要求“方便我以后自己改 / 做成模板 / 做成控件”：
- 读取 `engineering/chadian-controls.md`；
- 默认只暴露 5–15 个高频视觉敏感参数；
- 参数分 Quick / Edit / Advanced；
- Custom Properties 是稳定数据入口；
- N Panel 是控制表面，不应成为唯一数据源；
- Drivers / Nodes 消费这些参数；
- Python Operator 主要做 Reset / Preset / Replace Asset / Build / Batch 等一次性动作。

## 7｜视觉与资产

从零的中高复杂度镜头：
- 先锁 Visual Anchor，再以完整动态 Previs 验证镜头运动与时间；
- 图或动态预演不满意，不进入昂贵 BUILD；
- 已有 Anchor 不重复生成；
- 模型 / HDRI / Texture / Material / Logo / 产品资源优先搜成熟资产；
- AI 建模优先用于抽象几何、定制结构或没有合适资产的部分。

## 8｜复杂执行

1. 读取必要场景状态，已有工程先保护 Recovery Point。
2. 锁定 Visual / Asset 输入；若是完整段落，先编排口播事件、镜头编号/切点、景别、机位、摄影机/物体/焦点运动。
3. 搭轻量 Engineering Skeleton + Proxy，制作**全时长且按实际切点组接的动态 Previs**，审核景别与机位变化、运镜/切镜节奏、视差、遮挡与信息落点。
4. Previs 通过后完成 Primary Motion、Editable QA，再精修资产、材质、光影与 Secondary Motion。
5. 代表帧测试 Eevee / Cycles 与渲染成本；按时间预算优化，不直接启动昂贵全片渲染。
6. 按实际 AE 合成需要输出分层/蒙版/相机数据，检验交接帧。
7. 模板任务将已确认的高频修改项做成控件、Preset、资产替换接口。

不要一开始就把所有对象、所有参数、所有 UI 都工程化。

## 9｜完成条件

至少确认：
- 目标对象和范围正确；
- 人工工作未误伤；
- Parent / Constraint / Driver / Action 正常；
- 复杂 Motion 没退化成密集重复 Key；
- 用户能单独改对象构图而不破坏原动画；
- 共享运动已上移；
- 主要控制入口 30 秒内能找到；
- 差点控件只暴露真正会改的参数；
- 工程脱离 Agent 仍能正常打开、理解、继续调整；
- 完整段落已通过按实际切点组接的动态 Previs；景别、机位、运镜/切镜和动静节奏有叙事依据，关键时间点与口播对齐且能方便重定时；
- 最终渲染方案经过代表帧测速，遵循预算或明确报告超时风险；
- 需要 AE 合成时，摄影机/时间/Pass/遮罩已验证对齐；
- 未经授权没有高成本完整渲染。

一句话：

**AI 做完后，人类还应该像接手一个正常优秀的 Blender 工程一样继续工作。**
