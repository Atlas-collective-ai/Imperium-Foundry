# Deploy Imperium Foundry NOW (5 Steps)

**Status:** Ready to deploy immediately  
**Time to live:** 5-10 minutes  
**Cost:** $5-15/month

---

## STEP 1: Generate NextAuth Secret

Run this command (pick your OS):

**Mac/Linux:**
```bash
openssl rand -base64 32
```

**Windows (PowerShell):**
```powershell
[System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes((New-Guid).ToString().Replace("-", ""))) | Select-Object -First 32 -Wait
```

**Output:** Copy the 32-character secret (example: `s8F9K3b2Lm9P1Q4R5S6T7U8V9W0X1Y2Z`)

---

## STEP 2: Create .env.local

In `imperium/` folder, create `.env.local`:

```env
NEXTAUTH_SECRET=s8F9K3b2Lm9P1Q4R5S6T7U8V9W0X1Y2Z
NEXTAUTH_URL=https://imperiumfoundry.com
```

(Replace the secret with your generated one from Step 1)

---

## STEP 3: Push to GitHub

```bash
cd C:\Users\Owner\.openclaw\workspace\imperium

git add .
git commit -m "Deploy to Railway"
git push origin main
```

(If you haven't set up GitHub yet:)
1. Go to https://github.com/new
2. Name it: `imperium-foundry`
3. Create repo
4. Run these:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/imperium-foundry.git
   git branch -M main
   git push -u origin main
   ```

---

## STEP 4: Connect to Railway (5 min)

1. Go to **https://railway.app** (create account if needed)
2. Click **"Create New Project"**
3. Select **"Deploy from GitHub"**
4. Find + select **imperium-foundry**
5. Click **Deploy**

**While Railway deploys:**

Go to Railway project > **Variables** tab

Add these three:
```
NEXTAUTH_SECRET = (paste your secret from Step 1)
NEXTAUTH_URL = https://imperiumfoundry.com
NODE_ENV = production
```

Click **Deploy** (if not auto-deploying)

---

## STEP 5: Connect Domain (2 min)

1. In Railway, click **Domains**
2. Click **"Add Domain"**
3. Type: `imperiumfoundry.com`
4. Railway shows you a CNAME record
5. Go to **Cloudflare** (your domain provider)
   - Log in → imperiumfoundry.com
   - Click **DNS**
   - Click **"Add record"**
   - Type: **CNAME**
   - Name: `imperium-foundry`
   - Target: (paste Railway's DNS endpoint)
   - Proxied: **Proxied** (orange cloud)
   - Save

**Wait 5 min for DNS propagation.**

---

## DONE ✅

Visit **https://imperiumfoundry.com**

You should see the landing page.

---

## Test It Works

1. **Can you see the homepage?**
   - ✅ Great, it's live

2. **Try creating an account** (test)
   - ✅ If it works, database is connected

3. **Check HTTPS**
   - Should be `https://` (not `http://`)
   - No warnings

---

## If Something Fails

**Build failed?**
- Go to Railway > Logs tab
- Look for error message
- Most common: Missing env variable in Railway
- Solution: Add it to Variables tab + redeploy

**Site shows 404?**
- DNS not propagated yet
- Wait 10 more minutes + try again
- Or check Cloudflare DNS is correct

**Can't sign in?**
- NEXTAUTH_SECRET not set in Railway Variables
- Add it + redeploy

**Database error?**
- Railway auto-creates PostgreSQL
- Migrations should auto-run
- If not: Go to Railway > Terminal, run: `npx prisma db push`

---

## Next: Upload Sample Listings

Once live, populate with 10-20 sample listings:

```bash
# Seed with sample data (we provide SQL)
psql $DATABASE_URL < scripts/seed.sql
```

Then invite first 50 beta users.

---

## Deploy Again (Later)

Just push to GitHub:

```bash
# Make a change
git add .
git commit -m "Your message"
git push origin main
```

Railway auto-redeploys (~1-2 minutes).

---

**Questions?** See `DEPLOYMENT-RAILWAY.md` for detailed guide.

