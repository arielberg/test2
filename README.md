# CMS Builder - Site Repository

This is your **site repository** containing your site-specific content, configuration, and assets.

The CMS framework (`cms-core/`) is included as a [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules), allowing you to update it independently while keeping your site content separate.

## Quick Start

### 🚀 First Time Setup

1. **Clone with submodules**:
   ```bash
   git clone --recursive <this-repo-url>
   ```
   
   If you already cloned without `--recursive`:
   ```bash
   git submodule update --init --recursive
   ```

2. **Open the root URL**:
   - Navigate to your site's root URL (e.g., `https://yoursite.com/` or `http://localhost:8000/`)
   - The system will automatically detect if configuration is needed
   - You'll be redirected to the setup wizard if not configured

3. **Or run Setup Wizard directly**:
   - Open `/cms-core/init/index.html` in your browser
   - Follow the step-by-step wizard to configure everything automatically

4. **Access Admin Panel**:
   - After setup, go to `/cms-core/admin/index.html` or return to root URL
   - Start creating content!

### 📝 Manual Setup

1. **Configure your repository** in `config/appSettings.json`:
   ```json
   {
     "API_Params": ["your-username", "your-repo-name"],
     "GIT_Account": "your-username",
     "GIT_Repository": "your-repo-name"
   }
   ```

2. **Get GitHub Personal Access Token**:
   - Go to: https://github.com/settings/tokens
   - Create a token with `repo` scope
   - See `cms-core/CONFIGURATION.md` for detailed instructions

3. **Enable modules** in `config/modules.json`:
   ```json
   {
     "active": ["blog"]
   }
   ```

4. **Access admin panel** at `/cms-core/admin/`

5. **Create content** using the admin interface

## Repository Structure

```
site-repo/
├── cms-core/              # Git submodule → CMS framework
│   ├── admin/            # Admin panel
│   ├── core/             # Core engine
│   ├── modules/          # Pluggable modules
│   └── init/             # Setup wizard
├── config/               # Site-specific configuration
│   ├── appSettings.json  # Your GitHub settings
│   ├── contentTypes.json # Your content types
│   ├── modules.json      # Active modules
│   └── ...
├── assets/               # Site assets (CSS, images, fonts)
├── [content-dirs]/       # Your content (papers/, posts/, etc.)
└── index.html            # Root page (auto-redirects)
```

## Updating cms-core

To update to the latest cms-core version:

```bash
cd cms-core
git pull origin main
cd ..
git add cms-core
git commit -m "Update cms-core to latest"
git push
```

To pin to a specific version:

```bash
cd cms-core
git checkout v1.0.0  # or specific commit
cd ..
git add cms-core
git commit -m "Pin cms-core to v1.0.0"
git push
```

## Configuration

Site-specific configuration files are in `config/` (root level). These override defaults from `cms-core/config/`.

- `config/appSettings.json` - Your GitHub repository settings
- `config/contentTypes.json` - Your content types (created via admin)
- `config/modules.json` - Active modules for your site
- `config/customPages.json` - Custom pages
- `config/SEOFields.json` - SEO configuration
- `config/translations.json` - Translations

## Module System

The CMS uses a modular architecture. See `cms-core/README.md` and `cms-core/MODULE_GUIDE.md` for details.

### Available Modules

- **blog** - Blog posts with categories and reading time

## Migration from Monolithic Structure

If you're migrating from a single repository, see `REPOSITORY_SPLIT.md` for detailed instructions.

## Development

The CMS generates static HTML files from JSON content stored in GitHub. Content is managed through the admin panel and automatically committed to your repository.

## Deployment

This is a static site CMS. Deploy by:

1. **GitHub Pages**: Enable in repository settings
2. **Netlify/Vercel**: Connect your repository
3. **Any static host**: Push to repository, host serves static files

## Documentation

- `SITE_SETUP.md` - Detailed site setup guide
- `REPOSITORY_SPLIT.md` - Guide for splitting repositories
- `cms-core/README.md` - CMS framework documentation
- `cms-core/CONFIGURATION.md` - Configuration details
- `cms-core/MODULE_GUIDE.md` - Module development guide

## License

[Your License Here]
