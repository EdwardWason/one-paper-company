# Change Log

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [2.0.0] - 2026-07-25

### Added
- **重大升级**：技能从 `industry-cycle-scrollytelling` 重命名为 `一个公司一张纸` (slug: `one-paper-company`)
- **多产物形态框架**：新增 4 种产物形态说明（周期复盘/速览卡/估值地图/竞争格局）
- **触发词体系**：3 个主入口触发词 + 4 种形态直达触发词
- **用户引导示例**：首次触发时 AI 主动展示 10 种示例用法
- **响应式修复**：新增 961-1180px 中等屏幕断点 + @media print 打印样式
- **滚动跟随修复**：rootMargin 恢复为 `-38% 0px -52% 0px`（与原始素材 1:1 对齐）+ tail spacer + resize 防抖 + orientationchange 监听
- **目录重命名**：`industry-cycle-scrollytelling` → `one-paper-company`
- **YAML frontmatter**：添加完整 frontmatter 元数据（name/slug/displayName/description/version/license/summary/allowed-tools/metadata）
- **plugin.json**：添加 Claude 插件元数据
- **CHANGELOG.md**：独立变更日志文件（从 SKILL.md 抽离）
- **LICENSE**：MIT 许可证
- **.gitignore**：排除大文件和临时文件
- **README.en.md**：英文版说明
- **.github/ISSUE_TEMPLATE/**：社区模板（bug_report/feature_request/question）
- **.github/pull_request_template.md**：PR 模板
- **references/ 子文档**：拆分 4 个详细规范文档（product-forms/data-contract/responsive-spec/exception-handling）

### Changed
- SKILL.md 从 345 行精简到 143 行（详细内容拆分到 references/）
- validate.py / extract_nvidia_data.py 旧名引用修复

### Fixed
- **滚动联动不同步**：rootMargin 从 `-30% 0px -60% 0px` 恢复为 `-38% 0px -52% 0px`
- **s9/s10 不激活**：添加 `.steps::after` 60vh tail spacer
- **图表不显示**：移除模板占位符的注释包裹（`/*__X__*/` → `__X__`）
- **JS 语法错误**：JS 上下文字符串占位符用 `js_val()` 包裹

### Removed
- `_show_steps.py`：临时调试脚本，引用旧路径

---

## [1.0.0] - 2026-07-24

### Added
- 基于英伟达 2026Q3 scrollytelling 复盘页 1:1 逆向抽离
- 实现 10 步滚动联动 + 15 类数据可视化
- 完成 build_html.py 核心引擎 + data.json schema
- AMD 端到端验证通过
