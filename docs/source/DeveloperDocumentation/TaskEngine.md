# INTEGRATION

There are two main integration points for the Task Engine:

**API** – how jobs are submitted to the Task Engine.

**Callbacks** – how the Task Engine notifies client systems of job progress.

### API

The API is mainly used to trigger [workflows](TaskEngineWorkflows.md) within the Task Engine but additional API endpoints are available for job management. Full API documentation can be found [here](TaskEngineAPI.html)

### Callbacks

Callbacks are used to notify a integrated services with workflow execution updates. Callback URLs are submitted as part of the payloads and the Task Engine will send callbacks when:

1. A task starts
2. A task ends (success or fail)
3. A job ends (success or fail)

All default task callbacks will the contain the same JSON body structure:

```json
"job_id": "<job id>",
"task_id": "<task id>",
"task_name": "<task name>",
"workflow": "<workflow name>",
"event": "<task event>",
"content_id": "<content_id>",
"message": "<task exception message>"
```

Job callbacks vary depending on the workflow being executed but the following are common in all workflows.

```json
"job_id": "<job id>",
"status": "<job status>",
"workflow": "<workflow name>",
"content_id": "<content id>",
"custom_data": "<client custom data>"
```

More information of the callbacks for each workflow can be found [here](TaskEngineWorkflows.md).

Specifying the Vualto Control Hub Video Information Service web-hook (`https://vis.controlhub.[client].vualto.com/api/event/vuflow/taskenginecallback`) as a callback url, will be add the asset as a VOD event within the Vualto Control Hub CMS. A second CMS web-hook (`https://admin.controlhub.[client].vualto.com/vod/PublishVuflowData`) can also be included for realtime updates on the status of the job.

As Task Engine is an integration product that can be customised, any specific requirements are easily catered for (eg. setting authentication headers) and the body of a callbacks may also be modified.

## Authentication

All Task Engine API calls that require authentication currently use the client name and API key provided by Vualto. The credentials should be supplied as `client` and `api-key` headers respectively.