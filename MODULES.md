# MODULES.md

此文檔記錄 everything-claude-code 項目的模組結構及其關係。

---

## 目錄結構概覽

```
everything-claude-code/
├── .claude-plugin/           # Claude 插件清單
├── agents/                   # 專業化子代理
├── commands/                 # 斜杠命令
├── contexts/                 # 動態系統提示注入
├── examples/                 # 配置示例
├── hooks/                    # 觸發式自動化
├── mcp-configs/              # MCP 服務器配置
├── plugins/                  # 插件相關文檔
├── rules/                    # 必須遵循的準則
└── skills/                   # 工作流定義和領域知識
```

---

## 模組詳細說明

### .claude-plugin/

**用途**：Claude Code 插件元數據和市場清單

| 文件 | 說明 |
|-----|------|
| `marketplace.json` | 市場清單配置，定義組件路徑 |
| `plugin.json` | 插件元數據 |

---

### agents/ (9 個代理)

**用途**：專業化子代理，針對特定任務的專家代理

| 文件 | 功能 | 依賴 |
|-----|------|------|
| `architect-agent.md` | 架構設計代理 | `rules/patterns-rule.md` |
| `build-error-resolver-agent.md` | 構建錯誤解決 | `rules/security-rule.md` |
| `code-reviewer-agent.md` | 代碼審查代理 | `rules/coding-style-rule.md`, `security-review-skill.md` |
| `doc-updater-agent.md` | 文檔更新代理 | `rules/coding-style-rule.md` |
| `e2e-runner-agent.md` | E2E 測試運行 | `rules/testing-rule.md`, `tdd-workflow-skill.md` |
| `planner-agent.md` | 規劃代理 | `rules/patterns-rule.md` |
| `refactor-cleaner-agent.md` | 重構清理代理 | `rules/coding-style-rule.md` |
| `security-reviewer-agent.md` | 安全審查代理 | `rules/security-rule.md`, `security-review-skill.md` |
| `tdd-agent.md` | TDD 指導代理 | `rules/testing-rule.md`, `tdd-workflow-skill.md` |

---

### commands/ (14 個命令)

**用途**：斜杠命令，用戶可快速執行的快捷指令

| 文件 | 觸發 | 映射代理 | 相關技能/規則 |
|-----|------|---------|--------------|
| `build-fix-command.md` | `/build-fix` | `build-error-resolver-agent.md` | - |
| `checkpoint-command.md` | `/checkpoint` | - | `strategic-compact-skill.md` |
| `code-review-command.md` | `/code-review` | `code-reviewer-agent.md` | - |
| `e2e-command.md` | `/e2e` | `e2e-runner-agent.md` | `tdd-workflow-skill.md` |
| `eval-command.md` | `/eval` | - | `eval-harness-skill.md` |
| `learn-command.md` | `/learn` | - | `continuous-learning-skill.md` |
| `orchestrate-command.md` | `/orchestrate` | `planner-agent.md` | - |
| `plan-command.md` | `/plan` | `planner-agent.md` | - |
| `refactor-clean-command.md` | `/refactor-clean` | `refactor-cleaner-agent.md` | - |
| `tdd-command.md` | `/tdd` | `tdd-agent.md` | `tdd-workflow-skill.md` |
| `test-coverage-command.md` | `/test-coverage` | - | `verification-loop-skill.md` |
| `update-codemaps-command.md` | `/update-codemaps` | `doc-updater-agent.md` | - |
| `update-docs-command.md` | `/update-docs` | `doc-updater-agent.md` | - |
| `verify-command.md` | `/verify` | - | `verification-loop-skill.md` |

---

### contexts/ (3 個上下文)

**用途**：動態系統提示注入，針對不同開發模式提供專用上下文

| 文件 | 使用場景 | 關聯 |
|-----|---------|------|
| `dev-context.md` | 開發模式 | 被 `commands/*-command.md` 引用 |
| `research-context.md` | 研究模式 | 被代理和命令引用 |
| `review-context.md` | 代碼審查模式 | 被 `code-review-command.md` 引用 |

---

### examples/

**用途**：配置示例和模板

| 文件 | 說明 |
|-----|------|
| `CLAUDE.md` | 項目級 Claude 配置示例 |
| `user-CLAUDE.md` | 用戶級配置示例 |
| `statusline.json` | 狀態行配置示例 |

---

### hooks/

**用途**：觸發式自動化，在特定事件執行腳本

| 目錄 | 觸發時機 | 依賴 |
|-----|---------|------|
| `memory-persistence/` | 會話開始/結束 | `continuous-learning-skill.md` |
| `strategic-compact/` | 壓縮時 | `strategic-compact-skill.md` |

**腳本**：
- `memory-persistence/session-start.sh` - 會話開始時加載記憶
- `memory-persistence/session-end.sh` - 會話結束時保存記憶
- `memory-persistence/pre-compact.sh` - 壓縮前備份
- `strategic-compact/suggest-compact.sh` - 建議壓縮

---

### mcp-configs/

**用途**：MCP (Model Context Protocol) 服務器配置

| 文件 | 配置的服務器 |
|-----|------------|
| `mcp-servers.json` | GitHub, Supabase, Railway, ClickHouse, 等 17 個服務 |

---

### plugins/

**用途**：插件相關文檔（未完全實現）

| 文件 | 說明 |
|-----|------|
| `README.md` | 插件開發指南 |

---

### rules/ (8 個規則)

**用途**：必須遵循的準則，被所有代理和命令引用

| 文件 | 適用範圍 |
|-----|---------|
| `agents-rule.md` | 代理行為準則 |
| `coding-style-rule.md` | 代碼風格規範 |
| `git-workflow-rule.md` | Git 工作流 |
| `hooks-rule.md` | Hook 使用規則 |
| `patterns-rule.md` | 設計模式 |
| `performance-rule.md` | 性能要求 |
| `security-rule.md` | 安全檢查 |
| `testing-rule.md` | 測試要求 |

---

### skills/ (11 個技能 + 輔助文件)

**用途**：工作流定義和領域知識

| 文件 | 功能 | 依賴 |
|-----|------|------|
| `backend-patterns-skill.md` | 後端設計模式 | `rules/patterns-rule.md` |
| `clickhouse-io-skill.md` | ClickHouse 數據庫操作 | - |
| `coding-standards-skill.md` | 編碼標準 | `rules/coding-style-rule.md` |
| `continuous-learning-skill.md` | 持續學習工作流 | `hooks/memory-persistence/*` |
| `eval-harness-skill.md` | 評估框架 | - |
| `frontend-patterns-skill.md` | 前端設計模式 | `rules/patterns-rule.md` |
| `project-guidelines-example-skill.md` | 項目指南示例 | `rules/*` |
| `security-review-skill.md` | 安全審查清單 | `rules/security-rule.md` |
| `strategic-compact-skill.md` | 策略性壓縮 | `hooks/strategic-compact/*` |
| `tdd-workflow-skill.md` | TDD 工作流 | `rules/testing-rule.md` |
| `verification-loop-skill.md` | 持續驗證 | `eval-harness-skill.md` |

**輔助文件**：
- `continuous-learning/config.json` - 持續學習配置
- `continuous-learning/evaluate-session.sh` - 會話評估腳本
- `strategic-compact/suggest-compact.sh` - 壓縮建議腳本

---

## 依賴關係圖

```
                    Commands (用戶入口)
                        ↓
                    Agents (專家代理)
                        ↓
        ┌───────────────┴───────────────┐
        ↓                               ↓
    Rules (規則)                    Skills (技能)
        ↓                               ↓
    Contexts (上下文)              Hooks (自動化)
                                        ↓
                                 MCP-Configs (集成)
```

---

## 核心工作流

### 1. TDD 工作流
```
/tdd-command → tdd-agent → tdd-workflow-skill → testing-rule
```

### 2. 代碼審查工作流
```
/code-review-command → code-reviewer-agent → security-review-skill
```

### 3. 持續學習工作流
```
/learn-command → continuous-learning-skill → hooks/memory-persistence/
```

### 4. 規劃工作流
```
/plan-command → planner-agent → patterns-rule → dev-context
```

---

## 模組統計

| 類別 | 數量 |
|-----|------|
| Agents | 9 |
| Commands | 14 |
| Rules | 8 |
| Contexts | 3 |
| Skills | 11 |
| Hooks (腳本) | 4 |
| MCP 服務器 | 17 |
| 總計 | 66+ |

---

## 命名約定（Phase 1 後）

| 文件類型 | 命名格式 |
|---------|---------|
| Agents | `<purpose>-agent.md` |
| Commands | `<action>-command.md` |
| Rules | `<category>-rule.md` |
| Contexts | `<mode>-context.md` |
| Skills | `<domain>-skill.md` |

---

## 配置文件引用關係

### `.claude-plugin/marketplace.json`
定義所有組件的路徑，供 Claude Code 插件系統使用。

### `hooks/hooks.json`
定義所有 hook 的配置，包括觸發時機和執行的腳本。

### `examples/CLAUDE.md`
項目級配置模板，展示如何引用各個模組。

---

## 注意事項

1. **Skills 目錄結構**：部分 skill 位於子目錄中（如 `continuous-learning/`），包含腳本和配置
2. **Hooks 緊密耦合**：Hooks 直接引用 skills 中的腳本，修改時需同步更新
3. **規則通用性**：所有 rules 被多個 agents 和 commands 引用，修改需謹慎
4. **Context 注入**：Contexts 通過命令代理系統動態注入，非直接調用
