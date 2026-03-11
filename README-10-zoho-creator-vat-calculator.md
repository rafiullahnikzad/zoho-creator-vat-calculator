# zoho-creator-vat-calculator

![Zoho Creator](https://img.shields.io/badge/Zoho-Creator-blue) ![Deluge](https://img.shields.io/badge/Language-Deluge-orange) ![Topic](https://img.shields.io/badge/Topic-Tax%20Compliance-green)

## Business Problem

Accountants recalculate invoice totals manually in Excel after entering them in Creator. Tax rate errors on subform items cause VAT compliance issues and rework.

## Solution

On every user input change, loop through all subform rows, calculate per-line VAT by category, and update all total header fields in real time.

## Use Case

ZATCA-compliant Saudi invoices, UAE VAT, EU multi-rate VAT — any business running invoice forms in Zoho Creator needing automatic tax calculation.

## Trigger

**On User Input — Invoice Form**

## Fields Required

| Field Link Name | Type | Notes |
|---|---|---|
| Invoice_Line_Items | Subform | quantity, unit_price, vat_category |
| subtotal | Decimal | Auto-calculated |
| vat_total | Decimal | Auto-calculated |
| grand_total | Decimal | Auto-calculated |

## Setup

1. Add script to Invoice form On User Input workflow.
2. Ensure subform has: quantity, unit_price, vat_category, line_total, line_vat, line_grand_total.
3. Set vat_category dropdown: Standard / Reduced / Zero.
4. Adjust vatRate values to match your country tax rules.

## Deluge Script

```javascript
// ============================================================
// SCRIPT: Real-Time VAT Calculator on Invoice Subform
// TRIGGER: On User Input - Invoice Form
// AUTHOR: Rafiullah Nikzad | rafiullahnikzad.netlify.app
// ============================================================

subTotal = 0.0;
totalVAT = 0.0;
lineItems = input.Invoice_Line_Items;

for each item in lineItems
{
    qty = item.quantity;
    unitPrice = item.unit_price;
    vatCategory = item.vat_category;

    if(vatCategory == "Standard")
    {
        vatRate = 0.15;
    }
    else if(vatCategory == "Reduced")
    {
        vatRate = 0.05;
    }
    else
    {
        vatRate = 0.0;
    }

    lineTotal = qty * unitPrice;
    lineVAT = lineTotal * vatRate;

    item.line_total = lineTotal;
    item.line_vat = lineVAT;
    item.line_grand_total = lineTotal + lineVAT;

    subTotal = subTotal + lineTotal;
    totalVAT = totalVAT + lineVAT;
}

input.subtotal = subTotal.round(2);
input.vat_total = totalVAT.round(2);
input.grand_total = (subTotal + totalVAT).round(2);
```

## Notes

- Adjust rates: Saudi 15%, UAE 5%, Kenya 16%, EU standard 20%.
- Add more vat_category conditions for complex multi-rate scenarios.
- For ZATCA compliance also populate vat_registration_number on the invoice.

---
**Author:** Rafiullah Nikzad — Senior Zoho Developer
**Portfolio:** rafiullahnikzad.netlify.app | **Community:** Zoho Afghanistan on LinkedIn