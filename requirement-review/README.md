# 需求评审 Skill 系列

这个系列不是"7个功能相近的需求评审工具"，是同一件事的**7种不同做法**——有的是不同场景的专项工具，有的是同一组 Prompt 技巧的递进实验。先看下面这张表，找到适合你的那个，不用每个都试。

---

## 先看这张表：我该用哪个？

| 你的情况 | 推荐 Skill |
|---|---|
| 手头有份需求，想要一次性、全面地过一遍，日常首选 | [`qa-advisor/`](./qa-advisor/) |
| 需求是交互类的，只想让 AI 帮你列出要确认的问题，不要它替你下结论 | [`socratic-prd-review/`](./socratic-prd-review/) |
| 需求是交互类的，想让 AI 直接指出遗漏点和建议 | [`prd-interaction-detail-review/`](./prd-interaction-detail-review/) |
| 想看产品/测试/后端/前端/运营 5 个角色分别怎么挑刺 | [`multi-role-prd-review/`](./multi-role-prd-review/) |
| 想让 AI 一轮只专注一个维度（完整性→一致性→可测试性），别囫囵扫一遍 | [`requirements-stepwise-review/`](./requirements-stepwise-review/) |
| 上面两个都想要，追求覆盖最全、问题密度最高 | [`product-multi-role-stepwise-requirement-review/`](./product-multi-role-stepwise-requirement-review/) |
| 有支持多 Agent 并行的工具，想要更快、上下文更干净 | [`multi-agent-parallel-review/`](./multi-agent-parallel-review/) |

---

## 分组说明

### 核心评审

**[`qa-advisor/`](./qa-advisor/)** —— QA 智囊团。将质量团队高阶管理岗、技术专家、核心成员的判断风格蒸馏为可调用角色，在决策前预演多个视角的判断，发现盲点，聚合共识，识别分歧。这是日常最推荐的默认选项。

### 交互类专项

针对交互类需求单独设计，用同一份真实 PRD（车辆管理模块，8 个功能点）验证过，5 条历史评审批注全部命中，额外发现 4 类新问题。

- **[`socratic-prd-review/`](./socratic-prd-review/)** —— 苏格拉底追问型。只提问、不给结论，以 10 年 B 端测试经验 QA 工程师视角逐条阅读 PRD 功能点，找出"被默认但未明确"的假设，用追问方式输出问题并附测试重要性说明。
- **[`prd-interaction-detail-review/`](./prd-interaction-detail-review/)** —— 交互细节挖掘型。以专注交互逻辑测试的 QA 工程师视角，从 7 个维度（弹窗行为、下拉联动、操作反馈、异步状态感知、限制条件执行方式、只读与可编辑边界、字段业务语义边界）逐维度扫描 PRD，识别未明确描述的交互细节遗漏并给出建议补充方向。

两者不是谁更好，是要不要 AI 替你下结论的选择题。

### 构造技巧对比

这四个不是四个独立场景的工具，是**同一组 Prompt 技巧的递进实验**，用同一份购物返现网站需求文档做过对比测试，问题数量摆在这：

| 技巧 | 问题数 | 对应 Skill |
|---|---|---|
| 单角色（基线，不在本系列中） | 28 | — |
| 多角色，一次输出 | 47 | [`multi-role-prd-review/`](./multi-role-prd-review/) |
| 分轮次（完整性→一致性→可测试性），不区分角色 | 52 | [`requirements-stepwise-review/`](./requirements-stepwise-review/) |
| 外层轮次 × 内层角色叠加，密度最高，耗时也最长 | 最高（具体数值待补） | [`product-multi-role-stepwise-requirement-review/`](./product-multi-role-stepwise-requirement-review/) |
| 上一版的并行版本，3 个子 Agent 分别跑完整性/一致性/可测试性，各自内部 5 角色串行，主 Agent 汇总 | 同上 | [`multi-agent-parallel-review/`](./multi-agent-parallel-review/) |

**`multi-agent-parallel-review/` 有一点必须说清楚**：它依赖宿主工具是否真的支持多 Agent 并行执行，实测中大模型有时候并不会真的开出独立子 Agent，需要反复求证才能确认。这不是稳定可靠的默认选项，是有对应工具支持时的进阶用法，效果因宿主而异。

想深入评审重要需求，从上到下选，越往下越全但越慢；日常需求，`qa-advisor` 或 `multi-role-prd-review` 通常够用。

---

## 怎么用

1. 找到对应文件夹，复制到你的 Agent 宿主的 skills 目录（`.claude/skills/`、`.agents/skills/` 等）
2. 按需求场景触发，或把需求文档直接发给 Agent 调用

---

## 能力边界

这些 Skill 擅长什么、不擅长什么，是诚实说清楚的：

**擅长（可以放心用）**
- 完整性缺失：验收标准、异常流程、非功能需求的遗漏
- 逻辑矛盾：跨章节、跨段落的表述冲突
- 同类需求的经验匹配（知识库积累后会持续变强）

**不擅长（人必须主导）**
- 强业务判断类问题（该不该做、优先级对不对）
- 依赖团队潜规则的隐性风险
- 创新型 / 探索型需求（没有评审基准）
- 需要跨系统深度理解的逻辑矛盾
- 用户体验和产品方向的判断

**中间地带（建知识库后能改善，但有成本）**
- 复杂业务背景类需求
- 有合规标准要求的需求
- 有公司内部规范的需求

---

## 延伸阅读

完整的方法论演进过程（从一句话 MVP，到 Prompt 技巧，到知识库，到人格蒸馏，到评测和版本管理）：[讲透需求评审 Skill 系列合集](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MjM5NjA5MzIzNw==&action=getalbum&album_id=4596402718117953545&scene=21#wechat_redirect)