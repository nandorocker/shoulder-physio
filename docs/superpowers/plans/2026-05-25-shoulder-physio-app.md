# Shoulder Physio App Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a mobile-friendly, publicly hostable static page that renders all 10 shoulder rehab exercises as a collapsible accordion list with drag-to-reorder in edit mode.

**Architecture:** Single `index.html` with inline CSS and JS. Exercise data is a JS const array. SortableJS from CDN handles touch-friendly drag reorder. Order persisted to localStorage.

**Tech Stack:** Vanilla HTML/CSS/JS, SortableJS 1.15 (CDN), GitHub Pages for hosting.

---

## File Map

| File | Action | Purpose |
|---|---|---|
| `index.html` | Create | Entire app — data, styles, logic |
| `assets/images/.gitkeep` | Create | Placeholder so folder is tracked by git |
| `assets/videos/.gitkeep` | Create | Placeholder for future video files |
| `.gitignore` | Create | Ignore OS files, .superpowers/ |

---

## Task 1: Project Scaffold

**Files:**
- Create: `index.html`
- Create: `assets/images/.gitkeep`
- Create: `assets/videos/.gitkeep`
- Create: `.gitignore`

- [ ] **Step 1.1: Create directory structure**

```bash
mkdir -p assets/images assets/videos
touch assets/images/.gitkeep assets/videos/.gitkeep
```

- [ ] **Step 1.2: Create `.gitignore`**

```
.DS_Store
.superpowers/
*.log
```

- [ ] **Step 1.3: Create `index.html` shell**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="default">
  <title>Shoulder Rehab</title>
  <style>
    /* styles go here — Task 2 */
  </style>
</head>
<body>
  <div id="app">
    <header id="app-header">
      <div class="header-left">
        <h1>Shoulder Rehab</h1>
        <span class="badge">4× / week</span>
      </div>
      <button id="edit-btn">Edit</button>
    </header>
    <main>
      <ul id="exercise-list" role="list"></ul>
      <div id="sections-list"></div>
    </main>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/sortablejs@1.15.0/Sortable.min.js"></script>
  <script>
    // data + logic go here — Tasks 3–6
  </script>
</body>
</html>
```

- [ ] **Step 1.4: Open in browser and confirm blank page with no errors**

Open `index.html` in Chrome/Safari. DevTools console should show no errors.

- [ ] **Step 1.5: Commit**

```bash
git init
git add index.html assets/ .gitignore
git commit -m "chore: project scaffold"
```

---

## Task 2: CSS Styles

**Files:**
- Modify: `index.html` — replace `/* styles go here */` comment with full styles

- [ ] **Step 2.1: Replace the style block content with the full CSS**

Replace `/* styles go here — Task 2 */` with:

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg: #f2f2f7;
  --card-bg: #ffffff;
  --accent: #007aff;
  --text: #1c1c1e;
  --text-secondary: #6e6e73;
  --separator: #e5e5ea;
  --handle-color: #c7c7cc;
  --badge-bg: #e5e5ea;
  --header-height: 56px;
  --radius: 12px;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100dvh;
  -webkit-font-smoothing: antialiased;
}

#app {
  max-width: 430px;
  margin: 0 auto;
}

/* Header */
#app-header {
  position: sticky;
  top: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  padding-top: max(12px, env(safe-area-inset-top));
  background: rgba(242, 242, 247, 0.92);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-bottom: 1px solid var(--separator);
}

.header-left {
  display: flex;
  align-items: baseline;
  gap: 8px;
}

#app-header h1 {
  font-size: 20px;
  font-weight: 700;
  letter-spacing: -0.3px;
}

.badge {
  font-size: 12px;
  font-weight: 600;
  color: var(--text-secondary);
  background: var(--badge-bg);
  padding: 2px 8px;
  border-radius: 20px;
}

#edit-btn {
  font-size: 15px;
  font-weight: 600;
  color: var(--accent);
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px 0;
  -webkit-tap-highlight-color: transparent;
}

/* Main */
main {
  padding: 20px 16px;
  padding-bottom: max(20px, env(safe-area-inset-bottom));
  display: flex;
  flex-direction: column;
  gap: 12px;
}

/* Exercise list */
#exercise-list {
  list-style: none;
  background: var(--card-bg);
  border-radius: var(--radius);
  overflow: hidden;
  box-shadow: 0 1px 3px rgba(0,0,0,0.08);
}

/* Exercise row */
.exercise-item {
  border-bottom: 1px solid var(--separator);
}
.exercise-item:last-child {
  border-bottom: none;
}

.exercise-row {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 14px;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
  min-height: 64px;
}

.drag-handle {
  display: none;
  color: var(--handle-color);
  font-size: 18px;
  padding: 0 4px;
  cursor: grab;
  flex-shrink: 0;
}

body.edit-mode .drag-handle {
  display: flex;
  align-items: center;
}

.exercise-number {
  font-size: 12px;
  font-weight: 600;
  color: var(--text-secondary);
  background: var(--badge-bg);
  width: 24px;
  height: 24px;
  border-radius: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

body.edit-mode .exercise-number {
  display: none;
}

.exercise-thumb {
  width: 44px;
  height: 44px;
  border-radius: 8px;
  object-fit: cover;
  background: var(--separator);
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.exercise-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.exercise-thumb-placeholder {
  width: 44px;
  height: 44px;
  border-radius: 8px;
  background: var(--separator);
  flex-shrink: 0;
}

.exercise-info {
  flex: 1;
  min-width: 0;
}

.exercise-name {
  font-size: 15px;
  font-weight: 600;
  color: var(--text);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.exercise-volume {
  font-size: 12px;
  color: var(--text-secondary);
  margin-top: 2px;
}

.chevron {
  color: var(--handle-color);
  font-size: 16px;
  transition: transform 200ms ease;
  flex-shrink: 0;
}

.exercise-item.open .chevron {
  transform: rotate(90deg);
}

body.edit-mode .chevron {
  display: none;
}

/* Expanded content */
.exercise-detail {
  display: none;
  padding: 0 14px 14px;
  background: var(--bg);
}

.exercise-item.open .exercise-detail {
  display: block;
}

.media-area {
  display: none; /* shown when exercise has image/video */
  margin-bottom: 14px;
  border-radius: 8px;
  overflow: hidden;
  background: var(--separator);
  min-height: 120px;
}

.media-area.has-media {
  display: block;
}

.media-area img {
  width: 100%;
  height: auto;
  display: block;
}

.detail-section {
  margin-bottom: 12px;
}

.detail-section:last-child {
  margin-bottom: 0;
}

.detail-label {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: var(--text-secondary);
  margin-bottom: 4px;
}

.detail-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 3px;
}

.detail-list li {
  font-size: 14px;
  color: var(--text);
  line-height: 1.4;
  padding-left: 14px;
  position: relative;
}

.detail-list li::before {
  content: "·";
  position: absolute;
  left: 4px;
  color: var(--text-secondary);
}

.detail-volume {
  font-size: 14px;
  font-weight: 600;
  color: var(--text);
}

/* Bottom sections */
#sections-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.section-card {
  background: var(--card-bg);
  border-radius: var(--radius);
  overflow: hidden;
  box-shadow: 0 1px 3px rgba(0,0,0,0.08);
}

.section-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px 16px;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}

.section-title {
  font-size: 16px;
  font-weight: 600;
}

.section-chevron {
  color: var(--handle-color);
  font-size: 16px;
  transition: transform 200ms ease;
}

.section-card.open .section-chevron {
  transform: rotate(90deg);
}

.section-body {
  display: none;
  padding: 0 16px 16px;
  border-top: 1px solid var(--separator);
}

.section-card.open .section-body {
  display: block;
}

.section-group {
  margin-top: 12px;
}

.section-group-title {
  font-size: 12px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: var(--text-secondary);
  margin-bottom: 6px;
}

.section-group ul {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.section-group li {
  font-size: 14px;
  color: var(--text);
  line-height: 1.4;
  padding-left: 14px;
  position: relative;
}

.section-group li::before {
  content: "·";
  position: absolute;
  left: 4px;
  color: var(--text-secondary);
}

/* SortableJS ghost + drag state */
.sortable-ghost {
  opacity: 0.4;
}
.sortable-drag {
  opacity: 1;
  box-shadow: 0 4px 16px rgba(0,0,0,0.15);
}
```

- [ ] **Step 2.2: Open in browser, confirm header and background color render**

You should see a sticky header "Shoulder Rehab" with "4× / week" badge and an "Edit" button. Background should be light grey.

- [ ] **Step 2.3: Commit**

```bash
git add index.html
git commit -m "feat: add CSS styles"
```

---

## Task 3: Exercise Data

**Files:**
- Modify: `index.html` — replace `// data + logic go here` with data arrays

- [ ] **Step 3.1: Add exercise data array inside the `<script>` block**

Replace `// data + logic go here — Tasks 3–6` with:

```js
const EXERCISES = [
  {
    id: "chin-tuck",
    name: "Band-Resisted Chin Tuck",
    category: "Neck / posture activation",
    image: null,
    volume: "10 reps · 2 sets",
    setup: [
      "Stand upright with good posture.",
      "Resistance band goes behind the back of the head.",
      "Arms are extended forward holding the band."
    ],
    movement: [
      "Gently move the head backward against the resistance.",
      "Motion is subtle — like making a "double chin."",
      "Do not tilt the head upward.",
      "Hold for a couple of seconds.",
      "Return slowly to neutral."
    ],
    cues: [
      "Keep shoulders relaxed.",
      "Avoid shrugging.",
      "Keep the movement small and controlled."
    ]
  },
  {
    id: "wall-slide",
    name: "Foam Roller Wall Slide",
    category: "Shoulder mobility",
    image: null,
    volume: "10 reps · 2 sets",
    setup: [
      "Sit facing a wall.",
      "Place a long foam roller horizontally against the wall.",
      "Put a resistance band around both wrists.",
      "Elbows are bent in front of you, hands pointing upward.",
      "Keep slight outward tension on the band."
    ],
    movement: [
      "Slide the foam roller upward along the wall.",
      "Keep pushing gently outward into the resistance band.",
      "Go up until the pain limit.",
      "Slide back down slowly."
    ],
    cues: [
      "Keep shoulders down.",
      "Do not shrug.",
      "Keep movement smooth.",
      "Stop at pain level."
    ]
  },
  {
    id: "prayer-stretch",
    name: "Foam Roller Prayer Stretch",
    category: "Shoulder mobility / lat stretch",
    image: null,
    volume: "8–10 slow reps · 1–2 sets",
    setup: [
      "Stand facing a high table or bed.",
      "Place the long foam roller on the table or bed.",
      "Put both hands or forearms on the roller.",
      "Hinge forward at the hips, keeping back long and neutral."
    ],
    movement: [
      "Roll arms forward while moving hips backward.",
      "Let chest lower gently toward the floor.",
      "Feel stretch through shoulders, lats, and upper back.",
      "Return slightly, then repeat.",
      "Optional: pause 2–3 seconds at end of each rep."
    ],
    cues: [
      "Keep the back long.",
      "Do not collapse into the shoulder.",
      "Do not force the stretch.",
      "Stop at pain level."
    ]
  },
  {
    id: "ball-wall",
    name: "Ball-on-Wall Stability Drill",
    category: "Scapular control",
    image: null,
    volume: "10 reps each direction · 2 sets",
    setup: [
      "Use a small ball against the wall.",
      "Place hand or forearm on the ball.",
      "Keep arm at a comfortable, non-painful height."
    ],
    movement: [
      "Up and down — controlled.",
      "Side to side — controlled.",
      "Small controlled circles or directional movements."
    ],
    cues: [
      "Keep shoulder down.",
      "Do not raise arm too high.",
      "Keep movements small and controlled.",
      "Avoid painful range."
    ]
  },
  {
    id: "thread-needle",
    name: "Quadruped Thread-the-Needle",
    category: "Scapular control / thoracic rotation",
    image: null,
    volume: "10 reps each side · 2 sets",
    setup: [
      "Start on hands and knees.",
      "Hands under shoulders, knees under hips."
    ],
    movement: [
      "Reach one arm forward.",
      "Rotate outward and open to the side.",
      "Reach underneath torso toward the opposite side.",
      "Return to start position."
    ],
    cues: [
      "Move slowly.",
      "Do not collapse downward.",
      "Keep the motion controlled.",
      "Stay within comfortable range."
    ]
  },
  {
    id: "half-squat-band",
    name: "Half Squat + Band Shoulder Control",
    category: "Resistance band strengthening",
    image: null,
    volume: "10 reps · 2 sets",
    setup: [
      "Anchor resistance band low.",
      "Stand in a slight half squat, back straight.",
      "Hold the band with arms extended forward."
    ],
    movement: [
      "Pull with controlled movement.",
      "Elbows move down and back.",
      "Arms move outward and forward under control.",
      "Maintain tension throughout."
    ],
    cues: [
      "Do not raise shoulders.",
      "Keep shoulder blades engaged.",
      "Avoid upper trap compensation."
    ]
  },
  {
    id: "shoulder-circle",
    name: "Single-Arm Banded Shoulder Circle",
    category: "Resistance band strengthening",
    image: null,
    volume: "10 circles each direction · 2 sets",
    setup: [
      "Stand on one end of a long resistance band.",
      "Hold the other end with one hand.",
      "Arm is slightly below shoulder height, in the scapular plane.",
      "Forearm is closer to the chest, not fully out to the side."
    ],
    movement: [
      "Raise the arm forward and outward like holding a flag.",
      "Make controlled circular movements.",
      "Keep shoulder down.",
      "Perform circles in both directions."
    ],
    cues: [
      "Do not shrug.",
      "Do not turn this into a deltoid workout.",
      "Keep the arm slightly below shoulder height.",
      "Stop at pain level."
    ]
  },
  {
    id: "wing-spread",
    name: "Single-Arm Banded Wing Spread",
    category: "Resistance band strengthening",
    image: null,
    volume: "10 reps · 2 sets",
    setup: [
      "Same setup as Exercise 7.",
      "Stand on the band, hold with one hand.",
      "Arm begins slightly forward and below shoulder height."
    ],
    movement: [
      "Reach forward slightly with the shoulder.",
      "Then open the arm outward, like spreading a wing.",
      "Return slowly to the starting position."
    ],
    cues: [
      "Keep shoulder down.",
      "Keep movement controlled.",
      "Avoid pinching or sharp pain.",
      "Stop at pain level."
    ]
  },
  {
    id: "serratus-reach",
    name: "Banded Forward Serratus Reach",
    category: "Resistance band strengthening",
    image: null,
    volume: "10 reps · 2 sets",
    setup: [
      "Stand upright, elbows bent at your sides.",
      "Resistance band around wrists.",
      "Wrists stay under shoulder line.",
      "Keep outward tension on the band."
    ],
    movement: [
      "Reach both arms forward.",
      "Keep the band tense.",
      "Hold for about 2 seconds.",
      "Return slowly to the starting position."
    ],
    cues: [
      "Do not shrug.",
      "Keep shoulders down.",
      "Keep band tension the whole time.",
      "Move smoothly."
    ]
  },
  {
    id: "prone-dumbbell",
    name: "Prone Dumbbell Shoulder Sequence",
    category: "Resistance band / dumbbell strengthening",
    image: null,
    volume: "10 reps each movement · 2 sets",
    setup: [
      "Lie face down on a bench or table.",
      "Support forehead to avoid neck strain.",
      "Arms begin hanging downward.",
      "Use very light weight."
    ],
    movement: [
      "A — Side raises: lift arms sideways, do not raise too high, lower slowly.",
      "B — Forward/backward: move arms forward and backward under control.",
      "C — Small circles: 5 in one direction, 5 in the other direction."
    ],
    cues: [
      "Use low weight.",
      "Avoid shrugging.",
      "Keep movement slow and controlled."
    ]
  }
];

const SECTIONS = [
  {
    id: "daily-stretches",
    title: "Daily Stretches",
    subtitle: "Morning and Before Bed",
    groups: [
      {
        title: "Neck Stretch 1",
        items: [
          "2 reps each side.",
          "One hand holds top of head, tilt head toward that shoulder.",
          "Opposite arm hangs down with palm facing forward.",
          "Push opposite fingertips gently toward the floor.",
          "Hold around 5 seconds. Breathe and gently move further into range."
        ]
      },
      {
        title: "Neck Stretch 2",
        items: [
          "2 reps each side.",
          "One hand holds back of head, head faces diagonally down toward the shoulder.",
          "Opposite arm moves gently backward.",
          "Hold around 5 seconds. Breathe and gently move further into range."
        ]
      },
      {
        title: "Shoulder Shrugs",
        items: [
          "Shrug shoulders backward for 10 seconds.",
          "Shrug shoulders forward for 10 seconds."
        ]
      },
      {
        title: "Side-to-Side Head Rotation",
        items: [
          "Start looking sideways and slightly up.",
          "Rotate toward the other side while looking down.",
          "Finish looking sideways and slightly up on the other side.",
          "Do not force the upward position."
        ]
      }
    ]
  },
  {
    id: "heat-cold",
    title: "Heat / Cold",
    subtitle: "Recovery",
    groups: [
      {
        title: "Morning",
        items: [
          "Use warm water or heat on the shoulder for a few minutes."
        ]
      },
      {
        title: "After Exercises",
        items: [
          "Use cold pack or cold water for a few minutes if irritated.",
          "Around 2 minutes of cold water."
        ]
      }
    ]
  }
];
```

- [ ] **Step 3.2: Verify no JS syntax errors**

Open browser DevTools → Console. Confirm no errors after page load.

- [ ] **Step 3.3: Commit**

```bash
git add index.html
git commit -m "feat: add exercise and section data"
```

---

## Task 4: Render Functions + Initial Render

**Files:**
- Modify: `index.html` — add render functions below the data arrays

- [ ] **Step 4.1: Add render functions after the data arrays in the script block**

```js
// ─── Storage ────────────────────────────────────────────────────────────────
const STORAGE_KEY = 'shoulder-physio-order';

function loadOrder() {
  try {
    const saved = localStorage.getItem(STORAGE_KEY);
    if (!saved) return null;
    return JSON.parse(saved);
  } catch { return null; }
}

function saveOrder(ids) {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(ids));
}

function getOrderedExercises() {
  const savedIds = loadOrder();
  if (!savedIds) return EXERCISES;
  const map = Object.fromEntries(EXERCISES.map(e => [e.id, e]));
  return savedIds.map(id => map[id]).filter(Boolean);
}

// ─── Render helpers ──────────────────────────────────────────────────────────
function renderList(ul, items) {
  ul.innerHTML = '';
  items.forEach((ex, index) => {
    ul.appendChild(renderExerciseItem(ex, index + 1));
  });
}

function renderExerciseItem(ex, displayNumber) {
  const li = document.createElement('li');
  li.className = 'exercise-item';
  li.dataset.id = ex.id;

  const thumbHTML = ex.image
    ? `<div class="exercise-thumb"><img src="${ex.image}" alt="${ex.name}" loading="lazy"></div>`
    : `<div class="exercise-thumb-placeholder"></div>`;

  const mediaHTML = ex.image
    ? `<div class="media-area has-media"><img src="${ex.image}" alt="${ex.name}"></div>`
    : `<div class="media-area"></div>`;

  li.innerHTML = `
    <div class="exercise-row" role="button" aria-expanded="false">
      <span class="drag-handle" aria-hidden="true">≡</span>
      <span class="exercise-number">${displayNumber}</span>
      ${thumbHTML}
      <div class="exercise-info">
        <div class="exercise-name">${ex.name}</div>
        <div class="exercise-volume">${ex.volume}</div>
      </div>
      <span class="chevron" aria-hidden="true">›</span>
    </div>
    <div class="exercise-detail" aria-hidden="true">
      ${mediaHTML}
      <div class="detail-section">
        <div class="detail-label">Setup</div>
        <ul class="detail-list">${ex.setup.map(s => `<li>${s}</li>`).join('')}</ul>
      </div>
      <div class="detail-section">
        <div class="detail-label">Movement</div>
        <ul class="detail-list">${ex.movement.map(s => `<li>${s}</li>`).join('')}</ul>
      </div>
      <div class="detail-section">
        <div class="detail-label">Important Cues</div>
        <ul class="detail-list">${ex.cues.map(s => `<li>${s}</li>`).join('')}</ul>
      </div>
      <div class="detail-section">
        <div class="detail-label">Volume</div>
        <div class="detail-volume">${ex.volume}</div>
      </div>
    </div>
  `;

  // Expand/collapse on row click
  li.querySelector('.exercise-row').addEventListener('click', () => {
    if (document.body.classList.contains('edit-mode')) return;
    const isOpen = li.classList.contains('open');
    // Close all
    document.querySelectorAll('#exercise-list .exercise-item.open').forEach(el => {
      el.classList.remove('open');
      el.querySelector('.exercise-row').setAttribute('aria-expanded', 'false');
      el.querySelector('.exercise-detail').setAttribute('aria-hidden', 'true');
    });
    if (!isOpen) {
      li.classList.add('open');
      li.querySelector('.exercise-row').setAttribute('aria-expanded', 'true');
      li.querySelector('.exercise-detail').setAttribute('aria-hidden', 'false');
    }
  });

  return li;
}

function renderSections(container) {
  container.innerHTML = '';
  SECTIONS.forEach(section => {
    const div = document.createElement('div');
    div.className = 'section-card';
    div.dataset.id = section.id;

    const groupsHTML = section.groups.map(g => `
      <div class="section-group">
        <div class="section-group-title">${g.title}</div>
        <ul>${g.items.map(item => `<li>${item}</li>`).join('')}</ul>
      </div>
    `).join('');

    div.innerHTML = `
      <div class="section-header" role="button" aria-expanded="false">
        <div>
          <div class="section-title">${section.title}</div>
        </div>
        <span class="section-chevron" aria-hidden="true">›</span>
      </div>
      <div class="section-body" aria-hidden="true">
        ${groupsHTML}
      </div>
    `;

    div.querySelector('.section-header').addEventListener('click', () => {
      const isOpen = div.classList.contains('open');
      div.classList.toggle('open', !isOpen);
      div.querySelector('.section-header').setAttribute('aria-expanded', String(!isOpen));
      div.querySelector('.section-body').setAttribute('aria-hidden', String(isOpen));
    });

    container.appendChild(div);
  });
}

// ─── Init ────────────────────────────────────────────────────────────────────
const exerciseList = document.getElementById('exercise-list');
const sectionsContainer = document.getElementById('sections-list');

renderList(exerciseList, getOrderedExercises());
renderSections(sectionsContainer);
```

- [ ] **Step 4.2: Verify in browser**

Reload. You should see all 10 exercises listed with placeholder thumbnails. Tapping a row should expand it showing Setup, Movement, Cues, Volume. Only one row open at a time.

- [ ] **Step 4.3: Commit**

```bash
git add index.html
git commit -m "feat: render exercise list with accordion expand/collapse"
```

---

## Task 5: Edit Mode + SortableJS

**Files:**
- Modify: `index.html` — add edit mode logic after the init block

- [ ] **Step 5.1: Add edit mode wiring after the `renderSections(...)` call**

```js
// ─── Edit mode ───────────────────────────────────────────────────────────────
const editBtn = document.getElementById('edit-btn');
let sortable = null;

function enterEditMode() {
  document.body.classList.add('edit-mode');
  editBtn.textContent = 'Done';

  sortable = Sortable.create(exerciseList, {
    animation: 150,
    handle: '.drag-handle',
    ghostClass: 'sortable-ghost',
    dragClass: 'sortable-drag',
    onEnd() {
      updateDisplayNumbers();
    }
  });
}

function exitEditMode() {
  document.body.classList.remove('edit-mode');
  editBtn.textContent = 'Edit';

  if (sortable) {
    sortable.destroy();
    sortable = null;
  }

  // Save new order
  const ids = [...exerciseList.querySelectorAll('.exercise-item')].map(li => li.dataset.id);
  saveOrder(ids);
}

function updateDisplayNumbers() {
  exerciseList.querySelectorAll('.exercise-item').forEach((li, i) => {
    li.querySelector('.exercise-number').textContent = i + 1;
  });
}

editBtn.addEventListener('click', () => {
  if (document.body.classList.contains('edit-mode')) {
    exitEditMode();
  } else {
    enterEditMode();
  }
});
```

- [ ] **Step 5.2: Verify in browser**

Tap "Edit". Drag handles (≡) should appear on each row. Drag rows to reorder them. Tap "Done". Order should persist on reload.

Check DevTools Application → Local Storage. Key `shoulder-physio-order` should contain an array of exercise IDs in the new order.

- [ ] **Step 5.3: Commit**

```bash
git add index.html
git commit -m "feat: add edit mode with drag-to-reorder and localStorage persistence"
```

---

## Task 6: Mobile Polish + Final Touches

**Files:**
- Modify: `index.html` — small CSS additions

- [ ] **Step 6.1: Add active/pressed state for rows so taps feel responsive**

Add these rules inside the `<style>` block, after the `.chevron` rule:

```css
.exercise-row:active {
  background: var(--separator);
}

.section-header:active {
  background: var(--separator);
}
```

- [ ] **Step 6.2: Add a subtle intro paragraph below the header**

Inside `<main>`, before `<ul id="exercise-list">`, add:

```html
<p style="font-size:13px; color: var(--text-secondary); padding: 0 2px 4px;">
  Keep shoulder <strong>down</strong> throughout. Stop at pain. Move slowly.
</p>
```

- [ ] **Step 6.3: Verify on mobile (or DevTools mobile emulation)**

In Chrome DevTools, switch to mobile emulation (iPhone 14 Pro). Confirm:
- Header sticks on scroll
- Tapping rows feels snappy
- Drag handles work with touch
- Safe area insets render correctly at bottom

- [ ] **Step 6.4: Add `.gitignore` entry for the visual companion session files (if not already present)**

Confirm `.gitignore` contains `.superpowers/`.

- [ ] **Step 6.5: Final commit**

```bash
git add index.html
git commit -m "feat: mobile polish and active states"
```

---

## Task 7: GitHub Pages Deployment

- [ ] **Step 7.1: Create a GitHub repository**

Go to github.com → New repository. Name it `shoulder-physio`. Make it public (required for free GitHub Pages). Do not initialize with README.

- [ ] **Step 7.2: Push to GitHub**

```bash
git remote add origin https://github.com/<your-username>/shoulder-physio.git
git branch -M main
git push -u origin main
```

- [ ] **Step 7.3: Enable GitHub Pages**

GitHub repo → Settings → Pages → Source: "Deploy from a branch" → Branch: `main` → Folder: `/ (root)` → Save.

- [ ] **Step 7.4: Verify**

After ~60 seconds, visit `https://<your-username>.github.io/shoulder-physio/`. Full app should load. Test accordion and reorder on your phone.

---

## Self-Review Notes

- All 10 exercises covered in data (verified against source doc)
- `localStorage` order loads on page init via `getOrderedExercises()`
- `updateDisplayNumbers()` called after each drag so numbers stay accurate
- `image: null` falls back gracefully to `.exercise-thumb-placeholder` — no broken img tags
- Bottom sections (Daily Stretches, Heat/Cold) excluded from SortableJS scope
- `aria-expanded` and `aria-hidden` kept in sync for basic accessibility
- Media area reserved with class `media-area` — shows when `has-media` class present
- SortableJS destroyed (not just disabled) on exit to avoid ghost listeners
