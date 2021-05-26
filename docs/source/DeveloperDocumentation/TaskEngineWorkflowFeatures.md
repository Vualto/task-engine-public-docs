# TASK ENGINE WORKFLOW FEATURES

## PRIORITY

The Task Engine supports ordering of jobs by priority. The priority parameter can be submitted as part of the json payload being submitted. The priority is in ascending order as follows:

```
1 - Top Priority  
.  
.  
5 - Default  
.  
.  
10 - Least Priority
```

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

### Priority Slots

To further enhance support for priority jobs, a new setting has been added to reserve job slots for default to top priority (1 - 5) jobs. This will allow a client to schedule as many low priority jobs as required while still having job slots free for default and priority jobs.

The advantage of using priority slots is to stop the queue from being held up by low priority jobs. This is especially useful if long running jobs are given a lower priority than the default as they can be queued up without exhausting the setup's concurrency availability.

**Important note**: This will not increase the number of max concurrent jobs but it will reserve some fo the concurrency for jobs with priority between 1 and 5. As an example, if a setup has 5 maximum concurrent jobs and the priority slots is set to 2, any job can ustilise 3 concurrency slots but only jobs with priority between 1 and 5 can use the 2 priority slots.

## STITCHING CLIPS

The Task Engine includes a feature that will allow multiple clips to be stitched together into a single clip, in a single job. This can be done by defining multiple objects within the `"clips"` parameter in the json payload for [VOD Capture](TaskEngineWorkflows.md#vod-capture). This also allows a mixture of live and VoD sources to be captured and stitched together into a new clip. The example below shows how the `"clips"` parameter would need to be provided to achieve this.

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

## MULTIPLE SOURCES

In some cases, a live stream could have multiple origins setup (eg. for load balancing the origin servers). The Task Engine, allows for both streams to be defined as the source for a capture. It is smart enough to find which live stream will provide the best output capture and use that stream as the source. If the Task Engine discovers discontinuities within the streams, it will use segments from both streams to try and generate a clip with the least number of missing fragments.

The streams can be defined in the `"sources"` parameter when executing the [VOD Capture](TaskEngineWorkflows.md#vod-capture) workflow.

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

## GENERATE DOWNLOAD CLIPS

The Task Engine [VOD Capture](TaskEngineWorkflows.md#vod-capture) workflow supports generating download clips without creating VoD assets. This is done by setting the property `"generate_vod"` to false and `"generate_mp4"` to true. It is important that if `"generate_vod"` is set to false, to not manually override the `"create_dref"` parameter. Setting `"create_dref"` to true will lead to a failed workflow as this requires VoD assets to generate DREF mp4s.

The resulting download will be an MP4 containing all the video, audio and caption tracks defined using the clip's `"filter"` parameter. If no filter is defined, the resulting MP4 will contain all the tracks available in the stream.

## SCHEDULER

The Task Engine supports scheduling of jobs via the `run_at` and `sempahore_url` job attributes. Jobs are moved from a queue_state of `scheduled` to a queue_state of `queued` via a scheduler-worker. The interval at which this runs is pulled from the database settings table (schedule_interval, default: 1 hour).

The scheduler-worker looks for jobs which have a queue_state of `scheduled`, a `run_at` time in the past and a `semphore_url` that returns a successful response (2XX/3XX).

The schedule_interval can be set via an api call. (where x is time in seconds)

`post '/settings'`

```json
{
  "client": "demo-client",
  "name": "schedule_interval",
  "setting": "<x>"
}
```

A jobs `run_at` and `semaphore_url` attributes can be set in multiple ways. The `run_at` defaults to the time the job was created. If the job's `run_at` time is in the future, a log will be added to indicate such. The `run_at` time format should be `yyyy-MM-ddTHH:mm:ss.fff`.

1. When submitting a job

`post '/job'`

```json
{
  "client": "demo-client",
  "job": {
    "workflow": "vodcapture",
    "run_at": "2040-06-06T10:00:00.000",
    "semaphore_url": "https://www.vualto.com/"
  }
}
```

In the above example the job will be queued at 10:00AM on the 6th of June 2040 and when `https://www.vualto.com/` returns a successful response. The following message `Job will run at: "2040-06-06T10:00:00.000"` will be logged against the job.

2. When updating an existing job

`put '/jobs/:job_id'`

```json
{
  "client": "demo-client",
  "run_at": "2040-06-06T10:00:00.000",
  "semaphore_url": "https://www.vualto.com/"
}
```

3. When submitting a capture with a clip end time in the future

If a capture is submitted with a clip end time that is in the future, it will be automatically scheduled to run at the end time of the clip which is furthest in the future. The exception to this is if the `run_at` time is specified and is further in the future than the end time, then the `run_at` time will be used.

## TRACK PROPERTIES

There are instances when track properties need to be added to specific tracks within the VOD manifest. This usually occurs when custom track descriptions or track roles need to be set. The Task Engine supports adding track properties to audio and subtitle tracks. Filtering of tracks is based on type (`audio` or `textstream`) and a combination of language and/or track role. The filters and values can be set in Vualto's Central Configuration so they can easily be applied to all VODs being captured or ingested. They can also be defined as part of the job submission. The value set will overwrite the existing value for the property, if it already exists. Below are some samples of how the filters can be defined.

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
  "textstream": {
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
  "textstream": {
    "eng": {
      "track_role" : "caption",
      "track_description": "English CC"
    }
  }
}
```

Support is confirmed for `track_description`, `track_role` and `track_name` properties, but other properties may be supported.

## STORAGE SUPPORT

The Task Engine supports multiple storage types for ingesting content and saving VOD. Support has also been added so a combination of storage types can be used for the same job. This can be done by setting the `source_storage` and `destination_storage` in the job payload (for supported workflows). Eg. Ingesting content from local storage and save VOD assets on S3 would require `source_storage` to be set to `local` and `destination_storage` to be set to `S3`.

The system default storage type is Amazon S3, however; the default can be customised per client as well as set on a job per job basis. Additional setup may be required when using `local` storage, as the folders will need to be mapped to the worker docker containers.

Natively supported storage types:

- Amazon S3 (`S3`)
- Azure Blob Storage (`azure_blob`)
- On premises infrastructure (`local`)


## PREVIEW THUMBNAILS

Preview thumbnails refers to the thumbnails that appear on the a video player's timeline as the user hovers over the progress bar. These can be generated on 3 different occasions:

- When capturing content, by setting the `preview_thumbnails` property to `true` when submitting a [VOD Capture](TaskEngineWorkflows.html#vod-capture) job.
- When ingesting content, by setting the `preview_thumbnails` property to `true` when submitting a [VOD Stream](TaskEngineWorkflows.html#vod-stream) job.
- By submitting a separate [Build Thumbnails](TaskEngineWorkflows.html#build-thumbnails) job.

The process is pretty much the same for all of the above. A sprite is generated with thumbnails at every 'x' second intervals (default is 10 seconds) and a VTT file which relates each thumbnail within the sprite to corresponding time within the stream. These files must be made accessible to the players. On cloud platforms, this is usually done by create a CDN for .jgp and .vtt files. The [Vualto Assets API](https://docs.vualto.com/projects/VIS/en/latest/assets.html) can be used to retrieve the URL for the VTT file.

**Important Note**: The interval is a very close approximation and not an exact interval.The thumbnail image is generated from the closest iframe.

Different players reference the VTT file differently. The following are some examples of how some players reference them:

- [Bitmovin](https://bitmovin.com/demos/thumbnail-seeking)
- [THEOPlayer](https://docs.portal.theoplayer.com/docs/next/add-ons/user-engagement/text-track/text-track-5-how-to-implement-thumbnails/#__docusaurus) 

## AVOD AND LIVE COMPOSE

The Vualto Task Engine now supports creating AVOD and Live Compose playlists using the vodremix workflow.  

- AVOD refers to a playlist of clips (VOD or MP4s) with support for server side ad insertion and replacement through SCTE35 markers.
- Live Compose refers to a playlist of clips (VOD or MP4s), played out as a live stream, with support for server side ad replacement through SCTE35 markers. Ad insertion is currently not supported for simulated live streams. Live Compose playlists can be created by setting the `live_compose` flag to `true` in the job payload. The rest of the payload is identical to AVOD.

A markers object can be added to each clip object. The marker object supports setting the timescale and sync samples for each marker. The main use for the marker object is to specify meta events (SCTE35 markers. It's important to note that the `presentation_time` property of each meta event is relative to the clip and will always consider the clip start to be "00:00:00".

When adding meta events for ad replacement ( `"type": "replace"` - default) the `duration` property will indicate how much of the original clip content is to be replaced by an ad. When adding meta events for ad insertion (`"type": "insert"`) the `duration` property indicates how long the ad inserted will be. The original clip content will continue from where it stopped when the ad is inserted.

The AVOD example below shows three clips being stitched together. The first clip has two SCTE 35 markers, a 4 minute and 30 second replacement marker at the 10 minute mark and a 30second ad insertion at the 16 minute mark. The second clip doesn't have any SCTE 35 markers. The third clip has a 2 minute replacement marker at the 5 minute mark.

```json
{
  "parameters": {
    "content_id": "demo_1",
    "output_folder": "demo_1",
    "clips": [
      {
        "sources": [
          "https://bucket.s3-eu-west-1.amazonaws.com/clip1_1080.mp4",
          "https://bucket.s3-eu-west-1.amazonaws.com/clip1_720.mp4",
          "https://bucket.s3-eu-west-1.amazonaws.com/clip1_480.mp4"
        ],
        "markers": {
          "frame_accurate": true,
          "meta_events": [
            {
              "presentation_time": "00:10:00",
              "duration": "00:04:30.000"
            },
            {
              "type": "insert",
              "presentation_time": "00:16:00",
              "duration": "00:00:30.000"
            }
          ]
        }
      },
      {
        "sources": [
          "https://bucket.s3-eu-west-1.amazonaws.com/clip2_1080.mp4",
          "https://bucket.s3-eu-west-1.amazonaws.com/clip2_720.mp4",
          "https://bucket.s3-eu-west-1.amazonaws.com/clip2_480.mp4"
        ],
        "start": "00:00:00.000",
        "end": "00:10:00.000",
        "frame_accurate": "true"
      },
      {
        "sources": [
          "https://bucket.s3-eu-west-1.amazonaws.com/clip3_1080.mp4",
          "https://bucket.s3-eu-west-1.amazonaws.com/clip3_720.mp4",
          "https://bucket.s3-eu-west-1.amazonaws.com/clip3_480.mp4"
        ],
        "start": "00:00:00.000",
        "end": "00:20:00.000",
        "frame_accurate": "true",
        "markers": {
          "meta_events": [
            {
              "type": "replace",
              "presentation_time": "00:05:00",
              "duration": "00:02:00"
            }
          ]
        }
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

```json

```

## LIVE COMPOSE WITH MANIFEST MANIPULATION

The Vualto Task Engine now supports manifest manipulation to generate live compose streams (VOD playlist looping), using AWS Mediatailor Channel Assembly. This workflow takes VOD streaming URLs directly and the resulting Live stream fragments come frome the original VOD streaming URLs - this could result in cost savings in caching layers in some instances.

With this workflow, it is also possible to condition live streams for SSAI (server side ad insertion), however, it requires ad slate clips (also in the form of VODs) in order to signal ad breaks. The slate clips are stitched linearly with the clip sources at the given `presentation_time` (relative to the clip source) and the relevant ad break timed metadata added to the resulting live stream. If no `presentation_time` is provided, it will default to `00:00:00` (pre-roll).

> Important! VOD sources must be encoded similarly. For example the same number of renditions, codecs, resolutions, etc. Job requests with mixed encoding proflies will fail validation.

The example below would result in a live stream where assets 1, 2, and 3 would loop infintetly with ad breaks (with ad-slate as underlying content) in between each clip. Clip 3 would have an additional ad break approximately 30 seconds in the video (mid-roll).

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
        "source": "https://cdn.com/assets/1.m3u8",
        "markers": {
          "meta_events": [
            {
              "slate": "https://cdn.com/assets/ad-slate.m3u8",
              "presentation_time": "00:00:00"
            }
          ]
        }  
      },
      {
        "source": "https://cdn.com/assets/2.m3u8",
        "markers": {
          "meta_events": [
            {
              "slate": "https://cdn.com/assets/ad-slate.m3u8",
              "presentation_time": "00:00:00"
            }
          ]
        }
      },
      {
        "source": "https://cdn.com/assets/3.m3u8",
        "markers": {
          "meta_events": [
            {
              "slate": "https://cdn.com/assets/ad-slate.m3u8",
              "presentation_time": "00:00:00"
            },
            {
              "slate": "https://cdn.com/assets/ad-slate.m3u8",
              "presentation_time": "00:00:30"
            }
          ]
        }
      }
    ],
    "rest_endpoints": [
      "http://your.custom.endpoint"
    ]
  }
}
```