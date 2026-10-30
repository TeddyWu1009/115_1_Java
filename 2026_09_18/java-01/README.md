# java-01：HelloCourse 與 GreetingApp

物件導向程式設計第一週（2026_09_18）實作：用命令列與兩套 IDE 驗證 Java 執行環境，並把 C# WinForms 的 `BuildGreeting` 移植成不依賴 UI 的 Java 方法。

## 環境

- JDK 21
- VS Code＋Extension Pack for Java、IntelliJ IDEA（兩者開啟同一個 `java-01` 資料夾）
- 不使用 GUI 或外部函式庫

## 檔案

| 檔案 | 說明 |
|---|---|
| `src/HelloCourse.java` | 固定輸出兩行，用來比較不同工具的執行結果 |
| `src/GreetingApp.java` | `buildGreeting(name)` 回傳 `"Hello, " + name + "!"`，由 `main` 負責輸出 |

## 編譯與執行

在 `115_1_Java/2026_09_18/java-01` 目錄執行：

```powershell
javac -encoding UTF-8 -d out src/HelloCourse.java src/GreetingApp.java
java -cp out HelloCourse
java -cp out GreetingApp
```

- `-d out`：編譯產生的 `.class` 放在 `out`（已被根目錄 `.gitignore` 排除，不提交）
- `-cp out`：執行時從 `out` 尋找類別

## 預期輸出

`Hello, my Java course!`：

```text
Hello, my Java course!
Course: Object-Oriented Programming
```

`java -cp out GreetingApp`：

```text
Hello, Ada!
Hello, Grace!
Hello, Ada Lovelace!
```
