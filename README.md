# nv-lab-notes

Lab notes on NV center physics: superresolution, localization theory, and quantum sensing.
A living reference for group members, written to be derived from first principles.

---

## What this is

Running collection of technical writeups started during a literature review on superresolution
techniques for NV centers. Each note aims to be self-contained — definitions, derivations,
and physical intuition in one place — pitched at a first-year grad student in quantum sensing
who has a solid physics background but may be new to the topic.

Notes are written in HTML (rendered with MathJax) so they can be read directly in a browser
without any software installation. A LaTeX source is included for each note if you want to
compile a PDF or adapt the content.

**Live site:** https://yourusername.github.io/nv-lab-notes/

---

## Notes

| # | Title | Topics | HTML | TeX |
|---|-------|---------|------|-----|
| 01 | Positional Accuracy of NV Centers | σ scaling, Fisher information, Cramér–Rao bound, Rayleigh curse, superresolution | [view](NV_sigma_scaling_v2.html) | [source](NV_sigma_scaling.tex) |

*More notes added as the lit review grows.*

---

## How to read

**Online (recommended).** Click any link in the table above. Equations render automatically
via MathJax. Each equation has a **▶ Derive** button that expands a step-by-step derivation
inline — click it if you want to see where a result comes from.

**Offline.** Clone the repo and open any `.html` file in your browser. MathJax loads from a
CDN, so you need an internet connection for equations to render.

**PDF.** Compile the `.tex` source with `pdflatex` (requires a standard TeX Live or MiKTeX
installation). All packages used are part of the standard distribution.

---

## Topics covered so far

- Resolution vs. positional accuracy — why they are distinct concepts
- Single-emitter localization: the $\sigma \sim \sigma_\mathrm{PSF}/\sqrt{N}$ scaling and its derivation from Fisher information and the Cramér–Rao bound
- Two-emitter problem: error propagation, the Rayleigh curse, and why $I(d) \propto d^2$ as $d \to 0$
- $M$-emitter problem: density penalty ($\sqrt{M}$ factor), permutation explosion ($M!$ peaks), Fisher matrix ill-conditioning
- Unknown $M$: model order selection, the exponential photon threshold $N \sim (\sigma_\mathrm{PSF}/d_\mathrm{min})^{2M}$, and the combined scaling
- How superresolution (STED, GSD, spin-RESOLFT, STORM) rescues the problem via $M_\mathrm{eff} \leq 1$

---

## Literature referenced

A selection of key papers; full references are cited within each note.

- Rittweger et al., *Nat. Photon.* **3**, 144 (2009) — STED on NV centers, ~6 nm resolution
- Rittweger et al., *EPL* **86**, 14001 (2009) — GSD nanoscopy of NV centers
- Maurer et al., *Nat. Phys.* **6**, 912 (2010) — spin-RESOLFT
- Pfender et al., *PNAS* **111**, 14669 (2014) — single-spin STORM
- Jaskula et al., *Opt. Express* **25**, 11048 (2017) — spin-RESOLFT with 20 nm PSF
- Chen et al., *Light: Sci. Appl.* **4**, e230 (2015) — charge-state depletion (CSD)
- Gardill et al., *ACS Photonics* **9** (2022) — super-resolution Airy disk microscopy (SAM)
- Thompson, Larson & Webb, *Biophys. J.* **82**, 2775 (2002) — localization precision formula

---

## Contributing

This is primarily a personal reference, but if you are in the group and spot an error,
a missing reference, or want to add a note on a topic you have been working through,
open an issue or push a branch and we can discuss.

---

## License

Notes are shared for internal lab use. If you find them useful and want to adapt them
for a course or another group's reference, please reach out first.
