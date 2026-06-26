# PRD 文档目录

本目录用于沉淀本次「高保真截图转 HTML」项目的产品需求、交付说明与验收标准，方便后续继续精修、复用流程或交接给其他人维护。

## 文件说明

- `project-prd.md`：项目主 PRD，包含项目背景、目标、范围、页面结构、视觉还原要求、技术方案、交付物与风险说明。
- `acceptance-checklist.md`：验收清单，用于逐项检查页面还原、响应式、资源加载、部署和浏览器验证。

## 当前线上地址

- Vercel 生产地址：https://high-fidelity-html-reconstruction.vercel.app

## 当前核心交付物

- `index.html`：最终静态页面。
- `assets/`：从 `green_source.png` 提取出的页面素材。
- `green_source.png`：按高保真流程生成的绿色背景素材源图。
- `asset_manifest.json`：素材提取清单。
- `vercel.json`：Vercel 静态部署路由配置。
- `vercel-production-verification.png`：线上部署后的浏览器验证截图。
