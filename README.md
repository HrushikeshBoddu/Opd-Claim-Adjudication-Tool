# 🏥 OPD Claim Adjudication Tool

An AI-powered tool that automates the approval and rejection of Outpatient Department (OPD) insurance claims using a smart rule engine and a free local LLM — no API key required.

---

## 📁 Project Structure

```
opd-adjudication/
│
├── Opd Claim Adjudication Tool.ipynb   # Main notebook (run this)
├── policy_terms.json          # Insurance policy config
├── test_cases.json            # 10 test cases with expected outputs
├── adjudication_rules.md      # Business rules documentation
└── sample_documents_guide.md  # Guide for creating test documents
```

---

## 🚀 How to Run

### Step 1 — Open in Google Colab
Upload `Opd Claim Adjudication Tool.ipynb` to [colab.research.google.com](https://colab.research.google.com)

### Step 2 — Upload the JSON files
In the Colab file panel, also upload:
- `policy_terms.json`
- `test_cases.json`

### Step 3 — Install dependencies
Run the first cell:
```python
!pip install transformers torch sentencepiece ipywidgets -q
```

### Step 4 — Run all cells top to bottom
That's it. The UI will appear at the end.

---

## ✨ Features

- **Rule Engine** — 10 prioritized rules covering all claim scenarios
- **Free AI Model** — Uses `google/flan-t5-base` locally, zero API cost
- **Interactive UI** — 3-tab interface built with `ipywidgets`
- **Batch Validation** — Validates all 10 test cases with a score card
- **AI Explanations** — Plain English explanation for every decision

---

## 🖥️ UI Tabs

| Tab | What it does |
|-----|-------------|
| 📝 Submit Claim | Fill a form and adjudicate a custom claim |
| 🧪 Test Cases | Pick any of the 10 official test cases and run it |
| 📊 Batch Validate | Run all 10 at once and see pass/fail table |

---

## ⚖️ Decision Rules (in priority order)

| # | Rule | Decision |
|---|------|----------|
| 1 | Missing prescription | REJECTED |
| 2 | 3+ claims same day | MANUAL REVIEW |
| 3 | Obesity / weight loss diagnosis | REJECTED |
| 4 | Diabetes within 90-day waiting period | REJECTED |
| 5 | Cosmetic dental procedure in bill | PARTIAL |
| 6 | MRI without pre-auth, amount > ₹10,000 | REJECTED |
| 7 | Claim amount > ₹5,000 per-claim limit | REJECTED |
| 8 | Treatment at network hospital | APPROVED (20% off) |
| 9 | Ayurvedic / homeopathic treatment | APPROVED |
| 10 | Everything else | APPROVED (10% copay) |

---

## 🧪 Test Results

All 10 official test cases pass with correct decisions and amounts:

```
TC001 → APPROVED   ₹1,350  ✅
TC002 → PARTIAL    ₹8,000  ✅
TC003 → REJECTED           ✅
TC004 → REJECTED           ✅
TC005 → REJECTED           ✅
TC006 → APPROVED   ₹4,000  ✅
TC007 → REJECTED           ✅
TC008 → MANUAL_REVIEW      ✅
TC009 → REJECTED           ✅
TC010 → APPROVED   ₹3,600  ✅

Score: 10/10 🎉
```

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| Language | Python 3 |
| AI Model | google/flan-t5-base (HuggingFace, free) |
| UI | ipywidgets |
| Environment | Google Colab |
| Data | JSON |

---

## 💡 Assumptions Made

- Waiting period for diabetes is calculated from `member_join_date` field
- MRI pre-auth check applies only when claim amount exceeds ₹10,000
- Teeth whitening bill amount is set to 35% of total claim for UI submissions
- Network hospital discount is always 20% as per policy
- Default copay is 10% for all standard approvals
- Alternative medicine check covers: ayurveda, homeopathy, unani, panchakarma

---

## ⏱️ Development Time

| Task | Time Spent |
|------|-----------|
| Understanding business rules | ~1 hour |
| Building rule engine | ~2 hours |
| Integrating free AI model | ~1 hour |
| Building interactive UI | ~2 hours |
| Testing all 10 cases | ~30 mins |
| **Total** | **~6.5 hours** |
