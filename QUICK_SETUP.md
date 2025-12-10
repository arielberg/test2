# Quick Setup - Change Repository and Push

You're right! Just update the config and push. Here's what was done:

## Changes Made

1. ✅ Updated `config/appSettings.json`:
   - `GIT_Repository`: `"test2"`
   - `API_Params`: `["arielberg", "test2"]`

2. ✅ Changed git remote:
   - From: `git@github.com:arielberg/meshilut.git`
   - To: `git@github.com:arielberg/test2.git`

## Next Steps

### 1. Commit Changes

```bash
git add config/appSettings.json
git add .gitmodules cms-core
git add config/ .gitignore README.md
git commit -m "Configure for test2 repository"
```

### 2. Push to test2

```bash
# First push (if test2 is new)
git push -u origin main

# Or if test2 already exists
git push -u origin master
```

### 3. Verify

- Check `test2` repository on GitHub
- Should see all your files including `config/`, `cms-core/` submodule, content, etc.

## Important Notes

- **cms-core submodule**: Already points to `js.cms.git` (correct)
- **Site config**: Now points to `test2` repository
- **Content**: All your content (posts/, papers/, etc.) will be in `test2`
- **Deployment**: Deploy `test2` as your site repository

## After Push

1. **GitHub Pages**: Enable in `test2` repository settings
2. **Admin Panel**: Will save content to `test2` repository
3. **Updates**: Update cms-core via submodule when needed

That's it! Much simpler than migrating. 🎉

