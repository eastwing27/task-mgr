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

### Docker Note
SSR is disabled in Nuxt.js to simplify Docker networking. All API calls happen from browser to `localhost:3000`.

## Dev Environment Setup 
<details>
<summary>(and/or manual run)</summary>

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
# Create .env file (optional)
API_URL=http://localhost:3000/api/v1
```

Run the following commands:
```bash
npm install
npm run dev
```
The app is available at `localhost:3001`

</details>

## Backend Notes
### Architecture Overview
 - API endpoints are nested in version folder under `src/routes/` (e.g., `src/routes/v1/tasks.ts`).
 - Database interactions are handled via Prisma ORM located in `src/prisma/`.
 - Business logic is in `src/services/`. The idea is to keep route handlers thin.
 - Prisma and Redis clients are initialized in `src/utils/` for reuse across the app.
 - The DB has 2 tables: tasks and history which is used to log changes to tasks.

### Possible Improvements
 - Add user authentication and authorization.
 - Improve task list querying with pagination.
 - Add request validation; check bodies for the required fields.
 - More detailed error handling. Business logic could either throw custom errors or use special data/error handling types depending on the reason.
 - There is History table but it is not used anywhere. Task verbose endpoint can be added to fetch change history for a task.
 - Task changes are logged synchronously now. This can be moved to background job queue for better performance.
 - The app can broadcast real-time updates via WebSockets when tasks are created/updated/deleted.

## Frontend Notes
### Architecture Overview
I was trying to follow the standard Nuxt.js project structure with the following key directories:
- `components/` - Reusable Vue components like task list, task form, buttons, etc.
- `pages/` - At the moment there are only the landing page placeholder, the task list and task detail page. The latter is not in use and can only be accessed manually as it is replaced with modal. It is there just to demonstrate routing.
- `composables/` - Vue composables for API interactions (fetching, creating, updating, deleting tasks).
- `types/` - TypeScript type definitions for Task and API responses.
- `assets/css/` - Global styles.
- `utils/` - Utility functions.

### Possible Improvements
- Implement pagination for task list if there are many tasks.
- Add user authentication and authorization; possibility to set the task performer.
- At the moment, task list page fetches all the task on every task change. It can be optimized by updating only the changed task after successful result got from the API call.
- Real-time updates with WebSockets.
- Better UX:
  - Loading indicators for API calls.
  - Confirmation dialogs for deleting tasks.
  - Error handling and user feedback for API calls.
  - All the columns can be made sortable.
  - Better looking dropdowns.
  - Date pickers for deadline input are require time entered manually. This should be improved.
- Better mobile support.
- SSR is set to false for simplicity, it can be enabled for better SEO and initial load performance but requires proper Docker networking setup.
