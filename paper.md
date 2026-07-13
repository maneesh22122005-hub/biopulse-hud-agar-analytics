---
title: 'BioPulse HUD: A browser-based platform for automated colony detection, morphological classification, and growth-kinetics estimation from petri dish photographs'
tags:
  - JavaScript
  - computer vision
  - microbiology
  - colony counting
  - image analysis
  - browser-based software
authors:
  - name: '[Your Full Name]'
    orcid: '[Your ORCID iD, e.g. 0000-0000-0000-0000]'
    affiliation: 1
affiliations:
  - name: '[Your Institution / Affiliation]'
    index: 1
date: '[Submission date]'
bibliography: paper.bib
---

# Summary

Counting and characterizing bacterial colonies grown on agar plates is a routine but
labor-intensive task in microbiology teaching and research labs. BioPulse HUD is a
browser-based, client-side software tool that automates this process directly from a
photograph of a petri dish. Given an input image, it performs illumination correction,
locates the plate boundary, segments candidate colonies via adaptive thresholding and
connected-component analysis, corrects for colonies that have grown into contact with one
another or been erroneously fragmented during segmentation, classifies detected regions as
colonies, likely contaminants, or optical artifacts, and estimates population growth
kinetics either from a single timepoint (simulation mode) or from multiple photographs of
the same plate taken at different incubation times (measured-fit mode). All processing —
including an optional pretrained MobileNet embedding step used for outlier/duplicate
cleanup — runs locally in the browser via JavaScript and TensorFlow.js, so plate images
are never transmitted to an external server. This is relevant to laboratories operating
under data-governance constraints on strain- or patient-derived imagery, and it means the
tool has no server-side infrastructure or ongoing hosting cost.

# Statement of Need

Manual colony counting is slow, and inter-observer variability is well documented,
particularly at moderate-to-high confluence where colonies begin to merge visually.
Several open-source counting tools exist (see State of the Field below), but most require
a desktop install, a specific operating system, or a scripting environment (Python, ImageJ
macros), which creates friction for teaching labs, field settings, or users without a
computational background. BioPulse HUD is aimed at this gap: a zero-install,
single-file, browser-based tool that a user can open directly and use on a plate
photograph with no environment setup. It targets microbiology instructors, students, and
researchers who need a fast, reproducible colony count and a first-pass morphological
readout without installing software or sending images off their own machine.

# State of the Field

Existing open-source colony counters include OpenCFU [@geissmann2013opencfu], CellProfiler
[@carpenter2006cellprofiler], and various ImageJ/Fiji colony-counting plugins
[@schindelin2012fiji]. These tools are mature and, in the case of CellProfiler and Fiji,
support far more general image-analysis pipelines than colony counting alone. They differ
from BioPulse HUD primarily in deployment model: each requires a local desktop
installation, and building a custom pipeline in CellProfiler or an ImageJ macro requires
some familiarity with the respective tool's pipeline/scripting interface. BioPulse HUD
trades that flexibility for a fixed, opinionated pipeline that runs immediately in a
browser with no install step, and it adds a split/merge correction stage specifically
tuned for colonies that have grown into contact — a common failure mode of naive
threshold-and-connected-components approaches on moderate-to-high confluence plates. The
choice between these tools is therefore largely one of flexibility versus immediacy: users
needing a custom multi-stage biological image-analysis pipeline are better served by
CellProfiler or Fiji; users wanting a fast, no-install colony count and first-pass
morphology read are the intended audience for BioPulse HUD.

# Software Design

The detection pipeline proceeds through several explicit stages, each addressing a
specific failure mode observed during development:

1. **Illumination correction.** A smoothed local-brightness field, computed via a
   summed-area table for O(1) per-pixel cost, is subtracted from the raw image to flatten
   flash hot-spots and rim shadowing before thresholding.
2. **Plate boundary detection**, so that thresholding and blob search are restricted to
   the true interior of the dish rather than background outside it.
3. **Adaptive (Otsu) thresholding** within the plate mask, with polarity (colonies darker
   or lighter than background) inferred automatically from the minority-class assumption
   inside the mask, rather than requiring the user to specify it.
4. **Connected-component analysis**, followed by a **split-correction** step (a chamfer
   distance-transform watershed-style pass that separates oversized blobs likely
   representing multiple colonies in contact) and a **merge-correction** step
   (area-weighted centroid merging for blobs erroneously fragmented during erosion).
5. **Artifact classification**, distinguishing colonies from suspected contaminants and
   optical artifacts (for example air bubbles) using shape regularity, size percentile
   relative to the plate's own colony population, and local contrast.
6. **Morphology-based genus classification** (optional), which scores each colony's
   measured color, relative size, and margin regularity against a small reference registry
   of genus-level morphology profiles and reports the closest match with a confidence
   estimate derived from how many of the three attributes agree. This is presented
   explicitly as a first-visual-impression aid, not a diagnostic identification — a
   genuine species/genus call requires culture confirmation or sequencing.

The split/merge correction pair (stage 4) is the tool's main algorithmic contribution
relative to a naive thresholding pipeline, and is evaluated directly in the Research
Impact Statement below via an ablation (correction stages enabled vs. disabled).

Growth-kinetics estimation is provided in two modes: a descriptive simulation mode
(logistic, Gompertz, or exponential extrapolation from a single detected count and
user-specified doubling time), and a measured-fit mode that fits the same phenomenological
models to ≥2 observed (time, count) pairs from repeated photographs of the same plate via
nonlinear least-squares (Nelder–Mead simplex, multiple restarts), reporting goodness-of-fit
as R².

# Research Impact Statement

[This section requires real experimental results before submission — see the project's
`biopulse_methods_and_validation.md` for the intended experimental design. At minimum,
JOSS reviewers will expect this section to report:]

- [Split/merge correction ablation: colony-count accuracy with these stages enabled vs.
  disabled, on a labeled image panel, quantifying their individual contribution.]
- [Artifact classification performance: sensitivity/specificity or a confusion matrix for
  the colony / contaminant / bubble three-way classification against expert-labeled
  regions.]
- [Any evidence of external use: adopters, integrations, or feedback from users other than
  the author(s).]

# AI Usage Disclosure

Development of BioPulse HUD involved AI-assisted coding (Claude, Anthropic) for
implementation of specific pipeline stages, debugging, and iteration. Problem framing,
architectural decisions (including the choice of a client-side-only, no-install
deployment model, the split/merge correction design, and the decision to present genus
classification as a non-diagnostic aid with an explicit confidence score), and validation
of all reported results are the author's own. [Expand this section with specifics once the
real development/validation process is finalized — JOSS requires this disclosure to
describe how AI tools were used and what the human contribution consisted of, not merely
that AI assistance occurred.]

# Acknowledgements

[Acknowledge any funding, advisors, or other contributors here, if applicable.]

# References
