# Sarkome

**Mission:** To algorithmically identify, validate, and license therapeutic assets for Alveolar Soft Part Sarcoma (ASPSCR1-TFE3) without building physical infrastructure.

**Architecture:** Agentic AI (In Silico) → CRO Validation (Wet Lab) → IP Licensing (Commercial)

---

## 1. System Architecture

This project does not build a "platform" first. It builds a **Product Pipeline**. We operate as a "Single Asset Entity" (SAE)—a company that exists solely to find and sell one specific drug asset.

### The Pipeline Flow

1. **Input:** Global biomedical data (PubMed, ChEMBL, PDB)
2. **Processing:** A stack of 3 AI Agents filters noise to find signal
3. **Output:** A "Target Product Profile" (TPP) ready for wet-lab purchase

---

## 2. Technical Stack (The "Agentic Core")

We replace a human R&D team with three specific software agents.

### 🕵️ Agent A: The Miner (Knowledge Extraction)

**Function:** Scrapes unstructured text to find hidden relationships.

**Tech Stack:** Python, LangChain, PubMed API, GPT-4o

**Logic:**
1. Query PubMed for `TFE3 AND (Kinase OR Inhibitor)`
2. Use LLM to extract entity pairs: `(Drug X) --inhibits--> (Target Y)`
3. Filter for targets that interact with the TFE3 fusion protein

**Output:** `candidates_raw.json` (List of ~50 potential drugs)

---

### ⚛️ Agent B: The Physicist (Structural Validation)

**Function:** Simulates physics to see if the drug fits the target.

**Tech Stack:** Google Colab, AlphaFold 3 (Protein Folding), DiffDock (Molecular Docking)

**Logic:**
1. Generate 3D PDB structure of ASPSCR1-TFE3 IDR (Intrinsically Disordered Region)
2. "Dock" each candidate from Agent A into the protein pocket
3. Calculate Gibbs Free Energy (ΔG)

**Threshold:** If Binding Energy > -8.0 kcal/mol, DISCARD

**Output:** `docked_results.csv` (Ranked list of ~10 high-fit drugs)

---

### 🧪 Agent C: The Skeptic (Safety & Tox)

**Function:** Checks for chemical toxicity and commercial viability.

**Tech Stack:** RDKit, ChEMBL API, Tox21 Database

**Logic:**
1. Check molecular weight (<500 Da for oral bioavailability)
2. Flag PAINS (Pan-Assay Interference Compounds) – "fake" drugs
3. Check prior FDA toxicity flags

**Output:** `final_shortlist.md` (Top 3 Candidates)

---

## 3. Execution Roadmap (2026–2031)

### 🟢 Phase 1: Signal Identification (2026 Q1)

**Goal:** Run the code to find the "Top 3" candidates.

**Steps:**
1. Initialize this repository structure
2. Run `miner_agent.py` to scan 50,000 abstracts
3. Run `docking_agent.py` on the top hits
4. Generate the Target Product Profile (TPP) document

---

### 🟡 Phase 2: Biological Proof (2026 Q2–Q4)

**Goal:** Convert code into biological truth.

**Steps:**
1. **Synthesize:** Order small batches (5-10mg) of the Top 3 compounds from a synthesis CRO (e.g., WuXi AppTec)
2. **Validate:** Ship compounds to a biology CRO (e.g., Charles River)
3. **The Experiment:**
   - Assay A: Cell Viability (IC50) on ASPS-1 cells
   - Assay B: Western Blot (Proof that TFE3 protein levels drop)

**Success Metric:** IC50 < 100nM

---

### 🟠 Phase 3: Regulatory Asset Generation (2027)

**Goal:** Make the asset "Pharma-Ready."

**Steps:**
1. **ADME/Tox:** Contract a lab to run liver toxicity panels
2. **FDA Meeting:** Submit a "Pre-IND" meeting request
3. **Strategy:** Propose a "Basket Trial" (combining ASPS patients with Renal Cell Carcinoma patients)

---

### 🔴 Phase 4: The Exit (2028)

**Goal:** License the IP.

**Steps:**
1. **Package the data:** (AI Prediction + Wet Lab Proof + FDA Minutes)
2. **Pitch to major Pharma** (Pfizer, Novartis, etc.) needing assets for Kidney Cancer

**Outcome:** Licensing deal (Upfront payment + Royalties)

---

### 🔵 Phase 5: Scale & Automation (2029–2031)

**Goal:** Repeat the process.

**Timeline:**
- **2029:** Re-invest capital to target Epithelioid Sarcoma
- **2030:** Launch "ASPS Atlas API" (The software platform)
- **2031:** Automate the loop with Cloud Labs (Emerald Cloud Lab) for fully robotic discovery

---


Coming soon...


*Before life, the endless fight.*
