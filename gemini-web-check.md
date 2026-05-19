# Website Performance Evaluation

Based on the analysis of your DigiPols website and codebase, I have identified two primary issues causing the slow loading times on the `publications.qmd` and `team.qmd` pages compared to the rest of the site.

## 1. The `polyfill.io` Dependency (Critical Issue)

**The Problem:**
Pages with Quarto listings (`publications.qmd` and `team.qmd`) are automatically injecting two external scripts that are not present on your other pages:
1. `https://polyfill.io/v3/polyfill.min.js?features=es6`
2. `https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml-full.js`

The domain `polyfill.io` was recently acquired by a malicious actor and has been sinkholed or blocked by most major browsers, ISPs, and ad-blockers globally. When a user visits your `team` or `publications` page, their browser attempts to download this script, which hangs until the request eventually times out. This is why those specific pages appear to be loading infinitely, while `index` and `research` load instantly.

Quarto injects these scripts by default on listing pages to support potential LaTeX/Math equations in the listings.

**The Solution:**
Since you do not appear to be using complex mathematical equations on your site, you can safely disable MathJax (which will also remove the problematic `polyfill.io` script).

Add the following to your `_quarto.yml` file under the `html` format options:

```yaml
format:
  html:
    theme: [cosmo, digipols.scss]
    css: styles.css
    math: false # <-- Add this line
```

Alternatively, you can add `math: false` to the YAML frontmatter of `publications.qmd` and `team.qmd` directly.

## 2. Unoptimized Image Sizes (Performance Issue)

**The Problem:**
Quarto listings (especially in a grid layout) load all images simultaneously when the page is visited. During the analysis of your `docs/team/images_team/` directory, I found that one of the images is extremely large:
* `ioannides.png` is **5.6 MB**.

For context, most web avatar images should be well under 100 KB. Loading a 5.6 MB image will severely impact the visual load time and consume a lot of bandwidth, especially on mobile devices.

Other images are relatively fine but could also be optimized (e.g., `sergidou.png` is 272 KB, and `home1.png` is 282 KB).

**The Solution:**
Resize and compress the `ioannides.png` image before rendering the website. 
* Resize it to a maximum width/height of around 400x400 pixels.
* Export it as a compressed JPEG, PNG, or WebP. 

Once both of these issues are resolved, the `team` and `publications` pages will load just as quickly as the rest of your website!
