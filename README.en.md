# One Paper Company (一个公司一张纸)

> Turn any company into a shareable, offline-capable, printable deep-research HTML single page.

## What is this

A TRAE Skill: input `company name + quarter`, automatically fetch market/financial/segment/event data, inject into template, and produce a **self-contained HTML** with the same aesthetic as the NVIDIA 2026Q3 review page (run `python scripts/build_html.py references/nvidia_data.json output.html` to generate the NVIDIA sample locally).

**v2.0.0 Upgrade**: Evolved from single cycle-review form to a 4-product-form framework:
- **Cycle Review** (v1.0 implemented): 10-step scroll-linked + 15 chart types
- **Quick Card** (v2.1 roadmap): Single-page KPI + key events + trend mini-charts
- **Valuation Map** (v2.2 roadmap): Valuation history + peer comparison + DCF scatter
- **Competitive Landscape** (v2.3 roadmap): Industry landscape wall + clearance history + share chart

## Trigger Words

**Main entry** (any one enters the skill):
- 「一个公司一张纸」
- 「一公司一纸」
- 「公司一张纸」

**Form-specific**:
- 「公司周期复盘」/「scrollytelling 复盘」→ Cycle Review form
- 「公司速览卡」/「公司一页速览」→ Quick Card form
- 「公司估值地图」/「公司估值」→ Valuation Map form
- 「公司竞争格局」/「公司格局」→ Competitive Landscape form

## User Guidance Examples

```
What you say → What the skill does
─────────────────────────────────────────────────────────────
"用 AMD 做一个公司一张纸"            → List 4 forms for you to choose
"用台积电做一个公司周期复盘"          → Directly enter 10-step scroll form
"用特斯拉做一个公司速览卡"            → Directly enter single-page KPI form
"用英伟达做一个公司一张纸，2026Q3"   → Full params + choose form
"一个公司一张纸：茅台 2026Q2"        → Full params + choose form
```

## Product Features (Cycle Review form)

- **10-step scroll linkage**: scrollytelling narrative, left text + right chart, sticky linkage
- **15 data visualizations**: Financial chart, segment stacked, landscape wall, K-line, high-freq demand, inventory, analogy scoring, radar, 8-sector clock, 8 signal cards
- **Warm neutral + brand color**: Only `--green`/`--green-d` replaced per company, rest universal
- **ECharts 5.5.0 inline**: 1MB library inlined, offline self-contained, double-click to open
- **Pixel-font brand animation**: A-Z + 0-9 pixel font alphabet, renders company ticker
- **Responsive**: Landscape (≥1181px) / Medium (961-1180px) / Portrait (≤960px) / Small (≤560px) / Print PDF

## Quick Start

In TRAE, say:

```
用 AMD 做一个公司一张纸，2026Q2
```

Or:

```
对台积电做公司周期复盘
```

The skill automatically enters a two-phase workflow:
1. **Phase A**: Fetch objective data → Generate draft HTML (immediately deliverable)
2. **Phase B**: AskUserQuestion to confirm research views (scores/excluded/radar/clock/signals) → Regenerate final HTML

## File Structure

```
one-paper-company/
├── SKILL.md                       # Skill triggers + workflow + product forms
├── README.md                      # Chinese file
├── README.en.md                   # English file (this file)
├── CHANGELOG.md                   # Change log
├── LICENSE                        # MIT
├── plugin.json                    # Claude plugin metadata
├── template/
│   ├── template.html              # Cycle Review template (38KB, __PLACEHOLDER__ tokens)
│   ├── echarts.5.5.0.min.js       # ECharts 5.5.0 UMD (1MB, for inlining)
│   └── pixel-font.js              # A-Z + 0-9 pixel font (2KB)
├── scripts/
│   ├── build_html.py              # data.json + template → HTML
│   └── validate.py                # Output validation
└── references/
    ├── nvidia_data.json           # NVIDIA original page data (regression baseline)
    ├── amd_data.json              # AMD real-world data (end-to-end validation)
    └── ...                        # 4 detailed spec documents
```

## User Warnings (read before running)

**Side-effect scope of this skill**:
- **Network requests**: Phase A auto-calls WebSearch/WebFetch to fetch market/financial/event data (disable: skip Phase A, provide data.json directly)
- **File writes**: Writes HTML to user-specified output path (default: desktop, configurable via `--output`)
- **subprocess calls**: Calls `python scripts/build_html.py` and `python scripts/validate.py`
- **Does NOT read**: User memory/profile/credentials or any sensitive files
- **Does NOT write**: System directories, user config directories, or other skill directories

**Disabling side effects**:
- Skip Phase A: provide data.json directly, skip data fetching
- Skip Phase B: use draft HTML as final product
- Skip auto-write: omit `--output` parameter, only preview generated content

## build_html.py Usage

```bash
python scripts/build_html.py references/nvidia_data.json output.html
```

Optional args:
- `--template <path>`: Custom template (default `template/template.html`)
- `--pixel-font <path>`: Custom pixel font (default `template/pixel-font.js`)
- `--echarts <path>`: Custom ECharts (default `template/echarts.5.5.0.min.js`)

## Color Rules

**Fixed** (neutral colors):
```
--bg:#f7f5f0 --panel:#fffdf9 --ink:#20242b --ink2:#3a414c --muted:#717a86
--line:#e4e0d6 --soft:#efede6
--navy:#24425e --blue:#2c7be5 --red:#c0392b --up:#c0392b --down:#2e9e5b --gold:#b98a1d
```

**Per-company replacement** (only these two):
- `--green`: Brand color (default `#76b900`)
- `--green-d`: Darker variant (default `#5a9200`)

## Known Limitations

- Research views (wall/scores/excluded/radar/signals) require manual confirmation
- Single company single page (no multi-company comparison)
- Chinese only (no English version)
- K-line red-up green-down (Chinese color scheme, universal across companies)

## Version

- v2.0.0 | 2026-07-25 | Renamed to one-paper-company, 4-product-form framework
- v1.0.0 | 2026-07-24 | Initial version, reverse-engineered from NVIDIA 2026Q3 review page

---

**中文版**: [README.md](README.md)
