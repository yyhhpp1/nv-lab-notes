# nv-lab-notes

Lab notes on NV center physics: superresolution, localization theory, and quantum sensing.
A living reference for group members, written to be derived from first principles.

**Live site:** https://yyhhpp1.github.io/nv-lab-notes/

---

## What this is

Running collection of self-contained writeups started during a literature review on
superresolution techniques for NV centers. Each note includes full step-by-step derivations
pitched at a first-year grad student in quantum sensing — solid physics background assumed,
familiarity with NV centers not required.

Notes are written in HTML (rendered with MathJax) so they can be read directly in a browser
with no software installation. Each equation has a **▶ Derive** button that expands the
full derivation inline.

---

## Notes

| # | Title | Topics |
|---|-------|--------|
| [01](NV_sigma_scaling.html) | Positional Accuracy of NV Centers: σ Scaling | Fisher information, Cramér–Rao bound, single-emitter localization, Rayleigh curse, M-emitter combinatorial catastrophe, superresolution rescue |
| [02](NV_misalignment.html) | Magnetic Field Misalignment in NV Center Experiments | NV Hamiltonian, native vs. eigenbasis, initialization fidelity, π-pulse errors, PL measurement operator, level anti-crossing |

---

## Reference tables

| Title | Contents |
|-------|----------|
| [Superresolution Experiments on NV Centers](superresolution_table.html) | Side-by-side comparison of STED, GSD, CSD, STORM, spin-RESOLFT, and quantum-phase experiments: PSF FWHM, localization precision, photon budget, dwell time, scan parameters, and σ_pos estimates. Filterable by technique. |

---

## Topics covered

**Localization theory**
- Resolution vs. positional accuracy — why they are distinct
- Single-emitter localization: σ ~ σ_PSF / √N from Fisher information and the Cramér–Rao bound
- Two-emitter problem: error propagation, the Rayleigh curse, I(d) ∝ d² as d → 0
- M-emitter problem: density penalty (√M factor), permutation explosion (M! peaks), Fisher matrix ill-conditioning
- Unknown M: model order selection, exponential photon threshold N ~ (σ_PSF / d_min)^{2M}, combined scaling
- How superresolution (STED, GSD, spin-RESOLFT, STORM) rescues the problem via M_eff ≤ 1

**NV spin physics**
- NV Hamiltonian and native basis (zero-field splitting, Zeeman term)
- Effect of a misaligned magnetic field: first-order perturbation theory, mixed eigenstates
- Green initialization: ISC mechanism, why it always addresses the native basis
- π-pulse errors under misalignment: leakage into the dark state
- Ramsey tomography: Bloch sphere evolution in aligned vs. misaligned cases
- Why readout is native-basis-locked (crystal symmetry, C₃ᵥ selection rules)
- Why MW pulses address the eigenbasis (rotating wave approximation, ODMR calibration)
- Full PL measurement operator: first-order cross-contamination terms
- General basis transformation, level anti-crossing at ~102 mT

---

## How to read

**Online.** Click any link in the tables above. Equations render via MathJax CDN (internet
connection required). Click any **▶ Derive** button to expand the step-by-step derivation.

**Offline.** Clone the repo and open any `.html` file in your browser. MathJax loads from a
CDN so equations require an internet connection; everything else works offline.

---

## Repository structure

```
nv-lab-notes/
├── index.html                  — homepage
├── NV_sigma_scaling.html       — Note 01
├── NV_misalignment.html        — Note 02
├── superresolution_table.html  — reference table
├── STYLE_GUIDE.md              — CSS/HTML conventions for new notes
└── README.md
```

See [STYLE_GUIDE.md](STYLE_GUIDE.md) for the CSS variable system, component patterns
(key-result boxes, callouts, derivation toggles, TOC), and a checklist for adding new notes.

---

## Literature referenced

Full citations appear inside each note. Key papers:

**Localization theory**
- Thompson, Larson & Webb, *Biophys. J.* **82**, 2775 (2002) — localization precision formula
- Tsang, Nair & Lu, *Phys. Rev. X* **6**, 031033 (2016) — Rayleigh curse (SPADE)

**NV superresolution**
- Rittweger et al., *Nat. Photon.* **3**, 144 (2009) — STED on NV, ~6 nm
- Rittweger et al., *EPL* **86**, 14001 (2009) — GSD nanoscopy of NV
- Maurer et al., *Nat. Phys.* **6**, 912 (2010) — spin-RESOLFT
- Wildanger et al., *PRL* **107**, 017601 (2011) — STED resolving 5 NV from one confocal spot
- Arroyo-Camejo et al., *ACS Nano* **7**, 10912 (2013) — STED, 9.5 nm PSF
- Barbiero et al., *Light: Sci. Appl.* **6**, e17085 (2017) — spin-manipulation SMLM
- Storteboom et al., *Nanoscale Res. Lett.* **16**, 44 (2021) — GSD on nanodiamonds
- Gardill et al., *ACS Photonics* **9**, 3848 (2022) — super-resolution Airy disk microscopy (SAM)

**NV spin physics**
- Doherty et al., *Phys. Rep.* **528**, 1 (2013) — NV center review
- Rondin et al., *Rep. Prog. Phys.* **77**, 056503 (2014) — magnetometry review

---

## Contributing

Personal reference made shareable. If you are in the group and spot an error, open an issue
or push a branch. For style and formatting conventions, read STYLE_GUIDE.md first.

---

## License

Shared for internal lab use. If you want to adapt this material for a course or another
group's reference, please reach out first.
