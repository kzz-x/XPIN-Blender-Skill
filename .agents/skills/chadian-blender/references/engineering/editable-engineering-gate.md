# Engineering｜Editable Engineering Gate

> 完整动画 / 多对象 Motion 的执行门禁。目标不是零关键帧，而是：**少重复 Key、关系清晰、曲线可调、人工接手方便。**

## 1｜触发条件

命中任一项就应用：
- 从零制作完整动画镜头；
- 3+ 对象共享 Position / Rotation / Scale / Timing；
- Camera + 多对象协同运动；
- 用户要求“以后我自己方便改 / 做成模板 / 可编辑工程”；
- 当前工程已出现密集 Key、重复曲线、没有 Parent / Rig；
- Agent 正准备用循环逐帧写 Transform。

## 2｜Pre-animation Gate

首个 Primary Motion Key 前先声明：

```text
Global Motion       → RIG_SCENE / CTRL_MASTER
Group Motion        → Group Empty
Camera Motion       → RIG_CAMERA
Relationship        → Constraint / Driver / Target / Path
Unique Motion       → Local Object / Bone
Manual Adjustment   → Child Local Transform / Delta Transform / Exposed Control
```

**Shared motion goes upward. Unique motion stays local.**

Collection 只负责组织时，不把它当作不存在的“可变换 Precomp”；共享 Transform 使用 Empty / Rig Object。

## 3｜Keyframe Budget

普通 A→B：
- 2 个主 Key 起步；
- anticipation / overshoot / settle → 通常 3–4 个；
- 使用 Bezier Handles / Graph Editor 调速度；
- 需要循环、噪声、包络时优先考虑 F-Curve Modifier；
- 重复动作片段才考虑 Action / NLA。

除合理的 Tracking / Simulation / Mocap / Bake / 数据采样外，禁止：
- frame loop 每帧 `keyframe_insert()`；
- 用几十个 Transform Key 模拟 easing；
- 给多个对象复制同一套曲线；
- Parent 和 Child 同时承担同一整体运动。

## 4｜Animation / Adjustment 分离

### 首选 A｜Animated Parent + Editable Child

```text
RIG_TITLE      ← 主动画
└─ GEO_TITLE   ← 用户直接改局部位置 / 旋转 / 缩放
```

最接近 AE 的“动画载体 + 子层手调”。

### 首选 B｜Delta Transform

当对象本身已有主 Transform 动画，但仍需要人工位置/旋转/缩放偏移：
- 优先考虑 Delta Location / Rotation / Scale；
- 不移动原始 Key；
- 不批量重算整套曲线。

### 首选 C｜Controller / Driver

共享数值、比例、强度、进度、Target 关系：
- 用 Custom Property + Driver；
- 不把参数烘焙成多套重复 Key。

### 谨慎使用 NLA

NLA 适合真正需要 Action 复用、混合、分层的动作。
不要为了“高级”把普通 Motion 全塞进 NLA，导致用户找不到主 Graph。

## 5｜Relationship Encoding

存在真实关系时优先编码：
- Follow → Copy Location / Child Of / Driver
- Look At → Track To / Damped Track
- Attach / Carry → Parent / Child Of
- Path → Follow Path / Curve + Offset Factor
- Focus → Focus Target / Driver
- Repeated Layout → Geometry Nodes / Driver
- Destination → Target Object，而不是烘焙目标当前坐标

原则：

**Relationships should be encoded, not manually synchronized.**

## 6｜Editable QA

Primary Motion 后一次，交付前再一次。

至少检查：
1. 所有动画对象的 Parent / Constraint / Rig；
2. 每个动画属性 Key 数量；
3. 3+ 对象重复或近似 F-Curve；
4. Driver / Custom Property / Master Control；
5. 每个主要对象的人工 Override 路径；
6. Camera 是否把 Position / Aim / Focus 全揉成难改的一套曲线；
7. 是否存在 Python 每帧 Handler 其实可被 Driver / Constraint 替代。

### QA FAIL

出现任一项先修工程：
- 普通动作高密度 Key；
- 3+ 对象复制同一运动；
- 没有共享 Rig；
- 改一个对象需要重做原动画；
- 主要控制入口难找；
- Agent 脚本一停，场景就失去核心关系；
- Parent 与 Child 双重执行同一整体 Motion。

修复优先级：

`Parent / Constraint → Driver / Custom Property → Geometry Nodes → F-Curve/NLA → Local Keyframes → Bake`

## 7｜通过标准

用户应该能：
- 移动 Child / Delta → 构图变了，原动画还在；
- 调一个 Master Property → 多对象同步改变；
- 移动 Target → Follow / Aim / Path 继续成立；
- 打开 Graph Editor → 看到少量可理解主曲线；
- 不重新跑 Agent，也能完成常见微调。
