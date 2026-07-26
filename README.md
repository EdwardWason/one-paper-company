# 一个公司一张纸 (one-paper-company)

> 把任意公司做成一张可分享、可离线、可打印的深度研究 HTML 单页

## 这是什么

一个 TRAE Skill：输入「公司名 + 季度」，自动抓行情/财务/分部/事件数据，注入模板，产出与英伟达 2026Q3 复盘页同款审美的**自包含 HTML**（运行 `python scripts/build_html.py references/nvidia_data.json output.html` 即可在本地生成英伟达样例）。

**v2.0.0 升级**：从单一周期复盘形态升级为 4 种产物形态框架：
- **周期复盘**（v1.0 已实现）：10 步滚动联动 + 15 类图表
- **速览卡**（v2.1 待实现）：单页 KPI + 关键事件 + 趋势小图
- **估值地图**（v2.2 待实现）：估值历史 + 同业对比 + DCF 散点
- **竞争格局**（v2.3 待实现）：行业格局墙 + 出清史 + 份额图

## 触发词

**主入口**（任一即可进入技能）：
- 「一个公司一张纸」
- 「一公司一纸」
- 「公司一张纸」

**形态直达**：
- 「公司周期复盘」/「scrollytelling 复盘」→ 周期复盘形态
- 「公司速览卡」/「公司一页速览」→ 速览卡形态
- 「公司估值地图」/「公司估值」→ 估值地图形态
- 「公司竞争格局」/「公司格局」→ 竞争格局形态

## 用户引导示例

```
你说什么 → 技能做什么
─────────────────────────────────────────────────────────────
"用 AMD 做一个公司一张纸"            → 列出 4 种形态让你选
"用台积电做一个公司周期复盘"          → 直接进 10 步滚动形态
"用特斯拉做一个公司速览卡"            → 直接进单页 KPI 形态
"用英伟达做一个公司一张纸，2026Q3"   → 完整参数 + 选形态
"一个公司一张纸：茅台 2026Q2"        → 完整参数 + 选形态
"用比亚迪做一张公司纸"               → 列出形态
```

## 产物特征（周期复盘形态）

- **10 步滚动联动**：scrollytelling 叙事，左文右图，sticky 联动
- **15 类数据可视化**：财务季图、分部堆叠、格局墙、K 线、高频需求、库存、类比打分、雷达、8 扇区时钟、8 信号卡
- **暖米中性色 + 品牌色**：仅 `--green`/`--green-d` 按公司替换，其余配色跨公司通用
- **ECharts 5.5.0 内联**：1MB 库内联，离线自包含，双击即开
- **像素风品牌动画**：A-Z + 0-9 像素字字母表，渲染公司 ticker
- **响应式自适应**：横屏（≥1181px）/ 中等屏幕（961-1180px）/ 竖屏（≤960px）/ 小屏（≤560px）/ 打印 PDF

## 快速使用

在 TRAE 中说：

```
用 AMD 做一个公司一张纸，2026Q2
```

或：

```
对台积电做公司周期复盘
```

Skill 自动进入两段式工作流：
1. **Phase A**：抓客观数据 → 生成草案 HTML（可直接交付）
2. **Phase B**：AskUserQuestion 逐项确认研究观点（scores/excluded/radar/clock/signals）→ 重生成最终 HTML

## 文件结构

```
one-paper-company/
├── SKILL.md                       # Skill 触发词 + 流程编排 + 多产物形态说明
├── README.md                      # 本文件
├── template/
│   ├── template.html              # 周期复盘形态模板（38KB，__PLACEHOLDER__ 槽位）
│   ├── echarts.5.5.0.min.js       # ECharts 5.5.0 UMD（1MB，内联用）
│   └── pixel-font.js              # A-Z + 0-9 像素字（2KB）
├── scripts/
│   ├── build_html.py              # data.json + template → HTML
│   └── validate.py                # 产物校验
└── references/
    ├── nvidia_data.json           # 英伟达原页数据（回归基准）
    ├── amd_data.json              # AMD 实战数据（端到端验证）
    └── ...                        # 4 个详细规范文档
```

## 用户警告（运行前必读）

**本技能的副作用范围**：
- **网络请求**：Phase A 会自动调用 WebSearch/WebFetch 抓取公司行情、财务、事件数据（关闭方式：跳过 Phase A，直接提供 data.json）
- **文件写入**：会写入用户指定 output 路径的 HTML 文件（默认桌面，可通过 `--output` 参数指定）
- **subprocess 调用**：会调用 `python scripts/build_html.py` 和 `python scripts/validate.py`
- **不读取**：用户 memory/profile/credentials 等敏感文件
- **不写入**：系统目录、用户配置目录、其他 skill 目录

**禁用副作用的方式**：
- 跳过 Phase A：直接提供 data.json，跳过数据抓取
- 跳过 Phase B：使用草案 HTML 作为最终产物
- 跳过自动写入：不传 `--output` 参数，仅查看生成内容

## build_html.py 用法

```bash
python scripts/build_html.py references/nvidia_data.json output.html
```

可选参数：
- `--template <path>`：自定义模板（默认 `template/template.html`）
- `--pixel-font <path>`：自定义像素字（默认 `template/pixel-font.js`）
- `--echarts <path>`：自定义 ECharts（默认 `template/echarts.5.5.0.min.js`）

## validate.py 用法

```bash
python scripts/validate.py output.html --data references/nvidia_data.json
```

检查 8 项：结构完整性、10 步联动、数据注入、配色铁律、占位符、资产内联、体积、信源三层。

## 数据契约

完整 schema 见 `SKILL.md`。核心 34 个顶层键：

| 层 | 字段 | 说明 |
|---|---|---|
| 行情 | pre/closes/candles/events | 稀疏史前点 + 月K + 事件标注 |
| 财务 | fin/seg/hf/inv | 季财务 + 分部 + 高频需求 + 库存 |
| 研究 | wall/fateClass/scores/excluded/radar/stages/clock/signals | 格局墙 + 类比 + 雷达 + 时钟 + 信号 |
| 文案 | hero/steps/outro/footer | 标题 + 10 步正文 + 结语 |
| 品牌 | brand/quarter/asOf | 品牌色 + 季度 + 截至日 |

## 配色铁律

**固定不变**（中性色）：
```
--bg:#f7f5f0 --panel:#fffdf9 --ink:#20242b --ink2:#3a414c --muted:#717a86
--line:#e4e0d6 --soft:#efede6
--navy:#24425e --blue:#2c7be5 --red:#c0392b --up:#c0392b --down:#2e9e5b --gold:#b98a1d
```

**按公司替换**（仅这两个）：
- `--green`：品牌色（默认 `#76b900`）
- `--green-d`：深一档（默认 `#5a9200`）

## 回归测试

```bash
# 生成英伟达回归 HTML（产物默认不提交，由 .gitignore 和 .clawhubignore 排除）
python scripts/build_html.py references/nvidia_data.json output/nvidia_regression.html

# 校验
python scripts/validate.py output/nvidia_regression.html --data references/nvidia_data.json
```

预期：8 项全部通过，2.16 MB，无真实占位符残留。

## 10 步叙事结构（固定）

| Step | 联动面板 | 内容 |
|---|---|---|
| 1 | vp-fin | 财务季图（营收/净利/毛利率）|
| 2 | vp-seg | 分部堆叠柱（5 分部财年）|
| 3 | vp-wall | 格局墙（2 波 × 10-15 家）|
| 4 | vp-kline | K 线全周期（含稀疏史前点）|
| 5 | vp-kline | K 线 focus 模式（近 6 年）|
| 6 | vp-hf | 高频需求（capex/dcrev/租金）|
| 7 | vp-inv | 库存天数 |
| 8 | vp-score | 类比打分 + 负对照 |
| 9 | vp-clock | 8 扇区时钟 |
| 10 | vp-signal | 8 信号卡 |

## 已知限制

- 研究观点（wall/scores/excluded/radar/signals）依赖人工确认
- 单公司单页（不做多公司对比）
- 中文版（不做英文）
- K 线红涨绿跌（中国色系，跨公司通用）

## 版本

- v1.0.0 | 2026-07-24 | 初版，基于英伟达 2026Q3 复盘页逆向抽离
