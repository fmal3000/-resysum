# RESYSUM Dashboard - Vercel Deployment Guide

This guide will walk you through deploying the RESYSUM Real Estate Wholesaling Dashboard to Vercel with your custom domain `resystum.me`.

## 📋 Pre-Deployment Checklist

### ✅ Prerequisites
- [ ] GitHub account with repository access
- [ ] Vercel account (free tier works)
- [ ] Custom domain `resystum.me` with DNS access
- [ ] Neon PostgreSQL database (or other PostgreSQL provider)

### ✅ Required Environment Variables
Make sure you have the following values ready:

- [ ] `DATABASE_URL` - PostgreSQL connection string
- [ ] `NEXTAUTH_SECRET` - Random secret key (generate with: `openssl rand -base64 32`)
- [ ] `NEXTAUTH_URL` - Your production URL (`https://resystum.me`)

## 🚀 Step-by-Step Deployment

### Step 1: Database Setup

#### Option A: Create Neon Database (Recommended)
1. Go to [Neon Console](https://console.neon.tech)
2. Create a new project named "resysum-prod"
3. Select region: `aws-us-east-1`
4. Copy the connection string (it will look like):
   ```
   postgresql://username:password@ep-xxx.us-east-1.aws.neon.tech/neondb?sslmode=require
   ```

#### Option B: Use Existing PostgreSQL
- Ensure your PostgreSQL database supports SSL connections
- Connection string format: `postgresql://user:password@host:port/database?sslmode=require`

### Step 2: GitHub Repository Setup

1. **Initialize Git repository** (if not already done):
   ```bash
   cd /home/ubuntu/resysum-dashboard
   git init
   git add .
   git commit -m "Initial RESYSUM Dashboard commit"
   ```

2. **Create GitHub repository**:
   - Go to GitHub and create a new repository named `resysum-dashboard`
   - Don't initialize with README (we already have files)

3. **Push to GitHub**:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/resysum-dashboard.git
   git branch -M main
   git push -u origin main
   ```

### Step 3: Vercel Deployment

1. **Connect to Vercel**:
   - Go to [vercel.com](https://vercel.com)
   - Sign in with your GitHub account
   - Click "New Project"
   - Import your `resysum-dashboard` repository

2. **Configure Project Settings**:
   - **Framework Preset**: Next.js (auto-detected)
   - **Root Directory**: `app` (important!)
   - **Build Command**: `npm run build` (default)
   - **Output Directory**: `.next` (default)
   - **Install Command**: `npm install --legacy-peer-deps`

3. **Set Environment Variables**:
   In Vercel dashboard → Settings → Environment Variables, add:

   | Variable | Value | Environment |
   |----------|-------|-------------|
   | `DATABASE_URL` | Your PostgreSQL connection string | Production, Preview |
   | `NEXTAUTH_SECRET` | Generate with `openssl rand -base64 32` | Production, Preview |
   | `NEXTAUTH_URL` | `https://resystum.me` | Production |
   | `NEXTAUTH_URL` | `https://your-preview-url.vercel.app` | Preview |
   | `NODE_ENV` | `production` | Production |

4. **Deploy**:
   - Click "Deploy"
   - Wait for build to complete (2-5 minutes)
   - Your app will be available at `https://your-project.vercel.app`

### Step 4: Custom Domain Configuration

1. **Add Domain in Vercel**:
   - Go to Project → Settings → Domains
   - Add `resystum.me` and `www.resystum.me`

2. **Configure DNS**:
   Add these DNS records to your domain provider:

   | Type | Name | Value |
   |------|------|-------|
   | A | @ | 76.76.19.19 |
   | CNAME | www | cname.vercel-dns.com |

   **Alternative (if CNAME for root is supported)**:
   | Type | Name | Value |
   |------|------|-------|
   | CNAME | @ | cname.vercel-dns.com |
   | CNAME | www | cname.vercel-dns.com |

3. **Verify Domain**:
   - DNS propagation can take up to 48 hours
   - Check status in Vercel dashboard
   - SSL certificate will be automatically provisioned

### Step 5: Database Migration

After successful deployment:

1. **Run migrations** (automatic via build process):
   - Migrations run automatically during deployment
   - Check deployment logs to confirm success

2. **Verify database connection**:
   - Visit your deployed app
   - Check that the dashboard loads without database errors

## 🔧 Post-Deployment Configuration

### Continuous Deployment
- Every push to `main` branch automatically deploys to production
- Pull requests create preview deployments
- Preview deployments use separate preview environment variables

### Monitoring & Analytics
1. **Vercel Analytics** (built-in):
   - Go to Project → Analytics
   - Monitor page views, performance metrics

2. **Error Monitoring**:
   - Check Function Logs in Vercel dashboard
   - Set up error tracking (Sentry recommended)

### Performance Optimization
- Images are automatically optimized via Next.js Image component
- Static assets are served via Vercel's global CDN
- API routes run as serverless functions

## 🛠️ Troubleshooting

### Common Issues

#### Build Failures
```bash
# If build fails due to dependencies
npm install --legacy-peer-deps
npm run build
```

#### Database Connection Issues
- Verify `DATABASE_URL` is correctly set
- Ensure database allows connections from Vercel IPs
- Check SSL requirements (`?sslmode=require`)

#### Environment Variable Issues
- Variables must be set in Vercel dashboard, not in code
- Restart deployment after changing environment variables
- Use different values for Production vs Preview environments

#### Domain Configuration Issues
- DNS changes can take up to 48 hours to propagate
- Use DNS checker tools to verify records
- Ensure no conflicting DNS records exist

### Verification Commands

Run these locally to verify everything is configured correctly:

```bash
cd app

# Check environment variables
npm run deploy-check

# Test database connection
npx prisma db execute --stdin <<< "SELECT 1;"

# Verify build works
npm run build

# Check for security issues
npm audit
```

## 📊 Features Included

Your deployed RESYSUM Dashboard includes:

- ✅ **Property Management**: Add, analyze, and track properties
- ✅ **Lead Management**: Manage seller leads and follow-ups
- ✅ **SMS & Email Campaigns**: Automated marketing tools
- ✅ **Contract Management**: Generate and track contracts
- ✅ **Buyer Database**: Manage potential buyers
- ✅ **Analytics Dashboard**: Performance metrics and insights
- ✅ **Activity Tracking**: Complete audit trail
- ✅ **Responsive Design**: Works on all devices
- ✅ **Secure Authentication**: Protected user accounts
- ✅ **Real-time Updates**: Live data synchronization

## 🔐 Security Features

- SSL/TLS encryption (automatic via Vercel)
- Environment variable protection
- SQL injection prevention (Prisma ORM)
- XSS protection headers
- CSRF protection
- Secure session management

## 📈 Scaling Considerations

- **Database**: Neon automatically scales with usage
- **Serverless Functions**: Auto-scale with traffic
- **CDN**: Global edge caching for optimal performance
- **Monitoring**: Built-in performance metrics

## 🆘 Support

If you encounter issues:

1. Check Vercel deployment logs
2. Verify environment variables are set correctly
3. Ensure database is accessible
4. Review DNS configuration for custom domain

## 🎉 Success!

Once deployed, your RESYSUM Dashboard will be available at:
- **Production**: https://resystum.me
- **Vercel URL**: https://your-project.vercel.app
