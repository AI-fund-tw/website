# 補助情報站

用 [Astro](https://astro.build) 建立的靜態網站,包含：

- `/` 首頁 — 開放中方案總數、即將截止的補助、最新觀察筆記
- `/subsidies/` 補助方案總覽,可依分類篩選
- `/blog/` 部落格文章列表與內文(Markdown 撰寫)

補助資料目前放在 `src/data/subsidies.json`,是**手動整理的範例資料**。之後若要接上「定時自動抓取政府網站」的功能,由 Cloudflare Worker 定時抓資料後覆寫這個檔案內容(或改為在 build 時向 API 拉資料),即可讓網站自動更新。

## 本機開發

```bash
npm install
npm run dev
```

瀏覽器開啟 http://localhost:4321 即可預覽。

## 建置

```bash
npm run build
```

輸出在 `dist/` 資料夾。

---

## 部署到 Cloudflare Pages

### 1. 推上 GitHub

```bash
git init
git add .
git commit -m "init: 補助情報站"
git branch -M main
git remote add origin <你的 GitHub repo 網址>
git push -u origin main
```

### 2. 在 Cloudflare 建立 Pages 專案

1. 登入 https://dash.cloudflare.com
2. 左側選單「Workers & Pages」→「Create application」→「Pages」→「Connect to Git」
3. 選擇剛剛推上去的 repository
4. Build 設定：
   - Framework preset：`Astro`
   - Build command：`npm run build`
   - Build output directory：`dist`
5. 按「Save and Deploy」

幾分鐘後就會拿到一個 `*.pages.dev` 的網址。之後只要 `git push`,Cloudflare 就會自動重新 build 並部署。

### 3.（選用)綁定自訂網域

在該 Pages 專案的「Custom domains」頁籤新增你自己的網域,依指示設定 DNS 即可。

---

## 之後要接上「定時抓取政府補助資料」時

目前 `src/data/subsidies.json` 是靜態範例資料。要做到自動更新,建議的做法是：

1. 另外建立一個 Cloudflare Worker,用 `wrangler.toml` 設定 Cron Trigger(例如每天早上 8 點執行一次)
2. Worker 內容去抓政府開放資料 API 或目標網站,整理成跟 `subsidies.json` 一樣的格式
3. 把整理好的資料寫入 Cloudflare D1 或 KV
4. 這個 Astro 網站在 build 時,改成向該 Worker 提供的 API 拉取資料(取代直接 import `subsidies.json`)
5. 用 Cloudflare 的 **Deploy Hooks** 功能,讓 Worker 抓完資料後觸發 Pages 重新 build,網站就會自動換上最新資料

這部分需要另外建立 Worker 專案,如果需要我可以接著幫你把這一段做出來。
