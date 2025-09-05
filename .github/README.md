# opennext-cloudflare-workers

A lightweight, high-performance Next.js 15 template optimized for Cloudflare Workers.

Features:

- Next.js 15 App Router
- Cloudflare Workers (via opennextjs-cloudflare adapter)
- Tailwind v4
- shadcn/ui components
- Biome for linting & formatting with AI-optimized settings via Ultracite
- lefthook configured with biome + commitizen + commitlint on commit
- VSCode settings/extensions for optimal development experience

This allows you to utilize Cloudflare's global edge network for low-latency delivery of your Next.js application, while also benefiting from modern development tools and practices.

All for free.

## Known Limitations

- All the limitations of [opennextjs-cloudflare](https://opennext.js.org/cloudflare/known-issues) apply.
- You need to setup Cloudflare R2 for caching with `revalidatePath`, ISR, and `fetch()` cache.
  - I recommend not doing that and instead opt for no Next.js magic via `axios` and not use `ISR`.
- Your standard way of connecting to databases via long-running connections are not supported.
  - Refer to [opennextjs-cloudflare database connection How-To Guide](https://opennext.js.org/cloudflare/howtos/db) for alternatives.
- The Cloudflare Free Plan only supports up to `3 MiB` (gzipped) for server bundles.
  - This template outputs a `1041.62 KiB` gzipped server bundle, which is well within the limit.
  - Upgrading to the Cloudflare Workers Pro plan will allow you to deploy up to `10 MiB` (gzipped) apps.

## Initial Setup

> [!Caution]
> Please review this setup guide before working on the project.

### 🏗️ Setup

```bash
pnpm i         # installs dependencies
pnpm dev       # starts the development server
pnpm build     # builds the application
pnpm start     # starts the production server after running `pnpm build`
pnpm preview   # builds & previews the production build (only works on Unix environment)
```

### 🧼 Linting/Formatting

```bash
pnpm check     # checks code formatting and linting using Biome
pnpm fix       # automatically fixes issues found by Biome (uses --unsafe flag)
```

### 🤖 Setting Up Agentic Coding

1. Edit the context in `.github/copilot-instructions.md` to fit your project's description.
2. When prompting, add it as context.

### 🧩 Adding shadcn Components

You can add shadcn components using the following command:

```bash
pnpm dlx shadcn add button
```

It will be created in the `src/components/ui` folder - for shared UI components.

### 🤛 Setting Up Lefthook Manually

It should already be set up if the `postinstall` script ran successfully. If you need to set it up manually, run:

```bash
pnpm dlx lefthook install
```

## Deploying on Cloudflare

1. Create a new Cloudflare Workers project in your Cloudflare dashboard and select your repository with this template.
2. Fill up the following fields:
   - **Project name**: This will be used as a reference to your `workers.dev` subdomain, ie. `your-project` will be `your-project.cf-account.workers.dev`.
   - **Build command**: `pnpm cf:build`
   - **Deploy command**: `pnpm cf:deploy`
   - **Builds for non-production branches**: Check this if you want to deploy branches other than `main` or `master`. This is useful for staging environments.

### Enabling Preview URLs

By default, Cloudflare does not enable preview URLs for GitHub repositories. To enable it:

1. Go to your Worker's project settings in the Cloudflare dashboard.
2. Under **Domains & Routes** press `Enable` for **Preview URLs**.
