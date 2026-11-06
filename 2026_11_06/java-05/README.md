# java-05：把任務日誌設計成物件

物件導向程式設計第五週（2026_11_06）實作：把第四週三條清單整合成 `MissionStep` 物件（command、energy、result），以一條 `ArrayList<MissionStep>` 保存歷史；行動規則與輸出和第四週相同。

## 環境

- Oracle JDK 24.0.1
- 不使用 GUI 或外部函式庫

## 檔案

| 檔案 | 說明 |
|---|---|
| `src/MissionStep.java` | 作業程式：建構子、三個 getter、`isSuccessful()`（TODO 1–3） |
| `src/MissionLog.java` | 作業程式：新增記錄、回放、OK／RETURN 計數（TODO 4–6） |
| `src/StepCard.java`、`src/CreateStepDemo.java` | 課堂範例：類別、建構子、getter |
| `src/RoverBattery.java`、`src/BatteryDemo.java` | 課堂範例：封裝 |
| `src/ReferenceDemo.java` | 課堂範例：共用參考與傳值 |
| `src/SnapshotDemo.java` | 課堂範例：每步建立新快照 |

## 編譯與執行

在 `115_1_Java/2026_11_06/java-05` 目錄執行：

```powershell
javac -encoding UTF-8 -d out src/*.java
java -cp out MissionLog
```

範例入口：`CreateStepDemo`、`BatteryDemo`、`ReferenceDemo`、`SnapshotDemo`。

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
