---
sidebar_position: 4
---
# Project Setup & Installation
Setting up and installing the system

## Requirements & Prerequisites
1. **[Node.js v18+](https://nodejs.org/en/download)**
    - Required to run the frontend (Next.js) and backend services.
2. **[Docker](https://www.docker.com)**
	- Used to run Supabase locally in a containerized environment.
3. [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-**Git**)
    - Git clone the repo
4. **[PostgreSQL](https://www.postgresql.org/?utm_source=chatgpt.com)** (via Supabase)
    - Database engine used for storing all application data.
5. **[Supabase Cloud Project Database](https://supabase.com/docs/guides/getting-started)** OR **[Local Database](https://supabase.com/docs/guides/self-hosting/docker)**
	1. Create your own empty database
	2. Obtain these values (Click the "connect" button)
		1. Docs: [Connecting to Supabase](https://supabase.com/docs/guides/database/connecting-to-postgres) 
			1. **Database URL**
			2. **Direct URL**
		2. Docs: [Understanding API Keys](https://supabase.com/docs/guides/api/api-keys)
			1. **Project URL** (OPTIONAL)
			2. **Anon Key** (OPTIONAL)
6. Microsoft Entra ID (Azure) TODO:Isaac help me do this part
7. Obtain these values a Gmail Account with [App Password](https://support.google.com/accounts/answer/185833?hl=en)
	1. **Gmail address**
	2. **App password (16 characters)**
---
## Setup
Run the commands below in your terminal in your desired file location
```cmd title="Terminal"
# GitHub Repository
git clone https://github.com/hamm0208/student-study-planner-system
cd student-study-planner-system

# Install Dependencies
npm install 

# Local Docker Supabase (Make sure Docker is opened) (Can skip this part if your prisma is pointing to Supabase Cloud)
npx supabase start

# Push Schema to Database
npx prisma db push

#Generate Prisma Client
npx prisma generate
```
---
## Setup Environment Variables
```cmd title="student-study-planner-system/.env"
NEXT_PUBLIC_SERVER_URL = http://localhost:3000

#Connection to Local Supabase (Docker)
DATABASE_URL = postgresql://postgres:postgres@127.0.0.1:54322/postgres
DIRECT_URL = ""

# Connection to Cloud Supabase (Session Poller)
# DATABASE_URL="postgres://postgres.<project-ref>:<password>@aws-0-ap-southeast-1.pooler.supabase.com:6543/postgres?pgbouncer=true"
# DIRECT_URL="postgres://postgres.<project-ref>:<password>@aws-0-ap-southeast-1.pooler.supabase.com:5432/postgres"
  
# Connection to Cloud Supabase (Dedicated Pooler)
# DATABASE_URL="postgres://postgres:<password>@db.<project-ref>.supabase.co:6543/postgres?pgbouncer=true"
# DIRECT_URL="postgres://postgres:<password>@db.<project-ref>.supabase.co:5432/postgres"

#MODE OF THE PROGRAM
NEXT_PUBLIC_MODE = "DEV" OR "PROD"

# AZURE SETTINGS
# OAUTH CONFIGURATION
NEXT_PUBLIC_CLIENT_ID = <your-client-id>
NEXT_PUBLIC_AUTHORITY = <your-authority-key>

# LOCAL REDIRECT URI
NEXT_PUBLIC_REDIRECTURI = http://localhost:3000/view/dashboard
NEXT_PUBLIC_POSTLOGOUTREDIRECTURI = http://localhost:3000
  
# NODEMAILER SETTINGS
#GMAIL CREDENTIALS FOR NODEMAILER
GMAIL_EMAIL= <your-gmail-address>
GMAIL_APP_PW = <your-gmail-app-password>

# Default User Group ID for new users
DEFAULT_USER_GROUP_ID="1"
DEFAULT_USER_ROLE="Viewer"
  
# Turn whitelist on or off
WHITELIST_MODE = "false"

# For JWT Token
SESSION_SECRET = GENERATE-ANY-32-RANDOM-CHARACTERS

# Optional
NEXT_PUBLIC_DB_URL = "https://<project-ref>.supabase.co"
NEXT_PUBLIC_ANON_KEY = <your-anon-key> 
NEXT_PUBLIC_SUPABASE_STORAGE_NAME = "students-study-planners"

```
---
## Running the application
```cmd title="student-study-planner-system"
npm run dev
```