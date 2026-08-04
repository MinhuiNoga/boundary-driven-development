# Refine-Plan

只更新已存在的設計與 draft tickets；不要修改程式碼、建立外部 Issues，或把票標示為已實作。適用於重新拆票、縮小或澄清 ticket 範圍、補 dependencies／acceptance criteria／tests，或將過大的票拆成可獨立驗證與回滾的工作。

## 1. 建立基線

1. 讀取適用的 `AGENTS.md`、核准設計、目前票草案、traceability matrix，以及可驗證設計假設的直接相關 repository 證據。
2. 確認哪些設計決策已核准、哪些仍開放；Refine-Plan 可以改票的執行切分，但不得默默改變需求、component responsibilities、data ownership、dependency boundaries、state rules 或 protected contracts。
3. 找出觸發 refine 的具體原因：票過大、criteria 不可測、dependencies 循環、contract 與內部實作混雜、rollback boundary 不清楚、檔案重疊造成衝突，或需求沒有票／測試覆蓋。

若 repository 現況證明核准設計本身不可行，將它標為 design change request，列出證據與影響，停下來請使用者核准；不要用拆票名義改設計。

## 2. 評估每張票

逐張檢查：

- 是否只有一個主要、外部可描述的行為變化。
- Scope 與 non-scope 是否排除相鄰 tickets；是否可在不順手完成其他票的情況下實作。
- Dependencies 是否必要、方向清楚且無循環；可並行的票不要建立假依賴。
- Expected files 是否符合 component ownership；只能作為預期，不將未檢查的檔名當事實。
- Protected contracts 是否逐項寫出改變或 `No change`；schema/public contract change 不與一般內部重構含糊混合。
- Acceptance criteria 是否可觀察、可測試並涵蓋正常、邊界與失敗行為。
- Required tests 是否驗證需求與 boundary，而非複製實作演算法。
- Rollback boundary 是否能移除此票而不留下半套 schema、state 或 side effects。
- Definition of done 是否包含程式、驗證、文件／migration（若適用）與 contract 報告。

## 3. 拆分與重組規則

依行為、contract 與風險拆票，不以預估行數或檔案數為主要依據。優先分離：

1. Contract/schema 定義與相容性或 migration。
2. 核心 domain behavior 與 state transitions。
3. Adapter、UI、persistence 或 external side effects。
4. Integration、observability 與 rollout/cleanup。

只有在前置 contract 可穩定且各票仍可驗證時才依序拆分；不要創造只增加 scaffolding、沒有可驗證價值的 tickets。合併票時，確認合併後仍有單一主要行為且可獨立回滾。對 High-risk 變更明確安排 compatibility、migration、rollback 與 failure-path tests。

## 4. 更新產物

每張修訂票完整輸出 **Title、Goal、Scope、Non-scope、Dependencies、Expected files、Protected contracts、Acceptance criteria、Required tests、Rollback boundary、Definition of done**。標記 `New`、`Changed`、`Split from`、`Merged from` 或 `Removed`，並簡述原因，讓使用者能比較前後範圍。

重新建立精簡 traceability matrix：`Requirement → Component → Ticket → Acceptance Criterion → Test`。檢查每項 requirement 都有 owner、票、criterion 與驗證；列出 orphan tickets、未覆蓋 requirements、重複 criteria 與 unresolved dependencies。

## 5. Refine-plan checkpoint

摘要票數與依賴順序的變化、protected contract 影響、需要重新核准的範圍與 open decisions。明確說明尚未修改程式碼、尚未建立外部 tickets；等待使用者核准更新後的草案。除非使用者另外明確要求，不進入 Implement 或 Publish-Tickets。
