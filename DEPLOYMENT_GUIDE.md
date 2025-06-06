
# Azure Deployment Configuration Guide

## Critical Steps Required for Successful Deployment

### 1. Update package.json (MANUAL STEP REQUIRED)

**IMPORTANT**: You must manually update your `package.json` file with the following changes before deployment:

```json
{
  "name": "entra-secrets-monitor",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview",
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "serve-static": "^2.2.0"
  }
}
```

**Key Changes Needed**:
- Add `"start": "node server.js"` to scripts
- Add `express` and `serve-static` to dependencies (not devDependencies)

### 2. Azure App Service Configuration

In Azure Portal → Your App Service → Configuration:

#### General Settings:
- **Stack**: Node
- **Major Version**: 18 LTS
- **Minor Version**: 18.19
- **Startup Command**: `npm start`

#### Application Settings (Environment Variables):
Add these application settings:

```
NODE_ENV=production
WEBSITE_NODE_DEFAULT_VERSION=18.19.0
SCM_DO_BUILD_DURING_DEPLOYMENT=false
WEBSITE_RUN_FROM_PACKAGE=1
```

### 3. Deployment Process

The GitHub workflow now includes node_modules in the deployment package, ensuring all dependencies are available on Azure.

#### Manual Deployment Alternative:
If GitHub Actions fail, you can deploy manually:

1. Run locally:
   ```bash
   npm install
   npm run build
   ```

2. Create deployment ZIP with:
   - `dist/` folder
   - `node_modules/` folder  
   - `server.js`
   - `package.json`
   - `package-lock.json`
   - `web.config`

3. Upload via Azure Portal → Deployment Center → ZIP Deploy

### 4. Verification Steps

After deployment:
1. Check Azure logs: Portal → App Service → Log stream
2. Test health endpoint: `https://your-app.azurewebsites.net/api/health`
3. Monitor startup in Application Insights

### 5. Troubleshooting

If deployment still fails:
- Check Azure logs for specific error messages
- Verify Node.js version matches (18.x)
- Ensure startup command is `npm start`
- Confirm express dependency is in main dependencies, not devDependencies

### 6. Security Note

⚠️ **CRITICAL**: This configuration exposes client secrets in frontend code. For production:
- Implement backend authentication service
- Use Azure Key Vault for secrets
- Remove client secrets from frontend environment variables
