# PhiSpy

PhiSpy looks for golden-ratio (φ ≈ 1.618) structure in photographs using classical
computer vision — candidate-point detection, pairwise distance-ratio matching, and
a random-baseline statistical test — and reports a z-score ("aesthetic index")
instead of a raw, easily-misleading match count.

This is a personal portfolio project, not a peer-reviewed research tool. See
[Limitations](#limitations--honesty) below before drawing any conclusions from it.

## What it does

1. Detects salient points in an image (Shi-Tomasi corner detection).
2. Computes all pairwise distances between those points and counts how many
   ratios land close to φ.
3. Repeats the same matching procedure on randomly scattered points (hundreds of
   times) to build a null distribution of "how many φ-matches would happen by
   chance alone" for an image with that many points.
4. Compares the real image's match count to that random baseline and reports a
   z-score: `(observed - baseline_mean) / baseline_std`. This is the "aesthetic
   index" — a measure of how far the image's φ-matches deviate from chance, not
   a measure of beauty.

## Project structure

```
PhiSpy/
├── notebooks/
│   └── phispy.ipynb        # main notebook — all the code and analysis
├── images/
│   ├── paintings/          # your own / public-domain images go here (gitignored)
│   ├── architecture/
│   ├── nature/
│   └── control/            # low-structure "sanity check" images (e.g. a scribble)
├── requirements.txt
└── .gitignore
```

Image files themselves are gitignored (folder structure is kept via `.gitkeep`
files) since not all source photos may be freely redistributable.

## Setup

```bash
# clone / cd into the repo, then:
python -m venv venv

# activate it
source venv/bin/activate        # macOS/Linux
venv\Scripts\activate           # Windows (cmd/PowerShell)

pip install -r requirements.txt

jupyter notebook notebooks/phispy.ipynb
```

Drop your own photos (or public-domain / openly-licensed images) into the
matching `images/<category>/` folder, then edit the file paths in the
notebook's testing sections to point at them.

## Limitations & honesty

This project is a fun, statistically-honest exploration — not proof that any
artwork, building, or landscape was designed around the golden ratio, and not a
measure of beauty or quality. Specifically:

- **It measures structure, not beauty.** A high aesthetic index means the
  image's detected points contain more φ-ratio distance pairs than a random
  scatter of the same number of points would — nothing more. Plenty of
  structured-but-unremarkable images (grids, fences, tiled floors) could score
  high; plenty of beautiful images could score low.
- **Parameter sensitivity.** Results depend on corner-detection thresholds, the
  number of points kept, the tolerance used to call two ratios "matching," and
  image resolution. Different reasonable parameter choices can shift the
  z-score meaningfully — always sanity-check with the control image.
- **Small sample sizes.** A handful of images per category is not enough to
  draw general conclusions about "paintings" or "architecture" as classes.
- **Point detection is not "what a human notices."** Shi-Tomasi corners are a
  proxy for salient structure, not human visual attention or compositional
  intent.
- **Correlation, not causation.** Even a genuinely high, reproducible z-score
  only shows unusual φ-ratio structure exists in the point layout — it says
  nothing about whether an artist or architect intended it, or whether that
  structure is why a viewer finds the image pleasing.

Treat the aesthetic index as a talking point for a portfolio piece and a
demonstration of statistically-grounded image analysis, not as a scientific
finding.

## Possible next steps

- Try alternative point detectors (Harris, ORB, SIFT keypoints) and compare.
- Extend beyond pairwise distance ratios to golden-ratio rectangles/regions.
- Bootstrap confidence intervals around the aesthetic index instead of a single
  z-score.
- Build a small labeled dataset and check whether the index correlates with
  anything measurable (e.g. human-rated composition scores) — while remaining
  careful about what such a correlation would and wouldn't demonstrate.
