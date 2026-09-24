# AI 智慧測試資料自動產製工具 (Dify Smart Data Generator)

本專案是一個基於 **Dify 工作流** 與 **大型語言模型 (LLM)** 的自動化測試資料生成工具。主要為了解決軟體開發與測試階段中，手動建立合規、大量且具備複雜業務約束測試資料的痛點，**有效節省團隊約 50% 的測資準備時間**。

---

## 核心特色與技術亮點

- **多情境輸入解析（4-Branch Workflow）**：
  支援四種不同輸入維度，系統自動分支處理：
  1. `Both Files`：同時提供資料表 Schema 與外部代碼對照表。
  2. `Reference Only`：僅提供代碼表，由 LLM 自動反推欄位骨架與資料型態。
  3. `Schema Only`：僅提供 Schema 規格，依循欄位常識自動生成數據。
  4. `No File`：無規格輸入，全自動依語境推斷生成。

- **Token 限制突破（Context Window Optimization）**：
  針對 LLM 輸入字元上限瓶頸，設計「表頭 + 樣本特徵萃取節點（mini_text）」，大幅減少 Token 消耗，使工作流能穩定批量生成上千筆資料。

- **業務邏輯防呆與資料完整性約束**：
  - **主鍵去重**：內建複合主鍵 (Composite PK) 唯一性校驗，避免資料碰撞。
  - **條件破壞模式**：支援設定資料正確率（如 90% 正確、10% 格式異常測試數據），精準測試系統邊界條件。
  - **例外重試**：於 System Prompt 與 API 端實作限流 (Rate Limit 429) 與逾時 (502/504) 容錯重試機制。

- **個資自動去敏化 (Data De-identification)**：
  內建遮罩演算法，針對身分證號碼（故意破壞最後一碼校驗碼避免真實個資外洩）、手機號碼（中間 4 碼遮罩）及機敏欄位進行自動脫敏，確保符合資安合規規範。

---

## 技術架構與工具

- **工作流平台**：Dify
- **語言模型**：LLM (Prompt Engineering / Few-Shot Prompting / System Prompt 分層約束)
- **資料處理**：Python (資料格式化、特徵抽取與 CSV/SQL 匯出)
- **支援輸出格式**：CSV、SQL INSERT Scripts

---

## 快速開始

1. 下載 `workflow/dsl_workflow.yml`。
2. 進入 Dify 控制台，點選「建立空白 App」旁邊的「從 DSL 檔案匯入」。
3. 設定您的 LLM API Key（如 OpenAI / Gemini）。
4. 點擊「執行」，輸入您的 Table Schema 即可開始生成測試數據。
