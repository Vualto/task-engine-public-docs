# RELEASE NOTES

- [v2.1.x](#v2-1-x)
- [v2.0.x](#v2-0-x)
- [v1.173.x](#v1-173-x)
- [v1.172.x](#v1-172-x)
- [v1.171.x](#v1-171-x)
- [v1.170.x](#v1-170-x)
- [v1.169.x](#v1-169-x)
- [v1.168.x](#v1-168-x)
- [v1.167.x](#v1-167-x)
- [v1.166.x](#v1-166-x)
- [v1.165.x](#v1-165-x)

## v2.1.x

#### 2.1.2 - 2023-07-05

- Fixed an issue where the incorrect duration was being used to decide the transcode options.
- Added support for USP 1.12.3

#### 2.1.1 - 2023-05-16

- Fixed Twitter upload not working.
- Added the option to enabled/disable the use of a cpix document instead of the cpix proxy when generating VOD using the following workflows:
  - vodcapture
  - vodnpvr
  - vodremix
  - vodstream

#### 2.1.0 - 2023-04-19

- Cleaned up FFmpeg errors.
- Added support for Linux based trickplay track generation.
- Fixed an issue in the `drmswitch` workflow were the files array did not return full paths.
- Workflows can now be setup for more concurrent task execution.

## v2.0.x

#### 2.0.5 - 2023-04-19

- Changed track properties format, the old format is deprecated but still works.
- Track properties can now match based on the track kind.

#### 2.0.4 - 2023-03-31

- Adding an index to audio tracks to avoid accessibility audio tracks from being set as the default.

#### 2.0.3 - 2023-03-27

- Removed `+` from generated filenames to avoid issues with Apache on VOD Origins

#### 2.0.2 - 2023-03-22

- Fixed null objects being added to the json metadata file
- Added `file_parameters` property to `vodstream` to specify accessibility settings and language without needing to change the name of the file.

#### 2.0.1 - 2023-03-13

- Added support for USP 1.12.1.
- Removed native support for USP versions older than 1.11.20.

#### 2.0.0

- Added official multi-tenancy support.
  - Clients using a multi-tenant cluster will still have their own queue settings.
- Added support for client filtering in the queue page
- Client name is displayed in job rows.
- Added support for continuous captures.
  - This feature is experimental and needs to be explicitly specified when submitting a capture job.
  - This feature enables captures that end in the future to start before the end time and continuously capture until the end time is reached.
- Optimised job submission performance.
- Added support for `notify_subscribers` for YouTube syndication.
- Authentication has been made a requirement for more API endpoints.
- Removed native support for USP versions older than 1.11.12.
- The base images now run on top of Ubuntu 20
- A new persist flag has been added to jobs.
  - Jobs flagged to persist will not be cleared as part of the 30 day routines.
- Added a check on ingest (`vodstream`) workflows to fail the job as soon as no sources are found.
- General performance improvements.

## v1.173.x

#### 1.173.11 - 2022-12-05

- Hotfix: Fixed a race condition where a job may be started twice if multiple controllers are present

#### 1.173.10 - 2022-11-14

- Hotfix: vodremix workflow no longer uss the default package options `--timed_metadata --splice_media`

#### 1.173.9 - 2022-09-01

- Hotfix: NPVR hotfix to use archiver usp packaging options.
- Hotfix: NPVR hotfix to use symlinks when using local storage.
- Updated local storage:
  - Added support for symlinks.

#### 1.173.8 - 2022-08-22

- Hotfix: NPVR hotfix to support nil values for segment end iframe times.

#### 1.173.7 - 2022-08-02

- Hotfix: Deducting start time from the duration value when generating mp4s
  
#### 1.173.6

- Patch: Added rescue to Twitter publications to handle forbidden actions.
- Patch: Changed the source used for trickplay thumbnail generation to improve performance.
- Patch: Update mediatailor_channel_assembly workflow.
  - Added support for updating an existing VOD sources.
- Hotfix: Fixed the duration by reducing the time value from the metadata. This was causing issues when assets included capture timestamps.
- Hotfix: Removed hardcoded no multiplex option when packaging DRM manifests that include FairPlay.

#### 1.173.1

- Improved filtering for trickplay track selection.

#### 1.173.0

- Bugfix: Fixed API error codes for issues when submitting a job with missing parameters.
- Added support for the latest USP GA release, 1.11.9.

###### Callbacks

- Added UTC date and time to all workflow callbacks
- Added the client name to all workflow callbacks

###### VOD Capture

- New `seed` parameter has been added to the clip object. This can be used instead of the `content_key` and `key_id` to capture from VUDRM encrypted streams.
- Frame Accuracy is automatically enabled for captures from streams with discontinuities. This reduces the chance of audio/video sync issues.
- Added support for adding trickplay to captures.

###### VOD Stream

- Added support for adding trickplay to ingests.

## v1.172.x

#### 1.172.5 - 2021-08-18

- Minor hot fixes introduced by 1.172.3 release

#### v1.172.3 - 2021-08-17

- Added support for transmuxing when generating the mp4 based on the duration of the content.

#### v1.172.0 - 2021-06-07

- Added [`mediatailor_channel_assembly` workflow](DeveloperDocumentation/TaskEngineWorkflows.md#mediatailor-channel-assembly) for creating and updating MediaTailor VOD playlist channels.
- Added [`mediatailor_channel state` workflow](DeveloperDocumentation/TaskEngineWorkflows.md#mediatailor-channel-state) for starting, stopping and deleting MediaTailor VOD playlist channels.
- Added support for the [`priority_threshold` setting](DeveloperDocumentation/TaskEngineAPI.md#settings-endpoints).
- Minor UI updates to reflect the new setting.

###### VOD Capture

- New `transcode_proxy` parameter for off loading frame accurate transcoding to a remote proxy.

###### VOD Remix

- New `stream_start_time` parameter for setting the stream start time for a Live Compose stream.
- New `dvr_window_length` parameter for setting the DVR window for Live Compose streams
- New `custom_active_manifest_name` parameter to allow the consumer to specify the manifest name for the resulting stream.
- New `transcode_proxy` parameter for off loading frame accurate transcoding to a remote proxy.

###### VOD Remix

- New `transcode_proxy` parameter for off loading frame accurate transcoding to a remote proxy.

## v1.171.x

#### v1.171.1 - 2021-04-08

- Added a task to extract event metatdata to a json file.
- Added event duration to the job callback within a new metadata object.
- Bugfix: fixed an issue that could cause re-run job to fail.

###### VOD Stream

- Output transcoding step has been updated to only execute when an mp4 is requested.
- Added support for SRT subtitle files.
- Optimised the process of preparing captions for packaging.
- Extracting the audio track from the original source when transcoding the source.
- Added support for specifying the Bitmovin encoder version for transcoding.
- Added support for m4v files as encoding sources.

###### VOD Capture

- Output transcoding step has been updated to only execute when an mp4 is requested.
- Clip sorting for discontinuities has been improved.

###### VOD Remix

- Added support for creating AVOD playlists with SCTE35 ad insertion and replacement markers.
- Added support for creating live streams from VOD content with SCTE35 ad replacement markers.

###### Asset Delete

- Bugfix: Added checks for empty arrays before deleting attempting to delete files.

## v1.170.x

##### v1.170.4 - 2020-11-18

- Bugfix: Fixed failing re-run job endpoint.
  
##### v1.170.3 - 2020-11-18

- Bugfix: Transcoding file extension check was case sensitive. 
  
##### v1.170.2 - 2020-10-21

- Bugfix: Fixed issue causing some scheduled jobs to never be queued.

##### v1.170.1 - 2020-09-30

- File properties in callbacks are returned as a JSON array instead of a string containing an array.
- A new `custom_data` parameter has been added to all workflows to allow consumers to submit references they want returned in the final job callback.
- Running jobs can be paused using the update job endpoint and setting the queue_state to paused.
- SSL certificate verification of callbacks can be disabled.

###### VOD Stream

- Bugfix: The filename for audio tracks was not being checked for a language code when the language is missing within the metadata.

###### VOD Capture

- Stream decryption keys (`key_id` and `content_key`) now need to be specified as part of the clip object. This allows for the capture and stitching from two streams encrypted with different keys.
- Native support for capturing from local ingested streams through a new `local_source` property within clip objects.

###### VOD Delete

- The `content_id` parameter is now optional.

###### DRM Switch

- For VUDRM clients, if a clear or DRMed manifest is missing when trying to switch DRM on or off, the manifest will be generated dynamically.

###### VOD NPVR

- New workflow for capturing VOD from Vualto archiver segments.
- This workflow is intelligent in that a segment is only uploaded once even if it is used by multiple VODs.
- SCTE35 markers are preserved and included within the VOD.
- Supports custom manifests that apply DRM keys from Vualto Archiver profiles to the resulting VOD.

## v1.169.x

##### v1.169.3 - 2020-07-09

- Dashboard hotfix.

##### v1.169.2 - 2020-07-07

- General release build.

##### v1.169.1 - 2020-07-07

- Build fixes.

##### v1.169.0 - 2020-07-06

- API enhancements for UI integrations.
- Updated to a newer Ruby version.
- Support for specifying Bitmovin encoding region.
- Bitmovin encodings now have client labels assigned.
- Queue reservation functionality for priority jobs.
- Optimised thumbnail and VTT generation for timeline preview thumbnails.

## v1.168.x

##### v1.168.1 - 2020-03-17

- Added workflows for:
  - generating gifs
  - capturing single frames as jpgs
  - deleting multiple assets without deleting an entire VOD.
- Added support for multiple file formats when ingesting and encoding using Bitmovin.
  - Full list of supported source files are:
    - mp4
    - mov
    - mpg
    - mkv
    - avi
- Added number of failed jobs in the health check endpoint response.
- Bugfix: Added index to audio tracks when ingesting mp4s. This is to ensure each track name is unique.
- Added support for USP 1.10.18.

## v1.167.x

##### v1.167.2 - 2019-11-04

- Bitmovin enhancements for re-packaging.

##### v1.167.1 - 2019-10-29

- Bitmovin integration now supports setting the ACL permission for the mp4 outputs.

##### v1.167.0 - 2019-10-25

- Added support for Azure Blob Storage as a source and/or destination storage option.
  - The option needs to be specified as `azure_blob`.
- Added the functionality to encode ingest source into multiple smaller resolutions.
  - Caveat: Currently this only works if there is a single MP4 source in the ingest folder.
- Task Engine version is now visible within the queue page and returned as part of the health check api request.
- Updates to the preview thumbnail (trickplay) generation to improve performance.
- Updated VOD remix:
  - Added support for Frame Accurate clips.
  - Added option for setting the profile (for a clip) for the remix output.
- Updated Task Engine queue and logs UI.
  - Completed status filter will no longer load failed jobs.
  - Search by job IDs is now supported. Multiple job IDs must be comma separated.
  - There is no longer a requirement for both From and To date to be provided for the date filter to work.
  - Search results will now include the Created At information for the jobs.
  - Word wrapping has been added to logs.
- Added `output_root` option to all workflows.
- API error codes updated.
- CPIX support for DRM packaging.
- Completed [vodremix](DeveloperDocumentation/TaskEngineWorkflows.md#vod-remix) workflow.

## v1.166.x

##### v1.166.8 - 2019-09-04

- Hotfix: Updated DRM switch callback message.

##### v1.166.7 - 2019-08-20

- Hotfix: supporting spaces in file inputs/outputs.

##### v1.166.6 - 2019-08-14

- Added support for USP 1.10.12.
- Removed support for USP 1.8.5.

##### v1.166.5 - 2019-08-13

- Added visible field to log with show_all param toggling.
- Improvements to the `build_thumbnails` workflow.
- Complete re-write of the way storage is handled:
  - Storage controller dictates which storage medium is used.
  - S3 storage re-written to support this.
  - Added local storage support.
  - Added support for different source and destination storage types within the same job.
    - Eg. Source for ingest can be a local MP4 and the destination for the packaged content is S3.
- Rewrite of DRM switch.
- Added vodcapture.json optional parameter "output_root" to specify the output root key.
- Added vodcapture.json optional parameter "empty_target" to specify whether to delete the contents of the output folder or not.
- New versioning system for the Task Engine.

## v1.165.x

##### v1.165.0 - 2019-02-15

- Fixed issue with retry job call from the front end (queue page).
- Fixed bug with job scheduler not assigning the correct run_at time.
- Added a health check endpoint for improved Zabbix monitoring.
- Added proper support for TransDRM (without subtitles).
- Creating a clear manifest now requires the `clear` option to be added to the DRM list.
- Added `build_thumbnails` workflow (builds thumbnail sprite and vtt file for VOD content).
- Added transcoding option back to mp4 generation due to compatibility issues with Twitter.
- Added support for applying track properties to manifest files.
  - These can be added either through the job JSON payload or saved in Central Configuration.