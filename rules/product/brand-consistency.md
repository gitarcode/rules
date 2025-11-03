---
title: "Brand Consistency"
description: "Enforce unified brand messaging across web metadata, titles, and descriptions"
when: "Changes to Next.js layouts/pages with metadata exports, HTML files, or web manifest files"
actions: "Post inline code suggestions with corrected brand text"
---

# Unified Brand and Meta Tags

Ensure consistent brand across all meta tags, titles, and descriptions in web applications.

## When to Use This

Apply when changes involve:
- New Next.js layouts (`layout.tsx` files)
- New Next.js pages with metadata exports (`page.tsx` files with `export const metadata`)
- New HTML files (`index.html`, `*.html`)
- Web manifest files (`site.webmanifest`, `manifest.json`)
- React app configuration files
- Any new web application directories under `apps/`

## How It Works

**Official Brand Reference:**
- **Tagline**: "Gitar.ai is CI for the age of AI"
- **Description**: "Bring AI to your CI. Automatically fix broken builds and address review feedback so you can merge more code, faster"
- **Site Name**: "Gitar.ai"

1. **Verify Next.js metadata exports** match official brand:
   ```typescript
   export const metadata: Metadata = {
     title: "Gitar.ai - CI for the age of AI",
     description: "Bring AI to your CI. Automatically fix broken builds and address review feedback so you can merge more code, faster",
     openGraph: {
       title: "Gitar.ai - CI for the age of AI",
       description: "Bring AI to your CI. Automatically fix broken builds and address review feedback so you can merge more code, faster",
       siteName: "Gitar.ai",
       type: "website",
       locale: "en_US",
     },
     twitter: {
       card: "summary_large_image",
       title: "Gitar.ai - CI for the age of AI",
       description: "Bring AI to your CI. Automatically fix broken builds and address review feedback so you can merge more code, faster",
     },
   };
   ```
   **Note**: Page-specific titles can have a prefix (e.g., "Repositories - Gitar.ai") but must always include "Gitar.ai"

2. **Check HTML files** include proper meta tags:
   ```html
   <meta name="description" content="Bring AI to your CI. Automatically fix broken builds and address review feedback so you can merge more code, faster" />
   <title>Gitar.ai - CI for the age of AI</title>
   ```

3. **Validate web manifest files**:
   ```json
   {
     "name": "Gitar.ai - CI for the age of AI",
     "short_name": "Gitar.ai",
     "description": "Bring AI to your CI. Automatically fix broken builds and address review feedback so you can merge more code, faster"
   }
   ```

4. **Post inline comments** if inconsistent brand found:
   - Show code suggestion with correct brand text
   - Explain importance (SEO, social sharing, search engines, Slack/Discord previews)

## Why This Matters

- **Search engines** display accurate information in search results
- **Social media platforms** (LinkedIn, Twitter/X, Facebook) render proper preview cards when links are shared
- **Messaging apps** (Slack, Discord, Teams) show professional link previews
- **Browser tabs** and bookmarks display unified brand
- **Mobile installs** (PWA) show consistent app names and descriptions
