# AI 使用紀錄（2026_09_18）

> 這份是依實際使用情形寫的；若你另外用了其他 AI 或做了不同的驗證，請照實修改。【自填】欄位填完後刪除這行。

| 欄位 | 內容 |
|---|---|
| 工具、日期、使用目的 | Claude（Anthropic，Claude Opus 5.5）；2026-10-30；請 AI 協助完成 `GreetingApp.java` 的 TODO、整理 README，並協助撰寫實作紀錄的概念題答案 |
| 主要 Prompt 摘要 | 上傳課程講義（student_lab.md、git_cli_reference.md、投影片）與起始碼 `HelloCourse.java`、`GreetingApp.java`、`.gitignore`，請 AI 依講義要求完成要繳交的內容。環境：JDK 21、傳統 class/main，不使用 GUI 或外部函式庫 |
| AI 回覆摘要 | ① `buildGreeting` 改為 `return "Hello, " + name + "!";`，不在方法內輸出、不加 trim 或驗證；② `HelloCourse` 第一行改為 `Hello, my Java course!`（D3）；③ 撰寫專案 README（指令與預期輸出）與根 README 日期索引；④ 實際編譯執行 C 節三個錯誤實驗與 F2 範例，提供真實錯誤訊息與輸出 |
| 採用／未採用原因 | 採用 `buildGreeting` 的寫法：它只用參數 `name` 組字串並回傳，符合 B 的四項需求（回傳而非輸出、保留空白、不加驗證、非空姓名假設）。與 F2 的錯誤建議對照：F2 版本在方法內 `println` 並回傳 `""`，畫面雖有文字但回傳值錯誤，因此不採用那種寫法 |
| 人工驗證方式與結果 | 【自填：請在自己電腦實際執行後確認】<br>指令：`javac -encoding UTF-8 -d out src/HelloCourse.java src/GreetingApp.java`、`java -cp out GreetingApp`<br>預期：`Hello, Ada!`／`Hello, Grace!`／`Hello, Ada Lovelace!`<br>實際：＿＿＿＿　是否一致：＿＿<br>另以 Debug 確認 `name = "Ada"`、`first = "Hello, Ada!"`：＿＿ |
| 最終修改與反思 | 修改：`GreetingApp.java` 第 13 行由 `return "TODO";` 改為字串串接；`HelloCourse.java` 第 4 行輸出文字。<br>學到：C# `string` 在 Java 要寫 `String`；Java 方法慣例 camelCase 且大小寫敏感；「能編譯、能執行」不代表符合需求，要用多筆資料比對回傳值；`println` 印出來的不等於回傳值。<br>仍有疑問：【自填】 |
