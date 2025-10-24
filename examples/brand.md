# Unified Brand and Meta Tags

Whenever code changes affect web applications in this repository, ensure consistent brand across all meta tags, titles, and descriptions.

## Official Brand

**Tagline**: "Gitar.ai is CI for the age of AI"

**Description**: "Bring AI to your CI. Automatically fix broken builds and address review feedback so you can merge more code, faster"

**Site Name**: "Gitar.ai"

## When to Apply This Rule

Review and ensure brand consistency when changes involve:

- New Next.js layouts (`layout.tsx` files)
- New Next.js pages with metadata exports (`page.tsx` files with `export const metadata`)
- New HTML files (`index.html`, `*.html`)
- Web manifest files (`site.webmanifest`, `manifest.json`)
- React app configuration files
- Any new web application directories under `apps/`

## Required Meta Tag Standards

### For Next.js Metadata Exports

All `layout.tsx` and `page.tsx` files with metadata exports must include:

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

**Note**: Page-specific titles can have a prefix (e.g., "Repositories - Gitar.ai") but must always include "Gitar.ai" and should use the standard description.

### For HTML Files

All HTML files must include these meta tags in the `<head>`:

```html
<meta name="description" content="Bring AI to your CI. Automatically fix broken builds and address review feedback so you can merge more code, faster" />
<title>Gitar.ai - CI for the age of AI</title>
```

### For Web Manifest Files

All `site.webmanifest` and `manifest.json` files must include:

```json
{
  "name": "Gitar.ai - CI for the age of AI",
  "short_name": "Gitar.ai",
  "description": "Bring AI to your CI. Automatically fix broken builds and address review feedback so you can merge more code, faster"
}
```

## Actions

When changes are detected in web application files:

1. **Check for consistency**: Verify that all meta tags, titles, and descriptions match the official brand
2. **Add inline comments with suggestions**: If inconsistent brand is found, add an inline comment on the specific line with:
   - Reference to this rule file (`.gitar/rules/brand.md`)
   - A code suggestion showing the correct brand text to use
   - Brief explanation of why consistent brand matters (SEO, social sharing, search engines, Slack/Discord previews)

## Why This Matters

Consistent brand ensures:
- **Search engines** display accurate information in search results
- **Social media platforms** (LinkedIn, Twitter/X, Facebook) render proper preview cards when links are shared
- **Messaging apps** (Slack, Discord, Teams) show professional link previews
- **Browser tabs** and bookmarks display unified brand
- **Mobile installs** (PWA) show consistent app names and descriptions
