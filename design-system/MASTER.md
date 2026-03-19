# 遠雄人壽 AI 保險助理 — Design System MASTER
> 版本：V2（`pdf-assistant-v2.html`）
> 分支：`design/red-ci`
> 更新日期：2026-03-19

---

## 1. 品牌 & 識別

| 項目 | 值 |
|------|----|
| 品牌名稱 | 遠雄人壽保險事業股份有限公司 |
| 產品名稱 | AI 保險助理 |
| 品牌色 | `#E7242C`（遠雄紅） |
| Logo | SVG 紅色飛鳥造型，viewBox `0 0 144 118` |
| Tagline | 智能化服務，讓保險查詢更簡單 |

---

## 2. Color Palette

### 品牌色
| Token | Hex | 用途 |
|-------|-----|------|
| `--red` | `#E7242C` | 主色、CTA、Send 按鈕、User 氣泡 |
| `--red-dark` | `#C41E24` | Hover、漸層深色端 |
| `--red-light` | `#FFF0F0` | 淺色背景、Chip hover、File bar |

### 中性色
| Token | Hex | 用途 |
|-------|-----|------|
| `--white` | `#FFFFFF` | 卡片背景、助理氣泡 |
| `--gray-50` | `#F8FAFC` | Chat 背景 |
| `--gray-100` | `#F1F5F9` | Input 背景、元素 hover |
| `--gray-200` | `#E2E8F0` | Border、分隔線 |
| `--gray-400` | `#94A3B8` | 副文字、時間戳 |
| `--gray-500` | `#64748B` | 說明文字 |
| `--gray-800` | `#1E293B` | 主要文字 |

### Landing 主題色（CSS 自訂屬性）
| Token | Dark | Light |
|-------|------|-------|
| `--land-bg` | `#06090F` | `#F8F9FF` |
| `--land-text` | `#F1F5F9` | `#0F172A` |
| `--land-card-bg` | `rgba(255,255,255,0.05)` | `rgba(255,255,255,0.88)` |
| `--orb1` | `rgba(231,36,44,0.22)` | `rgba(251,191,36,0.28)` |
| `--orb2` | `rgba(88,50,240,0.14)` | `rgba(125,211,252,0.22)` |

### Chat Dark Mode
| 元素 | 顏色 |
|------|------|
| 聊天背景 | `#0B1120` |
| Header 背景 | `#0D1526` |
| 助理氣泡背景 | `#141E30` |
| 輸入區背景 | `#0D1526` |
| 助理氣泡文字 | `#D1D5DB` |

---

## 3. Typography

| 用途 | 字型 | 大小 | 粗細 |
|------|------|------|------|
| System Font Stack | `-apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang TC', 'Noto Sans TC', Roboto` | — | — |
| 品牌公司名稱 | 繼承 | `1.5rem` | `700` |
| Tagline | 繼承 | `0.875rem` | `400` |
| 卡片標題 | 繼承 | `1.075rem` | `600` |
| 卡片說明 | 繼承 | `0.845rem` | `400`，line-height `1.68` |
| 聊天氣泡 | 繼承 | `0.875rem` | `400`，line-height `1.65` |
| 輸入欄位 | 繼承 | `0.875rem` | — |
| 時間戳 | 繼承 | `0.67rem` | — |
| PDF Tag / Analysis | 繼承 | `0.7rem` | — |

> **建議字型升級**：IBM Plex Sans（金融信賴感）
> `@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@300;400;500;600;700&display=swap');`

---

## 4. Layout & Screens

### 畫面架構（State Machine）
```
idle
  └─ enterChat(mode) ──────────────────────► waiting_file_selection
                                                  │
                                            (輸入問題)
                                                  │
                                              waiting_confirmation
                                                  │
                                            (確認/選擇PDF)
                                                  │
                                              conversation (持續對話)
```

### 雙螢幕設計
| Screen | ID | 狀態 |
|--------|----|------|
| Landing | `#screen-landing` | 預設 active |
| Chat | `#screen-chat` | 點卡片後切入 |

切換：`opacity: 0/1` + `pointer-events: none/all`，轉場 `0.45s cubic-bezier(0.4,0,0.2,1)`

---

## 5. Components

### 5.1 Landing Cards（選擇服務模式）

- 2 欄 Grid（手機 1 欄），最大寬 `660px`
- 玻璃效果：`backdrop-filter: blur(8px)`
- Cursor spotlight：CSS `::before` + `radial-gradient` + JS 動態更新 `--cx` / `--cy`
- Hover 漸層：`::after` pseudo-element，Card 1 紅色、Card 2 藍色
- 動畫：`cardIn` — `translateY(26px) scale(0.97)` → `translateY(0) scale(1)`

### 5.2 Orb 光源（Landing 背景）

三個模糊光球，跟隨滑鼠 Lerp 移動：
```javascript
// Lerp 係數
const LERP = { orb1: 0.04, orb2: 0.03, orb3: 0.06 };
// 偏移係數（每顆 orb 跟隨滑鼠的程度不同）
```
- Dark mode：紅 + 紫 + 橙
- Light mode：金黃 + 天藍 + 淡紅

### 5.3 Chat Header

| 區域 | 內容 |
|------|------|
| 左側 | 返回按鈕 + Logo（28×23px）+ 公司名稱 |
| 右側 | 模式 Tab（保險條款諮詢 / 理賠諮詢）+ 主題切換按鈕 |

- Tab 選中：白底 + 紅字 + 陰影
- Theme 按鈕：Sun/Moon icon，位於 Tab 右側最末

### 5.4 訊息氣泡

| 角色 | 對齊 | 樣式 |
|------|------|------|
| 助理 | 左側 | 白底 + 邊框，左下圓角改 `3px` |
| 用戶 | 右側 | 紅色漸層，右下圓角改 `3px` |

- 助理頭像：遠雄紅 Logo SVG（`32×32px`，白底，rounded `9px`）
- 用戶頭像：灰底 "U" 文字

### 5.5 PDF Tag

顯示於助理氣泡上方，標示參考的 PDF 來源：
```html
<div class="pdf-tag">
  [paperclip SVG] 檔案名稱.pdf
</div>
```
- 灰底 + 邊框，小圓角 `5px`，`0.7rem`
- Dark mode：半透明白

### 5.6 Analysis 摺疊區塊

顯示批量查詢的分析步驟：
```html
<div class="analysis-wrap">
  <button class="analysis-toggle [open]" onclick="toggleAnalysis(id, this)">
    [chevron] Analysis >
  </button>
  <div class="analysis-steps [open]">
    <div class="analysis-step">步驟說明</div>
  </div>
</div>
```
- 預設摺疊（`display: none`）
- 點擊展開（加 `open` class）
- Chevron 旋轉 90°

### 5.7 Batch Progress Bubble（批量查詢進度）

單一氣泡包含整個批量進度，避免多訊息刷屏：
- `startBatchBubble()` — 建立氣泡，顯示動態點點
- `updateBatchStatus(text)` — 更新進度文字
- `finalizeBatchBubble(resultHtml)` — 替換為最終結果 + Analysis 摺疊

### 5.8 Loading Overlay

PDF 讀取中顯示：
- 文件圖示 + 紅色掃描光束動畫（`scanMove`）
- 標題、說明文字、進度條（`barFill`）
- 背景：`rgba(6,9,15,0.8)` + `backdrop-filter: blur(12px)`

### 5.9 Mode Switch Modal

切換模式前確認清除對話紀錄：
- 橘色警示 icon
- 取消 / 確定切換 兩顆按鈕

---

## 6. Animation Tokens

| 名稱 | 用途 | 設定 |
|------|------|------|
| `fadeSlideDown` | Landing 品牌區 | `0.9s cubic-bezier(0.4,0,0.2,1)` |
| `fadeSlideUp` | 卡片標題、Footer | `0.8s 0.2s` |
| `cardIn` | 模式卡片 | `0.75s`，delay 0.3s / 0.45s |
| `msgIn` | 訊息氣泡入場 | `0.3s translateY(10px)→0` |
| `wave` | Typing 三點 | `1.4s`，delay 0.2s / 0.4s |
| `wPulse` | Welcome ring | `3s ease-in-out infinite` |
| `dRing` | Loading doc ring | `2s ease-in-out infinite` |
| `scanMove` | PDF 掃描光束 | `2s ease-in-out infinite` |
| `barFill` | 進度條 | `2.5s ease-in-out infinite` |
| Screen 切換 | Landing ↔ Chat | `opacity 0.45s` |
| Theme 切換 | 背景、文字色 | `0.5s–0.6s ease` |

---

## 7. Interaction Patterns

### 卡片 Cursor Spotlight
```javascript
document.querySelectorAll('.mode-card').forEach(card => {
    card.addEventListener('mousemove', e => {
        const r = card.getBoundingClientRect();
        card.style.setProperty('--cx', (e.clientX - r.left) + 'px');
        card.style.setProperty('--cy', (e.clientY - r.top) + 'px');
    });
});
```

### Orb 滑鼠追蹤（Landing）
```javascript
// RAF loop with lerp
function lerp(a, b, t) { return a + (b - a) * t; }
// 每顆 Orb 有不同 lerp 係數（0.03–0.06）
// 使用 requestAnimationFrame 持續更新 transform: translate()
```

### 鍵盤送出
`Enter` 送出，`Shift+Enter` 換行

---

## 8. 功能列表

| 功能 | 說明 |
|------|------|
| 服務模式選擇 | 保險條款諮詢 / 理賠諮詢 |
| PDF 文件庫 | 17 份遠雄保單 PDF，含 HJ1–HJ5、HG3–HG6、RSJ/RSK/RSL/RSM、RJ1/RM1/RM2/RM3 |
| PDF 解析 | PDF.js 3.11.174，全文本提取，最多 100 頁 |
| AI 問答 | Claude API（`claude-sonnet-4-5`），streaming 回應 |
| 批量查詢 | 最多 2 份 PDF/批，65 秒限速等待，單一進度氣泡 |
| 對話記錄 | 多輪對話，歷史訊息隨 token 壓縮 |
| 快捷問題 Chip | 4 個預設問題，點擊填入輸入框 |
| 深色/淺色模式 | `data-theme` 切換，Landing + Chat 分別有主題變數 |
| 換檔案 | File bar「換檔案」按鈕，重置對話 |
| 返回首頁 | Header 左側返回箭頭 |

---

## 9. System Prompts

### 保險條款諮詢
```
你是遠雄人壽的保險條款諮詢助理。根據提供的 PDF 文件內容，幫助客戶了解保險條款。
1. 清楚解釋保障範圍、除外事項、等待期等條款細節
2. 引用具體條款號碼或頁碼
3. 用平易近人的語言解釋專業術語
4. 只根據文件內容回答，不編造資訊
5. 文件未提及時，誠實告知
6. 必要時用條列方式整理
7. 比較不同版本時，明確指出版本差異
```

### 理賠諮詢
```
你是遠雄人壽的理賠諮詢助理。根據提供的 PDF 文件內容，協助客戶了解理賠相關事宜。
1. 直接回答是否符合理賠條件
2. 明確列出所需申請文件清單
3. 說明理賠申請流程與時程
4. 計算或說明可獲得的理賠金額
5. 引用具體條款依據
6. 只根據文件內容回答，不編造資訊
7. 如有疑義，建議聯繫遠雄人壽理賠部門
```

---

## 10. Responsive

| 斷點 | 調整 |
|------|------|
| `≤ 600px` | Cards 改為單欄；Header 隱藏公司名稱與分隔線；縮小 Tab padding；訊息最大寬 88% |

---

## 11. Pre-Delivery Checklist

- [x] 全部 icon 使用 SVG（無 emoji）
- [x] 可點擊元素皆有 `cursor: pointer`
- [x] Hover 有視覺回饋（顏色、陰影、位移）
- [x] 轉場 150–300ms（micro），部分動畫 450ms
- [x] Dark mode 對比度足夠（助理氣泡 `#D1D5DB` on `#141E30`）
- [x] Light mode 主文字 `#1E293B` on white（對比 > 10:1）
- [x] 輸入框有 focus ring（`box-shadow: 0 0 0 3px rgba(231,36,44,0.07)`）
- [x] 手機斷點 `≤ 600px`
- [ ] `prefers-reduced-motion`：Orb 動畫尚未加入此 media query
- [ ] `aria-label`：部分 icon-only 按鈕缺少 aria-label
