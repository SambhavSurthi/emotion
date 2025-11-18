# 🚀 Render Deployment Guide

This guide will help you deploy the Emotion-Based Art Generator application to Render.

## 📋 Prerequisites

1. A **GitHub account** (or GitLab/Bitbucket)
2. A **Render account** (sign up at [render.com](https://render.com) - free tier available)
3. Your code pushed to a Git repository

## 🔧 Step-by-Step Deployment

### Step 1: Prepare Your Repository

1. Make sure all files are committed to Git:
   ```bash
   git add .
   git commit -m "Prepare for Render deployment"
   git push origin main
   ```

### Step 2: Create a New Web Service on Render

1. Log in to your [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** button
3. Select **"Web Service"**
4. Connect your repository:
   - If not connected, click **"Connect account"** and authorize Render
   - Select your repository
   - Click **"Connect"**

### Step 3: Configure the Service

Fill in the following settings:

#### Basic Settings:
- **Name**: `emotion-art-generator` (or your preferred name)
- **Region**: Choose closest to your users
- **Branch**: `main` (or your default branch)
- **Root Directory**: Leave empty (or `Emotion-based-art-generator-main-1` if your project is in a subfolder)
- **Runtime**: `Python 3`
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `gunicorn app:app`

#### Advanced Settings (Optional):
- **Auto-Deploy**: `Yes` (automatically deploys on git push)

### Step 4: Set Environment Variables

In the **Environment** section, add these variables:

| Key | Value | Description |
|-----|-------|-------------|
| `JWT_SECRET_KEY` | `your-super-secret-random-key-here` | A strong random string for JWT token signing |
| `FLASK_DEBUG` | `False` | Set to False for production |
| `PORT` | (Auto-set by Render) | Automatically set by Render, don't override |

**Important**: Generate a strong `JWT_SECRET_KEY`:
- Use a long random string (at least 32 characters)
- You can generate one using: `python -c "import secrets; print(secrets.token_urlsafe(32))"`

### Step 5: Deploy

1. Click **"Create Web Service"**
2. Render will start building your application
3. Watch the build logs in real-time
4. The build process may take **5-10 minutes** (downloading ML models)
5. Once complete, your app will be live at: `https://your-app-name.onrender.com`

## 📝 Important Notes

### Build Time Considerations

- First build may take **5-15 minutes** due to:
  - Installing PyTorch (~500MB)
  - Downloading HuggingFace transformers model
  - Installing all dependencies

### Free Tier Limitations

- **Spins down after 15 minutes** of inactivity (wakes up on next request)
- **Cold starts** can take 30-60 seconds
- Consider upgrading to a paid plan for better performance

### Database Considerations

- Currently using **SQLite** (ephemeral on Render)
- **Data will be lost on every deploy** or restart
- For production, consider:
  - **Render PostgreSQL** (free tier available)
  - **Supabase** (free tier)
  - **Railway** (PostgreSQL)

### Memory Requirements

- ML models require significant RAM
- Free tier: **512MB RAM** (may cause issues)
- Recommended: **1GB+ RAM** (Starter plan or higher)

## 🔍 Troubleshooting

### Build Fails

**Issue**: Build timeout or memory error
- **Solution**: Remove `pyaudio` and `speechrecognition` if not needed (they're heavy)
- Update `requirements.txt` to remove these if not using audio features

**Issue**: Import errors
- **Solution**: Check that all dependencies are in `requirements.txt`

### App Crashes on Startup

**Issue**: Port binding error
- **Solution**: Already handled - app uses `PORT` environment variable

**Issue**: Model loading fails
- **Solution**: App has fallback to keyword-based detection, should still work

### 502 Bad Gateway

**Issue**: App takes too long to start
- **Solution**: This is normal on free tier (cold start). Wait 30-60 seconds after first request

### CORS Errors

**Issue**: Frontend can't connect to API
- **Solution**: Already handled - app automatically adds Render URL to allowed origins

## 🔗 After Deployment

1. **Test your endpoints**:
   - `https://your-app-name.onrender.com/` - Homepage
   - `https://your-app-name.onrender.com/auth.html` - Login page
   - `https://your-app-name.onrender.com/chat.html` - Chat interface

2. **Update frontend URLs** if your frontend is separate:
   - Update API base URL to your Render URL
   - Update CORS settings if needed

3. **Monitor logs**:
   - Go to your service → **"Logs"** tab
   - Watch for errors or issues

## 🎯 Quick Reference

**Render Service URL**: `https://your-app-name.onrender.com`

**Key Files for Deployment**:
- ✅ `Procfile` - Tells Render how to run the app
- ✅ `requirements.txt` - Python dependencies
- ✅ `app.py` - Main application file
- ✅ `.gitignore` - Git ignore rules

**Environment Variables Needed**:
- `JWT_SECRET_KEY` (required)
- `FLASK_DEBUG=False` (recommended)
- `PORT` (auto-set by Render)

## 📚 Additional Resources

- [Render Documentation](https://render.com/docs)
- [Python on Render](https://render.com/docs/deploy-python)
- [Environment Variables](https://render.com/docs/environment-variables)

---

**Need Help?** Check Render's [community forum](https://community.render.com/) or [support](https://render.com/docs/support)

