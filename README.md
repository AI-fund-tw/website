# 亨起流通 形象官網

純靜態網站（單一 `index.html`，無需建置），可直接部署到 Cloudflare Pages。

## 內容
- 分頁：關於亨起、產品、客製化開發、聯繫我們（網址以 `#about`、`#product`、`#custom`、`#contact` 切換）
- 聯繫表單：公司名稱、統一編號、聯絡人姓名、電話、電子郵件、需求說明（目前僅前端驗證，尚未寄出資料）

## 部署到 Cloudflare Pages（連結 GitHub）
1. 將此資料夾內容推送到 GitHub 儲存庫（`index.html` 位於儲存庫根目錄）。
2. Cloudflare 後台 → Workers & Pages → Create → Pages → Connect to Git，選擇該儲存庫。
3. Build settings：Framework preset 選 None；Build command 留空；Build output directory 填 `/`。
4. 儲存並部署。之後每次 push 到 main 都會自動更新。
5. 自訂網域：Pages 專案 → Custom domains → Set up a domain。
