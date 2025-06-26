# n8n on Render.com

This repository contains the necessary files to deploy n8n (workflow automation tool) on Render.com using Docker.

## Files Created

- **Dockerfile**: Simple Docker configuration that uses the latest n8n image
- **render.yaml**: Render configuration file for automated deployment with persistent disk storage

## What You Need to Do Manually

### 1. Customize the render.yaml file

Before pushing to GitHub, update these values in `render.yaml`:

- `yourUsername` → Replace with your desired n8n username
- `yourPassword` → Replace with a secure password
- `https://your-service.onrender.com` → This will be replaced with your actual Render URL after deployment

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

### 4. Important Notes

- **Storage**: ✅ Persistent disk configured! Your workflows and data will be saved to `/var/data` and persist across deployments
- **Database**: The configuration uses file-based storage on the persistent disk. For production use, you may still want to configure external PostgreSQL database
- **Webhook URL**: After deployment, update the `WEBHOOK_URL` environment variable with your actual Render URL

### 5. Persistent Storage Configuration

This setup includes a Render disk for persistent storage:

- **Disk Mount**: `/var/data` (1GB disk)
- **n8n Data Folder**: `/var/data/.n8n` (set via `N8N_USER_FOLDER`)
- **What's Stored**: Workflows, credentials, execution history, and all n8n data

Your workflows and configurations will now persist across deployments and restarts!

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