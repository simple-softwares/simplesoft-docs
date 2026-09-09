# GitHub Organization Setup Checklist

Use this checklist to turn the local SimpleSoft workspace into a clean public GitHub organization.

## 1. Create The Organization

- Go to `https://github.com/organizations/new`
- Organization name: `simple-softwares`
- Display name: `SimpleSoft`
- Visibility: public
- Billing: free

## 2. Create Repositories

Create these repositories:

| Repository | Local source | Purpose |
| --- | --- | --- |
| `.github` | `.github/` | Organization profile, business model, hire-me page |
| `simplesoft-backend` | `backend/` | FastAPI backend |
| `simplesoft-frontend` | `frontend/` | React web app |
| `simplesoft-mobile` | `mobile_app/` | React Native app |
| `simplesoft-website` | `simplesoft_website/` | Public website |
| `simplesoft-docs` | `docs/` | Documentation |
| `simplesoft-docker` | create later | Docker Compose self-host setup |

## 3. Fill Required TODOs

Search before publishing:

```bash
rg "TODO" .github backend docs
```

Fill:

- Email
- WhatsApp number with country code
- LinkedIn profile
- YouTube demo link when ready

## 4. Do Not Push Secrets

Before creating public repos, review these files carefully:

- `.env`
- `*.db`
- `*.sql`
- keystores
- service account JSON files
- local backup archives
- uploaded media
- client data
- seed files with real customer data

Recommended: create a fresh public repo from cleaned files instead of pushing the whole local workspace.

## 5. Backend First Push

From a cleaned backend folder:

```bash
cd backend
git init
git add .
git commit -m "Initial open-source backend release"
git branch -M main
git remote add origin https://github.com/simple-softwares/simplesoft-backend.git
git push -u origin main
```

## 6. Organization Profile Push

From the `.github` folder:

```bash
cd .github
git init
git add .
git commit -m "Add SimpleSoft organization profile"
git branch -M main
git remote add origin https://github.com/simple-softwares/.github.git
git push -u origin main
```

## 7. Launch Order

Recommended order:

1. Publish `.github` profile repo
2. Publish backend repo
3. Publish frontend repo
4. Publish launch story on LinkedIn
5. Post technical feedback version to Reddit
6. Post to Hacker News after the GitHub links are stable
7. Start direct outreach to first 20 businesses

Use `docs/launch/LINKEDIN_REDDIT_POSTING_PLAN.md` for ready-to-post drafts.

## 8. First Week Success Target

Simple targets:

- GitHub organization live
- Backend README clear
- One setup path working
- One demo video recorded
- 20 direct outreach messages sent
- 3 serious conversations started
- 1 paid setup proposal sent
