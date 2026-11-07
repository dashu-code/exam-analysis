# exam-analysis · 考试逐题深度解析 Skill

一个遵循 SKILL.md 规范的 Agent Skill（ZCode / Claude Code 等均可用）：在用户给定的考试时长内逐题深度解析试卷，用「四遍式流程 + 分学科交叉验证」减少幻觉、提高答案准确率——对没有把握的地方诚实标注，而不是编造。

> An agent skill for exam-question analysis: budgets analysis depth by exam duration, runs a 4-pass anti-hallucination workflow (restate → solve → cross-verify → confidence tagging), and honestly flags uncertainty instead of fabricating answers.

## 特性

- ⏱ **时间门控** — 用户提到考试但没给时长时，主动询问（60 / 90 / 120 分钟 / 不限时），拿到答复才开工，绝不自行假设。
- 📊 **深度分档** — 每题预算 = 总时长 ÷ 题数，自动映射到深度 / 标准 / 精炼三档解析。
- 🔒 **验证铁律** — 时间只调节「解析写多详细」，验证步骤永不省略；优先级：答案正确 > 覆盖全题 > 单题详细度。
- 🔁 **四遍式流程** — 审题（抓否定词 / 陷阱词）→ 独立求解 → 交叉验证 → 置信度标注。
- 📚 **分学科验证库** — 数学 / 物理 / 化学 / 生物 / 英语 / 语文 / 政史地 / 编程题各有针对性校验手段（代回、量纲、排除法、原文引用、真实跑代码等）。
- 🏷 **置信度标注** — 每题标 ✅确定 / ⚠️较确定 / ❗不确定，最后附汇总表与存疑清单。

## 安装

```bash
# 用户级安装（所有项目可用）
git clone https://github.com/dashu-code/exam-analysis.git ~/.agents/skills/exam-analysis

# 或仅装入当前项目
git clone https://github.com/dashu-code/exam-analysis.git .agents/skills/exam-analysis
```

重启会话后生效。Agent 会按技能描述自动触发，也可用 `/exam-analysis` 手动调用。

## 用法示例

```text
帮我分析这套数学卷，考试是120分钟   → 已给时长，直接定档开始解析
我明天有一场考试，帮我看看这套真题  → 未给时长，先询问考试时长再开工
阅读理解做一下这篇，别问太多了      → 用户拒绝追问时，按最大深度兜底
```

## 防幻觉机制

| 常见错误 | 对策 |
|---|---|
| 审题答反（"不正确的是"） | Pass 1 强制圈出否定词与陷阱词、单位与隐含条件 |
| 凭感觉选选项 | 选择题逐项给出排除理由，说不出错因就主动降低置信度 |
| 计算错误 | 代回原式、量纲 / 极限 / 守恒律检验；两种解法冲突即回炉重解 |
| 阅读题过度发挥 | 答案必须引用原文依据句，找不到即判可疑 |
| 事实类编造（年份 / 法条 / 数据） | 拿不准一律标 ❗，可联网时先查证再作答 |
| 漏题 / 凭空多题 | 上传试卷先报告题量构成，请用户确认无遗漏后再开始 |

## 输出格式（节选）

```markdown
### 第3题 ✅确定
**答案**：B
**解析**：……
**排除项**：A——…；C——…；D——…
**考点**：二次函数顶点式

| 题号 | 答案 | 置信度 |
|---|---|---|
| 1 | C | ✅ |
| 2 | 见解析 | ❗ |

❗ 存疑清单（建议优先人工复核）：……
```

## 目录结构

```
exam-analysis/
├── SKILL.md                 # 主流程：时间门控 → 深度分档 → 四遍式流程 → 输出格式
├── references/
│   └── verification.md      # 分学科验证技巧库（按需加载）
├── README.md
└── LICENSE
```

## License

[MIT](LICENSE) © dashu-code
