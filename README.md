# NNBC Snack Bar

A simple web interface for the NNBC Snack Bar. Users browse the menu, add items to their cart, and check out via Venmo — with the correct Venmo account pre-filled automatically.

## 🌐 Live Site

Once deployed, the site is available at:
**https://jaugustineflory.github.io/NNBC-Shop/**

## 🚀 Deploying (GitHub Pages)

This site is automatically deployed to GitHub Pages whenever changes are pushed to the `main` branch via the included GitHub Actions workflow (`.github/workflows/deploy.yml`).

**One-time setup** (do this once in the repository settings):
1. Go to **Settings → Pages** in the GitHub repository.
2. Under **Source**, select **GitHub Actions**.
3. Save — the next push to `main` will trigger a live deployment.

## ✏️ Updating the Menu or Venmo Info

All menu items and the Venmo account details live at the top of `index.html` inside the `<script>` block:

```js
const VENMO_USERNAME = "NNBC-Snackbar";  // Venmo @handle (no @)
const VENMO_DISPLAY  = "NNBC Snack Bar"; // Display name shown to users

const PRODUCTS = [
  { id: 1, name: "Chips", emoji: "🥔", price: 1.00, category: "Snacks" },
  // ...
];
```

Edit those values and push to `main` — the site redeploys automatically.