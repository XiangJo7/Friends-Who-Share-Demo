# Navo Health — Clinical Evidence & Value Model Dashboard

A single-file interactive web tool for communicating Navo's clinical performance and quantifying the economic value it delivers to a hospital. Built for internal use and investor/hospital presentations.

---

## Why this exists

Selling a clinical AI product to a hospital requires two separate conversations: **does it work?** and **is it worth the cost?**

Most hospitals will only engage seriously once they can see a number — a credible, bottom-up estimate of what Navo would be worth to *their* specific ward. This dashboard was built to support exactly that conversation. It grounds the value claim in three things:

1. **Real evidence** — published literature, primary stakeholder research, and Navo's own held-out test set results
2. **Transparent assumptions** — every input is editable and every formula is shown, so a sceptical procurement team or CFO can interrogate it
3. **Hospital-specific outputs** — the model takes a hospital's own volume and cost figures so the output feels owned by them, not handed to them

The goal is not to produce a final price. It is to give a hospital enough signal to justify moving to a formal evaluation, and to give the Navo team a structured way to prepare for that conversation.

---

## The four tabs

### 1. Clinical Evidence
Presents the clinical case in three layers:

- **Stakeholder research** — qualitative failure modes surfaced from interviews with nurses, midwives, registrars, and O&G consultants across Singapore restructured hospitals
- **Published benchmarks** — comparison against the strongest published AI model (Google, 2025) at a consistent 90% specificity operating point
- **Navo test set results** — held-out performance metrics (AUC, sensitivity, specificity, precision, accuracy) from the five-branch neural network evaluated on 83 patients

This tab is intended to be read first. The numbers it establishes (sensitivity 0.471, specificity 0.69, AUC 0.821) are fixed model parameters that feed directly into the Value Model.

### 2. Value Model
A bottom-up financial model with five drivers across two categories:

| Driver | Category | Description |
|--------|----------|-------------|
| 1a — Nursing surveillance burden | Operational | Nurses spend 8–18 min/hr watching CTG streams. Navo performs first-level AI surveillance, freeing nursing capacity. |
| 1b — Alert fatigue & false escalations | Operational | 5–6 in 10 escalations are unnecessary. Navo's triage reduces the rate of pulling physicians for non-critical traces. |
| 1c — True negative clearance | Operational | Navo's specificity of 0.69 means it can objectively clear ambiguous-but-normal traces, avoiding unnecessary workup. This figure is fixed from the test set. |
| 1d — Interpretation disagreement cost | Operational | 4–6 in 10 correct escalations carry misaligned interpretations between frontliner and consultant. A shared AI risk score collapses re-review cycles. |
| 2 — Litigation & adverse outcome avoidance | Clinical safety | Missed fetal distress is implicated in ~50% of obstetric litigation. Navo's sensitivity advantage translates to a risk-adjusted litigation savings estimate. |

Each driver is an expandable accordion showing:
- The rationale and stakeholder evidence behind it
- Editable inputs with validated reference ranges
- The live formula so users can see exactly how the number is derived
- Sources for external validation

A **value capture slider** (10–50%) lets the user set what percentage of the total value Navo proposes to charge as a SaaS fee, generating a suggested annual price per site and a 10-hospital ARR estimate.

### 3. Impact Dashboard
A visual summary of the Value Model outputs:

- Top-line split between operational efficiency and litigation avoidance
- Before/after comparison table showing what changes on the ward
- Waterfall chart mapping each driver to its dollar contribution
- Sensitivity analysis showing what Navo's 0.471 sensitivity means in terms of actual patients caught per year vs a clinician baseline

This tab is intended for presentation contexts — it tells the same story as the Value Model but in a more visual, less spreadsheet-like format.

### 4. Hospital Records
A lightweight in-browser database (localStorage) for tracking multiple hospital configurations:

- Save any Value Model configuration under a hospital name
- Reload a saved configuration at any time to restore all inputs
- Export all records as a CSV for pipeline tracking or offline analysis

Records are stored in the browser only — nothing is sent to a server. Clearing browser data will remove them.

---

## How to use it

Open `navo-health-impact-dashboard.html` in any modern browser. No server, no build step, no dependencies beyond a Google Fonts request.

**For a hospital meeting:**
1. Before the meeting, open the Value Model tab and fill in the hospital's known figures (annual deliveries, nursing costs, escalation rates if available)
2. Walk through the Clinical Evidence tab first to establish the model performance baseline
3. Move to the Value Model and open each driver — let the hospital team challenge the assumptions and adjust them live
4. Use the Impact Dashboard as a summary slide
5. Save the configuration under the hospital's name for the record

**For investor use:**
The default inputs represent conservative mid-range estimates for a Singapore restructured hospital (~4,000 deliveries/year). They can be left as-is to show a representative output.

---

## Important caveats

- All values are in **USD**. If presenting in SGD, apply the prevailing exchange rate manually.
- The model assumptions are **illustrative** until validated with actual hospital data. The reference ranges shown for each input are based on published literature and stakeholder interviews, not audited hospital records.
- The litigation driver is the most assumption-heavy. The adverse event rate (default: 0.5 cases/yr) and litigation cost (default: $800K) should be replaced with the hospital's own risk management data before external presentation.
- Navo's test set (n=83) is small. Model performance figures will change as the dataset grows. The TN rate (0.69) used in Driver 1c is hardcoded as a constant and should be updated when the model is retrained.
- This tool is for **internal and investor use only**. It should not be presented as a formal health-economic evaluation.

---

## Technical notes

- Single HTML file — all CSS and JavaScript are embedded, no external dependencies except Google Fonts (DM Serif Display, DM Sans, DM Mono)
- Hospital records are stored in `localStorage` under the key `navoHospitalRecords`
- The file is fully offline-capable once loaded (Google Fonts will fall back to system fonts if offline)
- Mobile-responsive at 640px and 768px breakpoints

---

*© 2026 Navo Health Private Limited · Singapore*
