# RELEASE NOTES

## 1.168.1 - 17/03/2020

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
- Added support for USP 1.10.18

## 1.167.2 - 04/11/2019

- Bitmovin enhancements for re-packaging.

## 1.167.1 - 29/10/2019

- Bitmovin integration now supports setting the ACL permission for the mp4 outputs.

## 1.167.0 - 25/10/2019

- Added support for Azure Blob Storage as a source and/or destination storage option.
  - The option needs to be specified as `azure_blob`.
- Added the functionality to encode ingest source into multiple smaller resolutions
  - Caveat: Currently this only works if there is a single MP4 source in the ingest folder.
- Task Engine version is now visible within the queue page and returned as part of the health check api request.
- Updates to the preview thumbnail (trickplay) generation to improve performance.
- Updated VOD remix:
  - Added support for Frame Accurate clips
  - Added option for setting the profile (for a clip) for the remix output
- Updated Task Engine queue and logs UI.
  - Completed status filter will no longer load failed jobs.
  - Search by job IDs is now supported. Multiple job IDs must be comma separated.
  - There is no longer a requirement for both From and To date to be provided for the date filter to work.
  - Search results will now include the Created At information for the jobs.
  - Word wrapping has been added to logs.
- Added `output_root` option to all workflows
- API error codes updated.
- CPIX support for DRM packaging
- Completed vodremix workflow.

## 1.166.8 - 04/09/2019

- Hotfix: Updated DRM switch callback message.

## 1.166.7 - 20/08/2019

- Hotfix: supporting spaces in file inputs/outputs.

## 1.166.6 - 14/08/2019

- Added support for USP 1.10.12
- Removed support for USP 1.8.5

## 1.166.5 - 13/08/2019

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

## 165 - 15/02/2019

- Fixed issue with retry job call from the front end (queue page).
- Fixed bug with job scheduler not assigning the correct run_at time.
- Added a health check endpoint for improved Zabbix monitoring.
- Added proper support for TransDRM (without subtitles).
- Creating a clear manifest now requires the `clear` option to be added to the DRM list.
- Added `build_thumbnails` workflow (builds thumbnail sprite and vtt file for VOD content).
- Added transcoding option back to mp4 generation due to compatibility issues with Twitter.
- Added support for applying track properties to manifest files.
  - These can be added either through the job JSON payload or saved in Central Configuration.