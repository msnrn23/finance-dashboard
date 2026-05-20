# Finance Dashboard — Expenses & HC Created for Testing By Sagar

A fully client-side finance dashboard that reads any `.xlsx` file and renders:
- Multi-year expense & headcount trends
- Year-over-year comparisons
- Business unit pivot tables with scenario filters
- Expense/HC breakdown with heatmaps and scatter plots

**No server required.** Everything runs in the browser. Host it anywhere static files are served — GitHub Pages, Netlify, Vercel, S3, etc.

git init && git add .  ------> **This is for add/delete any files from client directory to Git respositary **
git commit -m "Finance dashboard"
git branch -M main ------> **This is for main branch 
git remote add origin https://github.com/YOUR_USERNAME/finance-dashboard.git
git push -u origin main ------> **This is for add/delete any files from client directory to Git respositary **

---<img width="1224" height="746" alt="image" src="https://github.com/user-attachments/assets/9f43e4b2-68d5-4b55-80df-e08755573fb2" />





## Quick start (GitHub Pages — recommended)

### 1. Fork or create the repository

```bash
# Option A — clone this repo
git clone https://github.com/YOUR_USERNAME/finance-dashboard.git
cd finance-dashboard

# Option B — create a fresh repo and copy files
mkdir finance-dashboard && cd finance-dashboard
git init
# copy index.html into this folder
git add .
git commit -m "Initial dashboard"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/finance-dashboard.git
git push -u origin main
```

### 2. Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings → Pages**
3. Under **Source**, select **Deploy from a branch**
4. Set branch to **main**, folder to **/ (root)**
5. Click **Save**

Your dashboard will be live at:
```
https://YOUR_USERNAME.github.io/finance-dashboard/
```

(Takes ~60 seconds to deploy after each push.)

---

## How to use the dashboard

1. Open the URL above in any modern browser
2. Drag & drop your `.xlsx` file onto the upload area (or click to browse)
3. The dashboard auto-detects your data structure and renders all charts

**The file never leaves your browser** — it is processed entirely client-side via [SheetJS](https://sheetjs.com/). Nothing is uploaded to any server.

---

## Expected Excel format

The dashboard looks for a sheet whose name contains `input`, `data`, or `dummy` (case-insensitive). If none match, it uses the first sheet.

| Column | Type | Notes |
|---|---|---|
| `Scenario` | text | e.g. `2025 A`, `2026 P`, `2026 FY` |
| `Fiscal_Year` | number | e.g. `2025` |
| `Business_Unit` | text | Any BU name |
| `Region` | text | APAC, EMEA, etc. |
| `Expense_Category` | text | `Direct Cost`, `Indirect Cost`, `HC` |
| `Amount_USD` | number | Raw USD value |
| `Month` | text | Jan–Dec |
| `Gen_2` | text | `Compensation` or `Non-Compensation` |
| `Employee_Type` | text | `Employees` or `Contractors` |

Extra columns are ignored. Column names are normalised (spaces → underscores, trimmed), so minor variations are fine.

**Scenario auto-detection:** For each fiscal year, the dashboard prefers the actual scenario (`A` suffix), then plan (`P`), then full-year (`FY`). You can override via the scenario dropdowns in the Pivot and Breakdown tabs.

---

## File structure

```
finance-dashboard/
├── index.html          ← entire dashboard (single file, no build step)
└── README.md
```

Everything is bundled in `index.html` — Chart.js and SheetJS load from CDN at runtime. The only hard dependency is an internet connection for those libraries on first load (they are cached by the browser afterwards).

---

## Customisation

### Change the colour palette
Edit the `PALETTE` array near the top of the `<script>` block in `index.html`:
```js
const PALETTE = ['#3b82f6','#10b981','#f59e0b', ...];
```

### Add your own column mappings
If your column names differ from the expected ones, add aliases in the `norm()` function inside `parseData()`:
```js
const norm = r => {
  const out = {};
  for(const k in r){
    let key = k.trim().replace(/\s+/g,'_');
    // Example alias: map "BU_Name" → "Business_Unit"
    if(key === 'BU_Name') key = 'Business_Unit';
    out[key] = r[k];
  }
  return out;
};
```

### Change the GitHub link in the upload screen
Search for `YOUR_USERNAME` in `index.html` and replace with your actual GitHub username.

---

## Running locally (optional)

Because the file uses `FileReader` (not `fetch`), you can open `index.html` directly in Chrome or Firefox without a local server. Just double-click it.

If you prefer a local server:
```bash
# Python 3
python -m http.server 8080
# then open http://localhost:8080
```

---

## Tech stack

| Library | Version | Purpose |
|---|---|---|
| [SheetJS (xlsx)](https://sheetjs.com/) | 0.18.5 | Read .xlsx/.xls files in the browser |
| [Chart.js](https://www.chartjs.org/) | 4.4.1 | All charts and visualisations |
| [Google Fonts — DM Sans + DM Mono](https://fonts.google.com/) | — | Typography |

No build tools, no Node.js, no npm. Just a single HTML file.

---

## Browser support

Works in all modern browsers: Chrome 90+, Firefox 90+, Safari 14+, Edge 90+.
Internet Explorer is not supported.

---

## License

MIT — use freely, modify, redistribute.
