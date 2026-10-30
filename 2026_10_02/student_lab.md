# 第二週實作｜火星探測車任務儀表板

作業的必做範圍、繳交清單與驗收標準見 [assignment.md](assignment.md)。本講義另存後作為實作與測試報告，不必再寫一份重複報告。

你是任務控制員。今天用固定資料判讀探測車狀態；下週才讓它接收指令與改變狀態。規則是教學遊戲設定，不是太空工程模型。

## 0. 取回與建立今日工作位置（10 分鐘，含診斷）

沿用學期 repo `115_1_Java`。沒有本機副本時用 GitHub Desktop **Clone repository**；已有副本時先保存並提交本機工作，再 **Fetch origin → 有更新才 Pull origin**。遇到衝突請教師協助，不強制覆蓋。

上課日期由教師指定，不把課綱日期區間當成確定日期。在 repo 內新增 `實際日期/java-02/src`（將「實際日期」替換成例如 `YYYY_MM_DD` 的真實上課日期）；不另建 Git repo。

- 複製 `starter/RoverDashboard.java`、兩支 `examples/*.java` 到 `src`。
- 本講義另存當日 `LAB_REPORT_WEEK02.md`，AI 範本另存 `AI_USAGE_WEEK02.md`。
- 沿用根 `.gitignore` 的 `*.class`、`out/`、`bin/` 規則。
- 下列指令都在 **當日 java-02 目錄**執行，先確認 `src` 存在。

```powershell
javac --release 21 -encoding UTF-8 -d out src/RoverDashboard.java src/TypeReferenceDemo.java src/OperatorDemo.java
java -cp out RoverDashboard
```

教學目標 JDK 21；若教師核准使用較新 JDK，以 `--release 21` 保持目標版本。編譯失敗先修正，不執行殘留 `.class`。

| 基本資料 | 填寫 |
|---|---|
| 姓名／上課日期／專案實際路徑 | 洪珩毅／2026-10-02／"C:\Users\m303\Desktop\115_1_Java\2026_10_02" |
| GitHub repo、取回的 commit | https://github.com/TeddyWu1009/115_1_Java ／ 取回 commit66cd250d91b1a180ebc9a99352b7165e97beb7b2 |
| `java -version`、`javac -version` | java version "24.0.1" 2025-04-15／javac 24.0.1；JDK 24 以 --release 21 編譯，保持目標版本 21 |

診斷：`double p = calculateBatteryPercent(7);` 的引數、參數、回傳值各是什麼？`return` 會自動顯示到畫面嗎？回答：待填。

## A. 為探測車選型（15 分鐘）

| 資料 | 選擇 Java 型態 | C# 對照／理由 |
|---|---|---|
| 名稱「赤砂號」 | C# 是 string（System.String 的別名） | Java 只有大寫的 String 類別，是參考型態，不是基本型態 |
| 區域 'A' | 	C# 也是 char | 單一字元用單引號 'A'，2 bytes 的 Unicode 字元 |
| 能源 7 格 | 	C# 也是 int | 能源是整數格數，0 至 20 用 int 最自然，也方便做整數除法與 % |
| 電量百分比 35.0 | double | 百分比有小數，用浮點數；Java 小數常值預設就是 double |
| 是否鎖定 false | bool | Java 寫 boolean，只能是 true／false，不能用 0／1 代替 |

1. Java 的八種基本型態：待填。哪個常用型態不在其中？byte、short、int、long、float、double、char、boolean。哪個常用型態不在其中？String。它是類別（參考型態），不是基本型態。。
2. Java 能寫 `bool locked = false;` 或 `string name = "赤砂號";` 嗎？修正：都不能。Java 沒有 bool 與小寫 string，修正為 boolean locked = false;、String name = "赤砂號";。
3. `char zone = 'A';` 與 `String zone = "A";` 的型態是否相同？不同。char 是基本型態，存一個字元，用單引號；String 是參考型態，指向字串物件，用雙引號，即使內容只有一個字也不是 char。。
4. `long distance = 3000000000L;` 與 `float sensor = 3.5F;` 的後綴用途：整數常值預設是 int，3000000000 超過 int 上限（約 21 億），加 L 才是 long 常值，否則編譯錯誤；小數常值預設是 double，指定給 float 會可能失去精度而編譯錯誤，加 F 表示 float 常值。

執行 `java -cp out TypeReferenceDemo`。八種型態變數的前兩行實際輸出：20 300 7 3000000000／3.5 35.0 A false。
## B. 值與參考追蹤（25 分鐘）

先讀 `TypeReferenceDemo`，先填預測，再執行觀察。

| 步驟 | energy | backupEnergy | 原因 |
|---|---|---|---|
| `int energy = 7; int backupEnergy = energy;` | 7 | 7 | int 是基本型態，指定時複製「值」，backupEnergy 得到自己的一份 7 |
| `energy = 6;` | 6 | 7| 只改 energy 這個變數的值；backupEnergy 是獨立的副本，不受影響（實際輸出 energy=6, backupEnergy=7） |

| 步驟 | consoleName 指向的字串內容 | logName 指向的字串內容 |
|---|---|---|
| `String consoleName = "赤砂號";` | 赤砂號 | 尚未宣告 |
| `String logName = consoleName;` | 赤砂號 | 	赤砂號 |
| `consoleName = "晨星號";` | 	晨星號 | 	赤砂號 |

畫出最後兩個變數與字串物件的箭頭：待填。這是重新指定參考，還是修改「赤砂號」物件？待填。能據此推論所有參考物件都不能修改嗎？待填。

密室門禁短題（最多 5 分鐘，已含在本節）：

```java
String code = "MARS";
String receivedCode = new String("MARS");
System.out.println(code == receivedCode);
System.out.println(code.equals(receivedCode));
```

| 檢查 | 預測 | 實際 | 比較的是什麼 |
|---|---|---|---|
| `code == receivedCode` |	false | false | 兩個變數是否指向同一個物件（參考位址）；new String 建立了另一個物件，所以不同 |
| `code.equals(receivedCode)` | 	true | 	true | 兩個字串的內容（字元序列）是否相同；都是 MARS，所以相同 |

教師用 `new String` 保證兩個不同字串參考，不要求自行設計類別。若兩個字串常值用 `==` 恰好得到 true，為何不能據此用 `==` 實作一般通行碼內容比較？待填。

`null` 表示參考未指向物件；本週字串題使用非 null 資料，先不加完整防護。

## C. 能源運算與條件（25 分鐘）

用 energy=7，先預測 `OperatorDemo`：

| 運算式 | 預測 | 實際 | 每步先算什麼／型態 |
|---|---|---|---|
| `energy / 20 * 100.0` | 0.0 | 0.0| 	先算 7 / 20，兩個 int 做整數除法得 0（int）；再 0 * 100.0 得 0.0（double） |
| `(double) (energy / 20) * 100` |0.0 | 0.0 | 括號先算 7 / 20 得 0（int）；轉成 double 是 0.0；再乘 100 得 0.0（double） |
| `(double) energy / 20 * 100` |35.0 | 35.0 |轉型優先：先把 7 轉成 7.0（double）；7.0 / 20 為浮點除法得 0.35；乘 100 得 35.0（double） |
| `energy / 20.0 * 100` | 35.0 | 35.0 |	20.0 是 double，7 / 20.0 為浮點除法得 0.35；乘 100 得 35.0（double） |
| `(int) 7.9` | 	7 | 	7 | 	double 轉 int 直接捨去小數部分（不是四捨五入），得 7（int） |
| `"合計=" + 2 + 3` | 	合計=23 | 	合計=23 | 由左至右："合計=" + 2 先變字串 合計=2，再接 3 得 合計=23（String） |
| `"合計=" + (2 + 3)` | 合計=5 | 合計=5 | 括號先算 2 + 3 得 5（int），再與字串串接得 合計=5（String） |

1. `int n=0; n+=2; n++; n--;` 最後 n：待填。`n++` 作獨立敘述有何用途？單獨一行時 n++ 與 n += 1、n = n + 1 效果相同，就是把 n 加 1，常用在計數。。
2. 能源介於 0 與 20（含兩端）的條件：energy >= 0 && energy <= 20。不要寫數學連寫 `0 <= energy <= 20`。
3. 能源至少 6 且未鎖定的條件：energy >= 6 && !locked。
4. 能源無效（小於 0 或大於 20）的條件：energy < 0 || energy > 20。

short-circuit evaluation 追蹤：`reportCheck()` 會印「右側檢查已執行」再回傳 true。

| 運算 | 右側方法會被呼叫嗎？ | 最後布林結果 | 實際輸出 |
|---|---|---|---|
| `false && reportCheck()` | 不會 | 	false | false（沒有印「右側檢查已執行」） |
| `true \|\| reportCheck()` | 不會 | 	true | true（沒有印「右側檢查已執行」） |
| `true && reportCheck()` | 會 | 	true | 先印 右側檢查已執行，再印 true |

實際執行：`java -cp out OperatorDemo`。`&&` 左側 false、`||` 左側 true 時，右側不求值；不只是在最後才比較結果。

手搖飲配料短題（最多 3 分鐘）：有 53 單位珍珠，每份用 8 單位，最多完整做幾份？剩多少？運算式與實測：份數 53 / 8 = 6，剩餘 53 % 8 = 5；實測輸出 配料份數=6，剩餘=5。

## D. 完成任務儀表板（25 分鐘）

電池容量 20；探索一次用 6 格。核心計算假設 energy 為 0 至 20。完成 starter 的五個 TODO，不加入 if、迴圈或輸入工具：

1. 跟做：完成 `calculateBatteryPercent(int energy)`，使用浮點除法，方法只回傳。
2. 獨立完成：完整探索次數、剩餘能源、有效範圍、探索資格。
3. 可改名稱與區域。只輸出資格，今天還不真正執行探索。
4. 保存人工初版；重新編譯，逐次修改 energy／locked 並重跑。

| energy | locked | 預測百分比／探索次數／餘量／資格 | 實際四個結果 | 通過／修正 |
|---|---|---|---|---|
| 0 | false | 	0.0／0／0／false| 	0.0／0／0／false | 通過 |
| 5 | false | 	25.0／0／5／false | 	25.0／0／5／false | 通過 |
| 6 | false | 	30.0／1／0／true | 	30.0／1／0／true |通過 |
| 7 | false | 	35.0／1／1／true | 	35.0／1／1／true | 通過 |
| 20 | false | 100.0／3／2／true | 100.0／3／2／true | 通過 |
| 6 | true | 30.0／1／0／false | 30.0／1／0／false| 通過 |

獨立範圍題：只評估 `energy >= 0 && energy <= 20`；分別代入 -1、0、20、21，結果：待填。-1、21 不屬於本週計算方法的輸入契約，不要求儀表板拒絕這些值。

## E. AI 建議反例與自主驗證（20 分鐘）

以下是教材自編的 AI 建議情境，並非真實服務對話：

```java
static double calculateBatteryPercent(int energy) {
    return energy / 20 * 100.0;
}
```

1. 對方說「回傳 double 所以一定有小數」，用 energy=7 解釋反例：energy / 20 兩邊都是 int，先做整數除法：7 / 20 得 0，小數部分已經被捨去；之後才乘 100.0 得 0.0。回傳型態是 double 只代表最後結果以 double 表示，不會補回之前被捨去的部分，所以得到 0.0 而不是 35.0。。
2. 為何只測 energy=0 和 20 可能漏掉問題？0 和 20 剛好都能整除：0 / 20 = 0 得 0.0，20 / 20 = 1 得 100.0，錯誤版本也會「答對」。只有 1 至 19 這種除不盡的值（例如 7）才會因整數除法被捨去而暴露錯誤，所以要測中間值。
3. 在測試副本重現錯誤，實際輸出與最小修正：副本改成 return energy / 20 * 100.0;，energy=7 實際輸出 能源：7；電量百分比：0.0。最小修正：把 20 改成 20.0，即 return energy / 20.0 * 100;，重跑得 35.0。。
4. 恢復正確程式，再跑 D 的六筆。證據位置：已恢復 return energy / 20.0 * 100;，六筆結果見本講義 D 的測試表，皆通過；AI 紀錄見 AI_USAGE_WEEK02.md。。
5. 用 AI_USAGE 範本記錄實際對話。沒有 AI 權限可自行解釋本情境，並明寫未使用服務；不捏造模型、回覆或測試。

## F. 課末檢核（15 分鐘）

不看教師答案完成：

1. energy=13、locked=false，百分比／探索次數／餘量／資格：65.0／2／1／true（13 / 20.0 * 100 = 65.0；13 / 6 = 2；13 % 6 = 1；13 >= 6 且未鎖定）。
2. 為何 `(double)(7 / 20)` 不能補回被捨去的部分？括號內 7 / 20 先做整數除法得 0，小數在這一步就已經捨去；之後轉成 double 只是把 0 變成 0.0，無法還原已遺失的 0.35。必須在除法前轉型，例如 (double) 7 / 20。。
3. `String a="A"; String b=a; a="B";`，b 內容與理由：b 是 "A"。b = a 讓 b 指向 a 當時指向的「A」物件；之後 a = "B" 只是讓 a 改指向另一個物件，b 不受影響，「A」物件也沒被修改。
4. 寫出「能源無效或車輛被鎖定」的條件：energy < 0 || energy > 20 || locked。
5. 用自己的話解釋方法回傳與 Console 輸出：方法回傳是把計算結果交回給呼叫它的程式，讓呼叫端存到變數或繼續使用，畫面上看不到；Console 輸出是用 System.out.println 把文字印到畫面給人看，不會把值交給其他程式。所以計算方法只負責回傳，由 main 決定要不要輸出。

同儕回饋：哪一筆測試最能揭露錯誤？為什麼？energy=7 那筆。0、5、6、20 剛好被 20 整除或結果為 0，錯誤的整數除法版本也會算對；7 / 20 會被捨成 0，最能揭露百分比的整數除法錯誤。另外 energy=6、locked=true 是唯一能揭露「忘記檢查鎖定」的測試。

## G. 保存、推送與繳交（15 分鐘）

- [ ] 當日專案 README 寫明用途、JDK、上述編譯指令、energy=7 的預期輸出。
- [ ] 本講義、AI 紀錄、原始碼已存檔；`.class` 和 out 未納入版本控制。
- [ ] Desktop 選對原本 repo/main，閱讀 Changes，Commit to main（例如 `week02: complete rover dashboard`）。
- [ ] Push origin 成功，再到 GitHub 核對最新 commit 與當日來源檔、紀錄。
- [ ] 繳交 repo 網址與最終 commit；若最後又修改紀錄，重新 commit/push。
- [ ] 無法 Push 時告知教師，備份完整 repo 至不會還原的位置；不要只留在還原磁碟。

| 保存證據 | 填寫 |
|---|---|
| 最終 commit／GitHub 連結 | 68a70a2a6327ed70b3203737a3413117609adc56/https://github.com/TeddyWu1009/115_1_Java/commit/68a70a2a6327ed70b3203737a3413117609adc56/2026_10_02 |
| 網站來源檔、紀錄與本機一致 | 一致：GitHub main 的 2026_10_02/java-02/src 三支程式、java-02/README.md、LAB_REPORT_WEEK02.md、AI_USAGE_WEEK02.md 與本機相同 |
| 尚未完成與後續事項 | 續事項	無 |

CLI 選讀：已有副本且工作已保存時，依第一週補充使用 `git pull --ff-only`；結束用 `git add`（明確檔案）、`git commit`、`git push`。命令失敗先確認路徑與歷史，不強制推送。

課後重現：將已推送 repo clone 到另一個不存在的檢查位置，在當日 java-02 重新編譯並執行；不要複製 out。記錄 clone commit 與輸出：待填。此步可依教師安排課後完成。

## 下週入口

你已會算「是否能探索」，下週用 if 讓條件決定行動，用 switch 選探索／掃描／充電，用迴圈完成任務。本週不要求 Scanner、陣列、集合或自行設計 Rover 類別。
