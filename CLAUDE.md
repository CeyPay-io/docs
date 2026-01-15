# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Mintlify-based documentation site for **CeyPay WordPress Plugin**—a WooCommerce payment gateway that enables online stores to accept cryptocurrency payments (Binance Pay, Bybit, Bitazza) and receive settlements in Sri Lankan Rupees (LKR).

## Repository Structure

```
/
├── CLAUDE.md                 # This file - guidance for Claude Code
├── tone_of_voice.mdx         # Writing standards and style guide
├── docs.json                 # Mintlify configuration (navigation, theme, metadata)
├── index.mdx                 # Main landing page
├── faq.mdx                   # FAQ page
├── report-abuse.mdx          # Abuse reporting page
├── wordpress/                # WordPress/WooCommerce plugin documentation
│   ├── introduction.mdx      # Plugin overview and features
│   ├── quickstart.mdx        # Installation and setup guide
│   ├── configuration.mdx     # Settings reference
│   ├── webhooks.mdx          # Webhook technical reference
│   ├── testing.mdx           # Test mode and debugging guide
│   └── troubleshooting.mdx   # Common issues and solutions
├── images/                   # Documentation images and screenshots
└── logo/                     # CeyPay logo assets
```


## Development Commands

Mintlify provides a local development server for previewing documentation changes:

```bash
# Install Mintlify CLI (if not already installed)
npm i -g mintlify

# Start local development server
mintlify dev

# Preview on specific port
mintlify dev --port 3001
```

The dev server watches for file changes and hot-reloads automatically.

## Configuration

### docs.json

The `docs.json` file is the central configuration controlling:
- **Navigation structure**: Tabs, groups, and page hierarchy
- **Theme**: Colors, logo variants (light/dark mode)
- **Global anchors**: Homepage, GitHub links
- **Navbar links**: Support email, "Get Onboarded" CTA
- **Footer socials**: X, GitHub, LinkedIn, Facebook, Instagram

When adding new pages:
1. Create the `.mdx` file in the appropriate directory
2. Add the page path to `docs.json` navigation (without file extension)
3. Verify the navigation hierarchy matches the desired structure

### Page Structure

All documentation pages use MDX format with frontmatter:

```mdx
---
title: "Page Title"
description: "Brief description for SEO and previews"
---

[Content here using Mintlify components]
```

## Mintlify Components

The documentation uses Mintlify's built-in components:

- `<Card>`, `<CardGroup>`: Feature highlights, link cards
- `<Steps>`, `<Step>`: Sequential workflows
- `<Note>`, `<Warning>`, `<Tip>`: Callout boxes
- `<ResponseField>`: API/settings documentation
- `<CodeGroup>`: Multi-language code examples
- `<Accordion>`, `<AccordionGroup>`: Collapsible sections

Refer to existing files for usage patterns before creating new content.

## Content Guidelines

### Writing Standards

**IMPORTANT**: Before creating or editing any documentation, consult `tone_of_voice.mdx` for comprehensive writing standards. This file defines:
- Voice and tone (professional, direct, merchant-first)
- Writing structure (strong verbs, concise sentences, active voice)
- CeyPay-specific terminology and conventions
- Mintlify component usage patterns
- Code example standards
- Before/after examples for common documentation patterns

Key principles:
- Use active voice with strong verbs
- Target 15-20 words per sentence (max 30)
- Start steps with imperative verbs (Install, Configure, Navigate)
- Explain crypto concepts without assuming expertise
- Focus on business outcomes for merchants

### WordPress Plugin Documentation

The primary focus is documenting the **CeyPay WordPress Plugin** for WooCommerce merchants:

1. **Target audience**: Store owners with basic WordPress knowledge, no crypto expertise required
2. **Payment providers**: Binance Pay, Bybit, Bitazza (these are the three supported platforms)
3. **Key concepts**:
   - Webhook endpoint: `https://yoursite.com/?wc-api=WC_Gateway_CeyPay`
   - HMAC-SHA256 signature verification for webhooks
   - Test mode vs. production mode
   - Payment statuses: SUCCESS, PAID, EXPIRED, FAILED
4. **Requirements**: WordPress 5.8+, WooCommerce 7.0+, PHP 7.4+, SSL certificate

### Technical Accuracy

- **API endpoint**: Always reference the correct webhook endpoint format
- **Signature verification**: Webhook security uses HMAC-SHA256 with the merchant's webhook secret
- **Payment flow**: Customer selects CeyPay → Choose provider (Binance/Bybit/Bitazza) → QR code/deep link → Real-time verification
- **Order statuses**: Map payment statuses correctly (SUCCESS/PAID → Processing, FAILED/EXPIRED → Failed)

## Images and Assets

- Store images in `/images/` directory
- Logo variants in `/logo/` (light and dark mode versions)
- Reference images using root-relative paths: `/images/filename.png`
- Use `className="block dark:hidden"` and `className="hidden dark:block"` for theme-aware images

## Links and Navigation

- Internal links use relative paths: `/wordpress/quickstart`
- External links use full URLs: `https://ceypay.io`
- Support email: `support@ceypay.io`
- Onboarding: `https://ceypay.io/apply`
- Homepage: `https://ceypay.io`
- GitHub: `https://github.com/CeyPay-io`

## Common Tasks

### Adding a new documentation page

1. Create the `.mdx` file in the appropriate directory (root or `wordpress/`)
2. Add frontmatter with `title` and `description`
3. Update `docs.json` navigation to include the new page path
4. Use Mintlify components for consistent styling
5. Test locally with `mintlify dev`

### Updating navigation

Edit the `navigation` section in `docs.json`. The structure is hierarchical:
- Tabs → Groups → Pages
- Pages can be strings (file paths) or objects with nested groups
- Remove `.mdx` extension from paths in navigation config

### Modifying theme or branding

Update the following in `docs.json`:
- `colors`: Primary, light, dark theme colors
- `logo`: Light and dark mode logo paths
- `favicon`: Site favicon path
- `footer.socials`: Social media links

## Architecture Notes

### Payment Provider Integration

CeyPay acts as an aggregator for three crypto payment providers:
1. **Binance Pay**: Popular exchange wallet
2. **Bybit**: Exchange wallet with deep linking support
3. **Bitazza**: Regional crypto exchange

The plugin generates provider-specific QR codes and deep links, then monitors payment status via:
- **Webhooks**: Real-time notifications (primary method)
- **Polling**: Fallback status checks every 5 seconds

### Webhook Verification

Security is critical for webhooks to prevent fraudulent payment confirmations:
- Each webhook includes `X-Webhook-Signature` header
- Plugin computes HMAC-SHA256 hash of payload using merchant's webhook secret
- Signature mismatch = webhook rejected
- Replay attacks prevented via timestamp validation

### Test Mode

Test mode allows merchants to simulate the complete payment flow:
- Uses separate test API endpoint
- No actual crypto transactions
- Generates mock QR codes
- Simulates payment confirmation after 10 seconds
- Order still follows normal WooCommerce lifecycle

## Git Workflow

Main branch: `main`

Standard workflow:
1. Create feature branch from `main`
2. Make documentation changes
3. Test locally with `mintlify dev`
4. Commit changes
5. Push and create pull request to `main`
