# TASK ENGINE API

- [Introduction to Task Engine](#introduction-to-task-engine)
    - [How it works](#how-it-works)
    - [System Requirements](#system-requirements)
- [Integration](#integration)
- [Triggers](#triggers)
    - [Workflow Trigger Example](#workflow-trigger-example)
- [Rest Endpoint Callbacks](#rest-endpoint-callbacks)

## Introduction to Task Engine

Task Engine is a product that facilitates easy integration between client systems and the vudrm platform.  It is designed to allow simple or complicated workflows to be constructed and tested easily and quickly.  Task Engine is exposed via a simple API and reports back to the client system via any required mechanism, although usually this is as simple as an HTTP POST to an endpoint.

### How it works

Task Engine breaks a workflow up in to several component parts.  Every piece of work submitted is a job, and each job is broken up in to a series of tasks.  These tasks are then scheduled on to queues which workers then process.

Every job and task have unique identities, and these are exposed back to the client system both through the response to the initial job submission and through the callbacks (unless for some reason the configuration prevents this from happening).

When a task is being processed, a callback is sent through the configured mechanism as it starts and when it completes or fails, this way the client system should be able to track the progress of a job as it progresses through the task engine and then perform any relevant tasks when the job as a whole is completed.

### System Requirements

The Task Engine is designed to run in Docker containers, and is therefore able to run on any Docker-supported Linux host, preferably running whatever the latest version of Docker is at the time (currently 1.12.1).  Any workers that need to run on Windows are currently NOT containerized but are installed as a native Windows service – this will change as Docker support for Windows matures.

When a worker starts a task, it will generally need to copy one or more files to local storage to perform its work in an efficient manner, so it’s critical that hosts have sufficient disk space for processing to take place; this is particularly important when dealing with video transcoding or DRM encryption.  This also means that sometimes running more than one worker per host is not an option.  Vualto will usually advise on disk space requirements once the workflow is fully defined and some representative test content has been processed.

## Integration

There are two main integration points for the task engine:

**Triggers** – how you notify task engine that a workflow needs to be executed, usually initiated by a call to the Task Engine API.

**Callbacks** – how we notify client systems of job progress.

## Triggers

While the task engine can be configured to use watch folders or other methods of notification (currently outside the scope of this document) to trigger a workflow, the most common route is to have the client system trigger a workflow via an API call.

There is a single endpoint, /job, that handles all requests, only the payload varies according to the requirements of the workflow.  As most clients have varying workflows, this  document will give an examples of some of the most common workflows.
 
Custom workflows are supported, however; these would require additional development. The main differences will be the name of the workflow and the parameters passed in the ‘parameters’ section.

### Workflow Trigger Example

An example call to the the Task Engine API to trigger a workflow would be as follows ( Parameters must be a parameters object):

```curl
  curl -X POST \    
  -H "Content-Type: application/json" \
  -H "API-KEY: <client_api_key>" \
  -d '{"client": "<client_name>",
  "job": { "workflow": "<workflow_name>" },    
  "parameters": {
      "content_id": "<unique_content_id>",
      .
      .
      .
      }}' "http://<vuflow_server>/job"
```     


The Headers are defined as follows:

| Header            | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| API-KEY           | The API-KEY assigned to the client.  

More information about specific workflows can be found [here](TaskEngineWorkflows.md).

## Rest Endpoint Callbacks

Specifying the Task Engine `https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback` “webhook” in the callback will register the VOD asset within the Vualto vuflow events management system. It’s recommended and this is optional but does mean you can view the VOD event within `https://admin.vuworkflow.staging.vualto.com`. This is also required for VOD assets to automatically go into private mode (configurable) after ingestion.


The callbacks will send JSON like below. Please note that ```status``` can come back as ```failed``` or ```completed.```
You will also be concerned with the **message**. This will be the resulting filename after creating the VOD stream or switching DRM. For example, you will require this for building your streaming Urls. For DASH it would look something like:

```…/054b58a1-8f05-4f08-bb17-b4698e3e9d1d_drm_57d82537-1775-4e29-97e4-bba86e0219fb.ism/.mpd``` 

Retrieving Urls is also possible within the VIS API if you don't want to build and generate these Urls yourself. All callbacks will send the following header and value. This can be used to validate the request:

```Workflow-Id: 52ec321b-8da3-4f5f-9cd3-749789577cd3```


Before and after each step in a workflow, Task Engine can provide a callback to a given endpoint.
Given that each workflow is different, the specific task names will vary, however the callbacks will generally be made as follows:


    curl –X POST \
    -H "Content-Type: application/json" \
    -H "<any_required_header>: <any_required_value>" \
    … \
    -d '{"job_id": <job_id>,
        "task_id": <task_id>,
        "task_name": "<name>",
        "content_id": "<content_id>", 
        "event": "<start | complete | fail>",
        "message": "<message>",
        "datetime": "yyyy-mm-ddThh:mm:ssZ"}' \
        "<configured_callback_endpoint>"

As Task Engine is an integration product that can be customised, any specific requirements are easily catered for (i.e. specific headers being set for authentication, notification via SNS, RabbitMQ, email etc), although the data available to be sent will remain the same as listed above.