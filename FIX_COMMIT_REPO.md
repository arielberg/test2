# Fix: Content Committing to Wrong Repository

If content is being committed to `meshilut` instead of `test2`, the browser is likely using cached configuration.

## The Problem

The CMS caches `appSettings` in browser localStorage. Even though `config/appSettings.json` is updated, the browser may still be using the old cached version.

## Solution: Clear Browser Cache

### Option 1: Clear localStorage (Recommended)

1. **Open browser console** (F12)
2. **Run this command**:
   ```javascript
   localStorage.removeItem('appSettings');
   location.reload();
   ```

### Option 2: Manual Clear

1. Open browser DevTools (F12)
2. Go to **Application** tab (Chrome) or **Storage** tab (Firefox)
3. Find **Local Storage** → your site URL
4. Delete the `appSettings` key
5. Refresh the page

### Option 3: Hard Refresh

1. **Chrome/Edge**: `Ctrl+Shift+R` (Windows) or `Cmd+Shift+R` (Mac)
2. **Firefox**: `Ctrl+F5` (Windows) or `Cmd+Shift+R` (Mac)

## Verify Configuration

After clearing cache, verify the config is loaded correctly:

1. Open browser console (F12)
2. Run:
   ```javascript
   // Check what's loaded
   console.log(window.appSettings);
   ```
3. Should show:
   ```javascript
   {
     API_Params: ["arielberg", "test2"],
     GIT_Account: "arielberg",
     GIT_Repository: "test2",
     ...
   }
   ```

## If Still Not Working

1. **Check config file** is committed and pushed:
   ```bash
   cat config/appSettings.json
   # Should show "test2" in GIT_Repository
   ```

2. **Check file is accessible**:
   - Open: `http://localhost:8000/config/appSettings.json`
   - Should show JSON with `"test2"` as repository

3. **Force reload admin panel**:
   - Close all admin panel tabs
   - Clear browser cache completely
   - Reopen admin panel

## Prevention

The CMS loads config in this order:
1. localStorage (cached) ← **This is the problem**
2. Site config (`/config/appSettings.json`)
3. cms-core defaults (`/cms-core/config/appSettings.json`)

After updating `config/appSettings.json`, always clear localStorage or do a hard refresh.

