# Launch Story Draft

Use this as the base post for LinkedIn, Reddit, Hacker News, and founder communities. Adjust tone per platform.

## Short Version

I built an open-source Odoo alternative in 6 months.

It is called SimpleSoft: a FastAPI, React, and React Native ERP/business platform with modules for CRM, HR, projects, invoicing, GST, inventory, helpdesk, service tickets, chat, notifications, and admin control.

I started with the usual ERP problem: small businesses need software, but most serious ERP systems are either expensive, complicated, difficult to customize, or tied too strongly to one vendor.

I tried the Odoo route first. Eventually I removed Odoo completely and built my own Python backend so I could control the architecture, permissions, modules, and implementation experience.

Now I am open-sourcing the whole journey:

- Backend
- Frontend
- Mobile app
- Website
- Docs
- Docker/self-host setup

The code is free to self-host. My monetization is transparent: I earn through implementation, customization, migration, support, training, and later managed hosting when revenue allows it.

I do not have investor money or a big team. I have the code, the implementation skill, and the belief that useful business software should be inspectable and ownable.

If you are a developer, try the repo and tell me what breaks.

If you run a small business and want this configured for your team, I can implement it for you.

GitHub: https://github.com/simple-softwares

Contact: `[TODO: add email / WhatsApp / LinkedIn]`

## Long Version

### I built an open-source Odoo alternative in 6 months

For the last few months, I have been building SimpleSoft: an open-source ERP and business operations platform for small and growing businesses.

The stack is simple and practical:

- FastAPI backend
- SQLAlchemy and PostgreSQL
- React frontend
- React Native mobile app
- Redis and Celery for background jobs
- WebSockets for chat and notifications
- Firebase for push notifications
- WeasyPrint for PDFs

The product includes modules for:

- Projects and tasks
- CRM and contacts
- Sales and quotations
- GST-ready invoicing and accounting
- Products and inventory
- HR, attendance, leave, and payroll
- Helpdesk and service tickets
- Files, notes, calendar, chat, and notifications
- WhatsApp and email configuration
- Workspace admin, roles, permissions, and pricing controls

This did not start as a polished company story. It started as a practical frustration.

Most ERP software is powerful, but it often becomes heavy for small businesses. You either pay a lot, fight complexity, accept vendor lock-in, or spend months bending the system to fit your real workflow.

I first explored the Odoo path. Later I made a hard decision: remove Odoo completely and build a custom Python backend. That gave me control over the database, permission system, workflows, modules, APIs, and the way implementation happens for real customers.

Now I am making another big decision: open-source everything.

Not just a tiny core. The whole journey:

- Backend
- Frontend
- Mobile app
- Website
- Documentation
- Docker setup

### Why open source?

Because ERP software is too important to be a black box.

A business should be able to inspect the code, self-host it, modify it, migrate away, or hire someone else to maintain it. Trust should come from openness, not from a sales page.

### How will I make money?

Transparent answer: consulting services first.

The source code is free. I earn by helping businesses actually use it:

- Consulting on setup and deployment
- Data migration
- User, role, and module configuration
- GST, invoice, quotation, product, HR, CRM, and service workflows
- Custom reports and integrations
- Training and support

Later, if consulting revenue funds it, I want to offer managed SimpleSoft Cloud for people who do not want to self-host.

But right now I am not pretending to have a big cloud company. I am a founder-developer with code, passion, and implementation expertise.

### Who is this for?

Developers who want a practical open-source ERP codebase.

Small businesses that need CRM, HR, invoicing, inventory, and service management without starting from scratch.

Founders who believe useful software can be open and still sustainable.

### What I need now

If you are technical:

- Star the project
- Try the setup
- Open issues
- Improve docs
- Tell me where the architecture can be better

If you run a business:

- Tell me your workflow
- I can help set up SimpleSoft for your team
- I can customize it around your real process

GitHub: https://github.com/simple-softwares

Contact: `[TODO: add email / WhatsApp / LinkedIn]`

## Platform-Specific Versions

### LinkedIn

I built an open-source ERP/business platform in 6 months.

It is called SimpleSoft, built with FastAPI, React, and React Native.

It includes CRM, projects, HR, GST-ready invoicing, inventory, helpdesk, service tickets, chat, notifications, WhatsApp/email configuration, and admin controls.

The biggest decision: I am open-sourcing the whole journey.

Backend, frontend, mobile app, website, docs, deployment setup.

Why?

Because small businesses deserve software they can inspect, self-host, customize, and own. ERP should not feel like a locked room.

The business model is transparent:

The code is free.

I earn through consulting on implementation, customization, migration, training, support, and eventually managed hosting.

No investor money. No big launch budget. Just code, consulting expertise, and a belief that useful business software can be open and sustainable.

GitHub: https://github.com/simple-softwares

If you are a developer, try it and tell me what breaks.

If you run a business, I can help implement it for your team.

### Reddit

Title: I built an open-source Odoo alternative with FastAPI, React, and React Native

Post:

I spent the last few months building SimpleSoft, an open-source ERP/business operations platform.

Stack: FastAPI, SQLAlchemy, PostgreSQL, React, React Native, Redis/Celery, WebSockets.

Modules: CRM, projects, HR, attendance, payroll, GST invoicing, quotations, inventory, helpdesk, service tickets, files, notes, chat, notifications, WhatsApp/email configuration, roles and permissions.

I originally explored Odoo, but eventually removed it completely and built my own backend because I wanted more control over the architecture and implementation experience.

I am now open-sourcing the whole thing: backend, frontend, mobile app, website, docs, and Docker setup.

The code is free to self-host. Monetization is through consulting on implementation, customization, migration, support, training, and later managed hosting if there is enough revenue.

GitHub: https://github.com/simple-softwares

I would genuinely appreciate technical feedback, especially around architecture, setup, permissions, and production deployment.

### Hacker News

Title: Show HN: SimpleSoft - open-source ERP built with FastAPI and React

Post:

I built SimpleSoft, an open-source ERP/business operations platform for small businesses.

It includes modules for CRM, projects, tasks, HR, attendance, payroll, GST-ready invoicing, quotations, products, inventory, helpdesk, service tickets, files, notes, chat, notifications, and workspace admin.

The backend is FastAPI with SQLAlchemy and PostgreSQL. The web app is React. The mobile app is React Native.

I originally explored Odoo, but removed it completely and built a custom Python backend to keep the architecture and implementation experience simpler.

The business model is transparent: free self-hosted source code, consulting on implementation/customization/support/training, and eventually managed hosting when revenue allows it.

GitHub: https://github.com/simple-softwares

Feedback welcome, especially on setup, architecture, and what would make the project easier to self-host.
