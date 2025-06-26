# n8n on Render.com

This repository contains the necessary files to deploy n8n (workflow automation tool) on Render.com using Docker.

## Files Created

- **Dockerfile**: Simple Docker configuration that uses the latest n8n image
- **render.yaml**: Render configuration file for automated deployment with persistent disk storage

## What You Need to Do Manually

### 1. Set Environment Variables in Render Console

For security reasons, authentication credentials are not included in the repository. You'll need to set these environment variables manually in the Render console after deployment:

**Required Environment Variables:**

Set all of these in the Render console (Environment tab):

**Authentication:**
- `N8N_BASIC_AUTH_USER` → Your desired n8n username
- `N8N_BASIC_AUTH_PASSWORD` → A secure password

**URL Configuration:**
- `WEBHOOK_URL` → Your full Render service URL (e.g., `https://your-service-name.onrender.com`)
- `N8N_HOST` → Just the hostname part (e.g., `your-service-name.onrender.com`)
- `N8N_PROTOCOL` → Should be `https`  
- `N8N_PORT` → Should be `443`

**How to set them:**
1. After deploying your service on Render
2. Go to your service dashboard
3. Click on the **"Environment"** tab
4. Add ALL the environment variables listed above (both authentication and URL configuration)
5. Click "Save Changes" and your service will automatically redeploy

### 2. Push to GitHub

```bash
# Add a remote repository (create one on GitHub first)
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git

# Push to GitHub
git push -u origin main
```

### 3. Deploy on Render.com

1. Go to [Render Dashboard](https://render.com/dashboard)
2. Click **"New Web Service"**
3. Select **"Deploy from a Git repository"**
4. Choose your GitHub repository
5. Render should automatically detect the Docker setup
6. Review the configuration and deploy
7. **Critical:** Immediately add all environment variables in Render console:
   - Go to your service → **Environment** tab
   - Add all 6 environment variables from the reference section
   - Click "Save Changes" to redeploy with the variables

### 4. Important Notes

- **Security**: ✅ Credentials are kept secure! Authentication variables are set in Render console, not in the repository
- **Storage**: ✅ Persistent disk configured! Your workflows and data will be saved to `/var/data` and persist across deployments
- **Database**: The configuration uses file-based storage on the persistent disk. For production use, you may still want to configure external PostgreSQL database
- **Environment Variables**: Remember to set the required environment variables in Render console after deployment

### 5. Persistent Storage Configuration

This setup includes a Render disk for persistent storage:

- **Disk Mount**: `/var/data` (1GB disk)
- **n8n Data Folder**: `/var/data/.n8n` (set via `N8N_USER_FOLDER`)
- **What's Stored**: Workflows, credentials, execution history, and all n8n data

Your workflows and configurations will now persist across deployments and restarts!

### 6. Environment Variables Reference

**Set ALL of these in the Render console (Environment tab):**

```env
# Authentication
N8N_BASIC_AUTH_USER=your_chosen_username
N8N_BASIC_AUTH_PASSWORD=your_secure_password_here

# URL Configuration (replace with your actual Render URL)
WEBHOOK_URL=https://n8n-render-f6eh.onrender.com
N8N_HOST=n8n-render-f6eh.onrender.com
N8N_PROTOCOL=https
N8N_PORT=443
```

> **Important**: Replace `n8n-render-f6eh.onrender.com` with your actual Render service URL. You can find this in your Render service dashboard.

**Why these URL variables fix the localhost issue:**
- `WEBHOOK_URL` - Tells n8n where webhooks should point
- `N8N_HOST` - Sets the hostname n8n thinks it's running on (fixes localhost problem)
- `N8N_PROTOCOL` & `N8N_PORT` - Ensures n8n knows it's running on HTTPS port 443

> **Security Note**: All sensitive variables (credentials and URLs) are kept in Render console, not in your repository code.

### Optional: Configure External Database

For persistent storage, add these environment variables in Render:

```
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=your-postgres-host
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=n8n
DB_POSTGRESDB_USER=your-db-user
DB_POSTGRESDB_PASSWORD=your-db-password
``` 