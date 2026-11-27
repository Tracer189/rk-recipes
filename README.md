# R&K Recipes

Source for **[rkdinners.com](https://rkdinners.com)**, served by GitHub Pages from the `master` branch.

## What's here

| File | What it is |
|---|---|
| `index.html` | The whole website: design, recipe cards, recipe data and code. This is the only page. |
| `images/` | Recipe photos |
| `CNAME` | Custom domain (rkdinners.com) |
| `UPDATE_WEBSITE.md` | Step-by-step guide for adding recipes and publishing |
| `.github/workflows/deploy.yml` | Check that runs on every push and confirms `index.html` has its recipe cards and data |

## Design (since 2026-11-27)

- **Homepage:** featured dinner hero (random each visit), oval category buttons, and a "Trending Tonight" carousel of the 10 newest recipes.
- **Category pages:** search bar, category buttons and a grid of recipe cards. Click any recipe for ingredients and instructions.
- All styling is inside `index.html`. There is no separate stylesheet.

The previous design (cuisine dropdown, Dinner Roulette, pagination, `style.css`) is in git history at commit `2950e5b`.

## Publishing

Push to **both** branches, because Pages serves `master`:

```
git push origin master master:main
```

See `UPDATE_WEBSITE.md` for the full recipe workflow.
