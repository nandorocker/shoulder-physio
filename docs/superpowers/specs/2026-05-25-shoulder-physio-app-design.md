# Shoulder Physio App — Design Spec

**Date:** 2026-05-25  
**Status:** Approved

---

## Overview

A mobile-friendly, publicly hosted static web app for the shoulder rehab routine. Displays exercises as an accordion list with a minimal collapsed view (number, thumbnail, name, volume) and full instructions when expanded. Supports drag-to-reorder with order persisted to localStorage.

---

## File Structure

```
shoulder-physio/
├── index.html          ← all data, styles, and logic in one file
├── assets/
│   ├── images/         ← one thumbnail per exercise (e.g. chin-tuck.jpg)
│   └── videos/         ← placeholder for future video files
└── docs/               ← existing markdown docs (unchanged)
```

---

## Tech Stack

- **Vanilla HTML/CSS/JS** — no build step
- **SortableJS** (CDN, ~10 KB) — drag-to-reorder on mobile touch
- **Deployment:** GitHub Pages (main branch root) or Netlify Drop

---

## Data Model

Exercises are defined as a JS array at the top of `index.html`. Each object:

```js
{
  id: "chin-tuck",                          // kebab-case unique ID, used for DOM and localStorage
  name: "Band-Resisted Chin Tuck",
  category: "Neck / posture activation",
  image: "assets/images/chin-tuck.jpg",     // optional; falls back to grey placeholder
  volume: "10 reps · 2 sets",
  setup: [                                  // array of strings, one per bullet
    "Stand upright with good posture.",
    "Resistance band goes behind the back of the head.",
    "Arms are extended forward holding the band."
  ],
  movement: [
    "Gently move the head backward against the resistance.",
    "Motion is subtle — like making a 'double chin.'",
    "Hold for a couple of seconds.",
    "Return slowly to neutral."
  ],
  cues: [
    "Keep shoulders relaxed.",
    "Avoid shrugging.",
    "Keep the movement small and controlled."
  ]
}
```

All 10 main exercises are encoded in this array with the structure above. Non-exercise sections (Daily Stretches, Heat/Cold) use a simpler structure:

```js
{
  id: "daily-stretches",
  type: "section",          // distinguishes from numbered exercises
  name: "Daily Stretches",
  content: "..."            // free-form HTML or structured sub-items
}
```

---

## Page Layout

### Header

- Title: "Shoulder Rehab"
- Subtitle: "4× per week"
- Top-right: "Edit" button

### Exercise List

A single vertically stacked list. Each row in **collapsed** state shows:

| Element | Detail |
|---|---|
| Number badge | Small rounded grey label (1, 2, … 10) |
| Thumbnail | 44×44 px rounded square; grey box placeholder if no image |
| Name | Bold, 15px |
| Volume | 11px grey (e.g. "10 reps · 2 sets") |
| Chevron | › rotates 90° when expanded |

Tapping a row toggles the **expanded** state inline (no page navigation). Expanded area shows:

1. Image area (full-width; single image now, multiple images/video carousel later — area reserved but not built)
2. **Setup** — bulleted list
3. **Movement** — bulleted list
4. **Important Cues** — bulleted list
5. **Volume** — text

Only one exercise can be expanded at a time (opening one closes the previous).

### Bottom Sections

After the 10 main exercises, two collapsible sections with the same accordion pattern but no number or thumbnail:

- **Daily Stretches** — Morning and Before Bed (4 stretch items)
- **Heat / Cold** — Morning / After Exercises

---

## Edit / Reorder Mode

Tapping "Edit" in the header:

1. Button label changes to "Done"
2. Each exercise row gains a drag handle icon (≡) on the left
3. SortableJS activates on the list container; touch drag reorders rows
4. Tapping "Done":
   - SortableJS disabled
   - Drag handles hidden
   - New order saved to `localStorage` as an array of exercise IDs
   - "Done" reverts to "Edit"

On page load: if a saved order exists in `localStorage`, exercises are rendered in that order; otherwise default order is used.

The bottom sections (Daily Stretches, Heat/Cold) are always pinned at the bottom and excluded from reordering.

---

## Future: Media Gallery

The expanded image area is a reserved `<div class="media-area">`. Initially empty (hidden when no image). Later this will support:

- Multiple images in a swipe carousel
- Video playback

The data model supports future `images: []` and `videos: []` arrays on each exercise object.

---

## Deployment

1. Initialize a git repo in the project root (if not already)
2. Push to GitHub
3. Enable GitHub Pages: Settings → Pages → Source: main branch, root `/`
4. Site live at `https://<username>.github.io/shoulder-physio/`

---

## Out of Scope

- Progress tracking / checkboxes
- Timer / rest timer
- Backend or user accounts
- Authentication
- Multiple routines
