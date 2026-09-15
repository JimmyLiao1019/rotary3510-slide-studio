# 扶輪 3510 簡報工作室

可發布至 Sites 或 GitHub Pages 的圖片式簡報工作室。每份簡報固定 11 頁：封面、講師介紹、目錄，以及 Markdown 講稿中的 8 頁正式內容。完成後可預覽並下載 PDF。

## 使用流程

1. 輸入主題、分享對象與扶輪年度；頁數固定，不提供頁數選項。
2. 到[第一波主題大綱](https://jimmyliao1019.github.io/rotary-first-wave-outlines/)下載單一 `.md` 講稿，再於第 2 步上傳。網站會檢查檔案是否完整包含 8 頁。
3. 填寫講師姓名、職稱／所屬扶輪社與簡介，並上傳一張已取得使用同意的講師照片。
4. 網站把講稿、講師資料、品牌限制與每頁視覺方向整理成 11 頁 GPT Images 生圖任務。按「一鍵開始全部產圖」可準備批次工作；ChatGPT 桌面版內建瀏覽器中的代理程式可逐頁生圖並透過 WebMCP 自動回填，一般瀏覽器則會複製指令並開啟 ChatGPT。
5. 網站把原始文字、真實講師照片與官方地區 Logo 疊加在 AI 背景上，顯示 11 頁 16:9 圖片預覽。
6. 儲存草稿；日後由「開啟草稿」重新載入。可另存副本。
7. 逐頁人工檢查後，下載 11 頁圖片式 PDF。

## 品牌

固定使用使用者指定的 `web/assets/district-logo.png`，源自 `地區Logo.png`，保留原始比例與顏色，版面另留安全空間。GPT Images 提示詞明確禁止產生 Logo、齒輪或品牌標誌；網站最後才疊加原始官方 Logo。依據使用者提供的 2019 年 12 月規範；不代表官方認證。

## 個人草稿

伺服器使用 Sites 的「使用 ChatGPT 登入」及 `oai-authenticated-user-id` 隔離草稿。R2 `BUCKET` 保存完整 JSON 文件及內嵌照片，使用 ETag 條件寫入避免不同視窗互相覆蓋。單份限制 16 MB。草稿不是以 localStorage 儲存。原始上傳照片與合成圖均會儲存。

GitHub Pages 為靜態網站，沒有 Sites 登入與 R2。該版本使用瀏覽器 IndexedDB 保存草稿及照片；資料只留在同一台裝置與同一瀏覽器，清除網站資料後也會移除。

## 使用自己的 ChatGPT 帳號

網站不需要也不接受共用的 `OPENAI_API_KEY`。文字研究與內容生成在訪客自己的 ChatGPT 工作階段完成，可用功能及用量依訪客的 ChatGPT 方案決定。

目前採用可立即公開使用的銜接流程：網站先驗證第一波講稿是否包含完整 8 頁，再把原稿、講師資料、固定頁序、品牌限制及視覺要求組成 11 個生圖指令。一般瀏覽器可複製全部或單頁提示詞至 ChatGPT，下載圖片後逐頁上傳。

網站同時註冊 WebMCP 工具，可讓支援 WebMCP 的 ChatGPT 或 Codex 讀取目前頁面的 11 頁生圖任務、逐頁或批次放回背景圖，並檢查 PDF 匯出狀態。網站按鈕會建立批次狀態與準備指令；實際模型執行仍由使用者在 ChatGPT 對話送出，因為 WebMCP 的呼叫方向是代理程式使用網站工具。依 [OpenAI Site tools 文件](https://learn.chatgpt.com/docs/webmcp)，使用者須在 ChatGPT 桌面版的內建瀏覽器開啟網站，且功能是否可用仍取決於版本、模型、工作區與推出狀態。

官方文件：
- https://learn.chatgpt.com/docs/sites
- https://learn.chatgpt.com/docs/webmcp
- https://learn.chatgpt.com/docs/image-generation

## 維護

- `node scripts/build.mjs`：將 `web/` 資產及 Worker 打包至 `dist/server/index.js`，不含秘密或本機草稿。
- `node scripts/dev.mjs`：本機私人測試轉接器，自動配置連接埠；使用 `.local/bucket` 保存測試資料，不會部署。
- `node --test tests/*.test.mjs`：測試新版草稿格式、草稿隔離、重開、版本衝突、公開狀態與 PDF 結構。
- 上線依 Sites 流程推送同一提交、打包、儲存版本並發布。

PDF 採用每頁一張 16:9 JPEG 的方式建立，讓講師在不同電腦上開啟時維持相同字型、圖片與版面。瀏覽器中的最後人工確認步驟留給使用者。
