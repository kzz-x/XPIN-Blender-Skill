# Engineering｜差点控件 Chadian Controls

> 目标：把“AI 生成一次”升级为 **可编辑、可调、可复用的 Blender Deliverable**。

标准流程：

**AI 生成 → 暴露少量关键参数 → 人工快速调整 → 保存 Preset / Template → 下次复用**

---

## 1｜硬规则

1. **Editable First**：用户不应为了小改再次截图让 Agent 重跑代码。
2. **5–15 Core Controls**：默认只暴露真正高频、视觉敏感的参数。
3. **Macro First**：优先“镜头距离 / 动画强度 / 错帧 / 线条粗细”这种设计意图，不优先暴露几十个底层节点值。
4. **Native Control First**：Custom Properties / Drivers / GN Inputs / Constraints 优先。
5. **UI ≠ Data**：N Panel 只是控制表面，数据应存在于 .blend 内的稳定属性里。
6. **Template Ready**：满意结果应能保存为 Preset / Asset / Template。
7. **Agent Optional**：日常微调尽量不依赖 Agent 在线。

---

## 2｜三层控件

### Level 1｜Quick
默认展开，约 5–8 个。

适合：
- 主体位置 / 尺度
- Camera Distance / Lens
- 主色 / 强调色
- Motion Speed / Strength
- Stagger
- Light Strength
- Background

### Level 2｜Edit
需要时展开，约 10–20 个。

适合：
- 布局间距
- 每组 Offset
- Path Bend
- Overshoot / Settle
- DOF / Focus
- Material roughness / emission
- 几何密度 / thickness
- Secondary Motion

### Level 3｜Advanced
默认隐藏。

适合：
- Shader / Nodes 内部值
- 复杂 Driver
- Geometry Nodes 技术参数
- Simulation
- Render / Sampling
- Debug

**不要把“能暴露”误当成“应该暴露”。**

---

## 3｜默认分类

统一概念 Schema：

```text
CONTENT
LAYOUT
CAMERA
STYLE
MOTION
TIMING
LIGHTING
ASSET
OUTPUT
PRESET
ADVANCED
```

N Panel 可使用同样分组。

---

## 4｜Blender 实现层

推荐链路：

```text
CTRL_MASTER Custom Properties
        ↓
Drivers / Constraints / GN Modifier Inputs
        ↓
Scene / Camera / Objects / Materials / Nodes
        ↓
N Panel 显示高频控件
```

### 数据源

优先：
- `CTRL_MASTER["motion_strength"]`
- `CTRL_MASTER["stagger"]`
- `CTRL_MASTER["camera_distance"]`
- `CTRL_MASTER["line_thickness"]`
- `CTRL_MASTER["light_strength"]`

对于对象级局部控制，可放在对应 `CTRL_xxx` Empty。

### N Panel

Python Panel 负责：
- 分类显示；
- Tooltip；
- Quick / Edit / Advanced 折叠；
- Preset / Reset / Replace Asset / Preview 按钮；
- 一次性 Utility Operator。

**不要让 Panel 自己持有唯一状态。**

### Driver

适合：
- 比例映射；
- 共享强度；
- Position / Scale Offset；
- Camera / Focus；
- Material / Modifier 参数；
- Geometry Nodes 输入。

Driver 表达式尽量短、可读，不堆复杂 Python 黑盒。

### Geometry Nodes

适合：
- 重复对象；
- 阵列 / 布局；
- 参数化线框；
- 可替换资产实例；
- procedural background / structure。

用户高频参数应提升到 Group Interface / Modifier Inputs 或 CTRL Driver。

---

## 5｜Animation + Manual Override

差点控件必须保留手改入口。

优先顺序：
1. Animated Parent + Child Local Transform；
2. Delta Transform；
3. Custom Property + Driver offset / multiplier；
4. NLA Add / Combine（确实需要动作叠加时）。

不要：
- 为了“参数化”把所有 Transform 都锁死；
- 用户一拖对象就破坏 Driver；
- 只能通过 Python 修改对象位置；
- 把简单 Offset 做成复杂节点系统。

---

## 6｜Preset

Preset 应保存“设计意图”，例如：
- CLEAN_PRODUCT
- HEAVY_TECH
- SLOW_FLOAT
- FAST_PUNCH
- DARK_STUDIO

Preset 可以修改一组 Custom Properties，但不要复制整个场景结构。

需要时增加：
- Save Preset
- Load Preset
- Reset Quick
- A/B Variation

---

## 7｜Asset Replacement

模板中的可替换资产要有明确插槽：
- HERO_ASSET
- LOGO_ASSET
- TEXTURE_SLOT
- ENV_ASSET

替换时优先保持：
- Parent；
- Constraint；
- Material Interface；
- Bounding / scale 规则；
- 控件关系。

不要让“换一个模型”变成重建场景。

---

## 8｜统一 Chadian Control Schema

任何平台都可以映射同一意图：

```yaml
id: camera.distance
label: 镜头距离
group: CAMERA
level: quick
type: float
default: 6.0
min: 2.0
max: 15.0
target: CTRL_MASTER.camera_distance
meaning: 控制主体在画面中的空间压缩与距离感
```

Blender → Custom Property / N Panel  
AE → Control Layer / Essential Property  
Web → UI Control

Schema 的意义是**统一“人要改什么”**，不是强制三个平台内部实现一致。

---

## 9｜何时必须做控件

强制：
- 用户明确说“做成模板 / 控件 / 以后复用”；
- 同类镜头未来会反复出现；
- 需要频繁换素材 / 文案 / 镜头 / 颜色；
- Agent 生成后仍需要大量截图沟通小改。

不强制：
- 一次性实验；
- 尚未锁定视觉；
- 低频技术内部参数；
- 用户永远不会手调的系统值。

---

## 10｜通过标准

用户无需 Agent 就能在 30 秒内找到并修改：
- 主体；
- Camera；
- Style；
- Motion；
- Timing；
- Light；
- Asset。

并且：
- 调控件不破坏主动画；
- 工程不开 Panel 脚本也不应整体报废；
- Preset 可恢复；
- 高级参数默认不污染日常 UI。
