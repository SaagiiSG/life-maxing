---
name: design-system
description: Fetch brand/platform DESIGN.md files from awesome-design-md repo on demand for frontend design work. Use when user says "design like X", "use X design system", "make it look like X", or asks for brand-specific UI guidance.
---

# Design System Fetcher

Source: https://github.com/VoltAgent/awesome-design-md

## Available Design Systems

**AI & LLM Platforms:** Claude, OpenAI, Mistral, Ollama (12 total)
**Developer Tools & IDEs:** Cursor, Vercel, Raycast, Warp (7 total)
**Backend / Database / DevOps:** MongoDB, Supabase, PostHog, Sentry (8 total)
**Productivity & SaaS:** Linear, Notion, Cal.com, Zapier (7 total)
**Design & Creative Tools:** Figma, Framer, Webflow, Airtable (6 total)
**Fintech & Crypto:** Stripe, Coinbase, Binance, Wise (7 total)
**E-commerce & Retail:** Shopify, Airbnb, Nike, Starbucks (5 total)
**Media & Consumer Tech:** Apple, Spotify, NVIDIA, The Verge (10 total)
**Automotive:** Tesla, Ferrari, BMW, Lamborghini (6 total)

## How to Use

When invoked, do the following:

1. **Identify the target brand** from the user's request (e.g. "Linear-style UI" → Linear)
2. **Fetch the DESIGN.md** using WebFetch:
   - URL pattern: `https://raw.githubusercontent.com/VoltAgent/awesome-design-md/main/designs/<BrandName>/DESIGN.md`
   - If unsure of exact path, fetch the README first to find the correct link:
     `https://raw.githubusercontent.com/VoltAgent/awesome-design-md/main/README.md`
3. **Parse and apply** the design rules — typography, colors, spacing, component style — to the frontend work at hand
4. **If no exact match**, pick the closest brand aesthetic and note it to the user

## Fetch Instructions

Use WebFetch with prompt: "Extract all design tokens, typography rules, color palette, spacing system, and component guidelines from this DESIGN.md."

Apply what you get directly to the current UI task. Do not summarize it back — just use it.
