# GitHub Actions Workflows

This directory contains automated build workflows for the Android Servicing Tool.

## 📋 Available Workflows

### 1. **Build Windows EXE** (`build-windows.yml`)
- **Triggers:** Release published/created, or manual trigger
- **Platform:** Windows Latest (x86_64)
- **Output:**
  - `unlock_tool.exe` - Standalone executable
  - `unlock_tool-v*.zip` - Compressed distribution package
- **Python:** 3.11+

**When it runs:**
```yaml
on:
  release:
    types: [published, created]
  workflow_dispatch:  # Manual trigger from Actions tab
```

### 2. **Build macOS DMG** (`build-macos.yml`)
- **Triggers:** Release published/created, or manual trigger
- **Platform:** macOS Latest (Universal - Intel + Apple Silicon)
- **Output:**
  - `unlock_tool-*.dmg` - macOS installer package
- **Python:** 3.11+

**When it runs:**
```yaml
on:
  release:
    types: [published, created]
  workflow_dispatch:  # Manual trigger from Actions tab
```

### 3. **Build Linux Executable** (`build-linux.yml`)
- **Triggers:** Release published/created, or manual trigger
- **Platform:** Ubuntu Latest (x86_64)
- **Output:**
  - `unlock_tool` - Standalone executable
  - `unlock_tool-v*.tar.gz` - Compressed distribution package
- **Python:** 3.11+

**When it runs:**
```yaml
on:
  release:
    types: [published, created]
  workflow_dispatch:  # Manual trigger from Actions tab
```

### 4. **Multi-Platform Release Build** (`multi-platform-release.yml`)
- **Triggers:** Release published only
- **Purpose:** Orchestrates all three platform builds in parallel
- **Coordinates:** Runs Windows, macOS, and Linux builds simultaneously
- **Creates:** Release summary with all platform results

**When it runs:**
```yaml
on:
  release:
    types: [published]
```

## 🚀 How to Use

### Automatic Builds (Recommended)
1. Create a new release on GitHub:
   - Go to **Releases** tab
   - Click **Draft a new release**
   - Fill in tag (e.g., `v2.2.0`), title, description
   - Click **Publish release**

2. GitHub Actions will automatically:
   - Detect the release
   - Trigger multi-platform builds
   - Build Windows EXE, macOS DMG, and Linux executable
   - Upload all artifacts to the release
   - Create a build summary

### Manual Trigger
1. Go to **Actions** tab
2. Select the workflow you want to run:
   - Build Windows EXE
   - Build macOS DMG
   - Build Linux Executable
3. Click **Run workflow**
4. Artifacts will be available for download (not added to release)

## 📦 Output Artifacts

### Windows
- **unlock_tool.exe** - Direct executable
- **unlock_tool-v2.1.0-windows-x86_64.zip** - Full package with dependencies

### macOS
- **unlock_tool-v2.1.0-macos-universal.dmg** - Universal installer (Intel + Apple Silicon)

### Linux
- **unlock_tool** - Direct executable
- **unlock_tool-v2.1.0-linux-x86_64.tar.gz** - Full package with dependencies

## 🔧 Configuration

### Python Version
Modify the `python-version` matrix in any workflow:
```yaml
matrix:
  python-version: ['3.11']  # Change to 3.12, 3.13, etc.
```

### PyInstaller Version
Update in each workflow:
```yaml
pip install PyInstaller==6.20.0  # Change version as needed
```

### Upload Location
Artifacts are uploaded to:
1. GitHub **Release** (if triggered by release event)
2. GitHub **Actions** artifacts (30-day retention)

## 📊 Build Status

Check build status at:
- **GitHub Actions:** https://github.com/djtjms/v2master-fix-exe-build-and-license/actions
- **Release Page:** https://github.com/djtjms/v2master-fix-exe-build-and-license/releases

## ⚙️ Requirements

### Repository Secrets
None required - uses `${{ secrets.GITHUB_TOKEN }}` automatically

### Branch Requirements
- Workflows run on the default branch (main)
- Release must be published to trigger automatic builds

## 🐛 Troubleshooting

### Build Fails
1. Check **Actions** tab for error logs
2. Verify all dependencies in `requirements.txt`
3. Check PyInstaller compatibility with Python version
4. Review build spec file (`build.spec`, `build_windows.spec`)

### Artifacts Not Uploaded
- Ensure release is **published** (not draft)
- Check that `GITHUB_TOKEN` has proper permissions
- Verify workflow has `GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}`

### Build Too Slow
- Parallel builds run simultaneously (multi-platform-release.yml)
- Each platform build takes ~3-5 minutes
- Total time: ~5-10 minutes for all platforms

## 📝 Workflow Files Reference

```
.github/workflows/
├── build-windows.yml          # Windows EXE builder
├── build-macos.yml            # macOS DMG builder
├── build-linux.yml            # Linux executable builder
└── multi-platform-release.yml # Orchestrator for all builds
```

## 🔐 Security Notes

- Workflows use repository GITHUB_TOKEN (limited to current repo)
- No sensitive data is stored in workflows
- Build artifacts are automatically cleaned up after 30 days
- Builds are reproducible from source code

## 🎯 Next Steps

1. **Commit these workflows:**
   ```bash
   git add .github/workflows/
   git commit -m "Add GitHub Actions CI/CD for multi-platform builds"
   git push origin main
   ```

2. **Create a new release:**
   - Tag: `v2.1.0` (or your version)
   - Title: "Android Servicing Tool v2.1.0"
   - Description: Your release notes
   - Click "Publish release"

3. **Watch the build:**
   - Go to Actions tab
   - See all three platforms building in parallel
   - Download artifacts when complete

## 📞 Support

For issues with workflows:
1. Check GitHub Actions logs
2. Review build spec files
3. Verify dependencies are installed
4. Check Python version compatibility
