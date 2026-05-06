# Tax Planning HTML Report Generator

本文件定义如何将税务筹划Skill各Phase的分析产出，组装为一份专业的HTML报告。

---

## 触发条件

Phase 4自动触发。用户也可以在Phase 2或Phase 3任意阶段说「生成报告」「出报告」「输出HTML报告」提前生成（使用当前已有数据，缺失section标注"待补充"）。

---

## 报告结构映射

HTML模板文件：[tax-report-template.html](tax-report-template.html)。生成时读取该模板，将所有 `{{PLACEHOLDER}}` 替换为实际内容。

| 模板 Placeholder | 数据来源 | 说明 |
|------|---------|------|
| `{{REPORT_TITLE}}` | Phase 0用户输入 | "[公司名称] 税务筹划分析报告" |
| `{{CONFIDENTIAL_LABEL}}` | 固定 | "机密 · 仅供内部决策使用" |
| `{{COVER_TITLE}}` | Phase 0 | "[公司名称] 税务筹划分析报告" |
| `{{COVER_SUBTITLE}}` | Phase 1C | "[行业] · 集团架构优化与风险防控" |
| `{{COVER_DATE}}` | 当前日期 | "202X年X月X日" |
| `{{COVER_PREPARED_FOR}}` | Phase 0/1 | "为 [公司/决策者] 编制" |
| `{{COVER_PREPARED_BY}}` | 固定 | "税务筹划分析 · AI辅助编制" |
| `{{EXEC_ANSWER}}` | Phase 2 TOP1 + Phase 3 | 一句话核心结论（行动标题） |
| `{{EXEC_SITUATION}}` | Phase 0/1 | 集团架构简述 + 当前税务画像，2-3句话 |
| `{{EXEC_COMPLICATION}}` | Phase 1 全诊断 | 核心发现/风险，2-3句话 |
| `{{EXEC_QUESTION}}` | Phase 1C 小结 | "如何优化集团税务架构，降低税负并防控风险？" |
| `{{EXEC_ANSWER_DETAIL}}` | Phase 2 TOP3 + Phase 3 | 建议 + 量化节税，2-3句话 |
| `{{KPI_CARDS}}` | Phase 1 + Phase 2 | 5个KPI卡片 |
| `{{E1_ACTION_TITLE}}` | Phase 1C | 行动标题 |
| `{{E1_ORG_CHART}}` | Phase 1C | 集团架构图（文本pre格式） |
| `{{E1_TAX_PROFILE_TABLE}}` | Phase 1C | 各主体税率/ETR对比表 |
| `{{E2_ACTION_TITLE}}` | Phase 1B | 行动标题 |
| `{{E2_INDUSTRY_TABLE}}` | Phase 1B | 优惠政策检查 + 风险检查 |
| `{{E2_ETR_CHART}}` | Chart 1 | 辖区ETR对比柱状图SVG |
| `{{E3_ACTION_TITLE}}` | Phase 1A | 行动标题 |
| `{{E3_GLOBE_TABLE}}` | Phase 1A | GloBE判定 + ETR计算 |
| `{{E3_GLOBE_CONCLUSION}}` | Phase 1A | GloBE结论文字 |
| `{{E4_ACTION_TITLE}}` | Phase 1C | 行动标题 |
| `{{E4_PROFIT_VS_SUBSTANCE_CHART}}` | Chart 2 | 利润vs实质对比SVG |
| `{{E4_IP_TABLE}}` | Phase 1C-2 | IP配置匹配表 |
| `{{E4_RPT_TABLE}}` | Phase 1C-3 | 关联交易风险表 |
| `{{E4_SHAREHOLDER_TAX_TABLE}}` | Phase 1C-5 | 股东层面税负 |
| `{{E5_ACTION_TITLE}}` | Phase 1C-3 | 行动标题 |
| `{{E5_TP_SCORING_TABLE}}` | Phase 1C-3 | 转让定价风险评分矩阵 |
| `{{E5_TP_RISK_CHART}}` | Chart 3 | TP风险维度柱状图SVG |
| `{{E5_TP_DOCUMENTATION_STATUS}}` | Phase 1C-3 | 同期资料合规状态 |
| `{{E6_ACTION_TITLE}}` | Phase 3 交付物3 | 行动标题 |
| `{{E6_RISK_MATRIX}}` | Phase 3 交付物3 | 3×3风险矩阵 HTML |
| `{{E6_RISK_BY_DOMAIN_TABLE}}` | Phase 3 交付物3 | 按领域分布表 |
| `{{E6_URGENCY_TIMELINE}}` | Phase 3 交付物3 | 紧急事项时间线 |
| `{{E7_ACTION_TITLE}}` | Phase 2 排序结果 | 行动标题 |
| `{{E7_PRIORITY_TABLE}}` | Phase 2 | 优先级排序表（含分数） |
| `{{E7_TOP3_CARDS}}` | Phase 2 | 三个方案的详细HTML卡片 |
| `{{E7_WATERFALL_CHART}}` | Chart 4 | 税负优化瀑布图SVG |
| `{{E8_ACTION_TITLE}}` | Phase 3 交付物1 | 行动标题 |
| `{{E8_TIMELINE}}` | Phase 2 + Phase 3 | 近/中/长期实施时间线HTML |
| `{{E8_PREREQUISITES}}` | Phase 2 TOP3 | 前置条件清单 |
| `{{E8_EXTERNAL_ADVISORS}}` | Phase 3 | 需外部顾问支持事项 |
| `{{APP_ALL_STRATEGIES}}` | Phase 2 全部策略 | 全部优化策略详情 |
| `{{APP_ENTITY_PROFILES}}` | Phase 1C | 各主体税务档案 |
| `{{APP_CFC_ANALYSIS}}` | Phase 1C-5 | CFC/7号公告/退出税分析 |
| `{{APP_DISCLAIMER}}` | 固定 | 来源与免责声明 |
| `{{REPORT_REF}}` | Phase 0 | 项目标识 |

---

## 行动标题（Action Title）写作规则

每个Exhibit的`action-title`不是话题标签，而是**包含结论的完整句子**。决策者应只读action titles就能理解整个故事。

**规则**：
- 20字以内，1-2行
- 包含定量结果（%、金额、评分）
- 回答"So What?"——这一页告诉读者什么结论
- 主动语态

**示例**：

| ❌ 错误（描述性标签） | ✅ 正确（行动标题） |
|------|------|
| "集团架构与税务画像" | "集团ETR 16.1%，利润70%留在香港2人公司，架构与实质严重错配" |
| "行业税务诊断" | "汽车零部件制造享高新15%税率仅剩1年，续期不确定将年增税1200万" |
| "GloBE评估" | "集团收入4亿元，远低GloBE 7.5亿欧元门槛，全球最低税暂不适用" |
| "转让定价风险评估" | "TP风险综合评分3.2/5.0，关联采购低15%且无同期资料，需3个月内补建文档" |
| "优化建议" | "转让定价合规重建为TOP1优先项，年化节税潜力逾千万，3个月内可启动" |

---

## SVG 图表生成规范

所有图表使用内联 `<svg>` 元素。以下定义5种图表的精确规范。

### 通用规则
- 所有文本 `font-family="Arial, Helvetica, sans-serif"`
- 坐标轴颜色 `#8C8C8C`，线宽1px
- 数据标签直接标注在图形元素上
- viewBox宽度统一为700
- 背景白色 `#FFFFFF`

### 通用坐标计算公式

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

| 参数 | Chart 1 (ETR对比) | Chart 2 (利润vs实质) | Chart 3 (TP风险) | Chart 4 (瀑布图) |
|------|-------------------|---------------------|-------------------|-------------------|
| viewBox | `0 0 700 320` | `0 0 700 300` | `0 0 700 280` | `0 0 700 360` |
| top_margin | 40 | 30 | 40 | 40 |
| bottom_margin | 60 | 50 | 50 | 60 |
| chart_h | 220 | 220 | 190 | 260 |
| left_margin | 80 | 70 | 80 | 80 |
| bar_width | 24 (每组双柱) | 24 (每组双柱) | 48 (单柱) | 55 |
| gap | 60 (组间) | 60 (组间) | 50 | 20 |

---

### Chart 1: 辖区ETR对比柱状图

**数据源**：Phase 1C 各主体税率/ETR对比表

**SVG绘制逻辑**：

```
1. Y轴：0 到 max(法定税率, ETR) × 1.2
2. X轴：每个辖区一组柱子（苏州、香港、BVI等）
3. 每组两根柱子并排：法定税率（navy #003A70）+ 实际ETR（blue #0066A1）
4. GloBE 15%参考线：红色虚线（stroke="#C0392B" stroke-dasharray="6,4"）
5. 每根柱子顶部标注税率数值（如"15%"）
6. X轴标签：辖区名称
7. 图例：右上角"■ 法定税率 ■ 实际ETR --- GloBE 15%"
```

**示例SVG结构**：

```svg
<svg viewBox="0 0 700 320" xmlns="http://www.w3.org/2000/svg">
  <!-- Y axis -->
  <line x1="80" y1="260" x2="80" y2="40" stroke="#8C8C8C" stroke-width="1"/>
  <!-- X axis -->
  <line x1="80" y1="260" x2="660" y2="260" stroke="#8C8C8C" stroke-width="1"/>
  
  <!-- GloBE 15% reference line -->
  <line x1="80" y1="150" x2="660" y2="150" stroke="#C0392B" stroke-width="1" stroke-dasharray="6,4"/>
  <text x="665" y="154" font-size="9" fill="#C0392B">GloBE 15%</text>
  
  <!-- Group: 苏州 -->
  <!-- Statutory rate bar: 25% -->
  <rect x="140" y="70" width="24" height="190" fill="#003A70"/>
  <text x="152" y="62" text-anchor="middle" font-size="10" font-weight="bold" fill="#003A70">25%</text>
  <!-- Actual ETR bar: ~12% (assuming 15% * 80% due to incentives) -->
  <rect x="168" y="170" width="24" height="90" fill="#0066A1"/>
  <text x="180" y="162" text-anchor="middle" font-size="10" font-weight="bold" fill="#0066A1">12%</text>
  <text x="159" y="280" text-anchor="middle" font-size="10" fill="#4A4A4A">苏州</text>
  
  <!-- ... repeat for each jurisdiction ... -->
</svg>
```

---

### Chart 2: 利润 vs 实质对比图

**数据源**：Phase 1C-1 利润布局表（利润占比% + 员工占比%）

**SVG绘制逻辑**：

```
1. Y轴：0% 到 100%
2. X轴：每个主体一组柱子
3. 每组两根柱子并排：利润占比%（navy #003A70）+ 员工占比%（gold #C4A35A）
4. 利润>员工的主体的柱子顶部加 ⚠ 标记
5. Y轴刻度：0%, 20%, 40%, 60%, 80%, 100%
```

---

### Chart 3: 转让定价风险维度柱状图

**数据源**：Phase 1C-3 转让定价风险定价矩阵

**SVG绘制逻辑**：

```
1. 5根柱子对应5个风险因素
2. Y轴：0到5（最大评分）
3. 每根柱子颜色按评分：
   - 1-2分：green #27AE60
   - 2.5-3.5分：amber #E67E22
   - 4-5分：red #C0392B
4. 柱子底部标注因素名称（可旋转或缩写）
5. 柱子顶部标注评分
6. 水平虚线标注加权综合分
```

---

### Chart 4: 税负优化瀑布图

**数据源**：Phase 2 优先级排序结果 + ETR估算

**SVG绘制逻辑**：

```
1. 第一个柱子：当前集团ETR（navy），从0开始
2. 中间柱子（每项优化措施）：向上（降低ETR=正向贡献）用green，向下用gray
3. 最后一个柱子：目标ETR（gold），从0开始
4. 虚线连接上一个柱子的顶部到底部
5. 每个柱子标注：措施简称 + ETR变化量
6. X轴标签简短
```

---

### Chart 5: 优化方案气泡图（可选）

当有≥4个方案且有足够数据时使用，否则用表格替代。

**SVG绘制逻辑**：

```
1. X轴：实施难度（1-5）
2. Y轴：年化节税（万元）
3. 气泡大小：紧迫性评分（1-5），半径 = score × 8
4. 颜色：TOP3用gold，其余用blue
5. 虚线分隔三个区域："立即做"/"计划做"/"观望"
```

---

## KPI卡片生成规范

执行摘要页展示5个KPI卡片：

| KPI | 数据来源 | 格式 | 颜色规则 |
|-----|---------|------|---------|
| 集团综合ETR | Phase 1C-1 | "16.1%" | >20% amber, >25% red |
| 年化税损 | Phase 3 交付物2 | "≈1200万/年" | 金额 |
| 潜在节税空间 | Phase 2 排序 | "≈1500万+" | green |
| TP风险评分 | Phase 1C-3 | "3.2/5.0" | >3 amber, >4 red |
| GloBE状态 | Phase 1A | "不适用" 或 "适用" | 不适用=green, 接近=amber, 适用=red |

KPI卡片HTML格式（5列，复用现有`.kpi-row`类）：

```html
<div class="kpi-row" style="grid-template-columns: repeat(5, 1fr);">
  <div class="kpi-card">
    <div class="kpi-value" style="color:var(--blue);">16.1%</div>
    <div class="kpi-label">集团综合ETR</div>
  </div>
  <!-- ... 4 more cards ... -->
</div>
```

---

## SCQA执行摘要规则

**Situation（情境）**：中性背景陈述。集团架构是什么？多少主体？各在哪？什么行业？当前ETR多少？

**Complication（复杂性）**：核心发现。不直接说"有问题"，而是用数据说话。"利润70%留在香港2人公司，IP法律所有权在香港但研发100%在苏州，转让定价文档为零，高新技术企业资质明年到期。"

**Question（核心问题）**：一句话。"集团税务架构如何优化？存在哪些优先要解决的风险？"

**Answer（回答）**：结论先行，有定量支撑。"优先完成转让定价合规重建（避免≈1500万稽查敞口）、保卫高新15%税率（年化节税≈1200万）、并在上市前18个月启动架构税务路线图规划。三项合计可优化年化税负逾2000万。"

---

## 风险矩阵（CSS Grid）生成规则

**数据源**：Phase 3交付物3风险仪表盘

**结构**：3×3 CSS Grid

```
                    发生可能性
                  低        中        高
影响程度   高   [zone-red] [zone-red] [zone-red]
          中   [zone-amber] [zone-amber] [zone-red]
          低   [zone-green] [zone-green] [zone-amber]
```

**风险ID放置规则**：
- 量化敞口 >500万 且无法缓释 → 影响=高
- 量化敞口 >100万 或在稽查中 → 发生可能性=中/高
- 量化敞口 <100万 或可完全缓释 → 影响=低
- CFC/7号公告类风险 → 发生可能性=中，影响=高
- 高新到期 → 发生可能性取决于续期把握

**渲染**：用HTML `<div class="risk-matrix">` 结构，每个cell填入该区域的风险ID列表。

---

## 报告生成工作流

### Step 1: 读取模板
读取 `tax-report-template.html`，理解所有 `{{PLACEHOLDER}}` 位置。

### Step 2: 收集数据
从本次对话的Phase 0-3输出中提取所有数值、表格、风险描述、优化结论。

### Step 3: 生成SVG图表
按上述SVG规范，用实际数据生成5张图表的完整 `<svg>` 代码。

### Step 4: 撰写行动标题
为每个Exhibit写一个包含结论的action title（≤20字，含定量结果）。

### Step 5: 组装HTML
将所有 `{{PLACEHOLDER}}` 替换为实际内容（包括完整的SVG字符串、HTML表格片段、KPI卡片片段）。

### Step 6: 生成风险矩阵HTML
根据风险评分规则，为每个风险ID分配矩阵cell，生成完整的 `<div class="risk-matrix">` 结构。

### Step 7: 写入文件
将完整HTML写入 `[Company]_Tax_Planning_Report_[YYYYMMDD].html`。

默认输出目录为当前工作目录。用户可指定其他路径。

### Step 8: 提示用户
```
"HTML 报告已生成：[文件路径]
您可以在浏览器中打开，或打印为PDF存档。
生成PDF：浏览器中打开后 → 文件 → 打印 → 另存为PDF（建议设置：A4、无边距、背景图形开启）"
```

---

## 风格检查清单

生成报告后自检：
- [ ] 每个Exhibit的action title是完整结论句（非话题标签）
- [ ] 5张图表格式正确、数据准确
- [ ] 所有数字有单位（万/%/分）
- [ ] 表格有来源标注
- [ ] 无emoji（专业报告不使用emoji，除Chart 2的⚠）
- [ ] 颜色使用克制（navy主色+gray辅助+gold仅高亮）
- [ ] 在 `<style>` 标签内，无外部CSS/JS依赖
- [ ] `@media print` 规则完整，打印效果正常
- [ ] KPI卡片5列布局在A4宽度内不换行
