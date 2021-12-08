# SUPPORT

## HOW TO CONTACT US

For all non-critical support requests please email support@vualto.com. If you are a registered user you can contact support via the help centre.

For all critical issues please contact support via one of the numbers below.

    +44 (0)800 0314391
    +44 (0)1752 916051
    (800) 857 1808 (toll-free US number)

If there is a problem, please include as much information about the issue as possible.

## SYSTEM REQUIREMENTS

The Task Engine is designed to run in Docker containers, and is therefore able to run on any Docker-supported Linux host. Recent releases of the Task Engine are also compatible with Kubernetes deployments and are fully integrated with Rancher for easy deployment and upgrade management. Any workers that need to run on Windows are currently **not** containerized but are installed as a native Windows service. Windows workers are a requirement for frame accurate captures.

When a worker starts a task, it will generally need to copy one or more files to storage to perform its work in an efficient manner, so it’s critical that hosts have sufficient disk space for processing to take place. This is particularly important when dealing with video transcoding or DRM encryption. A shared storage is also used to store any assets required during job execution and facilitate horizontal scaling of workers. JW Player will usually advise on disk space requirements once the workflow is fully defined and some representative test content has been processed.