# TASK ENGINE WORKFLOWS

## Vodstream

This workflow will generate a VOD asset from an offline source (eg. MP4). A server side manifest is created, with and/or without DRM, that can be used for on the fly delivery of VOD content via the Unified Streaming Platform.

### Vodstream: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodstream'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| source_folder     |Yes| Location of the source files. All files to be processed will need to be in a discrete folder, the ‘root’ folder will be specified in the client configuration. ||
| delete_source     |No | This boolean indicates whether the source should be deleted from source storage after the job has completed. | false |
| encrypted (deprecated) |No | Deprecated and replaced by `enable_drm` for clarity. | |
| enable_drm        |No | This boolean indicates whether the drm manifest (if created - read `drm` parameter) should be enabled. | true |
| output_folder     |No | The folder for processed files to be placed.  The ‘root’ folder will be specified in the client configuration. | source_folder |
| drm               |No | The type of DRM that is required. This could be “playready” and/or ”widevine” and/or ”fairplay” and/or “cenc” and/or "aes". If this value isn’t present then no DRM is applied. | ["clear"] |
| cpix              |No | This boolean indicates whether DRM will be handled using a CPIX document| false |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| create_thumbnail  |No | This boolean indicates whether a thumbnail should be created for the content. | true |
| thumbnail_time    |No | Time at which the thumbnail will be taken. | first frame |
| generate_mp4      |No | This boolean indicates whether an MP4 is generated for the VOD content. | false |
| mp4_filename      |No | Filename for the generated MP4, if generate_mp4 is set to true. | {content_id}.mp4 |
| mezzanine         |No | This boolean indicates whether the generated mp4 contains all the video tracks or just the highest bitrate audio and video track. | false |
| combine_sources   |No | This boolean indicates whether the isma/v/ts generated from the source content are to be combined into a single ismv before packaging the manifests. | true |
| create_dref       |No | This boolean indicates whether a dref MP4 is generated for the VOD content. | true |
| all_audio_tracks  |No | This boolean indicates whether all audio tracks or only the audio tracks with the highest bitrates for each language are packaged. | true |
| encrypt_ismv      |No | This boolean indicates whether the resulting ismv file should be encrypted. This is can be used to implement TransDRM. | false |
| playready_key     |No | The playready key used to encrypt the ismv file (if encrypt_ismv is set to true). If no playready key is provided, one will be generated through VuDRM. ||
| preview_thumbnails          |No | This boolean indicates whether to generate thumbnail assets which can be used for video timeline previews. | false |
| preview_thumbnails_interval |No | Interval time between thumbnail captures in seconds. | 10 |
| apply_track_properties      |No | This boolean indicates whether custom track properties (set in `track_properties` when submitting the job or in central configuration) should be applied to the VOD asset. | false |
| track_properties  |No | This is used to define track properties to be applied to the VOD (See [Track Properties](TaskEngineAdditionalFeatures.html#track-properties) section). ||
| retries           |No | This is used to indicate the number of times fetching the source should be re-tried. | 0 |
| source_storage    |No | This is used to indicate where the source content is stored (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | `S3` (system default) |
| destination_storage         |No | This is used to indicate the destination for the VOD assets (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | <source_storage> |
| encode_source     |No | This boolean indicates whether the source is to be encoded into multiple bitrates/resolutions. | false |
| encoding_profile  |No | This is used to indicate which encoding profiles are used when encoding the source. | "H264" |
| encoding_mode     |No | This is used to indicate which Bitmovin encoding mode is used (See [here](https://bitmovin.com/bitmovin-video-encoding-v2/) for more details). | "STANDARD" |
| encoding_region   |No | This is used to indicate in which region Bitmovin's encoding process should be executed. |  |

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
    "enable_drm": true,
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
    "destination_storage": "S3"
  }
}
```

### Vodstream: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the default properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message associated with the event. This will usually contain exception messages. |

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

This workflow allows you to create a frame accurate VOD clip by passing in a start and end time. If the source stream contains time stamps, UTC time stamps can be used for the start and end times. The result will be a new VOD asset and/or a downloadable MP4.

### Vodcapture: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodcapture'. ||
| content_id        |Yes| This is the id for the resulting capture. ||
| output_folder     |Yes| This is the folder where the resulting capture will be saved on the destination storage. This is cleared before the capture is uploaded. ||
| clips             |yes| This is an array of sources, with optional start and end times, please see the example request below. ||
| clip: source      |Yes| This would need to be either an HLS, MSS or Dash stream URL to the Live or Archive content. e.g. `http://mydomain.com/test.ism/.m3u8` , `http://mydomain.com/test.ism/manifest` , `http://mydomain.com/test.ism/.mpd` ||
| clip: start       |No | UTC timestamp for the start timecode. e.g 2016-10-13T10:10:40.251Z or Offsets e.g. “hh:mm:ss”. ||
| clip: end         |No | UTC timestamp for the end timecode e.g 2016-10-13T10:20:40.251Z or Offsets e.g. “hh:mm:ss”. ||
| clip: filter      |No | This allows you to pass filter expressions to select certain video, audio tracks. e.g. to all video bitrates below 8Mbps and all audio bitrates at 64Kbps "type==\\"video\\"&&systemBitrate==800000\|\|type==\\"audio\\"&&systemBitrate==64000". ||
| encrypted (deprecated) |No | Deprecated and replaced by `enable_drm` for clarity. | |
| enable_drm        |No | This boolean indicates whether the drm manifest (if created - read `drm` parameter) should be enabled. | true |
| drm               |No | The type of Output DRM that is required. This could be “playready” and/or  ”widevine” and/or ”fairplay” and/or “cenc” and/or "aes".  If this value isn’t present then no DRM is applied. | ["clear"] |
| cpix              |No | This boolean indicates whether DRM will be handled using a CPIX document| false |
| frame_accurate    |No | This boolean allows the capture to be done using frame accuracy. | true |
| copy_ts           |No | This boolean indicates whether the timestamps should be included in the resulting manifests. | false |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| key_id            |No | Should the stream be DRM’d we would require the KeyID. ||
| content_key       |No | Should the stream be DRM’d we would require the Content Key. ||
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
| destination_storage         |No | This is used to indicate the destination for the VOD assets (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | `S3` (system default) |
| apply_track_properties      |No | This boolean indicates whether custom track properties (set when submitting the job or in central configuration) should be applied to the VOD asset. | false |
| track_properties  |No | This is used to define track properties to be applied to the VOD (See [Track Properties](TaskEngineAdditionalFeatures.html#track-properties) section). ||
| preview_thumbnails          |No |  This boolean indicates whether to generate thumbnail assets which can be used for video timeline previews. | false |
| preview_thumbnails_interval |No | Interval time between thumbnail captures in seconds. | 10 |

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
    "enable_drm": false,
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

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the default properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message associated with the event. This will usually contain exception messages. |

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

This workflow allows you to a delete VOD asset from storage.

### Voddelete: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'voddelete'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| folder            |Yes| Folder where the content to be deleted is currently saved. ||
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| source_storage    |No | This is used to indicate where the VOD assets are stored (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | `S3` (system default) |

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

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the default properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message associated with the event. This will usually contain exception messages. |

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

This workflow allows you to toggle DRM on and off for a VOD asset. For DRM switching to work as expected, DRM parameters will have needed to be provided when generating the VOD asset (either through [vodcapture](#vodcapture) or [vodstream](#vodstream)).

### Drmswitch: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'drmswitch'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| folder            |Yes| Folder where the content to be DRM toggled is stored. ||
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| source_storage    |No | This is used to indicate where the VOD assets are stored (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | `S3` (system default) |

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

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the default properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message associated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Full path of the active manifest, for the generated content. |

## Create MP4

This workflow allows you to create an MP4 from a VOD asset.

### Create MP4: Parameters

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
| source_storage    |No | This is used to indicate where the source VOD is stored (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | `S3` (system default) |
| destination_storage         |No | This is used to indicate the destination for the generated MP4 (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | <source_storage> |

### Create MP4: Payload example

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

### Create MP4: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the default properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message associated with the event. This will usually contain exception messages. |

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

## Build thumbnails

This workflow allows you to generate thumbnail assets which can then be used for video timeline previews.

### Build thumbnails: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'build_thumbnails'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| source            |Yes| URL of the HLS source from which to create assets. Live sources (.isml) must be in a state of `stopped`. ||
| target_filename   |No | Prefix for the file names of generated assets, eg: `<target_filename>_sprite.jpg` .| <content_id> |
| output_folder     |Yes| This is the folder where the resulting assets will be saved on the destination storage. | <content_id> |
| preview_thumbnails_interval   |No | Interval time between thumbnail captures in seconds. | 10 |
| video_fps         |No | Fallback parameter, which will only be used if the fps cannot be obtained from the source metadata. | 24 |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| destination_storage         |No | This is used to indicate the destination for the generated thumbnail assets (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | `S3` (system default) |

### Build thumbnails: Payload example

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

### Build thumbnails: Callback properties


#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the default properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message associated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | List of thumbnail assets uploaded to the destination storage. |

## Vodremix

This workflow allows you to create a virtual VOD asset that is just a playlist referencing other VOD streams or video files.

### Vodremix: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodremix'.||
| content_id        |Yes| This is the id for the resulting VOD.||
| output_folder     |Yes| This is the folder where the resulting VOD will be saved on the destination storage. This is cleared before the capture is uploaded.||
| clips             |Yes| This is an array of sources, with optional start and end times, please see the example request below. ||
| clip: source      |Yes| This would need to be either a VOD stream or the URL to a video file. Must be accessible from both Task Engine and the Origin. E.g. `http://mydomain.com/manifest.ism`, `https://bucket-name.s3-eu-west-1.amazonaws.com/path/test.mp4` ||
| clip: start       |No | UTC timestamp for the start timecode. e.g 2016-10-13T10:10:40.251Z OR Offsets e.g. “hh:mm:ss”||
| clip: end         |No | UTC timestamp for the end timecode e.g 2016-10-13T10:20:40.251Z OR Offsets e.g. “hh:mm:ss” ||
| clip: frame_accurate    |No | This boolean indicates whether the specified clip will be trimmed using frame accuracy. | false |
| clip: output_description|No | This boolean indicates that this clip should be used to set the target profile. There should be only one clip with this set to true. | false |
| output_file       |No | Name of the output .mp4 file. | remix.mp4 |
| drm               |No | The type of Output DRM that is required. This could be “playready” and/or  ”widevine” and/or ”fairplay” and/or “cenc” and/or "aes".  If this value isn’t present then no DRM is applied.| ["clear"] |
| cpix              |No | This boolean indicates whether DRM will be handled using a CPIX document| false |
| empty_target      |No | This boolean indicates whether the target folder in storage should be cleared before the output assets are save. | true |
| enable_drm        |No | This boolean indicates whether the drm manifest (if created - read `drm` parameter) should be enabled. | true |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified.||
| destination_storage         |No | This is used to indicate the destination for the VOD assets (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | `S3` (system default) |

### Vodremix: JSON Payload example

```json
{
  "client": "staging",
  "job": {
    "workflow": "vodremix"
  },
  "parameters": {
    "content_id": "demo_1",
    "output_folder": "demo_1",
    "clips": [
      {
        "source": "https://bucket.s3-eu-west-1.amazonaws.com/manifest.ism",
        "start": "2018-06-06T10:00:00.000",
        "end": "2018-06-06T10:30:00.000"
      },
      {
        "source": "https://bucket.s3-eu-west-1.amazonaws.com/ad.mp4",
        "output_description": true
      },
      {
        "source": "https://bucket.s3-eu-west-1.amazonaws.com/manifest.ism",
        "start": "2018-06-06T10:40:00.000",
        "end": "2018-06-06T11:00:00.000"
      },
      {
        "source": "https://bucket.s3-eu-west-1.amazonaws.com/manifest.ism",
        "start": "2018-06-06T11:00:00.000",
        "end": "2018-06-06T11:10:00.000",
        "frame_accurate": true,
      }
    ],
    "drm": [
        "fairplay",
        "playready",
        "cenc",
        "widevine",
        "aes"
    ],
    "rest_endpoints": [
      "https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
      "http://your.custom.endpoint"
    ],
    "output_file": "remix.mp4"
  }
}
```

### Vodremix: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the default properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| message           | Any message associated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| output            | List of files (manifests, content files, thumbnail, etc...) that have been copied to the final destination. |

## Generate GIF

This workflow allows you to create animated GIFs from a VOD stream.

### Generate GIF: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'generate_gif'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| source            |Yes| URL for the VOD source used to generate the GIF ||
| start_at          |Yes| Start time for the GIF capture. This will either be in seconds, a UTC based timestamp or a time offset ("hh:mm:ss") ||
| duration          |Yes| The duration of the capture used for the GIF. This should be specified in seconds (milliseconds are supported).||
| output_folder     |Yes| Folder where the GIF should be saved. ||
| gif_filename      |Yes| The name of the resulting GIF file. ||
| bitrate           |No | This is used to filter the source and capture the GIF from a specific video bitrate within the stream. ||
| fps               |No | The frames per second of the resulting bitrate. | 12 |
| width             |No | The width of the resulting GIF. If not provided it will be calculated automatically based on the aspect ration and the height specified. | -1 |
| height            |No | The height of the resulting GIF. If not provided, it will be calculated automatically based on the aspect ration and width specified. If neither height or width are specified, a height of 480 pixels is used. | -1 |
| playback_loop     |No | The number of times the GIF should loop. | 0 |
| reverse           |No | This boolean indicates whether the GIF should be played in revers. | false |
| playback_speed    |No | This indicates the speed at which the GIF should be played back. Eg. `1.5` for GIF playback that is 1 and a half faster than the actual speed. | 1 |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| destination_storage         |No | This is used to indicate the destination for the generated MP4 (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | `S3` (system default) |

### Generate GIF: Payload example

```json
{
  "client": "demo-client",
  "job": {
      "workflow": "generate_gif"
  },
  "parameters": {
      "content_id": "demo-content",
      "source": "http://mydomain.com/example.ism/.m3u8",
      "start_at": 30,
      "duration": 6,
      "output_folder": "/demo-content/assets",
      "gif_filename": "half speed.gif",
      "bitrate": 1549288,
      "fps": 15,
      "width": 500,
      "playback_speed": 0.5,
      "rest_endpoints": [
        "https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
        "http://your.custom.endpoint"
      ]
  }
}
```

### Generate GIF: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the default properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message associated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | GIF filename. |
| files             | List of files uploaded to the destination storage. |


## Capture Frame

This workflow allows you to capture a single frame from a stream.

### Capture Frame: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'capture_frame'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| source            |Yes| URL for the VOD source used to capture the frame from ||
| frame_time        |Yes| Time of the frame within the source stream. This will either be a UTC based timestamp or a time offset ("hh:mm:ss") ||
| output_folder     |Yes| Folder where the captured frame should be saved. ||
| image_filename    |Yes| The name of the resulting image file. ||
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| destination_storage         |No | This is used to indicate the destination for the generated MP4 (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | `S3` (system default) |

### Capture Frame: Payload example

```json
{
  "client": "demo-client",
  "job": {
      "workflow": "capture_frame"
  },
  "parameters": {
      "content_id": "demo-content",
      "source": "http://mydomain.com/example.ism/.m3u8",
      "frame_time": "00:00:07.0400000",
      "output_folder": "/demo-content/assets",
      "gif_filename": "frame.jpg",
      "rest_endpoints": [
        "https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
        "http://your.custom.endpoint"
      ]
  }
}
```

### Capture Frame: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the default properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message associated with the event. This will usually contain exception messages. |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Image filename. |
| files             | List of files uploaded to the destination storage. |

## Asset Delete

This workflow allows you to delete individual assets without deleting an entire VOD directory.

### Asset Delete: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'asset_delete'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| files             |Yes| Array of files to be deleted from S3 ||
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| source_storage    |No | This is used to indicate where the source VOD is stored (see [Storage Support](TaskEngineAdditionalFeatures.html#storage-support) section). | `S3` (system default) |

### Asset Delete: Payload example

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "asset_delete"
  },
  "parameters": {
    "content_id": "demo-content",
    "files": [
      "demo-content/assets/foo.gif",
      "demo-content/thumbnail.jpg"
    ],
    "rest_endpoints": [
      "https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
      "http://your.custom.endpoint"
    ]
  }
}
```

### Asset Delete: Callback properties

#### Task Callback

Task callbacks are triggered after each task within a workflow is completed. Below is a list of the default properties for the callback:

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| task_id           | Unique task identifier generated by the Task Engine. |
| task_name         | Name of the task that triggered the callback. |
| workflow          | Name of the workflow being executed. |
| event             | This will identify the event that caused the callback to be triggered. It can be one of `start`, `complete` or `fail`. |
| content_id        | Content ID provided when the job was submitted. |
| message           | Any message associated with the event. This will usually contain exception messages. |
| files   |         | List of assets deleted |

#### Job Callback

Job callbacks are triggered when the entire job has completed. Below is a list of the default properties for the callback.

| Property Name     | Required |
| ----------------- | -------- |
| job_id            | Unique job identifier generated by the Task Engine. |
| status            | This will identify the status of the job. It can be either `completed` or `failed`. |
| workflow          | Name of the workflow being executed. |
| content_id        | Content ID provided when the job was submitted. |
| message           | List of files to requested for deletion the destination storage. |

## Workflow Trigger Example

Example of a curl command to trigger ingest for the [Vodstream](#vodstream) workflow:

```curl
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
      "enable_drm": false,
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
```

This results in the files `<content_id>_<drm_tag>_<unique_guid>.ism` and `<content_id>_<unique_guid>.ismv` being produced in the folder:

```<configured_root>(/<optional_output_folder>)/<content_id>```

The response from this call should be either a 200 OK, with the following payload:

```{ "job_id": <job_id>, "result": "accepted" }```

or a 400 BAD REQUEST with the following payload:

```{ "error": "<description_of_error>" }```

Assuming the call is successful, this would add an ingest job to the Task Engine queue, with the files to be ingested expected to be in the following location:

```<input_root>/input/demo1```

If the process completes successfully, then the output would be the following files:

```<output_root>/test/demo1.ism```
```<output_root>/test/demo1.ismv```

If the ‘output_folder’ parameter was excluded, then the files would be output to the following locations:

```<output_root>/demo1.ism```
```<output_root>/demo1.ismv```

**NOTE:** there may be some additional files, depending on the exact processes involved, but the minimum would usually be these.
