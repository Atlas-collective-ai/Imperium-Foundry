# Imperium Foundry - Deployment Guide

## Quick Start (Local)

### With Docker

```bash
# Build and run with docker-compose
docker-compose up

# In another terminal, setup database
docker-compose exec app npx prisma db push

# Access at http://localhost:3000
```

### Without Docker

```bash
# Install dependencies
npm install

# Create .env.local
cp .env.example .env.local

# Setup PostgreSQL locally and update DATABASE_URL

# Run database migrations
npx prisma db push

# Start dev server
npm run dev
```

---

## Deploy to Production

### Option 1: Vercel (Recommended for Frontend)

Vercel handles frontend automatically with zero-config.

```bash
# 1. Push code to GitHub
git push origin main

# 2. Go to vercel.com
# 3. Import your GitHub repo
# 4. Add environment variables:
#    - NEXTAUTH_SECRET (generate: openssl rand -base64 32)
#    - NEXTAUTH_URL=https://yourdomai.com
#    - DATABASE_URL=<from Railway/Render>
#    - GITHUB_ID, GITHUB_SECRET (for OAuth)

# 5. Deploy!
```

### Option 2: Railway (Full Stack - Easiest)

Railway handles both frontend and backend.

```bash
# 1. Sign up at railway.app
# 2. Create new project
# 3. Connect GitHub repo
# 4. Add PostgreSQL service:
#    - Create new > PostgreSQL
#    - Copy DATABASE_URL

# 5. Deploy app service:
#    - Create new > GitHub repo
#    - Select this imperium repo
#    - Add root directory: /
#    - Build command: npm run build
#    - Start command: npm start
#    - Add environment variables

# 6. Done! Railway auto-deploys on push
```

### Option 3: Render + Supabase

**Frontend on Render, Database on Supabase**

**Database (Supabase):**
```bash
# 1. Go to supabase.com
# 2. Create new project
# 3. Go to Settings > Database > Connection String
# 4. Copy PostgreSQL connection string
# 5. Update DATABASE_URL in your backend
```

**Frontend (Render):**
```bash
# 1. Go to render.com
# 2. Create new > Web Service
# 3. Connect GitHub repo
# 4. Settings:
#    - Build command: npm run build
#    - Start command: npm start
#    - Root directory: (leave blank if root)

# 5. Add Environment variables:
#    - All from .env.example
#    - NEXTAUTH_URL=https://<render-domain>.onrender.com

# 6. Deploy!
```

---

## Environment Variables (Production)

Must set in your hosting platform:

```env
# Critical
DATABASE_URL="postgresql://user:pass@host:5432/imperium"
NEXTAUTH_SECRET="<generate: openssl rand -base64 32>"
NEXTAUTH_URL="https://imperiumfoundry.com"

# Auth Providers
GITHUB_ID="<from GitHub OAuth app>"
GITHUB_SECRET="<from GitHub OAuth app>"

# Optional (for future features)
STRIPE_PUBLIC_KEY=""
STRIPE_SECRET_KEY=""
SENDGRID_API_KEY=""
AWS_ACCESS_KEY_ID=""
AWS_SECRET_ACCESS_KEY=""
```

### Generate NEXTAUTH_SECRET

```bash
# On macOS/Linux
openssl rand -base64 32

# On Windows PowerShell
[Convert]::ToBase64String((1..32 | ForEach-Object { [byte](Get-Random -Maximum 256) }))
```

---

## Domain Setup

### Connect Custom Domain

**For Vercel:**
```bash
# 1. Go to Project > Settings > Domains
# 2. Add imperiumfoundry.com
# 3. Update DNS records:
#    A      imperiumfoundry.com    76.76.19.165
#    CNAME  www                    cname.vercel-dns.com
```

**For Railway:**
```bash
# 1. Go to your service
# 2. Click "Public Networking"
# 3. Add custom domain
# 4. Update DNS CNAME
```

---

## Database Migrations (Production)

Always run migrations before deploying code changes:

```bash
# Locally
npx prisma db push

# Or on your hosting platform's shell
railway run npx prisma db push    # Railway
render exec npx prisma db push    # Render
```

---

## SSL/HTTPS

All major hosting platforms auto-handle SSL:
- ✅ Vercel: Automatic
- ✅ Railway: Automatic
- ✅ Render: Automatic

No additional setup needed.

---

## Monitoring & Logs

### Vercel
- Dashboard > Deployments > view logs
- Real-time logs in project settings

### Railway
- Logs tab shows real-time output
- Metrics tab for performance

### Render
- Logs tab shows application output
- Metrics for CPU, memory, bandwidth

---

## Troubleshooting

### "Database connection failed"
- Check DATABASE_URL is correct
- Ensure database is running
- Check firewall rules allow connection

### "NEXTAUTH_SECRET not set"
- Must be set in production environment
- Generate: `openssl rand -base64 32`
- Add to hosting platform's env vars

### "Build fails"
- Check `npm run build` works locally
- Check all dependencies installed
- Review build logs in hosting platform

### "Page shows 500 error"
- Check application logs
- Verify DATABASE_URL is accessible
- Check .env variables are set correctly

---

## Performance Optimization

### Database
```bash
# Add indexes for common queries
npx prisma db push
```

### Frontend
- Next.js auto-optimizes images
- Tailwind CSS purges unused styles
- Enable compression in host

### Caching
- Set HTTP cache headers
- Use Redis (optional)

---

## Backup Strategy

### Supabase
- Automatic daily backups
- 7-day retention by default

### Railway
- Persistent volumes for data
- Can export database snapshots

### Manual Backup
```bash
# Export PostgreSQL
pg_dump $DATABASE_URL > backup.sql

# Restore
psql $DATABASE_URL < backup.sql
```

---

## Scaling

### When traffic increases:
1. **Database**: Upgrade PostgreSQL tier
2. **Frontend**: Vercel handles auto-scaling
3. **API**: Add caching, optimize queries

### Add caching:
```bash
# Install redis
npm install redis

# Use in API routes for frequently queried data
```

---

## Security Checklist

- [x] NEXTAUTH_SECRET is set (32+ chars)
- [x] Database connection is encrypted (SSL)
- [ ] GitHub OAuth secrets are secure
- [ ] No sensitive data in logs
- [ ] CORS is properly configured
- [ ] Database backups are automated
- [ ] API rate limiting is configured

---

## Support

- Docs: imperiumfoundry.com/docs
- Email: hello@imperiumfoundry.com
- GitHub Issues: github.com/imperiumfoundry/platform

---

**Last Updated:** June 15, 2026
