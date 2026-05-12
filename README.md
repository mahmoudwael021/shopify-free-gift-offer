# shopify-free-gift-offer
Timed free gift offer snippet for Shopify themes
# Shopify Free Gift Offer — Timed Snippet

A lightweight Shopify theme snippet that shows a timed free gift offer to customers who reach a minimum cart value. No apps, no Shopify Plus required.

---

## What It Does

- Shows a floating banner when the cart total reaches the minimum value
- Starts a 3-hour countdown timer per customer from the moment they qualify
- Lets the customer pick their free gift size and color from a popup
- Auto-adds the gift to cart and auto-removes it if the total drops below the threshold
- Locks the gift quantity at 1 — no way to increase it
- Shows a progress bar encouraging customers to reach the minimum value
- Works on any Shopify plan

---

## Demo

Live example: [trimodeshop.com](https://trimodeshop.com)

---

## Installation

### Step 1 — Create the Free Gift Product

1. Go to **Shopify Admin → Products → Add product**
2. Set the product **Title** (e.g. `Free Socks`)
3. Set the **Price** to `0.00`
4. Set **Status** to `Active`
5. Add your variants (sizes and colors)
6. Set **Inventory** to `Don't track` or add sufficient stock

> The product must be **Active** for the cart API to add it. It won't appear on your storefront as long as it's not added to any collection or navigation menu.

To hide it from search engines and browse pages:
- Go to the product page
- Under **Search engine listing**, click **Edit** and enable **Hide from search engines**

---

### Step 2 — Get Your Variant IDs

Open this URL in your browser:

```
https://your-store.myshopify.com/products/your-product-handle.json
```

Look for the `variants` array and copy all the `id` values. You'll need them in the next step.

---

### Step 3 — Add the Snippet

1. Go to **Shopify Admin → Online Store → Themes**
2. Click **Actions → Edit code** on your active theme
3. Under **Snippets**, click **Add a new snippet**
4. Name it `free-socks-offer`
5. Paste the full contents of `free-socks-offer.liquid` from this repository
6. Update the `VARIANTS` object at the top of the JavaScript with your actual variant IDs:

```javascript
var VARIANTS = {
  'S/Red'  : 12345678901234,
  'S/White': 12345678901235,
  'S/Black': 12345678901236,
  'M/Red'  : 12345678901237,
  // ... and so on
};
```

Also update the config values if needed:

```javascript
var C = {
  MIN_VALUE   : 10,    // minimum cart value to trigger the offer
  OFFER_HOURS : 3,     // how many hours the offer lasts per customer
  CURRENCY    : 'KD',  // currency symbol shown in the banner
};
```

7. Click **Save**

---

### Step 4 — Render the Snippet in Your Theme

1. Under **Layout**, open `theme.liquid`
2. Find the closing `</body>` tag
3. Add the following line just before it:

```liquid
{% render 'free-socks-offer' %}
```

So it looks like this:

```liquid
  {% render 'free-socks-offer' %}
</body>
```

4. Click **Save**

---

## How It Works

| Situation | Result |
|-----------|--------|
| Cart total below minimum | Progress bar shows how much is needed |
| Cart total reaches minimum (first time) | 3-hour countdown starts, banner appears |
| Customer clicks "Claim Free Gift" | Size & color picker opens |
| Customer selects and confirms | Gift added to cart at price 0 |
| Cart total drops below minimum | Gift removed automatically |
| Timer expires | Banner hides, gift removed automatically |
| Customer removes gift manually | They can claim it again next time they qualify |

---

## Configuration

| Option | Default | Description |
|--------|---------|-------------|
| `MIN_VALUE` | `10` | Minimum cart total to trigger the offer |
| `OFFER_HOURS` | `3` | Hours the offer stays active per customer |
| `CURRENCY` | `KD` | Currency symbol displayed in the UI |

---

## Notes

- The timer is stored in `localStorage` — it's per browser/device
- The gift quantity lock works client-side; for server-side enforcement you need Shopify Scripts (Plus plan only)
- If your theme uses a custom cart drawer, you may need to adjust the `CART_SELECTOR` config value

---

## License

MIT — free to use and modify.
