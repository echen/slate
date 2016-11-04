---
title: Hybrid API

language_tabs:
  - shell

toc_footers:
  - <a href='https://www.gethybrid.io/poster_sign_up'>Sign up on Hybrid</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Hybrid API!

# Authentication

> To authorize, use this code. Note that the colon is not part of the API key, but follows it.

```shell
curl {{API_ENDPOINT}}
  -u {{YOUR_API_KEY}}:
```

Hybrid uses API keys to allow access to the API. You can find your API key in [your profile](https://www.gethybrid.io/me).

Authentication is performed via [HTTP Basic Auth](http://en.wikipedia.org/wiki/Basic_access_authentication). Your API key is your basic auth username, and you do not need to provide a password.

If you need to authenticate via bearer auth (e.g., for a cross-origin request), use

(TODO: is Bearer needed?)
`-H "Authorization: Bearer {{YOUR_API_KEY}}:"`

instead of `-u {{YOUR_API_KEY}}:`.

# Projects

## The project object

> Example Response

```shell
{
  "id": "17e323f1-f7e4-427c-a2d5-456743aba8",
  "callback_url": "https://www.mywebsite.com/hybrid_callback",
  "name": "Categorize this website",
  "num_tasks": 1000,
  "num_tasks_completed": 239,
  "num_workers_per_task": 1,
  "payment_per_response": 0.1,
  "status": "unlaunched"
}
```

Parameter | Type | Description
--------- | ---- | -----------
id | string |
callback_url | string | Once a task is completed, it is POSTed to this url.
created_at | string |
name | string | Name you gave this project.
num_tasks | integer | Number of tasks in this project.
num_tasks_completed | integer | Number of completed tasks.
num_workers_per_task | integer | How many workers work on each task (i.e., how many response per task).
payment_per_response | float | How much a worker is paid (in US dollars) for an individual response.
status | string | One of `unlaunched`, `in_progress`, `completed`, `canceled`, or `paused`.

## Create a project

> Definition

```
POST https://www.gethybrid.io/api/projects
```

> Example Request

```shell
curl https://www.gethybrid.io/api/projects/17e323f1-f7e4-427c-a2d5-456743aba8
  -u {{YOUR_API_KEY}}:
```

> Example Response

```json
{
  "id": "17e323f1-f7e4-427c-a2d5-456743aba8",
  "callback_url": "https://www.mywebsite.com/hybrid_callback",
  "name": "Categorize this website",
  "num_tasks": 1000,
  "num_tasks_completed": 239,
  "num_workers_per_task": 1,
  "payment_per_response": 0.1,
  "status": "unlaunched"
}
```

Creates a project.

### Parameters

Parameter | Type | Description
--------- | ---- | -----------
name | string | Name to give your project. (required)
payment_per_response | float | Amount in dollars to pay workers for each response. (required)
template | array | ... (required) TODO
callback_url | string | URL where completed tasks will be POSTed. (optional, default null)
num_workers_per_task | integer | Number of workers who work on each task. (optional, default 1)

## List all projects

> Definition

```
GET https://www.gethybrid.io/api/projects
```

> Example Request

```shell
curl https://www.gethybrid.io/api/projects
  -u {{YOUR_API_KEY}}:
```

> Example Response

```json
[
  {
    "id": "17e323f1-f7e4-427c-a2d5-456743aba8",
    "callback_url": "https://www.mywebsite.com/hybrid_callback",
    "name": "Categorize customer websites",
    "num_tasks": 1000,
    "num_tasks_completed": 239,
    "num_workers_per_task": 1,
    "payment_per_response": 0.1,
    "status": "unlaunched"
  },
  {
    "id": "81533541-0359-4d4b-a545-af38a2cb3e8c",
    "callback_url": null,
    "name": "Label product images",
    "num_tasks": 5000,
    "num_tasks_completed": 4315,
    "num_workers_per_task": 1,
    "payment_per_response": 0.25,
    "status": "in_progress"
  }
]
```

Lists all projects you have created.

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
cursor | TODO | TODO

## Retrieve a project

> Definition

```
GET https://www.gethybrid.io/api/projects/{{PROJECT_ID}}
```

> Example Request

```shell
curl https://www.gethybrid.io/api/projects/17e323f1-f7e4-427c-a2d5-456743aba8
  -u {{YOUR_API_KEY}}:
```

> Example Response

```json
{
  "id": "17e323f1-f7e4-427c-a2d5-456743aba8",
  "callback_url": "https://www.mywebsite.com/hybrid_callback",
  "name": "Categorize this website",
  "num_tasks": 1000,
  "num_tasks_completed": 239,
  "num_workers_per_task": 1,
  "payment_per_response": 0.1,
  "status": "unlaunched"
}
```

Retrieves a specific project you have created.

# Tasks

## The task object

> Example Response

```shell
{
  "id": "38da6bc5-a644-41b9-a964-4678bc7375c6",
  "project_id": "b31ede78-afdf-4938-b775-8813453c7fc5",
  "created_at": "2016-11-01T18:56:56.000Z",
  "is_complete": true,
    
  "data": {
    "company": "Hybrid",
    "city": "San Francisco",
    "state": "CA"
  },

  "responses": [
    {
      "created_at": "2016-11-01T23:29:17.971Z",
      "id": "1befb37b-8e4f-42b6-8a61-2c946ad4b5ce",
      "response_data": {
        "website": "https://www.gethybrid.io",
        "category": "Technology"
      }
    }
  ]
}
```

Parameter | Type | Description
--------- | ---- | -----------
id | string |
created_at | string |
project_id | string | ID of the project that this task belongs to.
is_complete | boolean | Whether or not this task has received the desired number of responses (equal to the project's `num_workers_per_task` field).
data | dictionary | A dictionary of named fields that get shown (according to the project template) to workers when working on the task.
responses | array | An array of responses to the task. Each response... TODO

## Create a task

> Definition

```
POST https://www.gethybrid.io/api/projects/{{PROJECT_ID}}/tasks
```

> Example Request

```shell
curl https://www.gethybrid.io/api/projects/17e323f1-f7e4-427c-a2d5-456743aba8/tasks
  -u {{YOUR_API_KEY}}:
  -d data=...TODO...
  
```

> Example Response

```json
{
  "id": "38da6bc5-a644-41b9-a964-4678bc7375c6",
  "project_id": "b31ede78-afdf-4938-b775-8813453c7fc5",
  "created_at": "2016-11-01T18:56:56.000Z",
  "is_complete": false,
    
  "data": {
    "company": "Hybrid",
    "city": "San Francisco",
    "state": "CA"
  },

  "responses": []
}
```

Creates a task. By default, the project is launched... TODO

### Parameters

Parameter | Type | Description
--------- | ---- | -----------
data | dictionary | A dictionary of named fields that get shown (according to the project template) to workers when working on the task. (required)

## List all tasks

> Definition

```
GET https://www.gethybrid.io/api/projects/{{PROJECT_ID}}/tasks
```

> Example Request

```shell
curl https://www.gethybrid.io/api/projects/b31ede78-afdf-4938-b775-8813453c7fc5/tasks
  -u {{YOUR_API_KEY}}:
```

> Example Response

```json
[
  {
    "id": "38da6bc5-a644-41b9-a964-4678bc7375c6",
    "project_id": "b31ede78-afdf-4938-b775-8813453c7fc5",
    "created_at": "2016-11-01T18:56:56.000Z",
    "is_complete": true,
    
    "data": {
      "company": "Hybrid",
      "city": "San Francisco",
      "state": "CA"
    },

    "responses": [
      {
        "created_at": "2016-11-01T23:29:17.971Z",
        "id": "1befb37b-8e4f-42b6-8a61-2c946ad4b5ce",
        "response_data": {
          "website": "https://www.gethybrid.io",
          "category": "Technology"
        }
      }
    ]
  },
  {
    "id": "dd66e80a-88b3-4c4f-9efc-2aca8eed73db",
    "project_id": "b31ede78-afdf-4938-b775-8813453c7fc5",
    "created_at": "2016-11-01T23:23:26.016Z",
    "is_complete": true,

    "data": {
      "company": "Google",
      "city": "Mountain View",
      "state": "CA"
    },

    "responses": [
      {
        "created_at": "2016-11-01T23:22:19.124Z",
        "id": "cc9f4263-1d0f-4730-a97e-199851b6ade5",
        "response_data": {
          "website": "https://www.google.com",
          "category": "Technology"
        }
      }
    ]
  }
]
```

Lists all tasks belonging to a project.

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
cursor | TODO | TODO

## Retrieve a task

> Definition

```
GET https://www.gethybrid.io/api/tasks/{{TASK_ID}}
```

> Example Request

```shell
curl https://www.gethybrid.io/api/tasks/38da6bc5-a644-41b9-a964-4678bc7375c6
  -u {{YOUR_API_KEY}}:
```

> Example Response

```json
{
  "id": "38da6bc5-a644-41b9-a964-4678bc7375c6",
  "project_id": "b31ede78-afdf-4938-b775-8813453c7fc5",
  "created_at": "2016-11-01T18:56:56.000Z",
  "is_complete": true,
  
  "data": {
    "company": "Hybrid",
    "city": "San Francisco",
    "state": "CA"
  },

  "responses": [
    {
      "created_at": "2016-11-01T23:29:17.971Z",
      "id": "1befb37b-8e4f-42b6-8a61-2c946ad4b5ce",
      "response_data": {
        "website": "https://www.gethybrid.io",
        "category": "Technology"
      }
    }
  ]
}
```

Retrieves a specific task you have created.