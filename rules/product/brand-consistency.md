---
title: "Brand Consistency"
description: "Enforce unified brand messaging across web metadata, titles, and descriptions"
slug: "brand_consistency"
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

**Finding Official Brand Reference:**
1. First, check the repository's README.md for brand information:
   - Look for the project title (typically in the H1 heading)
   - Find the tagline or description (usually in the first few paragraphs or under the title)
   - Extract the official product/site name
   - Note any specific formatting preferences (e.g., capitalization, special characters)

2. If README.md doesn't contain clear brand messaging, check:
   - `package.json` (name, description fields)
   - Documentation site homepage
   - Marketing/landing page content
   - Existing metadata in current codebase

**Validating Brand Consistency:**

1. **Verify Next.js metadata exports** match official brand:
   ```typescript
   export const metadata: Metadata = {
     title: "[Product Name] - [Tagline]",
     description: "[Official product description from README]",
     openGraph: {
       title: "[Product Name] - [Tagline]",
       description: "[Official product description from README]",
       siteName: "[Product Name]",
       type: "website",
       locale: "en_US",
     },
     twitter: {
       card: "summary_large_image",
       title: "[Product Name] - [Tagline]",
       description: "[Official product description from README]",
     },
   };
   ```
   **Note**: Page-specific titles can have a prefix (e.g., "Features - [Product Name]") but must always include the product name

2. **Check HTML files** include proper meta tags:
   ```html
   <meta name="description" content="[Official product description from README]" />
   <title>[Product Name] - [Tagline]</title>
   ```

3. **Validate web manifest files**:
   ```json
   {
     "name": "[Product Name] - [Tagline]",
     "short_name": "[Product Name]",
     "description": "[Official product description from README]"
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
