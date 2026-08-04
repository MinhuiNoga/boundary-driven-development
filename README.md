# Boundary-Driven Development for Codex

[繁體中文](#繁體中文) | [English](#english)

一個可跨 repository 使用的 Codex Skill，將 design-first、boundary-driven、contract-first、risk-based review 與 ticket-driven implementation 整合成可重複的開發流程。

A reusable Codex Skill that combines design-first, boundary-driven, contract-first, risk-based review, and ticket-driven implementation into a repeatable development workflow.

## 繁體中文

### 核心目的

在實作前先明確定義 requirement scope、component responsibilities、data ownership、dependency boundaries、public contracts、state transitions、dataflow、failure paths、acceptance criteria 與 test strategy，降低未聲明的架構決策、contract drift、跨範圍修改及難以審查的大型 PR。

### 模式

- **Design-and-Plan**：新功能的預設模式，執行 `Design → Draft Tickets → Checkpoint`。不會自動實作或建立外部 Issues。
- **Implement**：只實作指定且已核准的 ticket，並保護既有 contracts。
- **Review**：依需求、ticket、contracts 與 repository 證據審查 diff、commit 或 PR；預設不修改程式碼。
- **Refine-Plan**：重新拆票、調整 dependencies、acceptance criteria 與 rollback boundaries。
- **Publish-Tickets**：只有明確要求並核准預覽後，才將票草案寫入 GitHub 或指定系統。

Skill 會依任務風險選擇 **Quick**、**Standard** 或 **High-risk** 分析深度，並每次只載入目前模式所需的 reference。

### 安裝

將此 repository clone 到暫存位置，再複製 Skill 資料夾：

#### PowerShell

```powershell
git clone https://github.com/MinhuiNoga/boundary-driven-development.git
New-Item -ItemType Directory -Force "$HOME\.agents\skills" | Out-Null
Copy-Item -Recurse -Force ".\boundary-driven-development\boundary-driven-development" "$HOME\.agents\skills\boundary-driven-development"
```

#### Bash

```bash
git clone https://github.com/MinhuiNoga/boundary-driven-development.git
mkdir -p ~/.agents/skills
cp -R ./boundary-driven-development/boundary-driven-development ~/.agents/skills/
```

若 Codex 沒有立即偵測到 Skill，請重新啟動 Codex。

### 使用範例

```text
$boundary-driven-development 設計一個 2D RPG 探索世界的架構
$boundary-driven-development 按照核准設計只實作 Ticket 3
$boundary-driven-development 審查目前 git diff 是否偏離 Ticket 3
```

專案專屬規則仍由各 repository 的 `AGENTS.md`、程式碼、測試、設定與文件提供；此 Skill 不會用通用規則取代專案事實。

## English

### Purpose

Define requirement scope, component responsibilities, data ownership, dependency boundaries, public contracts, state transitions, dataflow, failure paths, acceptance criteria, and test strategy before implementation. This reduces unstated architectural decisions, contract drift, out-of-scope changes, and oversized pull requests.

### Modes

- **Design-and-Plan**: Default for new features. Runs `Design → Draft Tickets → Checkpoint` without automatically implementing code or creating external issues.
- **Implement**: Implements only the selected approved ticket while preserving protected contracts.
- **Review**: Reviews a diff, commit, or pull request against requirements, tickets, contracts, and repository evidence. It is read-only by default.
- **Refine-Plan**: Re-splits tickets and improves dependencies, acceptance criteria, and rollback boundaries.
- **Publish-Tickets**: Writes approved ticket drafts to GitHub or another requested system only after an explicit request and preview approval.

The Skill chooses **Quick**, **Standard**, or **High-risk** analysis based on repository evidence and task risk. It loads only the reference required by the current mode.

### Installation

Clone this repository to a temporary location, then copy the Skill directory:

#### PowerShell

```powershell
git clone https://github.com/MinhuiNoga/boundary-driven-development.git
New-Item -ItemType Directory -Force "$HOME\.agents\skills" | Out-Null
Copy-Item -Recurse -Force ".\boundary-driven-development\boundary-driven-development" "$HOME\.agents\skills\boundary-driven-development"
```

#### Bash

```bash
git clone https://github.com/MinhuiNoga/boundary-driven-development.git
mkdir -p ~/.agents/skills
cp -R ./boundary-driven-development/boundary-driven-development ~/.agents/skills/
```

Restart Codex if the Skill is not detected immediately.

### Example prompts

```text
$boundary-driven-development Design the architecture for a 2D RPG exploration world.
$boundary-driven-development Implement only Ticket 3 from the approved design.
$boundary-driven-development Review the current git diff for deviations from Ticket 3.
```

Repository-specific rules still come from the applicable `AGENTS.md`, source code, tests, configuration, and documentation. This Skill never replaces project facts with generic assumptions.

## Repository structure

```text
boundary-driven-development/
├── README.md
└── boundary-driven-development/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    └── references/
        ├── design-and-plan.md
        ├── implement.md
        ├── publish-tickets.md
        ├── refine-plan.md
        └── review.md
```
