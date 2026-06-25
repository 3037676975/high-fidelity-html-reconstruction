# High Fidelity HTML Reconstruction

> 一个用于 Codex 的高保真截图/设计稿转 HTML 技能。它强调严格的素材来源、可验证的资产链路，以及尽量接近原图的页面还原效果。

## 中文说明

```text
适用场景：
- 把上传的网页截图、App 截图、UI 设计稿、活动页、落地页等复刻成 HTML/CSS。
- 需要高保真视觉还原，而不是简单搭一个相似布局。
- 页面里包含大量图片化区域、复杂插画、活动 Banner、卡片、徽章、特殊艺术字或组合视觉素材。
- 用户明确要求使用 green_source.png、GPT img2img、绿幕素材提取、截图转 HTML、设计稿还原等流程。
```

这个技能的核心价值，是把“截图只能作为视觉参考”和“页面素材必须来自可追溯的生成源”分开处理，避免直接裁剪原图、偷用截图局部、或者用不可验证的临时素材糊弄结果。

## 核心流程

必须按照下面顺序执行，不能跳步，也不能改变顺序：

1. **原始截图只作为视觉参考**
   - 不把原始截图直接放进最终页面。
   - 不从原始截图裁剪 Banner、图标、卡片、背景、人物、商品图等素材。

2. **先用 GPT/img2img 生成 green_source.png**
   - 使用上传截图作为唯一视觉参考。
   - 生成一张高分辨率绿幕素材总图，文件名必须是 `green_source.png`。
   - 背景必须是纯 `#00FF00`。
   - 所有需要作为图片的视觉资产，都要独立放在绿幕背景上，并留足间距。

3. **用 Python 从 green_source.png 提取素材**
   - Python 只能读取 `green_source.png` 来提取图片素材。
   - 不允许读取原始截图像素来裁剪资产。
   - 提取结果输出到 `assets/`。
   - 同时生成 `asset_manifest.json`，记录每个资产的来源框、尺寸、缩放、透明处理方式等信息。

4. **用 HTML/CSS 重建页面**
   - 普通文字、导航、按钮、布局、间距、颜色、圆角、阴影等用 HTML/CSS 实现。
   - Banner、插画、复杂卡片、艺术字、徽章、商品/人物/场景图等使用从绿幕源图提取的资产。
   - 最终页面入口通常为 `index.html`。

5. **浏览器验证**
   - 打开 `index.html`。
   - 截取验证图。
   - 检查是否有绿色残留、素材变形、文字溢出、模块错位、比例错误、清晰度不足等问题。

## 适合保留为图片资产的内容

以下内容通常应该作为完整图片区域保留：

- 活动 Banner、广告图、宣传卡片、英雄视觉区。
- 商品图、人物图、场景图、复杂插画、视频封面。
- 带有内部文字、标签、徽章、角标、光效、复杂背景的组合视觉卡片。
- 特殊艺术字、Logo、图标徽章、状态标识、复合 UI 装饰。

这些内容如果强行拆成 CSS，往往会导致还原度下降、细节丢失或视觉不自然。

## 适合用 HTML/CSS 重建的内容

以下内容通常应该用真实页面元素实现：

- 普通导航文字、菜单项、标题、副标题、说明文字。
- 简单按钮、表单、价格、日期、统计数字、普通标签。
- 普通卡片容器、栅格布局、分割线、背景色、圆角、阴影。
- 响应式布局、滚动区域、固定栏、基础交互状态。

## 仓库结构

```text
high-fidelity-html-reconstruction/
├─ SKILL.md
├─ agents/
│  └─ openai.yaml
└─ references/
   └─ strict-green-source-workflow.md
```

### 文件说明

- `SKILL.md`  
  技能主说明文件，定义触发场景、核心规则、工作流和最终交付要求。

- `agents/openai.yaml`  
  Codex 技能界面相关配置，包括显示名称、简介、品牌色和默认提示语。

- `references/strict-green-source-workflow.md`  
  严格绿幕源图工作流的详细规则，包含 green_source 生成规范、Python 提取规范、HTML 重建规范和验证要求。

## 安装方式

把整个目录放到 Codex 的 skills 目录下，例如：

```text
C:\Users\JT\.codex\skills\high-fidelity-html-reconstruction
```

安装后，可以在对话里通过下面方式调用：

```text
$high-fidelity-html-reconstruction
```

也可以在用户提出“截图转 HTML”“设计稿还原”“高保真复刻网页”“使用绿幕素材工作流”等请求时自动触发。

## 典型使用提示词

```text
请以我上传的截图为唯一视觉参考，使用 $high-fidelity-html-reconstruction，严格按照 green_source.png -> Python 提取 assets -> HTML/CSS 重建 -> 浏览器验证 的流程，把这张图高保真还原成 HTML 页面。
```

## 最终交付物

一次完整执行通常应该产出：

- `green_source.png`
- `assets/*.png`
- `asset_manifest.json`
- `index.html`
- 必要的本地 CSS/JS 文件
- 浏览器验证截图

## 强约束

- 不允许直接裁剪原始截图作为最终页面素材。
- 不允许用网络图片、占位图、手绘 SVG 或随意生成的临时图替代绿幕提取资产。
- 不允许跳过 GPT/img2img 生成 `green_source.png` 的步骤。
- 不允许用 Python、Canvas、SVG、CSS 本地伪造 `green_source.png`。
- 如果当前环境没有真实 img2img 或图像生成能力，应该明确报告阻塞，而不是假装已经完成。

## English Summary

High Fidelity HTML Reconstruction is a Codex skill for converting screenshots and UI mockups into high-fidelity HTML pages through a strict green-source asset workflow. The original screenshot is used only as a visual reference. Image assets must first be regenerated into `green_source.png` through GPT/img2img, then extracted by Python into `assets/`, documented in `asset_manifest.json`, and finally assembled with real HTML/CSS.

This makes the reconstruction process more traceable, repeatable, and easier to verify.
