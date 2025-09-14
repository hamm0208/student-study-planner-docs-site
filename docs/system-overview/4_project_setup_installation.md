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
3. **[PostgreSQL](https://www.postgresql.org/?utm_source=chatgpt.com)** (via Supabase)
    - Database engine used for storing all application data.
4. Supabase Cloud Project Database 
	1. Create your own empty database
	2. Obtain these values (Click the "connect" button)
		1. Docs: [Connecting to Supabase](https://supabase.com/docs/guides/database/connecting-to-postgres) 
			1. **Transaction Pooler URL**
			2. **Session Pooler URL**
		2. Docs: [Understanding API Keys](https://supabase.com/docs/guides/api/api-keys)
			1. **Project** URL
			2. **Anon Key**
5. Microsoft Entra ID (Azure) TODO:Isaac help me do this part
6. Obtain these values a Gmail Account with App Password
	1. **Gmail address**
	2. **App password (16 characters)**
7. **Open AI API** (Used for importing study planner Status: TBD])
---
## Setup
Run the commands below in your terminal in your desired file location
```cmd title="Terminal"
# GitHub Repository
git clone https://github.com/hamm0208/student-study-planner-system
cd student-study-planner-system

# Install Dependencies
npm install 

# Local Docker Supabase (Make sure Docker is opened)
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

#Connection to Local Supabase (ORM Only)
DATABASE_URL = postgresql://postgres:postgres@127.0.0.1:54322/postgres
DIRECT_URL = ""

#Connection to Cloud Supabase (ORM Only)
# DATABASE_URL="postgres://postgres.<project-ref>:<password>@aws-0-ap-southeast-1.pooler.supabase.com:6543/postgres?pgbouncer=true"
# DIRECT_URL="postgres://postgres.<project-ref>:<password>@aws-0-ap-southeast-1.pooler.supabase.com:5432/postgres"
  
# Supabase DB URL and ANON KEY for Frontend
NEXT_PUBLIC_DB_URL = "https://<project-ref>.supabase.co"
NEXT_PUBLIC_ANON_KEY = <your-anon-key>
NEXT_PUBLIC_SUPABASE_STORAGE_NAME = "students-study-planners"

#MODE OF THE PROGRAM
NEXT_PUBLIC_MODE = "DEV"

#OAUTH CONFIGURATION
NEXT_PUBLIC_CLIENT_ID = <your-client-id>
NEXT_PUBLIC_AUTHORITY = <your-authority-key>
  
#LOCAL REDIRECT URI
NEXT_PUBLIC_REDIRECTURI = http://localhost:3000/view/dashboard
NEXT_PUBLIC_POSTLOGOUTREDIRECTURI = http://localhost:3000

#API KEYS FOR AND OPENAI
OPEN_AI_API = <open-ai-api>
  
#GMAIL CREDENTIALS FOR NODEMAILER
GMAIL_EMAIL= <your-gmail-address>
GMAIL_APP_PW = <your-gmail-app-password>
```
---
## Running the application
```cmd title="student-study-planner-system"
npm run dev
```