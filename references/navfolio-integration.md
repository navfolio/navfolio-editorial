# Navfolio 内的实现归属

仅在修改 Navfolio 生态源码时使用。以下根据首版整理时的本地源码核对；路径用于定位，实际修改前重新检查当前源码、包 exports 和 consumer。此 skill 在没有这些仓库时也可独立使用。

## 路由表

| 需求 | 优先检查 |
| --- | --- |
| 选择内置色板、配置字体 | astro-navfolio/src/config/site.toml |
| 新增色板 | theme-default/src/styles/palettes.css 与 astro-navfolio/src/content.config.ts 的 paletteSchema |
| 共享颜色、字体角色、容器、基础视觉 | theme-default/src/styles/global.css |
| 文章与列表视觉 | theme-default/src/styles/blog-article.css、blog-post-list.css 等 |
| 首页布局与未抽取 UI | astro-navfolio/src/components/layout/DashboardFlow.astro 及主站组件 |
| 默认主题组件与 exports | theme-default/src/components、package.json、src/index.ts |
| 主题公共类型 | core/src；保持 core 不依赖默认主题 |
| 页面模块样式 | 对应模块仓库，按其真实 owner 修改 |

`themes` 在首版核对时只有 README 和 LICENSE，其 token/type 定位尚无实现；不要假设存在可导入的共享 token 包。

## 关键约束

- 色彩家族由服务端输出的 `data-palette` 选择，明暗偏好由 `data-theme` 控制。新增色板同时维护明暗选择器与主站枚举，避免只改 CSS 而配置无法通过校验。
- 主站仍有直接导入默认主题组件的 compatibility wrappers。已有 theme manifest 不代表所有主题切换、组件覆盖和 slots 都已完成运行时接入。
- `.glass-card` 是历史名称，当前视觉是纸面、细边框和轻阴影；不要按名字恢复成毛玻璃。
- 全局 CSS 有历史规则和后续覆盖，提取参数时检查最终层叠结果。例如后面的 body 定义覆盖早期的字号与背景。
- 主站通过 GitHub spec 和 lockfile 消费包，同级 working tree 不自动参与构建。包变更与 consumer 验证按工作区实际开发流程执行；skill 本身不授权推送或发布。
- 旧色板文档仍可能引用主站 src/styles/palettes.css，而当前 owner 是 theme-default；README 的迁移进度也可能落后于 exports。以源码为准。

## 验证范围

选择受影响的首页、列表、文章和模块页面，在支持的明暗模式及宽窄屏中检查。新增色板除 CSS 外还要验证配置 schema 与最终 HTML 属性。

使用当前仓库 AGENT.md/AGENTS.md 指定的命令。主站常用 format:check、build、docs:build；包内有自己的 build/typecheck。只有真正构建了修改后的依赖，consumer 检查才覆盖该包改动。
