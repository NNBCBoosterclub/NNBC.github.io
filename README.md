# NNBC Snack Bar

A static web app for the NNBC Snack Bar with two pages:
- **Customer page** (`index.html`) for browsing, ordering, and payment
- **Admin page** (`admin.html`) for managing menu, stock, and order workflows

🌐 **Live site:** [https://nnbcboosterclub.github.io/NNBC/](https://nnbcboosterclub.github.io/NNBC/)

![QR Code](qr-code.png)

## 🌐 Live Site & Hosting on GitHub

The site deploys automatically to **GitHub Pages** whenever changes are pushed to the `main` branch.

---

## 🛍️ Customer Page (`index.html`)

### Menu browsing
- Category tabs plus subcategory tabs (for Drinks and Snacks)
- Search across item name, category, and subcategory
- Product cards with emoji or uploaded image, price, and quantity controls
- Stock awareness:
  - Out-of-stock items are disabled
  - Low-stock badge when 5 or fewer tracked units remain

### Nutrition & allergen visibility
- Optional inline nutrition highlights on cards (calories/protein)
- “Nutrition Facts / Allergen Info” modal per item
- Allergen badges on products with warnings
- Cart and checkout summaries include nutrition totals and allergen warnings

### Cart & checkout
- Slide-out cart with quantity adjust (+/−), line totals, and order total
- Checkout modal with full itemized summary
- Payment options:
  - **Cash**: creates a cash receipt modal for counter payment
  - **Venmo**: opens Venmo deep link with amount + generated order note
- Tracked stock is decremented when payment is confirmed
- Orders are logged locally with order IDs, payment method, status, and timestamps

### Account & profile
- Sign in, create account, or continue as guest
- **Email-based account creation** (email is the username)
- Passwords are hashed in-browser (SHA-256 + salt)
- Profile features:
  - Upload/remove profile photo
  - Save/update email
  - Set favorite item
  - View most purchased item
  - View weekly nutrition totals
  - View recent purchase history
  - Log out

### Sharing & in-person ordering tools
- Share button uses native Web Share API when available
- Fallback share modal includes:
  - Copy link
  - QR code generation
  - Printable QR code page
- Barcode scanner (camera) can scan product barcodes and add matched items to cart

### Store status messaging
- Shows admin-controlled status banner (ordered/restocked/normal) on the storefront

---

## 🔐 Admin Page (`admin.html`)

### Admin access
- First-time setup to create an admin PIN (minimum 4 chars)
- PIN login/logout flow
- Change PIN in Settings
- PIN is hashed with SHA-256 before storage

### Menu management
- Add, edit, and delete items
- Configure per-item fields:
  - Name, price, category, optional subcategory
  - Emoji
  - Barcode
  - Starting stock (tracked quantity or unlimited)
  - Product photo upload (resized/compressed in browser)
  - Optional nutrition facts
  - Optional allergy/ingredient warning
- Duplicate-name guard when adding new items

### Stock management
- Stock pills show unlimited / in stock / low / out-of-stock states
- Inline per-item restock action
- Bulk restock workflow from receipt form

### Receipt upload & bulk restock
- Upload optional receipt image
- Add optional notes
- Enter quantities received per menu item and apply in one action
- Saves receipt history records globally in the repository (with timestamp, notes, and restocked items)
- Delete receipt history entries

### Store status controls
- Set customer-facing status to:
  - Items ordered
  - Store restocked
  - Clear status (normal)

### Order history & analytics
- Order dashboard stats:
  - Total orders
  - Collected revenue
  - Pending cash totals
- Filters: all / cash / Venmo / pending
- Mark pending cash orders as paid
- Delete individual order records
- Clear all order history

### Admin navigation helpers
- Hero buttons to jump to Analytics and Stock sections
- Quick action button to open “New Item” modal

### Safety/reset
- Danger Zone reset restores menu to built-in defaults

---

## 📷 QR Code

A scannable QR code linking to the store is included at the top of this README and can be printed to post at the snack bar.

In the app, click **Share** to generate and print a QR code page.

---

## ✏️ Updating Venmo Info

The Venmo account details are at the top of `index.html`:

```js
const VENMO_USERNAME = "NNBoosterClub";              // Venmo @handle (no @)
const VENMO_DISPLAY  = "Northern Neck Booster Club"; // Display name shown to users
```

Edit and push to `main` — the site redeploys automatically.

---

## ⚠️ Data & device scope

This project is a static site with no backend.

- Admin-managed menu data, restock status, and receipt history are stored in GitHub-backed JSON files in this repository
- Admin PIN, GitHub sync token, customer accounts, sessions, and order history still use browser `localStorage`
- Customer checkout stock decrements remain browser-local unless they are later published through the admin workflow
