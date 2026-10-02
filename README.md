# FUNBOX／來玩聚抽選清單

延續原有網站的手機版清單。支援多縣市依點選順序、商品包含／排除、動態時間、每週資料與手動抽選回報。手動紀錄不代表 LINE 實際抽選狀態。

## 目前狀態

- 資料基準：UXUX11 2026/10/02 11:38，76 家有抽選門市。78 筆門市主檔，1,288 筆商品資料（原始 1,289 筆移除 1 筆完全重複）；一組不同商品共用連結標示待確認。
- 真正控制 LINE 的自動模式尚未實作。頁面清楚停用自動開始，不會把開啟連結算成抽選成功。佇列引擎的重試、略過及計數只有模擬介面測試，iOS／Android 未完成實機驗證。
- Facebook 更新引擎已建立，但目前只配置 1 家來源，並遇到 Facebook 登入限制。尚不具備全台自動更新能力。

## 使用網站

依序點縣市，再展開商品選擇包含或排除。選擇時間後查看摘要。手動模式建立清單，開啟 LINE 自行參加，再回網站回報狀態。重新載入可恢復同週未完成手動清單；資料只存在自己的瀏覽器。

## Windows 更新器

需 Node.js 22 以上；要發布另外需要 Git 和 GitHub 登入。雙擊 updater/Run-Console.cmd 啟動，保持視窗開啟、電腦供電且不休眠，Ctrl+C 停止。此方式不依賴 ChatGPT。updater/Start-Updater.cmd 提供視窗版，但尚未完整測試，若系統阻擋 PowerShell 請使用命令視窗版，無需降低系統安全設定。

複製 updater/config.example.json 為 updater/config.local.json，intervalSeconds 預設 240（4 分鐘），publish 預設 false。sources.json 需填入已確認的 Facebook 門市公開網址與 stores.json 對應 id。來源受登入限制時不繞過限制，可將公開貼文文字以 {"storeId":"門市id","sourceUrl":"完整Facebook貼文網址","text":"貼文全文"} 存入 updater/imports/任意名稱.json；日期及商品連結關係不明確會待人工核對，不猜測。

state/status.json 查看來源覆盖、抓取時間、解析狀態與發布狀態；state/parsed 保存可讀貼文與解析結果。抓取失敗不刪除舊資料。停止更新器不影響已發布的靜態網站。

GitHub 登入完成且 main 分支、origin 設定正確後，才將 publish 改 true。更新器只提交 stores.json 和 weeks，推送失敗於下一輪重試；不強制覆寫遠端，不自動合併衝突。pushed-awaiting-pages 代表推送完成，仍須 GitHub Actions 部署成功。

## 開發與發布

npm test 執行邏輯測試；npm run check 檢查資料；npm run preview 在 localhost:4173 預覽；npm run update 執行一輪。

GitHub Pages 使用 .github/workflows/pages.yml，發布 dist 目錄。Repository Settings → Pages → Source 選 GitHub Actions。公開發布不需上傳 updater/state、config.local.json、imports、.git 或 .openai。腳本 scripts/migrate.mjs 為一次性遷移且已加防覆寫保護，不應重跑。

原始資料與來源：https://uxux11.github.io/funbox-line/ 。非 FUNBOX／來玩聚／LINE 官方網站。
