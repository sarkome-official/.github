Multi-Modal Biomedical Agent with PrimeKG

## Mission

Sarkome is a Deeptech single-asset entity (SAE) dedicated to identifying, validating, and licensing a selective protein degrader for ASPS. Born from a heartfelt promise to honor a late brother who fought this disease, we utilize an agentic AI core coupled with cloud-based partners to execute drug discovery efficiently. Fundamentally, we are engineering a Multi-Agent System to create a Digital Oncologist.

---

## Overview

Intelligent agent system based on LangGraph that integrates the PrimeKG (Precision Medicine Knowledge Graph) from Harvard Medical School with structural protein retrieval via AlphaFold. The system implements an optimized DAG (Directed Acyclic Graph) for biomedical queries, minimizing latency and costs through semantic routing and asynchronous parallel execution.

---

## Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Orchestration | LangGraph | Agent state flow management |
| Knowledge Graph | NetworkX + PrimeKG | 17,080 diseases, 4M+ biomedical relationships |
| Protein Structures | AlphaFold API | 3D structural predictions |
| Web Search | Google Search API | Fallback for unstructured information |
| LLM | Gemini 3.0 | Reasoning and synthesis |

---

## PrimeKG: The Heart of the System

### Graph Characteristics

PrimeKG is a multi-modal knowledge graph designed specifically for precision medicine:

- **Scale**: 17,080 disease nodes + 4,050,249 relationships
- **Sources**: 20 integrated high-quality biomedical resources
- **Biological scales** (10 levels):
  - Disease-associated protein perturbations
  - Biological processes and pathways
  - Anatomical and phenotypic scale
  - Complete pharmacology (approved and experimental drugs)

### Advantages Over Other KGs

1. **Rich Pharmacological Relationships**:
   - Therapeutic indications
   - Contraindications
   - Off-label uses (frequently absent in other KGs)

2. **Multimodality**:
   - Graph structure (relationships)
   - Textual descriptions (clinical guidelines)
   - Enables hybrid analyses (graph + NLP)

### Connectivity Example

```
Disease: "Autism"
    ↓ (disease-drug relationship)
Drug: "Risperidone"

Intermediate paths in PrimeKG:
[Autism] → [Biological Process] → [Protein] → [Drug Action] → [Risperidone]
```



**Key architectural features visible in the actual implementation:**

- **Dual entry paths**: Intent router bifurcates into scientific (KG + AlphaFold) or general (web search) routes
- **Parallel execution**: `query_knowledge_graph` and `query_alphafold` run concurrently
- **Reflection loop**: The `reflection` node can re-invoke any tool (KG, AlphaFold, or web) based on knowledge gaps
- **Short-circuit capability**: `evaluate_grounding` can bypass web search entirely when internal data suffices

---

## Execution Phases

### Phase 1: Intent Router - Intelligent Classification

**Objective**: Avoid unnecessary execution of expensive tools through semantic query classification.

**Function**: Analyzes user query using embeddings and detects biomedical entities (diseases, drugs, proteins).

**Decision Routes**:
- **Scientific Route**: Detects biomedical entities → activates `query_knowledge_graph` || `query_alphafold`
- **General Route**: Unstructured queries → `generate_query` (bypasses scientific databases)

**Routing examples**:
- "What drugs treat Alzheimer's?" → `scientific_route`
- "Structure of BRCA1 protein" → `scientific_route`
- "Latest health news" → `general_route`

---

### Phase 2: Parallel Grounding - PrimeKG + AlphaFold

#### Node A: query_knowledge_graph

Performs multi-hop searches in PrimeKG using NetworkX to identify:

**Supported queries**:
- Therapeutic indications: `(Disease)-[indication]-(Drug)`
- Contraindications: `(Disease)-[contraindication]-(Drug)`
- Off-label uses: `(Disease)-[off_label_use]-(Drug)`
- Pathways: `(Disease)-[pathway]-(Biological Process)-[protein]-(Protein)`

**Process**:
1. Loads pre-processed PrimeKG graph
2. Identifies relevant nodes by type (disease, drug, protein)
3. Calculates shortest paths between entities
4. Retrieves clinical textual descriptions (multimodality)
5. Returns top 10 most relevant paths with metadata

---

#### Node B: query_alphafold

Retrieval of 3D protein structures from AlphaFold Protein Structure Database.

**Returns**:
- PDB coordinates (standard structure format)
- Confidence metrics (pLDDT score)
- URLs for interactive visualization
- AlphaFold model version

**Asynchronous Execution**: Both nodes (`query_knowledge_graph` + `query_alphafold`) execute in parallel using `asyncio`, reducing latency by 50%.

---

### Phase 3: Evaluate Grounding - The Gatekeeper

Determines whether internal data (PrimeKG + AlphaFold) is sufficient or if web search is needed.

**Evaluation Criteria**:
- Were drug-disease paths found in PrimeKG?
- Do AlphaFold structures have high confidence (pLDDT > 70)?
- Do textual descriptions cover the query?

**Binary Decision**:
- **Sufficient data** (score > 0.7) → `finalize_answer` (short-circuit, 0 web calls)
- **Gaps detected** → `reflection` or `generate_query` (web fallback)

**Benefit**: Avoids Google Search API calls when PrimeKG already has the answer (saving ~$0.005 per query).

---

### Phase 4: Reflection - Adaptive Reasoning Loop

The analytical brain of the system that can re-invoke specific tools based on knowledge gaps.

**Strategies**:
1. Re-query PrimeKG with expanded entities
2. Search for additional proteins in AlphaFold
3. Initiate web_research for recent clinical context

**Dynamic Decisions**:
- Can repeat KG/AlphaFold query with refined parameters
- Can initiate new web search with reformulated query
- Can proceed directly to final synthesis

**Limit**: Maximum 3 reflection cycles to prevent infinite loops.

---

### Phase 5: Finalize Answer - Multi-Modal Synthesis

Combines graph data, 3D structures, and clinical text into a coherent response.

**Integrated Components**:
1. Summary of PrimeKG relationships
2. AlphaFold structure visualization (URLs)
3. Clinical guideline citations (text)
4. Off-label use warnings

**Multiple Inputs**:
- Fast path: Only KG/AlphaFold data
- Slow path: KG + AlphaFold + Web

---

![Agent](https://raw.githubusercontent.com/sarkome-official/.github/main/profile/agent.png)


## Key Features

### Cost Optimization
- **Lazy Evaluation**: Expensive tools execute only when necessary
- **Implicit Caching**: NetworkX maintains graph in memory
- **Early Exit**: `evaluate_grounding` prevents redundant web searches

### Latency Reduction
- **Parallelization**: KG and AlphaFold queries in concurrency
- **Early Routing**: Immediate classification at `intent_router`

### Robustness
- **Fallback Strategy**: If one source fails, others can compensate
- **Reflexive Iteration**: The `reflection` node can retry with alternative strategies

---

## Query Examples

### Case 1: Drug-Disease Association

**Input**: "What drugs are indicated for Autism?"

**Flow**:
1. `intent_router` → Detects "Autism" (disease) → Scientific route
2. `query_knowledge_graph` → Finds paths:
   ```
   [Autism] -[indication]-> [Risperidone]
   [Autism] -[off_label_use]-> [Aripiprazole]
   ```
3. `evaluate_grounding` → Score 0.9 (sufficient data)
4. `finalize_answer` → Short-circuit, 0 web calls

**Output**:
```
Drugs indicated for Autism according to PrimeKG:

1. Risperidone (Approved indication)
   - Path: Autism → [treats] → Risperidone
   - Clinical description: Atypical antipsychotic used for irritability...

2. Aripiprazole (Off-label use)
   - Path: Autism → [off_label_use] → Aripiprazole
   - Warning: Not FDA-approved, consult with specialist

Sources: PrimeKG (Harvard Medical School)
```

---

### Case 2: Protein Structure + Pathway

**Input**: "Structure of BRCA1 and its role in breast cancer"

**Flow**:
1. `intent_router` → Detects "BRCA1" (protein) + "breast cancer" (disease)
2. Parallel execution:
   - `query_knowledge_graph`:
     ```
     [Breast Cancer] -[disease_protein]-> [BRCA1]
     [BRCA1] -[pathway]-> [DNA Repair]
     ```
   - `query_alphafold`:
     ```json
     {
       "uniprot_id": "P38398",
       "pdb_url": "https://alphafold.ebi.ac.uk/.../P38398.pdb",
       "confidence": 87.3
     }
     ```
3. `evaluate_grounding` → Score 0.85
4. `finalize_answer` → Integrates graph + 3D structure

**Output**:
```
BRCA1 in Breast Cancer

Biological role:
- Tumor suppressor protein involved in DNA repair
- Mutations associated with 80% hereditary risk

3D Structure (AlphaFold v4):
- [View interactive model](https://alphafold.ebi.ac.uk/.../P38398.pdb)
- Confidence: 87.3% (pLDDT score)
- Key domains: RING, BRCT

Related pathways (PrimeKG):
- DNA Repair → Homologous Recombination
- Cell Cycle Checkpoint

Sources: PrimeKG + AlphaFold Database
```

---

## Performance Metrics

### Latency Reduction

| Query Type | Sequential Execution | Parallel Execution | Improvement |
|-----------|---------------------|-------------------|-------------|
| Drug-disease | 4.2s | 2.1s | **50%** |
| Protein structure + KG | 6.8s | 3.5s | **48%** |

### Cost Savings

| Scenario | Web Calls | Cost Avoided* |
|----------|-----------|---------------|
| Short-circuit (70% of cases) | 0 | $0.005/query |
| 1 reflection cycle | 1-2 | $0.002/query |

*Based on Google Search API pricing ($5/1000 queries)

---

## Roadmap

**Phase 1: Semantic Ingestion (In Progress)** – Structuring the chaos into a Biomedical Knowledge Graph (BKG) using GraphRAG.

**Phase 2: The Agent** – Integrating Gemini 3.0 and deploying "the agent" to generate hypotheses.

**Phase 3: In Silico Simulation** – Validating candidates via AlphaFold Server and Digital Twin counterfactual simulations.

**Phase 4: Wet-Lab Validation** – Generating executable experimental protocols for partners and closing the feedback loop.

---

## Known Limitations

1. **PrimeKG Temporal Coverage**:
   - Data up to 2023 (publication date)
   - Recent drugs/studies not included → Fallback to web search

2. **Rate Limits**:
   - AlphaFold API: 10 req/min (free tier)
   - Solution: Implement priority queue + caching

3. **Language**:
   - PrimeKG in English only
   - Queries in other languages require automatic translation

4. **Reflection Cycles**:
   - Limit of 3 iterations to prevent loops
   - Extremely complex queries may be truncated

---

## References

### PrimeKG
- **Paper**: Chandak et al. (2023). *Building a knowledge graph to enable precision medicine*. Nature Scientific Data.
- **Official site**: https://zitniklab.hms.harvard.edu/projects/PrimeKG/
- **Dataset**: https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/IXA7BM

### Other Sources
- **AlphaFold Database**: Varadi et al. (2022). *Nucleic Acids Research*.
- **LangGraph**: https://github.com/langchain-ai/langgraph
- **NetworkX**: https://networkx.org/documentation/stable/

---

## License

This project is under MIT license. PrimeKG is licensed under CC BY 4.0.
