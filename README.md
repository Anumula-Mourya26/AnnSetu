# 🌾 AnnSetu — "Bridge to the Grain"
### *Live Gate & Queue Transparency Layer for MSP Procurement*
**Smart India Hackathon 2026 · Problem Statement 26032**  
*Ministry of Consumer Affairs, Food & Public Distribution · Theme: Smart Automation*

---

> **From "your slot is Tuesday" to "your money is in your account" — live, honest, farmer-facing MSP procurement.**

**AnnSetu** is a smart digital operations and gate transparency platform that sits beside existing state procurement systems (e-Uparjan, e-Kharid, RAJFED, etc.) rather than replacing them. It solves the critical bottlenecks documented in Indian grain mandis during peak procurement seasons:

1. **Deterministic Live Queue & Dynamic ETA**: Powered by operations-research queueing theory (using active counter count $c$ and mandi capacity factor $F$) rather than cosmetic counters or unexplainable black-box ML.
2. **4-Stage Payment-Trust Timeline**: The only platform that explicitly models and exposes the 4-stage payment journey (*Sold → Advice Generated on PFMS → Advice Reached Commission Agent/Arhatiya → Credited to Farmer DBT Account with UTR*).
3. **4-Digit Security PIN Authentication**: Secure, typo-preventing PIN registration and login for farmers and mandi vendors, replacing legacy credentials while keeping District Admin flows intact.
4. **District Congestion Command Center**: Turns passive charts into an actionable 1-click redirect and load-balancing mechanism to redistribute incoming tractor traffic before mandis bottleneck.
5. **Multilingual & Inclusive Access**: Full bilingual (English / Hindi / Regional) interface with high-contrast accessibility and SMS dispatch readiness.

---

## 🏛️ Comprehensive Architecture & Specification
- **[Engineering Blueprint (Docx)](AnnSetu_Engineering_Blueprint.docx)**: Complete 20-section specification covering data schemas, queueing engine, API contracts, notification matrices, security audit trails, and government adoption roadmap.
- **[Strategic Analysis & Jury Evaluation (Docx)](SIH26032_Strategic_Analysis.docx)**: Competitive analysis of candidate concepts, attack mitigations, and jury positioning strategy.

---

## 🚀 Quickstart & Development

### 1. Requirements
- Python 3.10+ (tested on Python 3.13)
- SQLite (included, zero-dependency async engine) or PostgreSQL for production

### 2. Installation & Setup
```bash
# Clone the repository
git clone https://github.com/Anumula-Mourya26/AnnSetu.git
cd AnnSetu/annsetu/backend

# Install dependencies
pip install -r requirements.txt
```

### 3. Seed Realistic Mandi Data
Populate the database with pre-configured Punjab & MP mandi centres (Khanna Grain Mandi, Samrala Mandi, Sahnewal), MSP rates, operational 120-minute slots, and active queue tokens:

```bash
python seed.py
```


### 4. Run Automated Tests
```bash
python -m pytest -v
```
All 22 test suites run out of the box covering:
- Health check & API metadata
- Queue Engine dynamic ETA formula calculation
- End-to-end Farmer Journey (Centres → Slot Booking → Gate Pass → Queue Admission → Weighbridge)
- Staged Payment-Trust Timeline & UTR tracking
- 4-Digit PIN security, confirmation matching, and legacy credential retirement
- Admin Mandi Onboarding & User Management

### 5. Launch Backend Server & UI
```bash
python -m uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```
- **Web Application Portal**: [http://127.0.0.1:8000](http://127.0.0.1:8000)
- **Interactive OpenAPI (Swagger) Docs**: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

---

## 🔐 Default Test Credentials

| Portal Role | Identifier | Password / PIN |
| :--- | :--- | :--- |
| **District Administrator** | Admin ID: `123457890` | `123456789` |
| **Mandi Vendor (Khanna Hub)** | Mobile: `9876500001` | PIN: `1234` |
| **Mandi Vendor (Samrala)** | Mobile: `9876500002` | PIN: `1234` |
| **Pre-Registered Farmer** | Mobile: `9876543210` | PIN: `1234` |
| **New Farmer / Vendor** | Registered Mobile | 4-Digit PIN set during signup / onboarding |

---

## 📐 Dynamic ETA Calculation Formula

AnnSetu avoids unexplainable black-box neural networks for wait-time estimation, using a verifiable operations-research model:

$$\text{ETA}(p) = \left\lceil \frac{p \times T_{\text{avg}}}{c \times F} \right\rceil$$

Where:
- $p$ = Farmer's live position in queue
- $T_{\text{avg}}$ = Average weighing & inspection duration per tractor (e.g. 15 minutes)
- $c$ = Number of active operational counters / weighbridges
- $F$ = Mandi Capacity Factor ($1.0$ = Normal, $0.8$ = Busy, $0.6$ = Congested, $0.0$ = Paused)

Whenever an operator completes inspection or an incoming tractor is checked in at the gate, all downstream waiting farmers receive real-time updates.

---

## 🛡️ Staged Payment-Trust Timeline

```
[ 1. Sold ] ──▶ [ 2. Advice Generated ] ──▶ [ 3. Advice Reached Agent ] ──▶ [ 4. Credited (DBT) ]
  Weighbridge         PFMS / State Portal          Arhatiya / Mandi ledger         Farmer Bank Account
  Inspection          Payment Advice (PPA)         Reconciliation                  Bank UTR Generated
```

This closes the critical trust gap in mandi operations, giving farmers transparent end-to-end tracking of their MSP proceeds.

---

## 📜 License
Distributed under the MIT License. Built for the Smart India Hackathon (SIH 2026).
