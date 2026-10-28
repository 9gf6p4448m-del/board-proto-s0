# 03｜技術架構與已有的表現能力

本頁是依原始碼整理的說明，不包含原始碼，也不是套件升級建議。

| 部分 | 現況 | 本機來源相對路徑 |
|---|---|---|
| 前端 | TypeScript＋Vite，直接操作 DOM／HTML／SVG；不是 React | `proto/package.json`、`proto/src/main.ts` |
| 首頁 | v0.9.0 首頁與設定、規則、圖鑑、教學等入口 | `proto/src/hmui.ts`、`hmview.ts` |
| 對局介面 | 正式疊陣由 stk 系列處理，舊模式另存 | `proto/src/stkui.ts`、`stkcore.ts` |
| 規則引擎 | 合法動作、套用行動、結果與事件獨立於 DOM | `g1/rules.ts`；`engine/` 是歷史模式，勿混用 |
| AI | 專用 Web Worker 搜尋，以訊息回傳；頁面不直接阻塞運算 | `proto/src/stk.worker.ts`、`stkai.ts`、`g1/ai.ts` |
| 棋盤 | DOM 格子配逐格 SVG，另有動畫／擴散 SVG 層 | `proto/src/stkview.ts` |
| 人偶與卡圖 | 已有 WebP 2x／3x 素材，用於面板、抽兵卡、特寫 | `proto/src/stkfig.ts`、`proto/public/fig/`、`proto/public/card/` |
| 動畫 | 已有時間軸、移動／疊棋、關鍵事件特寫、減少動態處理 | `proto/src/stkanim.ts`、`stkspot.ts` |
| 聲音 | WebAudio 即時合成，已有落子、疊棋、借勢、警告、勝負等聲音 | `proto/src/sfx.ts` |
| 離線研究 | 同一引擎做 AI 對打、記錄與重播；獨立 ref 用於差分核對 | `g1/sim/`、`verify-g1/` |

## 事件邊界已經存在

引擎會產生 `GameEvent`，介面利用同一批事件驅動動畫、特寫與聲音。因此「先把規則引擎與表現用事件隔開」不是一項從零開始的工作。現有接線集中在對局介面，尚不能誇稱完整的通用演出平台。

若要改善回饋，先檢查既有事件欄位、播放順序、取消／跳過與悔棋等流程，再決定是否需要新抽象。現在的資料包只足夠設計討論；真的要實作時仍需在本機讀原碼。

## 對工具建議的限制

- CSS transform、opacity、Web Animations 等可作候選，但先盤點現有動畫，不能宣稱專案尚無回饋。
- React Three Fiber、React hooks、Framer Motion 等需要 React 的建議，不能直接貼進這個專案；導入框架屬額外移植工程。
- GSAP、PixiJS、tsParticles、Lottie、Rive、Threlte 都不是本包已確認的現有依賴。提案時請先說哪個現有問題非用它不可，以及負擔和替代方案。
- 「透明 canvas 粒子層」「2.5D CSS 棋盤」「背景載入 3D」目前只是可討論方案，沒有已完成、已通過效能或已獲製作人選定的證據。
- 四天王 v2 位於離線研究版本，不能拿它的存在推論公開 Worker 或正式 UI 已有秘技按鈕。

核對位置：`proto/index.html:22`、`proto/src/main.ts:84`、`hmui.ts:52`、`stkui.ts:461` 與 `:480`、`stkcore.ts:349` 與 `:623`、`stkview.ts:127`、`stkfig.ts:1`、`stkanim.ts:137`、`stkspot.ts:36`、`sfx.ts:1`、`g1/rules.ts:448`。行號隨後續版本可能位移。
