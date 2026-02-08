# General Adaptive Agent Ontology (GAAO 1.0)

GAAO 1.0 is a unified event-sourced ontology for modeling and steering adaptive agents through time, integrating semantic topology, constraint systems, condition models, evidential reasoning, transformation dynamics, and recursive adaptation loops.

There is intentionally **no implementation code** in this repository. GAAO is an ontological and mathematical specification, not an algorithm or software framework.

---

## Structure

```
spec/                  Canonical v1.0 specification
  GAAO v1 Spec.md        Human-readable spec (tuple definition + layer semantics)
  main.tex               LaTeX source for the preprint PDF

research/              Layer-by-layer working papers and analysis
  Adaptive Reasoning & Recursive Loop.md
  Condition Space Layer.md
  Constraint Fabric.md
  Event Ledger Layer.md
  Evidential Graph Layer.md
  Semantic Topology Layer.md
  Transformation Layer.md
  GAAO v1 Formal Ontology.md
  Framwork Comparison - MAPE-K BDI RL.md
  Preprint gaao v1.0 - main.md

preprint/              Published preprint PDF
  gaao-1.0-preprint.pdf

arxiv/                 arXiv submission materials
  main.tex               LaTeX source as submitted
  arXiv generated submission PDF.pdf

instantiations/        Example GAAO instantiations
  Personal Health & Fitness Agent (md + pdf)
  Startup Founder Agent (md + pdf)
```

---

## Preprint and DOI

The current preprint is archived on Zenodo:

- **Version v1 DOI:**
  <https://doi.org/10.5281/zenodo.18226497>

- **Concept DOI (all versions):**
  <https://doi.org/10.5281/zenodo.18226496>

When citing a specific version, use the version DOI (`...497`). When referring to "the latest available version", use the concept DOI (`...496`).

---

## Building the PDF

To regenerate the preprint PDF from source:

```bash
cd spec/
pdflatex main.tex
pdflatex main.tex
```

Requires a standard LaTeX distribution (TeX Live, MacTeX, or MiKTeX).

---

## Lineage

GAAO was abstracted from the Intentional Life Architecture (ILA). ILA's architecture is a specific instantiation of GAAO's general ontology. GAAO is domain-agnostic; ILA applies it to human life design.
