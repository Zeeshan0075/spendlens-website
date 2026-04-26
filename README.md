# SpendLens Website

A 4-page static website for SpendLens, modeled after the SubWise pattern.

## Live links

- **App Store:** https://apps.apple.com/us/app/spendlens-track-money/id6762673414
- **Apple ID:** `6762673414`
- **Bundle ID:** `com.spendlens.app`

All "Download on App Store" CTAs and footer "App Store" links across the site point to the live App Store listing.

## Pages

| File | Purpose | Linked from |
|------|---------|-------------|
| `index.html` | Marketing landing page | Public, App Store, social |
| `privacy.html` | Privacy policy | **Settings → Privacy Policy** in app + App Store Connect |
| `terms.html` | Terms & Conditions | **Settings → Terms of Use** in app + App Store Connect |
| `support.html` | FAQ + contact | Settings → Help, App Store Connect support URL |

## Design choices

- **Landing (`index.html`):** Dark theme matching the SpendLens app aesthetic. Teal-to-blue gradient accents (`#4ECDC4` → `#45B7D1`). Hero with phone mockup, feature grid, privacy block, pricing card.
- **Legal pages (`privacy.html`, `terms.html`, `support.html`):** Light theme for readability of long-form text. Same brand identity in nav and footer.
- **Logo:** Sharp-edged gradient square with magnifying-lens motif (circle + diagonal line) — matches the SpendLens "lens" name and the icon system.
- **Email:** All pages reference `businessforzeeshan@gmail.com` for support and contact. Update if you choose a different inbox.
- **Jurisdiction:** Terms governed by UAE law, Abu Dhabi courts (matches SubWise pattern, since you're based in Abu Dhabi).

## How to deploy

Pick whichever host you used for SubWise — same flow.

### Option A: Cloudflare Pages (recommended)
1. Create a new Cloudflare Pages project
2. Upload the 4 HTML files (drag & drop, or via Git)
3. Buy `spendlens.app` (or whatever domain you want) and point it at the project
4. The contact email (`businessforzeeshan@gmail.com`) is already a real Gmail inbox — no forwarding setup needed

### Option B: GitHub Pages
1. Create a public repo `spendlens-web` (or use the same repo as the app's docs)
2. Drop the 4 HTML files at the root
3. Enable Pages from Settings → Pages → Deploy from main branch
4. Add a custom domain via Settings → Pages → Custom domain
5. Add an empty `CNAME` file with your domain to the repo

### Option C: Netlify
1. Drag the entire `spendlens-website/` folder onto netlify.com/drop
2. You'll get a `*.netlify.app` URL immediately
3. Add custom domain in site settings

## After deployment

Once the site is live at `https://spendlens.app/`, update these in your iOS app:

### 1. App Store Connect
- **Privacy Policy URL:** `https://spendlens.app/privacy.html` (or `/privacy` if you set up clean URLs)
- **Marketing URL:** `https://spendlens.app/`
- **Support URL:** `https://spendlens.app/support.html`

### 2. SettingsViewController.swift
Update the URL constants in the support section (per `SpendLens_Dev_Spec.md` Section 12.3):

```swift
// Support section row actions
case .privacyPolicy:
    let url = URL(string: "https://spendlens.app/privacy.html")!
    presentSafari(url: url)

case .termsOfUse:
    let url = URL(string: "https://spendlens.app/terms.html")!
    presentSafari(url: url)
```

Use `SFSafariViewController` for in-app presentation rather than opening Safari externally — keeps users in your app and is the standard iOS pattern.

### 3. Optional clean URLs
If your host supports it, configure rewrites so `/privacy` → `/privacy.html`:

**Netlify** — create `_redirects` file:
```
/privacy /privacy.html 200
/terms /terms.html 200
/support /support.html 200
```

**Cloudflare Pages** — same file, same syntax (via `_redirects`).

## Updating later

These pages are intentionally static and self-contained (no build step, no dependencies). To update:
1. Edit the HTML directly
2. Bump the "Last updated" date in `privacy.html` and `terms.html` if you make material changes
3. Re-deploy (drag & drop, or git push)

When v1.1 ships and the price moves to $9.99 with SMS Auto-Capture, update:
- `index.html` pricing card (`$6.99` → `$9.99`)
- `index.html` features section (add SMS capture)
- `support.html` already mentions Inbox/v1.1 features
- `privacy.html` already covers SMS Auto-Capture

## File checklist

- [x] `index.html` — landing page (dark, gradient hero)
- [x] `privacy.html` — privacy policy (light, legal)
- [x] `terms.html` — terms & conditions (light, legal)
- [x] `support.html` — FAQ + contact (light, helpful)
- [x] Self-contained (no external CSS/JS dependencies)
- [x] Inline SVG favicon (no separate image needed)
- [x] Mobile responsive
- [x] All cross-links work between pages

That's it. Drop the folder onto your host and you're live.
