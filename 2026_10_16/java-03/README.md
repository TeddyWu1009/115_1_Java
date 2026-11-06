# java-03：火星探測車互動任務

物件導向程式設計第三週（2026_10_16）實作：用 Scanner 讀取整數指令，以 switch 選擇探索、掃描、充電或返回，用 while 迴圈持續任務，直到返回、能源耗盡或樣本達標。

## 環境

- Oracle JDK 24.0.1，以 `--release 21` 編譯，目標版本 JDK 21
- 不使用 GUI 或外部函式庫

## 檔案

| 檔案 | 說明 |
|---|---|
| `src/RoverMission.java` | 作業程式：完成 D1 探索、D2 掃描、D3 充電 |
| `src/EnergyStatus.java` | 課堂範例：if／else if 能源分類 |
| `src/SingleAction.java` | 課堂範例：switch 單次行動 |
| `src/ChargeForecast.java` | 課堂範例：for 固定三輪充電 |
| `src/LoopComparison.java` | 課堂範例：while 與 do-while |

## 指令規則

| 指令 | 規則 |
|---|---|
| 1 探索 | 能源至少 6：扣 6、樣本 +2、行動次數 +1；不足印 `NOT_ENOUGH_ENERGY` |
| 2 掃描 | 能源至少 2：扣 2、樣本 +1、行動次數 +1；不足印 `NOT_ENOUGH_ENERGY` |
| 3 充電 | 未滿 20：加 5、最多 20、行動次數 +1；滿電印 `ALREADY_FULL` |
| 0 返回 | 結束任務 |
| 其他 | 印 `UNKNOWN_COMMAND`，狀態不變 |

## 編譯與執行

在 `115_1_Java/2026_10_16/java-03` 目錄執行：

```powershell
javac --release 21 -encoding UTF-8 -d out src/*.java
java -cp out RoverMission
```

執行後每次輸入一個整數再按 Enter。

## 預期結果（成功路線：輸入 1、1、2）

```text
energy=6, samples=5, actionCount=3, battery=30.0
SUCCESS
```
