# 前端架構實作報告書：單一 HTML 數位祭祖互動介面 (Portable Ancestral Worship Interface)

本報告書旨在規劃一個無須伺服器架設、雙擊即可執行的單一 `.html` 檔案專案。透過現代 CSS (Grid/Flexbox/Writing-mode) 與原生 JavaScript (ES6+ State Management)，完美還原台灣傳統祭祖（三節與清明）的儀式感與尊榮感，協助無法返鄉的遊子跨越時空慎終追遠。

---

## 📖 目錄
1. [專案範疇與使用情境 (Scope & Context)](#1-專案範疇與使用情境-scope--context)
2. [技術規格與核心限制 (Technical Specifications)](#2-技術規格與核心限制-technical-specifications)
3. [系統架構與資料結構 (System Architecture & Config)](#3-系統架構與資料結構-system-architecture--config)
4. [UI/UX 視覺佈局與結構 (Layout & DOM Structure)](#4-uiux-視覺佈局與結構-layout--dom-structure)
5. [核心工作細項與實作說明 (Work Breakdown Structure)](#5-核心工作細項與實作說明-work-breakdown-structure)
6. [交付檔案與驗收標準 (Deliverables & Acceptance Criteria)](#6-交付檔案與驗收標準-deliverables--acceptance-criteria)

---

## 1. 專案範疇與使用情境 (Scope & Context)

### 1.1 使用情境
使用者為因工作、距離或其他不可抗力因素無法返鄉的台灣遊子。使用者直接在電腦或手機瀏覽器雙擊打開 `index.html`，即可進入一個莊嚴、溫馨的數位公媽廳。

### 1.2 核心儀式流程
*   **設定與客製化**：讀取頂層配置，將祖先牌位以正統「由上至下、由右至左」的直書文字展現。
*   **準備供品**：開啟供品選單，點擊將祭品（如牲禮、水果、糕點）擺放至神桌對應位置。
*   **斟酒儀式**：點擊神桌前方的三杯小酒杯，執行倒酒動畫。
*   **點香祭拜**：點擊線香，線香燃起裊裊白煙（CSS 粒子），並隨時間遞減長度。
*   **化金迴向**：點選金紙放入金爐，觸發火焰燃燒與金紙淡出特效。

---

## 2. 技術規格與核心限制 (Technical Specifications)

*   **單一檔案架構 (Single-file Architecture)**：絕對禁止使用 `fetch()` 或 `XMLHttpRequest` 讀取外部本地檔案（避免觸發瀏覽器 CORS 阻擋）。所有的 HTML、CSS (包含動畫與字型封裝)、JavaScript 必須全數寫在單一的 `index.html` 內。
*   **零外部依賴 (Zero Dependency)**：不用 React/Vue/Angular。使用純原生 JavaScript (Vanilla JS)。不用 Tailwind CDN（確保離線可用性），全數使用 Modern Native CSS。
*   **圖資與視覺解決方案**：圖示與供品完全使用純 CSS 繪製 (CSS Shapes/Gradients) 或內嵌 SVG 向量原始碼，嚴禁外部圖片連結。
*   **響應式設計 (RWD)**：採用 `clamp()` 與 `vmax/vmin` 彈性單位，確保在手機（直式）與 PC（橫式）螢幕皆有舒適的視覺比例。

---

## 3. 系統架構與資料結構 (System Architecture & Config)

### 3.1 頂層客製化設定 (User Config Location)
在 `<script>` 標籤的最頂端，定義全域常數 `CONFIG`。非工程師的使用者只要用記事本打開檔案，即可修改此區塊。

```javascript
// ==========================================
// 使用者客製化設定區 (User Configuration)
// ==========================================
const CONFIG = {
  // 祖先牌位文字 (由上至下直書)
  tablet: {
    leftColumn: "西元二零二六端午吉旦",
    centerColumn: "詹氏歷代祖先神位",
    rightColumn: "陽上子孫叩拜"
  },
  // 初始金紙數量
  initialJossPaper: 50,
  // 線香燃燒時間 (單位：秒)
  incenseBurnDuration: 180,
  // 介面風格主題: 'classic-wood' (古風木紋) | 'temple-red' (宮廟宮廷紅)
  theme: "classic-wood"
};
```

### 3.2 應用程式狀態管理器 (State Management)
```javascript
const AppState = {
  incense: {
    isLit: false,
    timeLeft: CONFIG.incenseBurnDuration,
    timerId: null
  },
  cups: [
    { id: 1, filled: false },
    { id: 2, filled: false },
    { id: 3, filled: false }
  ],
  currentOfferings: [], // 儲存目前擺放在桌上的供品 ID
  jossPaperCount: CONFIG.initialJossPaper,
  isBurningGold: false
};
```

---

## 4. UI/UX 視覺佈局與結構 (Layout & DOM Structure)
網頁畫面主要分為三大區塊：
1. **左側/下方互動面板 (Control Panel)**：供品選單、線香控制、金紙控制與設定面板。
2. **中央核心祭祀區 (Main Altar)**：公媽牌位（祖先牌位）、香爐、神桌（供桌）、三只酒杯與供品放置槽。
3. **右側化金金爐區 (Burner Area)**：金爐本體、燃燒火焰動畫、金紙餘量與化金按鈕。

---

## 5. 核心工作細項與實作說明 (Work Breakdown Structure)

- [x] **細項 5.1：中文字體直書與牌位視覺 (CSS Writing-mode)**
  * **實作目的**：完美重現台灣傳統神主牌位的文字排列美學。
  * **實作說明**：對 `.ancestral-tablet` 及其子文字欄位套用 `writing-mode: vertical-rl;` 與 `text-align: start;`。使用 `letter-spacing` 微調字體間距。牌位本體利用 CSS `linear-gradient` 營造出紅木紋或金邊沉穩感，並加上 `box-shadow` 產生立體浮雕陰影。
- [x] **細項 5.2：動態線香與煙霧粒子系統 (Incense & Smoke Logic)**
  * **實作目的**：模擬真實點香時，香火明滅與輕煙繚繞的視覺意象。
  * **實作說明**：線香本體點擊「點香」後，啟動計時器。線香高度隨時間比例遞減，頂端使用 CSS 加上微微呼吸閃爍的橘紅色光暈。煙霧效果在線香頂端建立一個煙霧容器，利用 JS 動態生成多個帶有模糊濾鏡的半透明微小圓點，套用向上飄散且隨機向左右位移、逐漸淡出的 CSS Animation。
- [x] **細項 5.3：斟酒互動系統 (Wine Cup Interaction)**
  * **實作目的**：點擊酒杯即可完成「奠酒三巡」的動作。
  * **實作說明**：酒杯使用 SVG 或 CSS 圓角繪製。當點擊個別酒杯時，酒杯內部的液體層利用 `transform: scaleY(0)` 到 `scaleY(1)` 的動態升起。
- [ ] **細項 5.4：動態供品擺放系統 (Offering Position Management)**
  * **實作目的**：讓使用者自由挑選合適的節慶供品，並整齊排放在神桌上。
  * **實作說明**：在左側選單中建立供品清單。點選後，將該供品資料 Push 進 `AppState.currentOfferings` 並觸發 Render 機制。神桌上預設數個絕對定位的「聖位插槽」，當供品被啟用時，以漸顯與輕微下沉的動畫置入插槽中。
- [ ] **細項 5.5：化金區與金爐火焰特效 (Joss Paper Burner)**
  * **實作目的**：提供一個紓壓且具象化的燒金紙動態。
  * **實作說明**：點擊「燒一張」或「自動連燒」時，數量遞減。當處於化金狀態時，金爐口觸發 CSS 動態：利用多層色彩漸層搭配 `clip-path` 與 `transform: scale()` 的波浪形變動畫，做出熊雄烈火的效果。

---

## 6. 交付檔案與驗收標準 (Deliverables & Acceptance Criteria)

### 6.1 交付檔案
*   `index.html`（內含所有 HTML 結構、CSS 樣式表、JavaScript 邏輯與 SVG 資源）。

### 6.2 LLM Agent 驗收標準 (Definition of Done)
*   **零報錯開啟**：檔案直接用瀏覽器雙擊開啟（`file:///` 協定），控制台不得出現任何錯誤。
*   **設定檔連動**：修改程式碼最上方的設定文字後，網頁重新整理時，牌位文字必須正確同步更新，且文字排列絕對是正統直書。
*   **狀態同步**：點燃線香後，香會確實縮短；香燃盡後，煙霧效果必須自動停止。
*   **流暢度**：所有的動畫必須使用 CSS 3D 補間加速，在行動裝置上需保持 60fps 的流暢度。
*   **響應式防爆**：在極端螢幕尺寸下，牌位、神桌與金爐的相對位置不得錯位疊加。
