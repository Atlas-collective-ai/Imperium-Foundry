# Imperium Platform — Railway Deployment

**Quick start for deploying Imperium on Railway**

## What's Included

- Dockerfile + docker-compose.yml (containerized setup)
- Next.js configuration (frontend)
- Environment setup (.env.example)
- Railway deployment config (railway.json)
- Tailwind CSS + TypeScript config
- Deployment checklists and instructions

## Quick Deploy to Railway

1. **Clone repo to your local machine**
   ```bash
   git clone https://github.com/AmitKChawla/imperium.git
   cd imperium-railway-deploy
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up environment variables**
   ```bash
   cp .env.example .env.local
   # Edit .env.local with your values
   ```

4. **Deploy to Railway**
   - Option A: Use Railway CLI
     ```bash
     railway login
     railway up
     ```
   - Option B: Connect GitHub repo to Railway (recommended)
     - Go to railway.app
     - Create new project
     - Connect GitHub repo
     - Railway auto-deploys on push

## Environment Variables

See `.env.example` for required variables.

Key variables:
- `DATABASE_URL` - Database connection string
- `NEXT_PUBLIC_API_URL` - API endpoint URL
- `STRIPE_KEY` - Payment processor key (if using)

## Documentation

- `DEPLOYMENT-RAILWAY.md` - Step-by-step Railway deployment
- `DEPLOYMENT.md` - General deployment instructions
- `DEPLOYMENT-CHECKLIST.md` - Pre-deployment checklist
- `DEPLOY-NOW.md` - Quick deployment guide
- `RAILWAY-DEPLOY-INSTRUCTIONS.md` - Railway-specific setup

## Project Structure

```
.
├── pages/              - Next.js pages (frontend routes)
├── public/             - Static assets
├── styles/             - CSS/Tailwind styles
├── Dockerfile          - Docker container spec
├── docker-compose.yml  - Multi-service setup (local dev)
├── railway.json        - Railway deployment config
├── next.config.js      - Next.js configuration
├── tsconfig.json       - TypeScript configuration
├── postcss.config.js   - PostCSS configuration
├── tailwind.config.js  - Tailwind CSS configuration
└── package.json        - Dependencies and scripts
```

## Running Locally

### Docker Compose (recommended)
```bash
docker-compose up
```

### Direct Node
```bash
npm run dev
```

App will be available at `http://localhost:3000`

## Support

For deployment issues, check:
1. DEPLOYMENT-CHECKLIST.md - Common issues
2. RAILWAY-DEPLOY-INSTRUCTIONS.md - Railway-specific help
3. .env.example - Required environment variables

---

**Ready to deploy. Push to GitHub and connect to Railway.** 🚀
