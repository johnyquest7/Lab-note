# Lab notebook

A laboratory notebook that runs in the browser and stores every entry in a private GitHub repository. Quad-ruled pages, numbered and dated, signed with a timestamp, organised into one volume per project.

**Open it:** https://johnyquest7.github.io/Lab-note/

This repository holds only the app. Entries live in the private repository `johnyquest7/Lab-note-content-`, which the app reads and writes through the GitHub API with a token kept on each device.

## Setting up a device

1. Open the app and tap **Create a token on GitHub**. The link pre-fills a fine-grained token with Contents: Read and write. Under Repository access, choose **Only select repositories** and pick `Lab-note-content-`.
2. Paste the token into the app and connect.
3. On a phone, use **Add to Home Screen** (Safari share menu, or Chrome's install prompt) so it opens like an app and works offline.

Each device gets its own token, so one can be revoked without affecting the others.

## Writing

- **Pages**: free-form Markdown. Drafts are saved on the device as you type. **Save** commits to GitHub; **Sign & save** also records your name and the time. Editing a signed page records a dated amendment.
- **Links**: `[[42]]` links to page 42 in the same volume, `[[other-volume/3]]` to another volume. Each page lists the pages that refer to it.
- **Attachments**: images (paste or drag them in), CSV/TSV/JSON data, Jupyter notebooks, PDFs. Large photos are shrunk to 2400 px.
- **Charts**: a `chart` block draws an interactive chart from an attached CSV or rows written inline:

  ````
  ```chart
  type: line          # line, scatter, bar, histogram, box, violin, heatmap
  data: results.csv   # or put CSV rows after a --- line
  x: epoch
  y: train_auc, val_auc
  error: val_sd       # optional error bars
  group: model        # optional: one series per value
  title: AUC by epoch
  ```
  ````
- **Math**: `$inline$` and `$$display$$` (KaTeX). Code blocks are highlighted.
- **Jupyter**: `![[analysis.ipynb]]` on its own line shows code, output, tables and figures.
- **Search** (`/` or Ctrl/Cmd-K): full text, `#tags`, page numbers.
- **Print or PDF**: a page from its menu, or a whole volume (cover, contents and every page) from the volume menu.
- **Offline**: pages saved without a connection are kept on the device and committed when you're back online. If the same page was changed elsewhere, the app asks which version to keep or saves yours as a new page.

## How it works

Plain HTML, CSS and JavaScript modules, no build step. Libraries (marked, DOMPurify, KaTeX, highlight.js, Plotly, PapaParse) load from jsDelivr with pinned versions and integrity hashes. A service worker caches the app; pages and attachments are cached in IndexedDB.

Each save is one commit made through the Git Data API, so a page, its attachments and the volume's search index always change together. Pages pushed to the content repository from a computer are picked up on the next sync.

Look around without connecting anything: https://johnyquest7.github.io/Lab-note/?demo

A private GitHub repository is not a HIPAA-compliant store. Keep patient identifiers out of the notebook.
