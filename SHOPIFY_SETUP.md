# Wolfpack Billiards — Shopify Buy Button Activation Guide

The Pro Shop section is wired up but inactive. All eight product cards stay in
"Coming Soon" mode until you do the four steps below. You can launch
products one at a time as inventory comes in — partial activation is fine.

---

## What's already done

- 8 product cards are tagged with `data-shopify-id` placeholders
- A `.shop-buy-zone` container is ready inside each card
- The Shopify Buy Button SDK loader is included at the bottom of `index.html`
- Cart drawer + Add-to-Cart buttons are pre-styled to match the Wolfpack
  black/gold theme
- The cart drawer carries the give-back message: *"10% of every sale goes
  back to the pool community."*

---

## What you need to do

### Step 1 — Sign up for Shopify

You need a paid Shopify plan that supports the Buy Button channel. The cheapest
options:

- **Shopify Starter** ($5/month) — Buy Button + social selling, no full storefront
- **Shopify Basic** ($39/month) — Full Shopify store + Buy Button on any site

For Wolfpack's setup (Wix or GitHub Pages hosting the brochure site, Shopify
handling checkout), **Shopify Starter is enough**.

Sign up at https://www.shopify.com/starter

### Step 2 — Create your products in Shopify admin

For each of the 8 products on the site, create a Shopify product:

1. Wolfpack T-Shirt — $30 (variants: S / M / L / XL / 2XL)
2. Wolfpack Hat — $25
3. Pool Glove — $12 (variants: S / M / L)
4. Wolfpack Chalk — $4.50
5. Sticker Pack — $5
6. Cue Tip Tool — $9
7. Microfiber Towel — $12
8. Man Cave Sign — $22

For each product, write down the **numeric Product ID**. You can find it in the
admin URL when editing a product:
`https://admin.shopify.com/store/<your-store>/products/1234567890`
The ID is the long number at the end (in the example, `1234567890`).

### Step 3 — Get your Storefront API access token

1. In Shopify admin, go to **Settings → Apps and sales channels → Develop apps**
2. Click **Create an app**, name it "Wolfpack Buy Button"
3. Click **Configure Storefront API scopes** and enable:
   - `unauthenticated_read_product_listings`
   - `unauthenticated_read_product_inventory`
   - `unauthenticated_write_checkouts`
   - `unauthenticated_read_checkouts`
4. Click **Install app**
5. Under the **API credentials** tab, copy the **Storefront API access token**

### Step 4 — Wire the IDs and token into `index.html`

Open `index.html` and find the block near the bottom marked
`<!-- SHOPIFY BUY BUTTON INTEGRATION -->`.

Change three constants:

```js
const SHOP_LIVE      = true;                                 // flip to true
const SHOPIFY_DOMAIN = 'your-store.myshopify.com';           // your domain
const SHOPIFY_TOKEN  = 'shpat_xxxxxxxxxxxxxxxxxxxxxxxxxxxx'; // token from step 3
```

Then in the `.shop-grid` section above, replace each `REPLACE_ME_*` with the
numeric Product ID from Shopify admin:

```html
<div class="shop-card" data-shopify-id="REPLACE_ME_SHIRT">      ← becomes
<div class="shop-card" data-shopify-id="1234567890">
```

Save, push to GitHub Pages (or upload to Wix Custom Code if hosting on Wix),
done. Each card with a real ID gets a live "Add To Cart" button. Cards still
on `REPLACE_ME_*` stay as "Coming Soon."

---

## Wix vs. GitHub Pages — which to host on?

| Need | Wix | GitHub Pages + Shopify |
|---|---|---|
| Time to launch | Fastest (drag-and-drop) | Medium (DNS + repo setup) |
| Monthly cost | $17–$36 (Wix Core+) | $5 Shopify Starter only |
| Design control | Constrained by Wix builder | Full HTML control |
| Inventory mgmt | Wix Stores native | Shopify (better) |
| Performance | Slower (Wix overhead) | Very fast (static CDN) |
| Custom domain | Built-in | Free, manual DNS |

**Recommendation:** Stay on Wix for the September 4 Bad Boys launch — speed
matters more than savings right now. Revisit GitHub + Shopify after the
first tournament once you know what's actually selling.

---

## Quick deploy to GitHub Pages (when you're ready)

1. Create a new public repo: `wolfpack-billiards-site`
2. Drop the contents of this `wolfpack_site/` folder into the repo root
3. Commit and push
4. In repo **Settings → Pages**, set source to `main` branch, root folder
5. Add a file named `CNAME` containing only `wolfpackbilliards.com`
6. At your domain registrar (where wolfpackbilliards.com is registered),
   set DNS A records to GitHub's IPs:
   - 185.199.108.153
   - 185.199.109.153
   - 185.199.110.153
   - 185.199.111.153
7. Wait 10–60 minutes for DNS to propagate
8. Site is live with free HTTPS at https://wolfpackbilliards.com

---

## Files in this folder

- `index.html` — the site (copy-paste-ready for any host)
- `*.jpg / *.png` — all images referenced by the HTML
- `team_photos/` — championship photos
- `SHOPIFY_SETUP.md` — this file

Everything is self-contained. No build step, no dependencies to install.
