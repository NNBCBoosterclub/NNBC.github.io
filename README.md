# NNBC Snack Bar

A simple web interface for the NNBC Snack Bar. Users browse the menu, add items to their cart, and check out via Venmo — with the correct Venmo account pre-filled automatically.

## 🌐 Live Site & Hosting on GitHub

The site is deployed automatically to **GitHub Pages** whenever changes are pushed to the `main` branch.

### Getting a clean `NNBCBoosterclub.github.io` URL

By default the site lives at a URL like `jaugustineflory.github.io/NNBC-Shop/`. To get the clean **`NNBCBoosterclub.github.io`** address (no sub-path), follow these steps:

1. **Create a free GitHub organization** named exactly `NNBCBoosterclub`:
   - Go to [github.com/organizations/new](https://github.com/organizations/new) and choose the free plan.
   - Set the organization name to `NNBCBoosterclub`.

2. **Transfer this repository** to that organization:
   - Go to this repo → **Settings** → scroll to the bottom → **Transfer ownership** → enter `NNBCBoosterclub`.

3. **Rename the repository** to `NNBCBoosterclub.github.io`:
   - Inside the transferred repo → **Settings** → **General** → change the name to `NNBCBoosterclub.github.io`.

4. **Enable GitHub Pages** (one-time):
   - Go to **Settings → Pages** → under **Source** select **GitHub Actions** → Save.

5. Push any change to `main` (or re-run the workflow manually) and the site will be live at:
   **https://NNBCBoosterclub.github.io/**

> **Already have the right org & repo name?** You only need step 4 above — the workflow (`.github/workflows/deploy.yml`) handles everything else automatically.

---

## 📷 QR Code

The site has a built-in **QR Code** button in the top-right corner. Click it to:
- See a scannable QR code that links directly to the site.
- Print a clean QR code page to post at the snack bar so customers can scan and shop from their phones.

The QR code is generated from the live URL, so it always stays up to date.

---

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