# GBS Update Site

This folder contains a static update portal that lets users download the latest installer EXE, read release notes, and view feature usage instructions.

## Files

- `index.html`: the public update portal (download + release notes + usage guide)
- `releases.html`: previous releases and changelog archive page
- `trial.html`: dedicated trial download channel page
- `releases/latest.json`: manifest read by the page
- `releases/history.json`: release history list read by releases page
- `releases/trial-latest.json`: latest trial manifest
- `releases/trial-history.json`: trial release history
- `downloads/`: place published installer files here
- `v3.0.0-DOCUMENTATION.html`: downloadable user manual used by the latest release card

## Publish Flow

1. Build the installer first.
2. Run:

```powershell
./scripts/publish-update-site.ps1
```

Or specify a custom file:

```powershell
./scripts/publish-update-site.ps1 -InstallerPath "D:\path\to\GBS-Setup-3.0.0.exe" -Version "3.0.0"
```

You can pass release notes in a single string:

```powershell
./scripts/publish-update-site.ps1 -Version "3.0.0" -Notes "OTP throttling and lockout added | SMTP/SendGrid/Mailgun OTP providers | Encrypted secret storage and secure entry UI"
```

Each publish now updates both files automatically:

- `releases/latest.json`
- `releases/history.json` (newest entry first, deduplicated by version)

To publish the trial channel:

```powershell
./scripts/publish-update-site-trial.ps1 -Version "3.0.0-trial" -Notes "12-day trial edition | Limited advanced features | Install full edition for unlimited access"
```

This updates:

- `releases/trial-latest.json`
- `releases/trial-history.json`

The portal supports notes in `latest.json` as:

- single string
- string with `|` separators
- multi-line string

The public page also includes a local-browser comment section for the current EXE release so users can leave feedback even on static hosting. If you later move the site to Supabase-backed comments, keep the same page layout and swap the storage layer.

## Hosting Options

- GitHub Pages
- IIS
- Azure Static Web Apps
- Any web server that can host static files

## Notes

- The page provides a stable web link where users can manually download the latest EXE.
- The same `latest.json` can be reused for in-app update checks.
- The usage guide on the page is static HTML and should be updated when major features are added.
- Previous release archive is available at `releases.html` and is driven by `releases/history.json`.
- Trial channel is available at `trial.html` and is driven by trial manifests.