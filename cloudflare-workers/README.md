# Cloudflare Workers - Deployment Setup

This directory contains Cloudflare Workers that are automatically deployed via GitHub Actions.

## Structure

Each worker lives in its own subdirectory with:
- `worker.js` - The worker code
- `wrangler.toml` - Configuration file

## Setup Instructions

### 1. Install Wrangler CLI (Optional - for local development)

```bash
npm install -g wrangler
```

### 2. Configure GitHub Secrets

Add these secrets to your GitHub repository at Settings > Secrets and variables > Actions:

- `CLOUDFLARE_API_TOKEN` - Create at https://dash.cloudflare.com/profile/api-tokens
  - Use the "Edit Cloudflare Workers" template
  - Or create custom token with these permissions:
    - Account > Workers Scripts > Edit
    - Account > Account Settings > Read

- `CLOUDFLARE_ACCOUNT_ID` - Your Cloudflare account ID

## Automatic Deployment

The GitHub Actions workflow (`.github/workflows/deploy-cloudflare-workers.yml`) will automatically deploy workers when:

1. Changes are pushed to the `main` branch
2. Files in `cloudflare-workers/**` are modified
3. The workflow is manually triggered

Each worker is deployed independently - only workers with changed files are deployed.

## Manual Deployment

To deploy manually from your local machine:

```bash
cd cloudflare-workers/your-worker-name
wrangler deploy --env production
```

You'll need to authenticate with Wrangler first:
```bash
wrangler login
```

## Adding New Workers

To add a new worker to this monorepo:

1. Create a new directory: `cloudflare-workers/your-worker-name/`
2. Add your worker code as `worker.js`
3. Create a `wrangler.toml` configuration
4. Add a new job to `.github/workflows/deploy-cloudflare-workers.yml`
5. Push to main branch

## Local Development

To test a worker locally:

```bash
cd cloudflare-workers/your-worker-name
wrangler dev
```

This starts a local server at http://localhost:8787
