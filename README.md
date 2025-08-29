# iMediaReleasePages 
<a href="https://www.buymeacoffee.com/dvt1405"><img src="https://img.buymeacoffee.com/button-api/?text=Buy a coffee here&emoji=☕&slug=dvt1405&button_colour=5F7FFF&font_colour=ffffff&font_family=Cookie&outline_colour=000000&coffee_colour=FFDD00" /></a>
### This is release page of iMedia
* Lastest version: 25.04.05
* Android TV: [DownloadTVAPK](https://github.com/dvt1405/iMediaReleasePages/raw/refs/heads/gh-pages/iMedia-25.04.05.B.apk)
* Mobile: [MobileAPK](https://github.com/dvt1405/iMediaReleasePages/raw/refs/heads/gh-pages/iMedia.Mobile.25.04.01-modBeta-release.apk)


---

## Guide: Get Firebase token for GitHub Actions

The workflow `.github/workflows/update-apk-links.yml` deploys this site to Firebase Hosting and requires a repository secret named `FIREBASE_TOKEN`.

Follow these steps to create the token and add it to GitHub:

### Prerequisites
- You have a Firebase project (Hosting enabled).
- Node.js installed.
- Firebase CLI installed (or install it below).

### 1) Install and log in to Firebase CLI (locally)
```bash
npm install -g firebase-tools
firebase --version
firebase login
```
This opens a browser to authenticate with your Google account that has access to the Firebase project.

If you’re on a headless environment/SSH, use:
```bash
firebase login --no-localhost
```

### 2) Generate a CI token
Use the CLI to generate a long‑lived refresh token for CI:
```bash
firebase login:ci
```
Copy the printed token string (do NOT share it publicly).

If using a headless environment:
```bash
firebase login:ci --no-localhost
```

### 3) Add the token as a GitHub secret
1. Go to your GitHub repository → Settings → Secrets and variables → Actions → New repository secret.
2. Name: `FIREBASE_TOKEN`
3. Value: paste the token from step 2.

Optional (if your token doesn’t have a default project set or you want to be explicit):
- Add another secret `FIREBASE_PROJECT_ID` with your Firebase project ID (e.g., `my-project-id`).

The workflow already reads these:
```yaml
env:
  FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
  FIREBASE_PROJECT_ID: ${{ secrets.FIREBASE_PROJECT_ID }} # optional
```

### 4) Trigger the workflow
Push a change that matches the workflow triggers (e.g., update `index.html`, or add/replace an APK under `tv/` or `mobile/`). The workflow will:
- Update the direct APK links in `index.html` to the latest files.
- Deploy to Firebase Hosting: `firebase deploy --only hosting`.

### Rotate or revoke the token
- To revoke from the machine: `firebase logout` (or remove the session from your Google account’s security page).
- To rotate for CI: run `firebase login:ci` again and replace the `FIREBASE_TOKEN` secret in GitHub.

### Troubleshooting
- "FIREBASE_TOKEN secret is not set": ensure the secret exists under your repo settings.
- Permission errors: confirm the Google account used to create the token has access to the Firebase project and Hosting.
- Multiple projects: set `FIREBASE_PROJECT_ID` secret to explicitly select the project.

### Alternative: Service Account (optional)
Advanced users can use a Google Cloud Service Account with Hosting Admin permissions and set up authentication via GitHub Actions OIDC or a JSON key. For most cases, the `firebase login:ci` token method above is simpler and sufficient.
