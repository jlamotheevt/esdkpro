# eSDK Pro — Doxygen Static Site (GitHub Pages)

This repo publishes the eSDK Pro Doxygen output to GitHub Pages, with:

- **Versioned path:** `/esdk-pro/v1.3.0/`
- **Stable path:** `/esdk-pro/latest/` (redirects to `annotated.html`)
- **Doxygen entry points exposed:** `annotated.html`, `index.html`, `classes.html`, `files.html`

## Quick start

1. Create an empty GitHub repository (e.g., `evtech/esdkpro-docsite`) and enable **GitHub Pages → Deploy from GitHub Actions**.
2. Download this repository ZIP, extract, and `git push` its contents to your repo's **main** branch.
3. The included workflow `.github/workflows/deploy.yml` will publish the `./site` directory to Pages.
4. Your Pages URL will look like `https://<org>.github.io/<repo>/`. The eSDK paths will be:
   - `https://<org>.github.io/<repo>/esdk-pro/v1.3.0/annotated.html`
   - `https://<org>.github.io/<repo>/esdk-pro/latest/annotated.html`

### Custom domain (optional)

If you want a clean subdomain like `api-docs.emergentvisiontec.com`:

- Add a **CNAME** record in DNS pointing your subdomain to `<org>.github.io`.
- In the repo settings → Pages, set the **Custom domain**. GitHub will create a `CNAME` file automatically.
- After DNS propagates, the same versioned and latest paths will be available under your subdomain.

## Updating to a new SDK version

1. Build Doxygen as usual and produce the standard `html` folder (or a ZIP containing it).
2. Replace the contents of `site/esdk-pro/v1.3.0/` with your new `html` output **in a new versioned folder**, e.g. `v1.3.1`.
3. Update `site/esdk-pro/latest/` if you want to change the default "latest" version:
   - This template uses a simple HTML redirect in `latest/index.html` to `annotated.html` within the **latest** folder itself.
   - To update, just copy over the desired `annotated.html` etc. into `latest/` again or leave `latest/` as thin redirectors that load from the latest folder (current template keeps `latest/annotated.html` as a self-redirect).

> Tip: Keep each version immutable; never overwrite prior versioned content.

## Optional: make `annotated.html` the visible root

We already redirect `latest/index.html` → `annotated.html`, so that `/latest/` lands on the Class List.

If your CDN supports rules, you can add an edge redirect from `/esdk-pro/latest/` to `/esdk-pro/latest/annotated.html` for even faster navigation.

## Webflow integration

On your Webflow **eSDK Pro API Reference** page, add a small "Reference hub":

```html
<ul class="reference-hub">
  <li><a href="https://YOUR-PAGES-DOMAIN/esdk-pro/latest/annotated.html" target="_blank" rel="noopener">Class list (Doxygen)</a></li>
  <li><a href="https://YOUR-PAGES-DOMAIN/esdk-pro/latest/classes.html" target="_blank" rel="noopener">Class index</a></li>
  <li><a href="https://YOUR-PAGES-DOMAIN/esdk-pro/latest/files.html" target="_blank" rel="noopener">File list</a></li>
  <li><a href="https://YOUR-PAGES-DOMAIN/esdk-pro/latest/index.html" target="_blank" rel="noopener">Full index</a></li>
</ul>
```

For deep links (examples):
- Pipeline: `https://YOUR-PAGES-DOMAIN/esdk-pro/latest/classeSdkPro_1_1Pipeline.html`
- Camera: `https://YOUR-PAGES-DOMAIN/esdk-pro/latest/classeSdkPro_1_1Camera.html`

## Allowing iframe embeds (optional)

If you plan to embed Doxygen inside Webflow with an `<iframe>`, ensure your host does **not** send `X-Frame-Options: DENY` and your `Content-Security-Policy` allows `frame-ancestors` for your Webflow domain.

Then use:

```html
<div style="position:relative;padding-top:56.25%">
  <iframe
    src="https://YOUR-PAGES-DOMAIN/esdk-pro/latest/annotated.html"
    style="position:absolute;inset:0;width:100%;height:100%;border:0"
    loading="lazy"></iframe>
</div>
```

## Notes

- The `.nojekyll` file prevents GitHub Pages from altering file paths that contain underscores—important for Doxygen.
- This repo is **static**; no build step is required for publishing pre-generated Doxygen HTML.
- If you want to fully automate pulling the latest Doxygen ZIP from releases, add a job step that uses the GitHub REST API or `gh` CLI to download the latest asset and unpack into `site/esdk-pro/<new-version>/` before uploading the Pages artifact.