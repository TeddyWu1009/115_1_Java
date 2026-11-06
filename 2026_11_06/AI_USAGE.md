# 第五週 AI 使用紀錄

| 欄位 | 本人紀錄 |
|---|---|
| 使用工具／方式 | 真實 AI：Claude（Claude Code）。java-05/src 的 MissionStep TODO 1–3、MissionLog TODO 4–6 由 AI 產生程式碼，本人閱讀後保留；提問題目由 AI 建議、本人同意後提出 |
| 目的與提問摘要 | 審查 MissionStep.isSuccessful()：為什麼要寫 `"OK".equals(result)`，不能寫 `result == "OK"`？改成 `==` 時什麼輸入會出錯？ |
| 建議與本人處理 | AI 說明 `==` 比較是否為同一物件，`equals` 比較字串內容；反例為 `new MissionStep(1, 14, new String("OK"))`，用 `==` 會誤判為失敗、ok 少算。`{1,1,2}` 測不出來，是因為字串常值共用物件。本人採納，保留 `"OK".equals(result)`，未改成 `==` |
| 本人實際驗證 | 本人於 java-05 重新編譯並執行 `java -cp out MissionLog`（{1,1,2}）：回放 step 1–3 為 OK/14、OK/8、OK/6；counts=[0, 2, 1, 0]；steps=3, ok=3, rejected=0, ignored=0；lowest energy=6；final energy=6, samples=5, actionCount=3 -> SUCCESS；battery=30.0，與第四週一致。另在 main 暫加測試 `String r = new String("OK");`，實際輸出依序為：`r == "OK"` → false、`"OK".equals(r)` → true、`new MissionStep(1, 14, r).isSuccessful()` → true。證實 `==` 比較的是物件而非內容，若 isSuccessful 改用 `==` 會把這筆誤判為失敗；採用 equals 的版本判斷正確。測試後已移除暫加程式，重新編譯執行，輸出恢復為 === replay === 開頭、6/5/3 SUCCESS |

AI 的正確宣稱不能代替編譯執行。觀看教師展示只能記「教師示範觀察」；本人複驗才可記本人測試。
