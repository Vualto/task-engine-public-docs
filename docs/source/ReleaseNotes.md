# Release Notes

## 1.168.1 - 17/03/2020

- Added workflows for:
  - generating gifs
  - capturing frames
  - deleting multiple assets without deleting the entire VOD.
- Update mp4, thumbnail and timeline previews to use temp mp4 generated through mp4split transmuxing.
- Migrated more tests to rspec.
- Added support for multiple file formats when ingesting and encoding using Bitmovin. Full list of supported source files are mp4, mov, mpg, mkv and avi.
- System definitions are now split by system (Common, USP, Edgeware).
- Added initial support for ew_recorder.
- Added number of failed jobs in the health check endpoint response.
- Bugfix: Added index to audio tracks when ingesting mp4s. This is to ensure each track name is unique.
- Bugfix: Missing exceptions reference in the KeyProvider.
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
- The last 5 characters of the USP license are no longer masked in logs produced by Linux workers.
  - This was done so we have some way of verifying license keys being used for specific jobs.
- Task Engine version is now visible within the queue page and returned as part of the health check api request.
- Updates to the preview thumbnail (trickplay) generation to improve performance.
- Updated VOD remix:
  - Added support for Frame Accurate clips
  - Added option for setting the profile (from a clip) for the remix output
- Support for generating rSpec test cases.
- Updated Task Engine queue and logs UI.
  - Completed status filter will no longer load failed jobs.
  - Search by job IDs is now supported. Multiple job IDs must be comma separated.
  - There is no longer a requirement for both From and To date to be provided for the date filter to work.
  - Search results will now include the Created At information for the jobs.
  - Word wrapping has been added to logs.
- Added output_root option to all workflows
- API error codes updated.
- CPIX support for DRM packaging
- Completed vodremix workflow.
- Bug fixes.

## 1.166.8 - 04/09/2019

- Hotfix: Updated DRM switch callback message.

## 1.166.7 - 20/08/2019

- Hotfix: supporting spaces in command line inputs/outputs.

## 1.166.6 - 14/08/2019

- Added support for USP 1.10.12
- Removed support for USP 1.8.5
- Bugfix: Make sure retry attempts are converted to integers

## 1.166.5 - 13/08/2019

- Added visible field to log with show_all param toggling.
- Improvements to the `build_thumbnails` workflow.
- Big API refactor: bug fixes, validations, custom exceptions, separate api modules.
- Updated the parser to handle hashes from the job metadata and for task contracts.
- Complete re-write of the way storage is handled:
  - Storage controller dictates which storage medium is used.
  - S3 storage re-written to support this.
  - Added local storage support.
  - Added support for different source and destination storage types within the same job.
    - Eg. Source for ingest can be a local MP4 and the destination for the packaged content is S3.
- Rewrite of DRM switch.
- Custom callbacks new syntax for `type` in (JSON) workflow definition.
  - No longer requires the client name prefix.
  - Uses a double-colon prefix for consistency with custom tasks.
  - Example: old syntax `"type": "myclient_mycallback"`, new syntax `"type": "::mycallback"` or `"type": "myclient::mycallback"`.
- Added support for remix. RemixCli (lib), UspRemix (task) and remix.json (workflow).
- Added vodcapture.json optional parameter "output_root" to specify the output root key.
- Added vodcapture.json optional parameter "empty_target" to specify whether to delete output folder or not.
- New versioning of the Task Engine.

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

## 164 - 17/12/2018

- Support for Resque retries for failed resque jobs.
- Support for USP 1.9.5.
- Scheduler enchantment to stop schedules through the api.

## 163 - 13/12/2018

- Hotfix for temp MP4s not being cleared.

## 162 - 10/12/2018

- Hotfix for the audio only captures.

## 161 - 22/11/2018

- Hotfix for run_at migration script.

## 160 - 21/11/2018

- Fix for migration script adding run at time.
- Adding default option to custom_mp4 value from central config.

## 159 - 16/11/2018

- Added ability to update running/scheduled jobs with an api request to (put /jobs/:job_id).
  - Fields that can be updated: queue_state(broken/queued), priority, run_at.
  - When setting queue_state to broken, will also iterate over all the jobs tasks and set them to failed.
  - If a run_at time is provided that is the past, then the queue_state will automatically be updated to Time.now.
  - Run_at time will automatically be set to the clip end time if the end time is further in the future than the run_at time.
  - Settings can now be updated using a post request. Parameters need to be provided within the json payload.
- Minor fixes to discontinuity bugs affecting edge cases.


## 158 - 05/11/2018

- Refined the ability to break jobs and clean local storage via the api (TE-50).
  - Local storage was not being cleared when /jobs/:job_id/break was called.
  - Refactored some methods from the controller worker into a JobUtils module. added a clean_local_storage call to the api and made it a `PUT` instead of a `GET` request.
  - Added a call to set all of the job's tasks to failed, when we manually break a job.
  - Updated job.status and still_working? to return broken if the job state is set to broken.
- Rejecting files that do not exist before moving to output folder.
- Changed mapping for single source to mp4 to only map video and audio as invalid captions were being copied over.

## 157 - 31/10/2018

- Added support for multiple origins in clips.
  - New field in clips `sources` which accepts an array of source urls (string).
  - Backwards compatible with source.
- More accurate discontinuities as it now supports milliseconds too.
  - Capture will include what is available in the archive, as long as it falls within the start and end time. This means that in cases where the endpoint is not available within the stream, it will capture up to what is available from the start point (or the earliest start point available that is later than the start point).
- Updated MP4 generation logic to include subtitles.
  - Converts ismt and extracts subtitle streams from ismv to .vtt files.
  - Includes them as part of the transmuxing process.
- All captures will now generate a smil file that will be used in the unified_capture command.
- Support for capturing to downloadable mp4 directly and no VOD asset is created in the `vodcapture` workflow.
  - This means that the custom `vodcapture-mp4` workflow is no longer required.
  - A new option `generate_vod` has been added to enable this. By default the value is set to `true`.
  - `create_dref` option is set to the same value as `generate_vod` by default. If `create_dref` is manually set to true when `generate_vod` is set to false, the job will fail as dref generation requires VOD assets.
- Added `WIP` folder to windows capture paths to avoid issues when multiple captures with the same `content_id` are being executed at the same time.
- Updated `content-type` for mp4 files to `video/mp4`.
- ISMV is now renamed instead of re-packaged if there is only 1 source during the packaging process.
