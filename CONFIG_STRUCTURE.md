# Configuration Structure

This document explains how configuration files are organized between the site repository and cms-core.

## Overview

- **Site Repository (`config/`)**: Contains all site-specific configuration and data
- **cms-core (`cms-core/config/`)**: Contains only default/example templates

## Configuration Priority

The CMS checks for configuration files in this order:

1. **Site Config** (`/config/` or `./config/`) - **Takes precedence**
2. **cms-core Defaults** (`/cms-core/config/`) - Fallback templates

## Site Repository Structure

```
site-repo/
├── config/                    # Site-specific configuration (in git)
│   ├── appSettings.json      # Your GitHub repository settings
│   ├── contentTypes.json     # Your content types (created via admin)
│   ├── modules.json          # Active modules for your site
│   ├── customPages.json      # Custom pages
│   ├── SEOFields.json        # SEO configuration
│   └── translations.json     # Translations
├── cms-core/                 # Git submodule (framework only)
│   └── config/               # Default templates (not site-specific)
└── [content-dirs]/           # Your content (posts/, papers/, etc.)
```

## What Goes Where

### Site Repository (`config/`)

**All site-specific data:**
- `appSettings.json` - Your GitHub account, repository, languages
- `contentTypes.json` - Content types you create via admin panel
- `modules.json` - Which modules are active for your site
- `customPages.json` - Custom page definitions
- `SEOFields.json` - SEO field configuration
- `translations.json` - Translation strings

**Content directories:**
- `posts/`, `papers/`, etc. - All your actual content

### cms-core (`cms-core/config/`)

**Only default/example templates:**
- `appSettings.json` - Example with placeholders (`your-username`, `your-repo-name`)
- `contentTypes.json` - Empty array `[]`
- `modules.json` - Empty active modules list
- Other configs - Default templates only

## Setup Process

1. **Root `index.html`** checks for `config/appSettings.json`
2. If not found or not configured → redirects to setup wizard
3. **Setup wizard** saves to `config/appSettings.json` (site repository)
4. **Admin panel** loads from `config/` first, falls back to `cms-core/config/`

## Saving Configuration

- **Setup Wizard**: Saves to `config/appSettings.json` (site repo)
- **Content Type Manager**: Saves to `config/contentTypes.json` (site repo)
- **All admin operations**: Save to site repository, not cms-core

## Benefits

1. **Clean Separation**: Framework code vs. site data
2. **Version Control**: Site config is in your site repository
3. **Updates**: Updating cms-core doesn't affect your site config
4. **Multiple Sites**: Use same cms-core for different sites with different configs

## Important Notes

- **Never commit site-specific data to cms-core**
- **cms-core/config/** should only contain default templates
- **All site data** goes in your site repository's `config/` directory
- **Content items** are saved in your site repository (e.g., `posts/`, `papers/`)

