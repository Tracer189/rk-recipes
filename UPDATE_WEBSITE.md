# Dinner Website Update Workflow

**Status:** Active
**Last updated:** 2026-07-11
**Live site:** rkdinners.com (GitHub Pages → repo: rk-recipes-temp)

---

## Folder Structure

```
10 RK Dinners/
├── rk-recipes-temp/        ← Git repo (index.html lives here)
│   └── index.html          ← THE website file — all changes go here
├── Dinner Instructions/    ← Recipe markdown files (dinners)
├── Desserts Instructions/  ← Recipe markdown files (desserts)
└── Dinner Website/
    └── Dinner images/      ← Food photos (.png / .jpg)
```

---

## How to Add a New Recipe

### Step 1 — Add the markdown file

Create a `.md` file in `Dinner Instructions\` (or `Desserts Instructions\` for desserts).

**File name = Recipe name** (must match the card `<h2>` in index.html exactly, or close enough for auto-matching).

Supported header formats (any of these work):
```
## Ingredients          ← preferred
Ingredients             ← also works
Ingredients:            ← also works
### Instructions        ← also works
Instructions:           ← also works
```

Example file (`Spicy Peach Chicken.md`):
```
![[Spicy Peach Chicken.jpeg]]

## Ingredients
- 2 chicken breasts
- 2 ripe peaches, sliced
- 1 jalapeño, sliced

## Instructions
1. Season and sear the chicken.
2. Add peaches and jalapeño to the pan.
3. Serve and enjoy.
```

### Step 2 — Add the image

Drop the photo in `Dinner Website\Dinner images\`.
File name should match what the card `<img src="images/...">` references in index.html.

### Step 3 — Add the card to index.html

Open `rk-recipes-temp\index.html` and add a card block inside `<div class="card-grid" id="recipe-grid">`:

```html
<div class="card" data-cuisine="american" onclick="showRecipeDetail(this)">
  <img src="images/Recipe%20Name.png" alt="Recipe Name" style="cursor: pointer;" onclick="showRecipeDetail(this.parentElement)" />
  <div class="card-body">
    <span class="card-stars"></span>
    <h2>Recipe Name</h2>
    <p class="card-desc">Short description.</p>
    <span class="cuisine-tag">American</span>
  </div>
</div>
```

Set `data-cuisine` to one of: `american`, `italian`, `asian`, `thai`, `cajun`, `mexican`, `dessert`

### Step 4 — Rebuild recipeData and push

Ask Claude: **"Update the website with [Recipe Name]"**

Claude will:
1. Rebuild the `recipeData` JavaScript block from all markdown files
2. Commit and push to GitHub
3. Site goes live in ~1 minute

---

## Features on the Site

| Feature | How it works |
|---------|-------------|
| **Pagination** | 15 recipes per page, Prev/Next buttons at bottom |
| **Cuisine filter** | Dropdown filters cards by `data-cuisine` tag |
| **Star ratings** | 1–5 stars per recipe, saved to browser localStorage |
| **Recipe popup** | Click any card → modal shows ingredients + instructions |
| **Dinner Roulette** | Spins to a random recipe |

---

## Cuisine Tags

| Tag | Use for |
|-----|---------|
| `american` | Burgers, steak, BBQ, honey butter, cheese fries |
| `italian` | Pasta, gnocchi, risotto, pizza, carbonara |
| `asian` | Korean, Mongolian, Chinese, sesame, soy-based |
| `thai` | Pad Thai, Pad Krapow, coconut curry |
| `cajun` | Cajun, creole, Louisiana spice |
| `mexican` | Tacos, enchiladas, fajitas |
| `dessert` | Cake, pie, cookies, cheesecake, brownies |

---

## Filename Matching Rules

When Claude rebuilds `recipeData`, it matches card `<h2>` names to markdown filenames using:
1. **Case-insensitive exact match** — `"cajun chicken pasta"` matches `Cajun Chicken Pasta.md`
2. **Normalization** — strips non-alphanumeric chars, collapses spaces, removes trailing "Recipe"
3. **Manual overrides** for known typos in older filenames (e.g., `Load Cheese Fries.md` → "Loaded Cheese Fries")

If a recipe shows "Coming soon" in the popup, the filename probably doesn't match the card name closely enough — ask Claude to add an override.

---

## Pushing to GitHub

The repo has two branches. **GitHub Pages serves from `master`** — always push to both.

```
cd "C:\Obsidian\2nd Brain\RH Brain\10 RK Dinners\rk-recipes-temp"
git add index.html
git commit -m "Add [Recipe Name]"
git push origin main master
```

Or just ask Claude: **"Push to GitHub"**

> **Why two branches?** `master` is what the live site reads. `main` is the working branch. They must stay in sync or updates won't appear on rkdinners.com.
