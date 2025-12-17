# Sarkome: The Autonomous Discovery Engine

> **From Passive Search to Causal Inference.**
> *Building a Neuro-Symbolic "Digital Oncologist" to cure Alveolar Soft Part Sarcoma (ASPS).*

---

## 🚀 Mission

Sarkome is a **Deeptech** single-asset entity (SAE) dedicated to identifying, validating, and licensing a selective protein degrader for ASPS. Our primary objective is to develop **SAR-001**, a selective PROTAC degrader targeting the **ASPSCR1-TFE3** fusion protein.

Born from a heartfelt promise to honor a late brother who fought this disease, we utilize an agentic AI core coupled with cloud-based partners to execute drug discovery efficiently. **Fundamentally, we are engineering a Multi-Agent System (the technology) to create a Digital Oncologist (the product).**

---

## 🛑 The Problem: The Epistemological Gap

### Why "Search" is Not "Discovery"

In the context of ultra-rare diseases like ASPS, the current model of facilitating bibliographic search is **epistemologically insufficient**. While LLMs excel at text processing, they fail at scientific rigor due to:

1. **Stochastic Hallucination:** LLMs operate on statistical probability, not biological truth, often inventing plausible but impossible interactions.
2. **Fragmented Causality:** Search engines cannot independently infer that an upregulation is a necessary downstream effector of a fusion unless stated verbatim in the text.
3. **Inability to Reason:** Standard Vector RAG systems fail at **multi-hop reasoning** required to connect distant nodes of information.

---

## 🧠 The Solution: The Causal Engine

**The Causal Engine** is a **Neuro-Symbolic pipeline** designed to ingest unstructured data, reason about it logically, and validate findings via simulation.

### The Data Flow Pipeline

1. **Ingestion (From Text to Structure):** We deploy NLP agents (fine-tuned **BioBERT** models) to mine literature and extract semantic "triplets" (e.g., "ASPSCR1 *physically_interacts_with* VCP").
2. **The Brain (Biomedical Knowledge Graph):** Unlike vector stores, our **Graph Database** understands network topology, serving as the "Ground Truth" to prevent hallucination.
3. **Reasoning (The Neuro-Symbolic Layer):** We utilize **DeepProbLog** to fuse neural pattern matching with symbolic logic.
* *Neural Component:* Predicts link probabilities in noisy data.
* *Symbolic Component:* Enforces inviolable biological rules (e.g., rejecting targets essential for normal tissue survival).


4. **Validation (*In Silico* Filtering):**
* **Structural Docking:** Triggering **AlphaFold-Multimer** to verify physical binding.
* **Causal Inference:** Using **Do-Calculus** to simulate counterfactuals and distinguish drivers from passengers.



---

## 🧬 The Target: Alveolar Soft Part Sarcoma (ASPS)

ASPS is an ultra-rare, malignant sarcoma driven by a specific, catastrophic genetic accident rather than cellular aging.

* **The Driver:** A single chromosomal translocation, **t(X;17)(p11;q25)**, fuses **ASPSCR1** and **TFE3**.
* **The Mechanism:** This chimeric oncoprotein acts as a "molecular dictator," hijacking **Super-Enhancers** and the **VCP/p97 complex** to physically restructure the genome.
* **The Challenge:** ASPS is resistant to standard chemotherapy and creates a dense network of blood vessels (vascular shielding) to feed itself and metastasize early.

**Our Strategy:** Instead of managing symptoms with TKIs, we aim to induce a "synthetic lethal" collapse by targeting the fusion itself and its structural dependencies.

---

## 🗺️ Roadmap

We are systematically building a multi-agent system to solve the logic of ASPS.

* **Phase 1: Semantic Ingestion (In Progress)** – Structuring the chaos into a Biomedical Knowledge Graph (BKG) using GraphRAG.
* **Phase 2: The Neuro-Symbolic Logic Engine** – Integrating DeepProbLog and deploying autonomous agents ("Structural Biologist" & "Medicinal Chemist") to generate hypotheses.
* **Phase 3: *In Silico* Simulation** – Validating candidates via AlphaFold-Multimer and Digital Twin counterfactual simulations.
* **Phase 4: Wet-Lab Validation** – Generating executable experimental protocols for partners and closing the feedback loop.

---

## 🛠️ Tech Stack

* **Language:** Python (PyTorch/DeepProbLog)
* **Compute:** Serverless Functions
* **Database:** Managed Graph Database
* **Frontend:** Astro

---

### Contributing

Sarkome is an open initiative. We invite developers, computational biologists, and data scientists to help us bridge the gap between data and a cure.
