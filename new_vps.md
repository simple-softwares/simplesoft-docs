Everything is live. Here's the complete summary:

New VPS deployment complete.

Item	Value
URL	https://sbit.simplesoft.co.in
SSL	✅ Valid (Let's Encrypt, expires Sep 17)
Backend	✅ Running on port 8001
Migrations	✅ All applied
Workspace	sbit — SBIT (all modules enabled)
Login credentials:

Account	Email	Password
Super Admin	admin@simplesoft.co.in	Admin@2026!
SBIT Admin	admin@sbit.in	Sbit@2026!
Next steps:

Open https://sbit.simplesoft.co.in and log in as admin@sbit.in
Set up GST config, create employees, products, and any initial data
Mobile app API base URL: https://sbit.simplesoft.co.in/api (update config.js in the app if needed)
When you add a second client: bash 02-provision-client.sh client2 client2.simplesoft.co.in 8002 DbPass + DNS + certbot
For future deploys:


.\infra\scripts\deploy-backend.ps1 -Client sbit   # backend change
.\infra\scripts\deploy-frontend.ps1 -Client sbit  # frontend change