# Railway Deployment Instructions (Skip Local Build)

**Status:** Ready to deploy — no local npm install needed  
**Why:** Railway builds in their environment (saves disk space)  
**Time:** 5 minutes

---

## What You Need

✅ GitHub account (created)  
✅ Railway account (free, takes 30 sec)  
✅ .env.local file (created at C:\Users\Owner\.openclaw\workspace\imperium\.env.local)  
✅ Cloudflare access (for DNS)

---

## STEP 1: Push to GitHub

```powershell
cd C:\Users\Owner\.openclaw\workspace\imperium

# Add .env.local to git (Railway will read it)
git add .env.local

# Commit all changes
git add .
git commit -m "Ready for Railway deployment - June 16"

# Push to main
git push origin main
```

---

## STEP 2: Create Railway Account & Project

1. Go to **https://railway.app**
2. Click **"Sign up"** (or sign in)
3. Click **"Continue with GitHub"** (connect your GitHub)
4. Authorize Railway
5. Click **"Create New Project"**
6. Select **"Deploy from GitHub"**
7. Find + click **imperium-foundry** repo
8. Railway auto-detects it's a Next.js app
9. Click **Deploy**

---

## STEP 3: Watch the Build

Railway will:
1. Clone your GitHub repo
2. Install dependencies (`npm install`)
3. Build the app (`npm run build`)
4. Create PostgreSQL database
5. Provision HTTPS certificate
6. Deploy to production

**Status:** Watch the Deployments tab in Railway (takes 2-5 minutes)

---

## STEP 4: Set Environment Variables in Railway

While it's building, go to:

**Railway dashboard** → Your project → **Variables** tab

Add these three:

| Key | Value |
|-----|-------|
| NEXTAUTH_SECRET | jK8mP2vL5nQ9rT3wX7yA1bC4dE6fG0hI3jK5mN7pR0sT2uV4wX6yZ8aB0cD2eF4gH6iJ |
| NEXTAUTH_URL | https://imperiumfoundry.com |
| NODE_ENV | production |

Click **Save**

(PostgreSQL details are auto-set, don't touch those)

---

## STEP 5: Connect Domain

Once deployed:

1. In Railway, click **Domains**
2. Click **"Add Domain"**
3. Enter: `imperiumfoundry.com`
4. Railway generates a CNAME: `cname-value.railway.app`

Now go to Cloudflare:

1. Login to **https://dash.cloudflare.com**
2. Click **imperiumfoundry.com** domain
3. Click **DNS** tab
4. Click **"Add record"**
   - Type: **CNAME**
   - Name: `imperium-foundry` (or just `@` for root)
   - Target: (paste Railway's CNAME value)
   - Proxied: **Proxied** (orange cloud)
   - TTL: Auto
5. Save

**Wait 5-10 minutes for DNS to propagate.**

---

## STEP 6: Test It

Visit **https://imperiumfoundry.com**

You should see:
- ✅ Landing page loads
- ✅ No SSL warnings (https://)
- ✅ Can click around the site

---

## If Deployment Failed

Check Railway logs:

1. Railway dashboard → Your project
2. Click **Logs** tab
3. Look for red error messages
4. Common issues:
   - `npm install` failed → check package.json
   - Missing env variables → add to Variables tab
   - Database connection failed → Railway auto-creates it

---

## If DNS Doesn't Work

1. Verify Cloudflare CNAME is correct (copy-paste from Railway)
2. Wait 15 minutes (DNS takes time)
3. Try in incognito mode (clear browser cache)
4. Check Cloudflare DNS resolves: `nslookup imperiumfoundry.com`

---

## Auto-Deploy on Future Pushes

Railway auto-redeploys whenever you push to GitHub:

```powershell
# Make a change
# ...

git add .
git commit -m "Update something"
git push origin main

# Railway automatically redeploys (~1-2 min)
```

---

## You're Done!

imperiumfoundry.com is now live.

Next steps:
1. Create test account
2. Test listings upload
3. Invite beta users
4. Monitor Railway logs for errors

---

