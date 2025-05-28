# ICDMap 資料庫說明

本專案內含兩個 SQLite 資料庫，用於執行離線的 ICD 編碼轉換與死因分類。

---

## 📘 `icd_mapping.db`

表格名稱：`icd_mapping`  
用於 **ICD-9 ↔ ICD-10** 的相互轉換與中英文描述查詢。

| 欄位名稱        | 說明                      |
|----------------|---------------------------|
| `ICD-9`        | ICD-9 編碼                |
| `ICD-9 English`| ICD-9 對應英文診斷名稱   |
| `ICD-9 Chinese`| ICD-9 對應中文診斷名稱   |
| `ICD-10`       | 對應的 ICD-10 編碼       |
| `ICD-10 English`| ICD-10 英文診斷名稱     |
| `ICD-10 Chinese`| ICD-10 中文診斷名稱     |
| `Mapping Code` | 映射代碼，代表轉換類型   |

---

## 📕 `death_cause_mapping.db`

表格名稱：`death_cause_mapping`  
用於將 ICD 或 CAUSE code 映射為主要死因分類（中英文）。

| 欄位名稱        | 說明                             |
|----------------|----------------------------------|
| `CAUSE`        | 死因類別編號                     |
| `ICD9_RANGE`   | 對應的 ICD-9 範圍（可為多段）    |
| `ICD10_RANGE`  | 對應的 ICD-10 範圍               |
| `Cause_Chinese`| 死因分類中文名稱                 |
| `Cause_English`| 死因分類英文名稱                 |

---
