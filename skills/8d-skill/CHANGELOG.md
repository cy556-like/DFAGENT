# CHANGELOG

## 2.0.1 (2026-06-23)

### 修复：行业常识基准硬约束（解决"无常识示例"问题）

**问题背景**：v2.0.0 中所有不良率示例值（11.5% / 8% / 5% / 3%）与批次数量示例值（500）严重脱离汽车零部件行业实际，导致 Agent 在 AskUserQuestion 追问时模仿这些数字、生成"5.2% 不良率 + 2000 根批次"这类违反常识的提示。

**修复内容**：
1. **SKILL.md**：
   - defect_rate 默认示例 11.5% → **500 PPM (0.05%)**
   - batch_size 默认示例 500 → **12 件**（语义澄清为"8D 分析样本数"，非生产批量）
   - 调用示例 8% → 200 PPM (0.02%)
   - 新增第十章「行业常识基准（🔴 严禁违反）」，含 PPM 量级表、batch_size 分类表、合规/违规示例对照
   - 第八章新增第 8 条「行业常识优先」硬约束
2. **references/5why_examples.md**：5 个范例的不良率全部改为 PPM 级（50/100/200/500 PPM），并补充"退货件数/出货总量"的折算说明
3. **references/fishbone_guide.md**：漆面颗粒示例不良率 11.5% → 500 PPM
4. **references/8d_guide.md**：「不良率11.5%」→「不良率 500 PPM，本批次退货 15 件」
5. **scripts/generate_8d.py**：文档头示例参数 11.5% / 500 → 500PPM / 15，新增「⚠️ 行业常识基准」说明段
6. **app/agent/prompts.py**：8D 硬约束新增第 5 条「行业常识基准」，明确禁止 3%/5%/5.2%/8%/11.5% 与 500/2000/5000 作为示例值，并要求 batch_size 概念混淆时主动追问澄清

**预期效果**：Agent 在追问用户不良率/批次数量时，给出的示例值将落在 IATF 16949 行业常识区间内（PPM 级不良率 + 个位数到几十件的分析样本），不再出现"5.2% + 2000根"这类灾难级数字。

---

## 2.0.0 (2026-06-21)

### 重大升级：从模板框架到可用的 8D 报告生成器

**template.json v2.0**
- 5Why: hints(候选列表) -> steps(完整连贯预填路径，含答案和证据)
- 6M: 纯文本 -> 数组格式 + judgment字段(根本原因/贡献因子/排除/待确认)
- 新增 root_cause_summary: 预填RC1/RC2
- 新增 verification: 预填验证结论(含具体数据)
- 5套模板全部重写

**generate_8d.py 视觉升级**
- 章节标题: 深蓝底 -> 黄色底+深蓝字, 视觉分层
- Sheet: 单 Sheet，标签颜色 003366 深蓝
- D4生成器: 兼容新旧template格式
- Word同步更新

**SKILL.md**
- 新增第5条价值主张: 真实行业参考资料
- 新增v2.0 template.json数据结构文档
- 新增web_fetch知识增强搜索指引
- 新增YAML frontmatter(name+description)

**references/**
- 基于Quality-One 8D方法论补充真实行业实践内容

---

## 1.0.0 (2026-06-20)

### 初始版本
- 完整目录结构: SKILL.md + templates/ + references/ + scripts/
- 5套行业模板
- scripts/generate_8d.py (xlsx+docx双输出)
- references/: 8d_guide.md / 5why_examples.md / fishbone_guide.md
