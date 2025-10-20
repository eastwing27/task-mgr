# Task Manager

An example task management application.

This repo combines 2 projects (as git submodules):
- [task-mgr-be](https://github.com/eastwing27/task-mgr-be) (Express.js backend)
- [task-mgr-fe](https://github.com/eastwing27/task-mgr-fe) (Nuxt.js frontend)

## Cloning

Since this repository uses git submodules to combine backend and frontend projects, you need to clone it with submodule initialization:

```bash
git clone --recurse-submodules https://github.com/eastwing27/task-mgr.git
```

Make sure the `be/` and `fe/` folders contain project files and are not empty.

## Docker Setup

To create and start a Docker container run
```bash
sudo docker-compose up --build
```
The app is available at `localhost:3001`

<details>
<summary>## Dev Environment Setup (and/or manual run)</summary>

 - Make sure you've got Redis installed.
 - Make sure you've got either local or remote Postgres DB available.

### Backend
Go to the `be` folder.
Ceate an `.env. file
```
DATABASE_URL="your-postgres-connection-string"
```
Run the following commands:
```bash
npm install
sudo systemctl start redis-server
npm run start
```

### Frontend
Go to the `fe` folder.
OPTIONAL: create an `.env` file
```
API_URL=http://localhost:3000/api/v1
```

Run the following commands:
```bash
npm install
npm run dev
```
The app is available at `localhost:3001`

</details>
