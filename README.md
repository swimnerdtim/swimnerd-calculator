# Swimnerd Swimming Calculator

Interactive swimming calculator with personalized pacing, championship models, training zones, and complete progression tracking.

Built for competitive swimmers to plan meets, analyze performance, and optimize training.

---

## Features

### 📊 4 Comprehensive Tabs

1. **Personal Bests** - Quick reference with split tooltips
2. **Meet Goals** - Race pacing with 4 models:
   - Personalized (your actual race splits)
   - Championship (world-class pacing from Olympics/Worlds/NCAA)
   - British Model (research-based)
   - Australian Model (research-based)
3. **Training Zones** - 6 zones with detailed checkpoints (printable)
4. **Progression** - Complete meet history (1,819 swims across 16 events)

### 🏆 Championship Data

**103 world-class speed charts** from:
- **Paris 2024 Olympics** (LCM)
- **2025 World Championships Singapore** (LCM)
- **2024 World Championships Budapest** (SCM)
- **2025 NCAA Division I Championships** (SCY)

Top 8 finalists only - medal contender pacing.

### ⚡ Technical Highlights

- **Self-contained** - All data embedded inline, no dependencies
- **Fast** - Pure HTML/CSS/JavaScript, no frameworks
- **Responsive** - Works on desktop, tablet, mobile
- **Print-friendly** - Training zones designed for printing

---

## Quick Start

### Option 1: Open Locally

```bash
# Download index.html
open index.html
# or
python3 -m http.server 8000
# Then visit http://localhost:8000
```

### Option 2: Deploy to GitHub Pages

```bash
# Fork this repo, then:
gh repo clone yourusername/swimnerd-calculator
cd swimnerd-calculator

# Enable GitHub Pages
gh repo edit --enable-pages --pages-branch main --pages-path /

# Visit: https://yourusername.github.io/swimnerd-calculator/
```

### Option 3: Deploy Anywhere

Upload `index.html` to any web server. No backend needed.

---

## Usage Guide

### Meet Goals Tab

**How to use:**
1. Select event (e.g., "100 Free")
2. Click goal time button (PB, -1%, -2%, -3%)
3. See 4 pacing models side-by-side:
   - **Personalized**: Your actual race splits
   - **Championship**: Top 8 Olympic/World/NCAA finalists
   - **British**: Research-based model
   - **Australian**: Research-based model

**Example: 100 Free SCY, 42.56s goal**
- Personalized: 50y = 20.34s, 100y = 42.56s
- Championship: 50y = 20.16s, 100y = 42.56s (NCAA 2025 finalists)

All models **auto-scale** to your goal time.

### Training Zones Tab

**6 intensity zones:**
1. Recovery (60-70% effort)
2. Aerobic (70-80% effort)
3. Threshold (80-90% effort)
4. VO2 Max (90-95% effort)
5. Anaerobic (95-98% effort)
6. Sprint (98-100% effort)

Each zone shows:
- Target pace per 100
- Detailed checkpoints (25y, 50y, 75y, 100y)
- Effort level description

**Print it** and bring to practice!

### Progression Tab

**Complete meet history:**
- 1,819 swims across 16 events
- All 3 courses (SCY, SCM, LCM)
- Most recent swims shown first
- Times >60s formatted as MM:SS

**Use it to:**
- Track improvement over time
- Compare across courses
- Identify strengths/weaknesses
- Plan goal times

---

## Data Coverage

### Personal Bests (All Courses)

| Event | SCY | SCM | LCM |
|-------|-----|-----|-----|
| 50 Free | 21.37 | 24.50 | 27.41 |
| 100 Free | 45.69 | 52.94 | 1:00.44 |
| 200 Free | 1:38.32 | 1:50.77 | 2:08.42 |
| 500/400 Free | 4:27.08 | 4:04.24 | 4:33.11 |
| 1000/800 Free | 9:19.44 | 8:51.19 | - |
| 1650/1500 Free | 15:44.66 | - | - |
| 50 Back | 23.71 | 27.65 | 30.75 |
| 100 Back | 51.00 | 58.75 | 1:08.41 |
| 200 Back | 1:48.33 | 2:05.83 | 2:26.59 |
| 50 Breast | 27.07 | 30.73 | 34.46 |
| 100 Breast | 58.04 | 1:06.75 | 1:17.63 |
| 200 Breast | 2:07.43 | 2:23.75 | 2:48.56 |
| 50 Fly | 23.34 | 26.64 | 28.89 |
| 100 Fly | 51.23 | 58.78 | 1:06.43 |
| 200 Fly | 1:54.29 | 2:11.15 | 2:31.69 |
| 100 IM | 52.65 | 1:01.47 | 1:09.64 |
| 200 IM | 1:53.37 | 2:10.27 | 2:28.43 |
| 400 IM | 4:00.60 | 4:36.87 | 5:23.14 |

### Championship Speed Charts

**79 events total:**
- **LCM**: 47 events (Olympics + Worlds)
- **SCM**: 32 events (Worlds)
- **SCY**: 24 events (NCAA) *NEW*

**Coverage:**
- Men's and Women's
- All Olympic/NCAA events
- Top 8 finalists (medal contenders)
- Average splits + pacing percentages

---

## Technical Details

### File Size

- **index.html**: 288 KB
- All data embedded inline (no external files)
- 103 championship speed charts
- 1,819 progression swims
- 6 training zones × 16 events

### Browser Support

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Mobile browsers (iOS Safari, Chrome Android)

### No Dependencies

- Pure HTML5/CSS3/JavaScript
- No jQuery, React, Vue, etc.
- Works offline after first load
- No API calls or backend

---

## Deployment Options

### GitHub Pages (Free, Easy)

1. Fork this repo
2. Settings → Pages → Source: main branch
3. Visit: `https://username.github.io/swimnerd-calculator/`

### Netlify (Free, Auto-Deploy)

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
cd swimnerd-calculator
netlify deploy --prod
```

### Vercel (Free, Fast CDN)

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
cd swimnerd-calculator
vercel --prod
```

### Custom Domain

Upload `index.html` to:
- `calculator.swimnerd.com`
- `tools.swimnerd.com/calculator`
- Anywhere with HTTPS

---

## Customization

### Add Your Own Data

**Replace personal bests:**
```javascript
// Line ~50 in index.html
const personalBests = {
  "50-free": { scy: 21.37, scm: 24.50, lcm: 27.41 },
  // ... edit these values
};
```

**Add custom training zones:**
```javascript
// Line ~200
const trainingZones = [
  { name: "Recovery", effort: "60-70%", color: "#4CAF50" },
  // ... add your zones
];
```

### Styling

**Change colors:**
```css
/* Line ~30 */
:root {
  --primary-color: #1976d2;
  --secondary-color: #424242;
  /* ... edit theme */
}
```

---

## Data Sources

**Personal data:**
- SwimCloud athlete profile
- Meet results (1,819 swims)
- Split analysis from actual races

**Championship data:**
- Paris 2024 Olympics (Omega timing XLSX)
- 2025 World Championships Singapore (LENEX XML)
- 2024 World Championships Budapest (LENEX XML)
- 2025 NCAA Division I Championships (official results)

**Research models:**
- British Swimming pacing research
- Australian Institute of Sport guidelines

---

## Known Limitations

### Championship Model

**Not available for:**
- 500/1000/1650 Free (SCY only, not Olympic events)
- Some SCM/LCM combinations

**Why:** Championship data only includes events contested at major meets.

### Personalized Model

**Shows "No data" when:**
- No splits recorded for that event/course
- Event not yet swum in that course

**Solution:** Swim the event! Splits will populate automatically.

---

## Future Enhancements

**Potential features:**
- Time converter (SCY ↔ SCM ↔ LCM)
- Relay splits calculator
- Taper progression tracking
- Export to PDF/CSV
- Mobile app version

**Want to contribute?** Open an issue or PR!

---

## For Pavan (Swimnerd Integration)

See `INTEGRATION_GUIDE.md` for:
- React/Vue conversion tips
- API integration patterns
- User authentication flow
- Multi-swimmer support

Quick start:
1. Self-contained HTML works as-is
2. Can iframe into existing site
3. Or extract JS logic for React components

---

## License

MIT License - Free for personal and commercial use.

---

## Support

Questions? Contact:
- **Nate Tschohl** - @SwimNerds
- GitHub Issues: [Report bugs here]

---

**Built for swimmers, by swimmers** 🏊‍♂️  
Train smarter. Race faster.
