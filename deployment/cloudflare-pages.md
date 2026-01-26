# Deploying to Cloudflare Pages

Cloudflare Pages is a JAMstack platform for deploying static sites with automatic builds, global CDN, and free SSL certificates.

## Prerequisites

- GitHub, GitLab, or Bitbucket account
- Cloudflare account (free tier is sufficient)
- Your website code in a Git repository

## Option 1: Deploy via Git Integration (Recommended)

### Step 1: Push Your Code to Git

If you haven't already, initialize a Git repository and push to GitHub:

```bash
cd cv-portfolio
git init
git add .
git commit -m "Initial commit: CV portfolio website"
git branch -M main
git remote add origin https://github.com/yourusername/cv-portfolio.git
git push -u origin main
```

### Step 2: Connect to Cloudflare Pages

1. Log in to [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Navigate to **Pages** in the left sidebar
3. Click **Create a project**
4. Click **Connect to Git**
5. Authorize Cloudflare to access your Git provider
6. Select your repository: `cv-portfolio`

### Step 3: Configure Build Settings

Since this is a static HTML site, use these settings:

- **Project name**: `cv-portfolio` (or your preferred name)
- **Production branch**: `main`
- **Framework preset**: `None` (or select "Static HTML" if available)
- **Build command**: Leave empty
- **Build output directory**: `/`
- **Root directory**: `/` (or leave empty)

### Step 4: Environment Variables (Optional)

If you need any environment variables, add them in the **Environment variables** section. For this static site, you likely won't need any.

### Step 5: Deploy

1. Click **Save and Deploy**
2. Cloudflare will start building and deploying your site
3. Wait for the deployment to complete (usually 1-2 minutes)
4. Your site will be available at: `https://cv-portfolio.pages.dev`

### Step 6: Configure Custom Domain (Optional)

1. In your Cloudflare Pages project, go to **Custom domains**
2. Click **Set up a custom domain**
3. Enter your domain name (e.g., `romanvelicka.com`)
4. Follow the DNS configuration instructions
5. Cloudflare will automatically provision an SSL certificate

## Option 2: Deploy via Wrangler CLI

### Step 1: Install Wrangler

```bash
npm install -g wrangler
```

### Step 2: Authenticate

```bash
wrangler login
```

This will open a browser window to authenticate with Cloudflare.

### Step 3: Create Pages Project

```bash
cd cv-portfolio
wrangler pages project create cv-portfolio
```

### Step 4: Deploy

```bash
wrangler pages publish . --project-name=cv-portfolio
```

Your site will be deployed and you'll receive a URL.

## Option 3: Deploy via Direct Upload

### Step 1: Prepare Your Files

Make sure all your website files are in a single directory with `index.html` at the root.

### Step 2: Create Project in Dashboard

1. Go to [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Navigate to **Pages**
3. Click **Create a project**
4. Choose **Direct Upload**
5. Name your project: `cv-portfolio`

### Step 3: Upload Files

1. Click **Upload assets**
2. Drag and drop your entire `cv-portfolio` folder
3. Or click to browse and select your files
4. Click **Deploy site**

## Automatic Deployments

With Git integration, Cloudflare Pages automatically:

- Deploys on every push to the production branch
- Creates preview deployments for pull requests
- Provides unique URLs for each deployment

## Custom Domain Configuration

### If you own a domain:

1. **Option A: Use Cloudflare as your DNS provider** (Recommended):
   - Transfer or add your domain to Cloudflare
   - Go to Pages → Custom domains → Add
   - Enter your domain name
   - DNS records are configured automatically

2. **Option B: Use external DNS provider**:
   - Add a CNAME record:
     ```
     Type: CNAME
     Name: @ (or www)
     Value: cv-portfolio.pages.dev
     ```
   - In Cloudflare Pages, add the custom domain
   - Verify ownership

### SSL Certificate

Cloudflare automatically provisions and renews SSL certificates for your custom domain. No configuration needed!

## Performance Optimization

Cloudflare Pages includes these features by default:

- **Global CDN**: Your site is served from 275+ locations worldwide
- **Automatic minification**: HTML, CSS, and JS are minified
- **Brotli compression**: Better than gzip
- **HTTP/2 and HTTP/3**: Latest protocols enabled
- **DDoS protection**: Enterprise-level protection
- **Analytics**: Built-in web analytics

### Enable Additional Optimization:

1. Go to your site's **Settings**
2. Under **Build & Deployment**:
   - Enable **Auto Minify** for HTML, CSS, JS
   - Enable **Rocket Loader™** (optional, test first)

## Build Configuration File

Create a `_headers` file in your project root for custom headers:

```
/*
  X-Frame-Options: SAMEORIGIN
  X-Content-Type-Options: nosniff
  X-XSS-Protection: 1; mode=block
  Referrer-Policy: no-referrer-when-downgrade
  Permissions-Policy: geolocation=(), microphone=(), camera=()

/*.css
  Cache-Control: public, max-age=31536000, immutable

/*.js
  Cache-Control: public, max-age=31536000, immutable

/*.png
  Cache-Control: public, max-age=31536000, immutable

/*.jpg
  Cache-Control: public, max-age=31536000, immutable

/*.svg
  Cache-Control: public, max-age=31536000, immutable
```

Create a `_redirects` file for redirects (if needed):

```
# Redirect www to non-www
https://www.romanvelicka.com/*  https://romanvelicka.com/:splat  301!

# Or redirect non-www to www
# https://romanvelicka.com/*  https://www.romanvelicka.com/:splat  301!
```

## Deployment Workflow

1. Make changes to your local files
2. Test locally (open `index.html` in browser)
3. Commit changes:
   ```bash
   git add .
   git commit -m "Update: description of changes"
   ```
4. Push to GitHub:
   ```bash
   git push origin main
   ```
5. Cloudflare Pages automatically deploys
6. Check deployment status in Cloudflare Dashboard
7. Visit your site to verify changes

## Preview Deployments

For testing changes before going live:

1. Create a new branch:
   ```bash
   git checkout -b feature/new-design
   ```
2. Make your changes and commit
3. Push the branch:
   ```bash
   git push origin feature/new-design
   ```
4. Cloudflare creates a preview deployment
5. Test the preview URL
6. Merge to main when ready:
   ```bash
   git checkout main
   git merge feature/new-design
   git push origin main
   ```

## Monitoring & Analytics

### Built-in Analytics:

1. Go to your Pages project
2. Click on **Analytics** tab
3. View:
   - Page views
   - Unique visitors
   - Geographic distribution
   - Popular pages
   - Referrers

### Web Analytics (Privacy-friendly):

1. Enable in project settings
2. No JavaScript needed
3. GDPR compliant
4. Free on all plans

## Rollback Deployments

If something goes wrong:

1. Go to your Pages project
2. Click **Deployments** tab
3. Find a previous successful deployment
4. Click **···** (three dots)
5. Select **Rollback to this deployment**

## Environment-Specific Builds

If you need different configurations for production and preview:

1. In project settings, go to **Environment variables**
2. Add variables with different values for:
   - **Production**: Applied to production branch
   - **Preview**: Applied to all preview branches

## Troubleshooting

### Deployment fails:
- Check build logs in the Cloudflare dashboard
- Ensure all file paths are correct and case-sensitive
- Verify `index.html` exists at the root

### Custom domain not working:
- Wait up to 24 hours for DNS propagation
- Verify DNS records are correct
- Check SSL certificate status in dashboard

### 404 errors:
- Ensure file names match exactly (case-sensitive)
- Check that all referenced files exist
- Verify paths in HTML, CSS, and JS files

### Language detection not working:
- The JavaScript uses browser language detection
- This works client-side, no server configuration needed
- Test with different browser language settings

## Costs

**Cloudflare Pages is FREE for:**
- Unlimited sites
- Unlimited requests
- Unlimited bandwidth
- 500 builds per month
- 1 concurrent build

**Pro features** (paid plans):
- More builds per month
- Concurrent builds
- Advanced analytics

For a personal CV site, the free tier is more than sufficient!

## Additional Resources

- [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/)
- [Cloudflare Community](https://community.cloudflare.com/)
- [Cloudflare Status](https://www.cloudflarestatus.com/)

Your CV website will be live globally with enterprise-grade performance! 🚀
