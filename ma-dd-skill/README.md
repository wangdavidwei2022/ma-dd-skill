# M&A 财务尽调 AI Skill

一套覆盖 M&A 财务尽调全流程的 AI 分析框架。**估值环节采用达摩达兰（Aswath Damodaran, NYU Stern）的估值思维结构** — 不是机械套公式，而是"故事→数字→故事"的专业判断框架。

## 核心能力

| 能力 | 说明 |
|------|------|
| **报告解读与风险提取** | 从 FDD/Tax DD 报告中提取结构化风险清单、EBITDA 调整桥、净负债分析、红旗警示 |
| **财务建模** | 基于尽调数据搭建 5 年三表预测（P&L + BS + CF），含三情景分析 |
| **估值分析（达摩达兰驱动）** | DCF + EBITDA 倍数双重估值，含敏感性分析、估值三角验证、情景概率加权 |
| **税务尽调风险梳理** | 税务风险登记表、税务属性分析、交易结构影响、SPA 保护建议 |
| **投资备忘录草稿** | 完整的董事会级投资备忘录，含执行摘要、投资逻辑、风险评估、谈判策略 |
| **HTML 报告生成** | 整合所有分析产出为麦肯锡风格 HTML 报告（含 4 张 SVG 图表） |
| **达莫达兰归纳附录** | 完整的第一人称 7 维度估值哲学论述 |

## 部署方式

### Claude Projects（推荐）

1. 打开 claude.ai，创建新 Project
2. 将 `PROMPT.md` 的**完整内容**粘贴到 Project instructions（系统指令）
3. 将 `damodaran-distill.md`、`valuation-templates.md`、`output-templates.md`、`mckinsey-report.md`、`report-template.html` 上传到 Project knowledge
4. 保存，开启新对话

### ChatGPT GPTs

1. 创建新 GPT
2. 将 `PROMPT.md` 的完整内容粘贴为 Instructions
3. 将其他 5 个文件上传为 Knowledge 文件
4. 保存并发布

## 对话流程

共 8 个 Phase（0-8），AI 按顺序引导：

```
Phase 0: 语言确认
    ↓
Phase 1: 交易背景（一次性回答 7 个问题）
    ↓
Phase 2: 粘贴报告内容 / 财务数据
    ↓
Phase 3: AI 输出风险清单 + 红旗警示
    ↓
Phase 4: AI 输出 5 年财务模型
    ↓
Phase 5: AI 以达摩达兰框架做估值分析 ⭐
    ↓
Phase 6: 税务风险摘要
    ↓
Phase 7: 投资备忘录草稿
    ↓
Phase 8: McKinsey 风格 HTML 报告生成
```

可随时跳过不需要的 Phase，直达核心需求。

## 触发词

审阅尽调报告、财务尽调、税务尽调、估值、搭建财务模型、M&A 分析、DCF、收购估值、投资备忘录

## 达摩达兰估值模式

本 Skill 的 Phase 5 与传统"输入参数、出结果"的工具有本质不同：

| 传统工具 | 本 Skill |
|---------|---------|
| 直接让你填 WACC、增长率 | 先让你讲公司故事，再讨论参数 |
| 给你一个数字 | 给你区间 + 情景概率 + 概率加权结果 |
| "假设永续增长 3%" | "这个 g 能超过经济增速吗？" |
| "行业倍数 10x 就用这个" | "10x 隐含了什么增长率和 ROIC？和 DCF 一致吗？" |
| 终值占 85% 也不提醒 | 终值 >70% 立刻标黄警告 |

## 文件结构

```
ma-dd-skill/
  README.md
  PROMPT.md                  # 核心 AI 指令
  damodaran-distill.md       # 达摩达兰估值大师人物蒸馏
  valuation-templates.md     # 估值计算模板
  output-templates.md        # 输出格式模板
  mckinsey-report.md         # Phase 8 报告组装规范
  report-template.html       # HTML 报告骨架
  GUIDE.md                   # 用户指南
  EXAMPLE.md                 # 示例对话流程
```

## 适用人群

- CFO / 财务总监
- 投资总监 / M&A 负责人
- 企业战略/战投团队
- PE/VC 投资团队

## 局限性

- AI 输出需要专业人士审阅，不是最终决策依据
- 不构成法律或税务意见
- 不替代会计师事务所、律师事务所、FA 的专业服务

## 许可

MIT
