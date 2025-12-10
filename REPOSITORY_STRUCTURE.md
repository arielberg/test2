# Repository Structure Explained

You have 3 repositories. Here's how they should work:

## The 3 Repositories

### 1. **meshilut** (Original/Development Repository)
- **Purpose**: Your original repository, possibly used for development
- **Contains**: May have mixed content (framework + site data)
- **Status**: Can be used as a reference or archived

### 2. **cms-core** (js.cms.git)
- **Purpose**: CMS framework ONLY (reusable across multiple sites)
- **Contains**: 
  - Admin panel code
  - Core engine (moduleLoader, hooks)
  - Modules (blog, etc.)
  - Setup wizard
  - Default config templates
- **Does NOT contain**: Site-specific data, content, or configuration
- **Used as**: Git submodule in site repositories

### 3. **test2** (Site Repository)
- **Purpose**: Your actual website/site repository
- **Contains**:
  - `cms-core/` (submodule → points to js.cms.git)
  - `config/` (site-specific configuration)
  - `posts/`, `papers/`, etc. (your content)
  - `assets/` (site assets)
  - `index.html` (root page)
- **This is**: The repository you deploy/host your site from

## Proper Structure

```
test2/ (Site Repository - GitHub Pages, Netlify, etc.)
├── cms-core/              # Git submodule → js.cms.git
│   ├── admin/            # Framework code
│   ├── core/             # Framework code
│   ├── modules/          # Framework code
│   └── init/             # Framework code
├── config/               # Site-specific config (in git)
│   ├── appSettings.json  # YOUR GitHub settings
│   ├── contentTypes.json # YOUR content types
│   └── ...
├── posts/                # YOUR content
├── papers/               # YOUR content
├── assets/               # YOUR assets
├── index.html            # Root page
└── .gitmodules           # Points to js.cms.git
```

## How It Works

### Setup Process

1. **Create test2 repository** on GitHub (if not exists)

2. **Add cms-core as submodule**:
   ```bash
   cd test2
   git submodule add git@github.com:arielberg/js.cms.git cms-core
   ```

3. **Copy site-specific files** from meshilut to test2:
   - `config/` directory (with your actual settings)
   - Content directories (`posts/`, `papers/`, etc.)
   - `assets/` (if site-specific)
   - `index.html`

4. **Commit and push**:
   ```bash
   git add .
   git commit -m "Initial site setup with cms-core"
   git push
   ```

### Daily Workflow

1. **Update cms-core framework** (when new version available):
   ```bash
   cd test2/cms-core
   git pull origin master
   cd ..
   git add cms-core
   git commit -m "Update cms-core to latest"
   git push
   ```

2. **Add/edit content** (via admin panel):
   - Changes saved to `test2` repository
   - Content goes in `test2/posts/`, `test2/papers/`, etc.
   - Config changes go in `test2/config/`

3. **Deploy site**:
   - Push to `test2` repository
   - GitHub Pages/Netlify serves from `test2`

## What Goes Where

### cms-core (js.cms.git) - Framework Only
✅ **Should contain:**
- Admin panel code
- Core engine
- Modules
- Setup wizard
- Default config templates (with placeholders)

❌ **Should NOT contain:**
- Your GitHub username/repo
- Your content types
- Your content
- Your site-specific config

### test2 (Site Repository) - Your Site
✅ **Should contain:**
- `cms-core/` (submodule)
- `config/` (your actual settings)
- Your content (`posts/`, `papers/`, etc.)
- Your assets
- `index.html`

## Migration from meshilut to test2

If you want to move from `meshilut` to `test2`:

```bash
# 1. Clone test2 (or create new)
git clone git@github.com:arielberg/test2.git
cd test2

# 2. Add cms-core submodule
git submodule add git@github.com:arielberg/js.cms.git cms-core

# 3. Copy site-specific files from meshilut
# (config/, content dirs, assets/, index.html)

# 4. Commit
git add .
git commit -m "Migrate site to test2 repository"
git push
```

## Benefits

1. **Separation**: Framework code separate from site content
2. **Reusability**: Use same cms-core for multiple sites
3. **Updates**: Update framework without touching site content
4. **Clean**: Site repository only contains what's needed for that site

## Current Status

- ✅ `cms-core` (js.cms.git) - Framework repository exists
- ✅ `meshilut` - Original repository (can be reference)
- ⚠️ `test2` - Needs to be set up as site repository with cms-core submodule

