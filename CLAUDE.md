# Task: Add super-resolution NV data to an existing table

## What we are trying to do

We are building a literature review table of super-resolution imaging techniques
applied to nitrogen-vacancy (NV) centers in diamond. Each row corresponds to one
paper. The table already exists in your codebase — your job is to add or fill in
the columns described below for each paper listed here.

The key quantity we are computing is the **positional accuracy estimate**, defined as:

```
σ_pos = FWHM / sqrt(N_photons)
```

where:
- `FWHM` is the reported resolution (full width at half maximum of the PSF) in nm
- `N_photons` is the **total photon count collected over one resolution element**,
  computed as:

```
N_photons = count_rate (cps) × dwell_time (s) × (FWHM / pixel_size)^2
```

The term `(FWHM / pixel_size)^2` gives the number of pixels that fit inside one
PSF-sized patch (the resolution element). This is the correct way to count photons
because all pixels within the resolution element contribute to localising the emitter.

Where the count rate is not reported in the paper, we assume **50,000 cps (50 kcps)**
as a typical value for a single NV centre under ~100 µW green excitation at NA 1.3–1.4.
This assumption is flagged in the notes column.

For **spin-contrast methods** (Maurer 2010, Huang 2020), the raw photon count must be
multiplied by the spin contrast fraction (~1–6%) to get the number of *contrast photons*,
because only those carry positional information. Using total photons for these papers
would overestimate the precision by ~10–30×.

For **localization/blinking methods** (Barbiero 2017, Gu 2013), N_photons is summed
over all useful frames (frames where exactly one emitter is on), not just one frame.

The figure of merit `σ_pos` is a **shot-noise floor** on positional accuracy. In
practice, the achieved precision is almost always worse than this floor due to:
- mechanical drift of the piezo stage
- residual intensity at the doughnut/Airy zero
- limited spin contrast
- blinking statistics

---

## Columns to add (or fill in) for each paper

| Column name            | Description |
|------------------------|-------------|
| `figure`               | Which figure in the paper was used for extraction |
| `resolution_nm`        | Reported PSF FWHM in nm (or two-point separation if PSF not given) |
| `pixel_size_nm`        | Pixel size in nm (stated or derived from scale bar) |
| `scan_size_nm`         | Scan field of view in nm × nm |
| `scan_size_px`         | Scan size in pixels × pixels |
| `dwell_time_ms`        | Per-pixel dwell time in ms |
| `total_scan_time_s`    | Total scan time in seconds (= scan_size_px^2 × dwell_time_ms / 1000) |
| `count_rate_kcps`      | Photon count rate in kcps (stated or assumed 50) |
| `count_rate_assumed`   | Boolean: true if 50 kcps was assumed, false if stated |
| `px_per_res_element`   | (FWHM / pixel_size)^2, rounded to nearest integer |
| `photons_per_res_element` | count_rate × dwell_time × px_per_res_element |
| `sigma_pos_nm`         | FWHM / sqrt(photons_per_res_element), in nm |
| `notes`                | Flags, caveats, derivations |

---

## Data for each paper

### 1. Tzeng 2011 — Angew. Chem. Int. Ed. 50, 2262

- **figure**: Fig. 4c (cluster of up to 5 FND particles resolved from one confocal spot)
- **resolution_nm**: 40
- **pixel_size_nm**: 20 (derived from scale bar)
- **scan_size_nm**: 400 × 400
- **scan_size_px**: 20 × 20
- **dwell_time_ms**: NOT STATED — cannot compute σ_pos
- **total_scan_time_s**: not derivable
- **count_rate_kcps**: 50 (assumed)
- **count_rate_assumed**: true
- **px_per_res_element**: (40/20)^2 = 4
- **photons_per_res_element**: not estimable (dwell unknown)
- **sigma_pos_nm**: not estimable
- **notes**: Dwell time not stated anywhere in paper. Pixel size derived from scale bar
  divided by visible pixel count. Fig. 4c shows bare FNDs with up to 5 particles in
  one confocal spot — this is the flagship multi-emitter figure. Cannot compute σ_pos
  without dwell time.

---

### 2. Yang 2014 — RSC Adv. 4, 11305

- **figure**: Fig. 3b (STED image of bulk NV dot arrays; multiple ensemble spots per
  diffraction-limited region)
- **resolution_nm**: 110 (best pattern, Fig. 3g)
- **pixel_size_nm**: 20 (stated in Fig. 3 caption)
- **scan_size_nm**: 3000 × 3000
- **scan_size_px**: 150 × 150
- **dwell_time_ms**: 0.5 (stated in Fig. 3 caption)
- **total_scan_time_s**: 150 × 150 × 0.0005 = 11.25 s
- **count_rate_kcps**: 50 (assumed)
- **count_rate_assumed**: true
- **px_per_res_element**: (110/20)^2 = 30
- **photons_per_res_element**: 50000 × 0.0005 × 30 = 750
- **sigma_pos_nm**: 110 / sqrt(750) = 4.0 nm
- **notes**: NV ensemble spots — FWHM includes the physical extent of the implanted
  cluster, not a pure single-NV PSF. Saturation intensity labelled as "10.8 mW" in
  Fig. 1c — units appear wrong, should be MW/cm². The 110 nm FWHM is for the best
  (highest dose) pattern; other patterns give 130–180 nm.

---

### 3. Barbiero 2017 — Light: Sci. Appl. 6, e17085

- **figure**: Fig. 4d (two collectively blinking NV centres resolved to 23 nm
  separation by spin manipulation at ODMR frequency f1)
- **resolution_nm**: 52 (FWHM of unresolved collective blinking spot); demonstrated
  separation = 23 nm
- **pixel_size_nm**: 12 (estimated from 100 nm scale bar / ~8 visible pixels)
- **scan_size_nm**: 200 × 200
- **scan_size_px**: 17 × 17
- **dwell_time_ms**: 30 (EMCCD frame exposure; stated)
- **total_scan_time_s**: >200 selected frames × 0.030 s ≈ 6 s active acquisition
- **count_rate_kcps**: 50 (assumed; wide-field camera, likely lower ~10–20 kcps per pixel)
- **count_rate_assumed**: true
- **px_per_res_element**: (52/12)^2 = 19
- **photons_per_res_element**: This is a localization method — photons accumulate over
  all useful frames. Using 300 ph/px/frame (realistic for blinking ND in wide-field)
  × 19 px × 200 frames = 1,140,000 ph total
- **sigma_pos_nm**: 52 / sqrt(1140000) = 0.05 nm
- **notes**: Shot-noise floor is sub-Å; practical limit is drift and blinking statistics
  (23 nm demonstrated separation). Paper conflates reconstructed-image FWHM (34–52 nm)
  with resolution — these are histogram widths, not single-event σ. Excitation power
  not stated. Wide-field geometry: 300 ph/px/frame is an estimate, not stated.

---

### 4. Maurer 2010 — Nat. Phys. 6, 912

- **figure**: Fig. 3b (2D spin-RESOLFT image resolving two NV centres ~150 nm apart)
- **resolution_nm**: 50 (at t_D = 7.5 µs, 2 mW, from 1D scans in Fig. 2a)
- **pixel_size_nm**: 8 (derived: ~400 nm FOV / 50 px)
- **scan_size_nm**: ~400 × 400
- **scan_size_px**: 50 × 50 (stated in Fig. 3 caption)
- **dwell_time_ms**: 7200 (7.2 s per pixel; stated in Fig. 3 caption)
- **total_scan_time_s**: 50 × 50 × 7.2 = 18,000 s (5 hours)
- **count_rate_kcps**: 50 (assumed)
- **count_rate_assumed**: true
- **px_per_res_element**: (50/8)^2 = 39
- **photons_per_res_element**: SPIN CONTRAST METHOD.
  Total photons: 50000 × 7.2 × 39 = 14,040,000
  Contrast photons (spin signal ~3×10^-3 of total): 14,040,000 × 0.003 = 42,120
  USE CONTRAST PHOTONS ONLY.
- **sigma_pos_nm**: 50 / sqrt(42120) = 0.24 nm (using contrast photons)
- **notes**: SPIN-CONTRAST METHOD — must use contrast photons (~0.3% of total), not
  total photons. Using total photons gives a misleading 0.013 nm floor. Practical limit
  is doughnut zero quality and mechanical stability, not photon shot noise. Pixel size
  derived from FOV/pixel count. The 5-hour acquisition for a 50×50 pixel image reflects
  the long spin-state lifetime required per pixel. NV implant depth ~10 nm (6 keV 15N).

---

### 5. Gu 2013 — Opt. Express 21, 17639

- **figure**: Fig. 5 (super-resolution reconstruction of two NV centres at 20 nm
  apparent separation within a single nanodiamond)
- **resolution_nm**: 20 (two-point separation; σ_loc = 12 nm stated by authors)
- **pixel_size_nm**: 7 (camera pixel mapped to sample plane; not stated)
- **scan_size_nm**: wide-field 25,000 × 25,000 (full FOV); zoomed reconstruction ~300 × 300
- **scan_size_px**: wide-field camera (no scanning); 2000 frames total
- **dwell_time_ms**: 30 (EMCCD frame exposure; stated)
- **total_scan_time_s**: 2000 × 0.030 = 60 s
- **count_rate_kcps**: 50 (assumed; ~400 ph/frame/px estimated from Fig. 4a)
- **count_rate_assumed**: true (estimated from irradiance-vs-photons graph)
- **px_per_res_element**: (20/7)^2 = 8
- **photons_per_res_element**: Localization method — sum over all frames, on-fraction ~50%.
  400 ph/px/frame × 8 px × 2000 frames × 0.5 (on-fraction) = 3,200,000 ph
- **sigma_pos_nm**: 20 / sqrt(3200000) = 0.011 nm
- **notes**: Shot-noise floor is ~0.01 nm; achieved precision is 12 nm — entirely
  drift-limited (authors state this). The 20 nm apparent separation may correspond to
  only ~3.5 nm true physical NV-NV separation due to the nanodiamond acting as a solid
  immersion lens (refractive index n~2.4, magnification ~n^2 ≈ 5.8×). This is discussed
  by the authors but the headline "20 nm" number is the optical apparent value.

---

### 6. Huang 2020 — Phys. Rev. A 102, 040601(R)

- **figure**: Fig. 4 (confocal subtraction images resolving two NV centres at 266 nm
  separation after quantum-phase MFG photoswitching)
- **resolution_nm**: 266 (two-point separation; diffraction limit = 436 nm at NA=0.7)
- **pixel_size_nm**: 20 (derived: 1000 nm / 50 px)
- **scan_size_nm**: 1000 × 1000
- **scan_size_px**: 50 × 50 (stated)
- **dwell_time_ms**: ~14400 (~14.4 s/pixel; derived: 10 h / 2 images / 2500 px)
- **total_scan_time_s**: ~36,000 s (~10 h; stated)
- **count_rate_kcps**: 50 (assumed; optical readout window ~5 µs per average)
- **count_rate_assumed**: true
- **px_per_res_element**: (266/20)^2 = 177
- **photons_per_res_element**: SPIN CONTRAST METHOD.
  Optical photons: 50000 × (2×10^5 averages × 5×10^-6 s readout) × 177 px = 8,850
  Contrast photons (~6%): 8850 × 0.06 = 531
  USE CONTRAST PHOTONS.
- **sigma_pos_nm**: 266 / sqrt(531) = 11.5 nm (contrast photons)
- **notes**: SPIN-CONTRAST METHOD. The stated σ_loc = 0.8–1.4 nm is a Gaussian fit
  errorbar on the centroid of the averaged 50×50 confocal map — this is not a
  single-event localization precision and is not comparable to FWHM/sqrt(N).
  The claimed 0.15 nm "resolution" is purely theoretical (MFG calculation) and is
  never demonstrated in an image. NA = 0.7 is unusually low for super-resolution work.
  Total acquisition ~10 h for a 50×50 pixel image.

---

### 7. Wildanger 2011 — PRL 107, 017601

- **figure**: Fig. 2c (STED image resolving 5 NV centres from a single confocal spot)
- **resolution_nm**: 50
- **pixel_size_nm**: 7 (derived from 100 nm scale bar / ~15 visible pixels)
- **scan_size_nm**: ~500 × 500
- **scan_size_px**: ~70 × 70
- **dwell_time_ms**: 1 (stated in Fig. 2 caption)
- **total_scan_time_s**: 70 × 70 × 0.001 = 4.9 s
- **count_rate_kcps**: 50 (assumed; deep NV at 1–5 µm likely 10–30 kcps)
- **count_rate_assumed**: true
- **px_per_res_element**: (50/7)^2 = 51
- **photons_per_res_element**: 50000 × 0.001 × 51 = 2550 (optimistic)
  Conservative (20 kcps): 20000 × 0.001 × 51 = 1020
- **sigma_pos_nm**: 50 / sqrt(2550) = 1.0 nm (optimistic); 50 / sqrt(1020) = 1.6 nm (conservative)
- **notes**: Objective NA not stated in paper — unusual omission. PSF FWHM (~50 nm) is
  semi-empirical (derived from STED resolution formula I/I_s), not from a direct
  Gaussian fit to the image. Deep NV (1–5 µm below surface) gives lower count rate
  than surface NV. Pulsed STED at 80 MHz with time-gated detection.

---

### 8. Storterboom 2021 — Nanoscale Res. Lett. 16, 44

- **figure**: Fig. 4b (GSD super-resolved image of two NV centres, 72 nm centre-to-centre)
- **resolution_nm**: 37 (average of 36 and 38 nm from two NV centres, y-direction)
- **pixel_size_nm**: 1 (stated in Fig. 4 caption)
- **scan_size_nm**: 300 × 300
- **scan_size_px**: 300 × 300 (stated in Fig. 4 caption)
- **dwell_time_ms**: 24 (stated in Fig. 4 caption)
- **total_scan_time_s**: 300 × 300 × 0.024 = 2160 s (36 min)
- **count_rate_kcps**: 20 (estimated from pulse sequence analysis:
  cycle = 300 µs depletion + 20 µs probe + 20 µs reset = 340 µs per rep,
  1000 reps = 340 ms, probe fraction = 20/340,
  at 20 kcps: 20000 × (20/340) × 1000 × 0.001 s = ~1200 ph/px effective)
- **count_rate_assumed**: true (estimated from pulse sequence timing)
- **px_per_res_element**: (37/1)^2 = 1369
- **photons_per_res_element**: 1200 × 1369 = 1,642,800
  (using 400 ph/px from pulse-sequence estimate × 1369 px)
- **sigma_pos_nm**: 37 / sqrt(1642800) = 0.029 nm
- **notes**: Best-characterised scan in this batch — pixel size, dwell time, scan size
  all stated. Shot-noise floor is ~0.03 nm; practical limit is doughnut quality and
  drift. GSD mechanism confirmed as metastable-state shelving (not charge-state
  conversion) based on high nitrogen concentration (~500 ppm) in HPHT NDs. First
  demonstration of GSD nanoscopy on NV in nanodiamonds (not bulk).

---

### 9. Arroyo-Camejo 2013 — ACS Nano 7, 10912

- **figure**: Fig. 3a (five NV centres resolved in ~100 nm nanodiamond);
  Fig. 4 (two NV centres at 16.7 nm separation)
- **resolution_nm**: 10 (routinely 10–20 nm; 9.5 nm for headline single-NV result)
- **pixel_size_nm**: 5 (estimated from ~40 nm scale bar / ~8 visible pixels; not stated)
- **scan_size_nm**: ~150 × 150
- **scan_size_px**: ~30 × 30
- **dwell_time_ms**: NOT STATED — critical missing parameter
- **total_scan_time_s**: not derivable
- **count_rate_kcps**: 100–300 (NV in NDs under pulsed STED at 60 µW, 8 MHz;
  assumed 200 kcps as midpoint)
- **count_rate_assumed**: true
- **px_per_res_element**: (10/5)^2 = 4
- **photons_per_res_element**: If dwell = 2 ms: 200000 × 0.002 × 4 = 1600 ph
  If dwell = 5 ms: 200000 × 0.005 × 4 = 4000 ph
- **sigma_pos_nm**: 10 / sqrt(1600) = 0.25 nm (2 ms); 10 / sqrt(4000) = 0.16 nm (5 ms)
- **notes**: CRITICAL MISSING PARAMETER — dwell time not stated anywhere in the paper.
  This is the best-resolution paper in the set (9.5 nm PSF FWHM; 16.7 nm two-NV
  separation) but has the least acquisition metadata. Objective NA also not stated.
  Only 4 pixels per resolution element because PSF is only 2 pixels wide — tight
  photon budget. NDs on quartz coverslip with customised labelled matrix for SEM
  co-registration.

---

### 10. Gardill 2022 — ACS Photonics 9, 3848

- **figure**: Fig. 3a (SAM measurement of NV_A and NV_B pair at 115 nm separation)
- **resolution_nm**: 16.9 (n2 SAM ring FWHM at τ = 10 ms; stated with ±0.8 nm uncertainty)
- **pixel_size_nm**: 10 (stated as "order of 10 nm"; exact value in SI Table S2)
- **scan_size_nm**: ~1000 × 1000
- **scan_size_px**: ~100 × 100
- **dwell_time_ms**: 50 (dominated by 50 ms charge-state readout; stated)
- **total_scan_time_s**: 100 × 100 × 0.050 = 500 s (8 min)
- **count_rate_kcps**: 10 (10 µW at 589 nm → ~10 kcps for NV- charge-state readout)
- **count_rate_assumed**: true (estimated from readout power)
- **px_per_res_element**: (16.9/10)^2 = 3 (rounded from 2.85)
- **photons_per_res_element**: 10000 × 0.050 × 3 = 1500 ph
- **sigma_pos_nm**: 16.9 / sqrt(1500) = 0.44 nm
- **notes**: Shot-noise floor ~0.44 nm; practical limit is piezo positioning repeatability
  ΔR = ±3 nm (authors state this explicitly and extract it from model fit — good practice).
  SAM technique uses Airy disk nodes as intensity zeros; no phase mask required.
  Depletion is via charge-state conversion (NV- → NV0) using 638 nm, 20 mW, τ = 10 ms.
  PSF fit uses quartic Gaussian η = A·exp(-x^4/σ^4), not standard Gaussian.
  The residual intensity at the n2 Airy node is ε = 0.044% of I0, comparable to or
  better than typical STED doughnut zeros (0.1–1.5%).

---

## Summary of σ_pos estimates

| Paper                  | FWHM (nm) | Pixel (nm) | Dwell (ms) | px/res. el. | N_photons    | σ_pos (nm)       |
|------------------------|-----------|------------|------------|-------------|--------------|------------------|
| Tzeng 2011             | 40        | 20         | unknown    | 4           | unknown      | not estimable    |
| Yang 2014              | 110       | 20         | 0.5        | 30          | 750          | 4.0              |
| Barbiero 2017          | 52        | 12         | 30/frame   | 19          | 1,140,000    | 0.05             |
| Maurer 2010†           | 50        | 8          | 7200       | 39          | 42,120†      | 0.24†            |
| Gu 2013                | 20        | 7          | 30/frame   | 8           | 3,200,000    | 0.011            |
| Huang 2020†            | 266       | 20         | ~14400     | 177         | 531†         | 11.5†            |
| Wildanger 2011         | 50        | 7          | 1          | 51          | 2,550        | 1.0              |
| Storterboom 2021       | 37        | 1          | 24         | 1369        | 1,642,800    | 0.029            |
| Arroyo-Camejo 2013     | 10        | 5          | unknown    | 4           | ~1600–4000   | 0.16–0.25        |
| Gardill 2022           | 16.9      | 10         | 50         | 3           | 1,500        | 0.44             |

† Contrast photons used (spin-contrast method). Total photons would give misleading sub-pm values.

**Important**: σ_pos is the shot-noise floor. In every paper, the practical achieved
precision is substantially worse, limited by drift, doughnut/Airy zero quality, spin
contrast, or blinking statistics. The floor tells you what would be achievable with
perfect optics and no drift — useful for comparing the information content of different
experimental designs, but not the achieved performance.
