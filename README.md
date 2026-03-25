# SAWA Biochar Quote Tool — README
### File: `sawa-quote.html`

---

## What It Does

This is a standalone webpage that gives customers an **instant price range estimate** for biochar orders. Instead of filling out a Google Form and waiting for a reply, customers get a quote on the spot based on their quantity, intended use, and delivery location.

The customer goes through 3 steps:
1. **Their details** — name, phone, email, buyer type, institution, delivery location
2. **Order details** — quantity (slider + presets), intended use
3. **Price estimate** — an instant low–high price range, broken down line by line, with a product recommendation

There is an **"Email Me This Estimate"** button at the end that opens the customer's email app pre-filled with their full quote breakdown.

---

## How to Use It

Just open `sawa-quote.html` in any web browser — no internet connection, no server, no setup required. It works as a completely standalone file.

To put it on your website, upload the file to your hosting provider and link to it from your "Buy Biochar" button, replacing the current Google Form link.

---

## How to Change the Prices

Open `sawa-quote.html` in any text editor (Notepad on Windows, TextEdit on Mac, or a code editor like VS Code). Near the top of the file, you will find a clearly labelled section that starts with:

```
/* ============================================================
   PRICING CONFIGURATION — EDIT THIS SECTION TO UPDATE PRICES
   ============================================================ */

const PRICING = {
```

Everything inside this block is what you need to edit. Here is what each part means:

---

### 1. Quantity Tiers — Price Per Kg

```js
quantityTiers: [
  { min: 1,    max: 49,   pricePerKgLow: 8000,  pricePerKgHigh: 13000 },
  { min: 50,   max: 199,  pricePerKgLow: 6500,  pricePerKgHigh: 10000 },
  { min: 200,  max: 499,  pricePerKgLow: 5000,  pricePerKgHigh: 8000  },
  { min: 500,  max: 9999, pricePerKgLow: 3500,  pricePerKgHigh: 6500  },
],
```

Each line is a quantity bracket. Change the `pricePerKgLow` and `pricePerKgHigh` numbers to update what customers see.

**Example:** To change the price for small orders (1–49 kg) to IDR 9,000–14,000/kg:
```js
{ min: 1, max: 49, pricePerKgLow: 9000, pricePerKgHigh: 14000 },
```

You can also add or remove tiers. Just make sure the `min` and `max` values don't overlap.

---

### 2. Use Case Multipliers

```js
useCaseMultipliers: {
  "Rice Plantation": 1.0,
  "Compost":         1.0,
  "Palm Oil":        1.0,
  "Horticulture":    1.0,
  "Livestock Feed":  1.1,   // +10% premium
  "Construction":    0.85,  // -15% discount
  "Other":           1.0,
},
```

These adjust the price up or down based on what the biochar is being used for. `1.0` means no change. `1.1` means 10% more expensive. `0.85` means 15% cheaper.

**Example:** To add a 20% premium for Livestock Feed:
```js
"Livestock Feed": 1.2,
```

---

### 3. Delivery Costs

```js
delivery: {
  javaLow:   50000,
  javaHigh:  150000,
  outerLow:  200000,
  outerHigh: 600000,
},
```

The tool automatically detects whether the customer's address is on Java island or an outer island, and shows the right delivery range. Update these four numbers to change delivery estimates.

The tool recognises Java addresses by looking for keywords like "West Java", "Bandung", "Jakarta", "Surabaya", etc. in the address field. If a customer types an outer island location like "Bali" or "Kalimantan", it will use the outer delivery range.

---

### 4. Product Grade Labels

```js
productGrades: {
  "Rice Plantation": "Agricultural Grade – Rice & Soil Amendment",
  "Compost":         "Agricultural Grade – Compost Blend",
  ...
},
```

This is the product name shown on the quote. Change the text in quotes on the right to update the label for each use case.

---

### 5. Recommendations

```js
recommendations: {
  "Rice Plantation": "Apply at 1–2 t/ha mixed with compost before planting...",
  ...
},
```

This is the short agronomic tip shown at the bottom of each quote. Update the text on the right to change what customers read for each use case.

---

### 6. Quote Validity

```js
validDays: 10,
```

Change this number to set how many business days the estimate is valid for.

---

## How to Add or Remove a Use Case

To **add** a new use case (e.g. "Agroforestry"):

1. Add a tile in the HTML form section (find the other tiles and copy the pattern)
2. Add a multiplier: `"Agroforestry": 1.0,`
3. Add a product grade: `"Agroforestry": "Agricultural Grade – Agroforestry",`
4. Add a recommendation: `"Agroforestry": "Your tip here.",`

To **remove** a use case, delete the corresponding tile from the HTML and its entries from the three lists above.

---

## Contact Details

The contact details at the bottom of the quote (phone, email, Instagram, website) are hardcoded in the HTML. To update them, search the file for `+62 877-8964-7000` and update all instances, and similarly for `info@sawa.green`.

---

## Summary of What to Edit for a Price Update

| What you want to change | Where to find it |
|---|---|
| Price per kg | `quantityTiers` array |
| Premium/discount by use case | `useCaseMultipliers` |
| Delivery cost range | `delivery` object |
| Product grade name | `productGrades` |
| Agronomic recommendation text | `recommendations` |
| Quote validity period | `validDays` |
| Contact details | Search file for phone/email |
