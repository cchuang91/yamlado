# AGENTS.md - 專案開發規範與協作指引

本文件為「養樂多寵物樂園」品牌網站專案的開發守則與 Agent 協作規範。所有參與本專案的 AI Agent 與開發者均須嚴格遵守以下規則。

---

## 1. 核心開發守則

- **語系要求**：全面使用「臺灣繁體中文」（zh-TW），包含所有介面文案、程式碼註解、說明文件與測試資料，符合在地用語習慣（例如：「毛小孩」、「主食罐」、「凍乾」、「胸背帶」、「預購清單」）。
- **技術棧限制**：
  - 僅使用原生 HTML5、CSS3、現代 JavaScript (ES6+)。
  - **嚴禁引入重型前端框架**（如 React, Vue, Angular）。
  - **不使用任何後端服務**（No Backend）。
  - **不使用外部 API 或任何第三方金鑰 (API Keys)**。
- **資料與靜態資源管理**：
  - 所有商品資料、分類選單、常見問答皆以本地資料結構（如 `js/data.js`）維護。
  - 所有圖片、圖示皆放置於本地目錄或原生 SVG，確保離線或本機雙擊/靜態預覽時皆可完整渲染。
- **樣式與主題架構**：
  - 必須使用 CSS 變數（CSS Custom Properties）於 `:root` 定義並集中管理品牌色票、字級、間距、圓角與陰影。
  - 色彩設計嚴格依據 YouTube 官方頻道 `@養樂多寵物樂園` 視覺風格提煉：以「樂園森林綠」、「養樂多暖陽橙」、「樂樂圍巾紅」與「晴空藍」為核心。
- **RWD 行動優先策略**：
  - 採 **Mobile-first** 思維：先以手機螢幕尺寸（320px~480px）作為基礎排版設計，再使用 `@media (min-width: ...)` 逐步向上適配平板與桌機。
  - 觸控目標（按鈕、選單項目）需保有至少 44x44px 的易點擊區域。
- **無障礙與鍵盤互動 (Accessibility / a11y)**：
  - 所有按鈕（`<button>`）、連結（`<a>`）與互動控制元件必須支援純鍵盤操作（Tab、Enter、Space、Esc 等）。
  - 必須設定清晰的焦點樣式（`:focus-visible`），嚴禁無理由地 `outline: none`。
  - 表單元素需具有明確的 `aria-label`、`for` 與 `id` 關聯。

---

## 2. 目錄架構

```text
/
├── index.html            # 首頁與主介面
├── AGENTS.md             # Agent 規範手冊
├── README.md             # 專案說明與規劃
├── css/
│   └── style.css         # 全站樣式表（含品牌色 CSS 變數、RWD 與 a11y）
└── js/
    ├── data.js           # 本地商品、活動、評價與問答資料
    └── app.js            # 原生互動邏輯（搜尋篩選、彈窗、預購清單、剪貼簿與下載）
```

---

## 3. 品牌色彩管理規範 (依據 @養樂多寵物樂園 YouTube 頻道視覺)

```css
:root {
  /* 品牌主色：樂園森林綠（自然健康、毛孩友善、活力森林感） */
  --color-primary: #0D7A57;
  --color-primary-hover: #085A3F;
  --color-primary-light: #E6F5EE;

  /* 品牌輔色：養樂多暖陽橙（活力、陽光、開朗朝氣） */
  --color-secondary: #F58220;
  --color-secondary-hover: #DE6D0E;
  --color-secondary-light: #FFF3E6;

  /* 強調色與重要 CTA：樂樂圍巾紅（核心行動呼籲、促銷熱門角標） */
  --color-accent: #DC2626;
  --color-accent-hover: #B91C1C;
  --color-accent-light: #FEE2E2;

  /* 晴空藍與大地原木棕 */
  --color-sky: #0284C7;
  --color-wood: #854D0E;
  --color-wood-light: #FEF3C7;

  /* 底色與文字 */
  --color-bg: #FAF8F5;             /* 溫潤乳酸菌米白底色 */
  --color-surface: #FFFFFF;        /* 純白卡片表面 */
  --color-text-main: #2B2523;      /* 溫和木質炭褐黑文字 */
  --color-text-muted: #665E5A;     /* 次要文字灰 */
  --color-border: #E6DFC5;         /* 柔和米褐邊框 */

  /* 聚焦焦點環 */
  --focus-ring-color: #F58220;
}
```

---

## 4. 程式碼安全與實作指引

1. **安全與防護**：所有動態插入的文字內容（商品名稱、價格、描述、規格、評價、問答）必須嚴格使用 `textContent`，杜絕任何 XSS 隱患。
2. **無障礙體驗**：
   - 彈窗須支援 `Escape` 鍵與遮罩點擊關閉，關閉後焦點歸位。
   - FAQ 手風琴支援純鍵盤 `Enter` 與 `Space` 鍵展開。
