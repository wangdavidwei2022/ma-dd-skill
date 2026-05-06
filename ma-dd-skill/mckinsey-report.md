# McKinsey-Style HTML Report Generator

本文件定义了如何将 M&A DD Skill 各 Phase 的分析产出，组装为一份麦肯锡风格的完整 HTML 报告。

---

## 触发条件

Phase 8 自动触发。用户也可以在任意阶段说「生成报告」「出报告」「输出HTML报告」提前生成（使用当前已有数据，缺失 section 标注 "待补充"）。

---

## 报告结构映射

HTML 模板文件：[report-template.html](report-template.html)。生成时读取该模板，将所有 `{{PLACEHOLDER}}` 替换为实际内容。

| 模板 Placeholder | 数据来源 | 说明 |
|------|---------|------|
| `{{REPORT_TITLE}}` | Phase 1 Deal Context | "[目标公司] 收购项目 — 财务尽调分析报告" |
| `{{CONFIDENTIAL_LABEL}}` | 固定 | "机密 · 仅供内部决策使用" |
| `{{COVER_TITLE}}` | Phase 1 | "[Target] 收购项目" |
| `{{COVER_SUBTITLE}}` | Phase 1 | "财务与税务尽调分析报告" |
| `{{COVER_DATE}}` | 当前日期 | "202X年X月" |
| `{{COVER_PREPARED_FOR}}` | Phase 1 | "为 [买方名称] 编制" |
| `{{COVER_PREPARED_BY}}` | 固定 | "[编制方]" |
| `{{EXEC_ANSWER}}` | Phase 7 建议 | 一句话核心结论（行动标题） |
| `{{EXEC_SITUATION}}` | Phase 1 + Phase 2 | 交易背景 + 目标公司简介，2-3句话 |
| `{{EXEC_COMPLICATION}}` | Phase 3 | 核心发现/风险，2-3句话 |
| `{{EXEC_QUESTION}}` | Phase 5 | "在这个价格下，这笔交易值得做吗？" |
| `{{EXEC_ANSWER_DETAIL}}` | Phase 7 | 建议 + 条件，2-3句话 |
| `{{KPI_CARDS}}` | Phase 2 + Phase 5 | 4个KPI卡片：LTM GP, 调整后EBITDA, EV中点, 净现金 |
| `{{E1_*}}` | Phase 1 + Phase 2 | 目标公司概览、业务描述、组织架构 |
| `{{E2_*}}` | Phase 2 + Phase 4 | 历史财务表现 + 趋势SVG图表 |
| `{{E3_*}}` | Phase 3A | 盈利质量分析 + EBITDA瀑布图SVG |
| `{{E4_*}}` | Phase 3B | 净负债桥 + NWC趋势 |
| `{{E5_*}}` | Phase 5 | 估值分析 + Football Field SVG + Damodaran归纳 |
| `{{E6_*}}` | Phase 3C + 3D | 风险矩阵 + 风险登记表 |
| `{{E7_*}}` | Phase 6 | 税务风险 + SPA建议 |
| `{{E8_*}}` | Phase 7 | EV→Equity桥 + 谈判策略 + 建议 + 下一步 |
| `{{APP_DAMODARAN}}` | Phase 5.7 | 完整 Damodaran 7 维度第一人称归纳 |
| `{{APP_*}}` | Phase 4 + Phase 5 | 详细财务模型 + 可比公司 + 敏感性分析 |
| `{{REPORT_REF}}` | Phase 1 | 项目标识，如 "Project [代号] / [目标公司简称]" |

---

## 行动标题（Action Title）写作规则

每个 Exhibit 的 `action-title` 不是话题标签，而是**包含结论的完整句子**。Executives 应该只读 action titles 就能理解整个故事。

**规则**：
- 15字以内，1-2行
- 包含定量结果（%、金额、倍数）
- 回答 "So What?" ——这一页告诉读者什么结论
- 主动语态

**示例**：

| ❌ 错误（描述性标签） | ✅ 正确（行动标题） |
|------|------|
| "公司概览" | "NAVIS是年GP €9.4M的德国货运代理，核心资产为汉堡自有物业" |
| "历史财务表现" | "GP从FY23峰值€11.1M降至LTM €9.4M，反映疫情后运费正常化" |
| "盈利质量分析" | "经€0.6M调整后，可持续EBIT为€1.4M，调整项主要为推算租金和汇兑损益" |
| "估值分析" | "概率加权EV约€12.3M，估值区间€9-22.5M由终值方法选择主导" |
| "风险评估" | "无Deal Breaker，但养老金税务（€380k）和RETT（€366k）须SPA全覆盖" |

---

## SVG 图表生成规范

所有图表使用内联 `<svg>` 元素。以下定义了4种图表的精确规范。

### 通用规则
- 所有文本使用 `font-family="Arial, Helvetica, sans-serif"`
- 坐标轴颜色 `#8C8C8C`，线宽 1px
- 数据标签直接标注在图形元素上，不使用图例（legend）
- viewBox 宽度统一为 700
- 背景白色 `#FFFFFF`

### 通用坐标计算公式

**重要**：不要手工计算每个像素坐标。按以下公式**数据驱动**生成：

```
给定:
  data[]     = 要绘制的数值数组
  max_value  = max(data) × 1.15 (留15%顶部空间)
  chart_h    = viewBox高度 − top_margin − bottom_margin

y_position(value) = top_margin + chart_h × (1 − value / max_value)
bar_height(value) = chart_h × (value / max_value)

// 柱状图: x = left_margin + i × (bar_width + gap)
// i = 0, 1, 2, ... (数据点索引)
```

**关键坐标参数表**：

| 参数 | 瀑布图 | 趋势柱状图 | Football Field |
|------|--------|-----------|----------------|
| viewBox | `0 0 700 310` | `0 0 700 300` | `0 0 700 200` |
| top_margin | 40 | 30 | 40 |
| bottom_margin | 50 | 50 | 30 |
| chart_h | 220 | 220 | 140 |
| left_margin | 80 | 70 | 100 |
| bar_width | 48 | 14 (单条) | 条高28px |
| gap | 25 (柱间距) | 2 (组内) / 50 (组间) | 10 (条间距) |

**生成前必须**：
1. 列出 data[] 数组（所有要绘制的数值）
2. 计算 max_value = max(data) × 1.15
3. 对每个数据点，用公式计算 x, y, width, height
4. 将计算结果映射为 `<rect>` 标签

---

### Chart 1: EBITDA 调整瀑布图（Waterfall）

**数据源**：Phase 3A EBITDA 调整桥（报告EBITDA → DD调整 → Pro Forma调整 → 调整后EBITDA）

**SVG 绘制逻辑**：

```
1. 计算Y轴范围：从0到max(报告EBITDA, 调整后EBITDA) × 1.2
2. 第一个柱子：报告EBITDA，从0开始，颜色 navy #003A70
3. 中间柱子（每个调整项）：
   - 正值：从上一个累计值顶部向上，颜色 blue #0066A1
   - 负值：从上一个累计值底部向下，颜色 gray #B0B0B0
   - 每个柱子标注调整项简称和金额
4. 最后一个柱子：调整后EBITDA，从0累计到最终值，颜色 gold #C4A35A
5. 虚连接线：从上一个柱子的顶部/底部连到当前柱子的底部/顶部
```

**示例 SVG 结构**（以NAVIS LTM Oct25为例）：

```svg
<svg viewBox="0 0 700 380" xmlns="http://www.w3.org/2000/svg">
  <!-- Y axis -->
  <line x1="80" y1="280" x2="80" y2="40" stroke="#8C8C8C" stroke-width="1"/>
  <!-- X axis -->
  <line x1="80" y1="280" x2="680" y2="280" stroke="#8C8C8C" stroke-width="1"/>
  
  <!-- Column 1: Reported EBIT €2,056k -->
  <rect x="100" y="96" width="55" height="184" fill="#003A70"/>
  <text x="127" y="88" text-anchor="middle" font-size="11" font-weight="bold" fill="#003A70">2,056</text>
  <text x="127" y="300" text-anchor="middle" font-size="10" fill="#4A4A4A">报告EBIT</text>
  
  <!-- Connector line -->
  <line x1="155" y1="96" x2="175" y2="128" stroke="#8C8C8C" stroke-width="1" stroke-dasharray="3,3"/>
  
  <!-- Column 2: DD Adjustments -€206k (going DOWN from 2,056 to 1,850) -->
  <rect x="175" y="96" width="55" height="21" fill="#B0B0B0"/>
  <text x="202" y="88" text-anchor="middle" font-size="10" fill="#B0B0B0">-206</text>
  
  <!-- ... additional columns ... -->
  
  <!-- Final Column: Adjusted EBIT €1,431k -->
  <rect x="580" y="160" width="55" height="120" fill="#C4A35A"/>
  <text x="607" y="152" text-anchor="middle" font-size="11" font-weight="bold" fill="#C4A35A">1,431</text>
  <text x="607" y="300" text-anchor="middle" font-size="10" font-weight="bold" fill="#4A4A4A">调整后EBIT</text>
</svg>
```

**柱子宽度**：每个柱子 55px，间距 25px。X 坐标从 100 开始。

---

### Chart 2: GP 与 EBITDA 趋势柱状图

**数据源**：Phase 2 历史 P&L + Phase 4 预测（FY23, FY24, LTM Oct25, Y1-Y5）

**SVG 绘制逻辑**：

```
1. Y轴：0 到 max(GP) × 1.15
2. X轴：8组柱子（FY23, FY24, LTM, Y1, Y2, Y3, Y4, Y5），用虚线分隔历史和预测
3. 每组两根柱子并排：GP（navy #003A70）+ EBITDA（lighter blue #7FA9C7）
4. 每根柱子顶部标注数值
5. 在FY24和LTM之间画一条垂直虚线，标注"历史 | 预测"
6. X轴标签：年份 + 在Y2-Y5下方加小字"预测"
```

**柱子宽度**：每组30px宽（GP 14px + 间距2px + EBITDA 14px），组间距 50px。

---

### Chart 3: Football Field（估值区间图）

**数据源**：Phase 5.4 三角验证表

**SVG 绘制逻辑**：

```
1. 横向图：Y轴是方法名称，X轴是估值范围
2. X轴范围：从最小值×0.8 到 最大值×1.1
3. 每个方法用一条水平浮条表示：低-高区间用矩形，中点用菱形标记
4. 颜色按方法类型：DCF深色，可比公司浅色
5. 条右端标注 "€X.XM - €X.XM"
6. 条左端标注方法名称
7. 底部标注 "Enterprise Value (€ Million)"
```

**条颜色**：
- DCF 永续增长：navy `#003A70`
- DCF 退出倍数：blue `#0066A1`  
- 可比公司 EV/EBITDA：light blue `#7FA9C7`
- 可比交易：gray `#8C8C8C`

**垂直条高度**：每条28px，间距10px。

---

### Chart 4: 风险矩阵（CSS Grid，非SVG）

**数据源**：Phase 3C 风险登记表

**结构**：3×3 CSS Grid（已在HTML模板中预置 `.risk-matrix` 类）

```
                    发生可能性
                  低        中        高
影响程度   高   [zone-red] [zone-red] [zone-red]
          中   [zone-amber] [zone-amber] [zone-red]
          低   [zone-green] [zone-green] [zone-amber]
```

**风险ID放置规则**：
- 量化敞口 > €300k 且无法缓释 → 影响=高
- 量化敞口 < €100k 或完全可缓释 → 影响=低
- 税务风险 → 发生可能性=中/高（取决于文件完整性）
- 运营风险 → 发生可能性=中（取决于管理层留任）

**渲染**：用 HTML `<div class="risk-matrix">` 结构渲染，每个cell填入该区域的风险ID列表。

---

## SCQA 执行摘要规则

**Situation（情境）**：中性背景陈述。谁在买谁？目标公司做什么？交易规模和结构？

**Complication（复杂性）**：发现了什么？不直接说"风险"，而是用数据说话。"尽调发现GP较FY23峰值下降了16%，且创始人在签约前退休。"

**Question（核心问题）**：一句话。"在这个价格下，这笔交易是否值得推进？需要满足什么条件？"

**Answer（回答）**：结论先行。必须有定量支撑。"有条件推进。概率加权EV约€12.3M（€9-22.5M区间），对应Equity Value约€14M。前提是SPA全覆盖养老金税务风险（€380k）和RETT（€366k），且Steinmeyer留任至少2年。"

---

## 报告生成工作流

### Step 1: 读取模板
读取 `report-template.html`，理解所有 `{{PLACEHOLDER}}` 位置。

### Step 2: 收集数据
从 Phase 2-7 的对话输出中提取所有数值、表格、风险描述、估值结论。

### Step 3: 生成 SVG 图表
按上述 SVG 规范，用实际数据生成4张图表的完整 `<svg>` 代码。

### Step 4: 撰写行动标题
为每个 Exhibit 写一个包含结论的 action title。

### Step 5: 组装 HTML
将所有 `{{PLACEHOLDER}}` 替换为实际内容（包括完整的 SVG 字符串、HTML 表格片段、KPI 卡片片段）。

### Step 6: 写入文件
将完整 HTML 写入 `{{OUTPUT_DIR}}/{{TARGET_NAME}}_DD_Report_{{DATE}}.html`。

默认输出目录为当前工作目录。用户可指定其他路径。

### Step 7: 提示用户
```
"HTML 报告已生成：[文件路径]
您可以在浏览器中打开，或打印为 PDF 存档。
生成 PDF：在浏览器中打开后 → 文件 → 打印 → 另存为 PDF（建议设置：A4、无边距、背景图形开启）"
```

---

## 风格检查清单

生成报告后自检：
- [ ] 每个 Exhibit 的 action title 是完整结论句（非话题标签）
- [ ] 4张图表格式正确、数据准确
- [ ] 所有数字有单位（€k / €M / %）
- [ ] 表格有来源标注（FDD p.X / Tax DD p.X）
- [ ] 无 emoji（专业报告不使用emoji）
- [ ] 颜色使用克制（navy主色+gray辅助+gold仅高亮）
- [ ] 在 `<style>` 标签内，无外部CSS/JS依赖
- [ ] `@media print` 规则完整，打印效果正常

---

> 本文件为 M&A DD Skill 的 Phase 8 报告生成模块。
> 配合 [report-template.html](report-template.html) 和 [PROMPT.md](PROMPT.md) Phase 8 使用。
