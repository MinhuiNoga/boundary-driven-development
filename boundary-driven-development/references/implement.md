# Implement

只實作使用者指定且已核准的 ticket。開始前驗證四個 gate：存在核准設計、ticket 身分與範圍明確、acceptance criteria 可測試、protected contracts 已列明。任一項缺失且無法從 repository 或對話確認時，指出缺口並停止會造成設計決策的變更。

## 1. 建立實作基線

1. 閱讀適用的 `AGENTS.md`、核准設計、指定 ticket、直接相關程式碼、測試與設定。
2. 將 ticket 的 scope、non-scope、dependencies、expected files、contracts、criteria、required tests 與 rollback boundary 摘要成工作基線。
3. 檢查前置 tickets 是否真的存在於目前 repository 狀態；不要只因票上標示完成就假設已整合。
4. 選擇符合 criteria 的最小變更路徑。不要順手實作其他 tickets、重寫無關程式碼，或加入未核准 abstraction、global manager 或 service locator。

## 2. 執行變更

- 沿用現有 component responsibilities、dependency direction、data ownership 與 error-handling pattern。
- 每次跨 boundary 時，確認 input/output、validation、state transition、side effect 與 failure behavior 符合核准設計。
- 不透過放寬 assertions、刪除測試或複製實作邏輯到測試來掩蓋錯誤。只有核准設計明確改變舊行為時，才更新對應測試並說明原因。
- 保留 rollback boundary：避免將獨立 ticket 混成不可分離的變更；不要執行未授權的 production 寫入或不可逆資料操作。
- 發現 repository 事實與設計矛盾時，區分可在 ticket 內修正的實作細節與需要重新核准的設計變更。

## 3. Contract escalation

若實作需要設計未聲明的 DB/save schema、public API/method signature、event/signal/message schema、auth、transaction、retry/idempotency、external side effect、resource lifecycle、concurrency、config key、file format、migration、public node path 或跨模組資料結構變更：

1. 立即停止擴張該變更，不默默修改。
2. 說明觸發原因與 repository 證據。
3. 列出受影響 callers、consumers、資料、state/dataflow、相容性與測試。
4. 提出至少一個不改 contract 的最小替代方案；若不可行，說明限制。
5. 等待使用者選擇，之後才更新設計或 ticket。

若只需要在已核准 contract 內調整私有實作，可繼續，但仍保持 ticket scope。

## 4. 驗證

先執行最接近變更的 focused tests，再依風險執行整合、契約或完整測試。驗證正常、邊界與失敗 criteria；對 stateful/async 工作驗證非法 transition、重複訊息、順序、retry、取消與 cleanup 中適用的案例。檢查 diff 沒有無關變更或意外 contract drift。

只能報告實際執行的命令與結果。將未執行、環境阻擋、flaky 結果與未覆蓋 edge cases 分開列出，不把靜態推論描述成測試通過。

## 5. 完成輸出

### Implementation Summary

- 完成的外部可觀察行為
- 修改檔案及其責任
- 與核准設計一致之處
- 任何已核准或尚待核准的偏差

### Contract Impact

逐項列出 API、DB、Save data、Events/Signals/Messages、Config、File formats、External side effects；未變更也寫 `No change`。

### Validation

列出實際測試命令、結果、未能執行的驗證及未覆蓋 edge cases，並逐項回連 acceptance criteria。

### Human Review Hotspots

只列最值得人類檢查的檔案／位置、風險與原因；不要將一般變更清單重複貼上。若 ticket criteria 全部滿足且沒有未核准 contract change，明確宣告指定 ticket 完成，不暗示其他 tickets 也完成。
