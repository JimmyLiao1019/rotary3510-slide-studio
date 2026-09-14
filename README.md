# 扶輪 3510 簡報工作室

可發布至 Sites 或 GitHub Pages 的簡報工作室，固定 15–20 頁，前 3 頁依序為封面、講師介紹、目錄。訪客可在瀏覽器完成編輯及 PPTX 匯出。

## 使用流程

1. 設定主題、對象、年度與頁數。
2. 到[第一波主題大綱](https://jimmyliao1019.github.io/rotary-first-wave-outlines/)選擇主題，下載該題 Markdown 講稿，將完整 8 頁內容貼入第 2 步並執行格式檢查。
3. 填寫講師資料，上傳照片並選擇拱形、方框或圓形合成圖。
4. 複製網站準備的製作指令，在使用者自己的 ChatGPT 帳號查核官方資料，將 8 頁大綱擴寫為設定的 15–20 頁，再把 JSON 結果匯入。
5. 逐頁選擇文字、圖文、比較表或流程圖版型，編輯內容與引用。
6. 儲存草稿；日後由「開啟草稿」重新載入。可另存副本。
7. 人工核對後匯出 PPTX。所有文字、表格及流程形狀保留可編輯性，照片和合成圖為影像。

## 品牌

固定使用使用者指定的 `web/assets/district-logo.png`，源自 `地區Logo.png`，保留原始位元組、比例、顏色，版面另外預留安全空間。依據使用者提供的 2019 年 12 月規範；不代表官方認證。

## 個人草稿

伺服器使用 Sites 的「使用 ChatGPT 登入」及 `oai-authenticated-user-id` 隔離草稿。R2 `BUCKET` 保存完整 JSON 文件及內嵌照片，使用 ETag 條件寫入避免不同視窗互相覆蓋。單份限制 16 MB。草稿不是以 localStorage 儲存。原始上傳照片與合成圖均會儲存。

GitHub Pages 為靜態網站，沒有 Sites 登入與 R2。該版本使用瀏覽器 IndexedDB 保存草稿及照片；資料只留在同一台裝置與同一瀏覽器，清除網站資料後也會移除。

## 使用自己的 ChatGPT 帳號

網站不需要也不接受共用的 `OPENAI_API_KEY`。文字研究與內容生成在訪客自己的 ChatGPT 工作階段完成，可用功能及用量依訪客的 ChatGPT 方案決定。

目前採用可立即公開使用的銜接流程：網站先驗證第一波講稿是否包含完整 8 頁，再把原稿、主題、對象、年度、頁數、品牌限制及版型要求組成製作指令。訪客複製至 ChatGPT，完成官方資料搜尋後，把固定 JSON 格式貼回網站。網站驗證內容頁數、欄位長度及 `rotary.org`、`rid3510.org` 官方來源，再建立簡報。

網站同時註冊 WebMCP 工具，可讓支援 WebMCP 的 ChatGPT 或 Codex 讀取目前頁面已貼入的 8 頁大綱、匯入結構化內容及檢查匯出前狀態。依 [OpenAI Site tools 文件](https://learn.chatgpt.com/docs/webmcp)，使用者須在 ChatGPT 桌面版的內建瀏覽器開啟網站，且功能是否可用仍取決於版本、模型、工作區與推出狀態。

官方文件：
- https://learn.chatgpt.com/docs/sites
- https://learn.chatgpt.com/docs/webmcp

## 維護

- `node scripts/build.mjs`：將 `web/` 資產及 Worker 打包至 `dist/server/index.js`，不含秘密或本機草稿。
- `node scripts/dev.mjs`：本機私人測試轉接器，自動配置連接埠；使用 `.local/bucket` 保存測試資料，不會部署。
- `node --test tests/worker.test.mjs`：測試草稿隔離、重開、版本衝突、公開狀態與 ChatGPT 登入身分。
- 上線依 Sites 流程推送同一提交、打包、儲存版本並發布。

已驗證 18 頁含合成圖、原始 Logo、原生表格與流程圖形的 PPTX 結構；未宣稱已在 PowerPoint 檢視。瀏覽器中的最後人工確認步驟留給使用者。
