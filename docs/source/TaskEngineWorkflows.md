# TASK ENGINE WORKFLOWS

## Vodstream

This workflow will create a server side manifest, with and/or without DRM, that can be used for on the fly delivery of VOD content via USP.

### Vodstream: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodstream'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| source_folder     |Yes| Location of the source files. All files to be processed will need to be in a discrete folder, the ‘root’ folder will be specified in the client configuration. ||
| delete_source     |No | This boolean indicates whether the source should be deleted from source storage after the job has completed. | false |
| encrypted         |No | This boolean indicates whether the active manifest, for a job, should be the encrypted manifest. | true |
| output_folder     |No | The folder for processed files to be placed.  The ‘root’ folder will be specified in the client configuration. | source_folder |
| drm               |No | The type of DRM that is required. This could be “playready” and/or ”widevine” and/or ”fairplay” and/or “cenc” and/or "aes". If this value isn’t present then no DRM is applied. ||
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| create_thumbnail  |No | This boolean indicates whether a thumbnail should be created for the content. | true |
| thumbnail_time    |No | Time at which the thumbnail will be taken. | first frame |
| generate_mp4      |No | This boolean indicates whether an MP4 is generated for the VOD content. | false |
| mp4_filename      |No | Filename for the generated MP4, if generate_mp4 is set to true. | {content_id}.mp4 |
| mezzanine         |No | This boolean indicates whether the generated mp4 contains all the video tracks or just the highest bitrate audio and video track. | false |
| combine_sources   |No | This boolean indicates whether the isma/v/ts generated from the source content are to be combined into a single ismv before packaging the manifests. | true |
| create_dref       |No | This boolean indicates whether a dref MP4 is generated for the VOD content. | true |
| all_audio_tracks  |No | This boolean indicates whether all audio tracks are captured or only the audio tracks with the highest bitrates for each language are captured. | true |
| encrypt_ismv      |No | This boolean indicates whether the resulting ismv file should be encrypted. This is can be used to implement TransDRM. | false |
| playready_key     |No | The playready key used to encrypt the ismv file (if encrypt_ismv is set to true). If no playready key is provided, one will be generated through VuDRM. ||
| preview_thumbnails          |No | This boolean indicates whether to generate thumbnail assets which can be used for video timeline previews. | false |
| preview_thumbnails_interval |No | Interval time between thumbnail captures in seconds. | 10 |
| apply_track_properties      |No | This boolean indicates whether custom track propertes (set when submitting the job or in central configuration) should be applied to the VOD asset. | false |
| track_properties  |No | This is used to define track properties to be applied to the VOD (See [Track Properties](#track-properties) section). ||
| source_storage    |No | This is used to indicate where the source content is stored (see [Storage Support](#storage-support) section). | `S3` (system default) |
| destination_storage         |No | This is used to indicate the destinantion for the VOD assets (see [Storage Support](#storage-support) section). | <source_storage> |

### Vodstream: JSON Payload example

```json
{
  "client": "demo-client",
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
    "mezzanine": true,
    "combine_sources": true,
    "create_dref": true,
    "all_audio_tracks": true,
    "preview_thumbnails": true,
    "preview_thumbnails_interval": 20,
    "apply_track_properties": true,
    "source_storage": "local",
    "desination_storage": "S3",
  }
}
```

### Vodstream: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the defualt properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message assoicated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Full path of the active manifest, for the generated content. |
| files             | List of files (manifests, content files, thumbnail, etc...) that have been copied to the final destination. |

## Vodcapture

This workflow allows you to create a frame accurate vod clip by passing in a start and end UTC time stamp. The result will be an MP4 on disk.

### Vodcapture: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodcapture'. ||
| content_id        |Yes| This is the id for the resulting capture. ||
| output_folder     |Yes| This is the folder where the resulting capture wil be saved on the destination storage. This is cleared before the capture is uploaded. ||
| clips             |yes| This is an array of sources, with optional start and end times, please see the example request below. ||
| source            |Yes| This would need to be either an HLS, MSS or Dash stream URL to the Live or Archive content. e.g. http://mydomain.com/test.ism/.m3u8 , http://mydomain.com/test.ism/manifest , http://mydomain.com/test.ism/.mpd. || 
| start             |No | UTC timestamp for the start timecode. e.g 2016-10-13T10:10:40.251Z OR Offsets e.g. “hh:mm:ss”. ||
| end               |No | UTC timestamp for the end timecode e.g 2016-10-13T10:20:40.251Z OR Offsets e.g. “hh:mm:ss”. ||
| filter            |No | This allows you to pass filter expressions to select certain video, audio tracks. e.g. to all video bitrates below 8Mbps and all audio bitrates at 64Kbps "type==\\"video\\"&&systemBitrate==800000\|\|type==\\"audio\\"&&systemBitrate==64000". ||
| encrypted         |No | This boolean allows you to set the encrypted manifest as active after the capture is complete. | false |
| drm               |No | The type of Output DRM that is required. This could be “playready” and/or  ”widevine” and/or ”fairplay” and/or “cenc” and/or "aes".  If this value isn’t present then no DRM is applied. ||
| frame_accurate    |No | This boolean allows the capture to be done using frame accuracy. | true |
| copy_ts           |No | This boolean indicates whether the timestamps should be included in the resulting manifests. | false |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| key_id            |No | Should the stream be DRM’d we would require the KeyID. ||
| content_key       |No | Should the stream be DRM’d we would require the Content Key. ||
| output_file       |No | Name of the output ismv file. | <content_id>_capture.ismv |
| generate_vod      |No | This boolean indicates whether VOD manifests are generated for the capture. | true |
| create_thumbnail  |No | This boolean indicates whether a thumbnail should be created for the content. | true |
| thumbnail_time    |No | Time at which the thumbnail will be taken. | first frame |
| generate_mp4      |No | This boolean indicates whether an MP4 is generated for the VOD content. | false |
| mp4_filename      |No | Filename for the generated MP4. | {content_id}.mp4 |
| mezzanine         |No | This boolean indicates whether the generated mp4 contains all the video tracks or just the highest bitrate audio and video track. | false |
| create_dref       |No | This boolean indicates whether a dref MP4 is generated for the VOD content. | <generate_vod> |
| encrypt_ismv      |No | This boolean indicates whether the resulting ismv file should be encrypted. This is can be used to implement TransDRM. | false |
| playready_key     |No | The playready key used to encrypt the ismv file (if encrypt_ismv is set to true). If no playready key is provided, one will be generated through VuDRM. ||
| empty_target      |No | This boolean indicates whether the target folder in storage should be cleared before the output assets are save. | true |
| preview_thumbnails          |No |  This boolean indicates whether to generate thumbnail assets which can be used for video timeline previews. | false |
| preview_thumbnails_interval |No | Interval time between thumbnail captures in seconds. | 10 |
| apply_track_properties      |No | This boolean indicates whether custom track propertes (set when submitting the job or in central configuration) should be applied to the VOD asset. | false |
| track_properties  |No | This is used to define track properties to be applied to the VOD (See [Track Properties](#track-properties) section). ||
| destination_storage         |No | This is used to indicate the destinantion for the VOD assets (see [Storage Support](#storage-support) section). | `S3` (system default) |

### Vodcapture: JSON Payload example

```json
{
  "client": "demo-client",
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
    "create_dref": true,
    "preview_thumbnails": true,
    "preview_thumbnails_interval": 20
  }
}
```

### Vodcapture: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the defualt properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message assoicated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Full path of the active manifest, for the generated content. |
| files             | List of files (manifests, content files, thumbnail, etc...) that have been copied to the final destination. |

## Voddelete

This workflow allows you to delete VOD assets from storage.

### Voddelete: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'voddelete'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| folder            |Yes| Folder where the content to be deleted is currently saved. ||
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| source_storage    |No | This is used to indicate where the VOD assets are stored (see [Storage Support](#storage-support) section). | `S3` (system default) |

### Voddelete: JSON Payload example

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "voddelete"
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

### Voddelete: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the defualt properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message assoicated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Name of the folder deleted from storage. |

## Drmswitch

This workflow allows you to toggle DRM on and off.

### Drmswitch: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'drmswitch'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| folder            |Yes| Folder where the content to be DRM toggled is stored. ||
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| source_storage    |No | This is used to indicate where the VOD assets are stored (see [Storage Support](#storage-support) section). | `S3` (system default) |

### Drmswitch: Payload example

```json
{
  "client": "demo-client",
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

### Drmswitch: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the defualt properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message assoicated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Full path of the active manifest, for the generated content. |

## CreateMP4

This workflow allows you to create an MP4 from a VOD asset

### CreateMP4: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'createmp4'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| source_folder     |Yes| Folder where the VoD source content can be found. ||
| output_folder     |No | Folder where the MP4 should be saved. | <source_folder> |
| mp4_filename      |No | The name of the resulting mp4 file. | <content_id>.mp4 |
| retries           |No | Retry limit when attempting to copy from the source storage. | 2 |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| mezzanine         |No | This boolean indicates whether the generated mp4 contains all the video tracks or just the highest bitrate audio and video track. | false |
| source_storage    |No | This is used to indicate where the source VOD is stored (see [Storage Support](#storage-support) section). | `S3` (system default) |
| destination_storage         |No | This is used to indicate the destinantion for the generated MP4 (see [Storage Support](#storage-support) section). | <source_storage> |

### CreateMP4: Payload example

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "createmp4"
  },
  "parameters": {
    "content_id": "demo1",
    "folder": "vualto-test-1",
    "rest_endpoints": [
			"https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
			"http://your.custom.endpoint"
    ],
    "mp4_filename": "result.mp4",
    "output_folder": "vualto-test-1/downloads"
  }
}
```

### CreateMP4: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the defualt properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message assoicated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | MP4 filename. |
| files             | List of files uploaded to the destination storage. |

## Build_thumbnails

This workflow allows you to generate thumbnail assets which can then be used for video timeline previews.

### Build_thumbnails: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'build_thumbnails'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| source            |Yes| URL of the HLS source from which to create assets. Live sources (.isml) must be in a state of `stopped`. ||
| output_folder     |Yes| This is the folder where the resulting assets wil be saved on S3. | <content_id> |
| target_filename   |No | Prefix for the file names of generated assets, eg: `<target_filename>_sprite.jpg` .| <content_id> |
| preview_thumbnails_interval   |No | Interval time between thumbnail captures in seconds. | 10 |
| video_fps         |No | Fallback parameter, which will only be used if the fps cannot be obtained from the source metadata. | 0 |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| destination_storage         |No | This is used to indicate the destinantion for the generated thumbnail assets (see [Storage Support](#storage-support) section). | `S3` (system default) |

### Build_thumbnails: Payload example

```json
{
    "parameters": {
        "content_id": "demo1",
        "source": "http://mydomain.com/example.ism/.m3u8",
        "output_folder": "vualto-test-1/downloads",
        "target_filename": "demo_sample",
        "preview_thumbnails_interval": 20,
        "video_fps": 24,
        "rest_endpoints": []
    },
    "client": "demo-client",
    "job": {
        "workflow": "build_thumbnails"
    }
}
```
### Build_thumbnails: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the defualt properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message assoicated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | List of thumbnail assets uploaded to the destination storage. |

## Additional Workflow Features

### Priority

The Task Engine supports ordering of jobs by priority. The priority parameter can be submitted as part of the json payload being submitted. The priority is in ascending order as follows:

1 - Top Priority  
.  
.  
5 - Default  
.  
.  
10 - Least Priority

The `"priority"` parameter needs to be submitted within the `"job"` section of the json payload as shown below:

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "vodcapture",
    "priority": 3
  },
  "parameters": {
    "content_id": "demo1",
    ...
    ...
    ...
  }
}
```

Whenever an execution slot is available, the system will first check by priority and then check the submission time and date of the job. In the case where multiple jobs are executed with the same priority (eg. with the default priority 5), the Task Engine operates in a FIFO (First In First Out) manner.

### Multiple Clips

The Task Engine includes a feature that will allow multiple clips to be stitched togther into a single clip, in a single job. This can be done by defining multiple objects within the `"clips"` parameter in the json payload for `vodcapture`. This also allows a mixture of live and VoD sources to be captured and stitched toghether into a new clip. The example below shows how the `"clips"` parameter would need to be provided to achive this.

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "vodcapture"
  },
  "parameters": {
    "content_id": "demo_1",
    "output_folder": "demo_1",
    "clips": [
      {
        "source": "http://mydomain.com/copyright.ism/manifest"
      },
      {
        "source": "http://mydomain.com/live.isml/manifest",
        "start": "2018-06-06T10:00:00.000",
        "end": "2018-06-06T10:30:00.000",
        "filter": "type==\"audio\"||type==\"video\"&&systemBitrate==1300000"
      },
      {
        "source": "http://mydomain.com/live.isml/manifest",
        "start": "2018-06-06T10:35:00.000",
        "end": "2018-06-06T11:00:00.000",
        "filter": "type==\"audio\"||type==\"video\"&&systemBitrate==1300000"
      }
    ],
    ...
    ...
    ...
  }
}
```

### Multiple Sources

In some cases, a live stream could have multiple origins setup (eg. for load balancing the origin servers). The Task Engine, allows for both streams to be defined as the source for a capture. It is smart enough to find which live stream will provide the best output capture and use that stream as the source. If the Task Engine discovers discontinuities within the streams, it will use segments from both streams to try and generate a clip with the least number of missing fragements.

The streams can be defined in the `"sources"` parameter when executing the `vodcapture` workflow.

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "vodcapture"
  },
  "parameters": {
    "content_id": "demo_1",
    "output_folder": "demo_1",
    "clips": [
      {
        "sources": [
          "http://mydomain.com/live_1.isml/manifest",
          "http://mydomain.com/live_2.isml/manifest"
        ],
        "start": "2018-06-06T10:00:00.000",
        "end": "2018-06-06T10:30:00.000",
        "filter": "type==\"audio\"||type==\"video\"&&systemBitrate==1300000"
      }
    ],
    ...
    ...
    ...
  }
}
```

In this case, `"sources"`  replaces the `"source"` parameter, however; it can still be used in conjunction with other clips which only contain a single stream as shown below.

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "vodcapture"
  },
  "parameters": {
    "content_id": "demo_1",
    "output_folder": "demo_1",
    "clips": [
      {
        "source": "http://mydomain.com/copyright.ism/manifest",
      },
      {
        "sources": [
          "http://mydomain.com/live_1.isml/manifest",
          "http://mydomain.com/live_2.isml/manifest"
        ],
        "start": "2018-06-06T10:00:00.000",
        "end": "2018-06-06T10:30:00.000",
        "filter": "type==\"audio\"||type==\"video\"&&systemBitrate==1300000"
      }
    ],
    ...
    ...
    ...
  }
}
```

### Generate Download Clips

The Task Engine `vodcapture` workflow supports generating download clips without creating VoD assets. This is done by setting the property `"generate_vod"` to false and `"generate_mp4"` to true. It is important that if `"generate_vod"` is set to false, to not manually override the `"create_dref"` parameter. Setting `"create_dref"` to true will lead to a failed workflow as this requires VoD assets to generate DREF mp4s.

The resulting download will be an MP4 containg all the video, audio and caption tracks defined using the clip's `"filter"` parameter. If no filter is defined, the resulting MP4 will contain all the tracks availble in the stream.

### Scheduler

The Task Engine supports scheduling of jobs via a `run_at` attribute. Jobs are moved from a queue_state of `scheduled` to a queue_state of `queued` via a scheduler-worker. The interval at which this runs is pulled from the database settings table (schedule_interval, default: 1 hour).

The scheduler-worker looks for jobs which have a queue_state of `scheduled` and a `run_at` time in the past

The schedule_interval can be set via an api call. (where x is time in seconds)

`post '/settings'`

```json
{
  "client": "demo-client",
  "name": "schedule_interval",
  "setting": "<x>"
}
```

A jobs `run_at` attribute can be set in multiple ways and defaults to the time it was created at.
If the job's `run_at` time is in the future, a log will be added to indicate such.

Format: yyyy-mm-ddThh:mm:ss

1. When submitting a job

`post '/job/:job_id'`

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "vodcapture",
    "run_at": "2019-06-06T10:00:00.000"
  }
}
```

ex: `Job will run at: "2019-06-06T10:00:00.000"`

2. When updating an existing job

`put '/jobs/:job_id'`

```json
{
  "client": "demo-client",
  "run_at": "2019-06-06T10:00:00.000"
}
```

3. When submitting a capture with a clip end time in the future

If a capture is submitted with a clip end time that is in the future, it will be automatically scheduled to run at the end time of the clip which is furthest in the future. The exception to this is if the `run_at` time is specified and is further in the future than the end time, then the `run_at` time will be used.

### Track Properties

There are instances when track properties need to be added to specific tracks within the VOD manifest. This usually occurs when custom track descriptions or track roles need to be set. The Task Engine supports adding track properties to audio and subtitle tracks. Filtering of tracks is based on type (`audio` or `textStream`) and a combination of language and/or track role. The filters and values can be set in Vualto's Central Configuration so they can easily be applied to all VODs being captured or ingested. They can also be defined as part of the job submission. The value set will overwrite the exisitng value for the property, if it already exists. Below are some samples of how the filters can be defined. 

Setting the defined track description where the audio language is not set or set to `und` (undefined).

```json
"track_properties": {
  "audio": {
    "und|": {
      "track_description": "Original Audio Track"
    }
  }
}
```

Setting the defined track description and track name where the language is set `eng` and the role is set to `description`.

```json
"track_properties": {
  "audio": {
    "eng_description": {
      "track_name" : "Audio Description - English",
      "track_description": "English Audio Descriptive"
    }
  }
}
```

Setting the defined track role and description to the subtitle track where the language is set to `eng`.

```json
"track_properties": {
  "textStream": {
    "eng": {
      "track_role" : "caption",
      "track_description": "English CC"
    }
  }
}
```

Setting a combination of properties to both audio and subtitle tracks.

```json
"track_properties": {
  "audio": {
    "und|": {
      "track_description": "Original Audio"
    },
    "eng_alternate": {
      "track_name": "English Alt",
      "track_description": "English Alternate track"
    },
    "eng": {
      "track_role" : "main",
    }
  },
  "textStream": {
    "eng": {
      "track_role" : "caption",
      "track_description": "English CC"
    }
  }
}
```

Support is confirmed for `track_description`, `track_role` and `track_name` properties, but other properties may be supported.

### Storage Support

The Task Engine supports multiple storage types for ingesting content and saving VOD. Support has also been added so a combination of storage types can be used for the same job. This can be done by setting the `source_storage` and `destination_storage` in the job payload (for supported workflows). Eg. Ingesting content from local storage and save VOD assets on S3 would require `source_storage` to be set to `local` and `destination_storage` to be set to `S3`.

The system defualt storage type is Amazon S3, however; the default can be customised per client as well as set on a job per job basis. Additional setup may be required when using `local` storage, as the folders will need to be mapped to the worker docker containers.

Natively supported storage types:

- Amazon S3 (`S3`)
- On premises infrastructure (`local`)
  
## Workflow Trigger Example

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

This results in the files `<content_id>_<drm_tag>_<unique_guid>.ism` and `<content_id>_<unique_guid>.ismv` being produced in the folder:

```<configured_root>(/<optional_output_folder>)/<content_id>```

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
