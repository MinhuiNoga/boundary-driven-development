# Design-and-Plan

將設計與拆票視為同一個預設流程：`Design → Draft Tickets → Design-and-plan checkpoint`。先完成足夠的 repository inspection，再一次交付設計、票草案與 traceability；全程不要修改程式碼、建立 GitHub Issues／Jira tickets，或自行進入 Implement。

## 1. Repository inspection

1. 找出並完整閱讀作用於目標檔案的 `AGENTS.md`；其範圍較深者優先。
2. 閱讀直接相關的 entry points、元件、資料模型、public interfaces、設定、測試與文件。用搜尋與實際 caller/callee 關係確認架構，不依檔名推測。
3. 記錄現有資料建立者、owner、mutator、consumer 與銷毀時機；追蹤同步與非同步 dataflow、依賴方向、side effects 及 failure handling。
4. 標出既有 protected contracts 與相容性限制。若證據不足，列為未知事項或 open decision，不自行補完需求。
5. 選擇 Quick、Standard 或 High-risk 深度，說明風險依據。停止任何程式碼修改。

## 2. Design

依選定深度調整篇幅；Standard／High-risk 依序輸出：

1. **Requirement interpretation**：將使用者目標改寫成可觀察行為。
2. **Assumptions**：分開列出已知事實、合理推論與待確認事項。
3. **Existing architecture**：只描述由 repository 證據支持的現況並引用檔案。
4. **Scope / Non-scope**：明列本次做與不做的事。
5. **Component responsibilities**：每個元件只寫責任、輸入、輸出與不負責事項。
6. **Data ownership**：定義建立、持有、修改、讀取、持久化與銷毀者。
7. **Dependency boundaries**：定義允許與禁止的依賴方向，以及跨界方式。
8. **Public contracts**：逐項說明 API、DB、save、event/signal/message、config、file format 與 external side effects 是否改變。
9. **State model**：列出 states、初始／終止 state、合法與非法 transitions、trigger、guard、side effect 與失敗行為。優先使用 transition table，只有複雜關係需要視覺化時才加入 Mermaid。
10. **Dataflow**：從輸入到輸出依序描述資料、控制與錯誤流向，標示同步／非同步邊界。
11. **Failure model**：涵蓋驗證失敗、部分完成、retry、timeout、取消、重啟與 rollback；依風險選擇適用項目。
12. **Proposed design / Expected file changes**：提出沿用架構的最小方案；按檔案說明預期責任，不先寫實作。
13. **Acceptance criteria**：使用可觀察、可測試的 Given/When/Then 或等價敘述；不要只寫「良好」「順暢」「適當」。包含正常、邊界與失敗案例。
14. **Test strategy**：將每項 criteria 對應到測試層級與驗證方法；測試不得只複製實作演算法。
15. **Risks / Open decisions**：標示影響、觸發條件、緩解方式與必須由使用者決定的事項。

Quick 可合併上述段落，但至少保留 scope、affected components、protected boundaries、acceptance criteria 與 validation。

## 3. Draft Tickets

設計完成後自動產生文字票草案。每張 ticket 包含：

- **Title、Goal**
- **Scope、Non-scope**
- **Dependencies**：含順序與阻擋關係
- **Expected files**
- **Protected contracts**：列出改變或 `No change`
- **Acceptance criteria、Required tests**
- **Rollback boundary、Definition of done**

一張票只承擔一個主要行為變化，且可獨立驗證、盡可能獨立回滾。依行為與 contract 邊界拆分，不以程式碼行數拆分。不要未經說明混合 schema/public contract 變更與一般內部實作；過大票繼續拆分。

最後建立精簡矩陣：`Requirement → Component → Ticket → Acceptance Criterion → Test`。每項需求至少對應一張票、一項 criterion 與一個驗證方法；無法對應者列為 gap。

## 4. Design-and-plan checkpoint

摘要需要核准的設計決策、protected contract 變更、票範圍／依賴與 open decisions，明確說明尚未修改程式碼、尚未建立外部 tickets。詢問使用者是否核准設計與票；停在 checkpoint，不自動開始 Implement 或 Publish-Tickets。
