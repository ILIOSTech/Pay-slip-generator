# Payslip Generator

Creates monthly payslip PDFs directly from the **Payslip Register** Excel file.
Everything runs inside the browser — no salary data is ever uploaded to a server.

---

## Using it every month (for the payroll team)

1. **Set the month.** Type the company name/address once, then pick the month and year at the top.
2. **Upload the register.** Click the blue box and choose `Payslip Register 26-27.xlsx`.
   The employee list fills in automatically.
3. **Check the figures.** Anything inside a box can be typed over. Grey numbers
   (deductions, net pay, CTC) recalculate the moment you change something.
   - Untick **PF** or **ESIC** for anyone who should not be deducted.
   - **P.Tax** is filled in from the West Bengal slab; overtype it if needed.
4. **Download.**
   - **PDF** button on a row → that one person's payslip.
   - **Download All Selected as ZIP** → every ticked employee, one PDF each,
     inside `Payslips_JULY_2026.zip`.
   - **Download Calculated Register (Excel)** → the same figures back as a spreadsheet
     for your records.

Tip: **Save Data in Browser** keeps the list on that computer so next month you only
change the Pay Days and Gross columns.

---

## How each figure is calculated

| Item | Formula |
|---|---|
| Basic | Taken from the register; if blank, 50% of Gross |
| Special Allowance | Gross − Basic |
| PF Employee's Contribution | 12% of Basic, rounded to the nearest rupee |
| PF Employer's Contribution | 13% of Basic, rounded to the nearest rupee |
| ESIC Employee's Contribution | 0.75% of Gross, rounded **up** to the next rupee |
| ESIC Employer's Contribution | 3.25% of Gross, rounded **up** to the next rupee |
| P.Tax | West Bengal slab (see below), editable |
| Total Deductions | PF Employee + ESIC Employee + P.Tax |
| **Net Pay** | Gross − Total Deductions |
| **Monthly CTC** | Gross + PF Employer + ESIC Employer |

**West Bengal Professional Tax slab**

| Monthly gross | P.Tax |
|---|---|
| up to ₹10,000 | ₹0 |
| ₹10,001 – ₹15,000 | ₹110 |
| ₹15,001 – ₹25,000 | ₹130 |
| ₹25,001 – ₹40,000 | ₹150 |
| above ₹40,000 | ₹200 |

**Who gets PF / ESIC**

On upload, the tick boxes are set from the register itself — a `0` in the PF or ESIC
column means that person is exempt. If those columns are missing, PF is ticked when a
UAN number exists and ESIC is ticked when Gross is ₹21,000 or less. You can change any
tick box by hand.

**Rounding.** Every amount printed on a payslip is a whole rupee, and Net Pay always
equals the printed Gross minus the printed deductions. The ESIC "round up to the next
rupee" rule is the statutory one — it replaces the manual `+1` / `+2` adjustments that
were in the register formulas.

**Pay Days** prints as a plain number of days (`8`), not `8 / 26`.

---

## Deploying to Render

1. Put this folder in a GitHub repository (keep `index.html` and the `vendor/` folder together).
2. In Render: **New → Static Site**, connect the repository.
3. Settings:
   - **Build Command:** leave empty
   - **Publish Directory:** `.`
4. Deploy. Render gives you a URL like `https://payslip-generator.onrender.com`.

The included `render.yaml` sets this up automatically if you use **New → Blueprint** instead.

A static site on Render never sleeps and has no monthly server cost, unlike a web-service deployment.

---

## Running it without Render

Double-click `Payslip-Generator-Standalone.html` — it is one single file with everything
built in and works offline, including on a laptop with no internet.

The `index.html` + `vendor/` version needs both parts kept together, but is the one to
deploy to Render.

---

## Register columns it reads

`Employee Name`, `Designation`, `Employee ID`, `Date of Joining`, `Pay Days`,
`UAN Number`, `Basic`, `Gross`, `PF_Employee's Contribution`,
`ESIC_Employee's Contribution`, `P.Tax`

Column order does not matter, and extra columns are ignored. Rows with no Gross amount
are skipped.
