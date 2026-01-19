# skillforge · AI Coding 指导性架构与实践指南

> 本文档用于指导 `skillforge` 项目的整体设计、能力边界划分与 AI Coding 实践方式。
> 目标是：**避免过早绑定单一 AI 能力，构建一个长期可扩展的「Skill 工厂」体系。**

---

## 1. 项目核心目标（One-sentence Goal）

**将一次真实发生的编程问题解决过程，自动蒸馏为可复用、可版本化、可调用的 Claude Code Skill。**

关键词：

* workflow distillation
* skill as artifact
* meta-tooling

---

## 2. 一个重要的反模式（Anti-pattern）

### ❌ 不要这样做

> **把 skillforge 本身完整地实现为一个 Claude Code Skill**

原因：

* Claude Skill 是「行为模板」，不是「系统工具」
* 缺乏稳定的结构输出能力
* 难以处理文件系统、版本控制、后处理
* 会反向锁死项目的扩展性

**结论**：
Claude Skill ≠ Skill 编译器 ≠ 工作流引擎

---

## 3. 正确的总体架构（三层模型）

skillforge 采用 **“引擎 + 认知前端 + 产物”** 的三层架构。

---

### 🧱 Layer 1：核心引擎（Skillforge Core）

**形式**

* Python 脚本 / Library
* 可 CLI
* 可测试
* 可扩展

**职责**

* 解析原始 session / distilled workflow
* 映射为标准 Skill Schema
* 生成最终 Skill 文件
* 管理输出路径与命名

**这一层是：**

> 项目的灵魂
> 唯一的事实来源（single source of truth）

---

### 🧠 Layer 2：Claude Code Skill（认知前端）

**形式**

* 一个很薄的 `.claude/skills/skillforge.md`

**职责（严格限制）**

* 引导 Claude 进行：

  * intent 抽取
  * 步骤归纳
  * 约束总结
* 输出 **中间结构（distilled workflow）**
* 不负责文件生成
* 不负责版本控制

**定位一句话**

> Claude 在这里是“理解器”，不是“编译器”

---

### 📦 Layer 3：生成的 Skills（最终产物）

**形式**

* `.claude/skills/*.md`
* YAML / Markdown

**特点**

* 稳定
* 可读
* 可 diff
* 可人工修改
* 可直接调用

---

## 4. skillforge 的推荐使用方式（标准工作流）

### Step 1：自然发生的问题解决

* 用户完成一次真实的编程任务
* 产生 chat / terminal / notes / diff 等痕迹

---

### Step 2：Claude Skill 进行“认知蒸馏”

用户调用：

```
/skillforge distill this session
```

Claude 输出：

```yaml
intent:
inputs:
procedure:
constraints:
output:
```

⚠️ 注意：

* 此时**不生成最终 skill**
* 只生成「认知层结构」

---

### Step 3：核心引擎生成 Skill

```bash
python generate_skill.py distilled.yaml
```

输出：

```
.claude/skills/fix_python_import_error.md
```

---

## 5. Skill 的最小可用抽象（MVP Schema）

skillforge 只承认以下字段是 **“核心不可删除字段”**：

```yaml
name:
intent:
input:
procedure:
constraints:
output:
```

### 设计原则

* intent 决定 *何时触发*
* procedure 决定 *如何执行*
* constraints 防止 hallucination
* output 定义 *成功标准*

任何扩展字段，必须：

* 不破坏上述结构
* 不影响人类可读性

---

## 6. Claude 与 Python 的能力分工（红线）

### Claude 负责（必须）

* 自然语言理解
* 隐含意图总结
* 决策过程抽象
* 非结构化 → 半结构化

### Python 负责（必须）

* Schema 校验
* 命名与路径规则
* 文件生成
* 格式稳定性
* 自动化与测试

👉 **如果某件事 Claude 能做但不稳定，一定交给 Python**

---

## 7. v0.1 能力边界（刻意克制）

skillforge v0.1 **不做**：

* 自动调用 Git
* 自动分析 IDE 状态
* 多 session 合并
* 自我进化 / 自我修改 skill

skillforge v0.1 **只做一件事**：

> **把“刚刚解决完的一次问题”变成一个干净、可用的 skill**

---

## 8. 对 AI Coding 的核心指导原则

### 原则 1：先冻结认知，再追求自动化

> 能被描述清楚的流程，才值得被自动化。

### 原则 2：所有 AI 输出，必须能被 diff

> 如果不能 diff，就不能信任。

### 原则 3：Prompt ≠ 架构

> Prompt 是实现细节，架构是长期约束。

### 原则 4：技能是“经验的最小可复用单位”

> 不是代码，不是脚本，而是**决策模式**。

---

## 9. 未来演进方向（不在 v0.1 实现）

* Skill 质量评分（usefulness / reuse）
* Skill 合并与去重
* 自动生成 README / 文档
* 多模型协作（Claude + GPT + 本地模型）
* Skill → Agent 的跃迁

---

## 10. 一句写给未来自己的话

> **skillforge 不是在生成 skill
> 而是在对抗“经验的流失”**

如果你发现自己在堆 prompt、堆规则、堆魔法，
请回到这里，重新读一遍这份文档。
