# DigiPols — Cluster art (Option A) implementation

Adds a small accent-coloured signal glyph to each of the four cluster cards on `index.qmd`, using the per-cluster colours already defined in `digipols.scss` (`$c1–$c4`). Pure SVG, no asset pipeline.

---

## 1 · Drop the SVGs into the site

Copy the four files from the `images/` folder of this bundle into the Quarto project:

```
quarto/images/clusters/
├── cluster-01-applied.svg     # blue (#2E6FE0) — flow / nodes
├── cluster-02-monitoring.svg  # red  (#D85A3C) — sampled waveform
├── cluster-03-opinion.svg     # green (#4F8A4A) — scatter + trend
└── cluster-04-forum.svg       # purple (#8C5BD2) — convergence rings
```

Create the `clusters/` subfolder if it doesn't exist.

---

## 2 · Edit `quarto/index.qmd`

Replace the existing `<div class="cluster-grid"> … </div>` block (currently around lines 80–105 of `index.qmd`, immediately after `<h2 …>Four clusters, one forum.</h2>`) with the following. The change is: each `<a class="cluster">` now begins with an `<img class="cluster-art" …>` element; **nothing else changes**.

```html
<div class="cluster-grid">
  <a class="cluster" href="research.html#applied-political-communication">
    <img class="cluster-art" src="images/clusters/cluster-01-applied.svg" alt="" aria-hidden="true" width="64" height="64" />
    <span class="cluster-code">cluster.01</span>
    <h3 class="cluster-title">Applied Political Communication</h3>
    <p class="cluster-blurb">Experimental work on how political information moves between political actors, media and citizens — including pioneering research on Voting Advice Applications.</p>
    <span class="cluster-cta">explore →</span>
  </a>
  <a class="cluster" href="research.html#media-and-social-media-monitoring">
    <img class="cluster-art" src="images/clusters/cluster-02-monitoring.svg" alt="" aria-hidden="true" width="64" height="64" />
    <span class="cluster-code">cluster.02</span>
    <h3 class="cluster-title">Media &amp; Social Media Monitoring</h3>
    <p class="cluster-blurb">Bespoke data-collection platforms that track political communication across news media and social media in real time, during election campaigns and beyond.</p>
    <span class="cluster-cta">explore →</span>
  </a>
  <a class="cluster" href="research.html#public-opinion-argument-mining">
    <img class="cluster-art" src="images/clusters/cluster-03-opinion.svg" alt="" aria-hidden="true" width="64" height="64" />
    <span class="cluster-code">cluster.03</span>
    <h3 class="cluster-title">Public Opinion &amp; Argument Mining</h3>
    <p class="cluster-blurb">Techniques and tools for the analysis of public opinion, combining surveys with computational methods and argument mining from computational linguistics.</p>
    <span class="cluster-cta">explore →</span>
  </a>
  <a class="cluster" href="research.html#digipols-forum">
    <img class="cluster-art" src="images/clusters/cluster-04-forum.svg" alt="" aria-hidden="true" width="64" height="64" />
    <span class="cluster-code">cluster.04</span>
    <h3 class="cluster-title">The DigiPols Forum</h3>
    <p class="cluster-blurb">Umbrella organisation that convenes DigiPols researchers to exchange ideas, share state-of-the-art practice and set strategic direction for the lab.</p>
    <span class="cluster-cta">explore →</span>
  </a>
</div>
```

---

## 3 · Edit `quarto/digipols.scss`

Inside the existing `.cluster-grid { … .cluster { … } }` block, add one new rule for `.cluster-art`. The cleanest place is right after the `&:hover { background: $paper-2; }` line and before the `.cluster-code { … }` rule.

**Find:**

```scss
    &:hover { background: $paper-2; }

    .cluster-code {
```

**Replace with:**

```scss
    &:hover { background: $paper-2; }

    .cluster-art {
      width: 56px;
      height: 56px;
      display: block;
      margin: 0 0 0.4rem;
      // tiny lift on hover, matches the cta arrow's energy
      transition: transform 160ms ease;
    }
    &:hover .cluster-art { transform: translateY(-1px); }

    .cluster-code {
```

That's the only SCSS change required. The existing `flex-direction: column; gap: 0.9rem;` on `.cluster` already stacks the image above the code label correctly.

---

## 4 · Rebuild & verify

```bash
quarto render
```

Then open the homepage and confirm:

- [ ] Each of the four cluster cards shows its glyph above the `cluster.NN` label.
- [ ] Glyph colours match the cluster accent: **blue / red / green / purple** in order.
- [ ] Hover on a card still shows the existing paper‑2 background swap; the glyph nudges up 1px.
- [ ] On narrow screens (single column at ≤768px), nothing breaks — glyphs stay 56×56 and left‑aligned.
- [ ] No layout shift: the SVGs declare `width`/`height` attributes so they reserve space pre‑load.

---

## 5 · Optional: also use on `research.qmd`

If you want the same glyphs on the long-form cluster sections in `research.qmd`, place an `<img>` inside each `.research-cluster .rc-code` block (same file paths). No additional SCSS needed — `.rc-code` already pads correctly. Tell me if you want me to spec this too.

---

## File manifest

```
INSTRUCTIONS.md                         ← this file
images/cluster-01-applied.svg
images/cluster-02-monitoring.svg
images/cluster-03-opinion.svg
images/cluster-04-forum.svg
```

All SVGs are 96×96 viewBox, no external font references, no scripts — safe to inline or serve as static files.
