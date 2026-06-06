# BioBERT Biomedical Literature Mining

Mining biomedical literature from **PubMed** using **BioBERT** Named Entity Recognition (NER) to extract diseases, genes, and chemicals, then building a **co-occurrence knowledge graph** to surface the most connected biomedical concepts in chromatin / epigenetics / cancer research.

> Author: **Pradip Palekar (MT25215)** — M.Tech Computational Biology, IIIT Delhi

---

## What this project does

1. **Mines abstracts** from PubMed via the NCBI Entrez API across three research themes:
   - Chromatin compartments & cancer epigenetics
   - Biomolecular condensates & phase separation in transcription
   - Epigenetic memory & cancer drug resistance
2. **Extracts entities** using three specialized BioBERT NER models (disease / gene / chemical).
3. **Cleans & normalizes** entities (removes author/affiliation noise, merges spelling variants, drops tokenizer fragments).
4. **Builds a knowledge graph** of entities that co-occur in 2+ abstracts.
5. **Identifies key hubs** via degree centrality (candidate important genes / diseases / drug targets).
6. **Visualizes** the results: frequency plots, entity-type distribution, word cloud, topic distribution, and the network graph.

---

## Pipeline

```
PubMed (Entrez API)
        │  fetch abstract text (XML → AbstractText only)
        ▼
BioBERT NER ×3  (disease / gene / chemical)
        │  extract + confidence filter (>0.7)
        ▼
Clean + Normalize  (drop noise, merge variants, kill fragments)
        │
        ├──► Top entities + type distribution
        ├──► Word cloud
        ├──► Topic distribution
        ▼
Co-occurrence Knowledge Graph  (NetworkX)
        │
        └──► Degree-centrality hubs
```

---

## Results (sample run)

| Metric | Value |
|---|---|
| Papers mined from PubMed | 135 |
| Total entities extracted | 500 |
| Unique entities | 298 |
| Knowledge graph nodes | 13 |
| Knowledge graph edges | 14 |

**Top hubs surfaced:** cancer, tumor, breast cancer, inflammation, estrogen receptor, hepatocellular carcinoma, MYD88 ↔ Waldenström macroglobulinemia, and a DNA-methylation cluster (5mC / 5hmC / bisulfite).

### Figures

| File | Description |
|---|---|
| `top_entities.png` | Top 20 entities + entity-type pie chart |
| `knowledge_graph.png` | Entity co-occurrence network |
| `biomedical_wordcloud.png` | Word cloud of mined terms |
| `topic_distribution.png` | Mentions per research topic |
| `top_hubs.png` | Most connected entities (centrality) |

---

## Tech stack

`transformers` · `torch` · `biopython` (Entrez) · `networkx` · `pandas` · `numpy` · `matplotlib` · `seaborn` · `wordcloud`

**NER models:** `alvaroalon2/biobert_diseases_ner`, `alvaroalon2/biobert_genetic_ner`, `alvaroalon2/biobert_chemical_ner`

---

## How to run

1. Open the notebook in Google Colab or Jupyter.
2. Set your email for the Entrez API (required by NCBI):
   ```python
   Entrez.email = "your_email@example.com"
   ```
3. Run all cells top to bottom. Generated CSVs and PNGs are saved to the working directory.

```bash
pip install -r requirements.txt
```

---

## Limitations & future work

- **Edges = co-mention, not validated interaction.** An edge means two entities appeared in the same abstract; it does not imply a confirmed biological relationship.
- **Rule-based cleaning.** Entity normalization uses string rules, so a different random sample of abstracts may occasionally surface a new tokenizer fragment.
- **Future work:** replace string-based cleaning with **scispaCy + UMLS entity linking** to map every mention to a canonical concept ID — this eliminates fragments and spelling variants by design and is the publication-grade approach.

---

## Relevance

This mirrors the literature-mining approach used in AI-driven drug discovery (e.g. BenevolentAI, Recursion, Innoplexus) to surface candidate gene–disease–drug relationships from unstructured biomedical text.
