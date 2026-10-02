# Engineering｜Blender Project Architecture

## 1｜默认 Collection

复杂镜头建议：

```text
00_CTRL
01_CAM
02_HERO
03_ENV
04_LIGHT
05_FX
06_ASSET
07_BG
90_GUIDE
99_DEBUG
```

不是每个项目都必须全用；空分类不创建。

Collection 用于组织。
共享 Transform 使用 Empty / Rig Object。

## 2｜核心控制对象

推荐命名：
- `CTRL_MASTER`
- `RIG_SCENE`
- `RIG_CAMERA`
- `TARGET_CAMERA`
- `TARGET_FOCUS`
- `RIG_<GROUP>`
- `CTRL_<SYSTEM>`

用户可见命名可中文优先；机器接口 / property id 保持稳定英文 snake_case。

## 3｜Camera Rig

默认拆分：

```text
RIG_CAMERA
└─ CAMERA_MAIN

TARGET_CAMERA
TARGET_FOCUS
```

按需求使用：
- Rig Transform → Dolly / Truck / Crane / Orbit；
- Target → Aim；
- Focus Target → DOF；
- Camera Local → 少量镜头自身微调。

不要把 Position、Aim、Focus 全靠 Camera 本体大量 Key 手工同步。

## 4｜Scene / LookDev 与 Motion 分离

优先让：
- Model / Material / Light / Environment 能静态成立；
- Rig 单独承担结构关系；
- Action / F-Curve 单独承担 Motion；
- Controls 只暴露高频参数。

这样可以替换 Motion 而不重做 LookDev。

## 5｜Asset 组织

可复用资产优先：
- 明确命名；
- 原点 / scale / orientation 统一；
- 材质接口清楚；
- 依赖本地化；
- 不保留无用临时数据；
- 需要多场景复用时再进入 Asset Browser / Library 体系。

### Link / Override 谨慎

需要母版同步时可以使用 linked library / Library Override，但不要假设所有动画都能像本地 Action 一样自由编辑。

对于“用户之后经常手改动画”的模板：
- 优先保持 Animation / Action 本地可编辑；
- 不把所有关键 Motion 锁在难编辑的 linked data 里。

## 6｜Markers

Timeline Marker 用于：
- Motion Phase；
- 口播节点；
- Camera Event；
- Build / Review Pose；
- 章节。

示例：
- `IN`
- `HERO`
- `TURN`
- `SETTLE`
- `OUT`

Marker 是语义锚点，不代替真正 Timing Graph。

## 7｜Python 边界

Python 优先做：
- 创建结构；
- 批量生成；
- 参数注册；
- Panel；
- Operator；
- 一次性重构；
- QA / audit。

尽量不要：
- 用每帧 handler 替代 Driver / Constraint；
- 把核心关系只存在 Python 内存；
- 让 .blend 离开脚本就无法理解。

## 8｜完成条件

打开 Outliner 时：
- 30 秒内看懂主要结构；
- Camera / Hero / Light / FX 分区明确；
- 控制对象集中；
- 临时对象不混在最终资产中；
- 用户能定位“改什么去哪里”。
