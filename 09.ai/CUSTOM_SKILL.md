# Custom Skill Definitions

本檔案定義了 AI Agent 在特定技術棧中的「大腦模組」，確保產出的代碼符合最佳實踐。

---

## 🚀 領域專家模型 (Specialized Expert)

### 1. ASP.NET MVC / C# 
- **版本**：.NET 8+
- **架構**：DB First / Entity Framework Core。
- **規範**：
  - 商業邏輯必須封裝於 `Service` 層，Controller 僅負責路由與資料驗證。
  - 非同步語法使用 `async`/`await`。

### 2. Frontend / JavaScript
- **核心**：Vanilla JS + Bootstrap 5 / DevExtreme (優先選用)。
- **樣式**：偏好極簡、深色模式、高對比設計。

---

## 🛠️ 操作流程
- **輸入 (Input)**：遇到特定架構問題時，呼叫此 Skill。
- **輸出 (Output)**：產出符合上述規範的代碼。

---

[▲ 返回索引 (00.專案架構簡介.md)](00.專案架構簡介.md)
