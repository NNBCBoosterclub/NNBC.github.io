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

## 🔐 Admin Login & Item Management

Shop owners and maintainers can log in to manage the menu directly from the browser — no code changes needed.

### How to log in

1. Click the **🔐 Login** button in the top-right corner of the site.
2. Enter the admin password (default: **`NNBC2024`**).
3. The button changes to **⚙️ Admin** and the Admin Panel slides open.

### What admins can do

| Action | How |
|---|---|
| **Add a new item** | Fill in the "Add New Item" form and click **💾 Save Item** |
| **Upload a photo** | Click the photo area in the form to pick an image file (max 5 MB) |
| **Edit an item** | Click **✏️ Edit** next to any item in the "Current Items" list |
| **Delete an item** | Click **🗑️** next to any item |
| **Reset to defaults** | Click **↺ Reset to Defaults** in the Admin Panel header |
| **Log out** | Click **🔓 Logout** |

> **Tip:** Item changes are saved in the browser's local storage, so they persist across page refreshes on the same device. Use "Reset to Defaults" to go back to the built-in menu.

### Changing the admin password

Open `index.html` and find this line near the top of the `<script>` block:

```js
const ADMIN_PASSWORD = "NNBC2024";
```

Replace `"NNBC2024"` with your own password and push to `main`.

> **Important:** Change the default password before going live. Because this is a static site, the password is visible in the page source on GitHub — it provides a convenient access barrier for the Admin UI, not cryptographic security.

---

## 📷 QR Code

The site has a built-in **QR Code** button in the top-right corner. Click it to:
- See a scannable QR code that links directly to the site.
- Print a clean QR code page to post at the snack bar so customers can scan and shop from their phones.

The QR code is generated from the live URL, so it always stays up to date.

---

## ✏️ Updating Venmo Info (code)

The Venmo account details live at the top of `index.html` inside the `<script>` block:

```js
const VENMO_USERNAME = "NNBC-Snackbar";  // Venmo @handle (no @)
const VENMO_DISPLAY  = "NNBC Snack Bar"; // Display name shown to users
```

Edit those values and push to `main` — the site redeploys automatically.