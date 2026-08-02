# MWECAU DigiVote (Laravel v2)

[![Laravel](https://img.shields.io/badge/Laravel-11-FF2D20?logo=laravel)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP-8.2+-777BB4?logo=php)](https://php.net)
[![MySQL](https://img.shields.io/badge/MySQL-8.0+-00758F?logo=mysql)](https://www.mysql.com)
[![License](https://img.shields.io/badge/License-GPL--3.0-blue)](LICENSE)

Secure digital voting platform for Mwenge Catholic University student elections. **No queues. No paper. Just democracy.**

┌─────────────────────────────────────────┐ │ MWECAU DigiVote │ │─────────────────────────────────────────│ │ [ Login with Reg Number ] │ │ │ │ Active Elections: │ │ ▸ Presidential Election [VOTE →] │ │ ▸ Residential Leadership [VOTE →] │ │ ▸ Course Representative [VOTE →] │ │ │ │ Your vote is anonymous & secure │ └─────────────────────────────────────────┘

Code

## What It Does

- **Multi-level elections** — Presidential, Residential, Course Representative
- **Secure voting** — JWT + session auth, one-time voter tokens, tamper-proof voting
- **Self-service password recovery** — Token-based reset links via email
- **Real-time results** — Live vote counts with auto-refresh
- **Vote from anywhere** — Students on campus or abroad
- **Mobile responsive** — Works on all devices, optimized for touch
- **High performance** — Queue-based background jobs, Redis caching
- **Observer role** — Real-time results dashboard for election monitoring

## Tech Stack

| Component | Technology |
|-----------|-----------|
| **Framework** | Laravel 11 (PHP 8.2+) |
| **Database** | MySQL 8.0+ |
| **Cache** | Redis |
| **Queue** | Redis (Async jobs) |
| **Frontend** | Blade Templates, Tailwind CSS, Alpine.js |
| **API** | RESTful API with Sanctum (Token Auth) |
| **Email** | Laravel Queue + Mailable |
| **File Storage** | Local / S3 |
| **Authentication** | Laravel Auth + JWT Tokens |
| **Server** | Nginx + PHP-FPM |

## Architecture

Internet │ Nginx ← reverse proxy, SSL, static files │ PHP-FPM ← Laravel 11 application server │ Laravel App ← Eloquent ORM, async jobs, JWT auth │ ╲ MySQL Redis ← sessions, cache, queue broker │ Queue Workers ← email, notifications (async)

Code

**Why this stack:**
- Laravel's built-in authentication and authorization
- Eloquent ORM for clean database queries
- Queue system for sending 5000+ voter emails in parallel batches
- Redis for caching session data and vote counts
- Sanctum for stateless JWT authentication
- Blade templating for dynamic frontend rendering

## Election Scope

| Type | Who Votes |
|------|-----------|
| Presidential | All MWECAU students |
| Residential | Kifumbu, Mawela, Kwachange, White House, Moshi Mjini, On-Campus |
| Course | All undergraduate, postgraduate and diploma programs |

## Key URLs

| Page | URL |
|------|-----|
| Home | `/` |
| Login | `/login` |
| Register | `/register` |
| Forgot Password | `/password-reset` |
| Dashboard | `/dashboard` |
| Active Elections | `/elections` |
| Cast Vote | `/elections/{id}/vote` |
| Results | `/elections/{id}/results` |
| Commissioner Panel | `/commissioner/dashboard` |
| Observer Dashboard | `/observer/dashboard` |
| Admin Panel | `/admin` |

## Setup — Local Development

**Requirements:** PHP 8.2+, MySQL 8.0+, Redis, Composer

```bash
# Clone the repository
git clone https://github.com/mwecauict-club/mwecau_digivote_laravel.git
cd mwecau_digivote_laravel

# Install dependencies
composer install

# Create environment file
cp .env.example .env

# Generate application key
php artisan key:generate

# Configure database in .env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=digivote
DB_USERNAME=root
DB_PASSWORD=your_password

# Run migrations
php artisan migrate

# Seed test data (optional)
php artisan db:seed

# Create symbolic link for storage
php artisan storage:link

# Start development server
php artisan serve
Setup — Production (Nginx + PHP-FPM)
bash
# Install system dependencies
sudo apt update
sudo apt install php8.2-fpm php8.2-mysql php8.2-redis composer nginx mysql-server redis-server

# Clone and setup
git clone https://github.com/mwecauict-club/mwecau_digivote_laravel.git /var/www/digivote
cd /var/www/digivote

# Install PHP dependencies
composer install --optimize-autoloader --no-dev

# Configure environment
cp .env.example .env
nano .env
# Set: DB_HOST, DB_DATABASE, DB_USERNAME, DB_PASSWORD
# Set: CACHE_DRIVER=redis, QUEUE_CONNECTION=redis

# Generate key
php artisan key:generate

# Run migrations
php artisan migrate --force

# Set permissions
sudo chown -R www-data:www-data /var/www/digivote
sudo chmod -R 755 /var/www/digivote/storage /var/www/digivote/bootstrap/cache

# Collect logs
php artisan storage:link

# Start Redis
sudo systemctl enable redis-server
sudo systemctl start redis-server

# Configure Nginx (see nginx.conf example)
sudo cp nginx.conf /etc/nginx/sites-available/digivote
sudo ln -s /etc/nginx/sites-available/digivote /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx

# Start queue worker (systemd service or supervisor)
php artisan queue:work redis --queue=default,email --tries=3
User Roles
Role	Permissions
Voter	Login, vote in eligible elections, view personal results
Candidate	Same as voter + manage campaign profile
Class Leader	Upload/manage college student data for voter registration
Commissioner	Full election management, user verification, analytics dashboard
Observer	Read-only access to live results and election statistics
Administrator	System administration, user management, database access
Security
✅ Registration number + password authentication
✅ One-time voter tokens per election level (UUID-based, time-limited)
✅ Vote anonymity — votes linked to tokens, not identities
✅ Password reset via secure time-limited links (1-hour expiry)
✅ Brute-force protection — 5 failed logins trigger 15-minute lockout
✅ CSRF protection on all state-changing requests (Laravel token)
✅ Rate limiting on API endpoints via Laravel throttle middleware
✅ Security headers — HSTS, X-Frame-Options, Content-Type-Nosniff
✅ JWT tokens with short expiry times (15 minutes)
✅ Sanctum-based API authentication
Database Schema
Key tables:

users — Voters and system users
elections — Election metadata
candidates — Candidates per election
votes — Ballot records (anonymized)
voter_tokens — One-time tokens (one per election level, per user)
password_resets — Token-based password recovery
login_attempts — Brute-force tracking
Features to Build
 User registration & authentication (Reg Number + Password)
 Multi-level election creation & management
 Candidate nomination & profile management
 Vote casting with one-time token validation
 Real-time results dashboard (30-second refresh)
 Password reset via email
 Commissioner election analytics panel
 Observer read-only results view
 Admin user & election management
 Async email queuing (5000+ emails in batches)
 API endpoints for mobile/external clients
 Voter eligibility validation
 Vote audit trail (anonymized)
 Mobile-responsive Blade templates
 Brute-force login protection
 Rate limiting & API throttling
Deployment Checklist
 Set APP_DEBUG=false in production
 Configure .env with production credentials
 Enable Redis caching and queue
 Set up MySQL daily backups
 Configure SSL/TLS with Let's Encrypt
 Enable HTTPS only (redirect HTTP → HTTPS)
 Set up log rotation
 Configure queue worker as systemd service
 Enable firewall rules (allow 80, 443 only)
 Set up monitoring & alerts
 Test email delivery
 Create database backups before migrations
Key Files & Directories
Code
mwecau_digivote_laravel/
├── app/
│   ├── Models/              # Eloquent models
│   ├── Http/Controllers/    # Request handlers
│   ├── Http/Middleware/     # Custom middleware
│   ├── Mail/                # Mailable classes
│   └── Jobs/                # Queued jobs
├── database/
│   ├── migrations/          # Schema definitions
│   └── seeders/             # Test data
├── resources/
│   ├── views/               # Blade templates
│   └── css/                 # Tailwind + custom styles
├── routes/
│   ├── web.php              # Web routes
│   └── api.php              # API routes
├── config/
│   ├── app.php              # App configuration
│   ├── database.php         # DB config
│   └── queue.php            # Queue config
├── storage/                 # Uploads & logs
├── .env.example             # Environment template
├── nginx.conf               # Nginx configuration
└── docker-compose.yml       # Docker setup (optional)
Contributing
See CONTRIBUTING.md for guidelines.

Team
Cleven — Project Manager & Lead Developer
Lajokjohn — Technical Lead
Faustine Emmanuel — Feature Developer
MWECAU ICT Club — Institution
License
This project is licensed under the GNU General Public License v3.0 — see LICENSE file.

© 2026 MWECAU ICT Club · One vote at a time.
