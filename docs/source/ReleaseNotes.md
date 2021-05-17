# RELEASE NOTES

- [v1.171.x](#v1-171-x)
- [v1.170.x](#v1-170-x)
- [v1.169.x](#v1-169-x)
- [v1.168.x](#v1-168-x)
- [v1.167.x](#v1-167-x)
- [v1.166.x](#v1-166-x)
- [v1.165.x](#v1-165-x)

## v1.171.x

#### v1.171.1 - 08/04/2021

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

##### v1.170.4 - 18/11/2020

- Bugfix: Fixed failing re-run job endpoint.
  
##### v1.170.3 - 18/11/2020

- Bugfix: Transcoding file extension check was case sensitive. 
  
##### v1.170.2 - 21/10/2020

- Bugfix: Fixed issue causing some scheduled jobs to never be queued.

##### v1.170.1 - 30/09/2020

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

##### v1.169.3 - 09/07/2020

- Dashboard hotfix.

##### v1.169.2 - 07/07/2020

- General release build.

##### v1.169.1 - 07/07/2020

- Build fixes.

##### v1.169.0 - 06/07/2020

- API enhancements for UI integrations.
- Updated to a newer Ruby version.
- Support for specifying Bitmovin encoding region.
- Bitmovin encodings now have client labels assigned.
- Queue reservation functionality for priority jobs.
- Optimised thumbnail and VTT generation for timeline preview thumbnails.

## v1.168.x

##### v1.168.1 - 17/03/2020

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

##### v1.167.2 - 04/11/2019

- Bitmovin enhancements for re-packaging.

##### v1.167.1 - 29/10/2019

- Bitmovin integration now supports setting the ACL permission for the mp4 outputs.

##### v1.167.0 - 25/10/2019

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
- Completed [vodremix](TaskEngineWorkflows.md#vod-remix) workflow.

## v1.166.x

##### v1.166.8 - 04/09/2019

- Hotfix: Updated DRM switch callback message.

##### v1.166.7 - 20/08/2019

- Hotfix: supporting spaces in file inputs/outputs.

##### v1.166.6 - 14/08/2019

- Added support for USP 1.10.12.
- Removed support for USP 1.8.5.

##### v1.166.5 - 13/08/2019

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

##### v1.165.0 - 15/02/2019

- Fixed issue with retry job call from the front end (queue page).
- Fixed bug with job scheduler not assigning the correct run_at time.
- Added a health check endpoint for improved Zabbix monitoring.
- Added proper support for TransDRM (without subtitles).
- Creating a clear manifest now requires the `clear` option to be added to the DRM list.
- Added `build_thumbnails` workflow (builds thumbnail sprite and vtt file for VOD content).
- Added transcoding option back to mp4 generation due to compatibility issues with Twitter.
- Added support for applying track properties to manifest files.
  - These can be added either through the job JSON payload or saved in Central Configuration.