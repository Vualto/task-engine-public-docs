# TASK ENGINE API

The Task Engine endpoints will always return a JSON response unless explicitly indicated otherwise.

## STATUS ENDPOINTS

### GET: `/`

This endpoint will check if the Task Engine endpoint is reachable.

<details>

<summary>Details</summary>
<br />

**Requires Authentication: No**<br />
**Required Headers: None** <br />
**Optional Headers: None**

```json
{
    "result": "alive"
}
```

</details><br /><br />

### GET: `/health`

The health endpoint will run checks on the different Task Engine components and returns the status of each service. The endpoint will also return some information about the Task Engine and statistics about jobs and tasks.

<details>

<summary>Details</summary>
<br />

**Requires Authentication: Yes**<br />
**Required Headers:**

- `client` - client name, required for authentication
- `api-key` - required for authentication
 
**Optional Headers: None**

Successful Response:

```json
{
    "version": "1.169.3", // Task Engine version
    "databases": {
        "redis": "OK",
        "postgres": "OK"
    },
    "queues": {
        "windows_capture": {
            "workers": 12,
            "working": 0,
            "pending": 0
        },
        "scheduler": {
            "workers": 1,
            "working": 0,
            "pending": 0
        },
        "work": {
            "workers": 9,
            "working": 0,
            "pending": 0
        },
        "controller": {
            "workers": 1,
            "working": 0,
            "pending": 0
        },
        "callback": {
            "workers": 2,
            "working": 0,
            "pending": 0
        }
    },
    "tasks": {
        "failed": 0,
        "pending": 0,
        "processed": 987654321
    },
    "jobs": {
        "completed": 12345,
        "failed": 65,
        "pending": 0,
        "broken": 20,
        "queued": 4,
        "started": 8,
        "scheduled": 7,
        "paused": 0,
        "max_jobs": 8,
        "priority_slots": 3
    }
}
```

500 - Error Response:

```json
{
    "error": "<error message>"
}
```

</details><br /><br />

### GET: `/dashboard`

The dashboard endpoint returns information about the current Task Engine queue status. The information includes lists of started, queued and scheduled jobs as well as the setting information for the maximum concurrent jobs and the number of priority reserved job slots. The Task Engine version is also returned.

<details>

<summary>Details</summary>
<br />

**Requires Authentication: No**<br />
**Required Headers:None**<br />
**Optional Headers:**

- `client` - client name, used to filter by client-name in multi-tenant setups.

```json
{
    "max_jobs": 2,
    "priority_slots": 0,
    "started": [
        {
            "id": 123,
            "client": "demo-client",
            "workflow": "vodcapture",
            "priority": 5,
            "position": 1,
            "created_at": "2019-11-29T10:47:50.431Z",
            "updated_at": "2020-07-13T09:44:48.451Z",
            "queue_state": "started",
            "failed": false,
            "run_at": "2020-11-29T18:00:30.000Z"
        },
        {
            "id": 124,
            "client": "demo-client",
            "workflow": "vodcapture",
            "priority": 5,
            "position": 1,
            "created_at": "2020-06-19T15:23:36.197Z",
            "updated_at": "2020-09-07T15:28:12.034Z",
            "queue_state": "started",
            "failed": false,
            "run_at": "2030-06-19T17:00:30.000Z"
        }
    ],
    "queued": [
        {
            "id": 125,
            "client": "demo-client",
            "workflow": "vodcapture",
            "priority": 6,
            "position": 1,
            "created_at": "2019-11-29T10:47:50.431Z",
            "updated_at": "2020-07-13T09:44:48.451Z",
            "queue_state": "queued",
            "failed": false,
            "run_at": "2020-11-29T18:00:30.000Z"
        },
        {
            "id": 126,
            "client": "demo-client",
            "workflow": "vodcapture",
            "priority": 6,
            "position": 1,
            "created_at": "2020-06-19T15:23:36.197Z",
            "updated_at": "2020-09-07T15:28:12.034Z",
            "queue_state": "queued",
            "failed": false,
            "run_at": "2030-06-19T17:00:30.000Z"
        }
    ],
    "scheduled": [
        {
            "id": 122,
            "client": "demo-client",
            "workflow": "vodcapture",
            "priority": 5,
            "position": 1,
            "created_at": "2020-07-13T09:49:27.086Z",
            "updated_at": "2020-07-13T09:49:27.206Z",
            "queue_state": "scheduled",
            "failed": false,
            "run_at": "2030-06-19T17:00:30.000Z"
        }
    ],
    "version": "1.169.3"
}
```

</details><br /><br />

## JOB ENDPOINTS

### POST: `/job`

This endpoint is used to submit jobs to the Task Engine. It is the endpoint used most often. The payload for this endpoint varies substantially depending on the workflow being submitted. More information on the payload properties for each workflow can be found [here](TaskEngineWorkflows.html). Successful job submission will return an `accepted` result and the job id. An error message is returned when a job submission fails.

<details>

<summary>Details</summary>
<br />

**Requires Authentication: Yes**<br />
**Required Headers:**

- `client` - client name, required for authentication
- `api-key` - required for authentication
- `Content-Type` - set to `application/json`

**Optional Headers: None**

Successful Response:

```json
{
    "id": "<job id>",
    "result": "accepted"
}


400 - Error Response:

```json
{
    "id": "<job id>",
    "error": "<error message>"
}
```

500 - Error Response:

```text
    "Unable to create job request"
```

</details><br /><br />

### GET: `/jobs`

This endpoint is used to return a list of jobs from the Task Engine database. Filtering is supported through query string parameters. The default search (no parameters) will return the last 10 jobs.

<details>

<summary>Details</summary>
<br />

**Requires Authentication: No**<br />
**Required Headers: None**<br />
**Optional Headers:None**<br />
**Query String Parameters:**

- `limit` - the maximum number of jobs to return
- `order_by` - order the query by a job property
- `asc` - order the results in ascending or descending order. Accepts 1 (true) or 0 (false)
- `state` - filter by job state. One of the following IDs needs to be specified
  - 0 - queued
  - 1 - started
  - 2 - completed
  - 3 - pending
  - 4 - broken
  - 5 - scheduled
  - 6 - paused
- `from` - used to filter by date range, based on the job creation date
- `to` - used to filter by date range, based on the job creation date
- `failed` - returned failed jobs. If used with a state, jobs will only be returned if state is set to 2 (completed)
- `search` - search term used to filter by eg. the content id for a submitted job
- `job_ids` - comma separated job ids
- `client` - client name to filter by

Successful response for `/jobs?limit=3&client=demo-client&state=2`

```json
[
    {
        "id": 123,
        "client": "demo-client",
        "workflow": "vodcapture",
        "priority": 5,
        "position": 1,
        "created_at": "2020-09-24T11:55:33.755Z",
        "updated_at": "2020-09-24T12:17:33.566Z",
        "queue_state": "completed",
        "parameters": "<parameters submitted when creating the job>",
        "failed": false,
        "run_at": "2020-09-24T11:55:33.755Z"
    },
    {
        "id": 114,
        "client": "demo-client",
        "workflow": "vodcapture",
        "priority": 5,
        "position": 1,
        "created_at": "2020-09-24T11:55:32.971Z",
        "updated_at": "2020-09-24T12:05:18.937Z",
        "queue_state": "completed",
        "parameters": "<parameters submitted when creating the job>",
        "failed": false,
        "run_at": "2020-09-24T11:55:32.971Z"
    },
    {
        "id": 102,
        "client": "demo-client",
        "workflow": "vodcapture",
        "priority": 5,
        "position": 1,
        "created_at": "2020-09-24T11:55:32.031Z",
        "updated_at": "2020-09-24T12:18:33.641Z",
        "queue_state": "completed",
        "parameters": "<parameters submitted when creating the job>",
        "failed": false,
        "run_at": "2020-09-24T11:55:32.031Z"
    }
]
```

400 - Error Response:

```json
{
    "error": "<error message>"
}
```

</details><br /><br />

### GET: `/jobs/<job_id>`

This endpoints returns information about the specified job.

<details>

<summary>Details</summary>
<br />

**Requires Authentication: No**<br />
**Required Headers: None**<br />
**Optional Headers: None**

Successful response for `/jobs/123`

```json
{
    "id": 123,
    "client": "demo-client",
    "workflow": "vodstream",
    "priority": 5,
    "position": 1,
    "top_of_queue": false,
    "parameters": "<parameters submitted when creating the job>",
    "created_at": "2019-09-18T17:13:47.713Z",
    "updated_at": "2019-09-18T17:16:05.215Z",
    "queue_state": "completed",
    "failed": false,
    "run_at": "2019-09-18T17:13:47.713Z"
}
```

400 - Error Response:

```json
{
    "error": "<error message>"
}
```

</details><br /><br />
       
### PUT: `/jobs/<job_id>`

This endpoint is used to update job fields. Only a specific selection of fields can be updated after a job has been submitted. The response will return the job id and the result of the update.

<details>

<summary>Details</summary>
<br />

**Requires Authentication: Yes**<br />
**Required Headers:**

- `client` - client name, required for authentication
- `api-key` - required for authentication
- `Content-Type` - set to `application/json`

**Optional Headers: None**

The list of job fields that can be updated:

- `queue_state` - The queue state for a job can be updated. This can be used to pause or break a job by specifying the values `paused` or `broken` respectively
- `run_at` - Updating the run_at field for a job changes when the job will be queued. The date must be in UTC and in the following format `yyyy-MM-ddTHH:mm:ss.fff`
- `priority` - Updating the priority for a job. More information on job priority can be found [here](TaskEngineWorkflowFeatures.html#priority)
- `sempahore_url` - This url can be used as part of the scheduling process. More information on the semaphore url can be found [here](TaskEngineWorkflowFeatures.html#scheduler)

Sample payload:

```json
{
    "client": "demo-client",
    "priority": "2",
    "run_at": "2020-09-09T14:30:00.000"
}
```

Successful Response:

```json
{
    "id": "<job id>",
    "result": "Performed updates: <list of updates>"
}
```

400 - Error Response:

```json
{
    "id": "<job id>",
    "error": "<error message>"
}
```

</details><br /><br />

### POST: `/jobs/<job id>/rerun`

This endpoint is used to rerun a job with exactly the same parameters. When rerunning a job, the original job's queue state will be set to broken since the content it generated will no longer be valid.

<details>

<summary>Details</summary>
<br />

**Requires Authentication: Yes**<br />
**Required Headers:**

- `client` - client name, required for authentication
- `api-key` - required for authentication

**Optional Headers: None**

Successful Response:

```json
{
    "id": "<job id>",
    "result": "accepted"
}
```

400 - Error Response:

```json
{
    "error": "<error message>"
}
```

</details><br /><br />

### POST: `/push_jobs`

This endpoint is used to force queued jobs to consume any available queue slots. This can happen when a setting, such as the maximum number of concurrent jobs, is updated through the database and does not trigger the functionality for jobs to consume the new queue slots. Successful requests will return an `200 ok` status. An error message is returned when an issue occurs.

<details>

<summary>Details</summary>
<br />

**Requires Authentication: Yes**<br />
**Required Headers:**

- `client` - client name, required for authentication
- `api-key` - required for authentication
- `Content-Type` - set to `application/json`

**Optional Headers: None**

Successful Response:

```
Status: 200, ok 
```

401 - Error Response:

```json
{
    "result": "Unauthorised request."
}
```

500 - Error Response:

```json
{
    "error": "<error message>"
}
```

</details><br /><br />

## LOG ENDPOINTS

### GET: `/logs/<job id>`

This endpoint is used to retrieve the logs for the specified job.

<details>

<summary>Details</summary>
<br />

**Requires Authentication: No**<br />
**Required Headers:**

- `Accept` - set to `application/json`

**Optional Headers: None**

Successful Response:

```json
[
    {
        "id": 691464,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "api",
        "message": "{\"path\":\"POST /job\",\"remote_addr\":\"99.80.104.160\",\"headers\":{\"HTTP_VERSION\":\"HTTP/1.1\",\"HTTP_X_AUTH_KEY\":\"********************************3e2d\",\"HTTP_API_KEY\":\"********************************3e2d\",\"HTTP_X_API_KEY\":\"********************************3e2d\",\"HTTP_ACCEPT\":\"application/json, application/xml, text/json, text/x-json, text/javascript, text/xml\",\"HTTP_USER_AGENT\":\"RestSharp/106.3.1.0\",\"HTTP_HOST\":\"taskengine.demo-client.vualto.com\",\"HTTP_ACCEPT_ENCODING\":\"gzip, deflate\"},\"parameters\":{\"client\":\"demo-client\",\"parameters\":{\"folder\":\"a40fdaff-f904-4ea5-b893-a89152708952\",\"content_id\":\"a40fdaff-f904-4ea5-b893-a89152708952\",\"rest_endpoints\":[\"https://vis.controlhub.demo-client.vualto.com/api/event/vuflow/taskenginecallback\",\"https://admin.controlhub.demo-client.vualto.com/vod/PublishVuflowData\"]},\"job\":{\"workflow\":\"drmswitch\"}},\"payload\":\"job_created\"}",
        "created_at": "2020-07-06T12:48:49.591Z",
        "updated_at": "2020-07-06T12:48:49.591Z",
        "task_id": 0,
        "visible": true
    },
    {
        "id": 691465,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "No client definitions. Using common definitions.",
        "created_at": "2020-07-06T12:48:50.079Z",
        "updated_at": "2020-07-06T12:48:50.079Z",
        "task_id": 24100,
        "visible": true
    },
    ...
    ...
    ...
    {
        "id": 691516,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "'rename_manifests' completed successfully",
        "created_at": "2020-07-06T12:48:52.282Z",
        "updated_at": "2020-07-06T12:48:52.282Z",
        "task_id": 24102,
        "visible": true
    },
    {
        "id": 691519,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "controller",
        "message": "job has completed successfully",
        "created_at": "2020-07-06T12:48:52.684Z",
        "updated_at": "2020-07-06T12:48:52.684Z",
        "task_id": 24102,
        "visible": true
    },
    {
        "id": 691520,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "callback",
        "message": "No client definitions. Using common definitions.",
        "created_at": "2020-07-06T12:48:53.421Z",
        "updated_at": "2020-07-06T12:48:53.421Z",
        "task_id": 24102,
        "visible": true
    },
]
```

400 - Error Response:

```json
{
    "error": "<error message>"
}
```

</details><br /><br />

## SCHEDULER ENDPOINTS

### GET: `/schedules`

Returns a list of the currently active schedules. More information about the Task Engine scheduler can be found [here](TaskEngineWorkflowFeatures.html#Scheduler)

<details>

<summary>Details</summary>
<br />

**Requires Authentication: Yes**<br />
**Required Headers:**

- `client` - client name, required for authentication
- `api-key` - required for authentication

**Optional Headers:None**

Successful Response:

```json
{
    "result": "ok",
    "schedules": {
        "queue_scheduled_jobs": {
            "class": "QueueJobs",
            "every": [
                60,
                {
                    "first_in": 5
                }
            ],
            "queue": "scheduler",
            "description": "Enqueues scheduled jobs that have a run_at time in the past."
        }
    }
}
```

400 - Error Response:

```json
{
    "error": "<error message>"
}
```

</details><br /><br />

### PUT: `/scheduler`

This endpoint allows for activating or deactivating schedules. More information about the Task Engine scheduler can be found [here](TaskEngineWorkflowFeatures.html#Scheduler)

<details>

<summary>Details</summary>
<br />

**Requires Authentication: Yes**<br />
**Required Headers:**

- `client` - client name, required for authentication
- `api-key` - required for authentication

**Optional Headers:None**

Payload parameters:

- `schedule` - name of the schedule to be activated or deactivated
- `active` - accepts true or false to set the schedule to active or inactive

Sample Payload:

```json
{
    "schedule": "<schedule name>",
    "active": true
}
```

Successful Response:

```json
{
    "result": "ok",
    "schedules": ["<list of schedules>"]
}
```

400 - Error Response

```json
{
    "error": "<error message>"
}
```

</details><br /><br />

## SETTINGS ENDPOINTS

### POST: `/settings`

This settings endpoint is used to update or create new Task Engine settings. Only one setting can be added or updated at a time.

<details>

<summary>Details</summary>
<br />

System default settings:

- `max_jobs` - The maximum number of concurrent jobs. Default: 2
- `priority_slots` - The number of concurrent job slots that should be reserved for high priority jobs. More information can be found [here](TaskEngineWorkflowFeatures.html#priority-slots). Default: 0
- `priority_threshold` - The threshold at which jobs will start being considered as priority. Default: 5.
- `schedule_interval` - The interval, in seconds, between scheduler executions. Default: 60
- `retry_delay` - The delay, in seconds, between retries for failed Resque tasks. Default: 5
- `retry_limit` - The number of times a Resque task should be retried before a job is abandoned. Default: 3

**Requires Authentication: Yes**<br />
**Required Headers:**

- `client` - client name, required for authentication
- `api-key` - required for authentication
- `content-type` - set to `application/json`

**Optional Headers:None**

Payload parameters:

- `name` - setting name from the list above or name for a new setting
- `setting` - the value to be given to that setting

Sample Payload:

```json
{
    "name": "max_jobs", // setting name
    "setting": "4" // value
}
```

Successful Response:

```json
{
    "result": "ok",
    "message": "<setting name> setting created/updated"
}
```

400 - Error Response

```json
{
    "error": "<error message>"
}
```

</details>
