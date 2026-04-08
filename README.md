# NNBC Snack Bar

A simple web interface for the NNBC Snack Bar. Users browse the menu, add items to their cart, and check out via Venmo — with the correct Venmo account pre-filled automatically.

## 🌐 Live Site & Hosting on GitHub

The site is deployed automatically to **GitHub Pages** whenever changes are pushed to the `main` branch. The workflow (`.github/workflows/deploy.yml`) handles everything automatically.

**Live URL:** **https://nnbcboosterclub.github.io/NNBC.github.io/**

---

## 📷 QR Code

The site has a built-in **QR Code** button in the top-right corner. Click it to:
- See a scannable QR code that links directly to the site.
- Print a clean QR code page to post at the snack bar so customers can scan and shop from their phones.

The QR code is generated from the live URL, so it always stays up to date.

---

## ✏️ Updating the Menu or Venmo Info (Admin Panel)

The site now includes a built-in **owner admin panel** at:

```
https://nnbcboosterclub.github.io/NNBC.github.io/admin.html
```

### What you can do in the admin panel

| Feature | How |
|---|---|
| **Add items** | Fill in name, price, category, emoji, and optional photo → click *Add Item* |
| **Edit items** | Click ✏️ Edit on any row → update fields → click *Save Changes* |
| **Delete items** | Click 🗑 Delete on any row and confirm |
| **Upload a photo** | Use the *Product Photo* field when adding or editing; images are resized automatically |
| **Mark out of stock** | Toggle the *In Stock* switch on any item row |
| **Reset to defaults** | Scroll to *Danger Zone* → *Reset to Defaults* (clears all custom changes) |
| **Change admin PIN** | Scroll to *Settings* → enter and confirm a new PIN |

### First-time login

The first time you open `admin.html` you'll be asked to **create a PIN** (at least 4 characters). After that, you'll need the PIN every time you open the admin panel. If you forget the PIN, clear the browser's localStorage for the site.

> **Changes are instant** — customers see the updated menu as soon as they reload the store page.

---

## ✏️ Updating Venmo Info (still in code)

The Venmo account details live at the top of `index.html` inside the `<script>` block:

```js
const VENMO_USERNAME = "northernneckboosterclub@gmail.com";  // Venmo @handle (no @)
const VENMO_DISPLAY  = "Northern Neck Booster Club"; // Display name shown to users
```

Edit those values and push to `main` — the site redeploys automatically.