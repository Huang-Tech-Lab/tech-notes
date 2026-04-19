# Test & Quality Guidelines

品管主管 (TDD) 的核心軍律，確保每一行進入生產環境的代碼都是穩健且可預期的。

---

## 🧪 測試策略 (Testing Strategy)

### 1. 測試驅動開發 (TDD) 循環
1. **RED**：先寫一個會失敗的測試（定義功能邊界）。
2. **GREEN**：實作最簡單的代碼使測試通過。
3. **REFACTOR**：在不改變行為的前提下優化結構。

### 2. 品質大門 (Quality Gates)
- 所有 PR 必須伴隨單元測試或整合測試。
- 嚴格禁止直接修改核心邏輯而不更新對應測試。

---

## 🛠️ 工具鏈
- **Backend**：xUnit / NUnit (ASP.NET Core)。
- **Frontend**：Jest / Playwright。

---

[▲ 返回索引 (00.專案架構簡介.md)](00.專案架構簡介.md)
