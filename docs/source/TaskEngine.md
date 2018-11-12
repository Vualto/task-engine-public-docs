# Task Engine API

# 1. Introduction to Task Engine

Task Engine is a product that facilitates easy integration between client systems and the vudrm platform.  It is designed to allow simple or complicated workflows to be constructed and tested easily and quickly.  Task Engine is exposed via a simple API and reports back to the client system via any required mechanism, although usually this is as simple as an HTTP POST to an endpoint.

## 1.1 How it works

Task Engine breaks a workflow up in to several component parts.  Every piece of work submitted is a job, and each job is broken up in to a series of tasks.  These tasks are then scheduled on to queues which workers then process.

Every job and task have unique identities, and these are exposed back to the client system both through the response to the initial job submission and through the callbacks (unless for some reason the configuration prevents this from happening).

When a task is being processed, a callback is sent through the configured mechanism as it starts and when it completes or fails, this way the client system should be able to track the progress of a job as it progresses through the task engine and then perform any relevant tasks when the job as a whole is completed.

## 1.2 System Requirements

The Task Engine is designed to run in Docker containers, and is therefore able to run on any Docker-supported Linux host, preferably running whatever the latest version of Docker is at the time (currently 1.12.1).  Any workers that need to run on Windows are currently NOT containerized but are installed as a native Windows service – this will change as Docker support for Windows matures.

When a worker starts a task, it will generally need to copy one or more files to local storage to perform its work in an efficient manner, so it’s critical that hosts have sufficient disk space for processing to take place; this is particularly important when dealing with video transcoding or DRM encryption.  This also means that sometimes running more than one worker per host is not an option.  Vualto will usually advise on disk space requirements once the workflow is fully defined and some representative test content has been processed.

## 1.3 Integration

There are two main integration points for the task engine:

**Triggers** – how you notify task engine that a workflow needs to be executed, usually initiated by a call to the Task Engine API.

**Callbacks** – how we notify client systems of job progress.


# 2. Triggers


While the task engine can be configured to use watch folders or other methods of notification (currently outside the scope of this document) to trigger a workflow, the most common route is to have the client system trigger a workflow via an API call.

There is a single endpoint, /job, that handles all requests, only the payload varies according to the requirements of the workflow.  As most clients have varying workflows, this document will give an examples of some of the most common workflows :

**vodstream:**  This workflow will create a server side ISM manifest, with or without DRM, that can be used for on the fly delivery via USP.

**vodcapture:**  This workflow allows you to create a frame accurate vod clip by passing in a start and end UTC time stamp. The result will be an MP4 on disk.

**voddeletes3:**  This workflow allows you to delete content on S3.

**drmswitch:**  This workflow allows you to toggle DRM on and off.
 
Custom workflows can be easily created , the only difference will be the name of the workflow and the parameters passed in the ‘parameters’ section.

## 2.1 Workflow Trigger Example
 
 An example call to the the Task Engine API to trigger a workflow would be as follows ( Parameters must be a parameters object):
 
  
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


| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodstream'.
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system.|
| source_folder     |Yes| Location of the source files. All files to be processed will need to be in a discrete folder, the ‘root’ folder will be specified in the client configuration.      
| output_folder     |No | The folder for processed files to be placed.  The ‘root’ folder will be specified in the client configuration. | source_folder
|delete_source      |No | This boolean indicates whether the source should be deleted from s3 after the job has completed| true |
|encrypted          |No | This boolean indicates whether the active manifest after a job has completed should be the encrypted manifes| true|
| drm               |No |The type of DRM that is required. This could be “playready” and/or ”widevine” and/or ”fairplay” and/or “cenc” and/or "aes". If this value isn’t present then no DRM is applied. |
| rest_endpoints    |No | Multiple end points can be specified. | 
|create_thumbnail   |No |This boolean indicates whether a thumbnail should be created for the content|true|
|thumbnail_time     |No |Time at which the thumbnail will be taken.|first frame|
|generate_mp4       |No |This boolean indicates whether an MP4 is generated for the VOD content|false|
|mp4_filename       |No |Filename for the generated MP4|{content_id}.mp4|
|combine_sources    |No |This boolean indicates whether the isma/v/ts generated from the source content are to be combined into a single ismv before packaging|true|
|create_dref        |No |This boolean indicates whether a dref MP4 is generated for the VOD content|true|
|all_audio_tracks   |No |This boolean indicates whether all audio tracks are captured or only the audio tracks with the highest bitrates for each language are captured| true|

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
    "delete_source": false,
    "encrypted": true,
    "output_folder": "mz-ast-2055fcff-8cca-4e37-85b9-9647dbe50398-1",
    "drm": [
      "fairplay",
      "playready",
      "widevine"
    ],
    "rest_endpoints": [
      "https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
      "http://aaa.com/end",
      "http://bbb.com/end"
    ],
    "create_thumbnail": true,
    "thumbnail_time": "1:34.000",
    "generate_mp4": true,
    "mp4_filename": "demo_sample.mp4",
    "combine_sources": true,
    "create_dref": true,
    "all_audio_tracks": true
  }
}
```


## 2.1.2 vodcapture

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodcapture'.||
| content_id        |Yes| This is the id for the resulting capture.||
| output_folder     |Yes| This is the folder where the resulting capture wil be saved on S3. This is cleared before the capture is uploaded.||
| source            |Yes| This would need to be either an HLS, MSS or Dash stream URL to the Live or Archive content. e.g. http://mydomain.com/test.ism/.m3u8 , ||http://mydomain.com/test.ism/manifest , http://mydomain.com/test.ism/.mpd
| start             |No | UTC timestamp for the start timecode. e.g 2016-10-13T10:10:40.251Z OR Offsets e.g. “hh:mm:ss”||
| end               |No | UTC timestamp for the end timecode e.g 2016-10-13T10:20:40.251Z OR Offsets e.g. “hh:mm:ss” ||
| filter            |No | This allows you to pass filter expressions to select certain video, audio tracks. e.g. to all video bitrates below 8Mbps and all audio bitrates at 64Kbps "type==\\"video\\"&&systemBitrate==800000\|\|type==\\"audio\\"&&systemBitrate==64000"||
| encrypted         |No | This boolean allows you to set the encrypted manifest as active after the capture is complete| false|
| drm               |No | The type of Output DRM that is required. This could be “playready” and/or  ”widevine” and/or ”fairplay” and/or “cenc” and/or "aes".  If this value isn’t present then no DRM is applied.||
| frame_accurate    |No | This boolean allows the capture to be done using frame accuracy.|true|
| copy_ts           |No | This boolean indicates whether the timestamps should be included in the resulting manifests.|false|
| rest_endpoints    |No | Multiple end points can be specified.||
| key_id            |No | Should the stream be DRM’d we would require the KeyID ||
| content_key       |No | Should the stream be DRM’d we would require the Content Key ||
| output_file       |No | Name of the output ismv file| capture.ismv |
|create_thumbnail   |No |This boolean indicates whether a thumbnail should be created for the content|true|
|thumbnail_time     |No |Time at which the thumbnail will be taken.|first frame|
|generate_mp4       |No |This boolean indicates whether an MP4 is generated for the VOD content|false|
|mp4_filename       |No |Filename for the generated MP4|{content_id}.mp4|
|create_dref        |No |This boolean indicates whether a dref MP4 is generated for the VOD content|true|
|generate_vod       |No |This boolean indicates whether VOD content should be created for the capture.|true|

### 2.1.2.1 vodcapture JSON Request example

```json
{
  "client": "staging",
  "job": {
    "workflow": "vodcapture"
  },
  "parameters": {
    "content_id": "demo_1",
    "output_folder": "demo_1",
    "clips": [
      {
        "source": "http://mydomain.com/live.isml/manifest",
        "start": "2018-06-06T10:00:00.000",
        "end": "2018-06-06T10:30:00.000",
        "filter": "type==\"audio\"||type==\"video\"&&systemBitrate==1300000"
      }
    ],
    "encrypted": false,
    "drm": [
        "fairplay",
        "playready",
        "cenc",
        "widevine",
        "aes"
    ],
    "frame_accurate": true,
    "copy_ts": false,
    "rest_endpoints": [
	  "https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
	  "http://your.custom.endpoint"
    ],
    "key_id": "346AS5847333DDSHKFSDS7633429CD33",
    "content_key": "346AS5847333DDSHKFSDS7633429CD33",
    "output_file": "demo_sample.ismv",
    "create_thumbnail": true,
    "thumbnail_time": "1:34.000",
    "generate_mp4": true,
    "mp4_filename": "demo_sample.mp4",
    "create_dref": true
  }
}
```

## 2.1.3 voddelete_S3

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'voddeletes3'.||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system.||
| folder            |Yes| Folder where the content to be deleted resides||
| rest_endpoints    |No | Multiple end points can be specified.||

### 2.1.3.1 voddelete S3 JSON Request example

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

## 2.1.4 drmswitch 

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'drmswitch'.||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system.||
| folder            |Yes| Folder where the content to be DRM toggled resides||
| rest_endpoints    |No | Multiple end points can be specified.||


### 2.1.4.1 drmswitch JSON example

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
    http://vualto.demo.com/job \
    -H "API-KEY: aabbccdd-1122-3344-5566-eeff77889900" \
    -H 'Cache-Control: no-cache' \
    -H 'Content-Type: application/json' \
    -d '{
      "client": "vualto",
      "job": {
        "workflow": "vodstream"
      },
      "parameters": {
          "content_id": "demo_1",
          "source_folder": "/input/demo1",
          "delete_source": false,
          "encrypted": false,
          "output_folder": "/test",
          "drm": [
            "fairplay",
            "playready",
            "cenc",
            "widevine"
          ],"rest_endpoints": [
            "https://webhook.site/55151d14-cee1-416b-b956-a90525ae8f58",
            "https://webhook.site/bc4c13ee-f118-4d5b-a4af-7ac07890a7f1"
          ],
          "create_thumbnail": false,
          "generate_mp4": true,
          "combine_sources": true,
          "create_dref": true,
          "all_audio_tracks": false,
        }
      }

This results in the files <content_id>.ism and <content_id>.ismv being produced in the folder: 

```<configured_root>(/<optional_output_folder>)/<content_id>.```

The response from this call should be either a 200 OK, with the following payload:

``` { "job_id": <job_id>, "result": "accepted" } ```

or a 400 BAD REQUEST with the following payload:

``` { "error": "<description_of_error>" } ```

Assuming the call is successful, this would add an ingest job to the Task Engine queue, with the files to be ingested expected to be in the following location:

``` <input_root>/input/demo1 ```

If the process completes successfully, then the output would be the following files:

``` <output_root>/test/demo1.ism ```
``` <output_root>/test/demo1.ismv ```

If the ‘output_folder’ parameter was excluded, then the files would be output to the following locations:

``` <output_root>/demo1.ism ```
``` <output_root>/demo1.ismv ```

**NOTE:** there may be some additional files, depending on the exact processes involved, but the minimum would usually be these.

# 3. Rest Endpoint Callbacks

Specifying the Task Engine ```https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback``` “webhook” in the callback will register the VOD asset within the Vualto vuflow events management system. It’s recommended and this is optional but does mean you can view the VOD event within ```https://admin.vuworkflow.staging.vualto.com```. This is also required for VOD assets to automatically go into private mode (configurable) after ingestion.


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


The Parameters are defined as follows:


| Parameter Name    | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| job_id            | Unique job id, assigned by the Task Engine
| task_id           | No| Unique task id.  Each task in a jobs’ workflow has a unique id.  If this value is not present then the callback relates to the job as a whole rather than a specific task.
| task_name         | No| Name of the task to which the event related.  If this value is not present then the callback relates to the job as a whole rather than a specific task.
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


# 4. Public/Private switching

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

