# java-02：火星探測車任務儀表板

物件導向程式設計第二週（2026_10_02）實作：用固定資料計算探測車的電量百分比、完整探索次數、剩餘能源、能源是否有效與能否探索。只計算並顯示，不使用 Scanner、if 或迴圈。

## 環境

- Oracle JDK 24.0.1，以 `--release 21` 編譯，目標版本 JDK 21
- 不使用 GUI 或外部函式庫

## 檔案

| 檔案 | 說明 |
|---|---|
| `src/RoverDashboard.java` | 作業程式：完成五個 TODO |
| `src/TypeReferenceDemo.java` | 課堂範例：八種基本型態、值與參考 |
| `src/OperatorDemo.java` | 課堂範例：整數除法、轉型、字串串接、短路求值 |

## 編譯與執行

在 `115_1_Java/2026_10_02/java-02` 目錄執行：

```powershell
javac --release 21 -encoding UTF-8 -d out src/*.java
java -cp out RoverDashboard
```

`out/` 與 `.class` 已被根目錄 `.gitignore` 排除，不提交。

## 預期輸出（energy=7、locked=false）

```text
探測車：赤砂號；區域：A
能源：7；電量百分比：35.0
完整探索次數：1；剩餘能源：1
有效能源：true；能否探索：true
```
