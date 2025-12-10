# Migrating to test2 Repository

Step-by-step guide to set up `test2` as your site repository.

## Prerequisites

- `test2` repository exists on GitHub (create if needed)
- You have access to both `meshilut` and `test2` repositories

## Step 1: Clone or Initialize test2

```bash
# If test2 doesn't exist locally
git clone git@github.com:arielberg/test2.git
cd test2

# Or if you need to create it
mkdir test2
cd test2
git init
git remote add origin git@github.com:arielberg/test2.git
```

## Step 2: Add cms-core as Submodule

```bash
# From test2 directory
git submodule add git@github.com:arielberg/js.cms.git cms-core
```

This creates:
- `cms-core/` directory (points to js.cms.git)
- `.gitmodules` file

## Step 3: Copy Site-Specific Files from meshilut

From your `meshilut` directory, copy these to `test2`:

```bash
# From meshilut directory
cd /path/to/meshilut

# Copy config directory (your site settings)
cp -r config ../test2/

# Copy content directories (if any)
cp -r posts ../test2/ 2>/dev/null || true
cp -r papers ../test2/ 2>/dev/null || true

# Copy assets (if site-specific)
cp -r assets ../test2/ 2>/dev/null || true

# Copy root index.html
cp index.html ../test2/
```

## Step 4: Update Config Files

Edit `test2/config/appSettings.json` to point to `test2` repository:

```json
{
    "API_Gate": "GitHubAPI",
    "API_Params": [
        "arielberg",
        "test2"
    ],
    "GIT_Account": "arielberg",
    "GIT_Repository": "test2",
    ...
}
```

## Step 5: Commit and Push

```bash
cd test2

# Add all files
git add .

# Commit
git commit -m "Initial site setup with cms-core framework"

# Push
git push -u origin main
# or
git push -u origin master
```

## Step 6: Verify Setup

1. **Check submodule**:
   ```bash
   git submodule status
   ```

2. **Test locally**:
   - Open `test2/index.html` in browser
   - Should redirect to setup wizard or admin panel

3. **Verify config**:
   - Check `test2/config/appSettings.json` has correct repo name
   - Check `test2/cms-core/` exists and has framework files

## Step 7: Deploy

### GitHub Pages
1. Go to `test2` repository settings
2. Enable GitHub Pages
3. Select branch (usually `main` or `master`)
4. Your site will be at: `https://arielberg.github.io/test2/`

### Netlify/Vercel
1. Connect `test2` repository
2. Deploy automatically on push

## Troubleshooting

### Submodule not initialized
```bash
cd test2
git submodule update --init --recursive
```

### Wrong repository in config
Edit `test2/config/appSettings.json` and update:
- `GIT_Account`: Your GitHub username
- `GIT_Repository`: `test2`
- `API_Params`: `["arielberg", "test2"]`

### Missing config files
Copy from `meshilut/config/` or create defaults in `test2/config/`

## After Migration

- **meshilut**: Can be kept as reference or archived
- **test2**: Your active site repository
- **cms-core**: Updated independently via submodule

