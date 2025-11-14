---
sidebar_position: 5
---
# Useful Commands
## First Time Setup Database 
:::note  

Target DB must be empty

Run this on First Setup, ensure that you have supabase_dump.sql in the location that you clone this repo
:::

```
npm run db:setup
```

## Extract Schema from Prisma Client

:::note  
  
Extract the complete schema of the database and export as `supabase_dump.sql` 

Ensure that your Prisma client is **connected to the correct database** (Check your .env file)
  
:::
```shell
npm run db:export
```

## Danger Zone
### Seed Data Into DB
:::danger 
**🛑 CRITICAL WARNING: Data Loss Imminent**

This script executes a destructive two-step process:

1. **FULL CLEAR:** It will **DROP** all tables and **DELETE ALL DATA** in your current Prisma-connected database.
    
2. **RE-SEED:** It will then load data from `supabase_dump.sql`.
    

**DO NOT RUN THIS ON A PRODUCTION OR LIVE DATABASE!**
```
npm run db:seed
```
:::
