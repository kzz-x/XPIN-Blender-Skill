# Recipe｜Reusable Blender Template

用于：“做成模板 / 以后复用 / 做成控件 / 不想每次截图让 Agent 改”。

## 1｜流程

```text
V1 Scene
→ 人工验收
→ 找高频修改项
→ 参数化
→ 差点控件
→ Asset Replacement
→ Preset
→ Preview
→ Template Package
```

不要在视觉还没通过时提前工程化几十个控件。

## 2｜模板组成

建议至少有：
- .blend；
- Preview；
- README / AI_README；
- CTRL_MASTER；
- 清晰 Collection；
- 可替换资产槽；
- 5–15 个 Core Controls；
- Preset；
- 依赖说明；
- 使用条件 / 禁止事项。

## 3｜Control Surface

按 `engineering/chadian-controls.md`：
- Quick；
- Edit；
- Advanced。

首版优先包含：
- 内容 / 素材；
- Position / Scale / Rotation；
- Camera；
- 主色 / Background；
- Light；
- Motion Speed / Strength；
- Stagger / Ease-related macro；
- Output。

## 4｜Asset Replacement

替换 Hero 时：
- 不破坏 Rig；
- 不破坏 Camera；
- 不破坏 Material Interface；
- 尽量自动处理 scale / origin；
- 允许用户手动修正。

## 5｜模板复用策略

优先：
- Duplicate / Append → 完全自由修改；
- Asset Browser → 组件复用；
- Linked Library + Override → 需要母版同步时再用。

如果模板的核心价值是“用户之后大量手调动画”，不要把关键动画全部锁进难改的 linked override。

## 6｜Agent 使用

Agent 以后调用模板时：
1. 读模板 metadata；
2. 替换输入；
3. 改 exposed controls；
4. 必要时局部 Patch；
5. 不重建模板内部成熟 Rig。

## 7｜完成标准

一个模板成功的标志不是“UI 很多”，而是：

**下一次同类任务，大多数修改都能靠替换资产 + 调少量参数完成。**
