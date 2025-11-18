# 🚨 Quick Fix Guide for Render Deployment Errors

## ⚠️ Most Common Issue: PORT Environment Variable

**Problem**: If you manually added `PORT` in environment variables, **DELETE IT NOW**.

Render automatically sets the `PORT` variable. Having it manually set can cause conflicts.

### How to Remove PORT:
1. Go to your Render service dashboard
2. Click on **"Environment"** tab
3. Find `PORT` in the list
4. Click the **trash icon** to delete it
5. Save changes

## ✅ Correct Environment Variables

**Only set these:**
- ✅ `JWT_SECRET_KEY` = `your-random-secret-here`
- ✅ `FLASK_DEBUG` = `False`
- ❌ **DO NOT SET** `PORT` (Render handles this automatically)

## 🔧 Updated Configuration

### Root Directory
```
Emotion-based-art-generator-main-1
```

### Build Command
```
pip install -r requirements.txt
```

### Start Command
```
gunicorn --bind 0.0.0.0:$PORT app:app
```

**OR** if you're using Procfile (recommended), leave Start Command empty and use:
```
web: gunicorn --bind 0.0.0.0:$PORT app:app
```

## 🐛 Other Common Fixes

### If Build Fails:
1. Check that `requirements.txt` is up to date (pyaudio and speechrecognition removed)
2. Verify Python version is 3.12.0 or compatible
3. Check build logs for specific error

### If App Won't Start:
1. Check Logs tab for error messages
2. Verify Procfile exists in root directory
3. Make sure `gunicorn` is in `requirements.txt`

### If You Get 502 Error:
- Free tier: Wait 30-60 seconds (cold start)
- Check Logs for crash messages
- May need more RAM (upgrade plan)

## 📝 Quick Checklist

- [ ] Removed `PORT` from environment variables
- [ ] Set `JWT_SECRET_KEY` with a random string
- [ ] Set `FLASK_DEBUG` to `False`
- [ ] Root Directory: `Emotion-based-art-generator-main-1`
- [ ] Build Command: `pip install -r requirements.txt`
- [ ] Start Command: `gunicorn --bind 0.0.0.0:$PORT app:app` OR leave empty if using Procfile
- [ ] All files committed and pushed to Git
- [ ] Build logs show no errors
- [ ] Service shows "Live" status

## 💡 Still Having Issues?

Share the exact error message from the **Logs** tab, and I can help diagnose!

