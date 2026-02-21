# CLAUDE.md

## Repository Overview

This is the **İlkadım Group** repository (`ilkadimglobal/claud`) — the central codebase for AI-driven commerce, branding, and global distribution systems. The organization builds AI-powered commerce ecosystems connecting manufacturing, branding, logistics, and retail across Turkey, Europe, and global markets.

### Key Ventures

- **ShopEndShop** — Curated multi-brand global e-commerce platform (home, lifestyle, consumer products)
- **Highzinberg AI** — Agent-based creative/marketing system for automated content production and campaign scaling
- **My Creative Company** — AI-powered digital marketing and brand strategy agency
- **İlk Adım Europe GmbH** — European operations hub for logistics, showroom, distribution, and retail

## Hosting & Deployment — Hostinger

**All web-based projects in this repository MUST be packaged and ready for deployment on a Hostinger shared/VPS hosting environment.**

### Deployment Rules (CRITICAL)

1. Every web project must include a complete, working deployment package — no half-finished builds
2. All projects must work out-of-the-box on Hostinger without manual server configuration
3. PHP is the primary backend language (Hostinger native support)
4. File structure must match Hostinger's `public_html/` directory convention
5. Every project must include a `.htaccess` file for URL routing/rewriting where needed
6. No server-level config changes (no custom Apache modules, no root access assumptions on shared hosting)

### Deployment Package Structure

Every web project must follow this structure when packaged for deployment:

```
project-name/
├── public_html/             # Document root — upload contents to Hostinger's public_html
│   ├── index.php            # Entry point
│   ├── .htaccess            # Apache rewrite rules
│   ├── assets/              # CSS, JS, images (compiled/minified)
│   │   ├── css/
│   │   ├── js/
│   │   └── images/
│   └── uploads/             # User-uploaded files (writable directory)
├── src/                     # Application source code (above document root)
│   ├── config/
│   │   ├── database.php     # DB connection config (reads from .env)
│   │   └── app.php          # App-level settings
│   ├── controllers/
│   ├── models/
│   ├── views/
│   └── helpers/
├── sql/                     # Database setup files
│   ├── schema.sql           # Table creation scripts
│   ├── seed.sql             # Initial/demo data (if needed)
│   └── migrations/          # Incremental DB changes
├── .env.example             # Environment variable template
├── .gitignore               # Must exclude .env, vendor/, node_modules/
└── README.md                # Project-specific setup instructions
```

### Standard .htaccess Template

Every project with routing must include:

```apache
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php?url=$1 [QSA,L]

# Security headers
Header set X-Content-Type-Options "nosniff"
Header set X-Frame-Options "SAMEORIGIN"
Header set X-XSS-Protection "1; mode=block"

# Block access to sensitive files
<FilesMatch "\.(env|sql|md|gitignore)$">
    Order allow,deny
    Deny from all
</FilesMatch>
```

### .env Configuration Template

Every project must use a `.env` file for Hostinger credentials. The `.env.example` must include:

```env
# Hostinger MySQL Database
DB_HOST=localhost
DB_NAME=
DB_USER=
DB_PASS=
DB_PORT=3306
DB_CHARSET=utf8mb4

# Application
APP_ENV=production
APP_URL=
APP_DEBUG=false

# Mail (Hostinger SMTP)
MAIL_HOST=smtp.hostinger.com
MAIL_PORT=465
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_ENCRYPTION=ssl
```

## Database — MySQL on Hostinger

**MySQL is the single source of truth for ALL data storage.** Every form, user input, program output, or any data that needs to be stored goes into the Hostinger MySQL database.

### Database Connection (Standard)

Every project must use this database connection class:

```php
<?php
class Database {
    private static ?PDO $instance = null;

    public static function getConnection(): PDO {
        if (self::$instance === null) {
            $env = parse_ini_file(__DIR__ . '/../../.env');
            self::$instance = new PDO(
                "mysql:host={$env['DB_HOST']};dbname={$env['DB_NAME']};port={$env['DB_PORT']};charset={$env['DB_CHARSET']}",
                $env['DB_USER'],
                $env['DB_PASS'],
                [
                    PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
                    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
                    PDO::ATTR_EMULATE_PREPARES => false,
                ]
            );
        }
        return self::$instance;
    }
}
```

### Database Rules (CRITICAL)

1. **Always use PDO** with prepared statements — never `mysqli_*` with string concatenation
2. **Every form** that collects user data must save to MySQL — no flat files, no JSON storage, no SQLite
3. **Every table** must include `id INT AUTO_INCREMENT PRIMARY KEY`, `created_at DATETIME DEFAULT CURRENT_TIMESTAMP`, and `updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`
4. **All SQL schema files** must be included in the `sql/` directory so the database can be recreated from scratch
5. **Use `utf8mb4`** charset for full Unicode support (Turkish characters, emojis, etc.)
6. **Foreign keys** must be used for relational data — maintain referential integrity
7. **Indexes** must be added on columns used in WHERE, JOIN, and ORDER BY clauses

### Standard Table Template

```sql
CREATE TABLE IF NOT EXISTS `table_name` (
    `id` INT AUTO_INCREMENT PRIMARY KEY,
    `created_at` DATETIME DEFAULT CURRENT_TIMESTAMP,
    `updated_at` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### Common Tables to Include When Relevant

- **users** — Registration, login, profiles
- **sessions** — Session management (if custom auth)
- **form_submissions** — Generic form data capture
- **logs** — Application activity/error logging
- **settings** — Key-value app configuration

## Tech Stack

- **Backend:** PHP 8.x (Hostinger supported)
- **Database:** MySQL 8.x on Hostinger (PDO connection)
- **Frontend:** React (compiled to static JS for deployment) or vanilla HTML/CSS/JS
- **E-commerce:** Shopify & custom e-commerce systems
- **Integrations:** API integrations with third-party services
- **AI/Media:** AI image & video pipelines
- **Server:** Hostinger shared hosting or VPS
- **Web Server:** Apache with `.htaccess` (Hostinger default)

### React Projects — Build for Hostinger

When a project uses React:

1. Build locally with `npm run build`
2. Place the compiled output (`build/` or `dist/`) into the `public_html/assets/` directory
3. API endpoints must be PHP files in `public_html/api/`
4. Use relative paths for API calls (e.g., `/api/submit.php`)
5. Include a `.htaccess` rule to serve `index.html` for client-side routing:

```apache
RewriteEngine On
RewriteBase /
RewriteRule ^api/ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.html [L]
```

## Development Conventions

### Git Workflow

- Default branch: `master`
- Feature branches should use descriptive names prefixed with the feature area
- Commit messages should be clear and descriptive

### Code Style

- **PHP:** Follow PSR-12 coding standards
- **React/JavaScript:** Use modern ES6+ syntax; prefer functional components with hooks
- **MySQL:** Use parameterized queries; never interpolate user input into SQL strings
- **File naming:** Use lowercase with hyphens for files (e.g., `product-listing.php`, `brand-card.tsx`)

### Security

- Never commit secrets, API keys, or credentials to the repository
- Use `.env` file (excluded via `.gitignore`) for all sensitive configuration
- Sanitize all user input; use prepared statements for every database operation
- Follow OWASP top-10 guidelines for web application security
- Always validate and sanitize file uploads (type, size, name)
- Use CSRF tokens on all forms
- Hash passwords with `password_hash()` (bcrypt) — never store plaintext

### Form Handling Standard

Every HTML form that collects data must:

1. Use `method="POST"`
2. Include a CSRF token
3. Validate input on both client-side (JS) and server-side (PHP)
4. Use prepared statements to insert into MySQL
5. Return proper success/error responses
6. Log the submission in the database

Example PHP form handler pattern:

```php
<?php
require_once __DIR__ . '/../src/config/database.php';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Validate CSRF
    // Sanitize input
    $name = htmlspecialchars(trim($_POST['name'] ?? ''), ENT_QUOTES, 'UTF-8');
    $email = filter_var($_POST['email'] ?? '', FILTER_VALIDATE_EMAIL);

    if (!$email) {
        http_response_code(400);
        echo json_encode(['error' => 'Geçersiz e-posta adresi']);
        exit;
    }

    // Save to MySQL
    $db = Database::getConnection();
    $stmt = $db->prepare("INSERT INTO form_submissions (name, email) VALUES (:name, :email)");
    $stmt->execute(['name' => $name, 'email' => $email]);

    echo json_encode(['success' => true, 'message' => 'Kayıt başarılı']);
}
```

## Core Focus Areas

- AI-powered brand systems
- Multi-brand e-commerce platforms
- EU–Turkey trade infrastructure
- Digital marketing automation
- Agent-based content production

## Directory Structure

```
/
├── README.md        # Project overview and strategic vision
├── CLAUDE.md        # This file — AI assistant guide
└── projects/        # Individual web projects (each deploy-ready for Hostinger)
```

## Deployment Checklist

Before any project is considered complete, verify:

- [ ] All files structured for Hostinger `public_html/` upload
- [ ] `.env.example` file included with all required variables
- [ ] `.htaccess` configured for routing and security
- [ ] `sql/schema.sql` contains all table definitions
- [ ] Database connection uses PDO with prepared statements
- [ ] All forms save data to MySQL
- [ ] No hardcoded credentials anywhere in the code
- [ ] Sensitive files blocked from public access via `.htaccess`
- [ ] CSRF protection on all forms
- [ ] Input validation on both client and server side
- [ ] Error pages (404, 500) are user-friendly
- [ ] README.md with project-specific setup instructions included

## Notes for AI Assistants

- **Every web project must be fully deployable on Hostinger** — no Docker, no Node.js server, no serverless functions. PHP + MySQL + Apache only
- **All data storage uses Hostinger MySQL** — forms, user data, logs, settings, everything goes into MySQL
- **Include complete SQL schema files** so databases can be set up from scratch on Hostinger
- **Always provide `.env.example`** — never hardcode database credentials
- **Test with Hostinger constraints in mind** — shared hosting limits (memory, execution time, file size)
- The organization operates across Turkey and Europe — support Turkish characters (utf8mb4) and consider i18n
- AI/ML integrations are a core part of the business; expect pipelines for image generation, video production, and marketing automation
- When building React frontends, always compile to static files and serve via PHP/Apache — no Node.js runtime on the server
