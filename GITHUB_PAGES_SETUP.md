# GitHub Pages Deployment Guide for Jarvis

## 🚀 Deploy Your Web App to GitHub Pages

### Step 1: Enable GitHub Pages in Repository Settings

1. Go to: **https://github.com/siva716/jarvis/settings/pages**
2. Under "Build and deployment":
   - **Source**: Select `GitHub Actions`
   - **Branch**: `main` (or keep current)
3. Click **Save**

---

### Step 2: Create GitHub Pages Workflow File

Copy this workflow and create `.github/workflows/pages-deploy.yml` in your repository:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.13.0'
          channel: 'stable'
      
      - name: Get dependencies
        run: flutter pub get
      
      - name: Build web
        run: flutter build web --release --base-href /jarvis/
      
      - name: Setup Pages
        uses: actions/configure-pages@v3
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: './build/web'
      
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

---

### Step 3: Push the Workflow File to GitHub

```bash
# Create the workflow directory if it doesn't exist
mkdir -p .github/workflows

# Copy the workflow content to the file
# Then commit and push
git add .github/workflows/pages-deploy.yml
git commit -m "Add GitHub Pages deployment workflow"
git push origin main
```

---

### Step 4: Monitor Deployment

1. Go to: **https://github.com/siva716/jarvis/actions**
2. Click on the latest workflow run
3. Watch the build progress
4. Once complete, your app is live! 🎉

---

### Step 5: Access Your Live App

**Your web app will be available at:**
```
https://siva716.github.io/jarvis/
```

---

## 📱 What Gets Deployed

The GitHub Pages deployment will include:
- ✅ Full Flutter web app
- ✅ All assets and resources
- ✅ Chat UI and AI integration
- ✅ Service worker for offline support

---

## 🔧 Configuration

### Update Base URL (Important!)

The workflow uses `--base-href /jarvis/` which means your app runs at `/jarvis/` path.

If you want to deploy to your own domain instead:
1. Remove `--base-href /jarvis/` from the workflow
2. Add a `CNAME` file to the root directory with your domain name
3. Update workflow to include `cname: yourdomain.com`

---

## 🔌 API Endpoint Configuration

Before deploying, update your backend API endpoint:

**File:** `lib/services/ai_service.dart`

```dart
static const String baseUrl = 'https://your-production-api.com';
```

**Important:** Use an absolute URL (not `localhost`) for production!

---

## 🚀 Automatic Deployment

After setting up the workflow:
- Every push to `main` branch triggers automatic build
- GitHub Actions builds the Flutter web app
- Automatically deploys to `https://siva716.github.io/jarvis/`
- Updates complete in ~5-10 minutes

---

## ✅ Deployment Checklist

- [ ] GitHub Pages enabled in Settings
- [ ] Workflow file created at `.github/workflows/pages-deploy.yml`
- [ ] Backend API URL configured in `ai_service.dart`
- [ ] Committed and pushed changes to main branch
- [ ] GitHub Actions workflow completed successfully
- [ ] App accessible at https://siva716.github.io/jarvis/

---

## 🆘 Troubleshooting

| Issue | Solution |
|-------|----------|
| Build fails | Check Flutter version and dependencies: `flutter pub get` |
| App won't load | Verify base-href matches deployment path |
| API calls fail | Check backend URL is production endpoint, not localhost |
| Pages not updating | Clear browser cache or use incognito mode |
| 404 on index.html | Ensure build/web contains index.html after build |

---

## 📊 Check Deployment Status

1. **View workflow runs:** https://github.com/siva716/jarvis/actions
2. **View latest deployment:** https://github.com/siva716/jarvis/deployments
3. **Access live app:** https://siva716.github.io/jarvis/

---

## 🔐 Security Notes

- GitHub Pages serves over HTTPS automatically ✅
- API calls should use HTTPS endpoints ✅
- Sensitive credentials should NOT be in code
- Use GitHub Secrets for API keys if needed ✅

---

## 📞 Need Help?

- [GitHub Pages Docs](https://docs.github.com/en/pages)
- [Flutter Web Docs](https://docs.flutter.dev/platform-integration/web)
- [GitHub Actions Docs](https://docs.github.com/en/actions)

---

**Your Jarvis AI Assistant is ready for GitHub Pages deployment! 🎉**
