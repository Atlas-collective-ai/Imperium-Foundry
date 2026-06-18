# Imperium Foundry Deployment to Railway

**Status:** Ready to deploy (all code + config prepared)  
**Timeline:** 5-10 minutes from start to live  
**Cost:** $5-15/month (includes PostgreSQL + Next.js hosting)

---

## Step 1: Create .env.local File

Copy `.env.example` to `.env.local` and fill in minimal values:

```bash
cp .env.example .env.local
```

Edit `.env.local`:
```
NEXTAUTH_SECRET=generate-this-below
NEXTAUTH_URL=https://imperiumfoundry.com
DATABASE_URL=leave-blank-railway-sets-this
```

Generate a secure NextAuth secret:
```bash
# On Mac/Linux:
openssl rand -base64 32

# On Windows PowerShell:
[System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes((New-Guid).ToString() + (New-Guid).ToString())) | Select-Object -First 32
```

---

## Step 2: Push to GitHub

If not already done:

```bash
# Initialize git (if needed)
git init

# Add everything
git add .

# Commit
git commit -m "Initial Imperium Foundry deployment"

# Create GitHub repo (https://github.com/new)
# Name it: imperium-foundry

# Add remote
git remote add origin https://github.com/YOUR_USERNAME/imperium-foundry.git

# Rename branch
git branch -M main

# Push
git push -u origin main
```

---

## Step 3: Connect to Railway

1. Go to **https://railway.app**
2. Sign up (or log in with GitHub)
3. Click **"Create New Project"**
4. Select **"Deploy from GitHub"**
5. Find and select **imperium-foundry** repo
6. Click **Deploy**

Railway will automatically:
- ✅ Detect Next.js
- ✅ Install dependencies (npm install)
- ✅ Build application (npm run build)
- ✅ Create PostgreSQL database
- ✅ Set up environment variables

---

## Step 4: Configure Environment Variables in Railway

In Railway dashboard:

1. Click your project
2. Go to **Variables** tab
3. Add:

```
NEXTAUTH_SECRET = [paste the 32-char string from Step 1]
NEXTAUTH_URL = https://imperiumfoundry.com
NODE_ENV = production
```

4. PostgreSQL is auto-created (don't edit)

---

## Step 5: Connect Domain

1. In Railway dashboard, go to **Domains**
2. Click **"Add Domain"**
3. Enter: `imperiumfoundry.com`
4. Railway gives you DNS records
5. Go to your domain registrar (Cloudflare)
6. Add CNAME record:
   ```
   Name: imperium-foundry
   Type: CNAME
   Value: [Railway DNS endpoint]
   TTL: Auto
   ```
7. Wait 5-10 minutes for DNS to propagate

---

## Step 6: Set Up HTTPS (Auto)

Railway automatically provisions SSL certificate. No action needed.

---

## Step 7: Database Initialization

After first deploy:

```bash
# Run migrations
npx prisma migrate deploy

# Or push schema (if no migrations yet)
npx prisma db push
```

You can do this locally or via Railway shell:
1. In Railway, click your app
2. Click **Terminal** tab
3. Run the command above

---

## Step 8: Verify Deployment

Check these:

1. **Is it live?**
   - Visit https://imperiumfoundry.com
   - Should see the home page

2. **Is database working?**
   - Try creating an account
   - Check if it saves (sign in again)

3. **Are environment variables set?**
   - Go to Railway > Variables
   - All three should be present

4. **Is HTTPS working?**
   - URL should be `https://` (not `http://`)
   - No warnings

---

## Step 9: Set Up Auto-Deploys

Railway auto-deploys on every push to main:

```bash
# Make a change
echo "# Updated" >> README.md

# Push
git add .
git commit -m "Update README"
git push origin main

# Railway automatically redeploys (~1-2 min)
```

---

## Monitoring & Logs

In Railway dashboard:

**View Logs:**
1. Click your app
2. Click **Logs** tab
3. See all requests, errors, deploys

**Monitor Performance:**
1. Click **Metrics** tab
2. See CPU, memory, requests/sec

---

## Troubleshooting

### Build Fails
- Check logs in Railway
- Most common: Missing env variables
- Solution: Add to Railway Variables tab

### Site 404
- Domain not propagated yet (wait 10 min)
- Or CNAME record incorrect in Cloudflare

### Database Connection Error
- PostgreSQL auto-created, but migrations not run
- Solution: In Railway terminal, run `npx prisma db push`

### NextAuth Not Working
- NEXTAUTH_SECRET not set in Railway Variables
- NEXTAUTH_URL not set (must be https://imperiumfoundry.com)

---

## Cost Breakdown

| Item | Price | Notes |
|------|-------|-------|
| Next.js app | $5-7/month | Scales automatically |
| PostgreSQL | $5/month | 1GB included free, then $0.25/GB |
| Domain | $10/year | Via Cloudflare/GoDaddy (not Railway) |
| **Total** | **$15/month** | Very cheap for production |

---

## Deployment Commands (Quick Reference)

**Local development:**
```bash
npm run dev
# Open http://localhost:3000
```

**Build & test locally:**
```bash
npm run build
npm run start
```

**Deploy to Railway:**
```bash
git push origin main
# That's it! Railway auto-deploys
```

**View logs:**
```bash
# In Railway dashboard: Logs tab
# Or via CLI (if Railway CLI installed):
railway logs
```

---

## Next Steps After Deployment

1. ✅ Upload 10-20 sample listings (founder ideas, projects, raises, domains)
2. ✅ Test listing creation + submission process
3. ✅ Invite first 50 beta users
4. ✅ Set up team email/Telegram for listing reviews
5. ✅ Launch landing page with featured listings

---

## Running Deployment Script

Instead of manual steps, run:

**On Mac/Linux:**
```bash
chmod +x scripts/deploy.sh
./scripts/deploy.sh
```

**On Windows (PowerShell):**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
.\scripts\deploy.ps1
```

This automates:
- ✅ Git initialization
- ✅ GitHub push
- ✅ Dependency installation
- ✅ Build
- ✅ Prisma setup

Then you just connect to Railway manually (GitHub connection is fastest).

---

## Deployment Checklist

Before going live:

- [ ] .env.local created with all vars
- [ ] Code pushed to GitHub
- [ ] Railway project created
- [ ] Environment variables set in Railway
- [ ] Domain DNS configured
- [ ] Database migrations run
- [ ] https://imperiumfoundry.com loads
- [ ] Can create test account + log in
- [ ] Analytics/logging working
- [ ] HTTPS certificate valid (no warnings)

---

