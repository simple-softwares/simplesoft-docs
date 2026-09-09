# SimpleSoft Screenshots

Live demo of SimpleSoft ERP platform across all interfaces.

## Backend API

FastAPI Swagger documentation showing 25+ business modules:

![Backend API Documentation](frontend/public/screenshots/backend-api.png)

**Features:**
- Complete API documentation
- 25+ modules (Auth, Projects, CRM, HR, Invoicing, Inventory, etc.)
- JWT authentication
- WebSocket support for real-time features

## Web Dashboard

React web application for business users and admins:

![Web Dashboard](frontend/public/screenshots/web-dashboard.png)

**Features:**
- Projects and task management
- CRM and contact management
- GST-ready invoicing
- Real-time notifications
- Responsive design
- Dark/light theme support

## Mobile App

React Native mobile application for field teams:

### Navigation & Dashboard
- Login screen
- Main dashboard with key metrics
- Navigation drawer with all modules
- Dark and light theme support

### Features
- **Dashboard**: Real-time overview of key metrics
- **Projects**: View and manage projects on mobile
- **Tasks**: Create and update tasks
- **Contacts**: Access customer database
- **Invoicing**: Invoice creation and tracking
- **Chat**: Real-time team communication
- **Notifications**: Push alerts and updates
- **Settings**: Workspace and account settings

### Screens

![Mobile Login](frontend/public/screenshots/mobile/login.jpeg)
![Mobile Dashboard Light](frontend/public/screenshots/mobile/dashboard_light.jpeg)
![Mobile Dashboard Dark](frontend/public/screenshots/mobile/dashboard-dark.jpeg)
![Mobile Projects](frontend/public/screenshots/mobile/projects.jpeg)
![Mobile Tasks](frontend/public/screenshots/mobile/task.jpeg)
![Mobile Contacts](frontend/public/screenshots/mobile/contacts.jpeg)
![Mobile Notifications](frontend/public/screenshots/mobile/notifications.jpeg)
![Mobile Settings](frontend/public/screenshots/mobile/settings_light.jpeg)
![Mobile Drawer](frontend/public/screenshots/mobile/drawer_ligh.jpeg)

## Try It Yourself

### Quick Start (Self-Hosted)

```bash
git clone https://github.com/simple-softwares/simplesoft-docker
cd simplesoft-docker
docker compose up
```

Then open:
- **Backend API**: http://localhost:8000/docs
- **Frontend**: http://localhost:5173
- **API**: http://localhost:8000/api

### Demo Credentials

```
Workspace: simple_soft
Email: founder@simplesoft.co.in
Password: Simple@123
```

## Stack

- **Backend**: FastAPI + SQLAlchemy + PostgreSQL
- **Frontend**: React + Tailwind CSS
- **Mobile**: React Native
- **Real-time**: WebSockets + Firebase notifications
- **Hosting**: Docker Compose ready

## Get Started

- **Code**: https://github.com/simple-softwares
- **Docs**: https://github.com/simple-softwares/simplesoft-docs
- **Business Model**: https://github.com/simple-softwares/simplesoft-docs/blob/main/BUSINESS_MODEL.md
