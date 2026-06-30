# Dinner Website Update Workflow

**Status:** Ready for automation
**Last updated:** 2026-06-12

## How to Update the Website

1. **Add recipe to vault:**
   - Create markdown file in: `RH Brain/04 Dinners idea/[Recipe Name].md`
   - Add image to: `RH Brain/02 Projects/Dinner Website/images/[Image Name].png`
   - Format: 
     ```
     ![[Image.png]]
     
     Ingredients
     - item 1
     - item 2
     
     Instructions
     1. step 1
     2. step 2
     ```

2. **Ask Claude to sync:**
   - Message: "Update website" or "Sync dinner recipes"
   - Claude will:
     - Scan vault for new recipes
     - Infer cuisine from recipe name
     - Generate HTML recipe pages
     - Update index.html
     - Push to GitHub
     - Website updates automatically

3. **If cuisine is wrong:**
   - Ask: "Move [Recipe Name] to [Cuisine]"
   - Claude will fix the index.html card and re-push

## Recipe Naming

Use clear, descriptive names:
- ✅ "Brown Sugar Chicken.md"
- ✅ "Pad Krapow Gai.md"
- ❌ "recipe.md"

## Cuisine Inference

Claude infers cuisine from recipe name:
- **Asian:** Pad Thai, Kung Pao, Korean, Mongolian, Chinese, Sesame, Ginger, Soy, Teriyaki
- **Italian:** Pasta, Gnocchi, Risotto, Carbonara, Caprese, Bruschetta, Parmesan, Basil
- **Mexican:** Taco, Enchilada, Fajita, Quesadilla, Salsa, Jalapeño, Chile
- **Thai:** Pad Krapow, Thai Basil, Coconut, Fish Sauce
- **Cajun:** Cajun, Creole, Louisiana
- **American:** Burger, Steak, BBQ, Fried, Honey, Butter
- **Dessert:** Cake, Pie, Cookie, Cheesecake, Brownie, Chocolate, Candy

If inferred wrong, just ask Claude to move it!

---

**Ready to sync anytime!** 🚀
