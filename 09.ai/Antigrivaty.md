# Antigravity AI 指令指南

## 目錄
- [核心指令表](#核心指令表)
- [/compact - 對話壓縮](#compact---對話壓縮)
- [/forget - 遺忘特定上下文](#forget---遺忘特定上下文)
- [/clear - 清除對話](#clear---清除對話)
- [/stats - 系統狀態](#stats---系統狀態)
- [/token - Token 查詢](#token---token-查詢)
- [最佳實踐](#最佳實踐)
- [參考資料](#參考資料)

---

## 核心指令表

| 指令 | 功能 | 適用場景 | 詳細說明 |
| :--- | :--- | :--- | :--- |
| `/compact` | 對話壓縮，總結重點並清理冗餘 | 對話變長、反應變慢時 | [查看詳情](#compact---對話壓縮) |
| `/stats` | 顯示 Token、Context、檔案清單 | 掌握配額進度時 | [查看詳情](#stats---系統狀態) |
| `/clear` | 清除對話，保留專案層級 Context | 開啟全新主題時 | [查看詳情](#clear---清除對話) |
| `/forget` | 遺忘特定檔案或對話片段 | 代碼架構大改時 | [查看詳情](#forget---遺忘特定上下文) |
| `/token` | 查詢當前對話的 Token 消耗 | 評估任務成本時 | [查看詳情](#token---token-查詢) |

[▲ 返回目錄](#目錄)

---

## /compact - 對話壓縮

### 作用
優化對話長度而不遺忘重點內容。

### 執行方式
```bash
/compact
```

### 原理
1. **資訊過濾** - 移除無意義的回應（打招呼、廢話、失敗嘗試）
2. **知識蒸餾** - 將 10-20 回合的重要結論總結為核心內容
3. **重新加載** - 將精簡版總結注入 Context

### 效益
- 減少延遲：縮短記憶長度，提高回應速度
- 降低成本：節省無謂的 Token 支出
- 強化記憶：避免翻找細節導致的誤解

### 輔助系統 Skills (Technical Support)
當您需要更精確或具備「戰略性」的壓縮時，可以調用以下底層 Skills：

| Skill 名稱 | 具體功能 |
| :--- | :--- |
| [cc-skill-strategic-compact](#cc-skill-strategic-compact) | **戰略性壓縮**：專注於開發進度、技術難點與未來計畫的關鍵摘要。 |
| [context-management-context-save](#context-management-context-save) | **智能保存**：將目前語義序列化存檔，支援 JSON/Markdown，實現跨 Session 移轉。 |
| [context-management-context-restore](#context-management-context-restore) | **上下文恢復**：在新對話中快速由「存檔檔案」讀取並重建開發脈絡。 |
| [context-compression](#context-compression) | **語義壓縮**：移除 Logs 或重複文字，優化 Token 密度而不遺漏邏輯。 |

---

<a name="cc-skill-strategic-compact"></a>
#### cc-skill-strategic-compact
*   **啟動方式**：自然語言意圖啟動。
*   **使用情境**：開發遇到瓶頸或暫停時，用於提取技術關鍵點。
*   **指令範例**：「請使用 strategic compact 摘要目前的開發進度。」

<a name="context-management-context-save"></a>
#### context-management-context-save
*   **啟動方式**：結構化指令 / 自然語言。
*   **使用情境**：結束會話前，序列化目前心智模型。
*   **參數範例**：`context_save(project_root=".", format="json")`。

<a name="context-management-context-restore"></a>
#### context-management-context-restore
*   **啟動方式**：結構化指令。
*   **使用情境**：新會話開始時，從 `.context/` 檔案恢復。
*   **參數範例**：`context_restore(source=".context/snapshot.json")`。

<a name="context-compression"></a>
#### context-compression
*   **啟動方式**：指令 `/compact` 底層調用或手動觸發。
*   **使用情境**：清理 Token 冗餘（如編譯 Error Logs）。
*   **指令範例**：「請執行語義壓縮，移除歷史回應中的無效 Logs。」

---

### 最佳實踐
1. **階段結案**：每完成一個小功能就執行 `/compact`。
2. **備份總結**：將結果存入檔案（如 `AIAgent使用.md`）
3. **搭配 `/clear`**：
   - 先 `/compact` → 存檔 → `/clear` → AI 像新的一樣快，但讀檔後立即接上進度

[▲ 返回目錄](#目錄)

---

## /forget - 遺忘特定上下文

### 執行方式

```bash
# 忘記特定檔案
/forget d:\PS1\VulnerabilityScanner.ps1

# 忘記最後 10 回合
/forget last 10 turns

# 忘記所有訊息
/forget all messages

# 忘記特定主題
/forget "關於 SQL 叢集的舊理解"
```

### 使用時機
1. **代碼大規模變動** - 架構修正後避免舊記憶干擾
2. **清理 Token** - 已有總結文檔時
3. **錯誤修正** - 修正錯誤理解時

[▲ 返回目錄](#目錄)

---

## /clear - 清除對話

### 執行方式
```bash
/clear
```

### 清除 vs 保留

| 清除 ❌ | 保留 ✅ |
|--------|---------|
| 對話內容 | 專案檔案 |
| 短期上下文 | 系統層級指令 |
| | 自動讀取機制 |

### 使用時機
1. 對話累積過長，AI 變慢或產生幻覺
2. 開啟全新主題，不想受舊對話干擾
3. 多人環境下保護隱私

### 專業操作流
1. `/compact` → 取得總結
2. 存檔至 `current_status.md`
3. `/clear` → 重啟系統
4. 重啟後說：「請查看 `current_status.md` 並繼續」

[▲ 返回目錄](#目錄)

---

## /stats - 系統狀態

### 執行方式
```bash
/stats
```

### 回傳資訊

| 類別 | 內容 |
| :--- | :--- |
| **專案索引** | 讀取的檔案、Context 佔用率 |
| **Token 統計** | 總消耗、最後一回消耗、剩餘空間 |
| **工作流狀態** | Conductor 狀態、背景任務 |

### 判斷標準
- Context > 80% → 下 `/compact`
- Token 不足 → 下 `/clear`
- 沒看到檔案 → 手動標註 `@[檔案路徑]` 重新讀取

[▲ 返回目錄](#目錄)

---

## /token - Token 查詢

### 執行方式
```bash
/token
```

### 回傳資訊

| 項目 | 說明 |
| :--- | :--- |
| Input Tokens | 輸入的文字、讀取的檔案、對話記憶 |
| Output Tokens | 本次回應產生的字數 |
| Total Consumed | Session 總累積消耗 |
| Context Limit | 最大記憶容量（如 128k/200k） |

### 使用時機
1. **開發前**：確保空間充足
2. **開發中**：反應變慢時確認佔用
3. **開發後**：清理環境讓下一任務從 0 開始

[▲ 返回目錄](#目錄)

---

## 最佳實踐

### 指令組合

| 狀況 | 動作 |
| :--- | :--- |
| 想看系統多擠 | `/stats` |
| 想瘦身提速但保留記憶 | `/compact` |
| 想徹底清空重啟 | `/clear` |

### 手動優化 Context
1. 關閉不需要的 IDE 分頁
2. 維護 `.gitignore` 排除編譯產出
3. 使用 `task.md` 將進度寫入磁碟

### 專家流程
```bash
開發前 → /token 確認空間
開發中 → /stats 監控 + 必要時 /compact
開發後 → /compact 總結 → 存檔 → /clear
```

### 🛠️ 單次消耗查看
在大多數的 IDE 介面中，每次回答完畢，您可以在對話框的**右下角**或**訊息氣泡邊緣**看到灰色微小數字（例如 `1.2k / 15k`），那就是該回合的精確 Token 統計。

[▲ 返回目錄](#目錄)

---

## 參考資料

- [Antigravity AI 官方文件](https://docs.antigravity.ai)
- [Conductor 實作指南](實戰小技巧：Conductor.md)

[▲ 返回目錄](#目錄)