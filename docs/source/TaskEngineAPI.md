# TASK ENGINE API

The Task Engine endpoints will always return a JSON response unless explicitly indicated otherwise.

## Status Endpoints

### GET: `/`

This endpoint will check if the Task Engine endpoint is reachable.

<details>

<summary>Details</summary>
<br /><br />

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

The health endpoint will run checks on the different Task Engine components and returns the status of each service. The endpoint will also return some information about the Task Engine and some statistics about jobs and tasks.

<details>

<summary>Details</summary>
<br /><br />

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

The dashboard endpoint returns information about the current Task Engine queue status. The information includes lists of started, queued and scheduled jobs as well as the setting information for the maximum concurrent jobs and the number of priority reserved slots and the Task Engine version running.

<details>

<summary>Details</summary>
<br /><br />

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

## Job Endpoints

### POST: `/job`

This endpoint is used to submit jobs to the Task Engine. It is the endpoint used most often. The payload for this endpoint varies substantially depending on the workflow to be executed. More information on the payload properties for each workflow can be found [here](TaskEngineWorkflows.html). Successful job submission will return an `accepted` result and the job id. An error message is returned when a job submission fails.

<details>

<summary>Details</summary>
<br /><br />

**Requires Authentication: Yes**<br />
**Required Headers:**

- `client` - client name, required for authentication
- `api-key` - required for authentication
- `Content-Type` - set to `application/json`

**Optional Headers: None**

Successful Response:

```json
{
    "id": "123", // job id
    "result": "accepted"
}
```

400 - Error Response:

```json
{
    "id": "123", // job id
    "error": "<error message>"
}
```

500 - Error Response:

```text
    "Unable to create job request"
```

</details><br /><br />

### GET: `/jobs`

This endpoint is used to return a list of jobs from the Task Engine database. Filtering is supported through query string parameters but by default the the endpoint will return the last 10 successfully submitted jobs.

<details>

<summary>Details</summary>
<br /><br />

**Requires Authentication: No**<br />
**Required Headers: None**<br />
**Optional Headers:None**<br />
**Query String Parameters:**

- `items` - the maximum number of jobs to return
- `order_by` - field used to order the query by a job property
- `asc` - filed used to order the results in ascending order. Accepts 1 (true) or 0 (false)
- `state` - filter by job state. The ID needs to be specified
  - 0 - queued
  - 1 - started
  - 2 - completed
  - 3 - pending
  - 4 - broken
  - 5 - scheduled
  - 6 - paused
- `from` - used to filter by date range, based on the job creation date
- `to` - used to filter by date range, based on the job creation date
- `failed` - filter to only returned failed jobs. If used with a state, jobs will only be returned if state is set to 2 (completed)
- `search` - a search term used to filter by eg. the content id for a submitted job
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
<br /><br />

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
    "parameters": {
        "content_id": "832751d6-f94f-44b3-b30d-0281bebfb0ea",
        "output_folder": "832751d6-f94f-44b3-b30d-0281bebfb0ea",
        "clips": [
            {
                "source": "http://demo.video.stream/live/41f7b71b-fd1a-431e-ab07-0e39909d4fd1/live.isml/Manifest",
                "start": "2019-09-18T16:55:53.000",
                "end": "2019-09-18T17:08:20.520"
            }
        ],
        "encrypted": false,
        "frame_accurate": true,
        "copy_ts": true,
        "rest_endpoints": [
            "https://vis.controlhub.demo-client.vualto.com/api/event/vuflow/taskenginecallback",
            "https://admin.controlhub.demo-client.vualto.com/vod/PublishVuflowData"
        ],
        "generate_vod": true,
        "create_thumbnail": true,
        "thumbnail_time": "00:04:03.120",
        "generate_mp4": false,
        "create_dref": true,
        "mezzanine": true,
        "apply_track_properties": false
    },
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

### PUT: `/job/<job_id>`

This endpoint is used to update specific fields of a job. The response will return the job id and the result of the update.

<details>

<summary>Details</summary>
<br /><br />

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
- `sempahore_url` - This url can be used as part of the scheduling process. More information on the semaphore url can be found [here](TaskEngineWorkflowFeatures.html#semaphore-url)

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

This endpoint is used to rerun a job with exactly the same parameters. When rerunning a job, the original job will be set to broken.

<details>

<summary>Details</summary>
<br /><br />

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

## Log Endpoints

### GET: `/logs/<job id>`

This endpoint is used to retrieve the logs for the specified job.

<details>

<summary>Details</summary>
<br /><br />

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
    {
        "id": 691466,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "Task 'get_filenames' started",
        "created_at": "2020-07-06T12:48:50.089Z",
        "updated_at": "2020-07-06T12:48:50.089Z",
        "task_id": 24100,
        "visible": true
    },
    {
        "id": 691469,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "Root key: output_root Root Folder: content/vod",
        "created_at": "2020-07-06T12:48:50.110Z",
        "updated_at": "2020-07-06T12:48:50.110Z",
        "task_id": 24100,
        "visible": true
    },
    {
        "id": 691470,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "'get_filenames' completed successfully",
        "created_at": "2020-07-06T12:48:50.204Z",
        "updated_at": "2020-07-06T12:48:50.204Z",
        "task_id": 24100,
        "visible": true
    },
    {
        "id": 691474,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "callback",
        "message": "No client definitions. Using common definitions.",
        "created_at": "2020-07-06T12:48:51.100Z",
        "updated_at": "2020-07-06T12:48:51.100Z",
        "task_id": 24100,
        "visible": true
    },
    {
        "id": 691476,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "No client definitions. Using common definitions.",
        "created_at": "2020-07-06T12:48:51.197Z",
        "updated_at": "2020-07-06T12:48:51.197Z",
        "task_id": 24101,
        "visible": true
    },
    {
        "id": 691477,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "Task 'prepare_switch' started",
        "created_at": "2020-07-06T12:48:51.203Z",
        "updated_at": "2020-07-06T12:48:51.203Z",
        "task_id": 24101,
        "visible": true
    },
    {
        "id": 691479,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "switching to DRM manifest",
        "created_at": "2020-07-06T12:48:51.211Z",
        "updated_at": "2020-07-06T12:48:51.211Z",
        "task_id": 24101,
        "visible": true
    },
    {
        "id": 691480,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "Activating content/vod/a40fdaff-f904-4ea5-b893-a89152708952/1594039706_9529e5b3-14f0-4b3a-bb9c-97f5615a0d96_a40fdaff-f904-4ea5-b893-a89152708952.drm. New manifest: a40fdaff-f904-4ea5-b893-a89152708952_drm_7109e03e-9a62-4cbb-858f-6d79305e2d08.ism",
        "created_at": "2020-07-06T12:48:51.213Z",
        "updated_at": "2020-07-06T12:48:51.213Z",
        "task_id": 24101,
        "visible": true
    },
    {
        "id": 691481,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "switching to DRM manifest",
        "created_at": "2020-07-06T12:48:51.216Z",
        "updated_at": "2020-07-06T12:48:51.216Z",
        "task_id": 24101,
        "visible": true
    },
    {
        "id": 691482,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "Activiating content/vod/a40fdaff-f904-4ea5-b893-a89152708952/1594039706_9529e5b3-14f0-4b3a-bb9c-97f5615a0d96_a40fdaff-f904-4ea5-b893-a89152708952_aes.drm. New manifest: a40fdaff-f904-4ea5-b893-a89152708952_drm_7109e03e-9a62-4cbb-858f-6d79305e2d08_aes.ism",
        "created_at": "2020-07-06T12:48:51.218Z",
        "updated_at": "2020-07-06T12:48:51.218Z",
        "task_id": 24101,
        "visible": true
    },
    {
        "id": 691483,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "De-activiating content/vod/a40fdaff-f904-4ea5-b893-a89152708952/a40fdaff-f904-4ea5-b893-a89152708952_nodrm_771f5e37-97c0-476c-a1f7-43cceeb60585.ism. New name: 1594039731_bd0c779e-a4a6-45d2-bbe9-8485b4e61703_a40fdaff-f904-4ea5-b893-a89152708952.nodrm",
        "created_at": "2020-07-06T12:48:51.220Z",
        "updated_at": "2020-07-06T12:48:51.220Z",
        "task_id": 24101,
        "visible": true
    },
    {
        "id": 691484,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "'prepare_switch' completed successfully",
        "created_at": "2020-07-06T12:48:51.226Z",
        "updated_at": "2020-07-06T12:48:51.226Z",
        "task_id": 24101,
        "visible": true
    },
    {
        "id": 691486,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "callback",
        "message": "No client definitions. Using common definitions.",
        "created_at": "2020-07-06T12:48:51.366Z",
        "updated_at": "2020-07-06T12:48:51.366Z",
        "task_id": 24100,
        "visible": true
    },
    {
        "id": 691491,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "callback",
        "message": "No client definitions. Using common definitions.",
        "created_at": "2020-07-06T12:48:51.602Z",
        "updated_at": "2020-07-06T12:48:51.602Z",
        "task_id": 24101,
        "visible": true
    },
    {
        "id": 691494,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "No client definitions. Using common definitions.",
        "created_at": "2020-07-06T12:48:51.820Z",
        "updated_at": "2020-07-06T12:48:51.820Z",
        "task_id": 24102,
        "visible": true
    },
    {
        "id": 691495,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "Task 'rename_manifests' started",
        "created_at": "2020-07-06T12:48:51.828Z",
        "updated_at": "2020-07-06T12:48:51.828Z",
        "task_id": 24102,
        "visible": true
    },
    {
        "id": 691498,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "Root key: output_root Root Folder: content/vod",
        "created_at": "2020-07-06T12:48:51.851Z",
        "updated_at": "2020-07-06T12:48:51.851Z",
        "task_id": 24102,
        "visible": true
    },
    {
        "id": 691499,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "worker",
        "message": "S3 folder: content/vod/a40fdaff-f904-4ea5-b893-a89152708952, Active Manifest a40fdaff-f904-4ea5-b893-a89152708952_drm_7109e03e-9a62-4cbb-858f-6d79305e2d08.ism",
        "created_at": "2020-07-06T12:48:51.854Z",
        "updated_at": "2020-07-06T12:48:51.854Z",
        "task_id": 24102,
        "visible": true
    },
    {
        "id": 691502,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "callback",
        "message": "No client definitions. Using common definitions.",
        "created_at": "2020-07-06T12:48:51.869Z",
        "updated_at": "2020-07-06T12:48:51.869Z",
        "task_id": 24101,
        "visible": true
    },
    {
        "id": 691512,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "callback",
        "message": "No client definitions. Using common definitions.",
        "created_at": "2020-07-06T12:48:52.178Z",
        "updated_at": "2020-07-06T12:48:52.178Z",
        "task_id": 24102,
        "visible": true
    },
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
    {
        "id": 691523,
        "job_id": 123,
        "severity": 1,
        "severity_description": "INFO",
        "progname": "callback",
        "message": "No client definitions. Using common definitions.",
        "created_at": "2020-07-06T12:48:53.653Z",
        "updated_at": "2020-07-06T12:48:53.653Z",
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

## Scheduler Endpoints

### GET: `/schedules`

Returns a list of the currently active schedules. More information about the Task Engine scheduler can be found [here](TaskEngineWorkflowFeatures.html#Scheduler)

<details>

<summary>Details</summary>
<br /><br />

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
<br /><br />

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
    "schedule": "queue_scheduled_jobs", // schedule name
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

## Settings Endpoints

### POST: `/settings`

This settings endpoint is used to update or create new Task Engine settings. Only one setting can be added or updated at a time. Check the details below to the default settings, their values and their purpose.

<details>

<summary>Details</summary>
<br /><br />

System default settings:

- `max_jobs` - The maximum number of concurrent jobs. Default: 2
- `priority_slots` - The number of concurrent job slots that should be reserved for high priority jobs. More information can be found [here](TaskEngineWorkflowFeatures.html#priority-slots). Default: 0
- `schedule_interval` - The interval, in seconds, between the scheduler executions. Default: 60
- `retry_delay` - The delay, in seconds, between retries for failed Resque tasks. Default: 5
- `retry_limit` - The number of times a Resque task should be retried before a job is failed. Default: 3

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
