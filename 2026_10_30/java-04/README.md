# java-04：火星探測車任務日誌

物件導向程式設計第四週（2026_10_30）實作：用固定腳本 `int[] script` 執行任務，以三條 ArrayList 逐步保存指令、行動後能源與結果，再回放每一步、統計指令次數、摘要與最低能源。

## 環境

- Oracle JDK 24.0.1
- 不使用 GUI 或外部函式庫

## 檔案

| 檔案 | 說明 |
|---|---|
| `src/MissionLog.java` | 作業程式：完成 TODO 1–5 |
| `src/ArrayBasics.java` | 課堂範例：陣列長度、預設值、for 走訪 |
| `src/CommandCounter.java` | 課堂範例：0–3 指令計數 |
| `src/ArrayReference.java` | 課堂範例：陣列參考與方法 |
| `src/ListBasics.java` | 課堂範例：ArrayList 基本操作 |
| `src/CollectionsDemo.java` | 課堂範例：min／max／frequency／sort |
| `src/ScriptArgs.java` | 選讀：命令列參數轉 int[] |

## 編譯與執行

在 `115_1_Java/2026_10_30/java-04` 目錄執行：

```powershell
javac -encoding UTF-8 -d out src/*.java
java -cp out MissionLog
```

要測其他路線，修改 `MissionLog.main` 的 `int[] script`，重新編譯後執行；繳交版為 `{1, 1, 2}`。

## 預期輸出（script = {1, 1, 2}）

```text
=== replay ===
step 1: command=1, result=OK, energy=14
step 2: command=1, result=OK, energy=8
step 3: command=2, result=OK, energy=6
counts[0..3]=[0, 2, 1, 0]
steps=3, ok=3, rejected=0, ignored=0
lowest energy=6
final energy=6, samples=5, actionCount=3 -> SUCCESS
battery=30.0
```
