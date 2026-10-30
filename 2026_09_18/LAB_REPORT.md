# 第一週實作紀錄：C# WinForms 經驗遷移與 GitHub 版本控制

> 標示 **【自填】** 的欄位需要你本人的帳號、電腦或操作畫面，請在教室／自己電腦實際操作後填入，繳交前刪除這行提示。
> 命令列輸出與錯誤訊息是用 OpenJDK 21 實際跑出來的；Windows 中文環境下 `javac` 的錯誤訊息可能顯示成中文（例如「錯誤: 找不到符號」），請以你自己畫面上的為準。

| 基本資料 | 填寫 |
|---|---|
| 課堂暱稱／日期 | 洪珩毅／2026-09-18 |
| repo 本機根目錄 | C:\Users\m303\Desktop\115_1_Java\2026_09_18\java-01
| 當日專案相對路徑 | `2026_09_18/java-01` |
| GitHub repo 網址與初始 clone 版本 |https://github.com/TeddyWu1009/115_1_Java.git |
| 作業系統與 JDK 版本 |：Windows 11／JDK 21.0.x |

## A｜解釋並驗證執行環境

### A1. 連結已學過的 WinForms 經驗

1. `nameTextBox` 與 `resultLabel` 分別代表什麼？`Text` 是什麼？
   - 回答：`nameTextBox` 是表單上的 TextBox 控制項（讓使用者輸入姓名），`resultLabel` 是 Label 控制項（顯示結果）。兩者都是表單類別裡的欄位（控制項物件）。`Text` 是控制項的**屬性（property）**，型態是 `string`：讀取 `nameTextBox.Text` 取得使用者輸入的文字，設定 `resultLabel.Text` 改變畫面上顯示的文字。
2. 使用者尚未點擊按鈕時，已繫結的 Click 處理方法會自動執行嗎？WinForms 的程式起點是否就是這個方法？
   - 回答：不會。Click 事件處理方法只有在事件發生（使用者點擊按鈕）時才會被呼叫。它也不是程式起點；WinForms 的起點是 `Program.cs` 的 `Main()`，由 `Application.Run(new Form1())` 啟動表單與訊息迴圈，之後才依事件呼叫處理方法。Java 的對應起點是 `public static void main(String[] args)`。
3. 哪一段可以脫離畫面、用固定資料驗證？
   - 回答：`BuildGreeting(name)`。它只接收字串、回傳字串，不讀取也不修改任何控制項，所以可以直接用 `"Ada"`、`"Grace"`、`"Ada Lovelace"` 這類固定資料呼叫並比對回傳值。讀取 `nameTextBox.Text` 與寫入 `resultLabel.Text` 則依賴畫面。

### A2. 檢查命令列環境

```powershell
java -version
javac -version
where.exe java
where.exe javac
```

| 檢查 | 實際結果 | 判斷與處理 |
|---|---|---|
| `java` 主版本 | java version "24.0.1" 2025-04-15
Java(TM) SE Runtime Environment (build 24.0.1+9-30)
Java HotSpot(TM) 64-Bit Server VM (build 24.0.1+9-30, mixed mode, sharing) |
| `javac` 主版本 | javac 24.0.1|
| `java`／`javac` 實際路徑 | C:\Program Files\Common Files\Oracle\Java\javapath\java.exe
C:\Program Files (x86)\Common Files\Oracle\Java\java8path\java.exe

C:\Program Files\Java\jdk-24\bin\java.exe
C:\Program Files\Common Files\Oracle\Java\javapath\javac.exe
C:\Program Files\Java\jdk-24\bin\javac.exe|
 兩者應在同一個 JDK 的 `bin`；若 `where.exe` 列出多個，排在第一個的才是實際使用的 |
| GitHub Desktop 可開啟、登入且選對 repo |  GitHub Desktop 可開啟、登入且選對 repo | 可開啟；已登入帳號 TeddyWu1009；Current repository：115_1_Java；Current branch：main | 符合，不需處理  |

### A3. HelloCourse：命令列與兩套 IDE

```powershell
javac -encoding UTF-8 -d out src/HelloCourse.java
java -cp out HelloCourse
```

| 執行方式 | JDK | 實際程式輸出／證據 | 是否符合預期 |
|---|---|---|---|
| PowerShell | 21 | `Hello, Java!`／`Course: Object-Oriented Programming` | 符合 |
| VS Code | 【自填】Java: Configure Java Runtime 選定的 JDK 21 | 【自填】Terminal 兩行輸出（截圖位置） | 【自填】 |
| IntelliJ IDEA | 【自填】Project Structure → SDK 21 | 【自填】Run 視窗兩行輸出（截圖位置） | 【自填】 |

請解釋：IDE 啟動路徑不同，但程式輸出相同，是否正常？`javac` 與 `java` 各負責哪一段？

回答：正常。IDE 只是用不同方式（不同輸出資料夾、自動加上 classpath 參數）呼叫同一個 JDK 的編譯器與 JVM，原始碼相同、JDK 版本相同，程式輸出就應該相同；終端機上多出來的啟動指令或路徑不屬於程式輸出。`javac` 負責**編譯**：把 `.java` 原始碼檢查語法與型態後轉成 `.class` 位元組碼（編譯錯誤在這個階段出現）。`java` 負責**執行**：啟動 JVM，依 `-cp` 找到類別並從 `main` 開始執行（執行期錯誤與邏輯錯誤在這個階段出現）。

## B｜移植不依賴 UI 的方法

### B1. 完成後的方法

```java
static String buildGreeting(String name) {
    // 回傳 "Hello, " + 姓名 + "!"；此方法不負責輸出，由 main 決定如何呈現。
    return "Hello, " + name + "!";
}
```

```powershell
javac -encoding UTF-8 -d out src/HelloCourse.java src/GreetingApp.java
java -cp out GreetingApp
```

### B2. 先預測，再填實際結果

| 輸入 | 預期回傳值 | 實際輸出 | 是否通過／如何修正 |
|---|---|---|---|
| `Ada` | `Hello, Ada!` | `Hello, Ada!` | 通過（起始碼原本輸出 `TODO`，改為回傳字串串接後通過） |
| `Grace` | `Hello, Grace!` | `Hello, Grace!` | 通過 |
| `Ada Lovelace` | `Hello, Ada Lovelace!` | `Hello, Ada Lovelace!` | 通過，中間空白有保留 |

| 工具 | 三行輸出／截圖位置 | 與命令列結果是否一致 |
|---|---|---|
| VS Code | 【自填】 | 【自填】 |
| IntelliJ IDEA | 【自填】 | 【自填】 |

### B3. 追蹤方法呼叫

（中斷點設在 `String first = buildGreeting("Ada");`，Debug → Step Into → Step Over）

| 停下的位置 | 應觀察的資料 | 實際觀察 |
|---|---|---|
| 呼叫前 | 即將傳入的值 | 【自填確認】引數為 `"Ada"`；此時 `first` 尚未出現在 Variables |
| `buildGreeting` 內 | 參數 `name` | 【自填確認】`name = "Ada"` |
| 回到 `main` 且完成賦值後 | 變數 `first` | 【自填確認】`first = "Hello, Ada!"` |
| `println(first)` 執行後 | Console 輸出 | 【自填確認】Console 出現第一行 `Hello, Ada!` |

1. 方法命名 `BuildGreeting`／`buildGreeting` 是慣例差異；呼叫與宣告的大小寫不一致，是否仍可呼叫？
   - 不行。C# 慣例方法用 PascalCase，Java 慣例用 camelCase，但兩種語言的識別字都**區分大小寫**。宣告 `BuildGreeting` 卻呼叫 `buildGreeting`，編譯器會當成兩個不同名稱，回報找不到符號（見 C 實驗 2）。
2. C# `string` 能直接貼到 Java 使用嗎？
   - 不能。C# 的 `string` 是 `System.String` 的關鍵字別名；Java 沒有 `string` 這個關鍵字，字串類別是 `java.lang.String`（大寫 S）。寫 `string` 會出現 `cannot find symbol ... class string`（見 C 實驗 1）。
3. 本例把方法設為 `static`，是為了什麼？是否所有 Java 方法都應如此？
   - `main` 是 `static`，執行時沒有建立 `GreetingApp` 物件；`static` 方法屬於類別本身，所以 `main` 可以不用 `new` 物件就直接呼叫 `buildGreeting`。不是所有方法都應該 `static`：需要使用物件自己的狀態（欄位）的方法應該是實例方法，這正是之後物件導向要學的重點；這裡只是第一週簡化寫法。
4. 為何回傳字串比直接更新 `resultLabel.Text` 更容易用固定資料驗證？
   - 回傳值讓「計算」與「顯示」分開：可以直接比對 `buildGreeting("Ada")` 的回傳值是否等於 `"Hello, Ada!"`，不需要建立表單、控制項或模擬點擊。直接改 `resultLabel.Text` 會讓方法依賴 UI，要驗證就得開畫面或檢查控制項狀態。回傳字串之後也能用在命令列、UI 或測試等不同地方。

## C｜分類、重現與修正錯誤

| 實驗 | 預測結果 | 實際訊息／輸出 | 原因、最小修正及重跑結果 |
|---|---|---|---|
| 方法回傳型態的 `String` 改成 `string` | 編譯錯誤 | `src/GreetingApp.java:10: error: cannot find symbol`<br>`static string buildGreeting(String name) {`<br>`symbol: class string`<br>`1 error`（沒有產生 `.class`） | **編譯錯誤**。Java 沒有 `string` 型態。修正：改回 `String`。重跑後三行輸出正確。 |
| 宣告改 `BuildGreeting`，呼叫仍為 `buildGreeting` | 編譯錯誤 | 第 4、6、7 行各一個 `error: cannot find symbol`<br>`symbol: method buildGreeting(String)`<br>`3 errors` | **編譯錯誤**。識別字區分大小寫，三個呼叫處都找不到 `buildGreeting`。修正：宣告改回 `buildGreeting`（只改一處）。重跑後三行輸出正確。 |
| 方法回傳固定的 `Hello, Ada!` | 可編譯，但測試不全過 | 編譯成功，輸出：<br>`Hello, Ada!`<br>`Hello, Ada!`<br>`Hello, Ada!` | **邏輯錯誤**。只有 Ada 通過，Grace、Ada Lovelace 失敗，因為姓名被寫死、沒有使用參數 `name`。修正：改回 `return "Hello, " + name + "!";`。重跑後三筆都通過。 |

哪個問題可通過編譯，卻無法通過三筆需求測試？為什麼？

回答：第三個（回傳固定的 `"Hello, Ada!"`）。它語法與型態都正確——回傳值確實是 `String`——所以編譯器不會報錯；但編譯器只檢查「寫得合不合法」，不檢查「結果符不符合需求」。只用 Ada 一筆測試還會誤判成功，要用多筆不同資料逐行比對預期輸出才會發現。這也是為什麼「能按 Run」不等於完成。

結束本節前已恢復正確版本，三筆測試都通過。

## D｜在學期 repo 提交當日實作

### D1. README 與忽略規則

根目錄 `.gitignore`：

```gitignore
*.class
bin/
out/
.idea/
*.iml
```

- 為什麼 .java 要保存，out 與 .class 可以不保存？
  - `.java` 是人寫的原始碼，遺失就無法復原，必須版本控制。`out/` 與 `.class` 是 `javac` 編譯產生的，隨時能用 README 的指令重新產生；提交它們會讓每次編譯都出現無意義的差異，還可能和原始碼版本不一致。`.idea/`、`.iml` 是 IntelliJ 的個人設定，含本機路徑，IDE 可重建。
- repo 根 README 與 java-01/README.md 的用途有何不同？
  - 根 README 是整學期 repo 的說明與**日期索引**，讓人知道每次上課的內容放在哪個資料夾。`java-01/README.md` 是**單一專案**的說明：用途、JDK、編譯執行指令與預期輸出，讓別人（或下次 clone 後的自己）能照著重現結果。

### D2. 第一筆課堂提交與中途 Push

- History 中的本次 commit：【自填】`2026_09_18: add java-01 greeting baseline`（雜湊：_______）
- 包含哪些當日檔案？`.gitignore`、`README.md`、`2026_09_18/java-01/src/HelloCourse.java`、`2026_09_18/java-01/src/GreetingApp.java`、`2026_09_18/java-01/README.md`、`2026_09_18/LAB_REPORT.md`、`2026_09_18/AI_USAGE.md`（`out/` 未出現在 Changes，因為被 `.gitignore` 排除）【自填確認】
- Push 與 GitHub 核對結果：【自填】例：Push 成功，View on GitHub 看到 main 最新 commit 與上述檔案

### D3. 第二筆課堂提交

- 第二筆 commit 與實際修改：【自填雜湊】`2026_09_18: update course greeting`。`HelloCourse.java` 第一行由 `Hello, Java!` 改為 `Hello, my Java course!`；`java-01/README.md` 的 HelloCourse 預期輸出同步修改。
- 若取消勾選 README，這次 commit 是否包含它？原檔是否會消失？
  - 不包含，這筆 commit 只會有 `HelloCourse.java` 的修改。原檔不會消失，README 的修改仍留在工作目錄，Changes 會繼續顯示它，之後可以再勾選提交。
- 檔案已存檔，或只有 Commit 成功，為何不能直接關機？
  - 存檔只寫到本機硬碟，Commit 只是存成**本機**版本，兩者都還在還原卡的電腦上，關機後會被清除。必須 Push 到 GitHub，並在網頁上確認內容，才算真正保存。

## E｜檢查 GitHub 與演練下一次取回

### E1. 核對遠端已保存

| 核對項目 | 實際結果 |
|---|---|
| repo 網址／分支 | 【自填】`https://github.com/帳號/115_1_Java`／main |
| History 與 GitHub 最新 commit 是否一致 | 【自填】例：一致，皆為 `xxxxxxx` |
| 日期下的程式與紀錄是否齊全 | 【自填】例：`2026_09_18/java-01/src` 兩支程式、`java-01/README.md`、`LAB_REPORT.md`、`AI_USAGE.md` 皆為最新 |

### E2. 新位置 Clone，演練下次還原後取回

```powershell
javac -encoding UTF-8 -d out src/HelloCourse.java src/GreetingApp.java
java -cp out HelloCourse
java -cp out GreetingApp
```

- 此次 Clone 的 commit、實際路徑與兩支程式輸出：
  - commit：【自填】　路徑：【自填】例：`...\115_1_Java-check`
  - `HelloCourse`：`Hello, my Java course!`／`Course: Object-Oriented Programming`（修改後內容）
  - `GreetingApp`：`Hello, Ada!`／`Hello, Grace!`／`Hello, Ada Lovelace!`
- 下次剛 Clone 後為何要重新編譯？
  - `out/` 與 `.class` 被 `.gitignore` 排除，沒有上傳到 GitHub，clone 回來只有原始碼，必須重新用 `javac` 產生類別檔才能執行。這也確保執行的是目前原始碼，而不是舊的編譯結果。
- Clone 是否會帶回作者設定與 IDE 的 JDK 路徑？
  - 不會。作者 Name／Email 存在本機 Git 設定，不在 repo 裡；IDE 的 JDK 設定（`.idea/`、`.iml`、VS Code 的 Java Runtime 設定）是本機設定，且 `.idea/`、`.iml` 被忽略。還原後都要重新確認。

### E3. 每次上課與家中續作

- 下次日期／專案路徑：`2026_09_25/java-01/src`（依實際上課日；同日第二個專案用 `java-02`）
- Clone、Fetch、Pull 的差異：
  - **Clone**：本機還沒有 repo 時，把整個遠端 repo（含版本歷史）下載成新的本機資料夾，並自動設定 origin。
  - **Fetch**：只向遠端取得最新的提交資訊、更新遠端追蹤狀態，**不改動**本機工作中的檔案。
  - **Pull**：取得遠端修改並**整合**到目前本機分支，工作目錄的檔案會跟著更新。

### E4. 失敗處理

- 本次問題與保存方式：【自填】沒有遇到填「未遇到」

## F｜AI 審查、人工驗證與最終提交

### F1. AI 使用紀錄

詳見 [`AI_USAGE.md`](AI_USAGE.md)。

### F2. 即使畫面有文字，也要檢查回傳值

```java
static String buildGreeting(String name) {
    System.out.println("Hello, " + name + "!");
    return "";
}
```

1. 主程式執行 `System.out.println(buildGreeting("Ada"));`，包含空白行的輸出：

   ```text
   Hello, Ada!
   
   ```

   第一行 `Hello, Ada!` 是方法**內部**印出的；第二行是**空白行**，是 `main` 把回傳的空字串 `""` 交給 `println` 印出來的（只有換行）。

2. 實際回傳值是什麼？若 UI 顯示這個回傳值會得到什麼？
   - 回傳值是空字串 `""`。若 UI 寫成 `resultLabel.Text = buildGreeting(name);`，Label 會變成空白，什麼都不顯示；而 `println` 印的文字在 GUI 程式裡使用者根本看不到。
3. 為何此建議不符合 B 的需求？最小修正方向為何？
   - B 要求方法「回傳 `Hello, ` + 姓名 + `!`」且「方法內不做輸出」。這段兩點都違反：把結果印出來而不是回傳，回傳值錯誤，畫面看起來對只是巧合。最小修正：刪掉方法內的 `println`，把 `return "";` 改成 `return "Hello, " + name + "!";`，由 `main` 負責輸出。

實驗後已恢復符合需求的版本。

### F3. 下課關機前：完整保存

- [ ] 儲存所有檔案，將進度、測試結果與待辦寫入當日紀錄。
- [ ] 在 java-01 編譯並執行；未完成也須保存，標示錯誤與待辦。
- [ ] Desktop 選對原本的 `115_1_Java` 與 main，檢查 Changes、勾選所有必要檔案。
- [ ] Summary 填 `2026_09_18: save java-01 and learning records`，按 **Commit to main**。
- [ ] 按 **Push origin** 並等待成功。
- [ ] **Fetch origin** 後確認沒有待處理更新，History 與 GitHub main 最新 commit 一致，Changes 無遺漏。
- [ ] GitHub main 當日來源檔與紀錄齊全，最終 commit 連結填於繳交平台。
- [ ] 依教室規範登出 Desktop 與瀏覽器 GitHub；確認遠端保存後才關機。

## G｜課後延伸：Desktop 分支與 Pull Request

| 延伸紀錄 | 填寫（未做填「未做延伸」） |
|---|---|
| PR 網址與 base／compare | 未做延伸 |
| 修改目的、review 發現 | 未做延伸 |
| 是否已合併？本機 main 是否看得到文件？ | 未做延伸 |
| PR 與 Pull origin 有何不同？ | PR 是在 GitHub 上**請求**把一個分支的修改合併進另一個分支（例如 docs/verification → main），讓人審查後再合併，建立 PR 不等於已合併。Pull origin 是把**遠端已經存在**的提交下載並整合到本機目前分支。PR 合併後，本機 main 還要 Pull 才看得到結果。 |
