# Motion｜Blender Motion & Relationship Rigging

核心原则：

**Shared motion goes upward. Unique motion stays local.**

**Relationships should be encoded, not manually synchronized.**

## 1｜Relationship Scan

动画前判断：
- Parent / Child
- Follow
- Attach / Carry
- Look At / Aim
- Focus
- Path
- Target / Destination
- Shared Motion
- Repeated Layout
- Connector
- Camera Relationship

存在真实关系 → 优先编码。

## 2｜Follow / Attach

### Follow
优先：
- Parent
- Copy Location / Rotation
- Child Of
- Driver

并留 Offset。

### Attach / Carry
适合机械臂、产品跟随、零件装配。

```text
CARRIER
└─ ATTACH_POINT

PAYLOAD
```

不要给 CARRIER 和 PAYLOAD 手工复制完整路径。

## 3｜Destination Driven

对象从 A 去 B：
- B 是 Target；
- 动画引用 B 的实时位置 / 关系；
- 不默认把 B 当前坐标烘成永久终点。

用户之后移动 B，动画仍能适配。

## 4｜Path Rig

优先：
- Curve Path；
- Follow Path；
- Offset Factor / Evaluation Time；
- 控制点决定 Line of Action。

用户应该能改 Curve，而不是重画几十个 Position Key。

## 5｜Look At / Focus

Camera / Arrow / Mechanical Part：
- Track To / Damped Track；
- Target Object；
- Focus Object。

Position、Aim、Focus 解耦。

## 6｜Shared Progress

多对象同一节奏：
- Master Progress / Master Time；
- 每对象使用 Stagger / Delay / Mapping；
- 局部特殊性保留在各自 F-Curve / Driver。

不要复制完整 Key 再逐个平移时间。

## 7｜Graph Quality

语言转换示例：

“快速往前冲，然后减速，再缓慢继续前进”

不要逐帧 key。

优先：
- 2–4 个关键 Pose；
- Bezier F-Curve；
- 首段高速度；
- 中段明显 deceleration；
- 后段低速持续；
- 必要时拆成主位移 + 微量持续 Drift。

Graph Editor 是动画核心工具，不是最终补救工具。

## 8｜Secondary Motion

小幅随机 / 漂浮 / 抖动：
- F-Curve Modifier Noise；
- Driver；
- 少量局部 Key。

不要把 Noise Bake 成密集 Key，除非交付明确要求 Bake。

## 9｜Camera

默认优先简单：
- 固定镜头；
- 单一轻推；
- 单一 Truck；
- 单一轻绕；
- 模型慢旋；
- 局部灯光变化。

复杂 Camera choreography 只在叙事需要时做。

AI 默认先交：
- Rig；
- Target；
- Focus；
- Blockout；
- 简单主曲线。

最终复杂 Graph 可以人工接管。

## 10｜通过标准

- 改 Target，关系继续成立；
- 改 Rig，整组一起动；
- 改 Child，局部构图不影响主动画；
- 调 Master Timing，多对象不需要逐个挪 Key；
- Graph Editor 内没有无意义密集曲线。
