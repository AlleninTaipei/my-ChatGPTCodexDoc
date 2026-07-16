# ChatGPT 與 Codex 文件中心

以繁體中文重新編寫的 ChatGPT 與 Codex 官方文件, 轉換成六份可互動式學習的單頁 HTML 課程, 並提供一個統一的入口首頁。

- Repository: [github.com/AlleninTaipei/my-ChatGPTCodexDoc](https://github.com/AlleninTaipei/my-ChatGPTCodexDoc)
- 資料來源: [learn.chatgpt.com/docs](https://learn.chatgpt.com/docs) (OpenAI 官方文件)
- 製作日期: 2026 年 7 月

## 線上瀏覽

啟用 GitHub Pages 後, 可透過以下網址瀏覽:

`https://AlleninTaipei.github.io/my-ChatGPTCodexDoc/`

若尚未啟用, 請參考下方 "發布到 GitHub Pages" 一節。

## 內容一覽

首頁 `index.html` 提供六份課程的入口卡片, 每份課程都是自成一體的單一 HTML 檔案 (內嵌全部 CSS 與 JavaScript, 不依賴外部資源), 支援亮色 / 暗色主題切換。

| 課程 | 章節數 | 主題 |
|---|---|---|
| [ChatGPT.html](ChatGPT.html) | 20 | 快速入門, 提示技巧, 模型, 權限, 定價, 詞彙表 |
| [Features.html](Features.html) | 21 | 專案任務, 瀏覽器, Computer Use, 排程任務, 檔案處理, 系統設定 |
| [Administration.html](Administration.html) | 18 | 身分驗證, 角色權限, 受管理設定, 法規遵循, 稽核事件, 企業佈署 |
| [Configuration.html](Configuration.html) | 17 | config.toml / requirements.toml 參考, AGENTS.md, MCP, Windows, WSL |
| [Developers.html](Developers.html) | 22 | Code Review, Hooks, 執行環境, Codex SDK, App Server, 第三方整合 |
| [Security.html](Security.html) | 18 | 權限沙箱, 自動審查, Codex Security 掃描, 弱點修復追蹤, 威脅模型 |

每份課程對應同名的 Markdown 來源檔案 (例如 `ChatGPT.md` 對應 `ChatGPT.html`), 為官方文件的原始彙整版本, 供比對或後續更新使用。

## 目錄結構

```
.
├── index.html              入口首頁
├── ChatGPT.html            課程 01, 對應 ChatGPT.md
├── Features.html           課程 02, 對應 Features.md
├── Administration.html     課程 03, 對應 Administration.md
├── Configuration.html      課程 04, 對應 Configuration.md
├── Developers.html         課程 05, 對應 Developers.md
├── Security.html           課程 06, 對應 Security.md
├── docs/                   117 個原始碎片檔案, 依官方文件單一主題拆分, 為六份彙整 md 的來源素材
└── .claude/                Claude Code 本機設定, 與網站功能無關 (settings.local.json 已由 .gitignore 排除)
```

## 本機瀏覽

直接用瀏覽器開啟 `index.html` 即可, 不需要安裝任何相依套件或啟動伺服器。

## 發布到 GitHub Pages

1. 將本儲存庫推送到 GitHub。
2. 進入 Settings > Pages。
3. Source 選擇 Deploy from a branch, Branch 選擇 `main`, 資料夾選擇 `/ (root)`。
4. 儲存後等待數分鐘, 即可透過 `https://<你的帳號>.github.io/<儲存庫名稱>/` 存取。

## 說明

內容由官方 ChatGPT 與 Codex 文件翻譯並重新編排為互動式課程, 程式碼, 指令, 模型名稱與檔案路徑等技術用語維持原文, 其餘說明文字改寫為繁體中文。部分篇幅過大的參考資料表格 (例如完整的 config.toml 鍵值清單) 於課程內以可搜尋表格呈現代表性項目, 並在對應章節註明已做摘要處理, 如需完整內容請查閱 `docs/` 或對應的來源 `.md` 檔案。

## 資料來源與版權

本專案內容編譯並翻譯自 OpenAI 官方文件 [learn.chatgpt.com/docs](https://learn.chatgpt.com/docs), 原始文件內容版權所有 © OpenAI。

本儲存庫為非官方之繁體中文互動式整理與學習用途, 與 OpenAI 無關聯, 亦非其官方產品或服務, 內容如與官方文件有出入, 請以 OpenAI 官方文件為準。
