<!-- 
    NOTE: The generated css for the TOC doesn't have a
    scrollbar so it is added into the base.css file AFTER
    mkDocs Build runs. This stops the TOC running off the 
    end of the page:
    
    .bs-sidebar.well {
    padding: 0;
    max-height:85vh;
    overflow-y: auto;
    }
-->

## Basic Requirements

All requests have the prerequisite ```api/```

For example the "Get channel/{id}" REST call would be ```GET api/channel/{guid}```

All Id's are Guid's


# Channels

## GET channel/{id}
**Example Request: **

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```
**Example Response: **

Headers: 
```
X-Transaction-ID: {string}
```

Example JSON Response:

Will contain the channel JSON but will also contain the configuration JSON for a channel, which is a template JSON file that 
can be  `POST`ed back. Recommended to view the JSON in a JSON viewer.
```json
{    
    "encoders": [
        {
            "name": "FS CRW01 Channel01",
            "raiseOnError": true,
            "type": "MediaExcelHero",
            "encoderSpecific": {
                "heroApiUrlA": "http://10.196.50.49/hms/soapproxy.php",
                "heroApiUrlB": "http://10.196.50.49/hms/soapproxy.php",
                "heroChannelId": 830,
                "heroDeviceId": 5,
                "supportsContinousTimeStamp": true
            },
            "streamingServer": {
                "type": "USP",
                "streamingServerSpecific": {
                    "primaryPublishingPoint": { 
                     "publishingpointUrl": "http://10.196.50.53:8080/live/fs/crw01
                     /ch01/channel1/live.isml",
                        "uspAPI": "http://80.169.3.216:9292",
                        "uspAPIKey": "dff32df7-59a3-49e1-ae02-976e342beb55",
                        "dvrWindowLength": 30,
                        "filePath": "/home/user/live/fs/crw01/ch01/channel1/"

                    },
                    "secondaryPublishingPoint": {
                        "publishingpointUrl": "",
                        "uspAPI": "",
                        "uspAPIKey": "",
                        "dvrWindowLength": 30,
                        "filePath": ""
                    }
                }
            },
            "outputUrls": [
                {
                    "streamProtocol": "HLS",
                    "liveCdnUrl": "http://cdn.channel1.vis.org/live.isml/live.m3u8",
                    "vodCdnUrl": "http://cdn.channel1.vis.org/vod.ism/vod.m3u8",
                    "liveOriginPrimaryUrl": "http://cc.vualto.org/live/fs/crw01
                    /ch01/channel1/live.isml/live.m3u8",
                    "vodOriginPrimaryUrl": "http://cc.vualto.org/live/fs/crw01
                    /ch01/channel1/vod.ism/vod.m3u8",
                    "liveOriginSecondaryUrl": "",
                    "vodOriginSecondaryUrl": ""
                },
                {
                    "streamProtocol": "DASH",
                    "liveCdnUrl": "http://cdn.channel1.vis.org/live.isml/live.mpd",
                    "vodCdnUrl": "http://cdn.channel1.vis.org/vod.ism/vod.mpd",
                    "liveOriginPrimaryUrl": "http://cc.vualto.org/live/fs/crw01
                    /ch01/channel1/live.isml/live.mpd",
                    "vodOriginPrimaryUrl": "http://cc.vualto.org/live/fs/crw01
                    /ch01/channel1/vod.ism/vod.mpd",
                    "liveOriginSecondaryUrl": "",
                    "vodOriginSecondaryUrl": ""
                },
                {
                    "streamProtocol": "HSS",
                    "liveCdnUrl": "http://cdn.channel1.vis.org/live.isml/Manifest",
                    "vodCdnUrl": "http://cdn.channel1.vis.org/vod.ism/Manifest",
                    "liveOriginPrimaryUrl": "http://cc.vualto.org/live/fs/crw01
                    /ch01/channel1/live.isml/Manifest",
                    "vodOriginPrimaryUrl": "http://cc.vualto.org/live/fs/crw01
                    /ch01/channel1/vod.ism/Manifest",
                    "liveOriginSecondaryUrl": "",
                    "vodOriginSecondaryUrl": ""
                },
                {
                    "streamProtocol": "DASH",
                    "liveCdnUrl": "http://cdn.channel1.vis.org/live.isml/live.f4m",
                    "vodCdnUrl": "http://cdn.channel1.vis.org/vod.ism/vod.f4m",
                    "liveOriginPrimaryUrl": "http://cc.vualto.org/live/fs/crw01
                    /ch01/channel1/live.isml/live.f4m",
                    "vodOriginPrimaryUrl": "http://cc.vualto.org/live/fs/crw01
                    /ch01/channel1/vod.ism/vod.f4m",
                    "liveOriginSecondaryUrl": "",
                    "vodOriginSecondaryUrl": ""
                }
            ]
        },
        {
            "name": "FS CHF01 Channel01",
            "type": "MediaExcelHero",
            "raiseOnError": false,
            "encoderSpecific": {
                "heroApiUrlA": "http://80.169.3.218:80/hms/soapproxy.php",
                "heroApiUrlB": "http://80.169.3.219:80/hms/soapproxy.php",
                "heroChannelId": 829,
                "heroDeviceId": 5,
                "supportsContinousTimeStamp": true
                 },
            "streamingServer": {
                "type": "USP",
                "streamingServerSpecific": {
                    "primaryPublishingPoint": {
                       "publishingpointUrl": "http://10.196.50.54:8080/live/fs/chf01
                       /ch01/channel1/live.isml",
                        "uspAPI": "http://80.169.3.217:9292",
                        "uspAPIKey": "dff32df7-59a3-49e1-ae02-976e342beb55",
                        "dvrWindowLength": 30,
                        "filePath": "/home/user/live/fs/chf01/ch01/channel1/"

                    },
                    "secondaryPublishingPoint": {
                        "publishingpointUrl": "",
                        "uspAPI": "",
                        "uspAPIKey": "",
                        "dvrWindowLength": 30,
                        "filePath": ""
                    }
                }
            },
            "outputUrls": [
               {
                    "streamProtocol": "HLS",
                    "liveCdnUrl": "http://cdn.channel1.vis.org/live.isml/live.m3u8",
                    "vodCdnUrl": "http://cdn.channel1.vis.org/vod.ism/vod.m3u8",
                    "liveOriginPrimaryUrl": "http://cc.vualto.org/live/fs/chf01
                    /ch01/channel1/live.isml/live.m3u8",
                    "vodOriginPrimaryUrl": "http://cc.vualto.org/live/fs/chf01
                    /ch01/channel1/vod.ism/vod.m3u8",
                    "liveOriginSecondaryUrl": "",
                    "vodOriginSecondaryUrl": ""
                },
                {
                    "streamProtocol": "DASH",                     
                    "liveCdnUrl": "http://cdn.channel1.vis.org/live.isml/live.mpd",
                    "vodCdnUrl": "http://cdn.channel1.vis.org/vod.ism/vod.mpd",
                    "liveOriginPrimaryUrl": "http://cc.vualto.org/live/fs/chf01
                    /ch01/channel1/live.isml/live.mpd",
                    "vodOriginPrimaryUrl": "http://cc.vualto.org/live/fs/chf01
                    /ch01/channel1/vod.ism/vod.mpd",
                    "liveOriginSecondaryUrl": "",
                    "vodOriginSecondaryUrl": ""
                },
                {
                    "streamProtocol": "HSS",
                    "liveCdnUrl": "http://cdn.channel1.vis.org/live.isml/Manifest",
                    "vodCdnUrl": "http://cdn.channel1.vis.org/vod.ism/Manifest",
                    "liveOriginPrimaryUrl": "http://cc.vualto.org/live/fs/chf01
                    /ch01/channel1/live.isml/Manifest",
                    "vodOriginPrimaryUrl": "http://cc.vualto.org/live/fs/chf01
                    /ch01/channel1/vod.ism/Manifest",
                    "liveOriginSecondaryUrl": "",
                    "vodOriginSecondaryUrl": ""
                },
                {
                    "streamProtocol": "HDS",
                    "liveCdnUrl": "http://cdn.channel1.vis.org/live.isml/live.f4m",
                    "vodCdnUrl": "http://cdn.channel1.vis.org/vod.ism/vod.f4m",
                    "liveOriginPrimaryUrl": "http://cc.vualto.org/live/fs/chf01
                    /ch01/channel1/live.isml/live.f4m",
                    "vodOriginPrimaryUrl": "http://cc.vualto.org/live/fs/chf01
                    /ch01/channel1/vod.ism/vod.f4m",
                    "liveOriginSecondaryUrl": "",
                    "vodOriginSecondaryUrl": ""
                }
            ]
        }
    ]
} 
```

## GET channel

NOTE: Returns all data for all channels.

**Example request: **

Headers:
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```
**Example response:**

Headers:
```
X-Transaction-ID: {string}
```

**Example JSON Response:**

```Very similar to "GET channel/{id}" but as an array.```

## DELETE channel/{id}


**Example request: **

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```
**Example response: **

Headers: 
```
X-Transaction-ID: {string}
```

## POST channel

Add new Channel

**Example request: **

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```
**Example response: **

Headers: 
```
X-Transaction-ID: {string}
```

**Response Body Params:**
```text
ID: {string} - Optional Parameter. Same ID will cause and Update

Name: {string} - Name

ConfigurationJson: {string} - The Template channel JSON. Has to be the same JSON 
                              structure as the GET                             
```

Example Schema for JSON:
```json
{
  "encoders": {
            "type": "array",
            "items": {
                "title": "encoder",
                "type": "object",
                "properties": {
                    "name": { "type": "string" },
                    "raiseOnError": { "type": "boolean"},
                    "type": {
                        "type": "string",
                        "enum": [
                            "FFMPEGWindows",
                            "FFMPEGLinux",
                            "ExpressionEncoder",
                            "MediaExcelHero",
                            "FMLE"
                        ]
                    },
                    "encoderSpecific": {
                        "type": "object",
                        "oneOf": [
                            { "$ref": "#/definitions/heroEncoders" }
                        ]
                    },
                    "streamingServer": {
                        "title": "streaming Server",
                        "type": "object",
                        "properties": {
                            "type": {
                                "type": "string",
                                "enum": [
                                    "USP"
                                ]
                            },
                            "streamingServerSpecific": {
                                "type": "object",
                                "oneOf": [
                                    { "$ref": "#/definitions/usp" },
                                    { "$ref": "#/definitions/wowza" }
                                ]
                            }
                        }
                    },
                    "outputUrls": {
                        "type": "array",
                        "items": { 
                            "title": "stream",
                            "type": "object",
                            "anyOf": [
                                { "$ref": "#/definitions/hls" },
                                { "$ref": "#/definitions/hss" },
                                { "$ref": "#/definitions/dash" },
                                { "$ref": "#/definitions/hds" }
                            ]
                        }
                    }
                },
                "required": [ "name", "raiseOnError", "encoderSpecific", 
                "streamingServer", "outputUrls"}
        }
    },
    "definitions": {
        "heroEncoders": {
            "properties": {
                "heroApiUrlA": { "type": "string" },
                "heroApiUrlB": { "type": "string" },
                "heroChannelId": { "type": "integer" },
                "heroDeviceId": { "type": "integer" },
                "supportsContinousTimeStamp": { "type": "boolean" }
            },
            "required": [ "heroApiUrlA", "heroApiUrlB", "heroChannelId", 
            "heroDeviceId" ], "additionalProperties": false },
            "usp": {
            "properties": {
                "primaryPublishingPoint": {
                    "type": "object",
                    "properties": {
                        "publishingpointUrl": { "type": "string" }, 
                     "uspAPI": { "type": "string" },
                        "uspAPIKey": { "type": "string" },
                        "dvrWindowLength": { "type": "integer" },
                        "filePath": { "type": "string" }

                    },
                    "required": [ "publishingpointUrl", "uspAPI", "uspAPIKey", 
                    "dvrWindowLength", "filePath" ]
                },
                "secondaryPublishingPoint": {
                    "type": "object",
                    "properties": {
                        "publishingpointUrl": { "type": "string" },
                        "uspAPI": { "type": "string" },
                        "uspAPIKey": { "type": "string" },
                        "dvrWindowLength": { "type": "integer" },
                        "filePath": { "type": "string" }
                    },
                    "required": [ "publishingpointUrl", "uspAPI", "uspAPIKey", 
                    "dvrWindowLength", "filePath" ]
                }
            },
            "required": [ "primaryPublishingPoint", "secondaryPublishingPoint" ],
            "additionalProperties": false
        },
        "wowza": {
            "properties": {
                "not set": { "type": "string" }
                },
                "additionalProperties": false
        },
        "hls": {
            "properties": {
                "streamProtocol": { "enum": [ "HLS" ] }, 
                "liveCdnUrl": {
                    "type": "string"
                },
                "vodCdnUrl": {
                    "type": "string"
                },
                "liveOriginPrimaryUrl": {
                    "type": "string"
                },
                "vodOriginPrimaryUrl": {
                    "type": "string"
                },
                "liveOriginSecondaryUrl": {
                    "type": "string"
                },
                "vodOriginSecondaryUrl": {
                    "type": "string"
                }
            },
            "required": [ "streamProtocol", "liveCdnUrl", "vodCdnUrl", 
            "liveOriginPrimaryUrl", "vodOriginPrimaryUrl" ],
            "additionalProperties": false
        },
        "hss": {
            "properties": {
                "streamProtocol": { "enum": [ "HSS" ] },
                "liveCdnUrl": {
                    "type": "string"
                },
                "vodCdnUrl": {
                    "type": "string"
                },
                "liveOriginPrimaryUrl": {
                    "type": "string" 
                },
                "vodOriginPrimaryUrl": {
                    "type": "string"
                },
                "liveOriginSecondaryUrl": {
                    "type": "string"
                },
                "vodOriginSecondaryUrl": {
                    "type": "string"
                }
            },
            "required": [ "streamProtocol", "liveCdnUrl", "vodCdnUrl", 
            "liveOriginPrimaryUrl", "vodOriginPrimaryUrl" ], "additionalProperties": 
            false}, "dash": {
            "properties": {
                "streamProtocol": { "enum": [ "DASH" ] },
                "liveCdnUrl": {
                    "type": "string"
                },
                "vodCdnUrl": {
                    "type": "string"
                },
                "liveOriginPrimaryUrl": {
                    "type": "string"
                },
                "vodOriginPrimaryUrl": {
                    "type": "string"
                },
                "liveOriginSecondaryUrl": {
                    "type": "string"
                },
                "vodOriginSecondaryUrl": { 
                 "type": "string"
                }
            },
            "required": [ "streamProtocol", "liveCdnUrl", "vodCdnUrl", 
            "liveOriginPrimaryUrl", "vodOriginPrimaryUrl" ],
            "additionalProperties": false},
        "hds": {
            "properties": {
                "streamProtocol": { "enum": [ "HDS" ] },
                "liveCdnUrl": {
                    "type": "string"
                },
                "vodCdnUrl": {
                    "type": "string"
                },
                "liveOriginPrimaryUrl": {
                    "type": "string"
                },
                "vodOriginPrimaryUrl": {
                    "type": "string"
                },
                "liveOriginSecondaryUrl": {
                    "type": "string"
                },
                "vodOriginSecondaryUrl": {
                    "type": "string"
                }
            },
            "required": [ "streamProtocol", "liveCdnUrl", "vodCdnUrl", 
            "liveOriginPrimaryUrl", "vodOriginPrimaryUrl"],"additionalProperties": false
        }
    }
}
```

## PUT channel/{id}

**Example request: **

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```
**Example response: **

Headers: 
```
X-Transaction-ID: {string}
```
**Response Body Params:**
```text
ID: {string} - Optional Parameter. Same ID will cause and Update

Name: {string} - Name

ConfigurationJson: {string} - The Template channel JSON. Has to be the same JSON 
                              structure as the GET                             
```


## GET channel/statistics/{id}

Returns the Stats for the channels that are available within the HMS API

** Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```
**Example response: ** 

Headers: 
```
X-Transaction-ID: {string}
```

**Example JSON Response:**
```json
{
    "statistics": {
            "information": {
                "?xml": {
                    "@version": "1.0"
                },
                "HERAStats": {
                    "channel_id": "5",
                    "processId": "705",
                    "configName": "Channel 20",
                    "cpuLoad": "4.05",
                    "cpuSystemIdle": "93.99",
                    "cpuLoadMeasureDuration": "1.21",
                    "processMemory": "273700",
                    "vmSize": "1932724",
                    "videoPid": "0",
                    "audioPid0": "0",
                    "audioPid1": "0",
                    "audioPid2": "0",
                    "audioPid3": "0",
                    "inputFrameRate": "25.00",
                    "inputFrameCount": "55512",
                    "inputFrameLost": "0",
                    "inputFrameBuffered": "0",
                    "deinterlacedFrameBuffered": "0", 
                    "inputVideoBitRate": "124413986",
                    "inputAudioBitRate": "1535949",
                    "inputLostPackets": "0",
                    "inputPacketCount": "55512",
                    "inputAspectRatio": "4:3",
                    "inputByteCount": "34959237120",
                    "inputBytesDropped": "0",
                    "inputBitRate": "125953057",
                    "inputLastTime": "1437044031",
                    "startTime": "1437041811",
                    "currentTime": "1437044031",
                    "currentClock": "7524929",
                    "leftVolume": "255",
                    "rightVolume": "255",
                    "inputPcmLeveldB": "0.00",
                    "demuxerOffset": "0",
                    "vodProcState": "0",
                    "errorMessage": "",
                    "privateData": "",
                    "privateDataType": "0",
                    "channelPcmLevelsdB": "-1.$ -1.$ 0.0 0.0 0.0 0.0 0.0 0.0 ",
                    "progress": "0.00",
                    "outputCount": "4",
                    "statMode": "-1",
                    "sourceType": "6",
                    "sourceStat": "0",
                    "sourceInfo": "sdi[board_id:1 ch_id:1]",
                    "sourceReplaceFile": "",
                    "sourceReplaceType": "-1",
                    "videoFreeze": "0",
                    "audioSilence": "0",
                    "secondSource": {
                        "sourceType": "-1", 
                        "sourceStat": "-1",
                        "sourceInfo": ""
                        },
                    "muted": "0",
                    "myHmsIp": "10.196.50.49",
                    "stream": [
                        {
                            "@id": "0",
                            "videoTargetRate": "1300000",
                            "videoRate": "1315256",
                            "audioTargetRate": "64000",
                            "audioRate": "0",
                            "muxRate": "1315256",
                            "encoderLastClock": "7524929",
                            "encoderFrameCount": "55472",
                            "muxedLastClock": "7524929",
                            "muxedFrameCount": "55472",
                            "frameRate": "25.00",
                            "actualEncRate": "25.02",
                            "currentPosition": "0",
                            "totalDuration": "0",
                            "encodingSpeed": "0",
                            "progress": "0.00",
                            "videoCodecType": "0",
                            "audioCodecType": "2",
                            "dspId": "-2",
                            "coreId": "-1",
                            "dspLoad": "0.00",
                            "inputFrameBuffered": "0",
                            "outputFrameBuffered": "0",
                            "inputFrameCount": "55512",
                            "outputFrameCount": "55472",
                            "bufferedFrameCount": "40", 
                            "outputStartTime": "1437041809"
                        },
                        {
                            "@id": "1",
                            "videoTargetRate": "850000",
                            "videoRate": "852891",
                            "audioTargetRate": "64000",
                            "audioRate": "0",
                            "muxRate": "852891",
                            "encoderLastClock": "7524929",
                            "encoderFrameCount": "55473",
                            "muxedLastClock": "7524929",
                            "muxedFrameCount": "55473",
                            "frameRate": "25.00",
                            "actualEncRate": "24.99",
                            "currentPosition": "0",
                            "totalDuration": "0",
                            "encodingSpeed": "0",
                            "progress": "0.00",
                            "videoCodecType": "0",
                            "audioCodecType": "2",
                            "dspId": "-2",
                            "coreId": "-1",
                            "dspLoad": "0.00",
                            "inputFrameBuffered": "0",
                            "outputFrameBuffered": "0",
                            "inputFrameCount": "55512",
                            "outputFrameCount": "55474",
                            "bufferedFrameCount": "38",
                            "outputStartTime": "1437041809"
                        },
                        {
                            "@id": "2", 
                            "videoTargetRate": "300000",
                            "videoRate": "299267",
                            "audioTargetRate": "64000",
                            "audioRate": "64054",
                            "muxRate": "363537",
                            "encoderLastClock": "7524929",
                            "encoderFrameCount": "55474",
                            "muxedLastClock": "7524929",
                            "muxedFrameCount": "55474",
                            "frameRate": "25.00",
                            "actualEncRate": "25.01",
                            "currentPosition": "0",
                            "totalDuration": "0",
                            "encodingSpeed": "0",
                            "progress": "0.00",
                            "videoCodecType": "0",
                            "audioCodecType": "1",
                            "dspId": "-2",
                            "coreId": "-1",
                            "dspLoad": "0.00",
                            "inputFrameBuffered": "0",
                            "outputFrameBuffered": "18",
                            "inputFrameCount": "55512",
                            "outputFrameCount": "55474",
                            "bufferedFrameCount": "38",
                            "outputStartTime": "1437041809"
                        },
                        {
                            "@id": "3",
                            "videoTargetRate": "850000",
                            "videoRate": "852336",
                            "audioTargetRate": "64000",
                            "audioRate": "64043", 

                            "muxRate": "917146",
                            "encoderLastClock": "7524929",
                            "encoderFrameCount": "55475",
                            "muxedLastClock": "7524929",
                            "muxedFrameCount": "55474",
                            "frameRate": "25.00",
                            "actualEncRate": "25.00",
                            "currentPosition": "0",
                            "totalDuration": "0",
                            "encodingSpeed": "0",
                            "progress": "0.00",
                            "videoCodecType": "0",
                            "audioCodecType": "1",
                            "dspId": "-2",
                            "coreId": "-1",
                            "dspLoad": "0.00",
                            "inputFrameBuffered": "0",
                            "outputFrameBuffered": "0",
                            "inputFrameCount": "55512",
                            "outputFrameCount": "55474",
                            "bufferedFrameCount": "38",
                            "outputStartTime": "1437041809"
                        }
                    ]
                }
            },
            "isRunning": true
        },
        {
            "information": {},
            "isRunning": false
        }
    ] 
   }
}
```


## GET channel/{id}/events

Returns the events for the channel Id passed into call

** Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```

** Query Params: **

     pageSize: {int} = null
     -  optional integer to determine number of items in each page returned

     pageNumber: {int} = null
     -  optional integer used to determine the curent page when retrieving multiple pages

     order: {QueryOrder} = QueryOrder.ASC
     - default query order is Ascending (QueryOrder.ASC), QueryOrder.DESC = descending order


** Example response: ** 

Headers: 
```
X-Transaction-ID: {string}
```

**Example JSON response: ** 

```JSON

{
  "results": [
    {
      "video": null,
      "id": "684abc56-f578-4599-9de2-5d9a396fa65f",
      "title": " NEW VOD EVENT",
      "refId": "Test Ref",
      "scheduledStart": null,
      "scheduledEnd": null,
      "eventTypeId": "41450c8a-1959-4a49-91a2-a7b137efd479",
      "categoryId": "f3ddc01a-97af-4243-8009-f40eedfaa310",
      "roomId": null,
      "eventType": "NEW Event Type ",
      "category": "Extra Category",
      "room": "",
      "state": 10,
      "stateString": "VOD_PRIVATE",
      "channelId": "28f91bf8-571b-4802-8be2-ccc72e3760b2",
      "actualLiveStart": null,
      "actualLiveEnd": null,
      "publishedStart": null,
      "channel": "Test Channel",
      "displayStart": null,
      "displayEnd": null,
      "autoStart": false,
      "autoEnd": false,
      "publishedEnd": null,
      "drmJson": "{\"drm_provider\":\"NONE\"}",
      "drmData": null,
      "documents": null,
      "tags": null,
      "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect --iss.minimum_fragment_length=10 --hls.minimum_fragment_length=10 --hls.client_manifest_version=4 --hls.no_audio_only",
      "taskEngineJson": "",
      "vuflowId": "a37514d2-ab65-488f-bd87-453bac099526",
      "vuflow": "Secondary Test VuFlow",
      "dvrLength": 20,
      "maxBitrate": null,
      "minBitrate": null,
      "vodDrm": true
    },
    {
      "video": null,
      "id": "17a24de8-b89b-47ad-ab5e-d14da6919d74",
      "title": "Test Event",
      "refId": "New Test Ref Updated",
      "scheduledStart": "2016-10-29T13:00:00Z",
      "scheduledEnd": "2016-11-30T09:00:00Z",
      "eventTypeId": "41450c8a-1959-4a49-91a2-a7b137efd479",
      "categoryId": "6af55714-919c-444f-8ac4-c7dc4ce828d0",
      "roomId": null,
      "eventType": "NEW Event Type ",
      "category": "Test Category",
      "room": "",
      "state": 0,
      "stateString": "PRE_LIVE",
      "channelId": "28f91bf8-571b-4802-8be2-ccc72e3760b2",
      "actualLiveStart": null,
      "actualLiveEnd": null,
      "publishedStart": null,
      "channel": "Test Channel",
      "displayStart": "2016-10-29T13:00:00Z",
      "displayEnd": "2016-11-30T09:00:00Z",
      "autoStart": true,
      "autoEnd": true,
      "publishedEnd": null,
      "drmJson": "{\"drm_provider\":\"NONE\"}",
      "drmData": null,
      "documents": null,
      "tags": null,
      "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect --iss.minimum_fragment_length=10 --hls.minimum_fragment_length=10 --hls.client_manifest_version=4 --hls.no_audio_only",
      "taskEngineJson": null,
      "vuflowId": null,
      "vuflow": "",
      "dvrLength": 60,
      "maxBitrate": 400000,
      "minBitrate": null,
      "vodDrm": false
    },
    {
      "video": null,
      "id": "1083b3ad-0ca4-4dad-bf4d-cb0b115801ce",
      "title": "Test Live Event 2 Updated",
      "refId": "REF New Update",
      "scheduledStart": "2016-10-23T11:00:00Z",
      "scheduledEnd": "2016-10-24T10:00:00Z",
      "eventTypeId": "41450c8a-1959-4a49-91a2-a7b137efd479",
      "categoryId": "1aa837b1-036f-4dce-83b1-94537f4d9052",
      "roomId": null,
      "eventType": "NEW Event Type ",
      "category": "Category Alpha",
      "room": "",
      "state": 0,
      "stateString": "PRE_LIVE",
      "channelId": "28f91bf8-571b-4802-8be2-ccc72e3760b2",
      "actualLiveStart": null,
      "actualLiveEnd": null,
      "publishedStart": "2016-11-02T15:00:00Z",
      "channel": "Test Channel",
      "displayStart": "2016-11-02T15:00:00Z",
      "displayEnd": "2016-11-02T16:00:00Z",
      "autoStart": false,
      "autoEnd": false,
      "publishedEnd": "2016-11-02T16:00:00Z",
      "drmJson": "{\"drm_provider\":\"NONE\"}",
      "drmData": null,
      "documents": null,
      "tags": null,
      "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect --iss.minimum_fragment_length=10 --hls.minimum_fragment_length=10 --hls.client_manifest_version=4 --hls.no_audio_only",
      "taskEngineJson": null,
      "vuflowId": null,
      "vuflow": "",
      "dvrLength": 60,
      "maxBitrate": null,
      "minBitrate": null,
      "vodDrm": false
    },
    {
      "video": null,
      "id": "a3de4ec6-238a-4591-a25f-0d246f373b2d",
      "title": " VOD_PUBLIC TEST EVENT",
      "refId": null,
      "scheduledStart": "2016-11-25T01:01:15Z",
      "scheduledEnd": "2016-11-25T12:00:00Z",
      "eventTypeId": "41450c8a-1959-4a49-91a2-a7b137efd479",
      "categoryId": "6af55714-919c-444f-8ac4-c7dc4ce828d0",
      "roomId": null,
      "eventType": "NEW Event Type ",
      "category": "Test Category",
      "room": "",
      "state": 11,
      "stateString": "VOD_PUBLIC",
      "channelId": "28f91bf8-571b-4802-8be2-ccc72e3760b2",
      "actualLiveStart": null,
      "actualLiveEnd": null,
      "publishedStart": null,
      "channel": "Test Channel",
      "displayStart": "2016-11-25T01:01:15Z",
      "displayEnd": "2016-11-25T12:00:00Z",
      "autoStart": false,
      "autoEnd": false,
      "publishedEnd": null,
      "drmJson": "{\"drm_provider\":\"NONE\"}",
      "drmData": null,
      "documents": null,
      "tags": null,
      "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect --iss.minimum_fragment_length=6 --hls.minimum_fragment_length=6 --hls.client_manifest_version=4 --hls.no_audio_only",
      "taskEngineJson": "{\"parameters\":{\"content_id\":\"a3de4ec6-238a-4591-a25f-0d246f373b2d\",\"source_folder\":\"a3de4ec6-238a-4591-a25f-0d246f373b2d\",\"output_folder\":\"a3de4ec6-238a-4591-a25f-0d246f373b2d\",\"encrypted\":false,\"drm\":[\"fairplay\",\"playready\",\"cenc\",\"widevine\"],\"rest_endpoints\":null},\"client\":\"vrt-dev\",\"job\":{\"workflow\":\"vodstream\"}}",
      "vuflowId": "f3923c74-5a1c-4620-a5ca-6e48fcaeb066",
      "vuflow": "X Vuflow",
      "dvrLength": null,
      "maxBitrate": null,
      "minBitrate": null,
      "vodDrm": false
    }
  ],
  "totalCount": 4,
  "pageSize": 4
}

```

# Logs

## GET log

**Note:** Could be useful for displaying errors in the admin.

** Query Params: **
```
Level: {string}  
-    'Trace' or 'Debug' or 'Info' or 'Warn' or 'Error' or 'Fatal'

From: {ISO8601 string}
-    Datetime. E.g. '2015-04-21T11:48:38Z' or '2015-04-21T12:48:38+01:00'

To: {ISO8601 string}
-    Datetime. E.g. '2015-04-21T12:48:38Z' or '2015-04-21T13:48:38+01:00'

Page Size: {int}
-    The page size to return. E.g. 10.

Page Number:  {int}

Order: {string}
-    'ASC' or 'DESC'

```

**Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```
**Example response:** 

Headers: 
```
X-Transaction-ID: {string}
```

**Example JSON Response:**
```json
{    
    {
        "id": 16,
        "callSite": "VuPlatform.VIS.Service.Logging.VISLogging.Log(c:\\DevWork\
            \vualto-ip-videoplatform\\src\\VuPlatform.VIS.Service\\Logging\
            \VISLogging.cs:73)", "date":"2015-07-08T11:54:50Z",
        "exception": "", 
        "level": "Warn", 
        "logger": "VuPlatform.VIS.Service.Logging.VISLogging",
        "machineName": "CHRIS-PC",
        "message": "Event has been deleted with ID 
            2d5336e5-1e30-4ced-bf6a-1bd3a17d2485",
        "stackTrace": "<no type>.lambda_method => EventController.Delete => 
            VISLogging.Log",
        "thread": "14", 
        "username": "IIS APPPOOL\\vualto VIS" 
    },
    {   "id": 17, "callSite": "VuPlatform.VIS.Service.Logging.VISLogging.Log(c:\
            \DevWork\\vualto-ip-videoplatform\\src\\VuPlatform.VIS.Service\\Logging\
            \VISLogging.cs:73)", 
        "date":"2015-07-08T11:55:29Z",
        "exception": "",
        "level": "Warn",
        "logger": "VuPlatform.VIS.Service.Logging.VISLogging", 
        "machineName": "CHRIS-PC", 
        "message":"Event has been deleted with 
            ID 2d5336e5-1e30-4ced-bf6a-1bd3a17d2485",
        "stackTrace": "<no type>.lambda_method 
        => EventController.Delete => VISLogging.Log", 
        "thread":"14",
        "username": "IISAPPPOOL\\vualtoVIS"   
    },   
    {
        "id":18, 
        "callSite":"VuPlatform.VIS.Service.Logging.VISLogging.Log(c:\
            \DevWork\\vualto-ipvideoplatform\\src\\VuPlatform.VIS.Service\\Logging\
            \VISLogging.cs:73)",
        "date": "2015-07-08T11:57:33Z",  
        "exception": "",
        "level": "Warn", 
        "logger": "VuPlatform.VIS.Service.Logging.VISLogging", 
        "machineName": "CHRIS-PC",
        "message": "Event has been deleted with ID: 
            2d5336e5-1e30-4ced-bf6a-1bd3a17d2485. API Key: 
            25f0d858-b112-4fc38498-f1245e95fb2a", 
        "stackTrace":"<notype>.lambda_method => EventController.Delete => 
            VISLogging.Log", "thread":"6","username":
        "IISAPPPOOL\\vualtoVIS"   
    }
```

# Channel Profiles

Channel profiles are constructed of many AudioInputs, which are defined as follows:

    {
        LanguageCode : {string}     { e.g. "EN" }
        AudioChannel : {int}        { e.g. "1" }
        AudioEnabled : {bool}       { e.g. "true" }
    }

## POST channelprofile

Add new Channel Profile

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
    
** Example Response **

    X-Transaction-ID: {string}
    
** Body Params**

    EventID: {string}

    AudioInputs: {IEnumerable<AudioInput>}

## PUT channelprofile/{id}

Update a channel profile

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
    
** Example Response **

    X-Transaction-ID: {string}
    
** Body Params**

    EventID: {string}

    AudioInputs: {IEnumerable<AudioInput>}

## PUT channelprofile/event/{id}

Update a channel profile for a given Event

Same as above, only change is the route & ID

## GET channelprofile

Get ALL Channel Profiles

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
    
** Example Response **

    X-Transaction-ID: {string}
    
** Example JSON response**

    [
        {
            "id": "d9ab1b39-f08a-4f5a-badb-0a2a7faba164",
            "audioInputs": [
                {
                    "id": "c018bdad-ac0d-4505-b8a7-0117d3620fa7",
                    "languageCode": "bu",
                    "audioChannel": 6,
                    "audioEnabled": true
                },
                {
                    "id": "7698f22e-261e-4f5b-a606-277fc47ab418",
                    "languageCode": "fr",
                    "audioChannel": 5,
                    "audioEnabled": true
                },
                {
                    "id": "bd92d52a-d223-46b0-80f0-31642af06fbe",
                    "languageCode": "es",
                    "audioChannel": 2,
                    "audioEnabled": false
                },
                {
                    "id": "2bf86d10-b9f2-4d9e-9ac2-6c8e89fc622f",
                    "languageCode": "en",
                    "audioChannel": 3,
                    "audioEnabled": true
                },
                {
                    "id": "d4bd8b4b-b4b7-4960-be15-a3ff301b1974",
                    "languageCode": "de",
                    "audioChannel": 1,
                    "audioEnabled": false
                },
                {
                    "id": "8c6addbe-7805-42fd-adf9-a5e0d1bee5d2",
                    "languageCode": "po",
                    "audioChannel": 4,
                    "audioEnabled": false
                }
            ]
        },
        {
            "id": "47d6c1a8-b6c8-4742-9607-f32508d7b5eb",
            "audioInputs": [
                {
                    "id": "fa2e9a99-a306-4ca7-a7a9-288d550dd25d",
                    "languageCode": null,
                    "audioChannel": 0,
                    "audioEnabled": false
                }
            ]
        }
    ]

## GET channelprofile/{id}

Get the given Channel Profile

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
    
** Example Response **

    X-Transaction-ID: {string}
    
** Example JSON response**

    {
        "id": "47d6c1a8-b6c8-4742-9607-f32508d7b5eb",
        "audioInputs": [
            {
                "id": "fa2e9a99-a306-4ca7-a7a9-288d550dd25d",
                "languageCode": null,
                "audioChannel": 0,
                "audioEnabled": false
            }
        ]
    }

## GET channelprofile/event/{id}

Get the Channel Profile for a given Event

Same as above, only change is the route & ID

## DELETE channelprofile/{id}

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
    
** Example Response **

    X-Transaction-ID: {string}

## DELETE channelprofile/event/{id}

Delete the Channel Profile for a given Event

Same as above, only change is the route & ID

# Events

## Event States

```text
    PRE_LIVE
    This is the events initial state. It's possible to set an event back to pre-live and 
    this will remove the existing publishing point.
        
    LIVE_UNPUBLISHED
    This will start the encoder and create the publishing point. However, the state can 
    signal to a player that the stream should not yet be displayed.
    
    LIVE_PUBLISHED
    This will start the encoder if not already started, and create the publishing point. 
    The published state can be use to signal to a player that the stream is ready for 
    public viewing.
        
    SUSPENDED
    This state provides a way to stop the encoder, but keep the publishing point active 
    so that when the encoder restarts (e.g. change back to a LIVE state), it continues 
    streaming. This will also mean the VOD will not contain data when the encoder was 
    not publishing, and will appear seemless to a user.
    
    STOPPED
    This will stop the encoder and end the stream. This state will naturally 
    transition to INSTANT_VOD.
        
    INSTANT_VOD
    This state behaves the same as above but signifies the INSTANT_VOD is ready.
    
    INSTANT_VOD_BACKUP 
    The Instant VOD has been backed up to longer term storage, but no longer stream able. 
    INSTANT_VOD_BACKUP and INSTANT_VOD_DELETE is a configuration setting and can only be 
    one or the other.
    
    INSTANT_VOD_DELETE 
    The instant VOD has been deleted and is no longer stream able.
    INSTANT_VOD_BACKUP and INSTANT_VOD_DELETE is a configuration setting and can only be 
    one or the other.

    PRE_VOD
    The metadata is in the system, but no physical VOD asset has been attached.

    VOD_INGESTING
    The VOD is currently ingesting and will be available once completed.
    
    VOD_PRIVATE
    The VOD is private and only viewable from the protected URL. 
    When putting an event into private, please clear the CDN cache.
    
    VOD_PUBLIC
    The VOD is public and available to the CDN for public usage.           
```


## GET event/{id}

**Query Params: **
```
StreamSource: {string}
- 'CDN_INDEX_0' or 'CDN_INDEX_1' or 'ORIGIN_PRIMARY_INDEX_0' or
'ORIGIN_SECONDARY_INDEX_0' or 'ORIGIN_PRIMARY_INDEX_1' or
'ORIGIN_SECONDARY_INDEX_1' 

A stream source for the event object. Index 0 will relate to single threaded channel 
(first encoder channel). Index 1 will related to multithreaded channel (second encoder
channel). Handy if you want to see what the output is from any of the output stream 
sources.

AutoStart: {bool}
- 'True' or 'False' : Auto start the simple player.

ProcessVideo: {bool}
- 'True' or 'False' : Process the video object of the event. This will return 
                      player urls, embed codes and output stream urls.
                    

```

**Example request:** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```
**Example response:**

Headers: 
```
X-Transaction-ID: {string}
```

**Example JSON response:**
```json
{    
"video": {
    "webPageUrl": null,
    "playerUrl": "http://local.simpleplayer.vualto/player/embedcode
           /2d5336e5-1e30-4ced-bf6a1bd3a17d2484?streamSource=ORIGIN_PRIMARY_INDEX_0",
    "embedCode":"",
    "scriptablePlayerUrl":"http://local.simpleplayer.vualto/player/embedcode
        /2d5336e5-1e30-4ced-bf6a1bd3a17d2484?scriptable=True&streamSource=
        ORIGIN_PRIMARY_INDEX_0",
    "scriptableEmbedCode":"<div id=\"embedCodeIFrame\"></div>\r\n<script type=\"text
        /javascript\">\r\n var src=\"http://local.simpleplayer.vualto/Player/Index
        /2d5336e5-1e30-4ced-bf6a1bd3a17d2484?streamSource=ORIGIN_PRIMARY_INDEX_0\";
        \r\n src+=\"#\"+ encodeURIComponent(document.location.href);\r\n
        $(\"#embedCodeIFrame\").replaceWith(\"<iframe src=\\\"\" src+\"\\\" id=\\\
        "VuPlayer\\\"name=\\\"UKPPlayer\\\" title=\\\"VuPlayer\\\" seamless=\\\
        "seamless\\\" frameBorder=\\\"0\\\"allowfullscreen style=\\\
        "width:100%;height:100%;\\\"></iframe>\");\r\n</script>",
    "requestedInPoint": null, 
    "requestedOutPoint": null, 
    "requestedAudioOnly": false,
    "requestedAutoStart": false, 
    "liveUrls": [{"streamProtocol": 0, "streamProtocolString": "HLS",
                "url": "http://cc.vualto.org/live/ou/crw02/ch01
                    /2d5336e5-1e30-4ced-bf6a-1bd3a17d2484/live.isml/live.m3u8",
                "streamSource": 2,
                "streamSourceString": "ORIGIN_PRIMARY_INDEX_0"
            }, 
            {
                "streamProtocol": 3,
                "streamProtocolString": "DASH",
                "url": "http://cc.vualto.org/live/ou/crw02/ch01
                    /2d5336e5-1e30-4ced-bf6a-1bd3a17d2484/live.isml/live.mpd",
                "streamSource": 2,
                "streamSourceString": "ORIGIN_PRIMARY_INDEX_0"
            },
            {
                "streamProtocol": 2,
                "streamProtocolString": "HSS",
                "url": "http://80.169.3.216/live/ou/crw02/ch01
                    /2d5336e5-1e30-4ced-bf6a-1bd3a17d2484/live.isml/Manifest",
                "streamSource": 2,
                "streamSourceString": "ORIGIN_PRIMARY_INDEX_0"
            },
            {
                "streamProtocol": 4,
                "streamProtocolString": "HDS",
                "url": "http://80.169.3.216/live/ou/crw02/ch01
                    /2d5336e5-1e30-4ced-bf6a-1bd3a17d2484/live.isml/live.f4m",
                "streamSource": 2,
                "streamSourceString": "ORIGIN_PRIMARY_INDEX_0"
            }
        ],
        "vodUrls": [
            {
                "streamProtocol": 0,
                "streamProtocolString": "HLS",
                "url": "http://cc.vualto.org/live/ou/crw02/ch01/2d5336e5-1e30-4ced-
                    bf6a1bd3a17d2484/vod.isml/vod.m3u8?t=2015-07-06T14:48:08", 
                "streamSource":2, 
                "streamSourceString": "ORIGIN_PRIMARY_INDEX_0"},
            { 
                "streamProtocol":3,
                "streamProtocolString": "DASH",
                "url": "http://cc.vualto.org/live/ou/crw02/ch01/2d5336e5-1e30-4ced-         
                    bf6a1bd3a17d2484/vod.isml/vod.mpd?t=2015-07-06T14:48:08",
                "streamSource":2, 
                "streamSourceString":"ORIGIN_PRIMARY_INDEX_0"
            },       
                {"streamProtocol":2,
                "streamProtocolString":"HSS",
                "url":"http://80.169.3.216/live/ou/crw02/ch01/2d5336e5-1e30-4ced-
                    bf6a1bd3a17d2484/vod.isml/Manifest?t=2015-07-06T14:48:08",
                "streamSource":2, 
                "streamSourceString":"ORIGIN_PRIMARY_INDEX_0"
            },
                {"streamProtocol":4, 
                "streamProtocolString":"HDS", 
                "url":"http://80.169.3.216/live/ou/crw02/ch01/2d5336e5-1e30-4ced-   
                    bf6a1bd3a17d2484/vod.isml/vod.f4m?t=2015-07-06T14:48:08",
                "streamSource": 2, 
                "streamSourceString":"ORIGIN_PRIMARY_INDEX_0"
            }],
        "currentStateUrls":
                [ 
                {"streamProtocol":0,
                "streamProtocolString":"HLS", 
                "url":"http://cc.vualto.org/live/ou/crw02/ch01/2d5336e5-1e30-4ced-bf6a- 
                    1bd3a17d2484/vod.isml/vod.m3u8",
                "streamSource":2, 
                "streamSourceString": "ORIGIN_PRIMARY_INDEX_0"
                },
                { "streamProtocol":3,
                "streamProtocolString": "DASH",
                "url": "http://cc.vualto.org/live/ou/crw02/ch01/2d5336e5-1e30-4ced-bf6a-
                    1bd3a17d2484/vod.isml/vod.mpd",
                "streamSource": 2,
                "streamSourceString": "ORIGIN_PRIMARY_INDEX_0"
            },
            {
                "streamProtocol": 2,
                "streamProtocolString": "HSS",
                "url": "http://80.169.3.216/live/ou/crw02/ch01/2d5336e5-1e30-4ced-bf6a-
                    1bd3a17d2484/vod.isml/Manifest",
                "streamSource": 2,
                "streamSourceString": "ORIGIN_PRIMARY_INDEX_0"
            },
            {
                "streamProtocol": 4,
                "streamProtocolString": "HDS",
                "url": "http://80.169.3.216/live/ou/crw02/ch01/2d5336e5-1e30-4ced-bf6a-
                    1bd3a17d2484/vod.isml/vod.f4m",
                "streamSource": 2,
                "streamSourceString": "ORIGIN_PRIMARY_INDEX_0"
            }
        ]
    },
    "id": "2d5336e5-1e30-4ced-bf6a-1bd3a17d2484",
    "title": "Education Panel",
    "scheduledStart": "2015-06-12T13:25:00Z",
    "scheduledEnd": "2015-06-12T14:40:00Z",
    "eventTypeId": null,
    "categoryId": null,
    "roomId": null,
    "eventType": "",
    "category": "",
    "room": "",
    "state": 4,
    "stateString": "STOPPED", 
    "channelId": "dab1ba3f-1e21-4473-bcdf-a9d77f92b847",
    "actualLiveStart": "2015-07-06T14:48:08Z",
    "actualLiveEnd": null,
    "publishedStart": "2015-07-06T14:48:37Z",
    "documents": "http://local.vis.vualto/api/document/event
        /2d5336e5-1e30-4ced-bf6a-1bd3a17d2484",
    "tags": "http://local.vis.vualto/api/tag/event/2d5336e5-1e30-4ced-bf6a-1bd3a17d2484"
  }
```

## GET event
Get all Events

**Example request:** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```
**Example response:**

Headers: 
```
X-Transaction-ID: {string}
```

**Body Params:**

    From: {ISO8601 string} 
    datetime. E.g. “2015-04-21T11:48:38Z” or “2015-04-21T12:48:38+01:00”

    To: {ISO8601 string}
    datetime. E.g. “2015-04-21T12:48:38Z” or “2015-04-21T13:48:38+01:00”

    Page Size: {int}
    The page size to return. E.g. “10”.

    Page Number: {int}
    
    Order: {string}
    “ASC” or “DESC”

    Channel: {string}
    Name of a channel. E.g. “OU CRW02 Channel01”
    
    EventType: {string}
    The event type
    
    Tags: {string}
    Comma seperated list of tags
    
    Category: {string}
    Filter by the category


**Example JSON response:**
```json
{ 
    "results": [
        {
            "video": null,
            "id": "2d5336e5-1e30-4ced-bf6a-1bd3a17d2484",
            "title": "Education Panel",
            "scheduledStart": null,
            "scheduledEnd": "2015-06-12T14:40:00Z",
            "eventTypeId": null,
            "categoryId": null,
            "roomId": null,
            "eventType": "",
            "category": "",
            "room": "",
            "state": 0,
            "stateString": "PRE_LIVE",
            "channelId": "605143ce-d20c-4485-a554-0cb2901ded11",
            "actualLiveStart": "2015-06-12T13:25:00Z",
            "actualLiveEnd": "2015-07-15T10:52:48Z",
            "publishedStart": "2015-06-12T13:25:00Z",
            "channel": "OU CRW02 Channel01 and OU CRW02 Channel02",
            "displayStart": "2015-06-12T13:25:00Z",
            "displayEnd": "2015-07-15T10:52:48Z",
            "autoStart": false,
            "autoEnd": false,
            "documents": null,
            "tags": null
        },
        {
            "video": null,
            "id": "2d5336e5-1e30-4ced-bf6a-1bd3a17d2485", 
            "title": "Education Panel",
            "scheduledStart": "2015-06-14T13:25:00Z",
            "scheduledEnd": null,
            "eventTypeId": null,
            "categoryId": null,
            "roomId": null,
            "eventType": "",
            "category": "",
            "room": "",
            "state": 0,
            "stateString": "PRE_LIVE",
            "channelId": "dab1ba3f-1e21-4473-bcdf-a9d77f92b847",
            "actualLiveStart": "2015-07-15T10:53:09Z",
            "actualLiveEnd": "2015-07-15T10:43:27Z",
            "publishedStart": "2015-07-15T10:53:09Z",
            "channel": "OU CRW02 Channel01",
            "displayStart": "2015-07-15T10:53:09Z",
            "displayEnd": "2015-07-15T10:43:27Z",
            "autoStart": false,
            "autoEnd": false,
            "documents": null,
            "tags": null
        }
    ],
    "totalCount": 2,
    "pageSize": 2
} 
```


## POST event

All parameters are optional and can be omitted apart from ID which is required for updating or if you want to create your own ID.
ScheduledEnd parameter can be omitted or left empty to signify the event will never end.

**Example request:**

Headers:

    X-Auth-Key: {string}

    X-Transaction-ID: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}

Body Params: Note: '?' indicates Optional Parameter.
    
    id: {guid?}

    Title: {string}

    ScheduledStart: {ISO8601 string}

    ScheduledEnd: {ISO8601 string}

    AutoStart: {bool}

    AutoEnd: {bool}

    ChannelID: {guid}
    
    PublishedStart: {string}

    PublishedEnd: {string}    
    
    RoomId: {guid?}

    EventTypeId: {guid?} 

    CategoryId: {guid?} 

    ChannelId: {guid?}

    DrmJson: {string}

    AdditionalStreamingOptions: {string} 
    
    Tags: IEnumerable<TagModel>
    
**Example DrmJson parameter:**    

```json
{
  "drm_provider": "VUALTO",
  "drm_data": 
  [
    {
      "name": "CENC"
    },
    {
      "name": "FAIRPLAY",
      "ask": "asdasdadasd",
      "pem-file-url": "https://vualto-drm-resources.s3.amazonaws.com/clients/VUALTO
        /VUALTO.pem",
      "der-file-url": "https://vualto-drm-resources.s3.amazonaws.com/clients/VUALTO
        /VUALTO.der"
    },
    {
      "name": "AES"
    }
  ]
}
```



## DELETE event/{id}

**Example request:**

Request headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


Body Params: Note: '?' indicates Optional Parameter.
    
    hardDelete: {bool?}


**Example response:**

Headers:

    X-Transaction-ID: {string}

## PUT event/forcestop/{id}

Use this method to force an event into a stopped state. This function will also force stop the encoder channel it’s running under bypassing the concurrency lock mechanism and removing any existing locks. Should be used as a last resort in case the lock synchronisation becomes out of sync. Potentially dangerous as no checks will be made to see if an event channel encoder is already running.

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}

## PUT event/state/{id}

Use update state to start and stop the encoders for the event.

Send “ LIVE_PUBLISHED” to start the encoder.

Send “ STOPPED” to stop the encoder.

**Example request:**

Request headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}

Body Params:

    State

    “PRE_LIVE” or “LIVE_PUBLISHED” or “STOPPED”
    
## PUT event/vuflow/protection/{contentId}
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Body Params:**

    VuflowProtectionModel Model:
    
    protection: {string} VOD_PUBLIC VOD_PRIVATE
    
**Example response:**

Headers:

    X-Transaction-ID: {string}
    
    
## GET event/reference/{referenceId}    

Get Event by Reference Id 

    
**Example request:** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}

```

Query Params: 
```
referenceId : {string}
- client assigned reference Id
```
**Example response:**

Headers: 
```
X-Transaction-ID: {string}    
```

**Example JSON response:**

```json

{
  "video": {
    "webPageUrl": null,
    "playerUrl": "http://local.player.vuworkflow.vualto.vualto.com/player/embedcode
        /f4faec6a-e2da-451f-bc10-ec63a066e9c0",
    "embedCode": "",
    "scriptablePlayerUrl": "http://local.player.vuworkflow.vualto.vualto.com/player
        /embedcode/f4faec6a-e2da-451f-bc10-ec63a066e9c0?scriptable=True",
    "scriptableEmbedCode": "",
    "requestedInPoint": null,
    "requestedOutPoint": null,
    "requestedAudioOnly": false,
    "requestedAutoStart": false,
    "liveUrls": [],
    "vodUrls": [],
    "currentStateUrls": []
  },
  "id": "f4faec6a-e2da-451f-bc10-ec63a066e9c0",
  "title": "Vod Event Test",
  "refId": "Reference XYZ",
  "scheduledStart": "2016-11-04T09:30:00Z",
  "scheduledEnd": "2016-11-04T11:30:00Z",
  "eventTypeId": "41450c8a-1959-4a49-91a2-a7b137efd479",
  "categoryId": "7a03d03c-b751-4de4-8912-caec34e66f7f",
  "roomId": null,
  "eventType": "NEW Event Type ",
  "category": "Post Newton Category",
  "room": "",
  "state": 8,
  "stateString": "PRE_VOD",
  "channelId": null,
  "actualLiveStart": null,
  "actualLiveEnd": null,
  "publishedStart": null,
  "channel": "",
  "displayStart": "2016-11-04T09:30:00Z",
  "displayEnd": "2016-11-04T11:30:00Z",
  "autoStart": false,
  "autoEnd": false,
  "publishedEnd": null,
  "drmJson": "{\"drm_provider\":\"NONE\"}",
  "drmData": null,
  "documents": "http://local.vis.vuworkflow.vualto.vualto.com/api/document/event
    /f4faec6a-e2da-451f-bc10-ec63a066e9c0",
  "tags": "http://local.vis.vuworkflow.vualto.vualto.com/api/tag/event
    /f4faec6a-e2da-451f-bc10-ec63a066e9c0",
  "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 
    --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect 
    --iss.minimum_fragment_length=10 --hls.minimum_fragment_length=10 
    --hls.client_manifest_version=4 --hls.no_audio_only",
  "taskEngineJson": "",
  "vuflowId": "f3923c74-5a1c-4620-a5ca-6e48fcaeb066",
  "vuflow": "X Vuflow",
  "dvrLength": null,
  "maxBitrate": null,
  "minBitrate": null,
  "vodDrm": false
}
```


# Categories

## POST category

Add New category

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Body Params**

    Name: {string}

## PUT category

Update category

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Body Params**

    Name: {string}
    

## GET category

Get ALL categories

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response**  
```json
[
  {
    "id": "1e57588e-5d73-4960-ac80-9d759336ac55",
    "name": "Test Category Add 2",
    "systemCategory": false
  },
  {
    "id": "bd077c44-749e-4f87-9421-9b871be39fff",
    "name": "Test Category Add 3",
    "systemCategory": false
  },
  {
    "id": "6a29d3bf-54ab-4d1a-9275-659d77c7f21a",
    "name": "Test Category Update",
    "systemCategory": false
  }
]
```

## GET category/{id}

Get category by Id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Example JSON response:**  
```json
{
  "id": "1e57588e-5d73-4960-ac80-9d759336ac55",
  "name": "Test Category Add 2",
  "systemCategory": false
}
```

## DELETE category/{id}

Delete category by Id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}


<!--  Commented out as per Chris request

# Clients

## PUT client/environment

Update Primary Environment for client (UAT, LIVEA, LOCAL etc)

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Body Params**

    PrimaryEnvironment: {string}

## GET client/environment

Get ALL Environments for client (UAT, LIVEA, LOCAL etc)

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Example JSON response:**
```josn
[
  {
    "environment": "LOCAL",
    "current": false,
    "primary": true
  },
  {
    "environment": "UAT",
    "current": true,
    "primary": false
  },
  {
    "environment": "LIVEA",
    "current": false,
    "primary": false
  },
  {
    "environment": "LIVEB",
    "current": false,
    "primary": false
  }
]
```

-->

## GET category/{id}/events

Returns the events for the category Id passed into call

** Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```

** Query Params: **

     pageSize: {int} = null
     -  optional integer to determine number of items in each page returned

     pageNumber: {int} = null
     -  optional integer used to determine the curent page when retrieving multiple pages

     order: {QueryOrder} = QueryOrder.ASC
     - default query order is Ascending (QueryOrder.ASC), QueryOrder.DESC = descending order


** Example response: ** 

Headers: 
```
X-Transaction-ID: {string}
```

** Example JSON response: ** 

```JSON
{
  "results": [
    {
      "video": null,
      "id": "1083b3ad-0ca4-4dad-bf4d-cb0b115801ce",
      "title": "Test Live Event 2 Updated",
      "refId": "REF New Update",
      "scheduledStart": "2016-10-23T11:00:00Z",
      "scheduledEnd": "2016-10-24T10:00:00Z",
      "eventTypeId": "41450c8a-1959-4a49-91a2-a7b137efd479",
      "categoryId": "1aa837b1-036f-4dce-83b1-94537f4d9052",
      "roomId": null,
      "eventType": "NEW Event Type ",
      "category": "Category Alpha",
      "room": "",
      "state": 0,
      "stateString": "PRE_LIVE",
      "channelId": "28f91bf8-571b-4802-8be2-ccc72e3760b2",
      "actualLiveStart": null,
      "actualLiveEnd": null,
      "publishedStart": "2016-11-02T15:00:00Z",
      "channel": "Test Channel",
      "displayStart": "2016-11-02T15:00:00Z",
      "displayEnd": "2016-11-02T16:00:00Z",
      "autoStart": false,
      "autoEnd": false,
      "publishedEnd": "2016-11-02T16:00:00Z",
      "drmJson": "{\"drm_provider\":\"NONE\"}",
      "drmData": null,
      "documents": null,
      "tags": null,
      "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect --iss.minimum_fragment_length=10 --hls.minimum_fragment_length=10 --hls.client_manifest_version=4 --hls.no_audio_only",
      "taskEngineJson": null,
      "vuflowId": null,
      "vuflow": "",
      "dvrLength": 60,
      "maxBitrate": null,
      "minBitrate": null,
      "vodDrm": false
    }
  ],
  "totalCount": 1,
  "pageSize": 1
}

```

# Chapter Points

## POST chapterpoint

Add or Update New Chapter Point

**Example Requests**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
    
**Body Params**

    id: {guid?} - optional.  If set, will update Chapter Point otherwise will add new Chapter point
    parentId: {guid?} - optional.  If not set, will create new Chapter Point at root level
    eventId: {guid}
    referenceId: {string}
    title: {string}
    tag: {string}
    startTime: {datetime}
    endTime: {datetime}

Add new Chapter Point

```JSON
{
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "referenceId": "TEST_REF1",
    "title": "Example Title",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:06:00Z",
    "endTime": "2017-04-19T21:07:00Z"
}
```

Update Chapter Point

```JSON
{
    "id": "3ea7c0fa-e468-458b-8b81-a3f50adfcf98",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "referenceId": "TEST_REF1",
    "title": "Example Title Change",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:06:00Z",
    "endTime": "2017-04-19T21:07:00Z"
}
```

**Example response**

Headers:

    X-Transaction-ID: {string}


## GET chapterpoint/{id}

Get Chapter Point by id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Example response**

Headers:

    X-Transaction-ID: {string}

```json
{
  "id": "65c054fc-2044-47ad-aa54-d73493ed0a58",
  "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
  "parentId": "75392b2c-90ec-481c-b4d2-c64a3d099bb6",
  "referenceId": "TEST_REF1",
  "title": "Sub Debate 1-1",
  "tag": "LIVE",
  "startTime": "2017-04-19T08:06:00Z",
  "endTime": "2017-04-19T08:07:00Z",
  "chapterPointCustomProperties": [
    {
      "id": "48fbfdfb-c410-4a56-90fd-99efe045ab4d",
      "key": "Example Key",
      "category": "Example Category",
      "value": "Example Value"
    }
  ]
}
```
    
**Example JSON response**

## GET chapterpoint

Search Chapter Points by title, referenceId, startTime, endTime and tag

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

** Query Params: **

     title: {string} = null
     -  optional string searches LIKE on title

     referenceId: {string} = null
     -  optional string searches equality on referenceId

     startTime: {datetime} = null
     - optional datetime searches on StartTime (from)

     endTime: {datetime} = null
     - optional datetime searches on EndTime (To)

     tag: {string} = null
     - optional string searches on tag

**Example response**

Headers:

    X-Transaction-ID: {string}

```json
[
  {
    "id": "75392b2c-90ec-481c-b4d2-c64a3d099bb6",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": null,
    "referenceId": "TEST_REF1",
    "title": "Debate 1",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:01:00Z",
    "endTime": "2017-04-19T08:05:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "65c054fc-2044-47ad-aa54-d73493ed0a58",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "75392b2c-90ec-481c-b4d2-c64a3d099bb6",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-1",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:06:00Z",
    "endTime": "2017-04-19T08:07:00Z",
    "chapterPointCustomProperties": [
      {
        "id": "48fbfdfb-c410-4a56-90fd-99efe045ab4d",
        "key": "Example Key",
        "category": "Example Category",
        "value": "Example Value"
      }
    ]
  },
  {
    "id": "42287425-8b97-4c27-9902-44625e16f399",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "75392b2c-90ec-481c-b4d2-c64a3d099bb6",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-2",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:08:00Z",
    "endTime": "2017-04-19T08:09:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "d87910c1-e288-4ef6-b0d8-bf350069be49",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "75392b2c-90ec-481c-b4d2-c64a3d099bb6",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-3",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:10:00Z",
    "endTime": "2017-04-19T08:11:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "bf260756-403e-448f-ad8c-11bba7b785b1",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "75392b2c-90ec-481c-b4d2-c64a3d099bb6",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-4",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:12:00Z",
    "endTime": "2017-04-19T08:13:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "aa82da5d-7ef7-4ba2-a280-a8c3aa41a7bb",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "75392b2c-90ec-481c-b4d2-c64a3d099bb6",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-5",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:14:00Z",
    "endTime": "2017-04-19T08:15:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "486a509f-dd74-469a-ae84-35330d9844e8",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "75392b2c-90ec-481c-b4d2-c64a3d099bb6",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-6",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:16:00Z",
    "endTime": "2017-04-19T08:17:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "55df7803-e3f8-4a9b-bb89-9fd0336e137a",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "75392b2c-90ec-481c-b4d2-c64a3d099bb6",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-7",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:18:00Z",
    "endTime": "2017-04-19T08:19:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "48c09f57-fd3c-4ec2-a98b-739e542d1f78",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "75392b2c-90ec-481c-b4d2-c64a3d099bb6",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-8",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:20:00Z",
    "endTime": "2017-04-19T08:21:00Z",
    "chapterPointCustomProperties": []
  }
]
```

## GET chapterpoint/event/{eventId}

Get all Chapter Points by EventId including Chapter Point Custom Properties

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

```json
[
  {
    "id": "dabe33ea-9c41-41a0-b499-4748c80fd655",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "4e1a3e3f-7f48-4b73-9737-41981aba3293",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 2-1",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:26:00Z",
    "endTime": "2017-04-19T08:27:00Z",
    "chapterPointCustomProperties": [
      {
        "id": "01c76534-e725-427c-a59d-1e21160341a1",
        "key": "Test Key 2",
        "category": "Test Category 2",
        "value": "Test Value 2"
      },
      {
        "id": "23f52c33-9133-4999-b2f3-66c44de12432",
        "key": "Test Key",
        "category": "Test Category",
        "value": "Test Value"
      }
    ]
  },
  {
    "id": "e588b75d-bdf9-4ac1-8206-a0c575d9f880",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "4e1a3e3f-7f48-4b73-9737-41981aba3293",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 2-2",
    "tag": "LIVE",
    "startTime": "2017-04-19T08:28:00Z",
    "endTime": "2017-04-19T08:29:00Z",
    "chapterPointCustomProperties": []
  }
]
```

## GET chapterpoint/event/{eventId}/parent/{parentId}

Get all chapter points by EventId includes all related Chapter Custom properties with optional parentId parameter.  ParentId Null returns root nodes.

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

```json
[
  {
    "id": "973d102e-d231-446c-8ccb-659c3e07e33f",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "f588a8c6-0ff0-4492-bcff-7ad22862af45",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-1",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:06:00Z",
    "endTime": "2017-04-19T21:07:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "56ec862a-ef32-4a32-ac69-8c46e3b560be",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "f588a8c6-0ff0-4492-bcff-7ad22862af45",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-2",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:08:00Z",
    "endTime": "2017-04-19T21:09:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "87119742-6c1b-4c83-b4d1-997fd1ff26b3",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "f588a8c6-0ff0-4492-bcff-7ad22862af45",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-3",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:10:00Z",
    "endTime": "2017-04-19T21:11:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "ae50ca15-9a4b-41bd-809b-f4f744773b3b",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "f588a8c6-0ff0-4492-bcff-7ad22862af45",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-4",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:12:00Z",
    "endTime": "2017-04-19T21:13:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "e9994987-9dc1-42b4-852a-0847211025d2",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "f588a8c6-0ff0-4492-bcff-7ad22862af45",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-5",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:14:00Z",
    "endTime": "2017-04-19T21:15:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "b8af4bef-102e-4ecc-89ee-8d099b3d223d",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "f588a8c6-0ff0-4492-bcff-7ad22862af45",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-6",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:16:00Z",
    "endTime": "2017-04-19T21:17:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "39cc1b30-0bed-4534-b8af-36e00d87a538",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "f588a8c6-0ff0-4492-bcff-7ad22862af45",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-7",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:18:00Z",
    "endTime": "2017-04-19T21:19:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "78f12912-bc77-4243-b24e-00582487e6ac",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "f588a8c6-0ff0-4492-bcff-7ad22862af45",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-8",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:20:00Z",
    "endTime": "2017-04-19T21:21:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "41b3afa9-480b-4a27-a178-9eab8817d320",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "f588a8c6-0ff0-4492-bcff-7ad22862af45",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-9",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:22:00Z",
    "endTime": "2017-04-19T21:23:00Z",
    "chapterPointCustomProperties": []
  },
  {
    "id": "97cf0eed-e73b-4feb-80e3-3181e90aad59",
    "eventId": "d32ea324-606d-434d-8e58-1bffa40d10b7",
    "parentId": "f588a8c6-0ff0-4492-bcff-7ad22862af45",
    "referenceId": "TEST_REF1",
    "title": "Sub Debate 1-10",
    "tag": "LIVE",
    "startTime": "2017-04-19T21:24:00Z",
    "endTime": "2017-04-19T21:25:00Z",
    "chapterPointCustomProperties": []
  }
]
```


## DELETE chapterpoint/{id}

Deletes a chapter point including all children and Chapter Point Custom Property

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

# Chapter Points Custom Properties

## POST chapterpoint/customproperty

Add or Update New Chapter Point Custom Property

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Body Params**


    Id: {guid?} - optional. If set, will update Chapter Point Custom Property otherwise will add new Chapter point Custom Property

    ChapterPointId: {guid} 

    Key: {string}

    Category: {string}

    Value: {string} 

**Example response**

Headers:

    X-Transaction-ID: {string}



## GET chapterpoint/customproperty/{id}

Get Chapter Point Custom Property by id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

```json
{
  "id": "48fbfdfb-c410-4a56-90fd-99efe045ab4d",
  "key": "Example Key",
  "category": "Example Category",
  "value": "Example Value"
}
```

## GET chapterpoint/customproperty/event/{eventId}

Get all Chapter Point Custom Properties by EventId

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

```json
[
  {
    "id": "48fbfdfb-c410-4a56-90fd-99efe045ab4d",
    "key": "Example Key",
    "category": "Example Category",
    "value": "Example Value"
  },
  {
    "id": "23f52c33-9133-4999-b2f3-66c44de12432",
    "key": "Test Key",
    "category": "Test Category",
    "value": "Test Value"
  },
  {
    "id": "01c76534-e725-427c-a59d-1e21160341a1",
    "key": "Test Key 2",
    "category": "Test Category 2",
    "value": "Test Value 2"
  }
]
```

## GET chapterpoint/{chapterPointId}/customproperty

Get Chapter Custom Properties by ChapterPointId

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

```json
[
  {
    "id": "23f52c33-9133-4999-b2f3-66c44de12432",
    "key": "Test Key",
    "category": "Test Category",
    "value": "Test Value"
  },
  {
    "id": "01c76534-e725-427c-a59d-1e21160341a1",
    "key": "Test Key 2",
    "category": "Test Category 2",
    "value": "Test Value 2"
  }
]
```

## DELETE chapterpoint/customproperty/{id}  

Delete Chapter Point Custom Property by Id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}


## POST document

Add/Update Document Model:
Add: leave out optional Body Parameters
Update: use all parameters

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Body Params**


    EventId: {guid} 

    DocumentId: {guid?} - optional

    Title: {string }

    Url: {string}

    Order: {int?} - optional


## GET Document

Get ALL documents

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response**

```json
[
  {
    "id": "ba1ece20-9570-49de-b6ca-57307eb643aa",
    "order": 3,
    "name": "Test Document 2",
    "url": "https://abc.def.com"
  },
  {
    "id": "9152b712-edac-44bc-b0fc-ba170c2df7a6",
    "order": 2,
    "name": "Test Document",
    "url": "https://abc.def.com"
  },
  {
    "id": "98ece7b9-8c92-4143-ae50-c87302598f1a",
    "order": 1,
    "name": "Test Document",
    "url": "https://abc.def.com"
  },
  {
    "id": "f0da9a6e-9673-4b54-97e3-d24e5d9c81e6",
    "order": 4,
    "name": "Test Document 3",
    "url": "https://abc.def.com"
  }
]
```

## GET document/{id}

Get document by Id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response**

```json
{
  "id": "9152b712-edac-44bc-b0fc-ba170c2df7a6",
  "order": 2,
  "name": "Test Document",
  "url": "https://abc.def.com"
}
```

## DELETE Document/{id}

Delete document by Id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
    
    

## GET Document/event/{id}

Get document models fo an event by Event Id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response**

```json
[
  {
    "id": "ba1ece20-9570-49de-b6ca-57307eb643aa",
    "order": 3,
    "name": "Test Document 2",
    "url": "https://abc.def.com"
  },
  {
    "id": "9152b712-edac-44bc-b0fc-ba170c2df7a6",
    "order": 2,
    "name": "Test Document",
    "url": "https://abc.def.com"
  },
  {
    "id": "f0da9a6e-9673-4b54-97e3-d24e5d9c81e6",
    "order": 4,
    "name": "Test Document 3",
    "url": "https://abc.def.com"
  }
]
```


## PUT document/{id}/order/{int}

Update Order for a document by document Id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
    
# Event Types

## POST eventtype

Add new Event Type.


**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Body Params**

    Name: {string}
    ReferenceId: {string}

## PUT eventtype/{id}

Update an Event Type.


**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Body Params**

    Name: {string}
    ReferenceId: {string}
    

## GET eventtype

Get ALL Event Types.


**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Example JSON response**
```json
[
  {
    "id": "36728f24-ceca-41fd-983e-3e7d8e85ba43",
    "name": "Test Event Type 6"
  },
  {
    "id": "e08ce849-8627-4ed7-b8b5-7930446499ef",
    "name": "Test Event Type 2"
  },
  {
    "id": "b815b9f3-614b-4162-b64e-a812956aaa1e",
    "name": "Test Event Type 3"
  },
  {
    "id": "c158c02a-245b-4fbf-b8df-c2a318947e81",
    "name": "Test Event Type Update"
  },
  {
    "id": "19f59e69-0515-4d40-ab61-ec891437cbf7",
    "name": "Test Event Type 4"
  },
  {
    "id": "7682feba-99b5-4e38-a152-f0ca0ebf872b",
    "name": "Test Event Type 5"
  }
]
```

## GET eventtype/{id}

Get Event Type by Id


**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response**

```json

{
  "id": "b815b9f3-614b-4162-b64e-a812956aaa1e",
  "name": "Test Event Type 3"
}
```

## GET eventtype/reference/{referenceid}

Get Event Type by reference id


**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response**

```json

{
  "id": "b815b9f3-614b-4162-b64e-a812956aaa1e",
  "name": "Test Event Type 3"
}
```



## DELETE eventtype/{id}

Delecte Event Type by Id


**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}



## GET eventtype/{id}/events

Returns the events for the event type Id passed into call

** Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```

** Query Params: **

     pageSize: {int} = null
     -  optional integer to determine number of items in each page returned

     pageNumber: {int} = null
     -  optional integer used to determine the curent page when retrieving multiple pages

     order: {QueryOrder} = QueryOrder.ASC
     - default query order is Ascending (QueryOrder.ASC), QueryOrder.DESC = descending order


** Example response: ** 

Headers: 
```
X-Transaction-ID: {string}
```

** Example JSON response: ** 

```JSON

{
  "results": [
    {
      "video": null,
      "id": "23a1b661-ca1c-4e1a-831b-b9642514dd04",
      "title": "Test Live Event",
      "refId": null,
      "scheduledStart": "2016-10-20T11:00:00Z",
      "scheduledEnd": "2016-10-21T17:00:00Z",
      "eventTypeId": "d15ec87f-80f5-4ab3-9365-71aa81615b25",
      "categoryId": null,
      "roomId": null,
      "eventType": "Test Event Type",
      "category": "",
      "room": "",
      "state": 0,
      "stateString": "PRE_LIVE",
      "channelId": "7b85849e-d87c-4cbf-bbd1-80b8efab725a",
      "actualLiveStart": null,
      "actualLiveEnd": null,
      "publishedStart": "2016-10-24T14:00:00Z",
      "channel": "Channel Default",
      "displayStart": "2016-10-24T14:00:00Z",
      "displayEnd": "2016-10-25T11:00:00Z",
      "autoStart": true,
      "autoEnd": true,
      "publishedEnd": "2016-10-25T11:00:00Z",
      "drmJson": "{\"drm_provider\":\"NONE\"}",
      "drmData": null,
      "documents": null,
      "tags": null,
      "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect --iss.minimum_fragment_length=10 --hls.minimum_fragment_length=10 --hls.client_manifest_version=4 --hls.no_audio_only",
      "taskEngineJson": null,
      "vuflowId": null,
      "vuflow": "",
      "dvrLength": 55000,
      "maxBitrate": 2400,
      "minBitrate": null,
      "vodDrm": false
    }
  ],
  "totalCount": 1,
  "pageSize": 1
}



```






# Health

Get the Health of the system - Ping

## GET health

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
    
# Rooms

## GET room

Get ALL room data

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response: **

```json
{
  "id": "b815b9f3-614b-4162-b64e-a812956aaa1e",
  "name": "Test Room 1"
},
{
  "id": "aa65b9f3-614b-4162-b64e-a812956aaa1e",
  "name": "Test Room 2"
},
{
  "id": "ff11b9f3-614b-4162-b64e-a812956aaa1e",
  "name": "Test Room 3"
}
```

## GET room/{id}

Get room data by Id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response: **
```json
{
  "id": "b815b9f3-614b-4162-b64e-a812956aaa1e",
  "name": "Test Room 1"
}
```

## GET room/{id}/events

Returns the events for the room Id passed into call.

** Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```

** Query Params: **

     pageSize: {int} = null
     -  optional integer to determine number of items in each page returned

     pageNumber: {int} = null
     -  optional integer used to determine the curent page when retrieving multiple pages

     order: {QueryOrder} = QueryOrder.ASC
     - default query order is Ascending (QueryOrder.ASC), QueryOrder.DESC = descending order


** Example response: ** 

Headers: 
```
X-Transaction-ID: {string}
```

** Example JSON response **
```JSON
{
  "results": [
    {
      "video": null,
      "id": "684abc56-f578-4599-9de2-5d9a396fa65f",
      "title": "NEW VOD EVENT",
      "refId": "Test Ref",
      "scheduledStart": null,
      "scheduledEnd": null,
      "eventTypeId": "41450c8a-1959-4a49-91a2-a7b137efd479",
      "categoryId": "f3ddc01a-97af-4243-8009-f40eedfaa310",
      "roomId": b34d7ad4-5c48-4cb6-84f7-87b450a6fd2e,
      "eventType": "NEW Event Type ",
      "category": "Extra Category",
      "room": "",
      "state": 10,
      "stateString": "VOD_PRIVATE",
      "channelId": "28f91bf8-571b-4802-8be2-ccc72e3760b2",
      "actualLiveStart": null,
      "actualLiveEnd": null,
      "publishedStart": null,
      "channel": "Test Channel",
      "displayStart": null,
      "displayEnd": null,
      "autoStart": false,
      "autoEnd": false,
      "publishedEnd": null,
      "drmJson": "{\"drm_provider\":\"NONE\"}",
      "drmData": null,
      "documents": null,
      "tags": null,
      "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect --iss.minimum_fragment_length=10 --hls.minimum_fragment_length=10 --hls.client_manifest_version=4 --hls.no_audio_only",
      "taskEngineJson": "",
      "vuflowId": "a37514d2-ab65-488f-bd87-453bac099526",
      "vuflow": "Secondary Test VuFlow",
      "dvrLength": 20,
      "maxBitrate": null,
      "minBitrate": null,
      "vodDrm": true
    }
  ],
  "totalCount": 1,
  "pageSize": 1
}
```



# Tags

## POST tag

Add a new Tag

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Body Params:**
    
    Name: {string}
    

## PUT tag

Update a Tag

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    
**Body Params:**
    
    Id: {guid}
    Name: {string}


## DELETE tag/{id}

Delete a Tag

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}
    

## GET tag

Get ALL Tags

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Example JSON response:**
```json
[
  {
    "id": "485cd88f-21d7-41dc-aa96-966fb4e89e87",
    "name": "test tag 5 update"
  },
  {
    "id": "666508ff-4d27-4329-a97f-9babe996af9a",
    "name": "test tag 2"
  },
  {
    "id": "6a7fee54-e597-4cd6-a0a9-bf8b0c7f8bff",
    "name": "test tag 3"
  },
  {
    "id": "ec8f26d2-c61e-4cb5-8e79-e391ac01c6a4",
    "name": "test tag 4"
  }
]
```

## GET tag/{id}

Get Tag by Id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Example JSON response:**
```json
{
    "id": "485cd88f-21d7-41dc-aa96-966fb4e89e87",
    "name": "test tag 5 update"
}

```

## GET tag/event/{id}

Get ALL Tags by/for Event Id

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Example JSON response:**
```json
[
  {
    "id": "485cd88f-21d7-41dc-aa96-966fb4e89e87",
    "name": "test tag 5 update"
  },
  {
    "id": "666508ff-4d27-4329-a97f-9babe996af9a",
    "name": "test tag 2"
  },
  {
    "id": "ec8f26d2-c61e-4cb5-8e79-e391ac01c6a4",
    "name": "test tag 4"
  }
]

```

## PUT tag/event/{id}

Update Tags by Event Id.
Refreshes Tag data for Event Id.

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Body Params:**

    Tag Models:
    
    Id: {guid}
    Name: {string} 

    

## GET tag/exists/{text}

Test if Tag exists by Text (Tag Name)

**Example request**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response**

Headers:

    X-Transaction-ID: {string}

**Example response:**

    true/false



## GET tag/{id}/events

Returns the events for the tag Id passed into call

** Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```

** Query Params: **

     pageSize: {int} = null
     -  optional integer to determine number of items in each page returned

     pageNumber: {int} = null
     -  optional integer used to determine the curent page when retrieving multiple pages

     order: {QueryOrder} = QueryOrder.ASC
     - default query order is Ascending (QueryOrder.ASC), QueryOrder.DESC = descending order


** Example response: ** 

Headers: 
```
X-Transaction-ID: {string}
```

**Example JSON response: ** 

```JSON

{
  "results": [
    {
      "video": null,
      "id": "23a1b661-ca1c-4e1a-831b-b9642514dd04",
      "title": "Test Live Event",
      "refId": null,
      "scheduledStart": "2016-10-20T11:00:00Z",
      "scheduledEnd": "2016-10-21T17:00:00Z",
      "eventTypeId": null,
      "categoryId": null,
      "roomId": null,
      "eventType": "",
      "category": "",
      "room": "",
      "state": 0,
      "stateString": "PRE_LIVE",
      "channelId": "7b85849e-d87c-4cbf-bbd1-80b8efab725a",
      "actualLiveStart": null,
      "actualLiveEnd": null,
      "publishedStart": "2016-10-24T14:00:00Z",
      "channel": "Channel Default",
      "displayStart": "2016-10-24T14:00:00Z",
      "displayEnd": "2016-10-25T11:00:00Z",
      "autoStart": true,
      "autoEnd": true,
      "publishedEnd": "2016-10-25T11:00:00Z",
      "drmJson": "{\"drm_provider\":\"NONE\"}",
      "drmData": null,
      "documents": null,
      "tags": null,
      "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect --iss.minimum_fragment_length=10 --hls.minimum_fragment_length=10 --hls.client_manifest_version=4 --hls.no_audio_only",
      "taskEngineJson": null,
      "vuflowId": null,
      "vuflow": "",
      "dvrLength": 55000,
      "maxBitrate": 2400,
      "minBitrate": null,
      "vodDrm": false
    },
    {
      "video": null,
      "id": "17a24de8-b89b-47ad-ab5e-d14da6919d74",
      "title": "Test Event",
      "refId": "New Test Ref Updated",
      "scheduledStart": "2016-10-29T13:00:00Z",
      "scheduledEnd": "2016-11-30T09:00:00Z",
      "eventTypeId": "41450c8a-1959-4a49-91a2-a7b137efd479",
      "categoryId": "6af55714-919c-444f-8ac4-c7dc4ce828d0",
      "roomId": null,
      "eventType": "NEW Event Type ",
      "category": "Test Category",
      "room": "",
      "state": 0,
      "stateString": "PRE_LIVE",
      "channelId": "28f91bf8-571b-4802-8be2-ccc72e3760b2",
      "actualLiveStart": null,
      "actualLiveEnd": null,
      "publishedStart": null,
      "channel": "Test Channel",
      "displayStart": "2016-10-29T13:00:00Z",
      "displayEnd": "2016-11-30T09:00:00Z",
      "autoStart": true,
      "autoEnd": true,
      "publishedEnd": null,
      "drmJson": "{\"drm_provider\":\"NONE\"}",
      "drmData": null,
      "documents": null,
      "tags": null,
      "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect --iss.minimum_fragment_length=10 --hls.minimum_fragment_length=10 --hls.client_manifest_version=4 --hls.no_audio_only",
      "taskEngineJson": null,
      "vuflowId": null,
      "vuflow": "",
      "dvrLength": 60,
      "maxBitrate": 400000,
      "minBitrate": null,
      "vodDrm": false
    },
    {
      "video": null,
      "id": "1083b3ad-0ca4-4dad-bf4d-cb0b115801ce",
      "title": "Test Live Event 2 Updated",
      "refId": "REF New Update",
      "scheduledStart": "2016-10-23T11:00:00Z",
      "scheduledEnd": "2016-10-24T10:00:00Z",
      "eventTypeId": "41450c8a-1959-4a49-91a2-a7b137efd479",
      "categoryId": "1aa837b1-036f-4dce-83b1-94537f4d9052",
      "roomId": null,
      "eventType": "NEW Event Type ",
      "category": "Category Alpha",
      "room": "",
      "state": 0,
      "stateString": "PRE_LIVE",
      "channelId": "28f91bf8-571b-4802-8be2-ccc72e3760b2",
      "actualLiveStart": null,
      "actualLiveEnd": null,
      "publishedStart": "2016-11-02T15:00:00Z",
      "channel": "Test Channel",
      "displayStart": "2016-11-02T15:00:00Z",
      "displayEnd": "2016-11-02T16:00:00Z",
      "autoStart": false,
      "autoEnd": false,
      "publishedEnd": "2016-11-02T16:00:00Z",
      "drmJson": "{\"drm_provider\":\"NONE\"}",
      "drmData": null,
      "documents": null,
      "tags": null,
      "additionalStreamingOptions": "--archive_segment_length=600 --archiving=1 --archive_length=259200 --dvr_window_length=43200 --restart_on_encoder_reconnect --iss.minimum_fragment_length=10 --hls.minimum_fragment_length=10 --hls.client_manifest_version=4 --hls.no_audio_only",
      "taskEngineJson": null,
      "vuflowId": null,
      "vuflow": "",
      "dvrLength": 60,
      "maxBitrate": null,
      "minBitrate": null,
      "vodDrm": false
    }
  ],
  "totalCount": 3,
  "pageSize": 3
}

```


# Vuflow 

## GET vuflow/{id}

Get vuflow data for id

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Example response:**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response:**

```JSON

{
id : "a5d067bd-cb45-4d7b-814c-e1caa0833304",
configurationJson : "{ "vuflows": 
    "name": "1x vuflow Task Engine", 
    "type": "TaskEngine", 
    "taskEngineSpecific": { "apiUrl": "http://taskengine.vuworkflow.vualto.com" }, 
    "streamingServer": { "type": "USP", 
                         "streamingServerSpecific": 
                         {"primaryPublishingPoint": 
                            { "uspAPI": "http://ingest.staging.vrt.vualto.com:9292", 
                               "uspAPIKey": "55940a43-a4e6-430f-8977-45fa2f37f744" }, 
                               "secondaryPublishingPoint": 
                               { "uspAPI": "", 
                                 "uspAPIKey": "" } 
                        } }, 
"outputUrls": [ 
    {"streamProtocol": "HLS", 
      "vodCdnUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism/.m3u8", 
      "vodOriginPrimaryUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism
      /.m3u8","vodOriginSecondaryUrl": "" }, 
    {"streamProtocol": "DASH", 
      "vodCdnUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism/.mpd", 
      "vodOriginPrimaryUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism
      /.mpd","vodOriginSecondaryUrl": "" }, 
    {"streamProtocol": "HSS", 
      "vodCdnUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism/Manifest", 
      "vodOriginPrimaryUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism
      /Manifest","vodOriginSecondaryUrl": "" }, 
    {"streamProtocol": "HDS", 
      "vodCdnUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism/.f4m",
      "vodOriginPrimaryUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism
      /.f4m","vodOriginSecondaryUrl": "" } ] 
        } ] 
    }"
    name : "Vuflow Default"
}

```

## GET vuflow

Get all vuflow data

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Example response:**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response:**

```JSON

{
id : "a5d067bd-cb45-4d7b-814c-e1caa0833304",
configurationJson : "{ "vuflows": 
    "name": "1x vuflow Task Engine", 
    "type": "TaskEngine", 
    "taskEngineSpecific": { "apiUrl": "http://taskengine.vuworkflow.vualto.com" }, 
    "streamingServer": { "type": "USP", 
                         "streamingServerSpecific": 
                         {"primaryPublishingPoint": 
                            { "uspAPI": "http://ingest.staging.vrt.vualto.com:9292", 
                               "uspAPIKey": "55940a43-a4e6-430f-8977-45fa2f37f744" }, 
                               "secondaryPublishingPoint": 
                               { "uspAPI": "", 
                                 "uspAPIKey": "" } 
                        } }, 
"outputUrls": [ 
    {"streamProtocol": "HLS", 
      "vodCdnUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism/.m3u8", 
      "vodOriginPrimaryUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism
      /.m3u8","vodOriginSecondaryUrl": "" }, 
    {"streamProtocol": "DASH", 
      "vodCdnUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism/.mpd", 
      "vodOriginPrimaryUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism
      /.mpd","vodOriginSecondaryUrl": "" }, 
    {"streamProtocol": "HSS", 
      "vodCdnUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism/Manifest", 
      "vodOriginPrimaryUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism
      /Manifest","vodOriginSecondaryUrl": "" }, 
    {"streamProtocol": "HDS", 
      "vodCdnUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism/.f4m",
      "vodOriginPrimaryUrl": "http://ingest.staging.vrt.vualto.com/vod/{0}/vod.ism
      /.f4m","vodOriginSecondaryUrl": "" } ] 
        } ] 
    }"
    name : "Vuflow Default"
}

    ... etc


```


## POST vuflow

Add new vuflow data

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Body Params:**

    AddUpdateVuflowModel Model:
    
    name: {string} 
    configurationJson: {string}
    
**Example response:**

Headers:

    X-Transaction-ID: {string}


## PUT vuflow/{id}

Update vuflow data

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Body Params:**

    AddUpdateVuflowModel Model:
    
    id: {guid}
    name: {string} 
    configurationJson: {string}
    
**Example response:**

Headers:

    X-Transaction-ID: {string}

## DELETE vuflow/{id}

Delete vuflow data

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
        
**Example response:**

Headers:

    X-Transaction-ID: {string}


    
## POST cdn

Add new CDN Provider

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Body Params**

    AddUpdateCdnModel Model:
    
    Id: {guid}
    configurationJson: {string}
    Nane: {string}
    
**Example response:**

Headers:

    X-Transaction-ID: {string}

    
## GET cdn/getcdnproviders

Get All CDN Providers

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response:**

```json
[
  {
    "id": "4b78ddc0-b185-4f22-8448-dfd8c8456507",
    "configurationJson": 
     "{\"name\": \"Amazon S3\",\"type\": \"AMAZON_S3\",\"cdnSpecific\": 
        {\"cdnHost\": \"https://api.s3.com\",\"cdnPath\": \"/cdncontent/purge/\",
        \"customerNumber\": \"ah77d\",\"zoneId\": \"988dnjbhe\",\"secretKey\": 
        \"9yh87yh8yg8\"}}",
    "name": "Amazon S3"
  },
  {
    "id": "106aedc7-34a3-43cc-8399-132f26bb2598",
    "configurationJson": 
     "{\"name\": \"Amazon hh\",\"type\": \"AMAZON_S3\",\"cdnSpecific\": 
        {\"cdnHost\": \"https://api.AmazonZZ.com\",\"cdnPath\": \"/content/purge/\",
        \"customerNumber\": \"789954\",\"zoneId\": \"3589\",\"secretKey\": 
        \"jaljkhasdhasdladu\"}}",
    "name": "Amazon HH"
  }
]
...etc

```

## GET cdn/{id}

Get CDN Provider for Id

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response:**

```json
[
  {
    "id": "4b78ddc0-b185-4f22-8448-dfd8c8456507",
    "configurationJson": 
     "{\"name\": \"Amazon S3\",\"type\": \"AMAZON_S3\",\"cdnSpecific\": 
        {\"cdnHost\": \"https://api.s3.com\",\"cdnPath\": \"/cdncontent/purge/\",
        \"customerNumber\": \"ah77d\",\"zoneId\": \"988dnjbhe\",\"secretKey\": 
        \"9yh87yh8yg8\"}}",
    "name": "Amazon S3"
  }
]
```
## PUT cdn/{id}

Update CDN Provider

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}
    
**Body Params**

    AddUpdateCdnModel Model:
    
    Id: {guid}
    configurationJson: {string}
    Nane: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}
    
## DELETE cdn/{id}

Delete CDN Provider

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}
    
**Example response:**

Headers:

    X-Transaction-ID: {string}
  
## POST cdn/clearcdncache/{id}

Invalidate CDN Provider cache

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}
    
**Body Params**    
        
```
  ClearCdnModel Model:
  
  AltPath {string} (Alternative path to one in Configuration Json for the CDN)

```
    
**Example response:**

Headers:

    X-Transaction-ID: {string}
    


# Webhooks 

## GET webhook

Get All Webhooks in the system

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response:**

```json
[
  {
    "id": "0ac3e3e3-f8d8-4f48-b68a-bf9f310cc243",
    "webhookUrl": "http://a.test.webhookurl.com",
    "useHttpBasicAuthentication": true,
    "username": "JoeBloggs",
    "Password": "123Password",
    "useJsonBody": false 
  },
  {
    "id": "e1549469-e6a5-48ec-baf2-b8ade3c0b3dd",
    "webhookUrl": "Http://A.new.website.address.for.webhooks.com",
    "useHttpBasicAuthentication": false,
    "username": "SimonJones",
    "Password": "321Password",
    "useJsonBody": false
  }
]
```
## GET webhook/event/{eventId}

Get all event specific Webhooks by event id.

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response:**

```json
[
  {
    "id": "0ac3e3e3-f8d8-4f48-b68a-bf9f310cc243",
    "eventId": "0ac3e3e3-aabb-4f48-b68a-bf9f310cc243"
    "webhookUrl": "http://a.test.webhookurl.com",
    "useHttpBasicAuthentication": true,
    "username": "JoeBloggs",
    "Password": "123Password",
    "useJsonBody": false 
  },
  {
    "id": "e1549469-e6a5-48ec-baf2-b8ade3c0b3dd",
    "eventId": "7773e3e3-aabb-4f48-b68a-bf9f310cc243"
    "webhookUrl": "Http://A.new.website.address.for.webhooks.com",
    "useHttpBasicAuthentication": false,
    "username": "SimonJones",
    "Password": "321Password",
    "useJsonBody": false
  }
]
```

## GET webhook/{id}

Get Webhook by Id

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}
  
**Example response:**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response:**

```json
[
  {
    "id": "0ac3e3e3-f8d8-4f48-b68a-bf9f310cc243",
    "webhookUrl": "http://a.test.webhookurl.com",
    "useHttpBasicAuthentication": true,
    "username": "JoeBloggs",
    "Password": "123Password",
    "useJsonBody": false
  }
]
```

## GET webhook/{id}/event

Get event specific Webhook by Id

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}
    
**Example JSON response:**

```json
[
  {
    "id": "0ac3e3e3-f8d8-4f48-b68a-bf9f310cc243",
    "eventId": "7773e3e3-f8d8-4f48-b68a-bf9f310cc243",
    "webhookUrl": "http://a.test.webhookurl.com",
    "useHttpBasicAuthentication": true,
    "username": "JoeBloggs",
    "Password": "123Password",
    "useJsonBody": false
  }
]
```

## DELETE webhook/{id}

DELETE Webhook by Id

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}
 
**Example response:**

Headers:

    X-Transaction-ID: {string}

## DELETE webhook/{id}/event

DELETE Event specific Webhook by Id

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}
 
**Example response:**

Headers:

    X-Transaction-ID: {string}



## POST webhook

ADD a new Webhook

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Body Params**  

```
 WebhookUrl: {string}
 UseHttpBasicAuthentication: {boolean}
 Username: {string}
 Password: {string}
 UseJsonBody: {boolean}
```
    

**Example response:**

Headers:

    X-Transaction-ID: {string}

## POST webhook/event

ADD a new event specific Webhook

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}

**Body Params**  

```
 WebhookUrl: {string}
 EventId: {guid}
 UseHttpBasicAuthentication: {boolean}
 Username: {string}
 Password: {string}
 UseJsonBody: {boolean}
```
    

**Example response:**

Headers:

    X-Transaction-ID: {string}

## PUT webhook/{id}

Update Webhook by Id

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Body Params**  

```
 Id: {guid}
 WebhookUrl: {string}
 UseHttpBasicAuthentication: {boolean}
 Username: {string}
 Password: {string}
 UseJsonBody: {boolean}
```

**Example response:**

Headers:

    X-Transaction-ID: {string}

## PUT webhook/{id}/event

Update Event specific Webhook by Id

**Example request:**

Headers:
    
    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Body Params**  

```
 Id: {guid}
 EventId: {guid}
 WebhookUrl: {string}
 UseHttpBasicAuthentication: {boolean}
 Username: {string}
 Password: {string}
 UseJsonBody: {boolean}
```

**Example response:**

Headers:

    X-Transaction-ID: {string}

    
# Playlist 

Note: an Event may belong to more than one play list. 

## POST playlist

Add new playlist

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


Body Params: 

    Name: {string}
   

**Example response:**

Headers:

    X-Transaction-ID: {string}   
   
   
## PUT playlist/{id}   

Update existing playlist

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


Body Params: 

    id: {guid}
    Name: {string}
   

**Example response:**

Headers:

    X-Transaction-ID: {string}      
   
## GET playlist   

Get all playlists

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   

**Example response:**

Headers:

    X-Transaction-ID: {string}      

   
**Example JSON response:**

```JSON
[
  {
    "id": "15264fcf-20c6-48fe-9828-d2a6e8b26591",
    "name": "New Playlist A"
  },
  {
    "id": "26c7f92e-225f-403f-8e2a-45e0a4a3bea4",
    "name": "Playlist B"
  },
  {
    "id": "2f263485-0226-48ae-869c-57f7e0531a24",
    "name": "Playlist C"
  },
  {
    "id": "490c9737-fc0d-4c09-b689-41c571ff29cc",
    "name": "Playlist D"
  }
]

```

## GET playlist/{id}   

Get playlist for Id

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
Body Params:

    id: {guid}
    
    
**Example response:**

Headers:

    X-Transaction-ID: {string}      

   
**Example JSON response:**

```JSON
[
  {
    "id": "2f263485-0226-48ae-869c-57f7e0531a24",
    "name": "Playlist C"
  }
]

```


## GET playlist/{id}/playlistItems   

Get playlist items for playlist Id

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
Body Params:

    id: {guid}
    
    
**Example response:**

Headers:

    X-Transaction-ID: {string}      

   
**Example JSON response:**

```JSON
[
  {
    "id": "8973a249-a6a8-44f0-b937-881b8956ffec",
    "playlistId": "490c9737-fc0d-4c09-b689-41c571ff29cc",
    "eventId": "23a1b661-ca1c-4e1a-831b-b9642514dd04",
    "startTime": "2016-12-21T09:50:07Z"
  },
  {
    "id": "c4d94cf7-56a3-4e41-9ba2-428851c5294d",
    "playlistId": "490c9737-fc0d-4c09-b689-41c571ff29cc",
    "eventId": "01fe9079-0434-4771-8e52-78b92b0ce7e3",
    "startTime": "2016-12-21T11:06:19Z"
  },
  {
    "id": "48d7855b-bc52-4b47-9e50-19a53626fb96",
    "playlistId": "490c9737-fc0d-4c09-b689-41c571ff29cc",
    "eventId": "1083b3ad-0ca4-4dad-bf4d-cb0b115801ce",
    "startTime": "2016-12-22T09:32:39Z"
  }
]

```

## GET playlist/{id}/eventId   

Get playlist for Event Id

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
Body Params:

    id: {guid}
    
    
**Example response:**

Headers:

    X-Transaction-ID: {string}      

   
**Example JSON response:**

```JSON
{
  "id": "490c9737-fc0d-4c09-b689-41c571ff29cc",
  "name": "Playlist D"
}

```



## DELETE playlist/{id}   

Delete playlist for Id

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
Body Params:

    id: {guid}
    
    
**Example response:**

Headers:

    X-Transaction-ID: {string}      


# Custom Property 

Note: An Event may have none or many Custom Properties.

## POST customproperty

Add a new custom property

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


Body Params: 

    EventId: {guid}
    Key: {string}
    Value: {string}
    Category: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}   

## POST customproperty

Update new custom property

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


Body Params: 

    EventId: {guid}
    CustomProoertyId: {guid}
    Key: {string}
    Value: {string}
    Category: {string}

**Example response:**

Headers:

    X-Transaction-ID: {string}      


## GET customproperty/{id}

Get custom property for custom property id.

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
Body Params:

    id: {guid}
    
    
**Example response:**

Headers:

    X-Transaction-ID: {string}      

   
**Example JSON response:**

```JSON

[
  {
    "id": "1a51f3a1-90b6-426f-85f4-d6fbfd3ff508",
    "key": "Post Code",
    "category": "POLISH",
    "value": "Kod pocztowy"
  }
]
```

## GET customproperty/event/{id}

Get custom properties for Event by id.

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
Body Params:

    id: {guid}
    
    
**Example response:**

Headers:

    X-Transaction-ID: {string}      

   
**Example JSON response:**

```JSON
[
  {
    "id": "1a51f3a1-90b6-426f-85f4-d6fbfd3ff508",
    "key": "Post Code",
    "category": "POLISH",
    "value": "Kod pocztowy"
  },
  {
    "id": "f13eb203-622e-4f59-8bac-d2761ce0b178",
    "key": "Stream",
    "category": "POLISH",
    "value": " Strumień"
  },
  {
    "id": "cfdc2964-d6a2-4baf-8c4c-2c27739e1faa",
    "key": "Telephone",
    "category": "POLISH",
    "value": "Telefon"
  },
  {
    "id": "a96b6c33-c110-4b25-bbad-43f645a5f233",
    "key": "Title",
    "category": "POLISH",
    "value": "Tytuł"
  }
]
```

## GET customproperty/event/{id}/category/{category}

Get custom properties for Event by id and category

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
   
**Example response:**

Headers:

    X-Transaction-ID: {string}

**Example JSON response:**

```JSON
[
  {
    "id": "1a51f3a1-90b6-426f-85f4-d6fbfd3ff508",
    "key": "Title",
    "category": "lang-pl",
    "value": "Kod pocztowy"
  },
  {
    "id": "f13eb203-622e-4f59-8bac-d2761ce0b178",
    "key": "Title",
    "category": "lang-en",
    "value": " Title"
  }
]
```
## GET customproperty/event/{id}/key/{key}

Get custom properties for Event by id and key

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
   
**Example response:**

Headers:

    X-Transaction-ID: {string}

**Example JSON response:**

```JSON
[
  {
    "id": "1a51f3a1-90b6-426f-85f4-d6fbfd3ff508",
    "key": "Title",
    "category": "lang-pl",
    "value": "Kod pocztowy"
  },
  {
    "id": "f13eb203-622e-4f59-8bac-d2761ce0b178",
    "key": "Title",
    "category": "lang-en",
    "value": " Title"
  }
]
```

## GET customproperty/event/{id}/category/{category}/key/{key}

Get custom properties for Event by id, category and key

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
   
**Example response:**

Headers:

    X-Transaction-ID: {string}

**Example JSON response:**

```JSON
[
  {
    "id": "1a51f3a1-90b6-426f-85f4-d6fbfd3ff508",
    "key": "Title",
    "category": "lang-pl",
    "value": "Kod pocztowy"
  },
  {
    "id": "f13eb203-622e-4f59-8bac-d2761ce0b178",
    "key": "Title",
    "category": "lang-en",
    "value": " Title"
  }
]
```


## DELETE customproperty/{id}   

Delete customproperty for custom property Id

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
Body Params:

    id: {guid}
    
    
**Example response:**

Headers:

    X-Transaction-ID: {string}
    
    
# Update DVR Window Length

## POST event/stream/dvrwindowlength/{id}

Update the DVR Window Length for the current live stream, supplying the event id.

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


Body Params: 

    DvrWindowLength: {int}

**Example response:**

Headers:

    X-Transaction-ID: {string}
    

# Search 

The search API can be used for querying events by keywords and date range. 

## GET search events

Returns the events based on search parameters

** Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```

** Query Params: **

     keywords: {string} = null
     -  optional string of keywords

     pageSize: {int} = null
     -  optional integer used to specify the page number

     pageNumber: {int} = null
     -  optional integer used to determine the curent page when retrieving multiple pages

     from: {DateTime} = null
     - a from date to query events
     
     to: {DateTime} = null
     - a to date to query events


## GET search chapter points

Returns the chapter points and chapter points custom properties based on search parameters

** Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```

** Query Params: **

     keywordsChapterPoint: {string} = null
     -  optional string of keywords that search on ReferenceId, Tag and Title

     keywordsCustomProperties: {string} = null
     -  optional string of keywords that search on Category and Key

     pageSize: {int} = null
     -  optional integer used to specify the page number

     pageNumber: {int} = null
     -  optional integer used to determine the curent page when retrieving multiple pages

     from: {DateTime} = null
     - a from date to query events
     
     to: {DateTime} = null
     - a to date to query events
     


# Create MP4 Download

## POST voddownload

Create an MP4 based on a list of event ids

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


Body Params: 
```JSON
{
    "emailAddress": "chris.airey@vualto.com",
    "contentId": "Empty or NULL to be generated.",
    "downloadClips": [
        {
            "eventId": "2c7c4d75-3337-69ea-2933-1e28a666848b",
            "start": "2017-10-04T11:00:00.000",
            "end": "2017-10-04T11:05:00.000",
            "filter": null
        },
        {
            "eventId": "2c7c4d75-3337-69ea-2933-1e28a666848b",
            "start": "2017-10-04T11:05:00.000",
            "end": "2017-10-04T11:10:00.000",
            "filter": null
        }
    ]
}
```

**Example response:**

Headers:

    X-Transaction-ID: {string}



## GET voddownload/contentid/{contentid}

Returns the VOD download given the content ID.

** Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```


## GET voddownload/id

Returns the VOD download given the ID.

** Example request: ** 

Headers: 
```
X-Auth-Key: {string}
X-Transaction-ID: {string}
```




# Remixflow 

## GET remixflow/{id}

Get remixflow data for id

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Example response:**

Headers:

    X-Transaction-ID: {string}
    


## GET Remixflow

Get all remixflows data

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Example response:**

Headers:

    X-Transaction-ID: {string}
    



## POST Remixflow

Add new Remixflow data

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Body Params:**

    name: {string} 
    configurationJson: {string}
    
**Example response:**

Headers:

    X-Transaction-ID: {string}


## PUT remixflow/{id}

Update Remixflow data

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


**Body Params:**

    id: {guid}
    name: {string} 
    configurationJson: {string}
    
**Example response:**

Headers:

    X-Transaction-ID: {string}

## DELETE remixflow/{id}

Delete remixflow data

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
        
**Example response:**

Headers:

    X-Transaction-ID: {string}





# Remix Playlist 


## POST remix

Add new remix playlist

**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}

Body Params:

```JSON
{
    "Id": "5dfcaec6-7a84-5efb-b48f-1e86db87426a",
    "title": "The title",
    "private": false,
    "drm": false
    "referenceId": "vid-633136f5-d548-458c-ab1c-9d23462f4034-chris",
    "remixflowId": "1dfcaec6-7a84-5efb-b48f-1e86db87426a",
    "webhooks": [
        {
            "webHookUrl": "https://requestb.in/1iw6pd01",
            "useHttpBasicAuthentication": true,
            "useJsonBody": true
        }
    ],
    "playlistItems": [
        {
            "eventId": "4efcaec6-7a84-5efb-b48f-1e86db87426a",
            "duration": "00:00:05.250",
            "skippable": false
        },
        {
            "eventId": "5DD715C4-9935-55D4-9F5D-1481465ADE17",
            "duration": "00:03:10.323",
            "skippable": true
        }
    ]
}
```
 

**Example response:**

Headers:

    X-Transaction-ID: {string}   
   
   
## PUT remix/{id}   

Update existing remix playlist

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}


Body Params: 

```JSON
{
    "remixflowId": "1dfcaec6-7a84-5efb-b48f-1e86db87426a",
    "drm": true
    "playlistItems": [
        {
            "eventId": "4efcaec6-7a84-5efb-b48f-1e86db87426a",
            "duration": "00:00:05.250",
            "skippable": false
        },
        {
            "eventId": "5DD715C4-9935-55D4-9F5D-1481465ADE17",
            "duration": "00:03:10.323",
            "skippable": true
        }
    ]
}
```
   

**Example response:**

Headers:

    X-Transaction-ID: {string}     

   

## GET remix/{id}   

Get remix playlist for Id

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
Body Params:

    id: {guid}
    
    
**Example response:**

Headers:

    X-Transaction-ID: {string}      

   
**Example JSON response:**

```JSON
[
  {
    "id": "241f1bf1-f199-4e19-8f6e-188f71c7dcdf",
    "eventId": "5dd715c4-9935-55d4-9f5d-1481465ade17",
    "order": "2017-11-09T11:52:43Z",
    "duration": "00:03:10.3230000",
    "skippable": true
  },
  {
    "id": "80747170-1606-41c1-a48c-5bce51f07c53",
    "eventId": "4efcaec6-7a84-5efb-b48f-1e86db87426a",
    "order": "2017-11-09T11:52:42Z",
    "duration": "00:00:05.2500000",
    "skippable": false
  }
]
```


## DELETE remix/{id}   

Delete remix playlist for Id

   
**Example request:**

Headers:

    X-Auth-Key: {string}
    X-Transaction-ID: {string}
   
Body Params:

    id: {guid}
    
    
**Example response:**

Headers:

    X-Transaction-ID: {string}      
