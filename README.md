# Project Management App

### System Design Challenge

# UI/Design Wireframe

![Project Management App Design Mocks](project-management-appmain-app.jpg)

# Data Sources

Response payloads and endpoints for use in architecting hooks & components.

#### GET /me

**Get Current User / UserModel**

Response Body

```json
{
  "id": "u_41",
  "name": "Avery Chen",
  "email": "avery.chen@example.com",
  "avatarUrl": "https://cdn.example.com/avatars/u_41.png",
  "timezone": "America/Denver",
  "role": "staff_engineer",
  "defaultProjectId": "p_alpha",
  "featureFlags": {
    "taskBlockingReasons": true,
    "advancedFilters": true
  }
}
```

#### GET /projects

**List Projects / ProjectListItem\[\]**

Response Body

```json
{
  "projects": [
    {
      "id": "p_alpha",
      "name": "Alpha Launch",
      "status": "active",
      "ownerId": "u_41",
      "taskCounts": { "total": 12, "active": 5 },
      "updatedAt": "2026-02-03T20:10:11Z"
    },
    {
      "id": "p_nimbus",
      "name": "Nimbus Migration",
      "status": "active",
      "ownerId": "u_12",
      "taskCounts": { "total": 8, "active": 3 },
      "updatedAt": "2026-02-04T13:02:44Z"
    },
    {
      "id": "p_legacy",
      "name": "Legacy Cleanup",
      "status": "archived",
      "ownerId": "u_08",
      "taskCounts": { "total": 4, "active": 1 },
      "updatedAt": "2026-01-21T09:33:21Z"
    }
  ]
}
```

#### GET /projects/:projectId

**Get Project By ID / ProjectDetailsModel**

Response Body

```json
{
  "id": "p_alpha",
  "name": "Alpha Launch",
  "description": "Ship the Alpha launch, harden onboarding, and finalize pricing page.",
  "status": "active",
  "owner": { "id": "u_41", "name": "Avery Chen" },
  "members": [
    { "id": "u_41", "name": "Avery Chen", "role": "owner" },
    { "id": "u_12", "name": "Sam Rivera", "role": "editor" },
    { "id": "u_08", "name": "Jordan Lee", "role": "viewer" }
  ],
  "taskStatusCounts": {
    "todo": 3,
    "in_progress": 2,
    "blocked": 0,
    "done": 7
  },
  "updatedAt": "2026-02-03T20:10:11Z"
}
```

#### GET /projects/:projectId/tasks

**Project Tasks / Paginated\<TaskListItem\[\]\>**

Params:

- status: todo | in_progress | blocked | done
- page: number
- pageSize: number

Response Body

```json
{
  "projectId": "p_alpha",
  "filters": { "status": "in_progress" },
  "page": 1,
  "pageSize": 50,
  "total": 2,
  "tasks": [
    {
      "id": "t_1007",
      "projectId": "p_alpha",
      "title": "Implement task status filters",
      "status": "in_progress",
      "assignee": { "id": "u_41", "name": "Avery Chen" },
      "priority": "p1",
      "dueDate": "2026-02-06",
      "updatedAt": "2026-02-04T14:40:10Z"
    },
    {
      "id": "t_1009",
      "projectId": "p_alpha",
      "title": "Triage onboarding funnel bugs",
      "status": "in_progress",
      "assignee": { "id": "u_12", "name": "Sam Rivera" },
      "priority": "p2",
      "dueDate": "2026-02-05",
      "updatedAt": "2026-02-04T13:11:52Z"
    }
  ]
}
```

#### GET /tasks/:taskId

**Get Task By Id / TaskDetailsModel**

Response Body

```json
{
  "id": "t_1007",
  "projectId": "p_alpha",
  "title": "Implement task status filters",
  "description": "Add status filter chips to the task list. Persist selection in URL. Ensure counts and sidebar stay in sync.",
  "status": "in_progress",
  "priority": "p1",
  "assignee": { "id": "u_41", "name": "Avery Chen" },
  "reporter": { "id": "u_12", "name": "Sam Rivera" },
  "tags": ["frontend", "ux"],
  "blockingReason": null,
  "createdAt": "2026-01-29T18:22:10Z",
  "updatedAt": "2026-02-04T14:40:10Z",
  "activity": [
    {
      "type": "comment",
      "id": "c_901",
      "author": { "id": "u_12", "name": "Sam Rivera" },
      "message": "Make sure the sidebar selection survives refresh via URL.",
      "createdAt": "2026-02-03T21:33:02Z"
    },
    {
      "type": "status_change",
      "id": "e_777",
      "from": "todo",
      "to": "in_progress",
      "author": { "id": "u_41", "name": "Avery Chen" },
      "createdAt": "2026-02-04T14:02:00Z"
    }
  ]
}
```

#### GET /project/:projectId/permissions

**Project Permissions by ID / PermissionsModel**

Params:

- projectId: string

Response Body

```json
{
  "userId": "u_41",
  "projectId": "p_alpha",
  "capabilities": {
    "task": {
      "create": true,
      "edit": true,
      "delete": true,
      "changeStatus": true,
      "assign": true
    },
    "project": {
      "editSettings": true,
      "archive": true
    }
  },
  "updatedAt": "2026-02-04T15:05:19Z"
}
```

#### GET /notifications

**List Notifications / unreadCount & NotificationsModel\[\]**

Params:

- unread_only: boolean

Response Body

```json
{
  "unreadCount": 2,
  "notifications": [
    {
      "id": "n_5501",
      "type": "task_mention",
      "entity": { "type": "task", "id": "t_1008", "projectId": "p_alpha" },
      "message": "Sam mentioned you on “Resolve mobile nav overlap”.",
      "createdAt": "2026-02-04T12:31:10Z",
      "readAt": null
    },
    {
      "id": "n_5502",
      "type": "task_assigned",
      "entity": { "type": "task", "id": "t_1009", "projectId": "p_alpha" },
      "message": "You were assigned “Triage onboarding funnel bugs”.",
      "createdAt": "2026-02-04T13:12:05Z",
      "readAt": null
    }
  ]
}
```

## Challenge: Mutation Endpoints

#### POST /tasks

**Create Task**

#### PUT /task/:taskId/status

**Update Task Status**

#### PUT /task/:taskId/assign

**Update Task Assignee**

#### PUT /task/:taskId/description

**Update Task Description**

#### PUT /task/:taskId/comment

**Add Task Comment**
