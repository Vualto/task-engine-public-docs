# TASK ENGINE

## Introduction to Task Engine

The Task Engine is a product that facilitates the creation of VOD from online and offline sources. It is designed to allow simple or complicated custom workflows to be developed, deployed and maintained easily and quickly. The Task Engine is exposed via a REST API and reports back to the client system via a callback mechanism.

### How it works

The Task Engine breaks a workflow up in to several component parts. Every piece of work submitted is a job, and each job is broken up in to a series of tasks. These tasks are then scheduled on to queues which workers then process.

Every job and task have unique identities, and these are exposed back to the client system both through the response to the initial job submission and through the callbacks (unless for some reason the configuration prevents this from happening).

Callbacks are sent to the specified endpoints when each task starts and ends (successful or fail). This allows the integrated systems to keep track of the job's progress. A final callback is also submitted when a job completes. The final callback will always contain the status of the job (success or fail) and information about the assets related to the job.

### System Requirements

The Task Engine is designed to run in Docker containers, and is therefore able to run on any Docker-supported Linux host. Recent releases of the Task Engine are also compatible with Kubernetes deployments and are fully integrated with Rancher for easy deployment and upgrade management. Any workers that need to run on Windows are currently **not** containerized but are installed as a native Windows service. Windows workers are a requirement for frame accurate captures.

When a worker starts a task, it will generally need to copy one or more files to storage to perform its work in an efficient manner, so it’s critical that hosts have sufficient disk space for processing to take place. This is particularly important when dealing with video transcoding or DRM encryption. A shared storage is also used to store any assets required during job execution and facilitate horizontal scaling of workers. Vualto will usually advise on disk space requirements once the workflow is fully defined and some representative test content has been processed.

## Integration

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