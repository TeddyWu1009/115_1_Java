# 第四週 AI 使用紀錄

每欄簡短填寫，不必貼完整對話。沒用 AI，第一欄填「未使用」及原因，其餘免填；課堂測試仍要完成。使用模擬建議時，第一欄寫「教師模擬建議，未使用 AI 服務」，其餘依本人操作填寫。

| 欄位 | 記錄 |
|---|---|
| 使用工具 | Claude（Anthropic，Claude Code）＋教師模擬建議（F3 第 2 種：`<` 改成 `<=`） |
| 使用目的與提問摘要 | 請 Claude 依講義完成 MissionLog 的 TODO 1–5，並協助整理 F2 的資料需求與 Prompt、判讀 int[100] 與 HashSet 兩項模擬建議；F3 選「printReplay 改成 `<= commandLog.size()`，理由是最後一步也要印」 |
| AI 建議與本人處理 | Claude 產生：每步把 command、energy、result 同時 add 到三條清單；countCommands 只計 0–3；printReplay 用相同索引讀三條清單；摘要用 Collections.frequency；最低能源先用 isEmpty 判斷再用 min。我採納，因為拒絕、無效與返回也有記錄，越界指令不會當索引。F3 的 `<=` 建議我拒絕，保留 `i < commandLog.size()`，因為合法索引只到 size()-1 |
| 實際驗證結果 | 本人實測（正確版，`<` 與 {1,1,2}）：在 java-04 編譯執行 MissionLog，回放 14、8、6，counts=[0, 2, 1, 0]，steps=3, ok=3, rejected=0, ignored=0，lowest energy=6，final energy=6, samples=5, actionCount=3 -> SUCCESS，battery=30.0，與預期一致。待補測：腳本改 {0} 並把 printReplay 改成 `<=`（預期 IndexOutOfBoundsException: Index 1 out of bounds for length 1），改回 `<` 後 {0}（預期 20/0/0 RETURNED）。其餘路線見講義 E 與 F3 表 |

只記錄實際發生的操作。AI 宣稱正確不能取代編譯執行。不要上傳個資、密碼、憑證或未授權資料。

## 完整填寫範例（虛構／模擬）

以下假設學生真的做過表列操作，用於示範填寫；不是你的執行紀錄，也不是與 AI 的真實對話。提交時刪除本範例，保留上方本人紀錄。

| 欄位 | 虛構填寫示範 |
|---|---|
| 使用工具 | 教師模擬建議，未使用 AI 服務 |
| 使用目的與提問摘要 | 檢查「指令固定 0–3，所以 counts 不需範圍檢查」；沒有向真實 AI 提問 |
| AI 建議與本人處理 | 我拒絕這個建議：日誌也記無效指令。我恢復 command >= 0 && command < counts.length，再累加 counts[command] |
| 實際驗證結果 | 副本使用 {3,9,0}，修改前在 counts[9] 越界；恢復檢查後 counts=[1,0,0,1]、steps=3、ok=0、rejected=2、ignored=0，結果 RETURNED。重測 {1,1,2} 得 6/5/3 SUCCESS；完整輸出記在 LAB_REPORT F3 |
