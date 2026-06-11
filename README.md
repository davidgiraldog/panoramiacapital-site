# panoramiacapital.com — wrapper site

Static page served by GitHub Pages on `panoramiacapital.com`. It embeds the
Panoramia Capital Streamlit app full-screen via an iframe
(`?embed=true`, the official Streamlit Community Cloud embed mode), so the
browser URL stays on our domain.

## One-time setup

### 1. Point the iframe at the app
In `index.html`, replace `REPLACE-WITH-YOUR-SUBDOMAIN.streamlit.app` with the
app's real URL (keep the `/?embed=true` suffix).

### 2. Create the GitHub repo and enable Pages
```
gh repo create panoramiacapital-site --public --source . --push
```
Then on GitHub: **Settings → Pages → Source: Deploy from a branch →
Branch: main / (root) → Save**.

> GitHub Pages on the free plan requires a **public** repo. The only thing
> exposed is this wrapper HTML (the streamlit.app URL is public anyway).

### 3. DNS records at the domain registrar
For the apex domain `panoramiacapital.com` (GitHub Pages official IPs):

| Type  | Host | Value               |
|-------|------|---------------------|
| A     | @    | 185.199.108.153     |
| A     | @    | 185.199.109.153     |
| A     | @    | 185.199.110.153     |
| A     | @    | 185.199.111.153     |
| CNAME | www  | davidgiraldog.github.io |

Remove/replace any existing A or URL-forwarding records on `@`.

### 4. Bind the domain + HTTPS
On GitHub: **Settings → Pages → Custom domain → `panoramiacapital.com` →
Save**, wait for the DNS check (minutes to ~1 h), then tick
**Enforce HTTPS** once the certificate is issued.

The `CNAME` file in this repo keeps the domain binding across deploys —
don't delete it.

## Notes / limitations
- Browser URL never changes (single iframe): no deep links into tabs.
- `?embed=true` hides the Streamlit toolbar and badge — intended.
- App login (`local_auth`) lives in Streamlit session state, so it works
  inside the iframe; a page reload asks for login again, same as today.
- If Safari/iOS ever misbehaves (third-party cookie blocking can affect
  file uploads inside iframes), test there first before debugging the app.
