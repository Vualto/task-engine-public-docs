# Vuworkflow API

# 1. Introduction to vuworkflow

vuworkflow is a product that facilitates easy integration between client systems and the vudrm platform.  It is designed to allow simple or complicated workflows to be constructed and tested easily and quickly.  vuworkflow is exposed via a simple API and reports back to the client system via any required mechanism, although usually this is as simple as an HTTP POST to an endpoint.

## 1.1 How it works

vuworkflow breaks a workflow up in to several component parts.  Every piece of work submitted is a job, and each job is broken up in to a series of tasks.  These tasks are then scheduled on to queues which workers then process.

Every job and task have unique identities, and these are exposed back to the client system both through the response to the initial job submission and through the callbacks (unless for some reason the configuration prevents this from happening).

When a task is being processed, a callback is sent through the configured mechanism as it starts and when it completes or fails, this way the client system should be able to track the progress of a job as it progresses through vuworkflow and then perform any relevant tasks when the job as a whole is completed.

## 1.2 System Requirements

vuworkflow is designed to run in Docker containers, and is therefore able to run on any Docker-supported Linux host, preferably running whatever the latest version of Docker is at the time (currently 1.12.1).  Any workers that need to run on Windows are currently NOT containerized but are installed as a native Windows service – this will change as Docker support for Windows matures.

When a worker starts a task, it will generally need to copy one or more files to local storage to perform its work in an efficient manner, so it’s critical that hosts have sufficient disk space for processing to take place; this is particularly important when dealing with video transcoding or DRM encryption.  This also means that sometimes running more than one worker per host is not an option.  Vualto will usually advise on disk space requirements once the workflow is fully defined and some representative test content has been processed.

## 1.3 Integration

There are two main integration points for vuworkflow:

**Triggers** – how you notify vuworkflow that a workflow needs to be executed, usually initiated by a call to the vuworkflow API.

**Callbacks** – how we notify client systems of job progress.


# 2. Triggers


While vuworkflow can be configured to use watch folders or other methods of notification (currently outside the scope of this document) to trigger a workflow, the most common route is to have the client system trigger a workflow via an API call.

There is a single endpoint, /job, that handles all requests, only the payload varies according to the requirements of the workflow.  As most clients have varying workflows, this document will give an examples of some of the most common workflows :

**vodstream:**  This workflow will create a server side ISM manifest, with or without DRM, that can be used for on the fly delivery via USP.

**voddownload:**  The workflow will create packaged content, with or without DRM. This uses an the USP offline packager to create each of the output formats HLS, MSS, DASH, HDS.

**vodcapture:**  This workflow allows you to create a frame accurate vod clip by passing in a start and end UTC time stamp. The result will be an MP4 on disk.

**voddeletes3:**  This workflow allows you to delete content on S3.

**drmswitch:**  This workflow allows you to toggle DRM on and off.
 
Custom workflows can be easily created , the only difference will be the name of the workflow and the parameters passed in the ‘parameters’ section.

## 2.1 Workflow Trigger Example
 
 An example call to the vuworkflow API to trigger a workflow would be as follows ( Parameters must be a parameters object):
 
  
    curl -X POST \    
    -H "Content-Type: application/json" \
    -H "API-KEY: <client_api_key>" \
    -d '{"client": "<client_name>",
    "job": { "workflow": "<workflow_name>" },    
    "parameters": {
        "content_id": "<unique_content_id>",
        :
        :
        }}' "http://<vuflow_server>/job"
        


The Headers are defined as follows:

| Header            | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| API-KEY           | The API-KEY assigned to the client.  


## 2.1.1 vodstream


| Parameter Name    | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| workflow          | REQUIRED Specify 'vodstream'.
| content_id        | REQUIRED Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system.|
| source_folder     | Location of the source files. All files to be processed will need to be in a discrete folder, the ‘root’ folder will be specified in the client configuration.      
| output_folder     | OPTIONAL The folder for processed files to be placed.  The ‘root’ folder will be specified in the client configuration.   
| drm               | OPTIONAL The type of DRM that is required. This could be “playready” and/or ”widevine” and/or ”access” or “all”. If this value isn’t present then no DRM is applied. 
| rest_endpoints    | Multiple end points can be specified.

### 2.1.1.1 vodstream JSON Request example

```json
{
  "client": "staging",
  "job": {
    "workflow": "vodstream"
  },
  "parameters": {
    "content_id": "demo1",
    "source_folder": "mz-ast-2055fcff-8cca-4e37-85b9-9647dbe50398-1",
    "output_folder": "mz-ast-2055fcff-8cca-4e37-85b9-9647dbe50398-1",
    "encrypted": true,
    "drm": [
      "fairplay",
      "playready",
      "widevine"
    ],
    "rest_endpoints": [
      "https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
      "http://aaa.com/end",
      "http://bbb.com/end"
    ]
  }
}
```

## 2.1.2 voddownload

| Parameter Name    | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| workflow          | REQUIRED Specify 'voddownload'.
| source_folder     | All files to be processed will need to be in a discrete folder, the ‘root’ folder will be specified in the client configuration.                      
| output_folder     | OPTIONAL The folder for processed files to be placed.  The ‘root’ folder will be specified in the client configuration 
| drm               | OPTIONAL The type of Output DRM that is required. This could be “playready” and/or  ”widevine” and/or ”access” or “all”. If this value isn’t present then no DRM is applied.
| formats           | OPTIONAL The output format that is required. This could be “hls” and/or ”mss” and/or ”dash” or “ismv”. This parameter will be reliant on correct drm options being set.
| rest_endpoints    | Multiple end points can be specified.


## 2.1.3 vodcapture

| Parameter Name    | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| workflow          | REQUIRED Specify 'vodcapture'.
| stream_url        | This would need to be either an HLS or MSS stream URL to the Live or Archive content. e.g. http://mydomain.com/test.ism/.m3u8 , http://mydomain.com/test.ism/manifest
| timecode_in       | UTC timestamp for the start timecode. e.g 2016-10-13T10:10:40.251Z OR Offsets e.g. “hh:mm:ss”
| timecode_out      | UTC timestamp for the end timecode e.g 2016-10-13T10:20:40.251Z OR Offsets e.g. “hh:mm:ss”
| drm               | Optional The type of Output DRM that is required. This could be “playready” and/or  ”widevine” and/or ”access” or “all”.  If this value isn’t present then no DRM is applied.
| drm_key_keyid     | Should the stream be DRM’d we would require the KeyID 
| drm_key_key       | Should the stream be DRM’d we would require the Content Key 
| filter_expression | This allows you to pass filter expressions to select certain video, audio tracks. e.g. to all video bitrates below 8Mbps and all audio bitrates at 64Kbps ((trackName==%22video%22%26%26systemBitrate==800000)%7C%7C(trackName==%22audio_1%22%26%26systemBitrate==64000))
| rest_endpoints    | Multiple end points can be specified.


## 2.1.4 voddelete S3

| Parameter Name    | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| workflow          | REQUIRED Specify 'voddeletes3'.
| content_id        | REQUIRED Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system.|
| folder            | REQUIRED Folder where the content to be deleted resides
| rest_endpoints    | Multiple end points can be specified.

### 2.1.4.1 voddelete S3 JSON Request example

```json
{
  "client": "staging",
  "job": {
    "workflow": "voddeletes3"
  },
  "parameters": {
    "content_id": "demo1",
    "folder": "vualto-test-1",
    "rest_endpoints": [
			"https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
			"http://your.custom.endpoint"
		]
  }
}
```

## 2.1.5 drmswitch 

| Parameter Name    | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| workflow          | REQUIRED Specify 'drmswitch'.
| content_id        | REQUIRED Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system.|
| folder            | REQUIRED Folder where the content to be DRM toggled resides
| rest_endpoints    | Multiple end points can be specified.


### 2.1.5.1 drmswitch JSON example

```json
{
  "client": "staging",
  "job": {
    "workflow": "drmswitch"
  },
  "parameters": {
    "content_id": "demo1",
    "folder": "vualto-test-1",
    		"rest_endpoints": [
			"https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
			"http://your.custom.endpoint"
		]
  }
}
```


## 2.2 vodstream Workflow Trigger Example

Example of a curl command to trigger ingest for the vodstream workflow:

    curl -X POST \
    -H "Content-Type: application/json" 
    -H "API-KEY: aabbccdd-1122-3344-5566-eeff77889900" \
    -d '{"client": "vualto", "job": { "workflow": "vodstream" }, "parameters": {"content_id": 
    "demo1", "source_folder":"/input/demo1", "output_folder": "/test", "drm": ["playready", 
    "widevine"], "formats": ["vodmanifest"]}}' \"https://vuflow.example.com/job" 

This results in the files <content_id>.ism and <content_id>.ismv being produced in the folder: 

```<configured_root>(/<optional_output_folder>)/<content_id>.```

The response from this call should be either a 200 OK, with the following payload:

``` { "job_id": <job_id>, "result": "accepted" } ```

or a 400 BAD REQUEST with the following payload:

``` { "error": "<description_of_error>" } ```

Assuming the call is successful, this would add an ingest job to the vuworkflow queue, with the files to be ingested expected to be in the following location:

``` <input_root>/input/demo1 ```

If the process completes successfully, then the output would be the following files:

``` <output_root>/test/demo1.ism ```
``` <output_root>/test/demo1.ismv ```

If the ‘output_folder’ parameter was excluded, then the files would be output to the following locations:

``` <output_root>/demo1.ism ```
``` <output_root>/demo1.ismv ```

**NOTE:** there may be some additional files, depending on the exact processes involved, but the minimum would usually be these.

#3. Rest Endpoint Callbacks

Specifying the vuworkflow ```https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback``` “webhook” in the callback will register the VOD asset within the Vualto vuflow events management system. It’s recommended and this is optional but does mean you can view the VOD event within ```https://admin.vuworkflow.staging.vualto.com```. This is also required for VOD assets to automatically go into private mode (configurable) after ingestion.


The callbacks will send JSON like below. Please note that ```status``` can come back as ```failed``` or ```completed.```
You will also be concerned with the **message**. This will be the resulting filename after creating the VOD stream or switching DRM. For example, you will require this for building your streaming Urls. For DASH it would look something like:

```…/054b58a1-8f05-4f08-bb17-b4698e3e9d1d_drm_57d82537-1775-4e29-97e4-bba86e0219fb.ism/.mpd``` 

Retrieving Urls is also possible within the VIS API if you don't want to build and generate these Urls yourself. All callbacks will send the following header and value. This can be used to validate the request:

```Workflow-Id: 52ec321b-8da3-4f5f-9cd3-749789577cd3```


Before and after each step in a workflow, vuworkflow can provide a callback to a given endpoint.
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

As vuworkflow is an integration product that can be customised, any specific requirements are easily catered for (i.e. specific headers being set for authentication, notification via SNS, RabbitMQ, email etc), although the data available to be sent will remain the same as listed above.


The Parameters are defined as follows:


| Parameter Name    | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| job_id            | Unique job id, assigned by vuworkflow
| task_id           | OPTIONAL Unique task id.  Each task in a jobs’ workflow has a unique id.  If this value is not present then the callback relates to the job as a whole rather than a specific task.
| task_name         | OPTIONAL Name of the task to which the event related.  If this value is not present then the callback relates to the job as a whole rather than a specific task.
| content_id        | Content ID provided by the client when the job was created
| event             | This relates to the event being fired, will be one of start, complete, fail.
| message           | Any message associated with the event.  For example, if the event is ‘fail’ then ‘message’ will contain a description of the failure.
| datetime          | The date and time of the event in UTC.



Example JSON VOD
``` JSON
{
	"job_id": "21",
	"status": "completed",
	"workflow": "vodstream",
	"content_id": "d35517bc-9010-4dc5-b2f9-ecd3c20a5969",
	"message": "054b58a1-8f05-4f08-bb17-b4698e3e9d1d_drm_57d82537-1775-4e29-97e4-bba86e0219fb.ism"
}
```

Example JSON VOD Delete
```
{
	"job_id": "21",
	"status": "completed",
	"workflow": "voddeletes3",
	"content_id": "d35517bc-9010-4dc5-b2f9-ecd3c20a5969",
	"message": "d35517bc-9010-4dc5-b2f9-ecd3c20a5969"
}
```


#4. Public/Private switching

VIS API [vuflow protection](http://readme.vualto.com/vis-api/#put-eventvuflowprotectioncontentid)

The number of assets that can be in private mode is limited to around 250. The reason being that Amazon have a 20kb S3 policy limit. So, it’s only possible for 250 assets to be in private at any one time. Assets that cannot be switched because the limit has been reached, will automatically be public. The system is currently configured so that assets created will automatically go into a private state if the VIS vuflow events management system callback. 

```https://vis.vuworkflow.vrt.staging.vualto.com/api/event/vuflow/taskenginecallback``` is specified when creating a VOD Stream.

To do a private / public switch please see the document below. You will need to use the VIS API.
This functionality is also possible within the GUI ```https://admin.vuworkflow.staging.vualto.com```

Example API switch request:

HTTP PUT Request:
```URL
https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/protection/mz-ast-2055fcff-8cca-4e37-85b9-9647dbe50398-1
```

Headers:
```
X-Auth-Key: 3723a232-10ca-4ced-adfe-fa35933a74f6
Content-Type: application/json
```

Body:
```
{
  "protection": "VOD_PUBLIC"
}
```

The asset “demo1” will be available via CDN for public viewing. It is recommended to clear CDN cache when switching an asset.
An example on staging would mean that you could only playback the content using this URL.

```
http://protected.vod.origin.cdn.be/content/vod/054b58a1-8f05-4f08-bb17-b4698e3e9d1d/054b58a1-8f05-4f08-bb17-b4698e3e9d1d_drm_57d82537-1775-4e29-97e4-bba86e0219fb.ism/.m3u8
```


















