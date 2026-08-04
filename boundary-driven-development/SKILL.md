---
name: boundary-driven-development
description: "以 design-first、boundary-driven、contract-first、risk-based 與 ticket-driven 流程設計、實作、拆票及審查 repository 變更。用於新功能或系統設計、跨模組修改、架構重構、state machine、dataflow、component boundaries、schema 或 contract impact、已核准 ticket 的實作，以及 diff、commit 或 PR 審查；使用者明確呼叫 $boundary-driven-development 時也使用。不要自動用於 typo、單行孤立低風險修改、單純程式碼解釋、純文字改寫、非 repository／軟體設計問題，或使用者明確要求快速完成且範圍清楚的小修改。"
---

# Boundary-Driven Development

以 repository 證據先界定需求、責任、資料、依賴、狀態與 contracts，再進行可驗證的小範圍工作。預設跟隨使用者語言；繁體中文內容保留 Class、API、schema、state、dataflow、signal 與檔名原文。

## 選擇模式

- **Design-and-Plan**：用於尚未核准的新功能、系統、跨模組修改或重構。執行 `Design → Draft Tickets → Checkpoint`；不得修改程式碼、建立外部 tickets 或自行進入 Implement。讀取 [references/design-and-plan.md](references/design-and-plan.md)。
- **Implement**：只在設計已核准、ticket 已指定、acceptance criteria 與 protected contracts 已知時使用。讀取 [references/implement.md](references/implement.md)。
- **Review**：審查 diff、commit、PR 或實作與設計的偏差；預設只審查、不修改。讀取 [references/review.md](references/review.md)。
- **Refine-Plan**：重新拆票、調整範圍、dependencies 或 acceptance criteria；只更新票草案。讀取 [references/refine-plan.md](references/refine-plan.md)。
- **Publish-Tickets**：只有使用者明確要求將已核准草案寫入 GitHub 或指定系統時使用。讀取 [references/publish-tickets.md](references/publish-tickets.md)。

每次只讀取目前模式的一份 reference。若請求跨模式，依序完成並遵守各模式 checkpoint；不要預先載入其他 references。

## 選擇分析深度

- **Quick**：局部、低風險。涵蓋 scope、affected components、protected boundaries、acceptance criteria、validation 與一至數張 draft tickets。
- **Standard**：一般新功能或跨模組工作。涵蓋需求、既有架構、scope/non-scope、責任、資料、依賴、state、dataflow、contracts、failure paths、tests、risks、tickets 與 traceability。
- **High-risk**：涉及 DB/save schema、public API、event/signal/message schema、authentication/authorization、migration、transaction、concurrency、async workflow、retry/idempotency、external side effects、file compatibility、resource lifecycle 或不可逆資料操作。

若使用者未指定，依任務與 repository 證據選擇最低但足夠的深度；小任務不要產生過度完整報告。

## 共通原則

1. 先讀取適用的 `AGENTS.md`，再讀直接相關的程式碼、設定、測試與文件；沒有 `AGENTS.md` 時可建議建立，但不得視為必要前置條件。
2. 以實際內容為依據，不從檔名猜架構。區分已知事實、合理推論與未知事項。
3. 優先沿用既有架構與最小修改；不因偏好改寫無關程式碼，不任意新增 global manager、service locator 或 abstraction layer。
4. 定義資料的建立、持有、修改與銷毀者；定義合法／非法 state transitions、依賴方向、dataflow、failure paths 與 external behavior。
5. 明確報告 protected contracts 是否改變：DB/save schema、public API/method signatures、event/signal/message schema、auth、transaction、retry/idempotency、side effects、resource lifecycle、concurrency、config keys、file formats、migration、public node paths，以及跨模組依賴的資料結構。
6. 不宣稱未執行的測試已通過；圖表只輔助精確文字，生成文件不構成正確性證明。
7. 實作細節可調整，但不得破壞核准的 protected contracts、外部行為或 state 規則。

若 Implement 發現未核准的高風險變更，停止擴張：說明原因與受影響 caller、資料、流程及測試，提出不改 contract 的替代方案，等待使用者決策。
