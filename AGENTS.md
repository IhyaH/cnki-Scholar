# Repository Guidelines

## Project Structure & Module Organization
本仓库是一个原生 Manifest V3 浏览器扩展，没有打包层。`manifest.json` 定义权限、`background.js` 和内容脚本入口。`content.js` 负责在 CNKI 页面注入下载按钮、摘要悬浮层、期刊标签等主要逻辑；`content.css` 维护注入 UI 的样式；`background.js` 处理扩展安装事件和跨域请求转发；`download-worker.js` 用于批量下载节流。静态资源目前只有 `icon.png`，说明和截图集中在 `README.md`。

## Build, Test, and Development Commands
本项目无 `npm`/`make` 构建流程，开发依赖浏览器手动加载。

- `chrome://extensions/` 或 `edge://extensions/`：开启“开发者模式”后选择“加载已解压的扩展程序”。
- 修改脚本或样式后点击扩展页的“刷新”，再重开 CNKI 页面验证。
- `git status`：检查是否只包含预期改动。

若新增工具链，请在 PR 中同步补充本节。

## Coding Style & Naming Conventions
JavaScript 与 CSS 保持现有的原生风格：优先使用 ES6+、函数名采用 `camelCase`，CSS 类名使用 kebab-case，例如 `.pdf-download-btn`。新增代码建议统一 2 空格缩进、保留分号，并将 DOM 选择器、网络请求、页面注入逻辑按功能分组，避免把多站点兼容分支揉成超长函数。

## Testing Guidelines
仓库当前没有自动化测试；提交前必须做手工回归。至少验证以下场景：

- CNKI 检索结果页能正常注入“下载”和“下载所有 PDF”按钮。
- 悬浮摘要、关键词标签、期刊标签在新版/旧版页面不报错。
- 批量下载流程不会因节流逻辑卡死。

如修复页面适配问题，请在 PR 描述中写明验证页面类型，例如“检索页”“详情页”“国际版”。

## Commit & Pull Request Guidelines
历史提交以简短中文标题为主，偏功能导向，例如“功能更新”“增加详情页tags”。延续这一风格：每次提交只做一个主题，标题控制在一行内，直接说明变更点。PR 需要包含变更摘要、影响页面、手工验证步骤；涉及 UI 的改动请附截图或 GIF；若修改权限、下载逻辑或外部请求，请明确说明风险与回退方式。

## Security & Configuration Tips
不要提交账号、Cookie 或任何 CNKI 会话信息。新增外部请求前，先同步更新 `manifest.json` 中的 `permissions` 与 `host_permissions`，并说明用途，避免过度授权。
