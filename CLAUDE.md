# CLAUDE.md

## Repository Overview

This is the **İlkadım Group** repository (`ilkadimglobal/claud`) — the central codebase for AI-driven commerce, branding, and global distribution systems. The organization builds AI-powered commerce ecosystems connecting manufacturing, branding, logistics, and retail across Turkey, Europe, and global markets.

### Key Ventures

- **ShopEndShop** — Curated multi-brand global e-commerce platform (home, lifestyle, consumer products)
- **Highzinberg AI** — Agent-based creative/marketing system for automated content production and campaign scaling
- **My Creative Company** — AI-powered digital marketing and brand strategy agency
- **İlk Adım Europe GmbH** — European operations hub for logistics, showroom, distribution, and retail

## Project Status

This repository is in its early stages. Currently it contains only the project README with organizational information and strategic vision. No application code, configuration files, or infrastructure has been committed yet.

## Tech Stack (Planned)

As documented in the README:

- **Backend:** PHP, MySQL
- **Frontend:** React
- **E-commerce:** Shopify & custom e-commerce systems
- **Integrations:** API integrations with third-party services
- **AI/Media:** AI image & video pipelines
- **Infrastructure:** VPS-based automation tools

## Development Conventions

### Git Workflow

- Default branch: `master`
- Feature branches should use descriptive names prefixed with the feature area
- Commit messages should be clear and descriptive

### Code Style

When code is added to this repository, follow these conventions:

- **PHP:** Follow PSR-12 coding standards
- **React/JavaScript:** Use modern ES6+ syntax; prefer functional components with hooks
- **MySQL:** Use parameterized queries; never interpolate user input into SQL strings
- **File naming:** Use lowercase with hyphens for files (e.g., `product-listing.php`, `brand-card.tsx`)

### Security

- Never commit secrets, API keys, or credentials to the repository
- Use environment variables or a `.env` file (excluded via `.gitignore`) for sensitive configuration
- Sanitize all user input; use parameterized queries for database operations
- Follow OWASP top-10 guidelines for web application security

## Core Focus Areas

- AI-powered brand systems
- Multi-brand e-commerce platforms
- EU–Turkey trade infrastructure
- Digital marketing automation
- Agent-based content production

## Directory Structure

As the project grows, the expected structure will be documented here. Currently:

```
/
├── README.md        # Project overview and strategic vision
└── CLAUDE.md        # This file — AI assistant guide
```

## Commands

No build, test, or deployment commands are configured yet. This section should be updated as tooling is added (e.g., `composer install`, `npm install`, `npm run build`, test runners, linters).

## Notes for AI Assistants

- This is an early-stage repo — do not assume the existence of files, configs, or infrastructure that haven't been committed
- When adding new code, match the tech stack described above (PHP, React, MySQL, Shopify integrations)
- Prioritize simplicity and security in all implementations
- The organization operates across Turkey and Europe — consider internationalization (i18n) and multi-currency support where relevant
- AI/ML integrations are a core part of the business; expect pipelines for image generation, video production, and marketing automation
