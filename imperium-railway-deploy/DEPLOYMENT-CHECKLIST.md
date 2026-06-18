# Imperium Foundry Deployment Checklist

**Status:** Ready to deploy  
**Date started:** June 16, 2026  
**Target date:** June 16, 2026 (today)  

---

## Pre-Deployment (DONE ✅)

- [x] .env.local created with secrets
- [x] NextAuth secret generated
- [x] Code committed to GitHub
- [x] Railway config ready (railway.json)
- [x] Deployment guides written
- [x] DNS planning complete

---

## Deployment Phase (DO THIS NOW)

### Step 1: Git Push ⏳
- [ ] cd to imperium folder
- [ ] Run: `git push origin main`
- [ ] Verify push succeeded (no errors)

**Time: 30 seconds**

---

### Step 2: Railway Setup ⏳
- [ ] Go to https://railway.app
- [ ] Sign up or login with GitHub
- [ ] Click "Create New Project"
- [ ] Select "Deploy from GitHub"
- [ ] Find imperium-foundry repo
- [ ] Click Deploy
- [ ] Watch build progress (2-5 min)

**Time: 3-5 minutes**

---

### Step 3: Environment Variables ⏳
In Railway dashboard, Variables tab:

- [ ] NEXTAUTH_SECRET = `jK8mP2vL5nQ9rT3wX7yA1bC4dE6fG0hI3jK5mN7pR0sT2uV4wX6yZ8aB0cD2eF4gH6iJ`
- [ ] NEXTAUTH_URL = `https://imperiumfoundry.com`
- [ ] NODE_ENV = `production`
- [ ] Click Save

**Time: 1 minute**

---

### Step 4: Domain Connection ⏳
In Railway:
- [ ] Click Domains
- [ ] Add Domain: `imperiumfoundry.com`
- [ ] Copy CNAME value from Railway

In Cloudflare:
- [ ] Login to dash.cloudflare.com
- [ ] Select imperiumfoundry.com
- [ ] Go to DNS
- [ ] Add CNAME record:
  - Name: `imperium-foundry`
  - Target: `[paste Railway value]`
  - Proxied: `✅ (orange cloud)`
  - Save
- [ ] Wait 5-10 minutes for propagation

**Time: 2 minutes + 10 min DNS wait**

---

## Live Testing (AFTER DEPLOYMENT)

Once imperiumfoundry.com is live, physically audit:

### Access Test
- [ ] Visit https://imperiumfoundry.com
- [ ] Page loads without errors
- [ ] No 404 or 502 errors

### Security Test
- [ ] URL shows `https://` (not `http://`)
- [ ] No SSL certificate warnings
- [ ] Lock icon appears in browser

### Functionality Test
- [ ] Can see home page
- [ ] Can navigate menu
- [ ] Can click "Post Idea" button
- [ ] Form loads without errors

### Account Test
- [ ] Click "Sign Up"
- [ ] Create test account (test@imperium.com)
- [ ] Receive confirmation (check email)
- [ ] Can log in with new account
- [ ] Dashboard loads

### Listing Test
- [ ] Click "Post an Idea"
- [ ] Fill in required fields (title, problem, email)
- [ ] Click Submit
- [ ] Listing submitted successfully
- [ ] Get confirmation message
- [ ] Can see listing in draft

### Database Test
- [ ] Log out
- [ ] Log back in with same account
- [ ] Account still exists (database is persisting)
- [ ] Can see previously submitted listing

---

## Post-Deployment (AFTER LIVE AUDIT)

- [ ] Screenshot of imperiumfoundry.com live
- [ ] Screenshot of test account created
- [ ] Note any errors or issues
- [ ] Message @Atlascronosbot with status

---

## Troubleshooting

**If site shows 404:**
- [ ] DNS might not be propagated yet (wait 15 min)
- [ ] Try incognito mode (clear cache)
- [ ] Check CNAME is correct in Cloudflare

**If can't create account:**
- [ ] Check NEXTAUTH_SECRET is set in Railway Variables
- [ ] Check NEXTAUTH_URL is set
- [ ] Check database is running (Railway auto-creates it)

**If build failed:**
- [ ] Check Railway Logs tab
- [ ] Look for npm install errors
- [ ] Verify package.json dependencies

**If environment errors:**
- [ ] Make sure all three variables are in Railway Variables:
  - NEXTAUTH_SECRET
  - NEXTAUTH_URL
  - NODE_ENV
- [ ] No typos
- [ ] No missing values

---

## Success Metrics

You're done when:

✅ imperiumfoundry.com loads  
✅ HTTPS is active  
✅ Can create account  
✅ Can submit listing  
✅ Database persists data  
✅ No error messages  

---

## Quick Links

- Railway Dashboard: https://railway.app
- Cloudflare DNS: https://dash.cloudflare.com
- GitHub Repo: https://github.com/[YOUR_USERNAME]/imperium-foundry
- imperiumfoundry.com: https://imperiumfoundry.com

---

**Estimated Total Time: 15-20 minutes** (mostly waiting for DNS)

When done, message: "imperiumfoundry.com is LIVE"

