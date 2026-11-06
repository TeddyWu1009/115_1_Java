# 第四週學生實作：火星探測車任務日誌

姓名：洪珩毅　學號：A112223020　實際上課日期：2026-10-30　實際 JDK：Oracle JDK 24.0.1

另存為當日 LAB_REPORT.md。A–H 必做，最後的延伸選做。先預測再執行；教師示範觀察與本人實測分開。作業完整需求見 [assignment.md](assignment.md)。

## A｜取回與先備診斷（10 分鐘）

1. 無 repo 時 clone，有副本先保存再 Fetch/Pull；在 115_1_Java/YYYY_MM_DD 建 java-04/src。
2. 放五個核心 examples 與 starter/MissionLog.java，勿放 teacher 同名類別；日期下放本報告及 AI_USAGE。
3. 執行 java -version、javac -version，寫下完整版本：java version "24.0.1" 2025-04-15、Java(TM) SE Runtime Environment (build 24.0.1+9-30)、Java HotSpot(TM) 64-Bit Server VM (build 24.0.1+9-30, mixed mode, sharing)；javac 24.0.1。不限定特定 JDK 版本。
4. 在 java-04 編譯 `javac -encoding UTF-8 -d out src/*.java`；成功後跑 MissionLog。此時能跑任務但日誌與摘要未完成，是預期起始狀態。

診斷：第三週 {1,1,2} 的最終能源/樣本/成功行動次數：能源 6／樣本 5／成功行動 3。{2,1,1} 是否相同：相同。只有最終三個數字，能否分辨路線：不能。

## B｜固定資料：陣列與計數（30 分鐘）

教科書 §7.2–7.4、§7.7。先讀 ArrayBasics，再執行。

| 項目 | 預測 | 本人實際結果 |
|---|---|---|
| script.length | 	3 | 	length=3 |
| new int[4] 內容 | [0, 0, 0, 0]（int 預設值 0） | 	counts=[0, 0, 0, 0] |
| new String[2] 內容 | [null, null]（參考型態預設值 null） | labels=[null, null] |
| 逐步指令輸出 | step 1: 1、step 2: 1、step 3: 2 | step 1: 1／step 2: 1／step 3: 2 |
| 指令數值加總（不是行動數） |1+1+2=4 | 	sum=4 |

1. length 是欄位，不加括號；陣列合法索引範圍是：0 到 length-1，本例為 0–2。
2. 在副本取消越界行註解，編譯並執行，記錯誤種類與索引：執行期例外 java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3，可以編譯，執行到第 23 行才發生；索引 3 等於 length，超出最後一格 2。隨即恢復。
3. 執行 CommandCounter：腳本 {3,9,0,1,1,-1}，預測 counts：[1, 2, 0, 1]（0 一次、1 兩次、2 零次、3 一次；9、-1 跳過）；實際：counts=[1, 2, 0, 1]。9 和 -1 為何不得當索引：counts 只有索引 0–3，9 和 -1 超出範圍，直接當索引會發生。
4. 把 CommandCounter 的固定資料改為 {0,2,2,3}，預測：[1, 0, 2, 1]；實際：counts=[1, 0, 2, 1]；恢復原示範。
5. 一般 for 適合需要步號/索引；加強版 for 適合只讀每筆值。若在 `for (int command : script)` 內寫 command=0，是否改掉 script 的元素：不會。用副本檢查：在副本的加強版 for 內加上 command = 0;，迴圈後印出 script，仍為 [1, 1, 2]，sum 仍為 4。

## C｜參考與方法（10 分鐘）

教科書 §7.8–7.9。ArrayReference 的兩個變數先指向同一陣列；畫箭頭或用文字描述。

| 時間點 | 預測 energy 內容 | 本人實際 |
|---|---|---|
| alias[0]=19 後 |	[19, 14, 8]（alias 和 energy 指向同一陣列） | alias change=[19, 14, 8] |
| changeFirst 後 | 	[18, 14, 8] | element change=[18, 14, 8] |
| replaceLocal 內的新陣列 | [20, 20, 20] | 	inside local replace=[20, 20, 20] |
| 回到 main 後 | 	[18, 14, 8]（呼叫端不受影響） | after local replace=[18, 14, 8] |
| == 與 Arrays.equals 的兩個結果 | false（不同陣列物件）／true（內容相同） | same reference=false／same content=true |

Java 傳值；陣列參數取得的是：參考值的副本。為何修改元素看得見，重新指定參數卻不改呼叫端：修改元素是透過同一個參考改共同的陣列，所以呼叫端看得到；重新指定參數只是讓方法內那份參考副本改指向新陣列，呼叫端變數仍指向原陣列，不受影響。

## D｜筆數會增加：ArrayList（15 分鐘）

教科書 §7.16、§16.3–16.4。C# List<T> 若尚未學過，以右欄 Java 為準。

先分清腳本與日誌：{1,1,1,1}有四筆預定指令，但從20/0出發第三次探索就達標，因此只記三筆。陣列可搭配有效筆數保存；ArrayList每記錄一步就add，size()直接取得已存筆數，本週用它練習逐筆記錄。

| 操作 | C# List<int> | Java ArrayList<Integer> |
|---|---|---|
| 筆數 | Count | size() |
| 讀第 i 筆 | list[i] | get(i) |
| 改第 i 筆 | list[i]=x | set(i,x) |
| 尾端新增 | Add(x) | add(x) |
| 刪索引 | RemoveAt(i) | remove(i) |

執行 ListBasics：

| 項目 | 預測 | 本人實際 |
|---|---|---|
| 原清單/size/first | [1, 1, 2]／3／1 | commands=[1, 1, 2], size=3／first=1 |
| set(1,3) 後 | [1, 3, 2]（改索引 1，筆數不變） | after set=[1, 3, 2] |
| remove(0) 後 | [3, 2]（0 是索引，刪第一筆） | after remove index=[3, 2] |
| remove(Integer.valueOf(3)) 後 | [2]（按值刪除第一個 3） | after remove value=[2] |
| isEmpty() | false（還剩 1 筆） | empty=false |

int 放入清單會裝箱，讀成 int 會拆箱；ArrayList<int> 為何不能編譯：泛型的型態參數必須是參考型態，int 是基本型態，要改用包裝類別 Integer。副本編譯訊息：`error: unexpected type`、`required: reference`、`found: int`，已恢復可編譯版本。可以在副本重現編譯訊息，但必須恢復可編譯版本。remove 的兩種用途只需能分辨，不要求說明完整過載規則。

## E｜新增、回放、計數（35 分鐘）

教師提供第三週的行動規則、迴圈與方法骨架，先只完成 TODO **2→3→1**。每完成一項就編譯。F1 再完成 4、5。

- TODO 2：每步執行後把 command、energy、result 同時 add；拒絕、無效與返回也要保存。
- TODO 3：同一索引讀三條清單，格式 `step 1: command=1, result=OK, energy=14`。
- TODO 1：建立 int[4]，計算日誌中的 0–3 指令；越界指令不進 counts。

{3,9,0} 的逐步追蹤：

| 步 | 指令 | 預測結果/執行後能源 | 本人實際結果/能源 |
|---|---|---|---|
| 1 | 3 | ALREADY_FULL／20（已滿電，拒絕） | ALREADY_FULL／20 |
| 2 | 9 | UNKNOWN_COMMAND／20（無效指令） | UNKNOWN_COMMAND／20 |
| 3 | 0 | RETURN／20（返回，停止讀取） | RETURN／20 |

此時 commandLog/energyLog/resultLog 各有幾筆：各 3 筆。actionCount 是幾次：0 次。差異原因：日誌記錄每一筆讀到的指令，包含拒絕、無效與返回；actionCount 只計成功的行動，這三步都不是成功行動。

完成 F1 的摘要後，補齊六條本人測試。每列修改 main 固定腳本並重新編譯，初始值 20/0；將**完整輸出**貼在本表下方，包含回放、counts、摘要、最低能源、最終狀態與 battery。

| script | 預期狀態/結果 | 本人實際狀態/結果 | counts、摘要、每筆回放均相符？ |
|---|---|---|---|
| {0} | 20/0/0 RETURNED | 20/0/0 RETURNED | 相符：1 筆 RETURN，counts=[1, 0, 0, 0]，lowest=20 |
| {1,1,2} | 6/5/3 SUCCESS | 6/5/3 SUCCESS | 相符：14、8、6，counts=[0, 2, 1, 0]，ok=3 |
| {2,2,2,2,2} | 10/5/5 SUCCESS | 10/5/5 SUCCESS | 相符：18→10 共 5 筆，counts=[0, 0, 5, 0]，ok=5 |
| {3,9,0} | 20/0/0 RETURNED | 20/0/0 RETURNED | 相符：counts=[1, 0, 0, 1]（9 不計），ok=0、rejected=2 |
| {1,1,1,1} | 2/6/3 SUCCESS | 2/6/3 SUCCESS | 相符：只記 3 筆，ignored=1，lowest=2 |
| {2,1} | 12/3/2 UNFINISHED | 12/3/2 UNFINISHED | 相符：2 筆，ignored=0，腳本用完未返回 |

完整輸出：

```text
# script={0}
=== replay ===
step 1: command=0, result=RETURN, energy=20
counts[0..3]=[1, 0, 0, 0]
steps=1, ok=0, rejected=0, ignored=0
lowest energy=20
final energy=20, samples=0, actionCount=0 -> RETURNED
battery=100.0

# script={1, 1, 2}
=== replay ===
step 1: command=1, result=OK, energy=14
step 2: command=1, result=OK, energy=8
step 3: command=2, result=OK, energy=6
counts[0..3]=[0, 2, 1, 0]
steps=3, ok=3, rejected=0, ignored=0
lowest energy=6
final energy=6, samples=5, actionCount=3 -> SUCCESS
battery=30.0

# script={2, 2, 2, 2, 2}
=== replay ===
step 1: command=2, result=OK, energy=18
step 2: command=2, result=OK, energy=16
step 3: command=2, result=OK, energy=14
step 4: command=2, result=OK, energy=12
step 5: command=2, result=OK, energy=10
counts[0..3]=[0, 0, 5, 0]
steps=5, ok=5, rejected=0, ignored=0
lowest energy=10
final energy=10, samples=5, actionCount=5 -> SUCCESS
battery=50.0

# script={3, 9, 0}
=== replay ===
step 1: command=3, result=ALREADY_FULL, energy=20
step 2: command=9, result=UNKNOWN_COMMAND, energy=20
step 3: command=0, result=RETURN, energy=20
counts[0..3]=[1, 0, 0, 1]
steps=3, ok=0, rejected=2, ignored=0
lowest energy=20
final energy=20, samples=0, actionCount=0 -> RETURNED
battery=100.0

# script={1, 1, 1, 1}
=== replay ===
step 1: command=1, result=OK, energy=14
step 2: command=1, result=OK, energy=8
step 3: command=1, result=OK, energy=2
counts[0..3]=[0, 3, 0, 0]
steps=3, ok=3, rejected=0, ignored=1
lowest energy=2
final energy=2, samples=6, actionCount=3 -> SUCCESS
battery=10.0

# script={2, 1}
=== replay ===
step 1: command=2, result=OK, energy=18
step 2: command=1, result=OK, energy=12
counts[0..3]=[0, 1, 1, 0]
steps=2, ok=2, rejected=0, ignored=0
lowest energy=12
final energy=12, samples=3, actionCount=2 -> UNFINISHED
battery=60.0
```

繳交前恢復 {1,1,2}。課堂沒完成的路線，課後補齊本人實測。

## F｜摘要、需求與反例（30 分鐘）

### F1 Collections 與 TODO 4–5（10 分鐘）

先執行 CollectionsDemo，預測 min/max/frequency：6／20／1；實際：min=6、max=20、frequency(8)=1。排序後 original：[14, 8, 20, 6]；sorted：[6, 8, 14, 20]。為何不能只排序 energyLog 來做回放：排序會打亂時間順序；energyLog 和 commandLog、resultLog 靠相同索引配對，只排序 energyLog 會讓每一步配到錯誤的能源，回放就錯了。

- TODO 4：ok=frequency(resultLog,"OK")，returned=frequency(resultLog,"RETURN")；steps=size，rejected=steps-ok-returned，ignored=scriptLength-steps。
- TODO 5：isEmpty 為 true 時印 `lowest energy=N/A`；否則才用 min。
- 最低能源只看每步結束後的 energyLog，不含初始能源。

教師使用 FixedScriptMission 示範以下案例；學生填預測與觀察，不需另寫入口。未觀察請註明待補；自己另跑者再註明「本人複驗」。

| 初始 energy/samples；script | 預測結果/日誌筆數/最低能源 | 教師實際示範觀察 |
|---|---|---|
| 5/0；{1,3,0} | RETURNED／3 筆／最低 5（最終 10/0/1） | 【自填：教師示範觀察】AI 本機複驗：NOT_ENOUGH_ENERGY、OK、RETURN，3 筆，lowest=5，10/0/1 RETURNED |
| 2/4；{2} | SUCCESS／1 筆／最低 0（耗盡但成功優先） | 【自填：教師示範觀察】AI 本機複驗：1 筆 OK，lowest=0，0/5/1 SUCCESS |
| 0/0；{3,1} | EXHAUSTED／0 筆／N/A（不執行第一筆） | 【自填：教師示範觀察】AI 本機複驗：steps=0、ignored=2，lowest energy=N/A，EXHAUSTED |
| 20/5；{1} | SUCCESS／0 筆／N/A（一開始已達標） | 【自填：教師示範觀察】AI 本機複驗：steps=0、ignored=1，lowest energy=N/A，SUCCESS |
| 20/0；{} | UNFINISHED／0 筆／N/A（空腳本） | 【自填：教師示範觀察】AI 本機複驗：steps=0、ignored=0，lowest energy=N/A，UNFINISHED |

### F2 Prompt 設計（10 分鐘）

目的：把剛才實作的日誌轉成清楚的資料需求，作為判斷 AI 建議是否適合的依據。

先填下表，再自行組成可直接提問的一段 Prompt。接著用這份需求判讀下方兩個模擬建議。沿用原本的 MissionLog，本段產出為一段 Prompt 與兩項判斷；真實 AI 服務可選。

| 要交代的需求 | 本人的內容 |
|---|---|
| 欄位與型態 | 每一步三欄：command（int）、energy（int，行動後能源）、result（String，OK／NOT_ENOUGH_ENERGY／ALREADY_FULL／UNKNOWN_COMMAND／RETURN） |
| 筆數是否事先固定 | 不固定：依腳本長度與停止時機，停止後不再記錄；將來可能改回互動輸入，沒有上限 |
| 是否保留順序 | 要：回放必須依執行的時間順序，第 i 筆就是第 i 步 |
| 是否容許重複 | 要：同一指令、能源、結果都可能重複出現，例如連續探索 {1,1,2} |
| 新增、回放、計數與最低值等操作 | 每步尾端新增一筆（含拒絕、無效、返回）；依索引回放；counts 只計 0–3；frequency 算 OK 與 RETURN；最低能源只看行動後能源，空日誌印 N/A |
| 允許的技術與驗證案例 | 只用陣列、ArrayList、Collections；不寫新類別、不用 HashSet／Map；以 {0}、{1,1,2}、{2,2,2,2,2}、{3,9,0}、{1,1,1,1}、{2,1} 及空腳本 {} 驗證 |

完整 Prompt：「我在寫 Java 的火星探測車任務日誌。每執行一步要保存三個欄位：指令 command（int）、行動後能源 energy（int）、結果 result（String）。筆數事先不固定，要保留執行順序，也要允許重複；拒絕、無效指令與返回也要記錄。需要的操作有：每步新增、依索引回放 `step 1: command=1, result=OK, energy=14`、用 int[4] 計算 0–3 指令次數（其他整數不計）、用 Collections.frequency 計算 OK 與 RETURN、用 Collections.min 找最低能源（空日誌印 N/A）。只能用陣列、ArrayList 和 Collections，不要寫新類別。請說明你建議的資料結構，並用 {1,1,2}、{3,9,0}、{} 說明預期輸出。」

判讀下列**模擬建議**：

| 建議 | 採納/拒絕/補充條件與理由 |
|---|---|
| 歷程最多用 int[100] 保存就好 | 拒絕（除非補條件）：要另外記有效筆數，否則未使用的格子是 0，分不清是指令 0 還是空格；100 是任意上限，互動輸入時超過會越界，建議沒說怎麼處理；而且只有 int，不能存 result 字串。用 ArrayList 逐步 add，size() 直接得到筆數較合適 |
| ArrayList 保存日誌，HashSet 保存指令，避免重複 | ArrayList 保存日誌採納；HashSet 保存指令拒絕：Set 會去掉重複、不保證順序，{1,1,2} 只剩 {1,2}，無法回放，也無法用索引和 energyLog、resultLog 配對，次數也會算錯。任務本來就允許重複探索，順序與重複都是必要的 |

不要求實作 HashSet；依 List/Set/Map 的概觀判讀即可。使用固定腳本時筆數有上限，但日誌仍逐步成長且將來可能回到互動輸入；不能一概宣稱陣列無法記錄歷程。

### F3 模擬 AI 除錯二選一（10 分鐘）

先 commit 人工版，在副本測試，最後恢復正確版。

1. 刪掉 countCommands 的範圍檢查，理由「指令只有 0–3」；反例可用 {3,9,0}。
2. 把 printReplay 的 `< commandLog.size()` 改成 `<= commandLog.size()`，理由「最後一步也要印」；反例可用 {0}。

| 證據 | 本人紀錄 |
|---|---|
| 選哪種建議與反例；預期行為 | 選第 2 種：把 printReplay 的 `< commandLog.size()` 改成 `<=`。反例 {0}：應只印 `step 1: command=0, result=RETURN, energy=20`，再正常印 counts、摘要與最終狀態 |
| 修改前實際結果/錯誤訊息 | 副本印出 step 1 後丟出 `java.lang.IndexOutOfBoundsException: Index 1 out of bounds for length 1`，程式中止，後面的 counts、摘要、最終狀態都沒印 |
| 最小修正與採納/拒絕理由 | 拒絕此建議，改回 `i < commandLog.size()`。合法索引是 0 到 size()-1，`<=` 會多讀一個不存在的索引；最後一步本來就會印，步號用 i+1 顯示，不需要改條件 |
| 修正後反例實際結果 | {0}：step 1: command=0, result=RETURN, energy=20；counts=[1, 0, 0, 0]；steps=1, ok=0, rejected=0, ignored=0；20/0/0 RETURNED |
| 修正後 {1,1,2} | 6/5/3 SUCCESS，回放 14、8、6，見 E 的完整輸出 |
| 修正後 {3,9,0} | 20/0/0 RETURNED，ALREADY_FULL、UNKNOWN_COMMAND、RETURN，見 E 的完整輸出 |

若時間不足，課後補測；AI_USAGE 的驗證欄可引用本表，不必重抄。

## G｜課末檢核與第五週入口（5 分鐘）

1. length、size()、length() 分別用於什麼：length 是陣列的欄位（script.length，不加括號）；size() 是 ArrayList 的方法，取得目前筆數；length() 是 String 的方法，取得字元數。
2. 三筆日誌是否必定代表三次成功行動：不一定。日誌也記錄拒絕、無效與返回，例如 {3,9,0} 有 3 筆日誌，但成功行動是 0 次。
3. 三條清單漏加其中一筆會發生什麼：三條清單長度不一致，同一索引會配到不同步驟，回放顯示錯誤的能源；讀到最後一步時，較短的清單會越界，丟出 IndexOutOfBoundsException。
4. 為何固定四種指令計數用陣列，逐步增加的日誌用 ArrayList：指令種類固定 4 種，可直接用指令當索引放進 int[4]；日誌筆數事先不知道、會逐步增加，ArrayList 用 add 自動擴充，size() 就是筆數。
5. 下週如何把同一步三個欄位整合在一起：設計一個類別（例如 MissionStep），把 command、energy、result 放進同一個物件，再用一條 ArrayList 保存這些物件，就不會因漏加而對不上。本週不要求寫類別。

## H｜保存、推送與取回（15 分鐘）

- 恢復預設腳本，重新編譯執行；LAB_REPORT、AI_USAGE、專案 README 完成。
- commit、push，核對 GitHub main 最新 commit 與來源檔。只 commit 或 Changes 空白不代表已推送。
- 在另一位置 clone，同一專案重新編譯並跑 MissionLog；課堂不足可課後完成。

GitHub repo/作業目錄：https://github.com/TeddyWu1009/115_1_Java／2026_10_30；最終 commit：【自填：push 後的 commit 編號】。
本人新副本 JDK/編譯/預設輸出：【自填：在另一位置 clone 後，於 java-04 編譯並執行 MissionLog 的結果；未做則標示待補】。
推送若失敗先請教師協助，仍無法推送時保存完整 repo 至不會還原的位置，確認可讀並後續補推。

## 課後選做

- ScriptArgs：執行 `java -cp out ScriptArgs 3 9 0`，說明 args 的元素型態；非整數例外不列必做。
- 新增自訂路線，比較達標時剩餘能源與成功行動數，不要求最佳化演算法。
- 只顯示成功步驟時，以 if 跳過顯示；保留原日誌，驗證 counts 和摘要仍相同。
