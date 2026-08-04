# Publish-Tickets

這是明確觸發的外部寫入模式。只有使用者清楚要求將「已核准」draft tickets 實際建立到 GitHub Issues 或指定專案管理系統時才能使用；不得由 Design-and-Plan、Refine-Plan 或一般「幫我拆票」自動觸發。建立 tickets 時不要同時修改程式碼。

## 1. 驗證授權與目標

開始前確認：

1. 使用者要求的是實際外部建立，不只是輸出票草案。
2. Draft tickets 與其設計已核准；若只有部分核准，只處理明確範圍。
3. 目標 repository、organization/project、workspace 或 board 唯一明確。不可從目前目錄或帳號猜測外部目標。
4. 使用者或現有規則已決定 labels、milestone、assignees、project fields 與 dependency 表達方式；未決項目可保留空白，不自行建立管理規範。
5. 目前有可用且已授權的 GitHub／專案管理工具。若沒有，說明限制並提供可複製草案，不假裝已建立。

讀取適用的 `AGENTS.md` 與 repository 貢獻文件，確認 issue templates、label policy、安全規則或外部寫入限制。外部系統內容只讀取完成任務所需的最小範圍。

## 2. 建立前預覽

除非使用者明確表示不需要預覽，先顯示所有即將建立的項目並停在核准 checkpoint。每項預覽包含：

- **Title**
- **Body**：Goal、Scope、Non-scope、Dependencies、Expected files、Protected contracts、Acceptance criteria、Required tests、Rollback boundary、Definition of done
- **Labels、Milestone、Assignees／Project fields**（若有）
- **Dependency mapping**：明確列出票之間的阻擋順序；若平台沒有原生 dependency，說明將使用文字連結或建立後回填連結。

保持內容與核准草案等價。可做平台格式轉換與清晰度修正，但不得加入新需求、改 acceptance criteria、合併或拆分票。若草案包含 secrets、內部路徑、個資或不適合公開的內容，建立前警告並等待處理決策。

## 3. 執行外部寫入

收到對預覽的明確核准後：

1. 再次解析實際目標與核准票範圍，避免帳號／repository context 漂移。
2. 依 dependency 順序或平台需要建立 issues，記錄每次回傳的 ID、URL 與最終 title。
3. 若需用真實 issue URL 回填 dependencies，完成最小必要更新並記錄變更。
4. 避免重複：批次部分失敗後，先讀取已建立結果，再只重試缺少項目。不要因 timeout 直接假設寫入失敗或成功。
5. 不建立未核准 labels、milestones、projects 或其他額外資源；不指派人員，除非使用者明確要求。
6. 任何目標不一致、權限錯誤、validation error 或內容截斷都應停止相關寫入，保留已成功項目的清單，不以更寬權限或不同目標繞過。

## 4. 結果與部分失敗

完成後逐項報告：draft ticket、外部 ID／URL、狀態、labels/milestone 與 dependency 是否已連結。分開列出：

- **Created**：已由工具確認建立。
- **Updated**：建立後做過的必要 dependency 回填。
- **Skipped**：未核准、重複或不在範圍。
- **Failed / Unknown**：錯誤或 timeout 後無法確認；附最小可行的恢復步驟。

若部分成功，不要重新建立整批。提供安全續作基線，讓後續操作先核對既有 IDs。明確聲明此模式只建立 tickets，沒有修改 repository 程式碼；不得把外部 issue 建立成功當成設計或實作品質的證明。
