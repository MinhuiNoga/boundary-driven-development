# Review

預設只審查，不修改程式碼。以使用者需求、核准設計、指定 ticket、acceptance criteria、protected contracts、適用的 `AGENTS.md`、repository 證據、git diff／commit／PR 與既有測試作為依據。若缺少設計或 ticket，仍可審查具體 correctness，但將無法驗證的範圍標為 assumption，不自行發明規格。

## 1. 確定審查範圍

1. 確認審查目標是 working tree、特定 diff、commit 或 PR，並取得完整 patch 與必要上下文。
2. 閱讀受影響檔案適用的 `AGENTS.md`，以及被修改符號的 callers、consumers、tests、config 與資料模型。
3. 建立 `Requirement / Ticket criterion → Changed behavior → Validation` 對照，標出 scope 之外的變更。
4. 列出 protected contracts 的預期狀態；不要把檔案未修改誤當成 contract 未受影響，需追蹤行為與資料流。

## 2. 依風險審查

依序檢查，先找會造成外部錯誤或資料風險的問題：

1. 未滿足需求或 acceptance criterion。
2. 超出 ticket scope、夾帶其他 ticket，或破壞 rollback boundary。
3. 未聲明的 schema、public API、method signature、event/signal/message、config、file format 或 external side effect 變更。
4. 資料 ownership、建立／修改／銷毀責任或 dependency direction 遭破壞。
5. 非法、遺漏或不可達的 state transition；state 與持久化資料不一致。
6. Authentication/authorization、敏感資料、validation 與 trust boundary 錯誤。
7. Transaction 原子性、部分完成、retry/idempotency、timeout、取消、race、async ordering 或重複處理錯誤。
8. Resource acquire/release、訂閱解除、檔案／連線生命週期或 cleanup 漏洞。
9. Error propagation、fallback、compatibility、migration、performance 與 observability regression。
10. 測試漏掉 requirement/failure path、只複製實作邏輯、錯誤 mock boundary，或以改測試隱藏 regression。
11. 無助於需求的不必要複雜度；不要把純風格、命名偏好或可選重構當成 bug。

對每個疑點追到具體執行路徑。確認觸發輸入或狀態、受影響 caller/consumer、實際輸出或 side effect，以及現有保護為何不足。無法建立可重現條件或契約違反時，不升格為 finding；可列入 residual risk 或待驗證假設。

## 3. 驗證

在不改動狀態的前提下執行適度的 read-only inspection、靜態檢查或 tests。優先執行可證實／推翻高風險疑點的 focused validation。不要聲稱未執行測試已通過，也不要因 tests 綠燈忽略錯誤的 criteria 或 contract。

若使用者只要求 review，不套用修正。可提供最小修正方向，但不要擴成完整 redesign；需要設計決策時指出 checkpoint。

## 4. Findings 格式

先列 findings，按 **Critical → High → Medium → Low** 排序。每項必須包含：

- 簡潔標題與 severity
- 精確檔案與位置
- 觸發條件／輸入／state
- 實際影響與受影響範圍
- 違反的 requirement、criterion、contract 或 boundary
- 不擴張 scope 的最小修正方向

避免只寫「可能有問題」；清楚區分已證實、合理推論與需 runtime 驗證的事項。多個位置屬於同一根因時合併 finding，選最能定位修正的位置。

若沒有具體問題，寫：

`No concrete correctness or contract violations found.`

接著列出 **Residual risks**、**Untested assumptions**、**Runtime verification needs**。最後簡短說明已審查的範圍與執行過的驗證；不要用總結掩蓋 findings，也不要宣稱整個系統正確。
