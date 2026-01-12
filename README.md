# General Adaptive Agent Ontology (GAAO 1.0)

GAAO 1.0 (General Adaptive Agent Ontology) is a unified event-sourced ontology for modeling and steering adaptive agents through time, integrating semantic topology, constraint systems, condition models, evidential reasoning, transformation dynamics, and recursive adaptation loops.

This repository contains the canonical v1.0 specification and LaTeX source for the GAAO formal ontology.

---

## Contents

- `GAAO v1 Spec.md`  
  Human-readable specification of GAAO 1.0, including the tuple definition  
  \(A = (E, C, K, X, R, P, \Omega, I, L)\) and layer-by-layer semantics.

- `main.tex`  
  LaTeX source for the GAAO 1.0 preprint. This is the file used to generate
  the public PDF.

There is intentionally **no implementation code** in this repository. GAAO is an ontological and mathematical specification, not an algorithm or software framework.

---

## Preprint and DOI

The current preprint is archived on Zenodo:

- **Version v1 DOI:**  
  <https://doi.org/10.5281/zenodo.18226497>

- **Concept DOI (all versions):**  
  <https://doi.org/10.5281/zenodo.18226496>

When citing a specific version, use the version DOI (`...497`). When referring to “the latest available version”, you may use the concept DOI (`...496`).

---

## Building the PDF from `main.tex`

To regenerate the preprint PDF from source you need a standard LaTeX distribution
(e.g. TeX Live, MacTeX, or MiKTeX).

From the repository root:

```bash
pdflatex main.tex
pdflatex main.tex
