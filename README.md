# 🕯️ 數位祭祖互動介面 (Portable Ancestral Worship Interface)

「慎終追遠，民德歸厚矣。」

本專案是一個無須架設伺服器、雙擊即可執行的**單一 HTML 數位公媽廳（祭祖互動介面）**。旨在還原傳統祭祖儀式感，協助身處異鄉的遊子跨越時空與地理限制，在現代瀏覽器中以莊嚴且溫馨的視覺效果進行思念與追悼。

---

## ✨ 核心特色與功能

*   **🔤 祖先牌位正統直書**
    *   配合 `writing-mode: vertical-rl` 完美呈現傳統神主牌「由上至下、由右至左」的排版美學。
    *   使用 Google 字型 `Noto Serif TC` 並加入微幅發光與立體陰影，展現莊重感。
    *   支援古風木紋 (`classic-wood`) 與宮廟宮廷紅 (`temple-red`) 雙主題即時切換。

*   **💨 動態線香與裊裊輕煙**
    *   點燃線香後香體高度會隨時間比例遞減，火星處帶有呼吸閃爍效果。
    *   煙霧效果使用 HTML5 Canvas/DOM 結合 GPU 加速的 CSS 3D 動態隨機生成，輕煙飄散、淡出表現真實自然。
    *   可自訂燃燒時間（預設 180 秒），線香燃盡時煙霧自動熄滅。

*   **🍷 奠酒三巡互動**
    *   純 CSS 繪製的立體金色爵杯，點擊個別酒杯或面板按鈕即可倒入/倒掉紅酒。
    *   液體表面設有平滑的 `scaleY` 升降補間動畫，可完整體驗並記錄奠酒一巡、二巡、三巡的儀式進程。

*   **🍎 節慶供品擺放系統**
    *   內嵌 4 種極具台灣傳統特色之向量 SVG 供品：**紅龜粿**（長壽）、**紅蘋果**（平安）、**吉利橘**（大吉）、**發粿**（發財）。
    *   滑鼠點選後，供品將以「漸顯與微幅下沉」的彈性動畫（Bounce Cubic-bezier）妥善置入神桌插槽，點擊桌上供品可隨時撤收。

*   **🔥 八卦金爐與化金特效**
    *   內建精緻的雙層八卦金爐與 3D 質感金紙堆。
    *   點擊「燒一張」或啟用「自動連燒」時，金紙會劃出優雅的拋物線軌跡飛入爐口。
    *   爐口會動態觸發多層漸層火焰，模擬熊熊烈火的紓壓火光。

*   **📱 完美響應式佈局 (RWD)**
    *   使用 CSS Grid Area 優化不同載具排版。
    *   在手機或直式螢幕下，核心祭祀區（神桌與牌位）會自動置頂，控制面板與金爐區向下延伸，保持最佳視覺比例，防止元素錯位。

---

## 🚀 快速開始

### 1. 執行方式
本專案為**零外部依賴、純原生前端架構**，無須安裝 Node.js、無須編譯、無須跑 Web Dev Server：
1. 下載或複製本專案中的 `index.html`。
2. 在本地電腦上**雙擊開啟 `index.html`**，或者使用瀏覽器將其打開（路徑顯示為 `file:///...` 協定）。
3. 控制台保證 0 錯誤，離線環境下亦可直接執行。

### 2. 客製化神主牌文字與設定
非工程師使用者也可以透過任何文字編輯器（如記事本、VS Code）打開 `index.html`，直接修改最頂部的 `CONFIG` 常數來進行客製：

```javascript
// ==========================================
// 使用者客製化設定區 (User Configuration)
// ==========================================
const CONFIG = {
  // 祖先牌位文字 (由上至下直書)
  tablet: {
    leftColumn: "西元二零二六端午吉旦", // 左側紀年（生卒或節日）
    centerColumn: "詹氏歷代祖先神位",   // 正中牌位主文字
    rightColumn: "陽上子孫叩拜"         // 右側落款
  },
  // 初始金紙數量
  initialJossPaper: 50,
  // 線香燃燒時間 (單位：秒)
  incenseBurnDuration: 180,
  // 預設介面風格主題: 'classic-wood' (古風木紋) | 'temple-red' (宮廟宮廷紅)
  theme: "classic-wood"
};
```

---

## 🛠️ 技術規格

*   **Core**: HTML5, Vanilla JavaScript (ES6+)
*   **Styling**: Modern Native CSS (Variables, Grid, Flexbox, Keyframes, Custom Clip-paths)
*   **Graphics**: 100% Inline SVG & CSS Custom Shapes (無須載入任何外部圖檔)
*   **Performance**: 所有動畫均採用 `transform`、`opacity` 以及 GPU 3D 渲染加速，確保手機與低效能電腦上可達 60fps 流暢度。

---

## 📜 專案架構與檔案

*   [index.html](file:///home/kenny/Git_KennySpace/Virtual-ancestor-worship/index.html) - 單一應用程式入口（包含所有結構、樣式與控制代碼）
*   [requirement.md](file:///home/kenny/Git_KennySpace/Virtual-ancestor-worship/requirement.md) - 前端架構實作報告書與需求表
*   [README.md](file:///home/kenny/Git_KennySpace/Virtual-ancestor-worship/README.md) - 本說明文件
