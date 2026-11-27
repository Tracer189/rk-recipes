# Dinner Website Update Workflow

**Status:** Active
**Last updated:** 2026-11-27 (new site design went live: editorial homepage + category pages)
**Live site:** rkdinners.com (GitHub Pages → repo: rk-recipes-temp)

---

## Folder Structure

```
10 RK Dinners/
├── Dinners/
│   ├── Instructions/       ← Dinner recipe markdown files
│   └── Pictures/           ← Dinner photos (.png / .jpeg)
├── Desserts/
│   ├── Instructions/       ← Dessert recipe markdown files
│   └── Pictures/           ← Dessert photos
├── Drinks/
│   ├── Instructions/       ← Drink recipe markdown files
│   └── Pictures/           ← Drink photos
├── Upcoming Recipes/       ← Drafts not on the site yet
├── Dinner Website/         ← Dinner Index + this workflow doc
└── rk-recipes-temp/        ← Git repo (the live website)
    ├── index.html          ← THE website file: design, recipes and code all live here
    └── images/             ← Copies of the photos the site uses
```

> Since 2026-11-27 there is no `style.css`. The styling is inside `index.html`.
> The old design (dropdown, Roulette, pagination) is still in git history (commit `2950e5b`) if it's ever needed.

---

## How the Site Is Laid Out

```
Homepage (rkdinners.com)
├── Header ............ R&K Recipes logo · Home · All Recipes · search icon
├── Featured Dinner ... big dark hero, a RANDOM dinner every visit (never a dessert or drink)
├── Category buttons .. oval buttons, one per category, with recipe counts
└── Trending Tonight .. carousel of the 10 NEWEST recipes (the last 10 cards in the list)

Category page (rkdinners.com/#/italian, #/dessert, #/all, ...)
├── "What's for dinner tonight?" search (searches as you type)
├── Category buttons (the current one is filled black)
└── Grid of recipe cards with photo, category label, description and stars
```

Clicking any recipe opens the popup with ingredients and instructions.

**Button style:** white fill, black outline, black text everywhere.
```

---

## How to Add a New Recipe

### Step 1 — Add the markdown file

Create a `.md` file in the matching category's `Instructions` folder: `Dinners\Instructions\`, `Desserts\Instructions\` or `Drinks\Instructions\`.

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

Drop the photo in the same category's `Pictures` folder (for example `Desserts\Pictures\`). Claude copies it into `rk-recipes-temp\images\` when publishing.
File name should match what the card `<img src="images/...">` references in index.html.

### Step 3 — Add the card to index.html

Open `rk-recipes-temp\index.html` and add a card block at the **end** of `<div class="card-grid" id="recipe-grid">`. That div sits inside the hidden `<div id="recipe-source" hidden>` near the bottom of the page. The page reads these hidden cards and builds the homepage, the category pages and the search from them.

```html
<div class="card" data-cuisine="american">
  <img src="images/Recipe%20Name.png" alt="Recipe Name" />
  <div class="card-body">
    <span class="cuisine-tag">American</span>
    <h2>Recipe Name</h2>
    <p>Short description.</p>
    <div class="card-stars"></div>
  </div>
</div>
```

Set `data-cuisine` to one of: `american`, `italian`, `asian`, `thai`, `cajun`, `mexican`, `dessert`, `drinks`

> **Put new cards LAST.** "Trending Tonight" shows the last 10 cards in the list, so a new recipe lands there automatically.
> Older cards still have leftover `onclick="..."` attributes from the old design. They're harmless and can stay.

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
| **Featured Dinner** | Random dinner on every visit (desserts and drinks are skipped) |
| **Category buttons** | Built automatically from the `data-cuisine` values on the cards, with counts |
| **Category pages** | `#/italian`, `#/dessert`, etc. `#/all` shows everything |
| **Search** | Searches recipe names, descriptions and categories as you type |
| **Trending Tonight** | The 10 newest cards (end of the list), with Prev/Next |
| **Star ratings** | 1–5 stars per recipe, saved in the visitor's browser (same saved ratings as the old design) |
| **Recipe popup** | Click any card → ingredients + instructions. Close with ✕, Esc, or by clicking outside |

### Adding a brand-new category
Add a line to the `CATS` list near the top of the last `<script>` in `index.html`, for example `{ id: "greek", name: "Greek", color: "#..." }`, and a label color to `BADGE`. Then use `data-cuisine="greek"` on the cards. Categories with no recipes are hidden automatically.

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
| `drinks` | Eggnog, hot chocolate, cocktails |

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
git push origin master master:main
```

Or just ask Claude: **"Push to GitHub"**

> **Why two branches?** `master` is what the live site reads. `main` is the working branch. They must stay in sync or updates won't appear on rkdinners.com.
