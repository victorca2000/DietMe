# DietMe

A practice tool for nutritionists and dietitians. It keeps patient records, holds a recipe library with USDA-based nutrition, and builds meal calendars whose portions are fitted to each patient's macro targets and dietary restrictions.

It runs entirely in the browser, in English or Spanish. There are no accounts and no server.

---

## Purpose

Nutritionists spend a lot of time on three repetitive jobs: tracking patients, calculating the nutrition of recipes, and rebuilding meal plans by hand whenever targets or restrictions change. DietMe does the arithmetic and the first draft of the plan, and leaves the clinical decisions to the nutritionist.

Design principles:

- **The nutritionist stays in charge.** Targets, protein, macro split, substitutions and every meal in the calendar can be edited. The app suggests and the dietitian decides.
- **Portions have to make sense.** The planner won't solve a carb target with an enormous bowl of rice. Each food group has sensible limits, eggs and slices are whole items, and amounts appear in kitchen measures (for example "2 pc · 100 g") rather than odd multipliers.
- **Restrictions are respected.** Allergens, diet style and free-text exclusions are checked against every recipe's ingredients.

---

## How it works for a user

### 1. Patients tab

Add a patient and enter weight, height, age, sex, activity level and visit history. The app calculates:

- **BMI**, using the standard formula.
- **Calorie, carbohydrate and fat targets**, as suggestions the dietitian can override.
- **Protein as a range in g/kg of body weight** (default 1.2–1.6, editable). The dietitian can also set an **exact protein target**, which must stay inside the range. That exact number is what meal plans are fitted to.
- **Macro distribution.** The carbohydrate and fat percentages can be changed when the suggested split isn't right, and the app flags values outside common guidelines.

Restrictions are set in three ways:

- **Allergens** (checkboxes).
- **Diet style:** vegetarian, lacto-ovo-vegetarian, ovo-vegetarian, lacto-vegetarian, vegan or pescatarian.
- **Free text:** type something like "no pork, avoid spicy food, low sodium, diabetic" and the app turns it into structured exclusions and medical flags.

Heights can be entered in feet and inches or in centimetres, and the units switch between lb/in and kg/cm.

### 2. Recipes & Meal Plans tab

**Recipe library**

- **Import:** upload a PDF or Word file and the app extracts the recipes, including large files and Spanish-language recipe books. Individual recipes can also be added by hand.
- **Automatic ingredient matching:** each ingredient is matched to a USDA food (Foundation Foods and SR Legacy). Spanish ingredients are translated and matched automatically. Anything it can't match can be assigned by hand, and custom foods can be added.
- **Nutrition per serving** is calculated from the matched ingredients.
- **Filters:** by meal (breakfast, lunch, dinner, snack) and by a chosen patient, so you can see which recipes fit them.

**Substitutions**

Recipes that conflict with a patient's restrictions can be adapted instead of being thrown out. Built-in rules include:

- cow's milk becomes almond milk
- yogurt in sauces becomes light mayo or vegan yogurt
- cheese is omitted or replaced with vegan cheese
- butter becomes olive oil
- eggs in pancakes become a scoop of protein powder
- scrambled eggs become tofu scramble
- whole wheat bread becomes gluten-free bread or a corn tortilla
- flour tortilla becomes gluten-free tortilla
- whole wheat pasta becomes gluten-free pasta

Rules apply only when an ingredient conflicts with the patient. The dietitian can turn rules off, edit them, or add their own.

**Meal calendar**

- Choose a patient and **auto-fill** days, or add individual meals.
- Portions are fitted per food group (protein, carbohydrate, fat, vegetables, other) to hit the patient's targets, within realistic limits.
- **Day-level balancing:** each meal is sized from what the day still needs, so adding a heavy breakfast first doesn't starve lunch and dinner, and no single meal can take most of the day's protein. The snack is planned first and the other meals absorb the rest.
- Portions can be nudged up or down in sensible steps, a meal can be re-fitted, and recipes can be swapped. The app warns when a recipe is protein-heavy for its slot even at its smallest portions.

### 3. Your data

- Everything is stored **in the browser of the person using it**, on that device. Nothing is sent to a server, and no one else can see it.
- Use **Backup** (header) to download everything as one file, and **Restore** to load it on another device or after clearing the browser.
- Clearing site data, using a private window, or switching browsers or computers starts you with an empty app unless you restore a backup. **Back up regularly.**

---

## Deploying

The app is a static site (`index.html` plus a `vendor/` folder).

1. Go to [app.netlify.com/drop](https://app.netlify.com/drop).
2. Drag the unzipped site folder onto the page.
3. Share the link. Every person who opens it gets their own empty, private copy.

It also works on any other static host (GitHub Pages, Cloudflare Pages, Vercel).

**Not available outside Claude:** the Claude-powered helpers (estimating a food that isn't in the USDA data, and AI interpretation of free-text restrictions). The app falls back to built-in matching and parsing.

---

## Known limitations

- Data lives on one device and browser, with no sync, as described above.
- Nutrition comes from the USDA datasets bundled with the app. They aren't refreshed automatically, and branded or regional products may need a custom food.
- Ingredient matching is automatic but not perfect. Roughly 98% of ingredients in a large Spanish recipe book matched, and the rest need a manual pick. Review newly imported recipes before using them with patients.
- Targets and meal plans are decision support, not medical advice. The nutritionist is responsible for the clinical review of every plan.
- The snack slot depends on how many snack recipes are in the library. With few of them, lunch and dinner end up carrying slightly more.

---

## Potential next steps

**Accounts and cloud storage** (highest value)
- Sign-in per nutritionist and cloud-saved data, so patients and recipes follow them across devices and can't be lost by clearing a browser. Needs a small backend, for example Supabase or Firebase. Look at privacy requirements (such as HIPAA in the US, or local rules) before storing real patient data on a server.
- The storage layer is already isolated in one small module, so it can be swapped without rewriting the screens.

**Patient-facing features**
- A shareable, printable or PDF meal plan for each patient, with a shopping list.
- Weight and measurement trends in charts, and progress notes per visit.
- A patient portal or a link where patients can log meals or check in.

**Planning improvements**
- Weekly view with variety rules (no repeats within N days), prep-day batching and leftovers.
- Micronutrient targets (fibre, sodium, iron, calcium) alongside macros, and per-patient favourite and avoided foods.
- Cost and pantry awareness.
- Per-meal protein caps that the dietitian can set.
- More restriction types: FODMAP, kidney-friendly, gluten-free, halal and kosher.

**Library improvements**
- A shared starter recipe library that new users can import in one click.
- Recipe photos, and import from a URL.
- Faster correction of unmatched ingredients, with the app learning from the dietitian's choices.
- More languages beyond English and Spanish.

**Product**
- Subscription billing, team or clinic accounts with shared libraries, and usage analytics (in aggregate and with consent).
- A mobile-friendly install (PWA) with offline use.
- Audit trail of changes to a patient's plan.

---

## Credits

Nutrition data: USDA FoodData Central (Foundation Foods and SR Legacy). Built with React, pdf.js and mammoth.js.