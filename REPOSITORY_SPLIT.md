# Repository Split Guide

This guide explains how to split the CMS Builder into two separate repositories:
1. **cms-core** - The CMS framework (reusable, versioned)
2. **site** - Your site-specific content and configuration

## Architecture

```
site-repo/                    # Your site repository
├── cms-core/                 # Git submodule → cms-core repository
│   ├── admin/               # Admin panel (from cms-core)
│   ├── core/                # Core engine (from cms-core)
│   ├── modules/             # Modules (from cms-core)
│   └── init/                # Setup wizard (from cms-core)
├── config/                   # Site-specific configuration
│   ├── appSettings.json     # Your site's GitHub settings
│   ├── contentTypes.json    # Your site's content types
│   ├── modules.json         # Active modules for your site
│   ├── customPages.json     # Custom pages
│   ├── SEOFields.json       # SEO configuration
│   └── translations.json    # Translations
├── assets/                   # Site assets (CSS, images, etc.)
├── papers/                   # Site content (example)
├── index.html               # Root page
└── README.md
```

## Migration Steps

### Step 1: Create the cms-core Repository

1. **Create a new repository** (e.g., `cms-builder-core` on GitHub)

2. **Clone your current repository** to a temporary location:
   ```bash
   git clone <your-current-repo-url> temp-repo
   cd temp-repo
   ```

3. **Create the cms-core repository structure**:
   ```bash
   # Create new repo for cms-core
   mkdir cms-builder-core
   cd cms-builder-core
   git init
   
   # Copy cms-core directory
   cp -r ../temp-repo/cms-core/* .
   
   # Create .gitignore for cms-core
   cat > .gitignore << EOF
   node_modules/
   package-lock.json
   package.json
   .DS_Store
   *.log
   EOF
   
   # Initial commit
   git add .
   git commit -m "Initial cms-core framework"
   git branch -M main
   git remote add origin <cms-core-repo-url>
   git push -u origin main
   ```

### Step 2: Update Your Site Repository

1. **Remove cms-core from site repo** (we'll add it as submodule):
   ```bash
   cd <your-site-repo>
   git rm -r --cached cms-core
   git commit -m "Remove cms-core (will be submodule)"
   ```

2. **Add cms-core as a submodule**:
   ```bash
   git submodule add <cms-core-repo-url> cms-core
   git commit -m "Add cms-core as submodule"
   ```

3. **Move site-specific config** (if needed):
   ```bash
   # Create config directory at root if it doesn't exist
   mkdir -p config
   
   # Move site-specific config (these should stay in site repo)
   # Note: cms-core/config/ contains defaults, site config overrides them
   ```

### Step 3: Update Path References

The CMS is designed to work with `cms-core/` as a submodule. Most paths should work automatically, but verify:

- ✅ Admin panel: `/cms-core/admin/index.html`
- ✅ Setup wizard: `/cms-core/init/index.html`
- ✅ Config files: `/cms-core/config/` (defaults) or `/config/` (site overrides)

### Step 4: Update .gitignore

**Site repository `.gitignore`**:
```gitignore
# Dependencies
node_modules/
package-lock.json
package.json

# Keys and secrets
keys/
*.key
*.pem

# Development
playground/
localServer/

# OS
.DS_Store
*.log

# Submodule (keep this)
# cms-core/ is a submodule, don't ignore it
```

**cms-core repository `.gitignore`**:
```gitignore
node_modules/
package-lock.json
package.json
.DS_Store
*.log
```

## Working with Submodules

### Cloning a Repository with Submodules

```bash
# Clone with submodules
git clone --recursive <site-repo-url>

# Or if already cloned
git submodule update --init --recursive
```

### Updating cms-core

```bash
# Update to latest cms-core version
cd cms-core
git pull origin main
cd ..
git add cms-core
git commit -m "Update cms-core to latest version"
```

### Using a Specific cms-core Version

```bash
cd cms-core
git checkout v1.0.0  # or specific commit
cd ..
git add cms-core
git commit -m "Pin cms-core to v1.0.0"
```

### Adding cms-core to Existing Site

```bash
git submodule add <cms-core-repo-url> cms-core
git commit -m "Add cms-core framework"
```

## Configuration Strategy

### Site-Specific Configuration

Site-specific configuration should be in your **site repository**:

- `config/appSettings.json` - Your GitHub repository settings
- `config/contentTypes.json` - Your content types
- `config/modules.json` - Active modules for your site
- `config/customPages.json` - Custom pages
- `config/translations.json` - Translations

### cms-core Defaults

The `cms-core/config/` directory contains **default templates** that can be overridden by your site's `config/` directory.

## Benefits of This Structure

1. **Version Control**: Update cms-core independently
2. **Reusability**: Use same cms-core for multiple sites
3. **Clean Separation**: Framework vs. site content
4. **Easy Updates**: Pull latest cms-core without affecting site content
5. **Collaboration**: Different teams can work on framework vs. content

## Troubleshooting

### Submodule Shows as Modified

If `git status` shows `cms-core` as modified:
```bash
cd cms-core
git status  # Check what changed
# If you want to update to latest:
git checkout main
git pull
cd ..
git add cms-core
git commit -m "Update cms-core"
```

### Path Issues After Split

If paths don't work:
1. Ensure `cms-core/` is properly initialized as submodule
2. Check that paths use `/cms-core/` prefix
3. Verify web server serves from site root

### Config Not Found

The CMS looks for config in this order:
1. `/config/` (site-specific, takes precedence)
2. `/cms-core/config/` (defaults)

Ensure your site config is in the correct location.

## Next Steps

1. Create the cms-core repository
2. Add it as a submodule to your site
3. Test the admin panel and setup wizard
4. Commit and push both repositories
5. Document your site-specific setup

