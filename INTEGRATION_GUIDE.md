# Integration Guide for Pavan

How to integrate the Swimnerd Calculator into existing infrastructure.

---

## Quick Integration Options

### Option 1: Iframe (Fastest - 5 minutes)

**Pros:** Zero code changes, works immediately  
**Cons:** Isolated state, harder to customize

```html
<!-- In your existing Swimnerd site -->
<iframe 
  src="https://calculator.swimnerd.com" 
  width="100%" 
  height="800px"
  frameborder="0">
</iframe>
```

### Option 2: Direct HTML (Easy - 30 minutes)

**Pros:** Full control, easy styling  
**Cons:** Need to maintain HTML file

1. Host `index.html` on your domain
2. Link from main nav: `<a href="/calculator">Calculator</a>`
3. Done!

### Option 3: React Component (Best - 2-4 hours)

**Pros:** Integrated UX, shared state, extensible  
**Cons:** Requires refactoring HTML → React

See below for conversion guide.

---

## React Conversion Guide

### 1. Extract Data (10 minutes)

**Create:** `src/data/swimData.js`

```javascript
export const personalBests = {
  "50-free": { scy: 21.37, scm: 24.50, lcm: 27.41 },
  "100-free": { scy: 45.69, scm: 52.94, lcm: 60.44 },
  // ... copy from index.html line 50
};

export const championshipData = {
  "100-free-men-scy": {
    competition: "NCAA Division I 2025",
    sampleSize: 8,
    checkpoints: [
      { distance: "50y", avgTime: 20.16, pacing: 47.4 },
      { distance: "100y", avgTime: 42.53, pacing: 100.0 }
    ]
  },
  // ... copy from index.html line 500
};

export const progressionSwims = [
  { event: "100 Free", course: "SCY", time: 45.69, date: "2024-03-15", meet: "NCAA Championships" },
  // ... copy from index.html line 2000
];
```

### 2. Create Components (1 hour)

**Component structure:**
```
src/
  components/
    Calculator/
      Calculator.jsx           # Main container
      PersonalBests.jsx        # Tab 1
      MeetGoals.jsx            # Tab 2
      TrainingZones.jsx        # Tab 3
      Progression.jsx          # Tab 4
      EventSelector.jsx        # Shared
      CourseSelector.jsx       # Shared
```

**Example: MeetGoals.jsx**

```jsx
import React, { useState } from 'react';
import { personalBests, championshipData } from '../../data/swimData';

const MeetGoals = () => {
  const [event, setEvent] = useState('100-free');
  const [course, setCourse] = useState('scy');
  const [goalTime, setGoalTime] = useState(null);
  
  const pb = personalBests[event][course];
  const championship = championshipData[`${event}-men-${course}`];
  
  const calculatePacing = (model, baseTime) => {
    // Extract pacing logic from index.html
    return model.checkpoints.map(cp => ({
      distance: cp.distance,
      time: baseTime * (cp.pacing / 100)
    }));
  };
  
  return (
    <div className="meet-goals">
      <EventSelector value={event} onChange={setEvent} />
      <CourseSelector value={course} onChange={setCourse} />
      
      <div className="goal-buttons">
        <button onClick={() => setGoalTime(pb)}>PB</button>
        <button onClick={() => setGoalTime(pb * 0.99)}>-1%</button>
        <button onClick={() => setGoalTime(pb * 0.98)}>-2%</button>
      </div>
      
      {goalTime && (
        <div className="pacing-models">
          <PacingModel 
            title="Championship" 
            splits={calculatePacing(championship, goalTime)} 
          />
          {/* ... other models */}
        </div>
      )}
    </div>
  );
};

export default MeetGoals;
```

### 3. Styling (30 minutes)

**Option A: Extract CSS**
```bash
# Copy styles from index.html
cp index.html calculator.css
# Edit to remove HTML, keep only CSS
```

**Option B: Tailwind CSS**
```jsx
<div className="bg-blue-600 text-white p-4 rounded-lg">
  <h2 className="text-2xl font-bold">Meet Goals</h2>
</div>
```

**Option C: Styled Components**
```jsx
import styled from 'styled-components';

const Container = styled.div`
  background: #1976d2;
  color: white;
  padding: 1rem;
`;
```

### 4. State Management (30 minutes)

**Option A: Context (Simple)**
```jsx
// src/context/CalculatorContext.js
export const CalculatorContext = createContext();

export const CalculatorProvider = ({ children }) => {
  const [selectedEvent, setSelectedEvent] = useState('100-free');
  const [selectedCourse, setSelectedCourse] = useState('scy');
  
  return (
    <CalculatorContext.Provider value={{
      selectedEvent,
      setSelectedEvent,
      selectedCourse,
      setSelectedCourse
    }}>
      {children}
    </CalculatorContext.Provider>
  );
};
```

**Option B: Redux (Overkill)**
Not needed unless integrating with existing Redux store.

---

## Multi-Swimmer Support

### Database Schema

**Add these tables:**

```sql
-- Swimmers table
CREATE TABLE swimmers (
  id INT PRIMARY KEY,
  name VARCHAR(255),
  email VARCHAR(255) UNIQUE,
  created_at TIMESTAMP
);

-- Personal bests
CREATE TABLE personal_bests (
  id INT PRIMARY KEY,
  swimmer_id INT REFERENCES swimmers(id),
  event VARCHAR(50),  -- "100-free"
  course VARCHAR(10),  -- "scy", "scm", "lcm"
  time_seconds DECIMAL(10,2),
  updated_at TIMESTAMP
);

-- Meet results
CREATE TABLE meet_results (
  id INT PRIMARY KEY,
  swimmer_id INT REFERENCES swimmers(id),
  event VARCHAR(50),
  course VARCHAR(10),
  time_seconds DECIMAL(10,2),
  meet_name VARCHAR(255),
  meet_date DATE,
  splits JSONB  -- Store splits as JSON
);
```

### API Endpoints

**GET** `/api/swimmers/:id/personal-bests`
```json
{
  "100-free": {
    "scy": 45.69,
    "scm": 52.94,
    "lcm": 60.44
  }
}
```

**GET** `/api/swimmers/:id/progression/:event`
```json
[
  {
    "event": "100 Free",
    "course": "SCY",
    "time": 45.69,
    "date": "2024-03-15",
    "meet": "NCAA Championships",
    "splits": [20.34, 42.56]
  }
]
```

**POST** `/api/swimmers/:id/results`
```json
{
  "event": "100-free",
  "course": "scy",
  "time": 44.23,
  "meet": "Conference Championships",
  "date": "2026-02-20",
  "splits": [20.12, 44.23]
}
```

### React Integration

```jsx
// src/hooks/useSwimmerData.js
import { useQuery } from 'react-query';

export const useSwimmerData = (swimmerId) => {
  const { data: personalBests } = useQuery(
    ['personalBests', swimmerId],
    () => fetch(`/api/swimmers/${swimmerId}/personal-bests`).then(r => r.json())
  );
  
  const { data: progression } = useQuery(
    ['progression', swimmerId],
    () => fetch(`/api/swimmers/${swimmerId}/progression`).then(r => r.json())
  );
  
  return { personalBests, progression };
};

// Use in component
const Calculator = () => {
  const { swimmerId } = useAuth(); // Your auth hook
  const { personalBests, progression } = useSwimmerData(swimmerId);
  
  if (!personalBests) return <Loading />;
  
  return <MeetGoals personalBests={personalBests} />;
};
```

---

## Authentication Flow

### If you have auth already:

```jsx
import { useAuth } from '@/hooks/useAuth';

const Calculator = () => {
  const { user, isAuthenticated } = useAuth();
  
  if (!isAuthenticated) {
    return <Redirect to="/login" />;
  }
  
  return <CalculatorApp userId={user.id} />;
};
```

### If you don't have auth:

**Keep it simple:**
1. Start with demo data (current implementation)
2. Add "Save to Profile" button later
3. Users can manually export/import JSON

---

## Deployment

### Option 1: GitHub Pages

```yaml
# .github/workflows/deploy.yml
name: Deploy Calculator

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./
```

### Option 2: Netlify

```toml
# netlify.toml
[build]
  publish = "."

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### Option 3: Your Server

```bash
# Upload to existing Swimnerd infrastructure
scp index.html user@swimnerd.com:/var/www/calculator/
```

---

## Testing Checklist

- [ ] All 4 tabs load without errors
- [ ] Event selection updates all tabs
- [ ] Course selection (SCY/SCM/LCM) works
- [ ] Goal buttons calculate pacing correctly
- [ ] Championship model shows correct data
- [ ] Progression chart displays all swims
- [ ] Training zones calculate for all events
- [ ] Mobile responsive (test on iPhone/Android)
- [ ] Print layout works (Training Zones tab)
- [ ] No console errors

---

## Performance Optimization

### Current size: 288 KB

**If size is an issue:**

1. **Split data files**
   ```javascript
   // Load championship data on-demand
   const championship = await import('./data/championship.json');
   ```

2. **Compress with gzip**
   ```bash
   gzip -9 index.html  # → ~60 KB
   ```

3. **Use CDN**
   ```html
   <script src="https://cdn.swimnerd.com/calculator-data.js"></script>
   ```

---

## Support & Questions

**Common issues:**

**Q: Can multiple users use this?**  
A: Current version is single-swimmer. Add database + API for multi-user.

**Q: Can swimmers edit their own data?**  
A: Not yet. Add form inputs + save to localStorage or API.

**Q: Mobile app version?**  
A: Wrap in React Native WebView or use Capacitor.

**Q: Integrate with Swimnerd Live timing?**  
A: Yes! Export splits from Live → import to calculator.

---

## Timeline Estimate

| Task | Time | Difficulty |
|------|------|------------|
| Iframe integration | 5 min | Easy |
| Direct HTML hosting | 30 min | Easy |
| React conversion | 2-4 hours | Medium |
| Multi-user database | 1-2 days | Medium |
| API integration | 2-3 days | Medium |
| Mobile app | 1 week | Hard |

**Recommendation:** Start with Option 2 (Direct HTML), add React/DB later.

---

**Questions?** Ask Nate or open a GitHub issue.
