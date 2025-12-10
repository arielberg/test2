# Quick Migration Guide

If you're getting "Repository not found" error, you need to create the cms-core repository first.

## Option 1: Create Empty Repository First (Recommended)

1. **Create the repository on GitHub**:
   - Go to https://github.com/new
   - Repository name: `js.cms-core` (or your preferred name)
   - Make it **public** or **private** (your choice)
   - **Don't** initialize with README, .gitignore, or license
   - Click "Create repository"

2. **Run the migration script**:
   ```bash
   ./migrate-to-submodule.sh git@github.com:arielberg/js.cms-core.git
   ```

3. **Push cms-core content to the new repository**:
   ```bash
   cd cms-core
   git remote set-url origin git@github.com:arielberg/js.cms-core.git
   git push -u origin main
   cd ..
   ```

## Option 2: Manual Migration (If You Have cms-core Content)

If you have the cms-core files locally or in another location:

1. **Create the repository on GitHub** (same as Option 1)

2. **Clone and push cms-core content**:
   ```bash
   # If you have cms-core content somewhere
   git clone <your-cms-core-source> temp-cms-core
   cd temp-cms-core
   git remote set-url origin git@github.com:arielberg/js.cms-core.git
   git push -u origin main
   cd ..
   rm -rf temp-cms-core
   ```

3. **Add as submodule to your site**:
   ```bash
   git submodule add git@github.com:arielberg/js.cms-core.git cms-core
   git commit -m "Add cms-core as submodule"
   ```

## Option 3: Use HTTPS Instead of SSH

If SSH isn't configured, use HTTPS:

```bash
./migrate-to-submodule.sh https://github.com/arielberg/js.cms-core.git
```

## Verify SSH Access

Test your SSH connection to GitHub:

```bash
ssh -T git@github.com
```

You should see: `Hi arielberg! You've successfully authenticated...`

If not, set up SSH keys: https://docs.github.com/en/authentication/connecting-to-github-with-ssh

