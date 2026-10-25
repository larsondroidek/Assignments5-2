# Assignment 5.2 — Solution: CSS Selectors Practice

## Project Structure
```
css-selectors/
├── index.html
└── style.css
```

## index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Practice page for CSS selectors: element, class, ID, universal, grouping, cascade and specificity." />
    <title>CSS Selectors Practice</title>
    <link rel="stylesheet" href="style.css" />
  </head>

  <body>
    <header>
      <h1>CSS Selectors Practice</h1>
      <p>A page showing how different selectors style HTML elements.</p>
    </header>

    <main>
      <!-- Cards styled with a CLASS selector -->
      <section class="card">
        <h2>Element Selector</h2>
        <p>Headings and paragraphs on this page are styled by their tag names.</p>
      </section>

      <section class="card">
        <h2>Class Selector</h2>
        <p>These cards all share the class <code>card</code>, so one rule styles every card.</p>
      </section>

      <section class="card">
        <h2>Grouping Selector</h2>
        <p>The <code>h1, h2, h3</code> headings share one font family through a single grouped rule.</p>
      </section>

      <!-- Section styled with an ID selector -->
      <section id="featured" class="card">
        <h2>ID Selector</h2>
        <p>This section has the id <code>featured</code>, so it gets its own special style.</p>
      </section>

      <!-- Cascade and specificity demonstration -->
      <section class="card" id="conflict-demo">
        <h2>Cascade and Specificity</h2>
        <p class="highlight">This paragraph is targeted by several conflicting rules. Its color shows which rule wins.</p>
        <p class="highlight" id="special">This paragraph has a class and an id, so the ID rule wins.</p>
        <p>
          The first paragraph is green because the later <code>.highlight</code> rule
          overrides the earlier one (same specificity: the last rule wins), and
          <code>.highlight</code> beats the element selector <code>p</code>.
        </p>
      </section>
    </main>

    <footer>
      <p><small>&copy; 2026 CSS Practice</small></p>
    </footer>
  </body>
</html>
```

## style.css
```css
/* 1. UNIVERSAL selector: reset margin/padding and use border-box sizing */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* ELEMENT selector: styles every <body>, <p>, <code> */
body {
  font-family: Arial, sans-serif;
  background-color: #f3f4f6;
  color: #1f2937;
  line-height: 1.6;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

p {
  color: #374151;
  margin: 10px 0;
}

code {
  background-color: #e5e7eb;
  padding: 2px 6px;
  border-radius: 4px;
}

/* GROUPING selector: one rule for several selectors */
h1,
h2,
h3 {
  font-family: Georgia, serif;
  color: #1e3a8a;
}

/* ELEMENT selector for individual headings */
h1 {
  font-size: 2.2rem;
}

h2 {
  font-size: 1.4rem;
  margin-bottom: 8px;
}

header,
footer {
  text-align: center;
  padding: 16px 0;
}

/* 2. CLASS selector: styles all elements with class="card" */
.card {
  background-color: #ffffff;
  border: 1px solid #d1d5db;
  border-radius: 10px;
  padding: 16px 20px;
  margin: 16px 0;
}

/* 3. ID selector: styles only the element with id="featured" */
#featured {
  background-color: #fef3c7;
  border: 2px solid #f59e0b;
}

/* 4. CONFLICTING RULES — cascade and specificity */

/* Rule A: element selector (specificity 0,0,1) */
p {
  color: #374151; /* gray */
}

/* Rule B: class selector (specificity 0,1,0) — beats the element selector */
.highlight {
  color: red;
}

/* Rule C: same specificity as Rule B but written later — the LAST rule wins */
.highlight {
  color: green;
}

/* Rule D: ID selector (specificity 1,0,0) — beats all class/element rules, whatever the order */
#special {
  color: blue;
  font-weight: bold;
}
```

## Short Explanation of the Selectors Used
| Selector | Example | What it does |
|----------|---------|--------------|
| **Element** | `body`, `p`, `h1`, `h2`, `code` | Styles every element of that tag. |
| **Class** | `.card`, `.highlight` | Styles all elements with that class — reusable. Used for the 5 cards. |
| **ID** | `#featured`, `#special` | Styles the one unique element with that id. `#featured` gives one card a yellow style. |
| **Universal** | `*` | Matches every element. Used for a small reset (`margin: 0; padding: 0; box-sizing: border-box`) so all elements start from the same base. |
| **Grouping** | `h1, h2, h3` and `header, footer` | One rule shared by several selectors (separated by commas) to avoid repeating code. |

## Cascade and Specificity Experiment
The page contains these conflicting rules:

```css
p          { color: #374151; }  /* A: element   — specificity 0,0,1 */
.highlight { color: red;     }  /* B: class     — specificity 0,1,0 */
.highlight { color: green;   }  /* C: class     — specificity 0,1,0, written later */
#special   { color: blue;    }  /* D: ID        — specificity 1,0,0 */
```

| Element | Matching rules | Result | Why |
|---------|----------------|--------|-----|
| `<p class="highlight">` | A, B, C | **Green** | B and C beat A (class > element). B and C have the same specificity, so the **later one (C)** wins — this is the *cascade*. |
| `<p class="highlight" id="special">` | A, B, C, D | **Blue** | The ID rule D has the highest specificity, so it wins even though it is not the last rule — *specificity* beats order. |

**Rule of thumb:** inline style > ID > class/attribute/pseudo-class > element. If specificity is equal, the rule written last wins.

I ran the page in a browser and checked the computed colors: the first `.highlight` paragraph is `rgb(0, 128, 0)` (green) and the `#special` paragraph is `rgb(0, 0, 255)` (blue).

## How to Observe It Yourself
1. Open the page with Live Server → `F12` → **Elements**.
2. Click the "Cascade and Specificity" paragraphs. In the **Styles** pane the rules that lost are shown with a ~~strikethrough~~.
3. Swap the order of rules B and C in `style.css` — the paragraph turns red. Move rule D above the others — it stays blue.

## Submission
- ✅ `index.html`, `style.css` and this explanation (attached)
- 📸 Optional screenshot of the page and of DevTools showing the struck-through rules
