# Site Repository Setup

This is your **site repository** - it contains your site-specific content, configuration, and assets.

## Structure

```
site-repo/
├── cms-core/              # Git submodule → CMS framework
├── config/                # Site-specific configuration
│   ├── appSettings.json   # Your GitHub repository settings
│   ├── contentTypes.json  # Your content types
│   ├── modules.json       # Active modules
│   ├── customPages.json   # Custom pages
│   ├── SEOFields.json     # SEO configuration
│   └── translations.json  # Translations
├── assets/                # Site assets (CSS, images, fonts)
├── [content-dirs]/        # Your content (papers/, posts/, etc.)
├── index.html             # Root page (auto-redirects)
└── README.md              # This file
```

## Initial Setup

### 1. Clone with Submodules

```bash
git clone --recursive <this-repo-url>
```

If you already cloned without `--recursive`:

```bash
git submodule update --init --recursive
```

### 2. Configure Your Site

1. **Run the setup wizard**:
   - Open `index.html` in your browser
   - Or go directly to `cms-core/init/index.html`
   - Follow the wizard to configure GitHub settings

2. **Or manually edit** `config/appSettings.json`:
   ```json
   {
     "API_Params": ["your-username", "your-repo-name"],
     "GIT_Account": "your-username",
     "GIT_Repository": "your-repo-name"
   }
   ```

3. **Get GitHub token**:
   - Go to: https://github.com/settings/tokens
   - Create token with `repo` scope
   - Enter it in the setup wizard or admin panel

### 3. Access Admin Panel

- Go to `cms-core/admin/index.html`
- Or use the root `index.html` (auto-redirects)

## Configuration Files

All configuration files in `config/` are **site-specific** and override defaults from `cms-core/config/`.

### appSettings.json

Your GitHub repository settings:
```json
{
  "API_Params": ["username", "repo-name"],
  "GIT_Account": "username",
  "GIT_Repository": "repo-name",
  "Lanugages": ["", "en", "he"]
}
```

### contentTypes.json

Your site's content types (created via admin panel):
```json
[
  {
    "name": "post",
    "label": "Post",
    "labelPlural": "Posts",
    "urlPrefix": "posts/",
    "fields": [...]
  }
]
```

### modules.json

Which modules are active:
```json
{
  "active": ["blog"],
  "disabled": []
}
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
git checkout v1.0.0  # or specific commit hash
cd ..
git add cms-core
git commit -m "Pin cms-core to v1.0.0"
git push
```

## Adding Content

1. Use the admin panel to create content types
2. Add content items through the admin interface
3. Content is stored in your repository and rendered as static HTML

## Deployment

This is a static site CMS. Deploy by:

1. **GitHub Pages**: Enable in repository settings
2. **Netlify/Vercel**: Connect your repository
3. **Any static host**: Push to repository, host serves static files

The CMS generates static HTML files that can be served from any static hosting service.

## Troubleshooting

### cms-core shows as modified

If `git status` shows `cms-core` as modified:

```bash
cd cms-core
git status  # Check what changed
# If you want to update:
git checkout main
git pull
cd ..
git add cms-core
git commit -m "Update cms-core"
```

### Config not found

The CMS looks for config in this order:
1. `/config/` (site-specific, takes precedence)
2. `/cms-core/config/` (defaults)

Ensure your site config is in `./config/` (root level).

### Path issues

Ensure:
- `cms-core/` is properly initialized as submodule
- Web server serves from site root
- Paths use `/cms-core/` prefix

## Support

For CMS framework issues, see `cms-core/README.md`.

For site-specific issues, check your `config/` files and content structure.

