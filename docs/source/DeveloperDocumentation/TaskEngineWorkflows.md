# TASK ENGINE WORKFLOWS

## VOD STREAM

This workflow will generate a VOD asset from an offline source (eg. MP4). A server side manifest is created, with and/or without DRM, that can be used for on the fly delivery of VOD content via the Unified Streaming Platform.

### VOD Stream: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodstream'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| source_folder     |Yes| Location of the source files. All files to be processed will need to be in a discrete folder, the ‘root’ folder will be specified in the client configuration. ||
| delete_source     |No | This boolean indicates whether the source should be deleted from source storage after the job has completed. | false |
| encrypted (deprecated) |No | Deprecated and replaced by `enable_drm` for clarity. | |
| enable_drm        |No | This boolean indicates whether the drm manifest (if created - read `drm` parameter) should be enabled. | true |
| output_folder     |No | The folder for processed files to be placed.  The ‘root’ folder will be specified in the client configuration. | source_folder |
| drm               |No | A list of DRM systems o be applied to the VOD stream. This could be `"playready"` and/or `”widevine”` and/or `”fairplay”` and/or `“cenc”` and/or `"aes"`.  If this value isn’t present or `"clear"` is specified as a system a DRM-free manifest is created. | ["clear"] |
| cpix              |No | This boolean indicates whether DRM will be handled using a CPIX document. | false |
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
| track_properties  |No | This is used to define track properties to be applied to the VOD (See [Track Properties](TaskEngineWorkflowFeatures.html#track-properties) section). ||
| retries           |No | This is used to indicate the number of times fetching the source should be re-tried. | 0 |
| source_storage    |No | This is used to indicate where the source content is stored (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| destination_storage         |No | This is used to indicate the destination for the VOD assets (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | <source_storage> |
| encode_source     |No | This boolean indicates whether the source is to be encoded into multiple bitrates/resolutions. | false |
| encoding_profile  |No | This is used to indicate which encoding profiles are used when encoding the source. | "H264" |
| encoding_mode     |No | This is used to indicate which Bitmovin encoding mode is used (See [here](https://bitmovin.com/bitmovin-video-encoding-v2/) for more details). | "STANDARD" |
| encoding_region   |No | This is used to indicate in which region Bitmovin's encoding process should be executed. |  |
| encoder_version   |No | This is used to select which Bitmovin encoder version. This is useful to allow testing with BETA releases of Bitmovin encoders | `STABLE` |
| extract_audio     |No | This boolean indicates whether the audio track from the original source needs to be extracted. This only required when encoding the source into multiple bitrates | encode_source |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |

### VOD Stream: JSON Payload example

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

### VOD Stream: Callback properties

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
| metadata.duration | Duration of the VOD event. |
| custom_data       | Returns the custom data submitted to the workflow. |

## VOD CAPTURE

This workflow allows you to create a frame accurate VOD clip by passing in a start and end time. If the source stream contains time stamps, UTC time stamps can be used for the start and end times. The result will be a new VOD asset and/or a downloadable MP4.

### VOD Capture: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodcapture'. ||
| content_id        |Yes| This is the id for the resulting capture. ||
| output_folder     |Yes| This is the folder where the resulting capture will be saved on the destination storage. This is cleared before the capture is uploaded. ||
| clips             |yes| This is an array of sources, with optional start and end times, please see the example request below. ||
| clips.source      |Yes| This would need to be either an HLS, MSS or Dash stream URL to the Live or Archive content. e.g. `http://mydomain.com/test.ism/.m3u8` , `http://mydomain.com/test.ism/manifest` , `http://mydomain.com/test.ism/.mpd` ||
| clips.start       |No | UTC timestamp for the start timecode. e.g 2016-10-13T10:10:40.251Z or Offsets e.g. “hh:mm:ss”. ||
| clips.end         |No | UTC timestamp for the end timecode e.g 2016-10-13T10:20:40.251Z or Offsets e.g. “hh:mm:ss”. ||
| clips.filter      |No | This allows you to pass filter expressions to select certain video, audio tracks. e.g. to all video bitrates below 8Mbps and all audio bitrates at 64Kbps "type==\\"video\\"&&systemBitrate==800000\|\|type==\\"audio\\"&&systemBitrate==64000". ||
| clips.key_id      |No | Should the stream be DRM’d we would require the KeyID. ||
| clips.content_key |No | Should the stream be DRM’d we would require the Content Key. ||
| encrypted (deprecated) |No | Deprecated and replaced by `enable_drm` for clarity. | |
| enable_drm        |No | This boolean indicates whether the drm manifest (if created - read `drm` parameter) should be enabled. | true |
| drm               |No | A list of DRM systems o be applied to the VOD stream. This could be `"playready"` and/or `”widevine”` and/or `”fairplay”` and/or `“cenc”` and/or `"aes"`.  If this value isn’t present or `"clear"` is specified as a system a DRM-free manifest is created. | ["clear"] |
| cpix              |No | This boolean indicates whether DRM will be handled using a CPIX document. | false |
| frame_accurate    |No | This boolean allows the capture to be done using frame accuracy. | true |
| copy_ts           |No | This boolean indicates whether the timestamps should be included in the resulting manifests. | false |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
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
| destination_storage         |No | This is used to indicate the destination for the VOD assets (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| apply_track_properties      |No | This boolean indicates whether custom track properties (set when submitting the job or in central configuration) should be applied to the VOD asset. | false |
| track_properties  |No | This is used to define track properties to be applied to the VOD (See [Track Properties](TaskEngineWorkflowFeatures.html#track-properties) section). ||
| preview_thumbnails          |No |  This boolean indicates whether to generate thumbnail assets which can be used for video timeline previews. | false |
| preview_thumbnails_interval |No | Interval time between thumbnail captures in seconds. | 10 |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |
| transcode_proxy   |No | This field accepts the url for the remote transcode proxy. | |

### VOD Capture: JSON Payload example

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
        "filter": "type==\"audio\"||type==\"video\"&&systemBitrate==1300000",
        "key_id": "346AS5847333DDSHKFSDS7633429CD33",
        "content_key": "346AS5847333DDSHKFSDS7633429CD33"
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
    "transcode_proxy": "https://vualto.transcode-proxy.com",
    "copy_ts": false,
    "rest_endpoints": [
      "https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
      "http://your.custom.endpoint"
    ],
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

### VOD Capture: Callback properties

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
| metadata.duration | Duration of the VOD event. |
| custom_data       | Returns the custom data submitted to the workflow. |

## VOD DELETE

This workflow allows you to a delete VOD asset from storage.

### VOD Delete: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'voddelete'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| folder            |Yes| Folder where the content to be deleted is currently saved. ||
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| source_storage    |No | This is used to indicate where the VOD assets are stored (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |

### VOD Delete: JSON Payload example

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

### VOD Delete: Callback properties

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
| custom_data       | Returns the custom data submitted to the workflow. |

## DRM SWITCH

This workflow allows you to toggle DRM on and off for a VOD asset. Missing manifests will be generated when required. If the VOD asset does not have a DRM manifest and DRM is being enabled, a list of DRM systems needs to be provided as part of the payload.

### DRM Switch: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'drmswitch'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| folder            |Yes| Folder where the content to be DRM toggled is stored. ||
| drm               |No | A list of DRM systems o be applied to the VOD stream. This could be `"playready"` and/or `”widevine”` and/or `”fairplay”` and/or `“cenc”` and/or `"aes"`. | [] |
| cpix              |No | This boolean indicates whether DRM will be handled using a CPIX document| false |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| source_storage    |No | This is used to indicate where the VOD assets are stored (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |

### DRM Switch: Payload example

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "drmswitch"
  },
  "parameters": {
    "content_id": "demo1",
    "folder": "vualto-test-1",
    "drm": [
      "fairplay",
      "cenc"
    ],
    "rest_endpoints": [
      "https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
      "http://your.custom.endpoint"
    ]
  }
}
```

### DRM Switch: Callback properties

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
| custom_data       | Returns the custom data submitted to the workflow. |

## CREATE MP4

This workflow allows you to create an MP4 from a VOD asset.

### Create MP4: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'createmp4'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| source_folder     |Yes| Folder where the VoD source content can be found. ||
| output_folder     |No | Folder where the MP4 should be saved. | <source_folder> |
| retries           |No | Retry limit when attempting to copy from the source storage. | 0 |
| mp4_filename      |No | The name of the resulting mp4 file. | <content_id>.mp4 |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| mezzanine         |No | This boolean indicates whether the generated mp4 contains all the video tracks or just the highest bitrate audio and video track. | false |
| source_storage    |No | This is used to indicate where the source VOD is stored (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| destination_storage         |No | This is used to indicate the destination for the generated MP4 (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | <source_storage> |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |

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
| custom_data       | Returns the custom data submitted to the workflow. |

## BUILD THUMBNAILS

This workflow allows you to generate thumbnail assets which can then be used for video timeline previews.

### Build Thumbnails: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'build_thumbnails'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| source            |Yes| URL of the HLS source from which to create assets. Live sources (.isml) must be in a state of `stopped`. ||
| filename_prefix   |No | Prefix for the file names of generated assets, eg: `<target_filename>_sprite.jpg` .| <content_id> |
| output_folder     |Yes| This is the folder where the resulting assets will be saved on the destination storage. | <content_id> |
| preview_thumbnails_interval   |No | Interval time between thumbnail captures in seconds. | 10 |
| video_fps         |No | Fallback parameter, which will only be used if the fps cannot be obtained from the source metadata. | 24 |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| destination_storage         |No | This is used to indicate the destination for the generated thumbnail assets (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |

### Build Thumbnails: Payload example

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

### Build Thumbnails: Callback properties


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
| custom_data       | Returns the custom data submitted to the workflow. |

## VOD REMIX

This workflow allows you to create a virtual VOD asset that is just a playlist referencing other VOD streams or video files.

### VOD Remix: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodremix'.||
| content_id        |Yes| This is the id for the resulting VOD.||
| output_folder     |Yes| This is the folder where the resulting VOD will be saved on the destination storage. This is cleared before the capture is uploaded.||
| clips             |Yes| This is an array of sources, with optional start and end times, please see the example request below. ||
| clips.source      |Yes| This would need to be either a VOD stream or the URL to a video file. Must be accessible from both Task Engine and the Origin. E.g. `http://mydomain.com/manifest.ism`, `https://bucket-name.s3-eu-west-1.amazonaws.com/path/test.mp4`. Required unless `clips.sources` is used ||
| clips.sources     |Yes| An array of video and audio files (tracks or renditions). E.g. `["http://library/path/low.mp4","http://library/high.mp4","http://library/eng.m4a"]`. Required unless `clips.source` is used ||
| clips.start       |No | UTC timestamp for the start timecode. e.g 2016-10-13T10:10:40.251Z OR Offsets e.g. “hh:mm:ss”||
| clips.end         |No | UTC timestamp for the end timecode e.g 2016-10-13T10:20:40.251Z OR Offsets e.g. “hh:mm:ss” ||
| clips.frame_accurate    |No | This boolean indicates whether the specified clip will be trimmed using frame accuracy. | false |
| clips.output_description |No | This boolean indicates that this clip should be used to set the target profile. There should be only one clip with this set to true. | false |
| clips.markers       |No | This object contains all the information related to the SCTE35 markers for the clip (see [AVOD and Live Compose](TaskEngineWorkflowFeatures.html#avod-and-live-compose) section). | |
| clips.markers.timescale      |No | This is used to define the base timescale for the SCTE35 markers. | 1000 |
| clips.markers.frame_accurate |No | This is used to add sync samples at the markers position. | clip.frame_accurate |
| clips.markers.meta_events    |No | Array of meta_event objects. | |
| clips.markers.meta_events.presentation_time |Yes | This is the time position at which the marker will be inserted relative to the clip.| |
| clips.markers.meta_events.duration |Yes | This is the duration of the marker. | |
| clips.markers.meta_events.type |No | `replace` or `insert`. This indicates whether the intention is to replace the underlying content with ads, or to insert ads and then resume from the point the ad was inserted. | `replace` |
| output_file       |No | Name of the output .mp4 file. | remix.mp4 |
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified.||
| drm               |No | A list of DRM systems o be applied to the VOD stream. This could be `"playready"` and/or `”widevine”` and/or `”fairplay”` and/or `“cenc”` and/or `"aes"`.  If this value isn’t present or `"clear"` is specified as a system a DRM-free manifest is created. | ["clear"] |
| cpix              |No | This boolean indicates whether DRM will be handled using a CPIX document. | false |
| empty_target      |No | This boolean indicates whether the target folder in storage should be cleared before the output assets are save. | true |
| enable_drm        |No | This boolean indicates whether the drm manifest (if created - read `drm` parameter) should be enabled. | true |
| destination_storage         |No | This is used to indicate the destination for the VOD assets (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| remote_execute_timeout_seconds    |No | This parameter is used to specify the timeout length in seconds for remote workers to complete execution. | 0 |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |
| live_compose      |No | Generate a live stream looping the playlist (as opposed to the default VOD) |`false`|
| stream_start_time |No | This field accepts a UTC timestamp eg. 2016-10-13T10:10:40.251Z that will be used to indicate when the Live Compose stream should start. | |
| dvr_window_length |No | The duration in seconds of the live stream DVR window |60|
| custom_active_manifest_name |No | This field accepts a string that will be used as the manifest name. | |
| transcode_proxy   |No | This field accepts the url for the remote transcode proxy. | |

### VOD Remix: JSON Payload example

```json
{
  "parameters": {
    "content_id": "demo_1",
    "output_folder": "demo_1",
    "transcode_proxy": "https://vualto.transcode-proxy.com",
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
  },
  "client": "demo-client",
  "job": {
      "workflow": "vodremix"
  }
}
```

### VOD Remix: Callback properties

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
| metadata.duration | Duration of the VOD event. |
| custom_data       | Returns the custom data submitted to the workflow. |

## GENERATE GIF

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
| destination_storage         |No | This is used to indicate the destination for the generated MP4 (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |

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
| custom_data       | Returns the custom data submitted to the workflow. |

## CAPTURE FRAME

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
| destination_storage         |No | This is used to indicate the destination for the generated MP4 (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |

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
      "image_filename": "frame.jpg",
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
| custom_data       | Returns the custom data submitted to the workflow. |

## ASSET DELETE

This workflow allows you to delete individual assets without deleting an entire VOD directory.

### Asset Delete: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'asset_delete'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| files             |Yes| Array of files to be deleted from S3 ||
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| source_storage    |No | This is used to indicate where the source VOD is stored (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |

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
| custom_data       | Returns the custom data submitted to the workflow. |

## MEDIATAILOR CHANNEL ASSEMBLY

This workflow allows for creating and updating Live Compose streams - very similar to [VOD REMIX](TaskEngineWorkflows.html#vod-remix) [Live Compose](TaskEngineWorkflowFeatures.html#avod-and-live-compose). Please refer to [Live Compose with Manifest Manipulation](TaskEngineWorkflowFeatures.html#live-compose-with-manifest-manipulation) for more information and ad break signaling examples.

### MediaTailor Channel Assembly: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'mediatailor_channel_assembly'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| clips             |Yes| This is an array of sources, each with optional start and end times, please see the example request below. ||
| clips.source      |Yes| This is a VOD stream. Currently only HLS streams are supported. E.g. `http://mydomain.com/manifest.m3u8`. ||
| clips.markers     |No | This object contains all the information related to the SCTE35 markers for the clip (see [AVOD and Live Compose](TaskEngineWorkflowFeatures.html#avod-and-live-compose) section). ||
| clips.markers.meta_events    |No | Array of meta_event objects. | |
| clips.markers.meta_events.presentation_time |Yes | This is the time position at which the marker will be inserted relative to the clip.| |
| clips.markers.meta_events.slate |Yes | This is the duration of the marker. | |
| restart_channel   |No | This boolean indicates whether the MediaTailor channel must be restarted or not. Channel restart is required if the source types do not match |true|
| dvr_window_length |No | The duration in seconds of the live stream DVR window |60|
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference) and returns it as part of the job callback. | |

### MediaTailor Channel Assembly: Payload example

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "mediatailor_channel_assembly"
  },
  "parameters": {
    "content_id": "demo-content",
    "clips": [
      {
        "source": "https://cdn.com/assets/1.m3u8"
      },
      {
        "source": "https://cdn.com/assets/2.m3u8"
      },
      {
        "source": "https://cdn.com/assets/3.m3u8"
      }
    ],
    "rest_endpoints": [
      "http://your.custom.endpoint"
    ]
  }
}
```

### MediaTailor Channel Assembly: Callback properties

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
| message           | Resulting live streaming URL |
| custom_data       | Returns the custom data submitted to the workflow. |

## MEDIATAILOR CHANNEL STATE

This workflow allows for stopping and starting MediaTailor channels.

### MediaTailor Channel State: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'mediatailor_channel_assembly'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| state             |Yes| Either `start` or `stop` ||
| delete            |No | This boolean indicates whether the channel must be deleted or not |false|
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |

### MediaTailor Channel State: Payload example

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "mediatailor_channel_state"
  },
  "parameters": {
    "content_id": "demo-content",
    "state": "stop",
    "delete": true
  }
}
```

### MediaTailor Channel State: Callback properties

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
| custom_data       | Returns the custom data submitted to the workflow. |

## VOD NPVR

This workflow will generate a VOD asset from segments captured through the Vualto Archiver. Segments are shared across different VOD assets which reduces storage requirements and processing time. 

A server side manifest is created, with and/or without DRM, that can be used for on the fly delivery of VOD content via the Unified Streaming Platform. The [VOD NPVR](TaskEngineWorkflows.md#vod-npvr) workflow includes support to inherit the DRM keys form a specified Vualto Archiver profile and stores it as a custom manifest. This workflow will include any SCTE35 markers that occurred during each segment.

### VOD NPVR: Parameters

| Parameter Name    | Required |  Description | Default |
| ----------------- | -------- | ------------ | ------- |
| workflow          |Yes| Specify 'vodnpvr'. ||
| content_id        |Yes| Unique identifier of the content. This is usually a key that allows identification of the content in the client’s system. ||
| clips             |yes| This is an array of sources, with optional start and end times, please see the example request below. ||
| clips.capture_id  |Yes| This would be the capture id for the Vualto Archiver event to be used as the source ||
| clips.start       |Yes| UTC timestamp for the start timecode. e.g 2016-10-13T10:10:40.251Z ||
| clips.end         |Yes| UTC timestamp for the end timecode e.g 2016-10-13T10:20:40.251Z ||
| output_folder     |Yes| The folder for processed files to be placed.  The ‘root’ folder will be specified in the client configuration. ||
| rest_endpoints    |No | Endpoints that will receive the callbacks defined in the workflow. Multiple end points can be specified. ||
| apply_custom_drm  |No | This boolean indicates whether a custom DRM manifest using drm keys from the specified Vualto Archiver profile. | false |
| profile_id        |No | This is the id for the profile used for the custom DRM manifest. ||
| custom_manifest_name  |No | The name to be given to the custom DRM manifest. | `custom.ism` |
| drm               |No | The type of DRM that is required. This could be “playready” and/or ”widevine” and/or ”fairplay” and/or “cenc” and/or "aes". If this value isn’t present the the normal DRM manifest is not created. | ["clear"] |
| cpix              |No | This boolean indicates whether DRM will be handled using a CPIX document. | false |
| empty_target      |No | This boolean indicates whether the target folder in storage should be cleared before the output assets are save. | true |
| source_storage    |No | This is used to indicate where the source content is stored (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | `S3` (system default) |
| destination_storage         |No | This is used to indicate the destination for the VOD assets (see [Storage Support](TaskEngineWorkflowFeatures.html#storage-support) section). | <source_storage> |
| remote_execute_timeout_seconds    |No | This parameter is used to specify the timeout length in seconds for remote workers to complete execution. | 0 |
| overwrite_segments |No | This boolean indicates whether segments already in use by other VOD assets should be overwritten when generating the current VOD asset. | false |
| custom_package_options    |No | This contains package options required to support SCTE35 markers within remix profiles. | `--timed_metadata --splice_media` |
| missing_content_limit |No | The limit in seconds of missing content over which the VOD asset generation is abandoned. Missing content is usually caused by discontinuities from the Archiver source stream | 5.0 |
| enable_drm        |No | This boolean indicates whether the drm manifest (if created - read `drm` parameter) should be enabled. | true |
| custom_data       |No | This field accepts consumer custom data (such as consumer internal reference ) and returns it as part of the job callback. | |
| transcode_proxy   |No | This field accepts the url for the remote transcode proxy | |

### VOD NPVR: JSON Payload example

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "vodnpvr"
  },
  "parameters": {
    "content_id": "vudrm_1",
    "clips": [
        {
            "start": "2020-09-08T14:10:00Z",
            "end": "2020-09-08T14:44:00Z",
            "capture_id": "test4"
        }
    ],
    "transcode_proxy": "https://vualto.transcode-proxy.com",
    "output_root": "output_root",
    "apply_custom_drm": true,
    "profile_id": "test4_drm",
    "custom_manifest_name": "manifest.ism",
    "drm": [
      "fairplay",
      "cenc",
      "clear"
    ],
    "cpix": false,
    "missing_content_limit": 700,
    "output_folder": "vudrm/test4_1599574200000_1599576240000",
    "rest_endpoints": [
      "https://vis.vuworkflow.staging.vualto.com/api/event/vuflow/taskenginecallback",
      "http://aaa.com/end",
      "http://bbb.com/end"
    ],
    "custom_data": { "custom_ref" : "ref-123" }
  }
}
```

### VOD NPVR: Callback properties

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
| segments          | Segments used for the VOD asset. |
| metadata.duration | Duration of the VOD event. |
| custom_data       | Returns the custom data submitted to the workflow. |

## WORKFLOW TRIGGER EXAMPLE

Example of a curl command to trigger ingest for the [VOD Stream](#vod-stream) workflow:

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
