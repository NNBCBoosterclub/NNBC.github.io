# Barcode Scanning — Admin SOP

This document explains how to set up and use the barcode scanning feature in the NNBC Snack Bar app.

---

## How It Works

Customers can tap the **📷 Scan** button in the header to open a camera-based barcode scanner. When they point their phone's camera at a product's barcode, the app instantly looks up the matching menu item and adds one unit to their cart.

The feature supports all common retail barcode formats:
- **EAN-13 / EAN-8** — standard product barcodes worldwide
- **UPC-A / UPC-E** — standard North American retail barcodes
- **Code 128 / Code 39** — common warehouse/inventory barcodes
- **QR codes**

---

## Admin Setup: Assigning Barcodes to Products

For a product to be found when scanned, you must enter its barcode in the Admin panel.

### Steps

1. **Open the Admin panel** — navigate to `admin.html` and log in with your PIN.

2. **Find the product** in the *Current Menu* list and click **✏️ Edit** (or click **➕ New Item** to add a brand-new product).

3. **Locate the "Barcode" field** in the item form (it appears below the Emoji field).

4. **Enter the barcode number** exactly as it appears on the product packaging.
   - For EAN-13/UPC-A barcodes, type the full numeric string (e.g., `012345678905`).
   - Do **not** include spaces, dashes, or extra characters.
   - The barcode is case-insensitive, but numeric barcodes should be entered as digits only.

5. Click **Save Changes** (or **Add Item** for new products).

Repeat for every product you want to be scannable.

---

## How to Find a Product's Barcode

The barcode number is printed beneath the barcode stripes on product packaging. It is typically 8, 12, or 13 digits long.

You can also look it up online:
- Search `[product name] UPC barcode` on Google or [barcodelookup.com](https://www.barcodelookup.com).

---

## Using the Scanner (Customer Flow)

1. Open the NNBC Snack Bar page on a phone (or tablet with a camera).
2. Tap the **📷 Scan** button in the header.
3. Allow camera access when the browser prompts for permission.
4. Point the camera at the barcode on the product packaging.
5. The app beeps / shows a toast notification confirming the item was added to the cart.
6. Tap **Close Scanner** (or the ✕) to return to the menu, then proceed to checkout.

---

## Troubleshooting

| Symptom | Resolution |
|---|---|
| "Camera permission denied" | The customer must allow camera access in their browser settings. On iOS/Safari go to **Settings → Safari → Camera** and allow. On Android go to **Settings → Apps → Browser → Permissions → Camera** and allow. |
| "No product found for: `<barcode>`" | The barcode is not assigned to any menu item. Go to Admin → Edit that product and enter the barcode value. |
| Camera does not start | Ensure the page is served over **HTTPS** — camera access is blocked on plain HTTP in modern browsers. GitHub Pages and most web hosts serve HTTPS by default. |
| Scanner works but is slow | Ensure the barcode is well-lit and held steady. Standard rear cameras work best; front cameras may be lower quality. |
| Product added is wrong | Verify the barcode entered in the Admin panel matches the barcode on the physical product exactly (no leading/trailing spaces). |

---

## Notes

- **No backend required.** Barcodes are stored in the same browser `localStorage` as all other product data. Make sure you set up barcodes on the device that serves as the point-of-sale (or export/import product data if needed).
- **One barcode per product.** Each product supports one barcode value. If a product comes in multiple sizes with different barcodes, create a separate menu item for each size.
- **Scanning adds one unit at a time.** Customers can scan the same barcode multiple times to add more units, or adjust the quantity in the cart drawer.
